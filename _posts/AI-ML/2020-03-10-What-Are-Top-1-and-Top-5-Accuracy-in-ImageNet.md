---
title: ImageNet 中的 Top-1 與 Top-5 accuracy
date: 2020-03-10
is_modified: false
categories:
- AI/ML
tags:
- ImageNet
- 煉丹常識
--- 

在 ImageNet 的相關論文中很常見到兩種指標 <span class="highlighting">Top-1</span> 與 <span class="highlighting">Top-5</span> ，記錄下所代表的涵義。  
(我果然不擅長開頭阿:scream_cat:)
 
<center> <img src="https://i.imgur.com/XeUtj5v.png" alt="模型概覽"></center>
<center class="imgtext">The Learning 模型概覽（圖片來源: <a href="https://keras.io/zh/applications/" class="imgtext">Keras 中文文档</a>）</center>

<!--more-->
<br><br>

## Top-1 accuracy
即對圖片進行預測時，機率最大的結果為正確答案，才視為正確。

<br><br> 
## Top-5 accuracy
對圖片進行預測時，機率排序後的前五的結果中包含正確答案，即視為正確。
 

<br><br> 

## 參考資料 
1. Yannis Assael (2015-06-11)。[ImageNet: what is top-1 and top-5 error rate?](https://stats.stackexchange.com/questions/156471/imagenet-what-is-top-1-and-top-5-error-rate) 。檢自 Stack Exchange (2020-03-10)。