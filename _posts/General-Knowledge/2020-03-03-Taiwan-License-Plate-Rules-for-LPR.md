---
title: 臺灣車牌規則用於車牌辨識
date: 2020-03-03
categories:
- General-Knowledge
tags:
- OCR
- LPR
- CV
--- 

去年（2019）年年底接到**車牌辨識**的案子，主要是負責 <span class='highlighting'>OCR（Optical Character Recognition）</span> 這個部份。
  
不過這篇跟 OCR 沒關係，而是主要專注在**車牌**這件事上。 至於 OCR 那篇還在我草稿夾，連怎麼寫頭緒都還沒理出來呢 Orz...
    
<div class="alert info"> 
:information_source: <span class="head">關於文中的 Model</span>    
  
下文中所提到的 Model 指的是 CRNN + CTC Loss。而機器學習的訓練一些考量也是基於此模型出發的。
</div> 

<!--more-->

<br><br>

## 前言 

因為車牌是有<span class='highlighting'>地域性</span>的，各國車牌的字型、格式、顏色...等都不盡相同。雖然我不需要進行字模比對，但還是需要掌握這些規則，作為訓練特徵或 post-processing 的校正流程。

如果對車牌的演變或細節有興趣的，可以關注這個 [歐沃車牌情報站](https://www.facebook.com/TaiwanLicensePlate/) 這個粉絲團，或者是他的 Medium [車牌研究報告](https://medium.com/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A) ，關於這些這位作者考察得很詳細，不過可惜因為健康因素，最近都沒有更新 :crying_cat_face: 

<center> <img src="https://i.imgur.com/UNz6ZLZ.jpg" alt="日本車牌"></center>
<center class="imgtext">日本車牌（圖片來源: <a href="https://zh.wikipedia.org/wiki/%E6%97%A5%E6%9C%AC%E8%BB%8A%E8%BC%9B%E8%99%9F%E7%89%8C" class="imgtext">維基百科</a>）</center>


<br><br> 

## 目前車牌

距離新式第八代車牌啟用至今已經七年多了...時間還真快阿...（莫名的感嘆起來了...

由於七換八沒有實施全面換發，因此目前是採兩代車牌並行使用的做法。車牌字元由<span class='highlighting'>英文字母與阿拉伯數字混用</span>（不記入軍、試、臨、使...等特殊車牌），若僅依照 `-` 進行區分，共有 `2-4`、`4-2`、`2-2`、`3-2`、`2-3`、`3-3` 與 `3-4`，共 <span class='highlighting'>7 種格式</span>，長度由 <span class='highlighting'>4 到 7</span> 不等。

當然這些車牌是有搭配不同的車種使用，這邊先不考慮這個，因為預期執行完車牌定位後，拿到資料會是車牌本身。不過，也可以先用物件偵測，取得車種資料作為額外資訊，或許可以幫助提辨識效率。

<br>

### 第七代

第七代車牌行之有年，應該也是臺灣車牌史上使用最久的一代。但因其歷史悠久(?)，歷經車輛數目的大幅增加，再加上各種奇怪政令導致車牌樣式五花八門。（有興趣的看看這幾篇：[簡介](https://medium.com/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A-1-%E5%8F%B0%E7%81%A3%E7%AC%AC7%E4%BB%A3%E9%80%9A%E7%94%A8%E8%BB%8A%E8%BC%9B%E8%99%9F%E7%89%8C%E7%B0%A1%E4%BB%8B-2a0cf7414c75)、[演進 I](https://medium.com/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A-3-%E5%8F%B0%E7%81%A3%E7%AC%AC7%E4%BB%A3%E9%80%9A%E7%94%A8%E8%BB%8A%E8%BC%9B%E8%99%9F%E7%89%8C%E7%B7%A8%E7%A2%BC%E6%BC%94%E9%80%B2-i-d74999cc9750)、[演進 II](https://medium.com/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A-4-%E5%8F%B0%E7%81%A3%E7%AC%AC7%E4%BB%A3%E9%80%9A%E7%94%A8%E8%BB%8A%E8%BC%9B%E8%99%9F%E7%89%8C%E7%B7%A8%E7%A2%BC%E6%BC%94%E9%80%B2-ii-2194daa0ebf4)）



<center> <img src="https://i.imgur.com/aFibpWd.gif" alt="第七代各車種號牌繪圖範例"></center>
<center class="imgtext">第七代各車種號牌繪圖範例（圖片來源: <a href="https://zh.wikipedia.org/wiki/%E8%87%BA%E7%81%A3%E8%BB%8A%E8%BC%9B%E7%89%8C%E7%85%A7#cite_note-2" class="imgtext">維基百科</a>）</center>

<br>

#### 外觀

**1. 尺寸**：  
先看看外觀的部份，第七代外觀尺寸是 `320 * 150` 、 `250 * 140` 、 `260 * 150`  與 `320 * 150` 四種，分別常見於自小客車、機車、重機與拖車和電動車上上。

由於各式車牌大小相異，訓練時可以兩種嘗試方向，一個使用 padding 填充、二是選擇一個尺寸後縮放圖片長寬比例。不過考慮到實務上截圖時，三個維度會存在夾角，若進行轉正後，比例可能已經跑掉，倒不如直接選擇一個尺寸並縮放。是說，也可以來試試看不轉正，直接訓練會有啥效果？

<br> **2. 底色與文字顏色**：  
底色與文字顏色方面，除了常見的白底黑字外，又存在白底綠字、綠底白字、白底紅字、紅底白字、黃底黑字等五種，與車種存在一對多的配對關係。

因車種與編碼格式存在關聯性，訓練時或許可採用彩色圖片 RGB 3 個 channel 進行訓練。

<br> **3. 字體**：  
七代車牌採用的是<span class='highlighting'>現代電腦字體風格</span>，雖然有將字寬由 10mm 加粗至 12mm，意圖提昇準確度。

不過因為文字本身特性，與第六代相比，本來就缺少了些特徵點，這也導致第七代的車牌特別容易偽造與修改。這也反應到訓練過程中，導致當車牌被遮擋時，訓練難會變得艱難。

<center> <img src="https://i.imgur.com/BRsKmiY.png" alt="第六代與第七代號牌數字例示"></center>
<center class="imgtext">第六代與第七代號牌數字例示（圖片來源: <a href="https://medium.com/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A-1-%E5%8F%B0%E7%81%A3%E7%AC%AC7%E4%BB%A3%E9%80%9A%E7%94%A8%E8%BB%8A%E8%BC%9B%E8%99%9F%E7%89%8C%E7%B0%A1%E4%BB%8B-2a0cf7414c75" class="imgtext">車牌研究報告 Medium</a>）</center>
<br>

P.S. 我真的覺得臺灣歷代車牌中，第六代的字體最好看。 :heart_decoration: 


<br>

#### 編碼格式

在七代車牌字元長度若不計算分隔符號的話為 4 到 6，有 `2-4`、`4-2`、`3-2`、`2-3`、`3-3` 與 `2-2` 共六種格式。

<br> 先來整理 `2-4` 或 `4-2` 這格式。這種 6 碼的編碼格式是給小客車來使用，但這 2 種編碼格式又可延伸出 4 種排列組合...。

七代甫一開始的設計其實只有 `2-4` 且其字元為 <span class='highlighting'>英英-數數數數</span>，但因設計之初沒有考慮到舊牌換領，導致很快就面臨車牌不足的窘境。因此後來又推出 <span class='highlighting'>英數-數數數數</span> 與 <span class='highlighting'>數英-數數數數</span> 兩種。

但這樣的設計又撐了幾年後，最終還是再度面臨車牌不足的狀況。而此時八代換牌的前置作業尚未完成，公路總局乾脆把編碼區對調，因此產生了 `4-2` 的編碼格式，其衍生的字元組合有 <span class='highlighting'>數數數數-英英</span> 與 <span class='highlighting'>數數數數-英數</span> 兩種，至於 `數數數數-數英` ，在新式車牌目前逐漸普及的狀況下，應該是不會繼續生產了。

因為這樣的~~胡搞~~修改，打破一開始的七代設計，使得短編碼區不再遵循全英文的規則，而是變成<span class='highlighting'>短編碼區必有一碼為英文</span>。而為了避免英數混淆搖，在之後無論是 `英數` 開頭、`數英` 開頭、 短編碼區結尾的車牌中，短編碼區移除掉 `I`、`O` 、`1` 、`0` 四個字元。換句話說，只有 `AI` 開頭的車牌存在，其餘 `A1` 開頭、`AI` 結尾、`A1` 結尾，都是不存在的。

不過，在短編碼區必有一碼為英文得這個規則，在<span class='highlighting'>租賃車牌</span>這邊被打破過，曾出現過 <span class='highlighting'>數數數數-數數</span> 與 <span class='highlighting'>數數-數數數數</span> 兩種車牌，但這兩碼必是相同數字，且不使用 `11` 與 `00`。


<br> 而 `3-2` 與 `2-3` 這兩種格式的短編碼區也大抵適用上述 `2-4` 或 `4-2` 中的介紹。另外，拖車所使用的 `2-2`  與機車所使用的 `3-3`，有都存在兩編碼區互換狀況，且都打破全英文的規則。

<br> 最後在根據報導與介紹，有發生過 `B` 與 `8` 以及 `D` 與 `0` 被誤判的狀況，所以當預測結果出來時，若在數字區，出現 `B` 與 `D` 或許可以考慮用 `8` 或 `0` 還替換。

<center>...</center>

<br>

### 第八代

第八代是目前主要的發行車牌，可在細分八代一式與八代二式，這等等在說。八代在推行之初有點像是急就章，不像七代在設計編碼時還做了大量研究與民意調查，雖然後來還是搞出一堆問題，不過好歹還是有做，但八代的編碼則是直接就在原格式中<span class='highlighting'>加了一位區段碼</span>敷衍了事。

不過我想原本八代的特點應該在防偽，它們原先設計有三道防偽關卡，第一是分別是左上角的雷射標籤、車牌中間的浮水印，最後則是字體上的細刻。還要搭配車身的識別貼，看來能夠有效防止車牌的偽造及偷竊。

<center> <img src="https://i.imgur.com/11HVwgn.jpg" alt="印度最新版(2008年)統一號牌"></center>
<center class="imgtext">印度最新版(2008年)統一號牌（圖片來源: <a href="http://khalidhazmi.hatenablog.com/entry/TW06" class="imgtext">KH的車牌研究日記</a>）</center>
<br>

整體來說有點像是現在印度車牌的前身，當然沒有這麼多碼，印度這字元數也太多了吧 XDDD 但可惜的是，八代車牌唯一的優點，因為防偽技術的製作部分涉嫌綁標爭議，後來胎死腹中，只留下滿滿缺點（殘念

<center> <img src="https://i.imgur.com/QpTaHe9.gif" alt="第八代各車種號牌繪圖範例"></center>
<center class="imgtext">第八代各車種號牌繪圖範例（圖片來源: <a href="https://zh.wikipedia.org/wiki/%E8%87%BA%E7%81%A3%E8%BB%8A%E8%BC%9B%E7%89%8C%E7%85%A7#cite_note-2" class="imgtext">維基百科</a>）</center>



#### 外觀

**1. 尺寸**：  
一樣先看尺寸的部份，由於編碼擴增一碼，但字體寬度及鑄具排版不變的情況下，導致車牌寬度變寬為 `380 * 160` 、 `300 * 150` 、 `310 * 160` 與 `380 * 160` 四種，分別常見於自小客車、機車、重機與拖車和電動車上。 

不過因為車牌寬度實在太寬，在機車上真的很容易傷到用路人，所以在八代二式中尺寸下修成 `380 * 160` 、 `260 * 140` 、 `300 * 150` 與 `380 * 160`。但也因為尺寸下導致容易變形，使得<span class='highlighting'>字型更不易辨認</span>（Ｍ、Ｎ、Ｗ 與 V、Y）。而且這種誤認沒辦法使用規則修正，真的有點麻煩。


目前申請新號牌時是以八代二式為主，但一式並未被停用，且仍持續在發放，所以仍有機會收到八代一式的車牌資料。

<br> **2. 底色與文字顏色**：  
在底色與文字顏色這邊，八代完全蕭規曹隨，沒有任何變動。


<br> **3. 字體**：  
八代的字體為了防止偽造，因此改用了澳洲字體，但澳洲字體屬於比例字體，每個字的寬度有所差別；且臺灣是採用等寬字體鑄模，因此部份字元有<span class='highlighting'>內縮</span>的的狀況，例如：M，這樣的內縮會導致特徵不清楚，在解析度低或是斜角度的照片，會增加辨識的困難度。

<center> <img src="https://imgur.com/JBmsSY8.png" alt="第七代與第八代字型對照"></center>
<center class="imgtext">第七代與第八代字型對照（圖片來源: 待補）</center>
<br>

說是防偽，我懷疑選擇澳洲字體的原因只是為了讓 3 跟 8 有明顯區別，免得有人遮一半變造？ 但個人覺得 0 跟 8 還是很好改阿...而且 0 跟 C 更好改。

七八代文字的相異特徵不算多，尤其是八代澳洲字體。對於機器學習來說，要它學習分辨八代澳洲字體的 3 跟 5 是件非常吃力的任務，從下圖可以發現這兩者只差了一個頸部筆劃，一束一撇的差異而已，但對於機器進行特徵提取的時候，這兩個幾乎是同一件事情，再加上 CNN 存在平移不變性，使得辨識難度大大提昇。

<center> <img src="https://i.imgur.com/4bLIE9H.png" alt="澳洲車牌 3和5的差異"></center>
<center class="imgtext">澳洲車牌 3和5的差異（圖片來源: <a href="https://www.tteia.org.tw/magazine/index.php?download_id=380" class="imgtext">臺灣電信月刊</a>）</center>
<br>
 
<br> 說到這個就不得不抱怨一下，為啥不換成六代的字型，辨識度比較高，也可以避免 3 與 8 的問題？若說是不想使用重複字型，且又想從現有字型當基底...德國車牌字體就是個不錯的選擇阿...每個字都有獨立特徵，不怕修改、變造與遮擋...當然車牌全擋了還是沒有救 XD 
 
<center> <img src="https://i.imgur.com/B38GxCZ.png" alt="德國車牌字體"></center>
<center class="imgtext">德國車牌字體（圖片來源: <a href="https://dailycold.tw/4961/%E9%9B%A3%E4%BB%A5%E5%A1%97%E6%94%B9%E7%9A%84%E5%BE%B7%E5%9C%8B%E8%BB%8A%E7%89%8C%E5%AD%97%E9%AB%94/" class="imgtext">每日一冷</a>）</center>
<br>
 

#### 編碼格式
 
這邊分成一、二式來討論。

<br> 在八代一式車牌字元長度若不計算分隔符號的話為 <span class='highlighting'>5 到 7</span>，有 `3-4` 、 `3-3` 與 `2-3`  共六種格式。這套制度因歷時較短，所以目前現況都還符合設計之初的規劃。各規格對應的字元順序分別為 `英英英-數數數數`、 `英英英-數數數` 與 `英英-數數數`。

不過，八代一式在實務推廣上有非常大的問題，它與七代一樣採用<span class='highlighting'>非全車種統一編碼</span>，也就是不同車種之間的編碼格式與順序是獨立的，但監理系統的設計卻是全車種統一編碼。

在七代時，因為所有車種的編碼格式不同（車牌長短不同），所以在監理系統上可以正常運作，但在八代中，如下圖，汽機車是採同一種編碼格式，編碼區間又相同，導致監理系統運作出錯。因此八代一式在運作初期開錯單的新聞時有所聞...一部分也是這個原因...

<center> <img src="https://i.imgur.com/7IQ17KJ.jpg" alt="第八代通用車輛號牌編碼表"></center>
<center class="imgtext">第八代通用車輛號牌編碼表（圖片來源: <a href="https://www.thb.gov.tw/page?node=92d4a6e2-9afb-464d-a2eb-2be8d26d8d89" class="imgtext">交通部公路總局</a>）</center>
<br>

為什麼會說是只有一部份的原因，仔細觀察八代一式的編碼表碼會發現，它除了自身衝突外，它還跟上一代的編碼起衝突...八代一式的自用大貨車跟七代的機車編碼是重疊在一起的。真搞不懂，這麼明顯的錯誤怎麼會讓它立法通過的...

所以監理機關後來才又推出名為<span class='highlighting'>一車一號</span>的八代二式，在八代二式中無論車種，一律以七碼格式取代（`英英英-數數數數`）。

<br> 不過，這些對於訓練模型這些因素影響不大，只是降低引入額外資訊所能帶來的效益。

<br> 最後，關於字元的使用，在八代（無論一、二式），都移除 `I` 、 `O` 與 `4` 三個字元，使得臺灣成為全世界唯一僅使用 9 個數字作為車牌數字部分編碼的國家。

但實做上，由於七代與八代一式的規則混雜在一起，因此再做後處理時可能僅能針對 `3-4` 這個編碼格式進行處理而已。

<br><br> 

## 備註

如果所設計的系統有錯誤處理或信心度不足的處理流程，可以考慮在引入[禁用英文字母部分](https://zh.wikipedia.org/wiki/%E8%87%BA%E7%81%A3%E8%BB%8A%E8%BC%9B%E7%89%8C%E7%85%A7#%E7%9B%B8%E9%97%9C%E8%AD%B0%E9%A1%8C)，當預測結果出現在這列表中就打到錯誤處理流程去...


<br><br> 

## 附件


<center> <img src="https://i.imgur.com/I9Bj7iv.png" alt="原型式、新式、「一車一號」新編碼方式號牌區分對照表"></center>
<center class="imgtext">原型式、新式、「一車一號」新編碼方式號牌區分對照表（圖片來源: <a href="https://www.thb.gov.tw/page?node=92d4a6e2-9afb-464d-a2eb-2be8d26d8d89"  class="imgtext">交通部公路總局</a>）</center>
<br>

  


<br><br> 

## 參考資料 
1. 鄉下老師 (2018-07-07)。[車牌辨識當然是有地域性的](http://blog.udn.com/yccsonar/113103293) 。檢自 鄉下老師 - udn部落格 (2020-03-03)。
2. [歐沃車牌情報站](https://www.facebook.com/TaiwanLicensePlate/) 。檢自 FB 粉絲團 (2020-03-03)。
3. Khalid al-Hazmi。[車牌研究報告](https://medium.com/%E8%BB%8A%E7%89%8C%E7%A0%94%E7%A9%B6%E5%A0%B1%E5%91%8A) 。檢自 車牌研究報告 (2020-03-03)。
4. Khalid al-Hazmi (2017-09-18)。 [<#4> 淺談台灣車牌演進 IV](http://khalidhazmi.hatenablog.com/)。檢自 KH的車牌研究日記 (2020-03-03)。
5. Khalid al-Hazmi (2017-12-10)。 [<#6> 淺談台灣車牌演進 VI](http://khalidhazmi.hatenablog.com/)。檢自 KH的車牌研究日記 (2020-03-03)。
6. 協同撰寫 。[臺灣車輛牌照](https://zh.wikipedia.org/wiki/%E8%87%BA%E7%81%A3%E8%BB%8A%E8%BC%9B%E7%89%8C%E7%85%A7) 。檢自 維基百科 (2020-03-03)。
7. 監理組車輛管理科 (2020-02-26)。[號牌型式及編碼規則表](https://www.thb.gov.tw/page?node=92d4a6e2-9afb-464d-a2eb-2be8d26d8d89) 。檢自 公路總局 (2020-03-03)。
8. 李建興等 (2010)。[即時動態車牌辨識](http://ir.lib.ntust.edu.tw/retrieve/50742/DYNAMIC+REAL-TIME+LICENSE+PLATE+RECOGNITION.pdf)。檢自 技術學刊 第二十五卷 第二期 (2020-03-03)。
9. cevasu (2015-04-10)。[難以塗改的德國車牌字體](https://dailycold.tw/4961/%E9%9B%A3%E4%BB%A5%E5%A1%97%E6%94%B9%E7%9A%84%E5%BE%B7%E5%9C%8B%E8%BB%8A%E7%89%8C%E5%AD%97%E9%AB%94/)。檢自 每日一冷 (2020-03-03)。
10. 周天蔚 (2019-08)。[細說車牌辨識原理與市場](https://www.tteia.org.tw/magazine/index.php?download_id=380)。檢自 臺灣電信月刊 (2020-03-03)。




