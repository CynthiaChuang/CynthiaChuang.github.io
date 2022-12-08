---
title: "【論文筆記】Pedestrian Attribute Recognition : A Survey"
date: 2020-12-21 22:19
is_modified: false
categories:
- "智慧計算 › 人工智慧"
tags:
- AI/ML
- 行人屬性辨識 PAR
- 電腦視覺 CV
- 讀書筆記
- 文獻
--- 
 
> [Pedestrian Attribute Recognition: A Survey](https://arxiv.org/abs/1901.07474)  
> Wang X, Zheng S, Yang R, et. al.  
> 2019  

- [Blog](https://sites.google.com/view/ahu-pedestrianattributes/)
- [Github](https://github.com/wangxiao5791509/Pedestrian-Attribute-Recognition-Paper-List) 

<!--more-->


## 閱讀前自我提問
1. 期望能了解 Pedestrian Attribute Recognition 領域中的 Know-how，包含但不限於：
    - 研究現狀
    - 常用方法
    - 相關概念和挑戰 
    - 現存的benchmark 

2. 希望這篇當 Pedestrian Attribute Recognition 的敲門磚夠大！



## Abstract
說明本文目的，並根據傳統或深度學習網路的方法來說明下列事項：
1. Pedestrian Attribute Recognition （行人屬性辨識，以下簡稱 PAR）的基本概念和所面對的挑戰。
2. 現存的 benchmark，包含 datasets 與評估指標。
3. 分析 multi-task and multi-label learning 概念，與行人屬性辨識之間的關係。
4. 目前主流的解決方案，如：attributes group、part-based。
5. 對於其他領域將屬性納入考慮後，識別結果更好？
6. 提出可能的研究方向。

<br class="big">

下方的 Index Terms 有幾個不太熟悉的字彙： <mark>Graph Convolutional Network</mark> 與 <mark>Visual Attention</mark>。 

1. **Visual Attention**  
    這好猜，應該就是引入了 Attention，只是我之前不會特地區分是不是用在 CV 還是 NLP，雖然兩種都有看過 XDDD，說到 CV 的 Attention，忽然想到 Grad Cam 的熱力圖。~~史嘉蕾·喬韓森真的很漂亮 :heart_decoration:~~   
    
    <p class="illustration">
        <img src="https://i.imgur.com/m9ufMlz.png" alt="Grad Cam 的熱力圖">
        Grad Cam 的熱力圖（圖片來源: <a href="https://kknews.cc/zh-my/science/qorxb2b.html">每日头条</a>）
    </p>

2. **Graph Convolutional Network**  
    至於 Graph Convolutional Network，看起來是跟圖學相關的網路，先註記一下之後再來研究。  
    
    <div class="alert info"> 
    <div class="head">相關閱讀</div>
    <a href="https://medium.com/@yenhao0218/%E6%B7%B1%E5%BA%A6%E5%9C%96%E5%BD%A2%E5%8D%B7%E7%A9%8D%E7%B6%B2%E8%B7%AF-deep-learning-on-graphs-with-graph-convolutional-networks-part-1-69d98fe17995">深度圖形卷積網路 Deep Learning on Graphs with Graph Convolutional Networks | by 黃彥皓 | Medium</a>
    <br>
    </div>   



## Introduction ＆ Problem Formulation and Challenges

論文一開始先就 <mark>Attribute</mark> 與 <mark>Pedestrian Attribute Recognition</mark> 定義開始說起。


### Problem Formulation
**Attribute**    
屬性是屬於人類可以搜索的語義描述，與方向梯度直方圖（Histogram of Oriented Gradient, HOG）、局部二值模式（Local Binary Pattern，LBP）或深層特徵等低階特徵不同，屬性可以視為高階語義，它對視角變化和觀看條件多樣性（？）更具健壯性。
 
而 Attribute 可用作視覺監控中的 soft-biometrics，並應用於行人重新識別、面部驗證和人類識別，以實現更好的性能。
 
<div class="alert info"> 
<div class="head">soft-biometrics</div>
我找不到正式對應的翻譯。倒是找到幾篇網誌直翻成<b>軟生物識別</b>，感覺有點怪怪的。根據<a href="https://en.wikipedia.org/wiki/Soft_biometrics">維基百科</a>對它的定義為，一組可以提供個人資訊的特徵，但這些特徵可能缺乏獨特性和持久性。
<br>
</div> 

<br class="big">

**Pedestrian Attribute Recognition**   
而行人屬性辨識目的在於，從中挖掘出目標人物圖像中所能被觀察到的一些高階語義描述，如性別、年齡、服飾、配件等，如下圖所示：

<p class="illustration">
    <img src="https://i.imgur.com/RAzm5mx.png" alt="PAR Define">
    論文架構圖（圖片來源: <a href="https://arxiv.org/abs/1901.07474">論文</a>）
</p>

更正式的定義如下：

> 給定一張人物圖像 $I$，PAR 目的是從從預先定義的屬性列表 $A= \{a_1, a_2, ..., a_L\}$ 預測一組屬性 $a_i$ 來描述此人的特徵。


### 1.2 Challenges  
由於外觀多樣性（appearance diversity）和外觀歧義性，導致類別間差異（intra-class variations）頗大，這編列出了一些在訓練會遇到的問題：

1. **Multi-views**  
    由於攝影機放在不同的進行監測，所以會產生<mark>不同角度採樣</mark>的問題，在加上行人動作變化多端，這使得屬性識別變的更加複雜。

2. **Occlusio**  
    人體可能被其他障礙物或是行人遮擋，導致部分屬性被遮蔽，或是引入障礙物的特徵，導致訓練難度增加。如果是障礙物是其他行人，可能還會引入其他人特徵。
    
    另外一個我想到的可能比較偏向實做的問題，一般從大圖剪裁人物圖像出來的時候，不一定會像資料集裁剪的那剛好。有可能裁太小，導致部分特徵被裁到，另如後背包；或是裁太大，納入了太多雜訊，感覺這狀況也跟遮蔽物的情況相似。
 
3. **Unbalanced Data Distribution**  
    應該是在說<mark>類別不平均</mark>的問題。
 
4. **Low Resolution**  
    低解析度問題影像圖像品質。好的攝影機太貴了，沒辦法大量布置 XDDD
    
5. **Illumination**  
    光照問題。光源條件不同，可能導致白天影子干擾，把影子也認成一個人；或是晚上感光度太差。

6. **Blur**  
    目標移動時，圖像會模糊。
    

不過感覺這些問題，不是專屬於行人屬性辨識，之前在做車牌辨識的時候就遇過類似的問題的，感覺這些問題更像是辨識領域中共同的問題。

阿，除了行人跟行人之間的遮擋問題，車牌辨識中兩塊車牌還沒那容易擋在一起。


### 1.3 Address Issues  
本節尾端說明了一下本文所要探討的目標： 
1. 探討傳統行人屬性辨識與深度學習行人屬性辨識的異同？
2. 行人屬性辨識如何應用在其他 CV 任務上？
3. 如何更好利用深度學習網路來進行行人屬性辨識？
4. 行人屬性辨識的未來發展方向？

順便附上一張論文架構圖
<p class="illustration">
    <img src="https://i.imgur.com/yxxvCsJ.png" alt="論文架構圖">
    論文架構圖（圖片來源: <a href="https://sites.google.com/view/ahu-pedestrianattributes/">論文 Blog</a>）
</p>



## Benchmarks
這裡介紹了分兩部分一是資料集、二是評估指標。


### Dataset
與其他 CV 的領域不同，行人屬性辨識的標注含蓋了不同級別的標籤。
1. **低級屬性**  
    這些具體的形容詞，例如：長短髮、有無帽子、有無眼鏡、服裝顏色...等，且這些屬性對應於圖像的不同區域。    
2. **高級屬性**  
    另外有些屬性抽象的，它無法對應到特定區域，或者說要看完全圖才能推定的屬性，例如：性別、年齡...等。

不過看完資料集，覺得這些屬性可以在分成兩類：
1. **動作描述**  
    例如：走、跑、坐、跳...等。這些屬性與外觀描述無關，但卻會極大地影響外觀的呈現。
2. **外觀描述**  
    可以理解成描述此人所會用到的形容詞。
    
<p class="illustration">
    <img src="https://i.imgur.com/cyJXVyc.png" alt="Dataset Overview">
    Dataset Overview（圖片來源: <a href="https://sites.google.com/view/ahu-pedestrianattributes/">論文 Blog</a>）
</p>

文章中列了將近 10 個 dataset，不過其中最著名的 RAP 與 PETA，還有 2017 公布的 PA-100K 。下列排序按照論文排序，不做調整：

1. **PETA datase**  
    - [Paper](http://personal.ie.cuhk.edu.hk/~pluo/pdf/mm14.pdf") / [Blog](http://mmlab.ie.cuhk.edu.hk/projects/PETA.html)


    2014 年發布的資料集，是第一個行人屬性辨識，由 10 個人新識別資料集集結而成，共 19000 張圖像，圖像解析度從 `17x39` 至 `169x365`；其中含有 8705 位行人，每位行人具有 61 個二元屬性和 4 多分類屬性；圖像被隨機劃分為 9,500 張用於訓練、1,900 張用於驗證和 7600 張用於測試。

    <p class="illustration">
        <img src="https://i.imgur.com/2RydrgN.jpg" alt="Composition of PETA datase">
        Composition of PETA datase（圖片來源: <a href="http://mmlab.ie.cuhk.edu.hk/projects/PETA.html">PETA Blog</a>）
    </p>
    
    <p class="illustration">
        <img src="https://i.imgur.com/XSt8Am8.jpg" alt="Sample Images in PETA Dataset">
        Sample Images in PETA Dataset（圖片來源: <a href="http://mmlab.ie.cuhk.edu.hk/projects/PETA.html" >PETA Blog</a>）
    </p>
    
    目前的研究多從 PETA 中選出 35 個屬性進行識別，至於確切屬性可以參考下表，或是閱讀 [Dataset 的 ReadMe](https://www.dropbox.com/s/52ylx522hwbdxz6/PETA.zip?dl=0&file_subpath=%2FReadMe.txt) 有較完整描述：

    <p class="illustration">
        <img src="https://i.imgur.com/WboUDgM.png?1" alt="Attribute Groups">
        Attribute Groups（圖片來源: <a href="hhttps://blog.csdn.net/Code_Mart/article/details/87721803#t1">不进则退｜CSDN博客 Blog</a>）
    </p>
    
    不過這資料集有個問題是，它只會對同一個行人標注一次，但由於行人是在持續的運動，在運動過程中某些屬性可能會出現、消失、不可見。但在這資料集中，不論任何時間，他們全部擁有相同的屬性。
     

2. **PARSE27K**   
    - [Paper](https://openaccess.thecvf.com/content_iccv_2015_workshops/w11/papers/Sudowe_Person_Attribute_Recognition_ICCV_2015_paper.pdf) / [Blog](https://www.vision.rwth-aachen.de/page/parse27k)


    共包含 27,000 位行人圖像，圖像解析度為 `64x128`，每位行人具有 10 種屬性；圖像被隨機劃分 50％ 用於訓練、25％ 用於驗證和 25％ 用於測試。這資料集對所有的屬性都加了 N/A 的類別，用於出現遮擋、圖像邊界或任何其他原因而無法確定的狀況。
    
    | Group |  Attributes|  
    | -------- | -------- | 
    |Gender  | male, female, N/A|
    |Posture  | standing, walking, (sitting), N/A|
    |Orientation  | 4 discretizations , N/A|
    |Orientation8 | 8 discretizations , N/A|
    |Bag on Left Shoulder | yes, no, N/A|
    |Bag on Right Shoulder | yes, no, N/A|
    |Bag in Left Hand | yes, no, N/A|
    |Bag in Right Hand | yes, no, N/A|
    |Backpack | yes, no, N/A|
    |isPushing | yes, no, N/A|

    <p class="illustration">
        <img src="https://i.imgur.com/Je60V7m.png" alt="Sample Images in PARSE27K Dataset">
        Sample Images in PARSE27K Dataset（圖片來源: <a href="https://www.vision.rwth-aachen.de/page/parse27k">PARSE27K Blog</a>）
    </p>

3. **RAP/RAP-2.0**  
    - Paper： [RAP](https://arxiv.org/abs/1603.07054) / [RAP 2.0](https://ieeexplore.ieee.org/document/8510891)
    - Blog： [RAP](http://www.rapdataset.com/rapv1.html)/ [RAP 2.0](http://www.rapdataset.com/rapv2.html)


    不同於前面兩個資料集，RAP 是屬於真實室內收集的，RAP v1.0 其中包含 41,585 張圖像，圖像解析度為 `36×92` 到 `344×554` 不等，每位行人具有 69 個二元屬性和 3 個多分類屬性；其中 33,268 張圖像用於訓練，其餘用於測試。而 RAPv2.0 是 RAP v1.0 的擴展版，該資料集包含 84,928 張圖像，並加上了個人 ID 的部分。
     
    比較特別的是 3 個多分類屬性，分別是 viewpoints、occlusions 與 body parts information：

    <p class="illustration">
        <img src="https://i.imgur.com/xPna7Rb.png" alt="viewpoints and occlusion of the annotated">
        viewpoints and occlusion of the annotated （圖片來源: <a href="https://arxiv.org/pdf/1603.07054.pdf">RAP v1 Paper</a>）
    </p>
     
    <p class="illustration">
        <img src="https://i.imgur.com/2VFoFrI.png" alt="vSamples with body part annotations.">
        Samples with body part annotations. （圖片來源: <a href="http://www.rapdataset.com/rapv2.html">RAP v2 Blog</a>）
    </p>
     
    其餘屬性：
    <p class="illustration">
        <img src="https://i.imgur.com/sdKDEYn.png" alt="Overall annotations in the RAP dataset v2.0">
        Overall annotations in the RAP dataset v2.0（圖片來源: <a href="https://ieeexplore.ieee.org/document/8510891">RAP v2 Paper</a>）
    </p>

    RAP 是 2016 年提出的行人屬性辨識資料集，算是截至 2017 年底最大的資料集。現階段所有的 SOTA 都會在 RAP 上測試，並把它當作第一指標。不過這個資料集要下載得填寫[申請單](https://docs.google.com/forms/d/e/1FAIpQLSc-lcbWv_JFTeUYmqig15RQOXHdZlXWbXvi8OigDNWzkYB2xA/viewform)。

4. **HAT dataset**  
    - [Paper](https://jurie.users.greyc.fr/papers/11_bmvc_%20discriminative_spatial_representation.pdf) /  [Blog](https://jurie.users.greyc.fr/datasets/hat.html)


    不同於前面幾個是監視場景的截圖，這邊是由 Flickr 照片所組成的。該資料集包含 9344 張圖像，其中 3500、 3500 和 2344 張圖像分別用於訓練、驗證與測試。每位行人具備 27 種屬性，包含姿勢、年齡、服裝大類。一樣要寫信索取。

    | Group    | Attribute                                                                                                                  |
    | -------- | -------------------------------------------------------------------------------------------------------------------------- |
    | Sex      | female                                                                                                                     |
    | Pose     | frontalpose, profilepose, turnedback, upperbody, standing, runwalk, crouching, sitting, armsbent                           |
    | Age      | elderly, middleaged, young, teen, kid, baby                                                                                |
    | Clothing | tanktop, tshirt, mensuit, longskirt, shortskirt, shortskirt, smallshorts, lowcuttop, swimsuit, weddingdress, bermudashorts |

    <p class="illustration">
        <img src="https://imgur.com/y2URKQn.png" alt="HAT dataset">
        HAT dataset （圖片來源: <a href="https://jurie.users.greyc.fr/datasets/hat.html">HAT Blog</a>）
    </p>

5. **APiS dataset**  
    - [Paper](https://openaccess.thecvf.com/content_iccv_workshops_2013/W10/papers/Zhu_Pedestrian_Attribute_Classification_2013_ICCV_paper.pdf) / [Download](http://www.cbsr.ia.ac.cn/english/APiS1.0_agreement.pdf)
 

     從 4 個資料集整合而來：KITTI dataset, CBCL Street Scenes（CBCLSS for short） dataset, INRIA database 與 SVS dataset （Surveillance Video Sequences at a train station）。
     
     用行人檢測方法截出行人區域，並刪掉品質不佳的圖像。最後得到 3661 張圖像，每張圖像高度大於 90、寬度大於 35。每位行人帶有11 個二元屬性與 2 個多類別屬性。寫信索取。

    <p class="illustration">
        <img src="https://i.imgur.com/N26gCEu.png" alt="APiS dataset">
        APiS dataset （圖片來源: <a href="https://openaccess.thecvf.com/content_iccv_workshops_2013/W10/papers/Zhu_Pedestrian_Attribute_Classification_2013_ICCV_paper.pdf">APiS paper</a>）
    </p>

    <p class="illustration">
        <img src="https://i.imgur.com/V3Wcfhn.png" alt="APiS Sample">
        APiS Sample （圖片來源: <a href="https://openaccess.thecvf.com/content_iccv_workshops_2013/W10/papers/Zhu_Pedestrian_Attribute_Classification_2013_ICCV_paper.pdf">APiS paper</a>）
    </p>

6. **Berkeley-Attributes of People Dataset, BAP**  
   - [Paper](https://ieeexplore.ieee.org/document/6126413) / [Blog](https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/shape/poselets/)

    資料來自於 H3D dataset 和 PASCAL VOC 2010。它將不同圖片剪裁後，在 H3D 與 PASCAL 比例不變的前提下，分成 2003、 2010 和 4022 張圖像分別用於訓練、驗證與測試。每個圖像共有 9 種標記。
    
    好像有提供邊界？
    <p class="illustration">
        <img src="https://i.imgur.com/DNE56x2.png" alt="BAP Sample">
        BAP Sample （圖片來源: <a href="https://ieeexplore.ieee.org/document/6126413">BAP paper</a>）
    </p> 
    
7. **PA-100K**  
    - [Paper](https://arxiv.org/pdf/1709.09930.pdf) / [Github](https://github.com/xh-liu/HydraPlus-Net/blob/master/README.md) / [Download](https://drive.google.com/drive/folders/0B5_Ra3JsEOyOUlhKM0VPZ1ZWR2M) 


    PA-100K 是在 2017 年底公開專門針對行人屬性辨識中的最大的資料集，包含了 10 萬張行人圖像，解析度範圍從 `50x100` 到 `758x454`，但資料集只標注了 26 個屬性。訓練、驗證和測試集的比例為 8：1：1 。

    <p class="illustration">
        <img src="https://i.imgur.com/DXvDqqY.png" alt="PA-100K Sample">
        PA-100K Sample （圖片來源: <a href="http://www.cbsr.ia.ac.cn/users/jwan/papers/TIP2019-PedAttri.pdf">BAP paper</a>）
    </p> 
   
8. **WIDER Attribute Dataset**   
    - [Paper](http://personal.ie.cuhk.edu.hk/~ccloy/files/eccv_2016_human.pdf) / [Blog](http://mmlab.ie.cuhk.edu.hk/projects/WIDERAttribute.html) 


    包含了 30 個場景 13,789 張圖像，每張圖像都帶有邊界框，總共有 57524 個框，平均每個張圖像有 4 個以上的框，最多可以容納 20 個人；每個人都有 14 個不同的屬性標記。該資料集分為 5,509 張訓練集、 1,362 張驗證和 6,918 張測試。
    
    <p class="illustration">
        <img src="https://i.imgur.com/mUBisPA.jpg" alt="WIDER Attribute Sample">
        WIDER Attribute Sample （圖片來源: <a href="http://mmlab.ie.cuhk.edu.hk/projects/WIDERAttribute.html">WIDER Blog</a>）
    </p> 
    
    14 種二元屬性如下：      
    <p class="illustration">
        <img src="https://i.imgur.com/T4ImrfW.png" alt="Attribute">
        Attribute （圖片來源: <a href="http://personal.ie.cuhk.edu.hk/~ccloy/files/eccv_2016_human.pdf">WIDER Paper</a>）
    </p> 

9. **Market1501-attribute**  
    - [Paper](https://arxiv.org/pdf/1703.07220.pdf) / [Github](https://github.com/vana77/Market-1501_Attribute)


    資料集包含了 1,501 個 id 和 21,668 的邊界框，其中 751 個 id 用於訓練、750 個 id 用於測試，分別有 12,936 與 19,732 張圖像。每位行人共有 28 個屬性，其中一個屬性為 image_index，因此僅有 27 個屬性可供訓練。    
    
    <p class="illustration">
        <img src="https://imgur.com/o0MtfuN.png" alt="Market1501 Attribut">
        Market1501 Attribute （圖片來源: <a href="https://github.com/vana77/Market-1501_Attribute">Market1501 Github</a>）
    </p> 
    
    不過注意的是，儘管上下半身服裝分別有 8 、9 個屬性，但對於一種身分，只有一種顏色標記為**是**。

    <p class="illustration">
        <img src="https://i.imgur.com/m9CvkzV.png" alt="Market150 Sample">
        Market1501 Sample （圖片來源: <a href="https://github.com/vana77/Market-1501_Attribute">Market1501 Github</a>）
    </p> 

10. **DukeMTMC-attribute**  
    - [Paper](https://arxiv.org/pdf/1703.07220.pdf) / [Github](https://github.com/vana77/DukeMTMC-attribute)

    跟 Market1501 出自同一篇論文，資料集中 1,812 個 id 和 34,183 個邊界框，其中 702 個 id 用於訓練、1,110 個 id 用於測試，分別有 16,522 與 17,661 張圖像。 不算 image_index，每位行人有 24 個屬性可供訓練。
    
    <p class="illustration">
        <img src="https://i.imgur.com/oHYvXuo.png" alt="DukeMTMC Attribut">
        DukeMTMC Attribute （圖片來源: <a href="https://github.com/vana77/DukeMTMC-attribute">DukeMTMC Github</a>）
    </p> 
        
    <p class="illustration">
        <img src="https://i.imgur.com/rHU8ZfZ.png" alt="DukeMTMC Sample">
        DukeMTMC Sample （圖片來源: <a href="https://github.com/vana77/DukeMTMC-attribute">DukeMTMC Github</a>）
    </p> 

11. **CRP Dataset**  
    - [Paper](https://arxiv.org/pdf/1605.06177.pdf) / [Blog](http://www.vision.caltech.edu/~dhall/projects/CRP/)
    
    <p class="illustration">
        <a href="http://www.youtube.com/watch?v=aBjMwOr5gnA">
        <img src="http://img.youtube.com/vi/aBjMwOr5gnA/0.jpg" alt="CRP Example">
        </a> 
    </p> 

    這資料集有點特別，它是從車內向外側拍行人。資料集包含了 7 支 video 27,754 個邊界框，其中 4 支影片為訓練集，其餘 3 支影片為測試集。每個行人有 4 種標籤屬性年齡、性別、體重和服裝類型，每種標籤下又有不同的 labels:
    
    <p class="illustration">
        <img src="https://i.imgur.com/WFtcsNk.png" alt="DukeMTMC Sample">
        DukeMTMC Sample （圖片來源: <a href="https://github.com/vana77/DukeMTMC-attribute">DukeMTMC Github</a>）
    </p> 

12. **Clothing Attributes Dataset, CAD** 
    - [Paper](http://chenlab.ece.cornell.edu/people/Andy/publications/ECCV2012_ClothingAttributes.pdf) / [Blog](https://purl.stanford.edu/tb980qz1002)

    圖片來源來自 Sartorialist 和 Flickr，其中包含了 1,856 張圖像，並透過 Amazon Mechanical Turk 發布進行標記，共標記了 26 種屬性，其中 23 種是二元屬性和 3 個多類屬性。  

    <p class="illustration">
        <img src="https://i.imgur.com/rAQeZh7.png" alt="CA Dataset">
        CA Dataset（圖片來源: <a href="http://chenlab.ece.cornell.edu/people/Andy/publications/ECCV2012_ClothingAttributes.pdf">CA Dataset 論文</a>）
    </p> 


### 2.2 Evaluation Criteria
目前常使用的有兩種：

1.  **mean acccuracy (mA)**  
    $mA$ 分別計算每個**屬性**正負樣本分對的比例，再將兩者平均作為這一個屬性的精準度，最後再將所有屬性取平均做為最後的值 $mA$ 指標。這種以**屬性**下去算的方式，被稱作 <mark>label-based</mark> 的評估方式。
    
    具體公式如下： 

    $$
    mA  = \cfrac{1}{2N}  \sum^L_{i=1}(\cfrac{TP_i}{P_i} + \cfrac{TNi}{N_i} )
    $$
    
    其中，$L$ 是屬性的數量，$P_i$ 和 $TP_i$ 分別為正樣本數與分對的正樣本數；同理，$N_i$ 和 $TN_i$ 則為負樣本與分對的負樣本。
    
2. **Example-based evaluation**  
    但 label-based 的評估方式並<mark>沒有</mark>考慮到屬性之間的關聯性，因此提出了以每個樣本為基礎的評估方式：
    
    $$
    \begin{aligned}	
    Acc_{exam} &= \cfrac{1}{N}  \sum^N_{i=1}(\cfrac{\vert Y_i \bigcap f(x_i)\vert}{\vert Y_i \bigcup	 f(x_i)\vert})  \\
    Prex_{exam} &= \cfrac{1}{2N}  \sum^N_{i=1}(\cfrac{\vert Y_i \bigcap f(x_i)\vert}{\vert f(x_i) \vert})  \\
    Rec_{exam} &= \cfrac{1}{2N}  \sum^N_{i=1}(\cfrac{\vert Y_i \bigcap f(x_i)\vert}{\vert Y_i \vert})  \\
    F1 &= \cfrac{2 * Prex_{exam} * Rec_{exam} }{Prex_{exam}  + Rec_{exam}}
    \end{aligned}  
    $$ 
    
    其中 $N$ 是所有樣本數， $Y_i$ 是第 $i$ 個樣本中 ground truth 為 positive 的 label，$f(x)$ 則是第 $i$ 個樣本中預測結果為 true 的 label。
    
    $Acc_{exam}$ 看起可以表示成：
    
    $$
    Acc_{exam} = \cfrac{1}{N}  \sum^N_{i=1}(\cfrac{TP}{TP+FP+FN})  \\    
    $$
    
    與原先 [accuracy 的公式](/@CynthiaChuang/Common-Evaluation-MetricAccuracy-Precision-Recall-F1-ROCAUC-and-PRAUC)相比公式中少了 TN，會使得的分數算出來比按照原公式的分數還低，這應該是為了因應 Multilabel 中，為 0 資料較多所特地剔除的。
    
    不過剩下兩個 recall 跟 precision 的公式，我越看越奇怪，$1/2$ 倒底哪來的？  
    
    以 recall 來說 $\sum$ 後面是很標準定義，即<mark>在原本 positive 的資料中被預測出多少</mark>，每個樣本的 recall 相加算平均，怎也沒有 $1/2$ 的事，論文中也沒特地提及這個。
    
    所以只好去看看出處的公式...恩看來是打錯了呢 XD
    
    <p class="illustration">
        <img src="https://i.imgur.com/U0fVSKe.png" alt="Example-based evaluation*">
        Example-based evaluation*（圖片來源: <a href="https://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/tkde13rev.pdf">原論文</a>）
    </p> 

3. **其他指標**  
    除了這兩種外，在部分論文中有會採用 <mark>Receiver Operating Characteristic (ROC)</mark> 與 <mark>Area Under the average ROC Curve (AUC)</mark> 作為指標。 



## 3. Regular Pipeline for PAR
因為行人屬性標注的多樣性，有些屬性是 Binaryclass 或是 Multiclass（加了 NA 屬性），有些則是 Multilabel 的標法。最直觀的方法獨立學習每個屬性，不過這樣應該會 train 到天荒地老 XDDD

目前最主流的方法是用 Multitask，它將每個屬性估計視為一項任務，用一個模型搞定所有屬性。另外也有人用 Multilabel 的方式來進行。


### 3.1  Multitask Learning
在現實中，許多事務是相關聯的，例如，在行人屬性中性別和衣服樣式是互相關聯的。

若獨立處理單一任務很容易忽略這種相關性，可能導致最終性能的調校會遇到瓶頸，通過進行一定程度共享不同任務間的參數與特徵，可能會使原任務的泛化效果更好。而在多任務學習中，主要分成兩種方法： <mark>Hard parameter sharing</mark> 與  <mark>Soft parameter sharing</mark>。

<p class="illustration">
    <img src="https://i.imgur.com/vnCzxiq.png" alt="Multitask Learning 兩種方法">
    左、Hard parameter sharing 右、Soft parameter sharing （圖片來源: <a href="https://arxiv.org/pdf/1901.07474.pdf">論文</a>）
</p> 

1. **Hard parameter sharing**  
    聽說這概念是在 1993 年提出，已經 26 歲了。一般會在底層共享參數，在頂層的特定任務層中使用自己獨有的參數。相對於非共享參數，在這種情況，共享參數的應用會使 overfitting 的機會將低。
    
    這個忽然讓我想到 BERT 的預訓練方法，它們也是同時進行兩個任務：克漏字填空與下個句子預測，最後把共享參數的網路丟出，讓大家做兩階段遷移學習。
    
2. **Soft parameter sharing**  
     而 Soft parameter sharing 可以看做是 Hard parameter sharing 的另一個極端。每個任務有自己的參數，最後通過引入正規化，例如 L2 和 trace norm，對不同任務參數間的差異加約束。
     

### 3.2  Multilabel Learning
目前已有的 Multilabel Learning 問題求解策略大致可以分為以下三類：
1. **一階策略**  
    一階策略是最簡單的形式，直接將 Multilabel Learning 分解成 $N$ 個獨立 Binary class Learning 來建構 Multilabel Learning 的模型，這樣的方法好處是<mark>效率高且易實現</mark>，但它忽略的屬性之間關係，泛化能力較差，效能也會受影響。

2. **二階策略**  
    該類策略考慮屬性之間的相關性，比如為相關屬性和不相關屬性間的排序關係。這樣的方法在一定程度上察覺了屬性間的相關性，因此泛化性能較佳。
    
3. **高階策略**  
    考慮所有屬性之間的關聯，並通過對每個標籤對其他標籤的影響進行建模，來實現多標籤識別系統。泛化性最佳，但是具有很高的複雜度，難以處理大規模學習問題。

<br class="big"> 

若是依照演算法的核心來看，目前 Multilabel Learning 大致可以成兩類：

<p class="illustration">
    <img src="https://i.imgur.com/yIZalqZ.png" alt="代表性的多標籤學習算法的分類">
    代表性的多標籤學習算法的分類 （圖片來源: <a href="https://arxiv.org/pdf/1901.07474.pdf">論文</a>）
</p> 

1. **問題轉換, Problem Transformation**  
   該方法個核心是<mark>改造資料適應演算法，fit data to algorithm</mark>，將 Multilabel  Learning 問題轉換為其它已知的學習問題進行求解。
   
   依照剛剛的求解策略列舉一些代表性演算法，一階方法，該方法將 Multilabel  Learning 學習問題轉化為 Binary Classification 問題求解；二階方法，則是問題轉化校準標籤排名演算法，為**標記排序（label ranking）**，它考慮了成對標籤之間的相關性，然後進行排序；另一個二階方法是分類器鏈演算法，這演算法的基本思想是將問題轉化為二進制分類鏈，每個二進制分類器取決於其鏈中的上一個；高階方法，隨機 k-Labelsets 演算法，該方法將問題轉化為 MultiClass Classification 問題求解，這是個集合分類問題，在每一個集合裡面是一個多分類器，並且多分類器需要學習的分類是所有標籤的子集。

2. **自適應算法, Algorithm Adaptation**  
    該方法個核心是<mark>改造演算法適應資料，fit algorithm to data</mark>，如多標籤KNN，多標籤決策樹，多標籤SVM 。

 

## 4. Deep Neural Networks
介紹一些已經或者可能用於行人屬性辨識的著名的結構：
1. LeNet
2. AlexNet
3. VGG
4. GoogleNet
5. Residual Network
6. Dense Network
7. Capsule Network
8. Graph Convolutional Network
9. ReNet
10. Recurrent Neural Network, RNN



## 5. The Review of PAR Algorithms
他們這邊將論文分成了八面向：
1. global based
2. local parts based
3. visual attention based
4. sequential prediction based
5. new designed loss function based
6. curriculum learning based
7. graphic model based
8. others algorithms

就不每一篇都介紹了...實在太多，我還沒把他們都看完呢...，之後再把筆記連結附上來吧...如果我的拖延症沒發作的話...。


### 5.1 Global Image-based Models
<p class="illustration">
    <img src="https://i.imgur.com/FSdgNto.png" alt="SThe overall pipeline of DeepSAR and DeepMAR">
    The overall pipeline of DeepSAR and DeepMAR [（圖片來源: <a href="https://arxiv.org/pdf/1901.07474.pdf">論文</a>）
</p> 


Global Image-based Models，顧名思義就是整張圖下去訓練，不做任的剪裁。這些模型的好處是簡單且直觀，有助於實際應用的推廣，但因缺乏考慮細粒度識別，性能仍然限制。

這邊列舉了三個演算法：
1. ACN (ICCVW-2015)
2. DeepSAR and DeepMAR (ACPR-2015)
3. MTCNN (TMM-2015)

裡面最經典的方法是 <mark>DeepSAR</mark> 與 <mark>DeepMAR</mark>，據說是第一篇 CNN 的論文，方法不難。重點是作者在後面幾篇的改進中，弄出了 RAP/RAP-2.0 這兩個資料集。


### 5.2 Part-based Models
在這一類型的訓練中，除了全域的影像資訊外，會在試圖引入額外的身體特徵資訊，如：身體部位、姿態評估、骨架...等資訊，提高整體識別性能。但這一類型的識別精準度對於額外的資訊會有很大依賴性；二則，由於這些資訊的引入會使訓練與推理的時間拉長，因為可能需要先識別出這些資訊才能進行屬性識別；最後，要能識別這些資訊你的訓練集中必須要有這些資料得標注，資料標注釋最費時費力又費眼的工作了(可能還費錢？ QAQ 

這邊演算法列了還挺多的：
1. Poselets (ICCV-2011)
2. RAD (ICCV-2013)
3. PANDA (CVPR-2014) 
4. MLCNN (ICB-2015)
5. AAWP (ICCV-2015) 
6. ARAP (BMVC2016)
7. DeepCAMP (CVPR-2016)
8. PGDM (ICME-2018) 
9. DHC (ECCV-2016)
10. LGNet (BMVC-2018)


### 5.3 Attention-based Models
<p class="illustration">
    <img src="https://i.imgur.com/2wZhJbg.png" alt="Some visualizations of HydraPlus-Net">
    Some visualizations of HydraPlus-Net （圖片來源: <a href="https://arxiv.org/pdf/1901.07474.pdf">論文</a>）
</p> 

大概是 Attention is all you need，所以在這邊也有試著引入 Attention XD

1. HydraPlus-Net (ICCV-2017)
2. VeSPA (ArXiv-2017)
3. DIAA (ECCV-2018) 
4. CAM (PRL-2017)

這邊屬於必看的是 HydraPlus-Net ，這篇這邊同時解決屬性識別和行人新識別的工作，重點是他也弄了個資料集 PA-100k。
 

### 5.4 Sequential Prediction based Models
<p class="illustration">
    <img src="https://i.imgur.com/hOKnc9t.png" alt="The pipeline of JRL of attribute context and correlation">
    The pipeline of JRL of attribute context and correlation（圖片來源: <a href="https://arxiv.org/pdf/1901.07474.pdf">論文</a>）
</p> 

在後期一點的模型也開始引入 RNN ，RNN 的優點大概是能夠明確學習與利用背景資訊，但 RNN 訓練起來真的有點很慢。

1. CNN-RNN (CVPR-2016)
2. JRL (ICCV-2017)
3. GRL (IJCAI-2018) 
4. JCM (arXiv-2018)
5. RCRA (AAAI-2019)
 

### 5.5 Loss Function based Models
<p class="illustration">
    <img src="https://i.imgur.com/4gTkvc3.png" alt="The pipeline of AWMT-network">
    The pipeline of AWMT-network（圖片來源: <a href="https://arxiv.org/pdf/1901.07474.pdf">論文</a>）
</p> 

這一類型的文章真的很難懂，能做這類的人真的超強，看到這麼少的模型就知道：
1.  WPAL-network (BMVC-2017)
2.  AWMT (MM-2017)

這邊比較經典是 WPAL-network ，他考慮資料的不平衡分布，並根據訓練集中所有屬性類別上標籤的比例，提出 <mark>weighted cross-entropy loss function</mark>，已經廣泛使用在許多行人屬性是別的演算法中。。


### 5.6 Curriculum Learning based Algorithms
Curriculum Learning，課程學習，主要思想是模仿人類學習的特點，由簡單（i.e. 容易學習的樣本）到困難來學習課程（i.e. 不容易學習的樣本），這容易使模型找到更好的區域最佳解，同時加快訓練速度。

1. MTCT (WACV-2017)
2. CILICIA (ICCV-2017)


### 5.7 Graphic Model based Algorithms
恩... 我的圖論課本呢？

1. DCSA (ECCV-2012)
2. A-AOG (TPAMI-2018)
3. VSGR (AAAI-2019)


### 5.8 Other Algorithms
1. PatchIt (BMVC-2016)
2. FaFS (CVPR-2017) 
3. GAM (AVSS-2017)



## 6. Applications
視覺屬性可視為是一種中級特徵表示，它可以為與人相關的高階任務提供重要資訊，例如人的重新識別（person re-identification）、行人檢測（pedestrian detection）、人員跟踪（person tracking）、人員檢索（person retrieval）、人體動作識別（human action recognition）、場景理解（scene understanding）。

各個項目在自己找資料來看看，他寫的太抽象了。



## 7. Future Research Directions
章節一開始先提供了一些論文的原始碼，真是佛心來著 :joy: 
 
| Algorithm          | Source Code                                                           |
| ------------------ | --------------------------------------------------------------------- |
| [DeepMAR](https://ieeexplore.ieee.org/document/7486476)  | https://github.com/dangweili/pedestrian-attribute-recognition-pytorch |
| [Wang et al.](https://arxiv.org/pdf/1711.02816.pdf). | https://github.com/James-Yip/AttentionImageClass                      |
| [Zhang et al.](https://ieeexplore.ieee.org/document/8510891) | https://github.com/dangweili/RAP                                      |
| [PatchIt](https://www.researchgate.net/publication/317191507_PatchIt_Self-Supervised_Network_Weight_Initialization_for_Fine-grained_Recognition)    | https://github.com/psudowe/patchit                                    |
| [PANDA](https://arxiv.org/pdf/1311.5591.pdf)     | https://github.com/facebookarchive/pose-aligned-deep-networks         |
| [HydraPlus-Net](https://arxiv.org/pdf/1709.09930.pdf) | https://github.com/xh-liu/HydraPlus-Net                               |
| [DIAA](https://arxiv.org/pdf/1807.03903.pdf)  | https://github.com/cvcode18/imbalanced_learning     

不過瞄到好幾篇是 Caffe 的，這對我在後面整合會是個大問題，之後有需要再來找找有沒有 TF 或 Pytroch 的？


### 7.1 More Accurate and Efficient Part Localization Algorithm
在部分研究中，會提供人體區域或是姿勢的資訊，來輔助或進行屬性的推測。但這些資訊的準度或是標注方式可以再做進一步的改良。
1. 標注方式可以朝向弱監督或是無監督的方向進行。
2. 標注的精準度的提昇。


### 7.2 Deep Generative Models for Data Augmentation
可以用 generative models 來處理低質量的圖像或標籤分配不平衡的問題。


### 7.3 Further Explore the Visual Attention Mechanism
人類感知的一個重要特徵是不會傾向於<mark>一次處理整個場景</mark>；人類會選擇性地將注意力集中放在一小部分的空間，並在所需的時間地點來獲取資訊，且會隨時間流逝進行整合。但如何準確有效地定位注意力的關注區域仍然是一個開放的研究問題。


### 7.4 New Designed Loss Functions 
為行人屬性辨識計新的損失函數。


### 7.5 Explore More Advanced Network Architecture
目前做遷移學習所是用的骨幹網路，並不是為行人屬性辨識設計的，若能專門為行人屬性辨識設計或調整，或許會有不錯的成果。


### 7.6 Prior Knowledge guided Learning
導入一些先驗知識，如季節、場合...等。


### 7.7 Multi-modal Pedestrian Attribute Recognition
因 RGB 圖像對氣候（雨、雪、霧、豔陽）與照明（夜間）...等反應劇烈，進而影響成像品質。使得行人屬性辨識無法具備對行人的全天候識別能力。但在實際應用此能力需具備。一個直觀的想法是從其他人那裡取得有用的資訊：如深度感測器、溫度感測器或熱圖像之類的。


### 7.8 Video based Pedestrian Attribute Recognition
現有的資料集多是（至少三個主要指標是）從影片擷出來，如果能直接使用影片會多取得一些時空資訊，能輔助屬性的識別，尤其該屬性是用一連串動作表現時。

<div class="alert info"> 
<div class="head">引用了一篇 Video-based 的論文</div>
<ul>
  <li>Chen Z, Li A, Wang, "<a href="https://arxiv.org/pdf/1901.05742.pdf">A Temporal Attentive Approach for Video-Based Pedestrian Attribute Recognition</a>", *Pattern Recognition and Computer Vision*, pp: 209-220, 2019。</li>
  <li><a href="/Paper-A-Temporal-Attentive-Approach-for-Video-Based-Pedestrian-Attribute-Recognition">筆記連結</a></li>
</ul>
</div>


### 7.9 Joint Learning of Attribute and Other Tasks
把屬性識別和其他的諸如行人檢測，行人重新識別結合起來。
 


## 8. Conclusion
對行人屬性辨識（i.e. PAR）進行了回顧與介紹：傳統方法、深度學習方法、資料集、現有的網路架構與程式碼，最後是可能的未來研究方向。



## 閱讀後紀錄與動作
1. **這篇論文在說什麼？**  
    這篇論文概觀介紹行人屬性辨識中的基本概念、挑戰、主流研究方向與模型與未來研究方向...等，進行解說。
    
2. **它有什麼貢獻？我們從它學到什麼？**  
    是第一篇行人屬性辨識的 Survey Paper，有系統說明了整個領域的狀況。對於開發者來說，他羅列該領域所需的基礎認知，並提供了開發所需資料載點（雖然有的載不下來了）方便進行比較。算是還滿結實的敲門磚 XDDD
    
3. **它有什麼缺陷或不足的地方？**  
    作者筆誤還滿多，不曉得是不是因為我看的預印本的關係？另外有些資料可能是作者想表達的太多，篇幅又有限，有些部分反倒有些語焉不詳，建議拿關鍵字去查查詳細資料。
    
4. **看完這本書要做那些行動？**  
    從三個較大的資料集中選一個，另外在挑選一份有 source code 的 SOTA 試試看，實際感受一下。
   


## 參考資料 
1. 肤浅-的我 (2019-09-12)。[论文笔记Pedestrian Attribute Recognition（PAR）: A Survey](https://blog.csdn.net/weixin_39225983/article/details/100765667)。檢自 weixin_39225983的博客｜CSDN博客 (2020-08-26)。
2. lanmengyiyu (2019-10-23)。[Pedestrian Attribute Recognition](https://blog.csdn.net/lanmengyiyu/article/details/102686182)。檢自 张洁的笔记｜CSDN博客 (2020-08-26)。
3. Code_Mart (2019-03-02)。[[笔记] Pedestrian Attribute Recognition Dataset Summary](https://blog.csdn.net/Code_Mart/article/details/87721803#t14)。檢自 不进则退｜CSDN博客 (2020-08-26)。
4. zouxy09 (2012-08-31)。[目标检测的图像特征提取之（一）HOG特征](https://blog.csdn.net/zouxy09/article/details/7929348)。檢自 zouxy09的专栏｜CSDN博客 (2020-08-26)。
5. zouxy09 (2012-08-31)。[目标检测的图像特征提取之（二）LBP特征](https://blog.csdn.net/zouxy09/article/details/7929531)。檢自 zouxy09的专栏｜CSDN博客 (2020-08-26)。
6. 協同創作。[Soft biometrics](https://en.wikipedia.org/wiki/Soft_biometrics)。檢自 Wikipedia (2020-08-26)。
7. Anticoder (2019-03-16)。[Multi-task Learning(Review)多任务学习概述](https://zhuanlan.zhihu.com/p/59413549)。檢自 知乎 (2020-08-27)。
8. 张敏灵、周志华。[多标记学习](http://palm.seu.edu.cn/zhangml/files/mla11-mll.pdf)。檢自 PALM实验室｜东南大学 (2020-08-27)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-12-21</summary>
  <ul>
    <li>2020-12-21 發布</li>
    <li>2020-08-31 完稿</li>
    <li>2020-08-05 起稿</li>
  </ul>
</details>