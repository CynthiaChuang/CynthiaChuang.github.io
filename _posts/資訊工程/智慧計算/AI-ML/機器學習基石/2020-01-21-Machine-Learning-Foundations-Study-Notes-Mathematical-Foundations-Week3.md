---
title: 【機器學習基石筆記】數學基礎 Week3
date: 2020-01-21
is_modified: false
categories:
- "智慧計算 › 人工智慧"
tags:
- AI/ML
- Coursera
- 機器學習基石
- 讀書筆記
--- 

本週主題：Types of Learning，介紹機器學習各種類型的問題，將問題依照不同的標準進行分類。
<!--more-->



## Learning with Different Output Space
第一種分類是依照輸出空間分類，這邊介紹了四種分類：


### 二元分類 Binary Classification 
第一種就是<mark>是非題</mark>，輸出的標籤只有兩種，不是 Yes 就是 No。在上一節中信用卡發卡例子，就是一個標準的二元分類問題；另外一個例子則是在醫療用上，判斷病人是否罹患癌症。

用較為正式的定義就是，在二元分類中，其輸出為離散的且僅具有兩類，可表示為 $y = \{+1, -1\}$。


### 多元分類 Multi-class Classification
這種類型的題目輸出的標籤不只兩種，可能具有 $k$ 種有限的分類，因此 $y$ 可表示為  $\{1, 2, 3, ..., k\}$，老師稱它是一個<mark>單選題</mark>。

這種應用情況在現實中比較常見，主要用於<mark>辨識（Recognition）</mark>，無論圖片辨識或是語音辨識都是屬於這類；或是判斷病人罹患的屬於那一種癌症也是屬於多元分類。嚴格說來，二元分類屬於多元分類中的一種特例。

　
### 回歸分析 Regression  
不同於分類問題的輸出標籤都是離散的，在回歸分析中它的輸出是<mark>連續的值</mark> $y = \mathbb{R}$ ，或是輸出一個<mark>區間</mark> $y= [lower, upper] \subset R$。不同於選擇題，它更像一題<mark>計算題</mark>，因為其輸出標籤有無限多種可能。

常見的應用如：預測股票、明天天氣溫度、病人需要住院幾天...等。在統計學中對回歸分析的問題已有較為成熟的研究，因此常會借用統計學方面的工具，來解決或建構更複雜的演算法。

　
### 結構學習 Structured Learning
除了常見的分類或預測問題，還有其他更為複雜的問題，結構學習就是其中一種。

在自然語言中會有<mark>詞性標注</mark>需求，結構學習就是在處理這類問題，它會分析句子結構並為每個詞標上合理的詞性，最後給出最有可能的詞性結構或是或是將可能正確的詞性結構窮舉出來。

實際上，結構學習的一些解法都是從多類別的分類問題延伸出來的。



## Learning with Different Data Label
另外也可依照資料的標記方式來進行分類，常見的也有四種：


### 監督式學習 Supervised Learning
監督式學習，在訓練的過程中不僅給了機器輸入資料，還告訴它正確的答案，也就是給定資料中已經<mark>全部</mark>做好標記。舉例來說：我給機器看了 1000 張貓跟狗的照片後，拿出一張不在訓練集中照片，問它照片中的動物是貓是狗。

這種的訓練成本相當的昂貴，需要耗費大量的人力來做標記，在專業領域中，如：醫療，更需要專業人事來協助標記，非常的耗時、耗力、更耗錢，眼睛還有脫窗的危險！ ￩ 親身經驗！

記得上次自己標訓練資料，雖然有用一些規則跟模型協助標記，但還是標到眼睛快脫窗了...最後才標了一百多萬筆，但我的資料有快五千萬筆阿 ◢···· 崩╰(〒皿〒)╯潰····◣ 


### 非監督式學習 Unsupervised Learning
在這種訓練情況下，訓練資料沒有標準答案，因此機器只能自行摸索、找出潛在的規則進行分類，因此機器在學習並不知道它的分類結果是否正確。

這種類型的問題最常見的問題是分群問題（Clustering），其他例如：密度評估（Density Estimation）、異常檢測（Anomaly Detection 或 Outlier Detection）也是非監督式學習的應用。

非監督式學習雖然不需要事先以人力進行標注，但因不具有標籤，很難得到如同監督式一樣近乎完美的結果，也較難衡量演算法的好壞。不過有時候會分出滿有趣的結果的。


### 半監督學習 Semi-supervised Learning  
顧名思義，一種介於監督式與非監督式學習之間。

在資料集龐大或標記成本太高的情況下，因此僅標注資料集中部分的資料，期望能提高分類的精準度，但又可降低標記資料的成本，我那個沒標完的資料集就是屬於這一部分。常見的例子如：人臉辨識、藥效預測...等。

一個小問題關於 semi 這個字的發音，到底是 se-"my"  ([google 翻譯發音](https://translate.google.com/?hl=zh-TW#view=home&op=translate&sl=auto&tl=zh-TW&text=Semi) )  還是 se-"me" ([劍橋詞典發音](https://dictionary.cambridge.org/zht/%E8%A9%9E%E5%85%B8/%E8%8B%B1%E8%AA%9E-%E6%BC%A2%E8%AA%9E-%E7%B9%81%E9%AB%94/semi?q=Semi))?


### 強化學習 Reinforcement Learning (RL)
另一種學習方式，學習的過程有點類似在教導小孩或訓練寵物，從一開始麼都不懂，通過不斷的嘗試，從周圍環境中的得到回饋，並依此修正行為的一種學習方式，著名圍棋機器人 Alpha go 就有使用到這樣的學習方式。

以寵物的訓練過程舉例，寵物聽到指令後，例如：『坐下』，牠會進行行動選擇（Action Selection）做出一個動作，例如：聽到坐下，牠跟你握手，或許做出的動作並不正確但還可以接受的狀況，因此你給了牠一個餅乾作為<mark>獎勵</mark>；另一種可能是，牠聽到做坐下，卻對你狂吠一頓，因此你責罵牠作為<mark>懲罰</mark>。透過這樣的方式，牠會學到聽到坐下這兩個字的時候，應該做出怎樣的反應，才可以收到獎勵而不會被懲罰。

最常見的的例子就是廣告系統，你應該發現過當你點擊過某種類型的廣告後，之後這種類型的廣告會一直陰魂不散的跟著你，因為你的點擊給系統做出一個類似回饋的獎勵。

說到這個忽然想到，之前我們公司會在 APP 裡面埋廣告，結果過沒多久收到使用者的負評說：『你們可不可以不要一直投放色情廣告阿！』（笑翻了我 XD ）



## Learning with Different Protocol
第三種分類是依照訓練時，餵入資料的方式進行分類：


### 批次 Batch
這種是最常見的訓練方式，每一回訓練時就為餵入一批資料，讓機器從中學習。

老師形容這是<mark>填鴨式教育</mark>，給你一本書，讓你自己把它吃透，這樣你就學會了。我倒是想到指考前的題海戰術，每天寫個 N 張考卷，最後就算搞不懂也會寫了。


### 在線 Online
這種訓練是透過序列化的方式一筆一筆接收資料來進學習，比較像是老師手把手一題一題地教你做題目，上一節介紹的 PLA 就是屬於這種方式，它針對錯誤點一個一個地進行調整； RL 的訓練也是屬於這種方式，因為你寵物一次只能接受一條指令而已。


### 主動 Active
這種學習方式像是主動提問的學生，當機器遇到不卻確定的例子時，它回主動向你提問，希望你能提供正確的標記，得到回饋就標注該資料，調整後繼續學習。

想到的例子是，當上傳照片到臉書時，它會主動幫你標記照片中的人，但當它遇到不太確定的人它會主動問你：『這是 XXX 嗎？』，這就屬於這種學習方式，不過我很討厭被標記所以都把標記功能給關了就是...



## Learning with Different Input Space
最後一種分類則是，由給定的資料型態，或者說是給定的特徵 Features 進行分類：


### 具體特徵 Concrete Features
這種學習方式是直接給出了經過處理或統計後的特徵，通常是由人類或機器進行過前處理，具有 Domain Knowledge，因此對機器學習來說較為簡單。


### 原始特徵 Raw Features
最常見的原始特徵屬於圖片或是錄音，這種資料都需要在經過<mark>特徵工程</mark>（Feature Engineering），提取出具體的特徵，提取方式可以是人類定義的一些規則或是利用機器學習，例如：深度學習。


### 抽象特徵 Abstract Features
這種給定資料會是一些類似 ID 之類等看似無意義資訊，需要在經過特徵轉換，才能進行特徵提取。

以老師舉的例子，你會得到每個使用者的 ID 以及這 ID 評分過的歌曲，必須分別找出 ID 中的使用者特徵，如：年齡、居住地、語言...等，與歌曲的特徵，如：曲風、演唱者、類型、語言...等，將所得資料轉化成具體特徵後，才能進行特徵的提取。

<p class="paragraph-spacing"></p>

一般來說，若所提供的特徵越抽象，機器需要費力氣從中找出具體特徵，才能進行學習。因此特徵越抽象，學習難度會越高。



## 課堂測驗
**Q1.The entrance system of the school gym, which does automatic face recognition based on machine learning, is built to charge four different groups of users differently: Staff, Student, Professor, Other. What type of learning problem best fits the need of the system?**

1. [ ] binary classification  
2. [x] multiclass classification  
3. [ ] regression  
4. [ ] structured learning  
<p class="paragraph-spacing"></p>

**Q2. To build a tree recognition system, a company decides to gather one million of pictures on the Internet. Then, it asks each of the 1010 company members to view 100100 pictures and record whether each picture contains a tree. The pictures and records are then fed to a learning algorithm to build the system. What type of learning problem does the algorithm need to solve?**

1. [ ] supervised  
2. [ ] unsupervised  
3. [x] semi-supervised  
4. [ ] reinforcement  
<p class="paragraph-spacing"></p>

**Q3. A photographer has 100,000100,000 pictures, each containing one baseball player. He wants to automatically categorize the pictures by its player inside. He starts by categorizing 1,0001,000pictures by himself, and then writes an algorithm that tries to categorize the other pictures if it is `confident` on the category while pausing for learning from human input if not. What protocol best describes the nature of the algorithm?**

1. [ ] batch  
2. [ ] online  
3. [x] active  
4. [ ] random  
<p class="paragraph-spacing"></p>

**Q4. Consider a problem of building an online image advertisement system that shows the users the most relevant images. What features can you choose to use?**

1. [ ] concrete  
2. [ ] concrete, raw  
3. [ ] concrete, abstract  
4. [x] concrete, raw, abstract  



## 其他連結
1. [機器學習基石筆記目錄](/Machine-Learning-Foundations-Study-Notes-Contents/)
    


## 參考資料
1. [監督式學習？增強學習？聽不懂的話，一定要看這篇入門的機器學習名詞解釋！｜INSIDE](https://www.inside.com.tw/article/9945-machine-learning)
2.  [什么是 强化学习 (Reinforcement Learning) - 强化学习 Reinforcement Learning ｜莫烦Python](https://morvanzhou.github.io/tutorials/machine-learning/reinforcement-learning/1-1-A-RL/)



## 參考筆記
1. [机器学习基石笔记3——在何时可以使用机器学习(3)(修改版)｜杜少 - 博客园](http://www.cnblogs.com/ymingjingr/p/4271795.html)
2. [机器学习基石笔记3——在何时可以使用机器学习（3）｜Deribs4 - 博客园](https://www.cnblogs.com/Deribs4/p/5403542.html)
 