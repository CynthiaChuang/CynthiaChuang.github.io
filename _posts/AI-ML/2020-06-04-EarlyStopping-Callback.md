---
title: 【TF.Keras】 EarlyStopping Callbacks 介紹
date: 2020-08-11
is_modified: false
categories:
- AI/ML
tags:
- tf.keras/keras
--- 
 
昨天在訓練時，模型無預警的停止訓練，看中止的情況又不像是 OOM 或其他 ERROR 所造成的。因此懷疑是 EarlyStopping 自主中斷的，但...中止條件明明就不應該被觸發才對...

<!--more-->
<br><br> 

## Callbacks

在談 EarlyStopping 前，先來談談 Callbacks 本身好了。Callbacks 可用來指定在 train/predict/test 的 epoch 或 batch 的前後進行如存檔特定等操作。

部份 Callbacks 的操作觸發與否會由監控的數據變化來決定，有的則是會在指定時刻皆會觸發。


以我常用的 Callbacks 為例：EarlyStopping、ReduceLROnPlateau，會由監控數據來決定觸發與否；TensorBoard、ModelCheckpoint 則是會在指定的頻率進行動做。

<br><br> 

## EarlyStopping

EarlyStopping 顧名思義就是提前中止訓練，一般來說會在下列情況下停止訓練：
1.  出現 Overfitting 的現象。
2.  模型指標沒有明顯改進。例如：loss 不降、acc 不升...等。
3.  模型收斂不了或收斂過慢。

<center> <img src="https://i.imgur.com/Bp5cK74.jpg" alt="Early Stopping"></center>
<center style="color:Gainsboro;">Early Stopping（圖片來源: <a href="http://mmds-data.org/presentations/2016/s-martin.pdf" style="color:Gainsboro;">CC MMDS Talk 2106</a>）</center>
<br><br>

在 tf.keras 中，提供了下列參數可設置：
```python
tf.keras.callbacks.EarlyStopping(
    monitor='val_loss', min_delta=0, 
    patience=0, verbose=0, mode='auto',
    baseline=None, restore_best_weights=False
)
```
<br>

### monitor

這參數用來設置監控的數據，可以設置的數據除 `loss` 外，其他可監控的數據會與 <span class="highlighting">metric 所設定的指標相關</span>。

例如，我的 metrics 設置如下：
```python
metrics = [
    "accuracy", 
    tf.keras.metrics.AUC(curve="ROC", name="auc"),   
    tf.keras.metrics.AUC(curve="PR", name="auc_pr"),  
]
```
因此我可監控的數據就會有：`loss`、 `accuracy`、 `auc` 與 `auc_pr`，其中 `accuracy` 可縮寫成 `acc`。

另外如果在訓練時有設定驗證集，就會多出 `val_loss`、 `val_acc`、 `val_auc` 與 `val_auc_pr`。一般來說，監控的數據會從驗證集中挑選，會更符合模型能力的現況。
<br>

### min_delta
評斷監控的數據是否有改善標準，唯有當數據變動幅度<span class="highlighting">大於</span> min_delta 才算是有改善。
<br>

### patience
容忍...恩...就是說明你可以容忍在<span class="highlighting">多少個 epoch 內監控的數據都沒有出現改善</span>？patient 的設置會與 min_delta 會相關，一般來說 min_delta 小，patient 可以相對降低；反之，則 patient 加大。

但，一般來說，patient 若設置的太小，可能導致模型在訓練前期，還在全域搜尋時就被迫停止；反之，patient 若太大，也就失去 EarlyStopping 設置的意義了。
<br>

### verbose

有 `0` 或 `1` 兩種設置。 `0` 是 silent 不會輸出任何的訊息， `1` 的話會輸出一些 debug 用的訊息。
<br>

### mode

有 `auto`, `min` 和 `max` 三種設置選擇。用來設定監控的數據的<span class="highlighting">改善方向</span>，如過希望你的監控的數據是越大越好，則設置為 `max`，如：`acc`；反之，若希望數據越小越好，則設定 `min`，如：`loss`。

良心建議這個值一定要設，別用預設的 `auto`，不然就會像我這次一樣停的莫名其妙。
<br>

### baseline

> Baseline value for the monitored quantity. Training will stop if the model doesn't show improvement over the baseline.

這個參數我倒是是沒用過，不過看文件的意思應該是指監控的數據的底限。如果你 `acc` 的 baseline 設定 0.85，如果在某個時間斷後跌到 0.85 以下，如 0.75，則模型將停止訓練；或是 `loss` 超過設定 baseline 則停止訓練。

應該是這樣的意思，不負責任推測 XD
<br>

### restore_best_weights

最後一個，是假設真的發生 EarlyStopping 時，此時權重通常都不是最佳的。因此如果要在停止後儲存最佳權重，請將此值設定為 `True`。

不過我通常會用 ModelCheckpoint 或是自製一個 Callback 來儲存權重，所以這個參數我通常設定 `False`。


<br><br> 

## 參考資料 
1. [EarlyStopping](https://keras.io/api/callbacks/early_stopping/#earlystopping) 。檢自 keras (2020-06-03)。
2. [tf.keras.callbacks.EarlyStopping](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/EarlyStopping) 。檢自 TensorFlow Core v2.2.0 (2020-06-03)。
3. silent56_th (2017-06-02)。[keras的EarlyStoppingcallbacks的使用与技巧](https://blog.csdn.net/silent56_th/article/details/72845912) 。檢自 silent56_th的博客 CSDN (2020-06-03)。
4. 宿宝臣 (2019-07-21)。[Tensorflow的EarlyStopping技术](https://subaochen.github.io/tensorflow/2019/07/21/tensorflow-earlystopping/) 。檢自 宿宝臣的博客 (2020-06-03)。
5. Sumanth Meenan (2019-09-23)。[Groom your model using Keras Callbacks](https://medium.com/analytics-vidhya/groom-your-model-using-keras-callbacks-e97b4fa1b21c) 。檢自 Analytics Vidhya - Medium (2020-06-03)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-11</summary>
  <ul class="timestamp">
    　<li>2020-08-11 發布</li>
    　<li>2020-06-04 完稿</li>
    　<li>2020-06-03 起稿</li>
  </ul>
</details>