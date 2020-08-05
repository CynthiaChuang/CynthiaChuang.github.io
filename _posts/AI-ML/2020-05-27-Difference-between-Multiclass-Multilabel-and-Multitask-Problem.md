---
title: Multi-class, Multi-label 以及 Multi-task 的區別
date: 2020-05-27
modified: 2020-05-27
categories:
- AI/ML
tags:
- 煉丹常識
--- 

<center> <img src="https://i.imgur.com/elR7EfU.png" alt="Multi-Label Classification"></center>
<center class="imgtext">Multi-Label Classification（圖片來源: <a href="https://suhitaghosh10.github.io/EurLexClassification/" class="imgtext">EurLex classification </a>）</center>
<br>
  
最近在糾結 Multi-class 與 Multi-label 的區別，順便頭疼他們該採用的 activation 與 loss function。

<!--more-->
<br><br> 

## Binary classification
在開始前先提提最常見的 Binary classification 二元分類，也就是學習 Yes/No 。

在此類型的分類任務中，會將給定訓練資料分成兩類別進行訓練，常見的二元分類（或者說我最近常見的？）包含：
1. 診斷病患是否患病？
2. 質量控制，確定生產的商品是否滿足規格？
3. 辨識此圖片是貓或狗？

實務上訓練時，標籤通常會標成 `[0] 與 [1]`。另外，在二元分類的 activation 與 loss function 會選擇 <span class='highlighting'>Sigmoid</span> 與 <span class='highlighting'>Binary cross-entropy</span>。

P.S. 是說選 Softmax 也沒關係？因為在二元分類情况下 Softmax 會退化成 Sigmoid。

<br><br> 

## Multiclass classification
而 Multiclass classification 多類別分類。顧名思義，在此類型的分類任務中，會具有兩個以上類別的分類任務，但每個樣本只能被標記為一個類別。

舉例來說，給定一組動物圖片，牠們可能是貓、狗或是熊。在這情況下，每隻動物會被標註成這三類中的其中一種，且不可能同時被標註兩種類別，因為一隻動物不可能同時是貓又是狗。
 
實務上訓練時，標籤通常會標成 
```
貓, 狗, 熊
[[0, 0, 1] 
[0, 1, 0] 
[1, 0, 0]]
```

不過也看過有人的訓練集標註是
```
貓=0, 狗=1, 熊=2
[[0], [1], [2]]
```
<br> 

而此訓練任務的 activation 與 loss function 會選擇 <span class='highlighting'>Softmax</span> 與 <span class='highlighting'>categorical cross-entropy</span>。使用 Softmax 原因在於其輸出值是相互關聯，且其機率的總和始終為1，因此一旦提升某一類別的機率時，其他類別的機率必須會相對應減少，以符合每個樣本只能被標記一類別的定義。



<br><br> 

## Multilabel classification
<center> <img src="https://i.imgur.com/SiUMrKl.png" alt="Multi-Label Classification"></center>
<center class="imgtext">Multi-Label Classification（圖片來源: <a href="https://gombru.github.io/2018/05/23/cross_entropy_loss/" class="imgtext">Raúl Gómez blog</a>）</center>
<br>
 
Multilabel classification 多標籤分類任務，在此訓練任務中存在著兩個以上類別，但每個類別之間並不互斥。

舉例來說，進行文本分類時，一篇文章可能可以同時是 \{教育、政治\} ，這兩個類別；另一篇文章也可能屬於  \{財經、政治、國際\} ，三個類別。

實務上訓練時，標籤通常會標成：
```
教育, 政治, 財經, 國際
[[1 0 0 1]
 [0 0 1 1]
 [0 0 0 0]]
```
<br> 

而 activation 與 loss function 則會選擇 <span class='highlighting'>Sigmoid</span> 與 <span class='highlighting'>binary cross-entropy</span>。使用 Sigmoid 是因為它分別處理各個原始輸出值，使結果相互獨立。


<br><br> 

## Multitask classification
最後一個 Multitask classification，這是我在找資料過程中發現的。在這類型的訓練任務中一樣存在著兩個以上類別，且每個類別之間並不互斥，但與 Multilabel 不同的地方在於每個類別的存在兩種以上的可能。

因此它的資料集標註可能會是類似這樣：
```
[[1 0 2 1]
 [0 0 1 1]
 [0 2 0 0]]
```



<br><br> 

## 參考資料 
1. [Multilabel classification format](https://scikit-learn.org/stable/modules/multiclass.html#multilabel-classification-format) 。檢自 scikit-learn (2020-05-21)。
2. [What is the difference between multiple outputs and multilabel output?](https://www.researchgate.net/post/What_is_the_difference_between_multiple_outputs_and_multilabel_output)。檢自 Researchgate (2020-05-21)。
3. 东明山庄 (2016-04-26)。[Multi-class, Multi-label 以及 Multi-task 问题](https://blog.csdn.net/u012176591/article/details/51251252) 。檢自 金良山庄 CSDN博客 (2020-05-21)。
4. Leoch007 (2018-06-12)。[Multi-class Multi-label Multi-task区别](https://www.geek-share.com/detail/2739811489.html) 。檢自 极客分享 (2020-05-21)。
5. 協同創作。[Multi-label classification](https://en.wikipedia.org/wiki/Multi-label_classification) 。檢自 Wikipedia (2020-05-21)。
6. Anu (2019-02-10)。[What is the difference between Multitask and Multiclass learning](https://stats.stackexchange.com/a/391805)。檢自 Cross Validated - StackExchange (2020-05-21)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期： 2020-05-27</summary>
  <ul class="timestamp">
    　<li>2020-05-27 發布</li>
    　<li>2020-05-24 完稿</li>
  </ul>
</details>