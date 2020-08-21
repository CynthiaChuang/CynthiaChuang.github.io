---
title: "【論文筆記】Convolutional Neural Networks for Fashion Classification and Object Detection"
date: 2020-08-21
modified: 2020-08-21
categories:
- Study-Notes
- AI/ML
tags:
- Papers/Documents
- Fashion Classification
- CV
--- 

> [Convolutional Neural Networks for Fashion Classification and Object Detection](http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf)  
> Lao B , Jagadeesh K（2015）  
> Final Paper, CS231N, Stanford   



<!--more-->
<br><br> 

## 閱讀前自我提問
1. 期望能了解服裝分類領域中的 Know-how，包含但不限於：常用名詞、定義與方法、挑戰與現存的 benchmark

<br><br> 


## Abstract
1. **本文重點放在四個任務**：
    1. 服裝類型（Type）分類
    2. 服裝屬性（Attribute）分類
    3. 相似服裝檢索
    4. 服裝物體（Object）檢測
2. **結果**：
    - 在服裝風格（Style）分類上準確率有 50.0 %
    - 在服裝屬性（Attribute）分類上準確率有 74.5%

我這邊對於**相似服裝檢索**不感興趣，僅專注在服裝類型（Type）與屬性（Attribute）的分類上。是說，說到 Fashion Classification，可以瞄一下 <span class='highlighting'>Fashion MNIST</span> 資料集。
<center> <img src="https://i.imgur.com/oRSeVTN.png" alt=" Fashion MNIST 資料集"></center>
<center class="imgtext"> Fashion MNIST 資料集（圖片來源: <a href="https://github.com/zalandoresearch/fashion-mnist" class="imgtext">zalandoresearch/ fashion-mnist ｜ github </a>）</center>

<br><br> 

## 1. Introduction

段落一開始先介紹 Fashion classification 的應用：
1. 在**電子商務**方面，可以根據服裝照片推薦相似相品，或是推薦設計師作品。
2. 在**監視環境**中，可以利用行人服裝屬性輔助進行行人再識別（ReID）
3. 在**檢索應用**方面，可以使用文字檢索圖片，例如：穿紅衣的小女孩...等
<center> <img src="https://i.imgur.com/HJYJ70U.png" alt="洋裝下擺與單裙下擺"></center>

<br>

<center class="imgtext">洋裝下擺與單裙下擺（左：洋裝、右：單裙）（圖片來源: 左: <a href="https://tw.baddiary.com/product/%E5%BE%8C%E6%8B%89%E9%8D%8A%E9%96%8B%E8%A1%A9%E8%A2%96%E5%A2%8A%E8%82%A9%E7%99%BE%E8%A4%B6%E6%B4%8B%E8%A3%9D%E9%99%84%E8%85%B0%E5%B8%B6/16754/?cafe_mkt=google_tw_dy&utm_source=Google&utm_medium=cpc&utm_campaign=Google_shopping&gclid=Cj0KCQjwg8n5BRCdARIsALxKb94BR8R5d_GGycXTqWv9UyWr_UdzFvFKTQN6VJ4T91wxmY8VS9dNkwYaAgxiEALw_wcB" class="imgtext">baddiary</a>、右:<a href="https://www.google.com/url?sa=i&url=https%3A%2F%2Fworld.taobao.com%2Fproduct%2F%25E5%2596%25AE%25E8%25A3%2599%25E9%25A1%25AF%25E7%2598%25A6.htm&psig=AOvVaw2Nentv0o4g8jWz-gOhqw-y&ust=1597222436780000&source=images&cd=vfe&ved=0CA0QjhxqFwoTCKCcx7LjkusCFQAAAAAdAAAAABAI" class="imgtext">淘寶海外</a>）</center>

並說明了在此領域會遇到的挑戰：
1. 各類衣服會具有<span class='highlighting'>相似的特徵</span>，如：洋裝下擺與單裙下擺。
2. 服裝由於材料的的緣故，衣服容易變形，導致<span class='highlighting'>特徵縮放</span>。
3. 視角與長寬比的不同，容易導致<span class='highlighting'>衣物特徵變形</span>，而看起來不同。

<br><br> 


## 2. Problem Statement

在 [Abstract](#Abstract) 中出現的四種分類 Type、Attribute、Object 與 Style，這邊先進行定義。不過 Type 並沒有標註在圖上，該詞僅用於相似服裝檢索，根據上下文推測指得應該是<span class='highlighting'>服裝的風格</span>，如：淑女、學院、中性、洛麗塔、街頭、簡約...等。

<center> <img src="https://i.imgur.com/Qg6EdSw.png" alt="分類任務的摘要"></center>
<center class="imgtext"> 分類任務的摘要（圖片來源: <a href="http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf" class="imgtext">論文</a>）</center>

<br> 

這篇的四個子任務主要使用 CNN 來實做，不過他這邊提到 **Fashion classification has more generally consisted of nonCNN approaches**？這讓我感到有點驚訝，剛剛看 Pedestrian Attribute Recognition（PAR），兩者都是在辨識身上的屬性，但在 PAR 中有大半的方法引入 CNN。

<div class="alert info"> 
<div class="head">關於 CNN PAR 的時間軸</div>
Oops! 我好像搞混時間軸了。這篇是 2015 的文章，另外一篇被認為是是第一篇的 CNN PAR 論文 - Multi-attribute learning for pedestrian attribute recognition in surveillance scenarios，發表的時間也是 2015，是同一時期的事情。
</div>

<br> 

### 2.1 Clothing Type Classification

服裝類型（Type）分類，是屬於 <span class='highlighting'>multiclass classification</span>，訓練時使用了 [Apparel Classification with Style (ACS)](https://data.vision.ee.ethz.ch/cvl/lbossard/accv12/) 資料集進行訓練，該資料集包含了 89,484 張圖片，主要是集中<span class='highlighting'>上半身</span>衣物的分類，共有 15 個類別。 

<center> <img src="https://i.imgur.com/sfmFXHQ.png" alt="ACS Dataset"></center>
<center class="imgtext"> ACS Dataset（圖片來源: <a href="http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf" class="imgtext">論文</a>）</center>

<br> 

### 2.2 Clothing Attribute Classification

<div class="alert warning"> 
<div class="head">the multi-label CNN architecture we are using</div>
同一段落中有提到這句話，作者認為所使用的是 multilabel？但在一個章節看網路架構圖，比較偏向我所認知的 <b>Multitask Learning</b> 的架構。
</div>



服裝屬性分類就是分類服裝<span class='highlighting'>顏色</span>、<span class='highlighting'>圖案</span>、<span class='highlighting'>長度</span>...等屬性的問題。這些屬性有些可以用二進制量來表達，如：有無領帶；但有些無法，如上裝顏色。感覺資料格式有點類似之前在看的 [Multitask classification](/@CynthiaChuang/Difference-between-Multiclass-Multilabel-and-Multitask-Problem#Multitask-classification)。


這部份網路訓練時採用的資料集是 [Clothing Attribute (CA) Dataset](http://chenlab.ece.cornell.edu/people/Andy/publications/ECCV2012_ClothingAttributes.pdf)，該資料集包含了 1856 張<span class='highlighting'>上身服裝</span>照片，，共有 26 個類別。 

<center> <img src="https://i.imgur.com/rAQeZh7.png" alt="CA Dataset"></center>
<center class="imgtext"> CA Dataset（圖片來源: <a href="http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf" class="imgtext">論文</a>）</center>

<br> 

### 2.3. Clothing Retrieval
Pass

<br> 

### 2.4. Clothing Object Detection

服裝物體檢測，顧名思義就是找出圖片中衣服存在的區域，就像下圖這樣：
<center> <img src="https://i.imgur.com/ENlRAdX.png" alt="Clothing Object Detection"></center>
<center class="imgtext"> Clothing Object Detection（圖片來源: <a href="https://arxiv.org/pdf/1807.01394.pdf" class="imgtext">ModaNet</a>）</center>

<br> 

這邊資料集採用 [Colorful-Fashion(CF) dataset](https://ieeexplore.ieee.org/document/6630093)，但該資料集是 superpixel-labeling，所以在使用前需要先將標籤轉換為 ground-truth bounding box。這邊採用 Selective Search，且  intersection over union (IOU) 設為 0.5 來做標籤的處理。

<center> <img src="https://i.imgur.com/eOPeWwL.png" alt="CF Dataset"></center>
<center class="imgtext"> CF Dataset（圖片來源: <a href="http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf" class="imgtext">論文</a>）</center>

<br> 
 
這邊題到了幾個我不太熟的幾個名詞，稍微紀錄一下：
1. **Superpixel**  
    中文翻作超像素，它是將一系列位置相鄰且顏色、紋理、亮度相似的像素組成的一個區域。這些小區域大多保留可供圖片的做進一步分割的資訊，因此可視為對圖片做基本資訊的抽象。最終所得的圖片會從一張像素級（pixel-level）的圖，變成區域級（district-level）的圖。
    
    <center> <img src="https://i.imgur.com/lThCHJY.jpg" alt="superpixel"></center>
    <center class="imgtext"> superpixel（圖片來源: <a href="https://stackoverflow.com/questions/50264064/gabor-filter-on-on-each-superpixel" class="imgtext">stackoverflow</a>）</center>
    <br> 

- **Selective Search**  
    是個 pixel-based 的圖像分割演算法，詳細執行步驟可以看看這篇[網誌](https://www.jianshu.com/p/99e121c3beb8)。
    
- **IoU（Intersection over Union）**  
    在目標檢測中，IoU 指的是一種衡量指標，是用來計算模型產生的 bounding box 與原先標記的 bounding box 的重疊律。簡單來說就是算匡的準不準，一般來說分數大於 0.5 就可以視為不錯的結果。

<br><br> 

##  3. Technical Approach and Models

作者總共訓練了四個網路分進行四個任務

### 3.1. Clothing Type Classification

這邊他直接採用 AlexNet 進行遷移學習，在這邊的 AlexNet 在論文中是使用 Caff 實做的版本故又稱做 CaffeNet。並使用 ACS 資料集共 89,484 張、15個類別進行 Fine-Tune。

<br> 

### 3.2. Clothing Attribute Classification

屬性分類的部份，它使用 AlexNet 作為基礎，後接 26 個 Softmax 層，進行分類。標準的 Multi-task learning 的網路架構。
   
<center> <img src="https://i.imgur.com/ZXCao4H.png" alt="Clothing Attribute Classification"></center>
<center class="imgtext"> Clothing Attribute Classification（圖片來源: <a href="http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf" class="imgtext">論文</a>）</center>
<br> 

### 3.3. Clothing Retrieval
Pass

<br> 

### 3.4. Clothing Object Detection

物件偵測的部份採用 R-CNN 做遷移學習。並用改標註 ground-truth bounding box 的 CF 資料集進行 Fine-Tune。是說有點奇怪，是作者敘述順序放錯嗎？Object Detection 怎會在最後一步？


<br><br> 

##  4. Results

### 4.1. Clothing Type Classification

在 [ACS 論文](https://data.vision.ee.ethz.ch/cvl/lbossard/accv12/accv12_apparel-classification-with-style.pdf)中，依照特徵提取分法的不同其準確率分別落在 35.03%、 38.29% 與 41.36%。而在本文中 Fine-Tune Full-Connected 與所有的層後，所的的準確率分別 46.0% 和 50.2%，高於論文原始數據。

<center> <img src="https://i.imgur.com/iyhFKam.png" alt="與 ACS 比較服裝分類結果"></center>
<center class="imgtext"> 與 ACS 比較服裝分類結果（圖片來源: <a href="http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf" class="imgtext">論文</a>）</center>

<br> 

### 4.2. Clothing Attribute Classification

分類結果看來對於顏色的分類是較為準確，但是於服裝細節的分類略糟。

<center> <img src="https://i.imgur.com/YCffx0m.png" alt="分類結果"></center>
<center class="imgtext"> 分類結果（圖片來源: <a href="http://cs231n.stanford.edu/reports/2015/pdfs/BLAO_KJAG_CS231N_FinalPaperFashionClassification.pdf" class="imgtext">論文</a>）</center>

<br>

是說作者舉了 placket 跟 solid 的例子，我還認真去查了下這兩個到底是什樣子。

<center> <img src="https://i.imgur.com/JC1ESan.png?1" alt=" placket 跟 solid "></center>
<center class="imgtext">左： placket 右：solid）（圖片來源: 左: <a href="https://www.beautifulhalo.com/mens-simple-long-sleeves-half-button-placket-loose-fit-solid-color-linen-shirt-p-407928.html" class="imgtext">Beautifulhalo</a>、右:<a href="https://www.google.com/url?sa=i&url=https%3A%2F%2Fshopee.tw%2F%25E5%259B%259E%25E8%25B3%25BC%25E7%258E%2587%25E8%25B6%2585%25E9%25AB%2598%25EF%25BC%258C%25E8%25B6%2585%25E4%25BA%25BA%25E6%25B0%25A3%25E6%258E%25A8%25E8%2596%25A6-%25E7%2599%25BD%25E8%25A5%25AF%25E8%25A1%25AB-%25E7%2594%25B7-(%25E5%258F%25B0%25E7%2581%25A3%25E7%258F%25BE%25E8%25B2%25A8-%25E6%259C%2589%25E5%258F%25A3%25E8%25A2%258B)%25E4%25B8%258A%25E7%258F%25AD%25E6%2597%258F%25E7%2599%25BD%25E8%25A5%25AF%25E8%25A1%25AB-%25E9%2595%25B7%25E8%25A2%2596-mcps20-i.6368458.662654313&psig=AOvVaw3v5kCwmoCUStkFuUqkBW-1&ust=1597301305337000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCLjDl5qJlesCFQAAAAAdAAAAABAD" class="imgtext">蝦皮購物</a>）</center>

<br> 

### 4.3. Clothing Retrieval
Pass
<br>

### 4.4. Clothing Object Detection

在使用 R-CNN 做遷移學習時，做了兩階段的 Fine-Tune。在第一階段 Accuracy 達 91.25%、第二階段 達 93.4％

<br><br> 

### 5. Discussion
簡略紀錄下幾點：
1. 當圖片具有許多重疊特徵時，手動標記圖像可能會涉及一些主觀性，導致分類失誤。
2. 屬性分類中，目前顏色方面表現很好，但涉及細微屬性時，就有代加強。如果能建立一個更大的資料集，或許將能幫助模型學習。

<br><br> 

## 參考資料 
1. Linear_Luo (2016-09-19)。[超像素(Superpixel)理解](https://blog.csdn.net/Linear_Luo/article/details/52588515) 。檢自 Linear_Luo的专栏 ｜ CSDN博客 (2020-08-12)。
2. studyeboy (2019-06-28)。[超像素—学习笔记](https://blog.csdn.net/studyeboy/article/details/93981017) 。檢自 studyeboy的专栏 ｜ CSDN博客 (2020-08-12)。
3. hanranV (2016-08-05)。[检测评价函数 intersection-over-union （ IOU ）](https://blog.csdn.net/Eddy_zheng/article/details/52126641) 。檢自 hanranV的专栏 ｜ CSDN博客 (2020-08-12)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-21</summary>
  <ul class="timestamp">
    　<li>2020-08-21 發布</li>
    　<li>2020-08-12 完稿</li>
    　<li>2020-07-31 起稿</li>
  </ul>
</details>
