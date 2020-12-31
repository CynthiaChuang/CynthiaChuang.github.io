---
title: 【Python】 斷言 assert
date: 2020-02-27
is_modified: false
categories:
- 程式語言與架構
tags:
- Python
--- 

在 Python 中 assert 陳述句長這樣：
```python
assert <test>, <message>
```
  
我會跟你說我記錄這個的原因是因為我老是拼錯嗎？

<!--more-->
<br>

## 斷言（Assertion）

所謂的斷言，指的就是<span class="highlighting">斷定程式進行到特定時間、地點時其變數的狀態</span>，像是變數大小、變數狀態、是否為空值...等。我個人還滿喜歡用斷言，與其讓程式在執行時 crash，再一行一行去追 log，倒不我先檢查變數，錯誤時直接中斷運作，...至少 log 也好看一點 XD

<br>

下面這段程式碼用來檢查選擇的預訓練模型是否支援。因為 `selected` 不在 `SUPPORT_MODELS` 中，所以會中斷運作，並跳出我寫好的錯誤訊息。

```python
SUPPORT_MODELS = ['ResNet50', 'Xception', 'VGG16', "DenseNet121",
                  "InceptionV3", "InceptionResNetV2", "MobileNetV2"]

selected = 'VGG19'

assert selected in SUPPORT_MODELS, \
 "{} isn't supported Please select a model of the following: {}".format(self.apps, ",".join(SUPPORT_MODELS))

```




<br><br> 

## 參考資料 
1. 林信良。[使用 assert](https://openhome.cc/Gossip/Python/Assert.html) 。檢自 語言技術：Python Gossip (2020-02-27)。