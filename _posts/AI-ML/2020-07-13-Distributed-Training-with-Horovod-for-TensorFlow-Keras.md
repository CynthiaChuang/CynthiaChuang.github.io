---
title: 【TF.Keras】用 Horovod 進行分散式訓練 
date: 2020-08-11
is_modified: false
categories:
- AI/ML
- Computer-Language/Framework
tags:
- Horovod
- Keras/TF.Keras
--- 

其實對於放棄使用 TensorFlow 分散式，擁抱 Horovod 這件事我並沒有太大的感覺，如可以放棄...我還比較想放棄 TF，擁抱 Pytorch（個人嚴重偏愛 Pytorch :heartpulse: )
   
不過 TF 既不能丟，專案又指定要用 Horovod 做為分散式深度學習框架，還是乖乖來看看怎改吧。

<!--more-->
<br><br> 

## Horovod 使用

這邊就不介紹[如何安裝 Horovod](https://horovod.readthedocs.io/en/stable/install_include.html)了，直接針對訓練的腳本進行改，依照文件可分5個步驟：

### 1. 初始化

```python
import tensorflow as tf
import horovod.tensorflow.keras as hvd

# Initialize Horovod
hvd.init()
```
<br> 

### 2. 為每個設置一個 GPU

這邊需要注意的事，在定步驟有分 TF1 與 TF2 的不同，以下是 TF2 的設定法：

```python
# Pin GPU to be used to process local rank (one GPU per process)
gpus = tf.config.experimental.list_physical_devices('GPU')
for gpu in gpus:
    tf.config.experimental.set_memory_growth(gpu, True)
if gpus:
    tf.config.experimental.set_visible_devices(gpus[hvd.local_rank()], 'GPU')
```
<br> 

### 3. 依照分散的的個數縮放 lr 

```python
# Build model and dataset
dataset = ...
model = ...

lr = 0.001
opt = tf.optimizers.Adam(lr * hvd.size())
```
<br> 

### 4. 將 optimizer 包裝到 hvd.DistributedOptimizer 中

這動作主要是為了讓 Horovod 去處理 allreduce 並調整梯度。

```python
# Horovod: add Horovod DistributedOptimizer.
opt = hvd.DistributedOptimizer(opt)
```
<br> 

### 5. 添加 callback 函數 BroadcastGlobalVariablesCallback

它是保證所有參數只在 rank 0 初始化，然後廣播給其他節點。

```python
callbacks = [
    hvd.callbacks.BroadcastGlobalVariablesCallback(0),
]

model.fit(dataset,
          steps_per_epoch=500 // hvd.size(),
          callbacks=callbacks,
          epochs=24,
          verbose=1 if hvd.rank() == 0 else 0)
```

訓練完成後，使用 `hvd.rank() == 0` 來判斷是否為 master，理論上來說只有 master 會負責模型的儲存與其他的輸出。上述完整程式碼請見[Horovod 範例](https://github.com/horovod/horovod/blob/master/examples/tensorflow2_keras_mnist.py)。

<br><br>  

## 資料集切分

關於 Keras 的實做，Horovod 提供了三份參考程式碼（ [tf1 keras mnist](https://github.com/horovod/horovod/blob/master/examples/keras_mnist.py)、[tf1 keras mnist advanced](https://github.com/horovod/horovod/blob/master/examples/keras_mnist_advanced.py)、[tf2 keras mnist](https://github.com/horovod/horovod/blob/master/examples/tensorflow2_keras_mnist.py)）。但在這三份程式碼中，關於資料集的分割存在著兩種不同的方式：
1. 切割 **steps_per_epoch**  
    也就是將一個資料集分成 N 份，也就是在同一個 epochs 中，每個節點看的資料是不同的，因此每個節點所需執行的總 epochs 不變。
    
2. 另一種則是分 **epochs** 數  
    在這情況每個節點每次都會看完整份資料集，因此總 epochs 降低。
<br>

認真研究下兩種狀況的 dataset 的差別：
### 切割 steps_per_epoch

發現這個類型必須要可以控制==每個 epoch step 數目==，此外==會盡可能的去打散資料==，如 `ImageDataGenerator.flow` ，更甚至 `dataset.repeat().shuffle(10000).batch(128)`，保證每個節點每個 batch 拿到資料內容不盡相同。然後在用steps_per_epoch 去聲明一個 epoch 的總步數，當然這步數比實際小，所以每個節點每個 epoch 只會看到 dataset 的一部分，以達到將一個資料集分成 N 份效果。

但這方式隨機性太高，我覺得不能保證整個資料集都被看過，若資料集小則不太適合這樣的切割法。

<br>

### 切割 epochs 數

若是資料量小、或是沒有合適的切分方法，又或者是想保證整個資料集都被看過，可能就得分 epochs。但缺點是，實際上所有節點執行的 epoch 總和會是節點個數的倍數，換就話說，如果使用者給定 epoch 數不是節點個數倍數，會多 train 就是了....

<br>

### 討論：分散式效能下降？

發現若是使用分散式最終所得分數，好像略低於乖乖用一個 GPU 全跑完的分數。不過好像也合理，畢竟是用 all reduces 加總之後平均所得的梯度，比不上真的看過資料來做擬合？

另外一件事，因為我是用 `tf.dataset` 來實做，為了嘗試分割資料集我找到了 [shard](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#shard) 這個 method，程式碼如下：

```python
dataset = tf.data.TextLineDataset(files)
dataset = dataset.shard(hvd.rank())
dataset = dataset.shuffle(buffer_size=1000, reshuffle_each_iteration=True)
dataset = dataset.repeat()
```

但不曉得，是不因為我 shard 做在 shuffle 之前，導致每個節點拿到資料是固定，訓練出來的結果比切割 epochs 數更差...

<br><br>  

## Horovod 與 TF2.2 兼容性問題 

紀錄下一個問題，那天在 tensorflow 看到一條 [Horovod 無法進行梯度計算的 issue](https://github.com/tensorflow/tensorflow/issues/35138)，應該在 TF2.2 以上會遇到，不過 Horovod 這邊有用一些 [HACK 方式進行 HotFix](https://github.com/horovod/horovod/issues/1688)，目前看來程式可以照常執行。若之後 TF 有更新在關注這 issue 的狀況。 
 
<br><br> 

## 參考資料 
1. [Horovod with Keras](https://horovod.readthedocs.io/en/stable/keras.html) 。檢自 Horovod documentation (2020-07-13)。


<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-11</summary>
  <ul class="timestamp">
    　<li>2020-08-11 發布</li>
    　<li>2020-07-13 完稿</li>
    　<li>2020-07-02 起稿</li>
  </ul>
</details>