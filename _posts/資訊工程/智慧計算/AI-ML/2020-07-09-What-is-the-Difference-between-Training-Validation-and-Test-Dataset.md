---
title: 訓練集、驗證集、測試集的定義與劃分
date: 2021-03-10
is_modified: true
categories:
- "智慧計算 › 人工智慧"
tags:
- AI/ML
- 煉丹常識
- 模型評估
--- 
 
在機器學習，我們所搜集到資料並不能全拿來做訓練，必須保留一些當作測試資料來評估模型的最終表現。
  
最偷懶的狀況下，我會把資料會被切分成測試集（training）跟訓練集（test）兩種。不過一般來說，標準的資料劃分會分成三種，分別是：訓練集（training）、驗證集（validation）和測試集（test）。

<!--more-->
<br><br> 

## 資料集定義

先舉個例來描述三個數據集：

- **訓練集（training)**  
    舉例來說就是上課學習。
- **驗證集（validation）**  
    舉例來說就是模擬考，你會根據模擬考的成績繼續學習、或調整學習方式重新學習。
- **測試集（test）**  
    就像是學測，用來評估你最終的學習結果。
    
    使用**學測**來比喻，是因為<mark>測試集不應該做為參數調整、選擇特徵等依據</mark>。這些選擇與調整可以想像成學習方式的調整，但學測已經考完，你不能時光倒轉回到最初調整學習方式。
    
<br> 

### 訓練集（Training Set）

訓練集（Training Set）主要用在訓練階段，用於模型擬合，直接參與了<mark>模型參數調整的過程</mark>。
<br> 

### 驗證集（Validation Set）

驗證集（Validation Set）是在訓練過程中，用於評估模型的初步能力與<mark>超參數調整的依據</mark>。
 
不過驗證集是非必需的，不像訓練集和測試集。如果不需要調整超參數，就可以不使用驗證集。
<br> 

### 測試集（Test Set）

用來評估模型最終的泛化能力。為了能評估模型真正的能力，測試集<mark>不應該</mark>為參數調整、選擇特徵等依據。

<br><br> 

## 資料劃分

我個人較常用來劃分資料的方法有兩種：<mark>留出法</mark> 與 <mark>k-fold</mark>
<br> 

### 留出法（Holdout method）

留出法是按照<mark>固定比例</mark>將資料集劃分爲訓練集、驗證集、測試集，屬於<mark>靜態的</mark>劃分方法。

不過其劃分比例並沒有明確的規定，但有些經驗法則可供參考：

1. **對於小資料量**  
    可遵循 60%、20%、20% 的比例下去劃分。另一種常見的比例是 80%、10%、10%，比例的選擇由你資料集大小來決定。 
2. **對於大資料量**  
    只要驗證集和測試集的數量足夠即可，如有 100 萬筆數據，那麼可分 1 萬當驗證集、 1 萬當測試集，其餘皆可作為訓練集。

另外若是需調整超參數量少，可以適當減少驗證集的資料量，分配更多給訓練集。

<center> <img src="https://i.imgur.com/EzCJTJV.png" alt="留出法"></center>
<center class="imgtext">留出法（圖片來源: <a href="https://blog.csdn.net/lanchunhui/article/details/50522424"  class="imgtext">CSDN博客</a>）</center>

<br>

<div class="alert info">
<div class="head">oldout cross validation / Holdout method</div>
之前，在這邊我是將留出法的英文是用 Holdout cross validation，但我覺得留出法並不算一種交叉驗證，畢竟當測試集劃分後並沒有被交叉使用，它比較像是一種簡單驗證方式，所以我改用了維基百科中 Holdout method。<br>

另外說到維基百科，在 Holdout method 的中文翻譯是直接翻成 Holdout 驗證，不過我覺得留出法這個名字非常的形象，因此我這邊還是保留了這個翻譯。
</div>

<br> 

### k 折交叉驗證（k-fold cross validation）

另一個常用的是<mark>動態的</mark> k-fold 劃分方法，這種方式可以降低數據劃分帶來的影響。雖說是動態，不過測試集仍要事前先切出來，通常會保留 60%~80% 當作訓練資料，其餘作測試集。

而訓練資料會在訓練過程中被化分成 k 份，每次使用 k 份中的 1 份作爲驗證集，其他 k-1 份作爲訓練集，算出一個 Validation Error。交叉驗證重複 k 次，每個子樣本驗證一次，算出 k 個 Validation Error，最後我們再將這 K 個 Validation Error 做平均，用他們的平均分數來做為我們評斷模型好壞的指標。

<center> <img src="https://i.imgur.com/4ubxxws.png" alt="k 折交叉驗證"></center>
<center class="imgtext">k 折交叉驗證（圖片來源: <a href="https://blog.csdn.net/lanchunhui/article/details/50522424"  class="imgtext">CSDN博客</a>）</center>

<br><br> 

## 參考資料 
1. CHEN TSU PEI (2019-12-17)。[機器學習怎麼切分資料：訓練、驗證、測試集. 訓練集、驗證集、測試集分別代表什麼含義，又該怎麼劃分](https://medium.com/nlp-tsupei/%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92%E6%80%8E%E9%BA%BC%E5%88%87%E5%88%86%E8%B3%87%E6%96%99-%E8%A8%93%E7%B7%B4-%E9%A9%97%E8%AD%89-%E6%B8%AC%E8%A9%A6%E9%9B%86-f5a92576d1aa) 。檢自 NLP-trend-and-review｜Medium (2020-06-30)。
2. 區塊鏈遊戲研究院區塊鏈 (2019-12-20)。[一文看懂 AI 訓練集、驗證集、測試集（附：分割方法+交叉驗證](https://www.chainnews.com/zh-hant/articles/879556443394.htm) 。檢自 鏈聞 ChainNews (2020-06-30)。
3. 程序員小新人學習 (2018-08-12)。[訓練集、驗證集、測試集以及交驗驗證的理解](https://kknews.cc/zh-tw/code/jbm8ray.html) 。檢自 每日頭條 (2020-06-30)。
4. 清文 (2018-08-11)。[求解神经网络做十字交叉验证k=10，这种方法到底是得到十个模型还是一个模型](https://www.zhihu.com/question/29350545/answer/466060995) 。檢自 知乎 (2020-06-30)。
5. 協同撰寫。[交叉驗證](https://zh.wikipedia.org/wiki/%E4%BA%A4%E5%8F%89%E9%A9%97%E8%AD%89)。檢自 維基百科 (2021-03-10)。
6. 協同撰寫。[Cross-validation (statistics)](https://en.wikipedia.org/wiki/Cross-validation_(statistics))。檢自 Wikipedia (2021-03-10)。
7. Inside_Zhang (2016-01-15)。[cross-validation：从 holdout validation 到 k-fold validation](https://blog.csdn.net/lanchunhui/article/details/50522424)。檢自 鏈聞 CSDN博客 (2021-03-10)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-03-10</summary>
  <ul>
    <li>2021-03-10 更新：新增圖片、改動留出法英文</li>
    <li>2020-08-10 發布</li>
    <li>2020-07-09 完稿</li>
    <li>2020-06-30 起稿</li>
  </ul>
</details>