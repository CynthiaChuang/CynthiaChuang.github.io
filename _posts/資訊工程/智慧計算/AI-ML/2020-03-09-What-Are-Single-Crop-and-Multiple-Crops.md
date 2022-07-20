---
title: 關於 AI/ML 中的 single crop / multiple crops
date: 2020-03-09
is_modified: false
categories:
- "智慧計算 › 人工智慧"
tags:
- AI/ML
- 資料前處理
- 資料轉換
- 煉丹常識
--- 

當訓練資料不足時， <mark>multiple crops</mark> 是其中一種資料增強方法。且 multiple crops 也可以避免 Overfitting。
<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/EUxkmxF.png" alt="Desperate">
    Desperate （圖片來源: <a href="https://pixabay.com/photos/youtuber-blogger-screenwriter-2838945/">pixabay - lukasbieri</a>）
</p>



## single crop
又稱 **1-crop**，顧名思義就是進行一次裁剪，將圖片 resize 到某個維度。舉例來說，輸入影像若為不定大小，但網路訓練所需的影像大小卻是 224x224，此時可以選擇 centor crop，從圖片中央位置裁剪出 224x224 大小的圖片。



## multiple crops
最常見的 multiple crops 是 10-corp。它會先執行 centor crop，裁出中間影像，接著依序從左上、右上、左下與右下進行裁剪，緊接著，鏡像反轉輸入圖片，重複此步驟分別截中間、左上、右上、左下與右下五張（把之前截下鏡像反轉也行），合計共 10 張圖片。

<p class="paragraph-spacing"></p> 

另外還有看過 144-crops，這個略為複雜，依照 [ChrisZZ 的解釋](https://www.cnblogs.com/zjutzz/articles/8733044.html)，先將輸入的圖片 resize 成 4 種相異尺寸，例如：256xN、320xN、384xN 與 480xN，並在各尺寸中由左至右截取出各三個正方形區域，針對正方形區域進行 10-corp 裁剪，到此已得 4x3x10=120 張圖片。針對正方形 10-corp 執行完畢後，將此正方形區域直接 resize 成 224x224，並做水平翻轉，如此又得到 4x3x2=24 張，合計共 144 張圖片。
 


## 參考資料 
1. 大D (2018-12-13)。[什么是深度学习中的1-crop和10-crop?](https://www.zhihu.com/question/58217321/answer/551019030) 。檢自 知乎 (2020-03-09)。
2. ChrisZZ (2018-04-07)。[炼丹常识：关于single crop/multiple crops](https://www.cnblogs.com/zjutzz/articles/8733044.html) 。檢自 ChrisZZ - 博客园 (2020-03-09)。

