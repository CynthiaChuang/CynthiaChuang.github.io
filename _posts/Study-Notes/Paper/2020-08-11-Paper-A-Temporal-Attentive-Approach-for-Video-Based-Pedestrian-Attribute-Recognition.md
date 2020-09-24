---
title: "【論文筆記】A Temporal Attentive Approach for Video-Based Pedestrian Attribute Recognition"
date: 2020-08-13
is_modified: false
categories:
- Study-Notes
- AI/ML
tags:
- Papers/Documents
- Pedestrian Attribute Recognition
- CV
--- 

> [A Temporal Attentive Approach for Video-Based Pedestrian Attribute Recognition](https://arxiv.org/pdf/1901.05742.pdf)  
> Chen Z, Li A, Wang    
> Pattern Recognition and Computer Vision, 2019, pp: 209-220

- [程式碼](https://github.com/yuange250/video_pedestrian_attributes_recognition)
- [資料集](http://irip.buaa.edu.cn/mars_duke_attributes/index.html)

<!--more-->
<br><br> 

## 閱讀前自我提問
1. 期望能闡述 Video-Based Pedestrian Attribute Recognition（PAR） 與 Image-Based PAR 的異同與挑戰。
2. 期望能介紹該方面的實做與 STOA。
3. 期望能收集現有的資料集。
 
<br><br> 

## Abstract
1. **目的**  
    研究 Video-Based PAR，並提出網路架構。
2. **挑戰**  
    如何根據<span class='highlighting'>空間和時間建模</span>，並結合兩者進行有效的 Video-Based PAR。
3. **貢獻**  
    1. 提出了基於 CNN 和時間注意力策略的 multi-task 網路架構（a novel multi-task model based on the conventional neural network and temporal attention strategy）。
    2. 重新標註了兩個大型[資料集](http://irip.buaa.edu.cn/mars_duke_attributes/index.html)以用於 Video-Based PAR。


<br><br> 

## Introduction

所謂的 Video-Based 指的是同一運動目標的<span class='highlighting'>連續的靜態影像序列</span>。目前在 PAR 的學術研究上有不錯的成果，但主流方法都是基於單一靜態影像來實現的。而實際的監視場景中，用影片，i.e. 連續的靜態影像序列，可能會較佳。 

在 PAR 的模型中，辨識結果高度依賴輸入的圖像，尤其當輸入的單一靜態影像時。如下圖所示，僅靠單一靜態影像（紅色矩形），可能出現<span class='highlighting'>裁切</span>（i.e. 圖 a 的包包）、<span class='highlighting'>難以識別</span>（i.e. 圖 b、c 的裙子與頭髮）和<span class='highlighting'>遮擋</span>的現象（i.e. 圖 d 的下半身），而導致該屬性難以辨識。但若引入序列資料，可以提供強有力的時間線索，以避免上述的狀況。

<center> <img src="https://i.imgur.com/HEXeWvL.png" alt="Video-Based 和 Image-Based PAR 之間的比較"></center>
<center class="imgtext">Video-Based 和 Image-Based PAR 之間的比較（圖片來源: <a href="https://arxiv.org/pdf/1901.05742.pdf" class="imgtext">論文</a>）</center>
<br>
 
本文的具體貢獻：  
1. 第一個利用影片進行行人屬性識別的方法，並引入時間注意力的方法提昇行人屬性識別的準確率。
2. 重新標註了兩個大型[資料集](http://irip.buaa.edu.cn/mars_duke_attributes/index.html)以用於 Video-Based PAR。

<br><br> 

## Dataset

由於此領域的公開資料集較少，因此作者只好自給自足，對兩個用於行人再識別（ReID）的資料集：**MARS** 和 **DukeMTMC-VideoReID** 進行了重新標註。

MARS 的資料是來自於 6 隻監視器中所監控到 1261 人的 20478 條運動軌跡所組成，是 Market-1501 的擴充集；而 DukeMTMC-VideoReID 的資料則是來自於 8 隻監視器中的 1402 人共 20478 條運動軌跡，是 DukeMTMC-ReID的 的擴充。其中，兩個原始資料集是 Image-Based Dataset，而兩個擴充資料集則是 Video-Based Dataset。

兩個擴充集與原始資料集間，雖皆遵循相同的身分規則，但因 Image-Based 與 Video-Based Dataset 之間的實體不是一對一的對應的，無法直接使用，需要重新標註。另一個需要重新標註的原因是，由於行人在持續的運動，即使是同一個人但在不同的軌跡中，可能某些屬性會出現或消失，因此不能直接按行人的 ID 進行屬性標註，必須檢查當下運動軌跡重新標註屬性。

<center> <img src="https://i.imgur.com/WI137nK.png" alt="MARS 中屬性改變的例子"></center>
<center class="imgtext">MARS 中屬性改變的例子（圖片來源: <a href="https://arxiv.org/pdf/1901.05742.pdf" class="imgtext">論文</a>）</center>
<br>

重標註後的屬性共有 2 類 14 種共 52 個項目，其中第一類是**行為相關屬性**，這屬性與外觀描述無關，但卻會極大地影響外觀的呈現；第二類則是**身分相關屬性**，可以理解成描述此人所會用到的形容詞。

<div class="alert danger"> 
<div class="head">關於重標註後的屬性類別數</div>
論文寫 16 種，但我怎麼算都只有 14 種，因此網誌我寫 14 種。
</div>

下圖是重標註後 14 種屬性，除運動（Motion）與姿態（Post）是行為相關屬性外，其餘皆是身分相關屬性。

<center> <img src="https://i.imgur.com/2F56VgW.png" alt="MARS 行人屬性標註"></center>
<center class="imgtext">MARS 行人屬性標註（圖片來源: <a href="https://arxiv.org/pdf/1901.05742.pdf" class="imgtext">論文</a>）</center>

<br><br> 

## Approach

這章分成了兩部份，第一部份先是介紹了網路架構，第二部份著重介紹了他們所提出的**時間注意力策略**。

### Network Architecture

在 CV 方面，似乎使用遷移學習是起手勢？在這邊作者挑選了<span class='highlighting'>ResNet-50</span>作為骨幹網路，並將最後一個 flatten 層作為 frame-level 的 feature map，然後將網路分成兩個分支分別進行 multi-task learning。

兩個分支的概念對應到前面所提到的<span class='highlighting'>行為</span>與<span class='highlighting'>身分</span>相關屬性的區份，一個分支會進行動作與姿態的識別，另一個分支會進行行人外觀屬性的識別。

這邊將兩種類型屬性區分開，應該是為將兩者的任務參數分開來學習不互相影響與約束。因為這兩種類型的屬性，在學習時會關注特徵不同的部份，這會導致兩種類型屬性對於同一特徵的表示有競爭的情況。

舉例來說，你在跑步又不一定是穿褲子，也有可能是穿裙子，因為兩者並不相關，故應將兩種類型屬性區分；但兩者卻又共用前半部特徵空間，或許是因為會互相影響呈現的方式，例如穿裙子跑步時，步伐可能會與穿褲子時不太一樣，又或者跑步因為下擺劇烈擺動，沒有那麼容易判斷是褲子或裙子。呼應到先前所說的：「<span class='highlighting'>行為相關屬與外觀描述無關，但卻會極大地影響外觀的呈現</span>」。

<center> <img src="https://i.imgur.com/NRMMac1.png" alt="網路架構"></center>
<center class="imgtext">網路架構（圖片來源: <a href="https://arxiv.org/pdf/1901.05742.pdf" class="imgtext">論文</a>）</center>
<br>

訓練時輸入為某行人某軌跡的的圖片序列，可表示為 $I = \{I_1, I_2...,I_n\}$，其中 $n$ 為幀數，每幀的長寬分別為 $(h,w,c) = (224,112,3)$。當圖片序列通過 Resnet-50 後，可以得到 `nx2048×4×7` 的 feature map。此 feature map 會被分別送往兩個分支進一步對空間特徵向量進行提取，得到一個 `nx2048` 的二維矩陣。

<div class="alert danger"> 
<div class="head">關於此段落符號的表示</div>
1. 根據上下文，文中的 n 與圖中的 T ，指得是同一個參數，用來表示幀數。<br>
2. 圖片序列 I 中元素的下標，是從 1 開始，而不是常用的 0。
</div>
<br> 

### Temporal-attention Strategy

在分支中還存在每個屬性判別任務的子分支中，這些子分支是帶有時間注意力機制的小網路。這是為了補全 ResNet-50 無法有效提取時間相關特徵而提出的概念。

在某些幀中，各個屬性可能因為<span class='highlighting'>裁切</span>、<span class='highlighting'>難以識別</span>和<span class='highlighting'>遮擋</span>等現象，無法作為有效的判斷依據。因此不同屬性的識別可能必須依賴不同幀。因而引入時間注意力機制，以產生 `n×1` 的時間注意力向量 $A$ ，用來表示每幀在識別特定屬性中的重要性。然後利用時間注意力向量對每幀的空間特徵進行加權，根據加權後的特徵，得到屬性分類的結果。

<center> <img src="https://i.imgur.com/8aOEopZ.png" alt="時間注意力機制"></center>
<center class="imgtext">時間注意力機制（圖片來源: <a href="https://arxiv.org/pdf/1901.05742.pdf" class="imgtext">論文</a>）</center>
<br>

## Experiments

這邊實驗比較對象是
1. image-based method
2. [3DCNN](https://ieeexplore.ieee.org/document/6165309) 
3. [CNN-RNN model](https://ieeexplore.ieee.org/document/7780517) 

<div class="alert danger"> 
<div class="head">關於比較對象</div>
image-based method 這項我實在沒看懂它究竟是出自於哪個方法？
</div>
<br> 

### Settings

資料集是重新標註 **MARS** 與 **DukeMTMC-VideoReID** 並按照原本訓練/測試集分法，並從資料集中隨機取樣 64 個軌跡，每個軌跡取 6 幀，e.g. $n=6$，並選擇 <span class='highlighting'>Cross Entropy Loss</span> 作為  loss function 、 <span class='highlighting'>Adam</span>  作為  optimizer、 learning rate 取 0.0003。

段落中有一句話，**與連續採樣策略相比，隨機採樣更適合於時間注意力模型，因為它增加了採樣幀之間的差異**，有點看不懂它的意思，不過看其他人的論文筆記，它的隨機採樣似乎還做了隨機剪裁、水平翻轉...等數據增強，不過這段文字我在論文中沒注意到，或許是在程式碼中的實作細節？
<br> 

### Comparison with other Approaches

<center> <img src="https://i.imgur.com/yfk4SKH.png" alt="結果比較"></center>
<center> <img src="https://i.imgur.com/Cr0AHnK.png" alt="結果比較"></center>
<center class="imgtext">結果比較（圖片來源: <a href="https://arxiv.org/pdf/1901.05742.pdf" class="imgtext">論文</a>）</center>
<br>

根據實驗結果：
1. **3DCNN**：不適合用來做 Video-Based PAR，可能是三維 convolution 會丟失軌跡中許多的空間特徵？
2. **3CNN-RNN**： 更適合用於動作識別，因為動作識別非常倚賴時間特徵，而且這些線索也不易通過時間注意力機制來提取。
3. **3Image-Based PAR**： 可能更適合細部特徵提取，忽然想到 Fashion Classification。
4. 總體上來說，本篇論文的方法更好。
<br> 

### Ablation study

這段實驗是用來證明他們引入了**行為與身分屬性分離**與**時間注意力機制**的效能：

<center> <img src="https://i.imgur.com/Q3VZYec.png" alt="結果比較"></center>
<center class="imgtext">結果比較（圖片來源: <a href="https://arxiv.org/pdf/1901.05742.pdf" class="imgtext">論文</a>）</center>
<br>

Temporal Pooling 應該是指用 ResNet50 當骨架直接做 multi-task 訓練，也就是 ResNet50 出來的特徵直接接各個屬性的 conv + pooling 最後做分類；separate channel 指的是行為與身分屬性分離；temporal attention 則是時間注意力機制。

在表格中可以看到，單就兩方法的比較，temporal attention 所能帶來的效益較大，不僅能輸入軌跡中提取出有區別的幀，還能緩解在[網路架構章節中](#network-architecture)所提到特徵競爭問題。不過真的解決特徵競爭問題的是 separate channel，temporal attention 只能達到緩解的效果，在反向傳遞的過程中幫助梯度平滑傳遞到底層，達到類似的效果。


此外綜觀表1、表2和表3可以看出，即使不採用 separate channel 與 temporal attention 兩項方法，Video-based baseline 依舊優於 Image-based baseline，顯示出 Video-based 用於 PAR 的優越性。 

<br><br> 

## 閱讀後紀錄與動作

### 紀錄
1.  **Video-Based PAR 與 Image-Based PAR 的異同與挑戰**
    - **異同**：  
    兩者對於輸入的圖像皆有高度依賴性，可能會因為裁切、難以識別和遮擋的現象，導致屬性分類困難。但在 Video-Based 中引入序列資料，添加時間軸資料，能夠提 PAR 的準確率。
    - **挑戰**：
        1. 公開資料集少，因此本文作者還重新標註了資料庫，如果我們要自己標註自己的資料庫，會所費不貲。但這應該算是 AI/ML 的共同問題。
        2. 雖然有用 ResNet-50 作為骨幹網路，進行遷移學習，但不同在 Multi-task learning 中，針對不同的任務收斂速率應該會不同，在加上 Temporal-attention，會不會導致難以訓練？

2. **期望能介紹該方面的實做與 STOA**
    - 所謂的 Video-Based 指的是同一運動目標的連續的靜態影像序列。
    - 使用 ResNet-50 作為骨幹網路，進行遷移學習，後面依照行為與身分屬性分成兩個分支，並進行 multi-task 訓練，在每個任務的子分支中，又都加入時間注意力機制對每幀的中的特定屬性進行加權。
    - 子分支中網路應該就是兩個 1D-Convolution 做並聯，時間 Temporal-attention 負責壓縮時間、另一邊則負責提取空間特徵。
    - 比較對象有 [CNN-RNN model](https://ieeexplore.ieee.org/document/7780517) 。
   
3. **期望能收集現有的資料集**  
    - 作者有公佈他們重新標註的[資料集](http://irip.buaa.edu.cn/mars_duke_attributes/index.html)，但按照文中訓練時的方法設置訓練與測試集，可能會導致訓練集的資料量遠少於測試集的資料量。
 
<br>

### 動作
1. Study Code
    - Multi-task 的 conv + pooling 的層數與參數沒特別說明，可能要去看看程式碼。
    - 連續採樣策略的部份，也需要看看程式碼了解它的實做，並確定是否有做數據增強。
    - 子分支中網路驗證一下實做，特別是時間注意力機制的部份。
    - 它 Batch 的部份怎 train？ResNet-50 的 input shape 應該只有四個維度，但現在原 Batch 被幀數使用掉了。 
1. Study Multi-task learning
2. Study [CNN-RNN model](https://ieeexplore.ieee.org/document/7780517) 

<br><br>  
 
## 參考資料 
1. huangyiping_dream (2020-04-10)。[行人属性识别：A Temporal Attentive Approach for Video-Based Pedestrian Attribute Recognition](https://blog.csdn.net/huangyiping12345/article/details/105434463) 。檢自 huangyiping12345｜CSDN (2020-08-10)。
2. [A Temporal Attentive Approach for Video-Based Pedestrian Attribute Recognition](https://www.pianshen.com/article/91351109327/) 。檢自 程序员大本营 (2020-08-10)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-13</summary>
  <ul class="timestamp">
    　<li>2020-08-13 發布</li>
    　<li>2020-08-11 完稿</li>
    　<li>2020-08-10 起稿</li>
  </ul>
</details>

