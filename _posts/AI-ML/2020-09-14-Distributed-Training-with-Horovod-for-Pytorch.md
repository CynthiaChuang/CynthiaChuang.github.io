---
title: 【PyTorch】用 Horovod 進行分散式訓練
date: 2020-12-21 21:39
is_modified: false
disqus: cynthiahackmd
categories:
- AI/ML 
- Computer-Language/Framework 
tags:
- Horovod
- PyTorch
--- 

繼上次用 [Horovod 做為 TensorFlow 分散式深度學習框架](/Distributed-Training-with-Horovod-for-TensorFlow-Keras)後，我這次如願改寫 PyTorch 了，雖然是另外一個案子...。

<!--more-->
<br>

## Horovod 使用

一樣直接拿著[文件](https://horovod.readthedocs.io/en/stable/pytorch.html#horovod-with-pytorch)直接上了，大致可以分成 5 個步驟，這跟 TensorFlow Keras 倒是滿像的：

### 1. 初始化

```python
import horovod.torch as hvd

# Initialize Horovod
hvd.init()
```
 

<br> 

### 2. 為每個節點設置一個 GPU

```python
torch.cuda.set_device(hvd.local_rank())
```

但，我記得官方不推薦用 [set_device](https://pytorch.org/docs/stable/cuda.html)？

<div class="alert info"> 
<div class="head">torch.cuda.set_device(device: Union[torch.device, str, int]) → None</div>
Sets the current device.  <br>
<br>
Usage of this function is discouraged in favor of device. In most cases it’s better to use CUDA_VISIBLE_DEVICES environmental variable.
</div>
<br>

另外在看 Horovod 所提供的兩隻範例程式碼（[resnet50](https://github.com/horovod/horovod/blob/master/examples/pytorch/pytorch_imagenet_resnet50.py) 與 [mnist](https://github.com/horovod/horovod/blob/master/examples/pytorch/pytorch_mnist.py)）的時候，發現在 `set_device` 之後後都會接著：

```
torch.cuda.manual_seed(args.seed)
```

推測是因為每個節點在起網路的時候，初始值不盡相同，因此為了使初始值相同所以啟用了該值。
<br>

不過, 我想了一下，我只有在做分散式訓練的會希望所有網路擁有相同的初始值，所以我改了下程式，多點判斷式：  
1. 進行分散式訓練的時候。
2. 由 master 統一設定。

```python
if hvd.rank() == 0:
   seed = random.randint(1, 100)
   torch.manual_seed(seed)
   torch.cuda.manual_seed(seed)
   torch.cuda.manual_seed_all(seed)

# Horovod: pin GPU to local rank.
torch.cuda.set_device(hvd.local_rank())
```     

<br> 

### 3. 依照分散的個數縮放 lr 

文件範例這部份好像沒做，但上方的動作分解有。所以我按照之前做 TensorFlow Keras 的經驗直接用 lr 乘上節點個數。

```python
# Build model and dataset
dataset = ...
model = ...

lr = 0.001
opt = optim.Adam(model.parameters(), lr=lr*hvd.size())
```
<br>

後來在 [resnet50](https://github.com/horovod/horovod/blob/master/examples/pytorch/pytorch_imagenet_resnet50.py) 中有看到它們實做的學習率調整，看起來是做 warmup ，不過這部份我沒跟著調整了。

```python
# Horovod: using `lr = base_lr * hvd.size()` from the very beginning leads to worse final
# accuracy. Scale the learning rate `lr = base_lr` ---> `lr = base_lr * hvd.size()` during
# the first five epochs. See https://arxiv.org/abs/1706.02677 for details.
# After the warmup reduce learning rate by 10 on the 30th, 60th and 80th epochs.
def adjust_learning_rate(epoch, batch_idx):
   if epoch < args.warmup_epochs:
      epoch += float(batch_idx + 1) / len(train_loader)
      lr_adj = 1. / hvd.size() * (epoch * (hvd.size() - 1) / args.warmup_epochs + 1)
   elif epoch < 30:
      lr_adj = 1.
   elif epoch < 60:
      lr_adj = 1e-1
   elif epoch < 80:
      lr_adj = 1e-2
   else:
      lr_adj = 1e-3
   for param_group in optimizer.param_groups:
      param_group['lr'] = args.base_lr * hvd.size() * args.batches_per_allreduce * lr_adj
```
<br>

### 4. 將 optimizer 包裝到 hvd.DistributedOptimizer 中


```python
# Horovod: add Horovod DistributedOptimizer.
opt = hvd.DistributedOptimizer(opt, named_parameters=model.named_parameters())
```

在這邊我遇到的問題是，我有是使用 scheduler 去 reduced learning rate 的部份。我有找到一條  issue，是在討論這個：
- [Add support for broadcasting learning rate scheduler in PyTorch · Issue #1281 ](https://github.com/horovod/horovod/issues/1281)。

雖然它的狀態是 open，但是程式碼的部份有[合併](https://github.com/horovod/horovod/pull/371/files#diff-b134e260de34b7f653af3c43e362f305L124)回去了，所以我參考他們[測試用的程式碼](https://github.com/horovod/horovod/blob/e4554de96100f5a0e8686cd41cf99a6fe8a71e62/test/test_torch.py#L1506)，把 master 目前的 learning rate 取出後傳到其他節點，其他節點接收到後設定到自己的 optimizer 中。

```python
optimizer = ...
scheduler = ReduceLROnPlateau(optimizer, factor=lr_reduced_factor,
patience=5, mode='min')
for epoch in range(10):
   train(...)
   val_loss = validate(...)
   scheduler.step(val_loss)
   
   lr = multi_gpu_opt.make_broadcast_object(optimizer.param_groups[0]['lr'])
   optimizer.param_groups[0]['lr'] = lr
```

<br>

### 5.  Broadcast
最後廣播給其他節點。

```python
# Horovod: broadcast parameters & optimizer state.
hvd.broadcast_parameters(model.state_dict(), root_rank=0)
hvd.broadcast_optimizer_state(optimizer, root_rank=0)
```

<br><br>

## 資料集切分

最後是資料集切分，這邊我直接仿照[上次的作法](/Distributed-Training-with-Horovod-for-TensorFlow-Keras)採用分 epochs 數的作法。


<br><br> 

## 參考資料 
1. [Horovod with PyTorch](https://horovod.readthedocs.io/en/stable/pytorch.html#horovod-with-pytorch) 。檢自 Horovod documentation (2020-09-14)。
2. [pytorch_imagenet_resnet50.py](https://github.com/horovod/horovod/blob/master/examples/pytorch_imagenet_resnet50.py) 。檢自 horovod/horovod ｜ github (2020-09-14)。
3. [pytorch_mnist.py](https://github.com/horovod/horovod/blob/master/examples/pytorch_mnist.py)。檢自 horovod/horovod ｜ github (2020-09-14)。
4. lbin (2018-09-06)。[Horovod, 分布式进阶](https://github.com/horovod/horovod/blob/master/examples/pytorch_mnist.py)。檢自 知乎 (2020-09-14)。
5. 王晗 (2018-08-04)。[torch.manual_seed(1)是干嘛用的? - 王晗的回答](https://www.zhihu.com/question/288350769/answer/460171615)。檢自 知乎 (2020-09-14)。
6. tgaddair (2019-08-07)。[Add support for broadcasting learning rate scheduler in PyTorch · Issue #1281](https://github.com/horovod/horovod/issues/1281)。檢自 horovod/horovod ｜ github (2020-09-14)。
7. [Added PyTorch support for restoring optimizer state on model load and broadcast by tgaddair · Pull Request #371](https://github.com/horovod/horovod/pull/371/files#diff-b134e260de34b7f653af3c43e362f305L124)。檢自 horovod/horovod ｜ github (2020-09-14)。
8. [test_torch.py](https://github.com/horovod/horovod/blob/e4554de96100f5a0e8686cd41cf99a6fe8a71e62/test/test_torch.py)。檢自 horovod/horovod ｜ github (2020-09-14)。
 
<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-12-21</summary>
  <ul class="timestamp">
    　<li>2020-12-21 發布</li>
    　<li>2020-09-14 完稿</li>
    　<li>2020-09-10 起稿</li>
  </ul>
</details>