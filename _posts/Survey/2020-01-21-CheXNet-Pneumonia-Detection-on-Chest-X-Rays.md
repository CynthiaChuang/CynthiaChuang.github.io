---
title: "【Survey】CheXNet: 肺部 X-Ray 醫學影像判讀技術"
date: 2020-01-21
categories:
- AI/ML
- Survey
tags:
- CV
- Medical-Imaging
- X-Rays
--- 

前一陣子工作上在 Survey 的東西，想了解深度學習在醫療影像上目前的近況、模型訓練的難易度和應用場景...等。最後挑選了吳恩達的團隊所設計的 [CheXNet](https://arxiv.org/abs/1711.05225) 來入手。

<!--more-->
<br> 

## AI 醫療影像

所謂的<span class='label'>醫療影像</span>一直是窺視人體內部結構與組成的主要方法，常見的拍攝方式包括 X 光攝影、超音波影像、電腦斷層掃描（CT）、核磁共振造影（MRI）和心血管造影...等等。

不同的拍攝技術會決定影像性質、維度與張數多寡，這些因素也會直接影響深度學習的模型建立與應用場景。
<br>

<center> <img src="https://i.imgur.com/ruIHb7k.jpg" alt="X 光判讀"></center>
<center class="imgtext">X 光判讀（圖片來源: <a href="https://www.seinsights.asia/article/3289/3270/6005" class="imgtext">社企流</a>）</center>
<br>

舉例來說，X 光是利用組織間對輻射吸收能力不同來成像，其吸收能力與密度呈正比，並將原本三維的空間關係疊壓在二維平面上成像。因此，涵蓋的資訊較為龐雜，且病灶可能會被組織或器官給遮擋。反之，電腦斷層可以用三維重建技術計算恢復人體的斷層面影像。

但電腦斷層影像的單筆資料較大，會造成訓練上的困難，且電腦斷層並非常規檢查，整體資料量也較少。反之，X 光是初階檢查因此容易累積出大量資料，滿足訓練的先決條件。兩者各有利弊，需視訓練目標來挑選資料。
<br>

一般來說，深度學習在醫療影像應用上，較適合需要透過影像判別腫瘤位置、偵測骨頭裂縫線條...等單純且單一的識別工作。之後再將識別結果交由醫師，醫師可進一步參考臨床資訊進行最終的診斷。

在現階段過程中，對於 AI 系統定位仍僅止於助手或是診療工具，醫師必須知曉工具的能力與限制，最後的決策仍以醫師為主。就實務層面來說，這也是最不涉及醫生利益，與診斷所產生的醫療糾紛的定位。
<br>

此外，AI 在醫學影像領域中，另一個需要克服的困難是資料部份，雖然各大醫療院所在長年累月下累積了大量的醫學影像，但這些影像的儲存方式並非為 AI 應用而設計，且再考慮到不同廠商、機器之間可能會儲存方式的差異，因此必須要進一步整理與提取成可供深度學習使用的資料。

此外還有標註的部份，不論是採用半監督式或是監督式學習都需要進行資料的標註，但請專業人員進行標註不僅成本昂貴，且相當費時，因此如何得到高品質的訓練資料將是訓練過程中的一大挑戰。
 
<br><br>

## ChestX-ray14

說到資料集，在來看吳恩達的 CheXNet 之前，先來看看它所使用的資料集—ChestX-ray14。
<br>

### 資料集簡介
為目前最大規模的肺部 X 光資料集，由 [NIH 美國國立衛生研究院](https://www.nih.gov/news-events/news-releases/nih-clinical-center-provides-one-largest-publicly-available-chest-x-ray-datasets-scientific-community)所提供（[載點](https://nihcc.app.box.com/v/ChestXray-NIHCC)）。

ChestX-ray 資料集包含 30 萬名病人與 10 萬張胸部前視圖 X 光影像（約 42G），研究人員對數據採用 NLP 方法從病例報告中提取關鍵字對影像進行標註，1-14 類分別對應 14 種肺部疾病，第 15 類表示未發現疾病。據稱，該數據庫使用 NLP 標註準確率超過 90%。
<br>

<div class="note info"> 
關於資料標註的相關研究，請參考 <a href="https://arxiv.org/abs/1705.02315">X. Wang, Y. Peng, L. Lu, Z. Lu, M. Bagheri, and  R.M. Summers. ChestX-ray8: Hospital-scale Chest X-ray Database and Benchmarks on Weakly-Supervised Classification and Localization of Common Thorax Diseases. 2017 </a>。ChestX-ray14 Data 的描述，請看論文的附件 B 。
</div>

<br>

### 資料集內容

14 種肺部疾病包含：Atelectasis、Hernia、Cardiomegaly、Infiltration、Consolidation、Mass、Edema、Nodule、Effusion、Pleural_Thickening、Emphysema、Pneumonia、Fibrosis、Pneumothorax。

中文翻譯對照分別為：肺陷落 / 肺不張、疝氣 / 脫腸、心臟肥大、滲透、實變、腫塊、浮腫 / 水腫、結節、滲出、胸膜增厚、氣腫 / 肺氣腫、肺炎、纖維化、氣胸。
<br>

先前提過資料集中約有 10 萬張胸部前視圖 X 光影像，每張圖片大小為 1024 * 1024、格式為 PNG，下圖隨機展示些 ChestX-ray14 中的 X 光影像，並搭配額外的標註框，以顯示病灶所在。

<center> <img src="https://i.imgur.com/CZ53CBj.png" alt="ChestX-ray14"></center>
<center class="imgtext">ChestX-ray14圖片 + Box 標註（圖片來源: <a href="https://arxiv.org/abs/1705.02315" class="imgtext">ChestX-ray14</a>）</center>
<br>

好吧，說實話就算把病灶框給我，我也看不出差異在哪裡 orz 

另外雖說有 10 萬張胸部前視圖 X 光影像，但只有約 1000 張影像有標註框而已，所以這部份資訊，可能只能來當 test set 確認模型的效能，無法用來參與訓練。

<br> 

再來看看影像的標記，會發現這是一個 Multi-label Classification 或這說是 Multi-task Classification，每張影像都會被標註上一或多種疾病。

在印出各個類別的資料個數，會發現除了疾病與疾病間極度不平衡外，如：滲透的影像有近兩萬張，但疝氣只有兩百多張；患病與未患病之間的影響也頗為懸殊，大約是 6 : 4。
<br>


| Pathology| |Total||Pathology||Total||
| --------|--------|--------|--------|--------|-------- |-------- |-------- |
|Atelectasis|肺陷落 / 肺不張|11559 |10.31%   |Hernia|疝氣 / 脫腸| 227 |0.20% |
|Cardiomegaly|心臟肥大|  2776| 2.48% |Infiltration|滲透| 19894|17.74%|
|Consolidation|實變|4667 | 4.16%  |Mass|腫塊| 5782 |5.16%|
|Edema|浮腫 / 水腫|2303 |  2.05%  |Nodule|結節|6331 |5.65% |
|Effusion|滲出|13317 |11.88% |Pleural_Thickening |胸膜增厚|3385 | 3.02%  |
|Emphysema|氣腫 / 肺氣腫|  2516 | 2.24%  |Pneumonia|肺炎|1431 |1.28% |
|Fibrosis|纖維化| 1686 |1.50%   |Pneumothorax|氣胸|5302 |  4.73% |
|||||No Finding|未患病|60361 | 53.84% |

P.S. 這數據因為我刪除過些影像，所以數字可能有出入，但概觀比例應該不受影響。

<br> 這樣的不平衡，預期會對訓練過程中造成一些影響。

<br>

### 資料集問題
除去資料不平衡外，資料集中還包含部份質量很差的影像，並不建議用於訓練。這部份 Azure 有列出一些[黑名單](https://github.com/Azure/AzureChestXRay/tree/master/AzureChestXRay_AMLWB/Code/src/finding_lungs)，可以參考這些名單將不適用的影像加以排除。
<br>

但這份資料集最引人討論的並非影像本身，而是它的<span class='label'>標籤</span>。有位放射科醫生 Luke Oakden-Rayner （盧克·奧克登·雷納），這位醫生也曾在 2017 年 5 月的 Nature 上發表關於[利用深度學習技術預測人類壽命的相關研究](https://www.nature.com/articles/s41598-017-01931-W)，是一名同時熟悉深度學習與醫療影像的研究者。
<br> 
 
<center> <img src="https://i.imgur.com/GFQFd3T.jpg" alt="纖維化（Fibrosis）"></center>
<center class="imgtext">纖維化標記錯誤，紅色=明顯錯誤的標籤；橙色=懷疑態度（圖片來源: <a href="https://lukeoakdenrayner.wordpress.com/2017/12/18/the-chestxray14-dataset-problems/?fbclid=IwAR0oc-Zwz4EOPyp_rDvs8i__6ODgjWNqv-LHJ2B0t5KZiEIkwmF_o3hcUsU" class="imgtext">Luke Oakden-Rayner 網誌</a>）</center>
<br>

在他所提出的[質疑](https://lukeoakdenrayner.wordpress.com/2017/12/18/the-chestxray14-dataset-problems/?fbclid=IwAR0oc-Zwz4EOPyp_rDvs8i__6ODgjWNqv-LHJ2B0t5KZiEIkwmF_o3hcUsU)中，他開宗明義講到：「我認為目前的 ChestXray14 資料集並<span class='label'>不適合</span>用於訓練醫用 AI 系統以進行診斷工作」。

他認為與人類醫生的視覺評估相比，這份資料集中的標籤準確度值得商榷，其標註並不準確或不清楚，且部分標籤屬於醫學上的次要發現，因此認為，無法匹配影像中顯示的疾病，並對這些標籤在臨床上的真實意義與實用性進行討論。
<br>

但，針對使用NLP挖掘疾病標籤的功效以及導致不良標籤質量的討論，Azure 倒是[認為](https://github.com/Azure/AzureChestXRay#criticisms)，即使標籤很髒，深度學習模型有時仍能夠學到良好的分類性能。
<br>

### Google 裁決標籤
除了直接使用 ChestX-ray 的標籤進行訓練，有些~~財大氣粗~~的研究單位，會結合公開 ChestX-ray 與其他資料集，進行重新標註與 review。

像是 Google 用來[診斷胸部 X 光片的最新研究](https://ai.googleblog.com/2019/12/developing-deep-learning-models-for.html)，他們不僅引入美國國立衛生研究院的 ChestX-ray14 資料集，還取得自阿波羅醫院的胸部 X 光片，兩資料集總共約60萬張圖片。重新標註成 4 個重要的臨床病徵分類，分別是氣胸、結節和腫塊、骨折以及氣腔陰影。

不過雖說重新標註，但由於整體數量過於龐大，還是無法全由人工手動為影像上標籤，必須借助 NLP 擷取關鍵字為影像添加標籤，最後再藉由放射科醫生人工檢視約 37,000 張圖片，以提高資料集標籤的品質。

會特別提到這件事的原因是，Google 有對社群[開放](https://console.cloud.google.com/storage/browser/gcs-public-data--healthcare-nih-chest-xray-labels/) ChestX-ray14 資料集中所有裁決標籤，以幫助社群對胸部X光片進行研究。

但數量比不上原先的資料集大小，僅包含 2,412 張圖像的訓練與驗證資料集，還有 1,962 張圖像的測試集，且與原先的資料集一樣存在<span class='label'>資料傾斜</span>的現象需要克服。
<br>

| Pathology | train | test |
| ---------- | ----------|---------- | 
|Fracture | 114 |72|
|Pneumothorax|43|195|
|Airspace opacity|1031|1135|
|Nodule or mass|310|295

<br><br>

## CheXNet

<center><img src="https://i.imgur.com/vZxoroP.png"></center> 
<center class="imgtext">網路輸入為人體正面掃描的胸片，輸入時患肺炎的機率，為了更好的可視化，使用了熱力圖。</center>
<br>

回到一開始的的目標 - CheXNet，這是吳恩達的研究團隊在 2017 年所提出的技術，能從胸部透視圖偵測肺炎，號稱水準超越專業放射科醫生(？)。

該網路在目前最大的開放式胸部透視圖資料庫 ChestX-ray14 訓練。也就是上一節所 Survey 的內容。
<br>

### 網路架構

<center> <img src="https://i.imgur.com/PxR7kqH.png" alt="CheXNet"></center>
<center class="imgtext"> CheXNet（圖片來源: <a href="https://dosudodl.wordpress.com/2017/11/17/chexnet-%E8%B6%85%E8%B6%8A%E5%B0%88%E6%A5%AD%E6%94%BE%E5%B0%84%E7%A7%91%E9%86%AB%E7%94%9F%E7%9A%84%E8%82%BA%E9%83%A8xray%E9%86%AB%E5%AD%B8%E5%BD%B1%E5%83%8F%E5%88%A4%E8%AE%80%E6%8A%80%E8%A1%93/" class="imgtext">Dosudo矽谷工程師 deep learning newsletter</a>）</center>  
<br>


ChexNet 是 121 層的 CNN 網路，用的是 DenseNet、輸入是胸部正面 X 光片、而輸出是每個類別從 0~1 的肺炎機率，是一個 Multi-task Classification。

網路的訓練過程中，會將由 ChestX-ray14 的影像大小由 1024 x 1024 調整為 224 x 224，同時根據在 ImageNet 中圖片的均值和偏差進行正規化，並使用了隨機水平傾斜來做數據增強。

訓練時使用 Adam 作為 optimizer，並採用 binary cross entropy loss （BCELoss） 當 loss function。 最後替將 DenseNet 最後一層的 fully connected layer 加上 sigmoid 輸出各疾病機率。

最後的輸出除了各疾病機率外，為判斷學習成果並擁有更好的可視化效果，可使用了熱力圖（Class activation mapping），取得網路的分類關注區域。
<br>

### 實作
想要按照論文重新製作一個相同的模型還算簡單，因為大部分的框架都有提供現成 DenseNet 可供 import，只需要將最後一層改成所要輸出的類別數即可，以 pytroch 時作為例：

```python
import torch.nn as nn
import torchvision

class DenseNet121(nn.Module):

    def __init__(self, classCount, isTrained):	
        super(DenseNet121, self).__init__()
        
        self.densenet121 = torchvision.models.densenet121(pretrained=isTrained)
        kernelCount = self.densenet121.classifier.in_features		
        self.densenet121.classifier = nn.Sequential(nn.Linear(kernelCount, classCount), nn.Sigmoid())

    def forward(self, x):
        x = self.densenet121(x)
        return x
```
<br>

不過如果是自己刻模型，還必須自己重新訓練模型，對於我這個只是想看看成果的人來說有點費時，所以我這邊用了人家已經訓練完成模型 - PyTorch ChexNet（[master](https://github.com/zoogzog/chexnet) /  [torch.1.1.0](https://github.com/dgrechka/chexnet/tree/torch.1.1.0)） 


還找到如何 Azure 在架構 ChexNet 的[手把手教學](https://github.com/Azure/AzureChestXRay) XD

<br>

根據 ptrtoch 版本的 ChexNet 實做，要達到與論文的相同的效能大約需要：

<div class="note info"> 
The training was done using single Tesla P100 GPU and took approximately 22h.
</div>
<br>

### 結果
可以看到兩份實做都是以 AUROC（或稱 [ROC-AUC](/ai/ml/2020/01/09/Common-Evaluation-MetricAccuracy-Precision-Recall-F1-ROCAUC-and-PRAUC/)）作為評價模型效能的指標，推測應該是為了克服資料不平衡而採用，兩份實做的數據如下：

| Pathology |PyTorch AUROC| Azure AUC Score |
| -------- | -------- |  -------- |  
|Atelectasis	|0.8321| 0.828543|
|Cardiomegaly	|0.9107| 	0.891449|
|Effusion	|0.8860|	0.817697|
|Infiltration	|0.7145|0.907302|
|Mass	|0.8653|0.895815|
|Nodule	|0.8037|	0.907841|
|Pneumonia	|0.7655|0.817601|
|Pneumothorax	|0.8857|0.881838|
|Consolidation	|0.8157|0.721818|
|Edema	|0.9017 	|0.868002|
|Emphysema	|0.9422 	|0.787202|
|Fibrosis	|0.8523	|0.826822|
|P.T.	|0.7948 	|0.793416|
|Hernia	|0.9416	|0.889089|

<br>

可以看到所得的分數相當的漂亮，大約座落在 0.866 ，用此數據畫出來的 ROC 曲線也很"標準" XD
<center> <img src="https://i.imgur.com/2Lw1L70.png?1" alt="ROC 曲線"></center>
<center class="imgtext"> ROC 曲線</center>  
<br>

不過當我是用這組曲線去反推適合的門檻值時，發現門檻值似乎有點低？

| Pathology | best_threshold|
| ---------- | ----------|
|Atelectasis |0.12584402|
|Cardiomegaly |0.033162296|
|Effusion |0.16826268|
|Infiltration |0.18238032|
|Mass |0.072381005|
|Nodule |0.07161161|
|Pneumonia |0.017619494|
|Pneumothorax |0.0711597|
|Consolidation |0.0655834|
|Edema |0.039323617|
|Emphysema |0.039765418|
|Fibrosis |0.022241825|
|Pleural_Thickening |0.04171453|
|Hernia |0.0019557325|

<br> 此外，我還印出 Precision Mean 與 Recall Mean，分數只有 0.696 與 0.242，這樣的分數感覺不太符合我預期的說...

<br> 最後來看看預測結果與熱力圖的效果：
<center> <img src="https://i.imgur.com/kQ2SSuA.png" alt="熱力圖"></center>
<center class="imgtext"> 由左而右分別是原始圖、熱力圖和標註框</center>  
<br>

隨機選了三組結果，剛好最對應到了三種 case：
1. 最上面一組圖預測結果是錯的，由熱力圖看來關注點完全不對。
2. 第二個預測結果是對的，熱力圖的關注點是有點偏，但算是有擦到邊，勉強可以算是有學到？
3. 第三組圖，算是最完美的 case 了，預測對，熱力圖的關注點也與標註框吻合。


<br><br>

## 小結

整體來說，網路架構並沒有的創新，使用的是先前的研究，或許有時間再來複習一次 DenseNet 的[論文](https://arxiv.org/abs/1608.06993)。

另外論文中的這句提到<span class='label'>對於放射科醫生來說，不知道病人先前的病史對於判斷正確有幫助</span>，我還滿驚訝的，照理來說不是資料越多越好？

<br><br>

##  參考資料
1.  [【AI浪潮席捲醫療業】透視5大類醫療影像辨識的AI應用場景｜iThome](https://www.ithome.com.tw/news/129973)
2.  [智慧判讀醫療影像 AI將成醫師最強助手｜DIGITIMES 智慧應用](https://www.digitimes.com.tw/iot/article.asp?cat=158&id=0000568416_WGI3445R2PT5R15ZCDZ5W)
3. [【Research】Explore ChestX-ray Dataset｜L简书](https://www.jianshu.com/p/617f0d30c8d2)
4. [【3万患者11万图像14类病理】NIH公开大规模胸部X光数据集｜云栖社区-阿里云](https://yq.aliyun.com/articles/222235)
5. [Multi-class, Multi-label 以及 Multi-task 问题｜金良山庄-CSDN博客](https://blog.csdn.net/golden1314521/article/details/51251252)
6. [Exploring the ChestXray14 dataset: problems｜Luke Oakden-Rayner](https://lukeoakdenrayner.wordpress.com/2017/12/18/the-chestxray14-dataset-problems/?fbclid=IwAR0oc-Zwz4EOPyp_rDvs8i__6ODgjWNqv-LHJ2B0t5KZiEIkwmF_o3hcUsU)
7. [机器之心报道：吴恩达的最新研究是否严谨？Nature论文作者撰文质疑AI医疗影像研究现状｜虎嗅网](https://www.huxiu.com/article/226463.html)
8. [醫療影像AI的「九九八十一難」？｜獨家深度 - 每日頭條](https://kknews.cc/zh-tw/tech/gzjx8gl.html)
9. [Google訓練深度學習模型判讀多種肺部X光片病徵｜iThome](https://www.ithome.com.tw/news/134637?fbclid=IwAR2bRVnEGGW07MYUms4T-WwTpQKhUCt78FoBjEu0QZK0M6lgl1MuSOfgC0c)
10. [Google AI Blog: Developing Deep Learning Models for Chest X-rays with Adjudicated Image Labels](https://ai.googleblog.com/2019/12/developing-deep-learning-models-for.html)
11. [吳恩達團隊最新醫學影像成果：肺炎診斷準確率創新高｜TechNews 科技新報](https://technews.tw/2017/11/17/chexnet-radiologist-level-pneumonia-detection-on-chest-x-rays-with-deep-learning/) 
12. [【论文阅读笔记】CheXNet: Radiologist-Level Pneumonia Detection on Chest X-Rays with Deep Learning｜寸先生的AI道路](https://blog.csdn.net/cskywit/article/details/79141831/) 
13. [CheXNet : 超越專業放射科醫生的肺部Xray醫學影像判讀技術｜LDosudo矽谷工程師 deep learning newsletter](https://dosudodl.wordpress.com/2017/11/17/chexnet-%E8%B6%85%E8%B6%8A%E5%B0%88%E6%A5%AD%E6%94%BE%E5%B0%84%E7%A7%91%E9%86%AB%E7%94%9F%E7%9A%84%E8%82%BA%E9%83%A8xray%E9%86%AB%E5%AD%B8%E5%BD%B1%E5%83%8F%E5%88%A4%E8%AE%80%E6%8A%80%E8%A1%93/)
