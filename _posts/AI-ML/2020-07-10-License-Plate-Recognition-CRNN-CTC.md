---
title: 車牌辨識：CRNN + CTC
date: 2020-08-12
modified: 2020-08-12
categories:
- AI/ML
tags:
- OCR
- LPR
- CV
--- 

前一陣子的工作成果紀錄。文章內容及架構是基於同事的投影片進行擴充與刪減而成，雖然同事整理過了，但畢竟自己整理過才是自己的咩 :smile:。

<!--more-->
<br><br>  

## Optical Character Recognition, OCR

OCR 為光學字元辨識（Optical Character Recognition）的縮寫，主要目的是將文字資料的圖像檔案進行分析辨識處理，取得文字及版面資訊。

### 基本辨識流程

一般來說，一套完整的辨認系統通常包含三部分：影像掃描器（Image scanner）、光學字元辨認軟/硬體以及輸出介面（Output interface）。

<center> <img src="https://i.imgur.com/CoO98Wb.png" alt="two stage flow"></center>
<br>

以超速照相為例為例，會先透過影像掃描器，即攝影機或是照相機拍下超速車輛，再並將圖片送往光學字元辨識軟體，也就是我們的車牌辨識系統，得到超速車輛的車牌，最後再把此車牌號碼送往罰單的系統，開出一張超速罰單，此步驟對應到就是流程中的輸出介面。

雖說整套流程包含三個部份，但在這邊我們只將目光放在光學字元辨識系統上。

<br>

細分光學字元辨識系統，可在分成兩個步驟：分別是<span class='highlighting'>文字檢測</span>與<span class='highlighting'>文字識別</span>。
- **文字檢測**：  
    顧名思義就是找出哪裡有文字以及文字範圍大小，對應到車牌辨識系統中就是找出車牌的位置並框出車牌。
    
- **文字辨識**：  
    是對定位出的文字區域進行辨識，一般狹義的 OCR 指的是這一部份，本文也是專注在這一部份。

<center> <img src="https://i.imgur.com/bRpsT8T.png" alt="two stage flow"></center>

<br> 

### 文字辨識方法

文字辨識的方法依照實做的方式，可區分成 <span class='highlighting'>Two Stage</span> 或 <span class='highlighting'>End To End</span> 兩種。
<br> 

Two Stage 顧名思義會分成兩個主步驟來進行，它會先將目標區域中的字元進行分割，也就是將車牌中的字一個一個擷出來，並將擷出的字元進行正規化，最後透過特徵截取將字元一一進行辨識。

<center> <img src="https://i.imgur.com/irrsatm.png?1" alt="two stage flow"></center>
<center class="imgtext">文字檢測 + 文字辨識(Two Stage) 辨識流程（圖片來源: <a href="https://sites.google.com/site/cs0991326/home/che-pai-bian-shi" class="imgtext">Google 協作平台</a>）</center>
<br>

而另一種 end to end 的方式則不需要進行字元分割，是一步到位的辨識方法。它會直接將擷取出的整份車牌送入神經網路中學習。

兩相比較，前者多建立資料庫來進行比對，資料多寡並不會影響準確率，全倚賴特徵擷取的規則，但計算上較簡單。反之，後者的準確率受資料量影響，但計算較為複雜。

<br><br> 

## CRNN

說到車牌，之前[提過](/Taiwan-License-Plate-Rules-for-LPR)臺灣目前車牌是採兩代車牌並行使用的做法。目前流通的車牌總共有 `2-4`、 `4-2`、 `2-2`、 `3-2`、 `2-3`、 `3-3` 與 `3-4`  共 7 種格式，長度由 4 到 7 不等。


考慮到不定長度的狀況，最後決定採用 [CRNN](https://arxiv.org/pdf/1507.05717.pdf)，將文字辨識轉為序列問題，透過 DCNN（深度卷積網絡）串接 RNN 的模型架構，對圖片進行文字識別。

<br>

### 網路基本架構
<center> <img src="https://i.imgur.com/jLmg7u1.png" alt="CRNN OCR Network Architecture
"></center>
<center class="imgtext">CRNN OCR Network Architecture
（圖片來源: <a href="https://arxiv.org/pdf/1507.05717.pdf" class="imgtext">論文</a>）</center>
<br>

根據上圖架構 CRNN 模型主要可以分成三個部分：
1. Convolutional Layers
2. Recurrent Layers
3. Transcription Layer

<br>

**Convolutional Layers**  
這層的主要目的是使用 Convolutional Layers 的特性，去提取圖像特徵。

在將圖片餵進網路之前，需要對圖片做前處理，在原始的網路架構中，僅接受 `W x 32 (寬 x 高)` 的灰階圖片，因此輸入尺寸為 $(width, height, channel)=(W, 32, 1)$。當讀入圖片後，會經由使用 VGG 概念而採用的大量 $3 \times 3$ Conv 及 $2 \times 2$ pooling 所構成的網路，最終會得到此圖的特徵向量。


<center> <img src="https://i.imgur.com/EpQEZpo.png" alt="Network configuration summary"></center>
<center class="imgtext">Network configuration summary（圖片來源: <a href="https://arxiv.org/pdf/1507.05717.pdf" class="imgtext">論文</a>）</center>
<br>

在此必須提到一件事，根據論文架構表格中，作者將最後後兩層的 pooling size 改為 <span class='highlighting'>1 X 2</span> pooling。有人認為這是為了盡可能不丟失寬度方向的資料，會較適合英文字母的識別（例如非等寬字體中較窄的 i 與 l）。

不過在論文作者的[原始碼](https://github.com/bgshih/crnn/blob/f5d41e3355fc4447f77cd6d5e5777b3a160c93cb/model/crnn_demo/config.lua#L68)中，在最後兩層的 pooling size 並非如論文內紅框處設計，[作者表示](https://github.com/bgshih/crnn/issues/6)論文中的 $1 \times 2$，其實是筆誤 XDDDD 
<br>

<center> <img src="https://i.imgur.com/ykx0PYw.png" alt=""></center>
<center class="imgtext">（圖片來源: <a href="https://github.com/bgshih/crnn/issues/6" class="imgtext">bgshih/crnn</a>）</center>
<br>

但根據我同事的實驗，在我們的資料集上使用 $1 \times 2$ 的 pooling size，表現確實比使用 $2 \times 2$ 好，這算是美麗的誤會嗎？


因此我們決定當作沒看到筆誤這件事，仍舊按照論文中這張表格進行實作。當過完所有的 Maxpooling 後，寬度為 $W	\div 2^2$ ，高度為 $H \div 2^4$，在過最一層 $2 \times 2$ Conv，且 padding size 為 0（padding = ‘VALID’），最終輸出尺寸為 $(\frac{W}{4}, 1, 512)$。
  
<center> <img src="https://i.imgur.com/FMOuqrN.png" alt="Convolutional Layers"></center>
<center class="imgtext">Convolutional Layers（圖片來源: <a href="https://arxiv.org/pdf/1507.05717.pdf" class="imgtext">論文</a>）</center>
<br>

但此階段的輸出無法直接最為下一階段的，需要經過一次轉換，因為 RNN 所接受的輸入格式應該為 $(timesteps, dim)$，很明顯的維度不對。

在論文中作者將這樣的轉換稱之為 <span class='highlighting'>Map-to-Sequence</span>，不過其實就是做 reshape 變成 $(\frac{W}{4} * 1, 512)$，恩...可以想像成依照寬度將原圖分成 $\frac{W}{4} * 1$ 等分，每等分會有 512 個特徵。因此最後產生就是，原圖從左到右按照順序生成的特徵向量，做為下一階段的輸入。
<br>

**Recurrent Layers**  
由於車牌長度是不定的，因此第二階段引入了雙向 RNN。不過習慣上說是 RNN，但實際上指的是 LSTＭ，用 GRU 也行，別真的用 RNN，不然應該會遇到梯度消失的問題。

在這一層中，網路會對上一階段傳入的特徵向量進行學習，以進行預測。由上圖可以了解，上一階段所傳入的特徵向量，其實相當於車牌中的一個矩形區域。RNN 所要學習的就是這個矩形區域所屬的字元為何，其輸出尺寸為 $(\frac{W}{4} * 1, 256*2)$。

最後再將輸出結果透過 softmax 「壓縮」成各個字元的機率分布，作為 CTC 層計算的輸入。假若你的目標字元為 A~Z 與 0~1，共 36 個，而此層的輸出為  $(\frac{W}{4} * 1, 36+1)$，多出來的一個類別保留給 CTC 層計算時所使用的 Blank。
<br>

**Transcription Layer**  
將從 Recurrent Layers 取的的標籤通過去重複、整合等等動作得到最終的辨識結果。白話的說就是將 RNN 的輸出做 Softmax 後得到各字元。

值得一提的是，這邊並不是採用常用的 Softmax cross-entropy loss，而是所謂的 <span class='highlighting'>CTC loss（Connectionist Temporal Classification，聯接時間分類）</span>。

<br>

### Connectionist Temporal Classification, CTC

在這邊引入 CTC loss 是為了處理<span class='highlighting'>不定長度序列的對齊問題</span>！

在一般使用 Softmax cross-entropy 計算 loss 時，會每一行輸出都對應到一個字元。因此你必須先標出每個字元在圖像中的位置，並調整網路以保證 Recurrent Layers 後所輸出的特徵值中，每一行會剛好對應到一個字元，才能進行訓練。

<center> <img src="https://i.imgur.com/fi6mMeg.png?1" alt="one column mapping to one char"></center>
<center class="imgtext">上層輸出的特徵值必須每一行對應到一個字元，才能進行訓練（圖片來源: <a href="https://www.cnblogs.com/skyfsm/p/10335717.html" class="imgtext">冠军的试炼</a>）</center>
<br>

但在實務上，若要何外標註每個字元位置會比單純標記字元，多出 5 倍以上的時間，耗時又費工。且由於車牌字元長度不定、照片角度不一、字寬不等、字型也有所差別，想讓每一行會剛好對應到一個字元，幾乎不可能。

但若忽略這個問題，直接對 Recurrent Layers 輸出進行序列分類，可能會出現冗餘資訊，如一個字母被連續識別兩次。遇到這情況又不可直接去除重複的字母，因為原始文字可能本來就存在連續重複字元，如：cook、geek、hello。

<center> <img src="https://i.imgur.com/FuIOQqa.png" alt="直接序列分類可能會出現冗餘資訊"></center>
<center class="imgtext">直接序列分類可能會出現冗餘資訊（圖片來源: <a href="http://fancyerii.github.io/books/ctc/" class="imgtext">李理的博客</a>）</center>
<br>

幸運的是，這問題在語音辨識中已有所研究。因為每個人語速不一，如何針對每一幀進行對齊與識別，這問題在語音辨識中存又已久。
，因為每個人語速不一。

論文中引入的是 CTC 來計算 loss，計算概念上類似 beam search，計算每條路徑的機率、並加總相同輸出的機率，最終取最高值最為輸出。

<div class="alert info"> 
<div class="head">若對 CTC 的實做理論有興趣的，請詳閱這兩篇</div>

1. <a href="https://blog.csdn.net/Left_Think/article/details/76370453">语音识别：深入理解CTC Loss原理 | 左左左左想 | CSDN博客</a> <br>
2. <a href="http://fancyerii.github.io/books/ctc/">CTC理论和实战 | lili on | 李理的博客</a>
</div>
<br>

<center> <img src="https://i.imgur.com/JTaLcpw.png" alt="直接序列分類可能會出現冗餘資訊"></center>
<center class="imgtext">直接序列分類可能會出現冗餘資訊（圖片來源: <a href="http://fancyerii.github.io/books/ctc/" class="imgtext">李理的博客</a>）</center>
<br>

實做過程中須注意的是，CTC 存在著 Blank 的概念，可以想像成序列剛好位於字與字之間的空白，因此在設計 Recurrent Layers 的輸出時會保留一個維度給 Blank，Blank 本身並不影響輸出結果，在輸出時 Blank 會被視為無意義字元給濾掉，但在計算每條路徑的機率時會將納入計算。

<center> <img src="https://i.imgur.com/JxSAKwO.png" alt="直接序列分類可能會出現冗餘資訊"></center>

<br><br> 

## CRNN 實作

在實做方面我們參考了 [CRNN-Keras](https://github.com/qjadud1994/CRNN-Keras) 的實做。但我們針對我們資料集，我們將輸入的圖像改成了彩色圖片，不做額外優化 baseline 分數約為 94.69%。

### 優化與改善

我們分別針對資料本身、前處理、模型與後處理做了些嘗試與調整，這些調整約提昇了 3.37%，準確度由 94.69% 來到了 98.06%。
<br> 

**資料本身**  
在這邊我們重新檢閱了資料集本身清除訓練資料集中品質不好、非車牌與標注錯誤...等資料，以得到較好資料集本質。而針對加入非車牌資料，我們以全 Blank 作為它的標籤，一齊加入訓練，以增強模型的容錯能力。預期看到非車牌資料時能有辨識，並輸出空值，如此一來，可以在利用系統面的設計，來作為例外處理或補救。

另外依照台灣的車牌比例更了輸入資料的尺寸，並添加更多實際場景資料作為訓練集，重新進行訓練。
<br> 

**前處理**  
另外採用第三方 library 的 [aleju/imgaug](https://github.com/aleju/imgaug) 與 [mdbloice/Augmentor](https://github.com/mdbloice/Augmentor) ，來做 **Data Augmentation** 以增加模型的泛化能力。Augmentation 主要針對對比、變焦、偏斜、旋轉進行調整，並對高反光、耀光、雨景、日落、鏡頭模糊...等情況進行模擬。


P.S. Augmentor 的安裝與使用可以看[這篇](/Augmentor-Image-Augmentation-Library-in-Python)。
<br> 

**後處理**  
後處理則是預測後校正，主要針對[維基百科](https://zh.wikipedia.org/wiki/%E8%87%BA%E7%81%A3%E8%BB%8A%E8%BC%9B%E7%89%8C%E7%85%A7)所提及的一些規則進行校正，主要是 0、1、I、O 這些規則的調整。
<br> 

**模型**  
這邊參考語音作法，先針對不同特性的聲音訓練分類器（男生、女生、外國人...），並提取其內某一層當作特徵，當作額外輸入資訊。這邊則是這邊增加了一個車牌樣式分類器，抽取分類器 Dense 層特徵，當作 CRNN LSTM 額外的 input 資料一起訓練。

另外修改了 CTC decode 將它由 greedy search 改成了 prefix beam search。

其他嘗試 ResNet、[CRNN12](https://sci-hub.tw/10.1007/978-981-13-1733-0_9)、[STN(Spatial Transformer Networks)](https://zhuanlan.zhihu.com/p/37110107)，不過最終模型實驗的改善都先擱置了。


<br><br> 

## 參考資料 
1. PLUSTEK 公司 (2009-09-12)。[什麼是 OCR?](http://www.fuji.com.tw/posts/1889) 。檢自 蘋果網 (2020-03-17)。
2. 協同撰寫。[光學字元辨識](https://zh.wikipedia.org/wiki/%E5%85%89%E5%AD%A6%E5%AD%97%E7%AC%A6%E8%AF%86%E5%88%AB) 。檢自 維基百科 (2020-03-17)。 
3. 王梅玲 (2011-04-27)。[技術服務小百科: 光學字元識別 (OCR, Optical Character Recognition)](http://techserviceslibrary.blogspot.com/2011/04/ocr-optical-character-recognition.html)。檢自 技術服務小百科 - Blogger (2020-03-17)。
4. 騰訊雲。[嘿，OCR文字識別瞭解下！](https://codertw.com/%E4%BA%BA%E5%B7%A5%E6%99%BA%E6%85%A7/320/)。檢自 程式前沿 (2020-03-17)。
5. cs0991326 (2011-03-16)。[車牌辨識](https://sites.google.com/site/cs0991326/home/che-pai-bian-shi)。檢自 cs0991326 - Google 協作平台 (2020-03-17)。
6. [車用電子系統導論_13 車牌辨識](http://140.117.156.238/course/IAE/2013/%E8%BB%8A%E7%94%A8%E9%9B%BB%E5%AD%90%E7%B3%BB%E7%B5%B1%E5%B0%8E%E8%AB%96_13.pdf)。檢自 葉家宏老師課程網頁 Courses (2020-06-03)。
7. 白裳 (2020-01-06)。[一文读懂CRNN+CTC文字识别](https://zhuanlan.zhihu.com/p/43534801)。檢自 机器学习随笔 - 知乎 (2020-03-17)。
8. AI科技園 (2019-11-01)。[大話文本識別經典模型：CRNN](https://kknews.cc/tech/pvgpqk8.html)。檢自 每日頭條 (2020-06-03)。
9. z.defying (2019-10-20)。[理解文本识别网络CRNN](https://zhuanlan.zhihu.com/p/71506131)。檢自 深度学习漫谈 知乎 (2020-06-03)。
10. Madcola (2019-01-29)。[【OCR技术系列之七】端到端不定长文字识别CRNN算法详解](https://www.cnblogs.com/skyfsm/p/10335717.html)。檢自 冠军的试炼 (2020-06-03)。
11. 左左左左想 (2017-07-30)。[语音识别：深入理解CTC Loss原理](https://blog.csdn.net/Left_Think/article/details/76370453)。檢自 CSDN博客 (2020-06-03)。
12. lili on。[CTC理论和实战](http://fancyerii.github.io/books/ctc/)。檢自 李理的博客 (2020-07-10)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-12</summary>
  <ul class="timestamp">
    　<li>2020-08-12 發布</li>
    　<li>2020-07-10 完稿</li>
    　<li>2020-03-17 起稿</li>
  </ul>
</details>
