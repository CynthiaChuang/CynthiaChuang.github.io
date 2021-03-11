---
title: 【機器學習基石筆記】數學基礎 Week2
date: 2020-01-17
is_modified: false
categories:
- 研讀筆記
- AI/ML
tags:
- Coursera
- 《機器學習基石》
--- 

本週主題 ：**Learning to Answer Yes/No**，就是學會~~說 Yes/No 問句~~，其實是學會進行二元分類（Binary classification）

<!--more-->
<br>

## 上週回顧

[上週課程](/Machine-Learning-Foundations-Study-Notes-Mathematical-Foundations-Week1)提到，在機器學習中存在一個 Learning Algorithm $A$ 與一個假說集合 Hypothesis Set $H$，$A$ 在觀察餵入的資料集合 $D$ 後，會從假說集合挑選一個最符合的 $g$ ，這個 $g$ 會在最後應用階段時，被使用來進行分類。

本週延續上週核發信用卡的例子，發不發卡是一個二元分類問題，使用者資料 $x$ 最終經由 $g$ 後，會得到 $y$，也就是 Yes（發卡）或 No（不發卡）結果。

<br><br>

## 感知器 Perceptron

開始上課前，我先岔開查了<mark>感知器（Perceptron）</mark> 這個詞所代表的意思與定義。

感知器這個名詞緣起於類神經網路時期，這個演算法的概念跟真實的生物神經元傳遞訊號的機制類似。

<center> <img src="http://pic.baike.soso.com/p/20140122/20140122141220-1597040664.jpg" alt="神經元結構"></center>
<center class="imgtext">神經元結構（圖片來源: <a href="https://blog.csdn.net/aws3217150/article/details/46316007" class="imgtext">CarlXie-CSDN博客</a>）</center>
<br>

生物的神經細胞可被視為只有兩種狀態的機器 — 激動時為『是』，而未激動時為『否』。而其狀態，取決於所接收到的輸入訊息，及輸入訊息的強度。當強度超過了某個門檻值時，細胞體會激動產生電脈衝，透過軸突發送訊息給下一個神經元。

如果不管生物學上的例子，感知器其實就是一種二元線性分類器（Linear Binary Classifiers），唯有當輸入的訊息符合標準或門檻值時，才會觸發下一個動作（發卡）。

<br><br>

## Perceptron Hypothesis Set

在發卡的例子中，銀行可能掌握了用戶各種屬性資料，如年齡、性別、年收入、負債、工作經歷...等情況。而每位使用者的資料可以向量來表示：

$$
X = \{x_1, x,2, ... ,x_d\}
$$

銀行會對每個條件進行評分，也就是依照每個條件的正/負相關性給予給與<mark>權重</mark>，例如：年收入的權重給予 1，負債的權重給予 -1...等。而權重也可以用向量來表示：

$$
W = \{w_1,w_2,...,w_d\}
$$

而銀行唯有當這些條件的總分，超過門檻值（threshold , $T$）時，才會核發信用卡：

$$
score = \sum_{i=1}^{d}{w_ix_i}    ,    and
\left\{
\begin{array}{l}
score > T ,\ approve
\\ 
score < T , \ reject 
\\
score = 0 ,\ ignored
\end{array}
\right .
$$

<br><br>

我們將其輸出 $y$ 的結果集合使用符號來表示，可稱為Label，可表示為 $y = \{ +1 (approve) , -1 (reject)\}$ , 0 ignored，因此式子可簡化成：

$$
h(x) =  sign(\sum_{i=1}^{d}{w_ix_i} - T) ,\ where\  h(x) ∈ H
$$

其中

$$
sign(x) =  \left\{
\begin{array}{l}
+1,\ x >0 
\\ 
-1,\ x < 0
\end{array}
\right .
$$

<br><br>
為簡化數學式的表示，可針對數學符號再做進一步的化簡：

$$
\begin{aligned} 
h(x) &=  sign(\sum_{i=1}^{d}{w_ix_i} - T) \\
&=   sign(\sum_{i=1}^{d}{w_ix_i} + \underbrace{(-T)}_{w_0} \times  \underbrace{(+1)}_{x_0})   \\
&=   sign(\sum_{i=0}^{d}{w_ix_i}) \\
&=   sign(W^TX) 
\end{aligned}
$$

<br>

個人經驗雖然兩者意義上相同，但在實務上偏向使用矩陣運算，因為矩陣運算的可以藉由 GPU 使用 CUDA 來加速。

<br><br>

### Perceptrons in $R^2$

為可視化，我們將感知器使用在二維平面上，即表示只使用兩種條件，例如：年收入與負債兩種，若使用越多條件會映射到更高維度的空間。

因此 $h(x)$ 可表示成 $sign(w_0 + w_1x_1+w_2x_2)$，因為 $sign$ 是以 0 為分界線的函數，因此可設 $w_0 + w_1x_1+w_2x_2=0$ ，該式恰是一條直線方程式，而不同權重 $W$ 會對應到不同的直線。

若將平面上個點依照線段來劃分，所有的點會分落在線段兩側，一側為正另一側為負。而我們期望的目標是能找到一條直線 ，剛好能將不同的 Label 劃分開來。

<center> <img src="https://i.imgur.com/JD6GGY3.png" alt="Perceptrons in R^2"></center>
<center class="imgtext">Perceptrons in R^2（圖片來源: <a href="coursera.org/learn/ntumlone-mathematicalfoundations/lecture/GNlJL/perceptron-learning-algorithm-pla" class="imgtext">課程截圖</a>）</center>
<br>

因為感知器的實做其實是藉由一條直線方程，來劃分平面上所有點，因此只能作為一個二元線性分類器（Linear Binary Classifiers）。

<br><br>

## Perceptron Learning Algorithm (PLA)

在上一節中，我們可得知感知器中 Hypothesis Set 的所有可能集合 $H$，也就是平面上所有的直線。一旦有了 $D$ 與 $H$ 後，我們就可以藉由機器學習演算法來挑選最適合的線段。

<br>

### 何謂最適合的線段?

之前提過，機器在觀察餵入的資料會從 Hypothesis Set $H$ 中學到一函數 Hypothesis $g$ ，期望上我們希望 $g$ 越接近 Target function $f$ 越好。但之前也提過，我們並不知曉 $f$ 到底長怎樣。因此實做時根本不可能與 $f$  相互比較。

但我們可使用餵入的資料來協助找到最好的 $g$ ，先忽略錯誤標記及存在雜訊的情況下，我們可以假設所輸入的資料 $x$ 在經由 $f(x)$ 後，可以得到一輸出結果 $y$。 如果我們可以從 $H$ 中找到一條 $g$ ，對每個點其輸出結果與 $f$ 完全一致，我們則認為 $g$ 是個不錯的結果。

因此在這邊對於合適線段的定義應該是，<mark>找到一條只直線 $g$，使資料集中所有點的分類結果與本身的 Label 一致</mark>。

<br>

### 如何找到最適合的線段?

若在 Hypothesis Set $H$ 不大的情況下，可以每條線段逐一檢查。但在平面上的線段是無窮多的，根本不可能一一檢查。

因此這邊採用逐漸修正的方式，在取得一條初始線段 $g_0$ 的情況下，經過不斷的錯誤修正，對線段進行平移旋轉等操作，最終能找到一條 $g_f$ ，這就是 Perceptron Learning Algorithm (PLA) 演算法的核心。
<br>

<div class="alert info">
<div class="head">提醒</div>
$W$ 其實是直線方程的法向量
</div>

<br>

### PLA 演算法

具體演算法流程如下：
1. 初始化權重 $W$ 為 $W_0$，設定 $W_0$ = 0
2. 按序或隨機遍歷所有資料，也就是二維平面上所有的點，找出分類結果與真實標籤不符的資料：
    1. 若存在，則更新權重 $W$ 後，重新執行步驟二。
    2. 若不存在，停止執行。

<br>

其中，我們將所找到分類結果的資料標記為 $(X_{n(t)}, Y_{n(t)})$ ，而權重更新公式如下：

$$
W_{t+1} \leftarrow W_t + Y_{n(t)}X_{n(t)} 
$$

<br>

<center> <img src="https://i.imgur.com/Q5wRz1x.png" alt="向量修正"></center>
<center class="imgtext">向量修正（圖片來源: <a href="https://www.coursera.org/learn/ntumlone-mathematicalfoundations/lecture/GNlJL/perceptron-learning-algorithm-pla" class="imgtext">課程截圖</a>）</center>
<br>

如果 $sign()=-1$，但是 $y=+1$，也就是说 $W$ 和  $X$ 的夾角過大，需要使 $W$ 向  $X$ 靠攏，即左圖。

如果 $sign()=+1$，但是 $y=-1$，也就是说 $W$ 和  $X$ 的夾角過小，需要使 $W$ 與  $X$ 遠離，即右圖。
<br>

雖然 PLA 的核心相當的簡單，但仍有幾個最基本的問題需要解決：
1. 這演算法何時會停下來？
2. 如果停下來，找到的 $g_f$ 真的接近 $f$ 嗎？

<br><br>

## Guarantee of PLA

PLA 演算法停止必須滿足訓練集所有樣本都是<mark>線性可分的（linear separable）</mark>，也就是說平面上必須至少存在一條線的，並且使的線的一側全為藍點，另一側全為紅點。
<br>
<center> <img src="https://lh3.googleusercontent.com/proxy/Ye7LGG-0fNVwehCXHyoR4HYxpSCVFKqx5kUnOeWyJhV3Aeen6nqY7-Omqw99UYiy6f9x=w1200-h630-p-k-no-nu" alt="線性可分"></center>
<center class="imgtext">線性可分（圖片來源: <a href="https://www.coursera.org/learn/ntumlone-mathematicalfoundations/lecture/XckQ1/guarantee-of-pla" class="imgtext">課程截圖</a>）</center>
<br>

### [證明] PLA 會停止運行
##### `TODO: 證明改成英文的， LaTeX 搭中文好醜` 

1. 評估 $W_{t+1}$ 及 $W_{t}$ 與 $W_f$ 的相近程度  

    $\because$ $D$ 為線性可分    
    $\therefore$ 必存在一條直線其法向量為 $W_f$ ，使 $D$ 中所有點皆符合 $Y_n = sign(W_f^TX_n)$  
    <br>
    
    $\because$ 已知存在一條法向量為 $W_f$ 的直線且 $Y_n = sign(W_f^TX_n)$  
    $\because$ 可知平面上任一點皆與法向量為 $W_f$ 的直線存在一定距離  
    $\therefore$ 故推知 $Y_nW_f^TX_n > 0$ 且 $min( Y_nW_f^TX_n) > 0$  
    <br>
    
    $\because$  在運行  $t$ 次時，存在一個分類錯誤的點  $(X_{n(t)}, Y_{n(t)})$  
    $\because$ 又 $(X_{n(t)}, Y_{n(t)}) \in D$   
    $\therefore$  可推知  $Y_{n(t)}W_f^TX_{n(t)} \geq\  min( Y_nW_f^TX_n) > 0$   
    <br>
    
    為評估 $W_f$ 與 $W_{t+1}$ 的相近程度，故使向量內積　
    
    $$
    \begin{aligned}
    W_f^TW_{t+1}  &= W_f^T( W_t + Y_{n(t)}X_{n(t)} ) \qquad \because\  W_{t+1} \leftarrow W_t + Y_{n(t)}X_{n(t)}  \\
    &= W_f^TW_t +  W_f^TY_{n(t)}X_{n(t)} ) \\
    &\geq  W_f^TW_t +  min( Y_nW_f^TX_n)  \qquad  \because\   Y_{n(t)}W_f^TX_{n(t)} \geq\  min( Y_nW_f^TX_n)  \\
    &>  W_f^TW_t +   0 \qquad  \because\    min( Y_nW_f^TX_n)  > 0
    \end{aligned} 
    $$
    
    可得 $W_f^TW_{t+1}  >  W_f^TW_t$ ，向量內積隨執行次數增加而增加。

<br>

2. 向量內積的增長可能原因有二：<mark>向量長度增加或向量夾縮小</mark>。為排除向量長度增長對向量內積增加的影響，進行下列證明：  

    $\because$ 在運行 $t$ 次時，存在一個分類錯誤的點  $(X_{n(t)}, Y_{n(t)})$ ，使的 $sign(W_t^T, X_{n(t)}) \neq Y_{n(t)}$  

    $\therefore$ 可推知 $Y_{n(t)}(W_t^T, X_{n(t)}) \leq 0$ 

    <br>

    $\because$ 已知 $W_{t+1} \leftarrow W_t + Y_{n(t)}X_{n(t)}$ ，故可得  

    $$
    \begin{aligned}
         { \parallel  W_{t+1} \parallel}^2 &= {\parallel W_t + Y_{n(t)}X_{n(t)} \parallel} ^2 \\
         &=  {\parallel W_t \parallel} ^2  + 2 W_tY_{n(t)}X_{n(t)} +  {\parallel Y_{n(t)}X_{n(t)} \parallel} ^2 \\
         &\leq {\parallel W_t \parallel} ^2  + 0 +  {\parallel Y_{n(t)}X_{n(t)} \parallel} ^2  \qquad \because   Y_{n(t)}(W_t^T, X_{n(t)})   \leq  0  \\
          &\leq {\parallel W_t \parallel} ^2  + {max\parallel Y_{n(t)}X_{n(t)} \parallel} ^2  \\
          &\leq {\parallel W_t \parallel} ^2  + {max\parallel X_{n(t)} \parallel} ^2  \qquad  \because  Y_{n(t)} 為  
        \pm1，取平方後無影響 \\
      \end{aligned}  \\
      $$
      
    可得 ${ \parallel  W_{t+1} \parallel}^2 \leq {\parallel W_t \parallel} ^2  + {max\parallel X_{n(t)} \parallel } ^2$ ，證明在運行中向量長度增加緩慢

<br>

3. 證明隨執行次數增加而 $W_{t+1}$ 與 $W_f$ 逐漸靠近    
    $\because$ 已知向量內積公式為 $\vec a \cdot \vec b = |\vec a||\vec b| cos{\theta}$  
    $\because$  又從(1)中得知，向量內積隨執行次數增加而增加  
    $\because$  又從(2)中得知，在運行中向量長度增加緩慢  
    $\therefore$  向量內積的增加，為 $cos{\theta}$ 增加所導致  
    $\therefore$  證明隨執行次數增加而 $W_{t+1}$ 與 $W_f$ 逐漸靠近  

<br>

4. 證明PLA會停止  
    設定初始向量為 $W_0 = 0$，從 0 開始進行 $T$ 次迭代  
    <br>
   
    由(2)推知： 
   
    $$
    \begin{aligned}
        { \parallel  W_T \parallel}^2   &\leq   {\parallel W_{T-1} \parallel} ^2  + {max\parallel X_{n} \parallel} ^2  \\
        &\leq   {\parallel W_{T-2}  \parallel} ^2 + {max\parallel X_{n} \parallel} ^2   +{ max \parallel X_{n} \parallel} ^2  \\
        &\leq   {\parallel W_{0}  \parallel} ^2 + T \cdot  {max\parallel X_{n} \parallel} ^2 \\
        &\leq   T \cdot  {max\parallel X_{n} \parallel} ^2 \qquad \because\  W_{0} = 0 \\
        \end{aligned}
    $$

   <br>
   
    由(1)推知：
    
    $$
    \begin{aligned}
    W_f^TW_{T}  &\geq  W_f^TW_{T-1}+  min( Y_nW_f^TX_n)  \\
    &\geq  W_f^TW_{T-2}+  min( Y_nW_f^TX_n)   +  min( Y_nW_f^TX_n)   \\
    &\geq  W_f^TW_{0}+  T \cdot min( Y_nW_f^TX_n)    \\
    &\geq  T \cdot min( Y_nW_f^TX_n)    \\
    \end{aligned} 
    $$

   <br>
   
    結合上述兩結論，可得
    
    $$
    \begin{aligned}		
        \frac{W_f^T}{ \parallel W_f\parallel} \frac{W_T}{ \parallel W_T \parallel}   &\geq  \frac {T \cdot min( Y_nW_f^TX_n)}{\parallel W_f\parallel \parallel W_T \parallel} \\
         &\geq  \frac {T \cdot min( Y_nW_f^TX_n)}{\parallel W_f\parallel \sqrt{T \cdot  {max\parallel X_{n(t)} \parallel } ^2  }} \\
         &\geq  \frac {\sqrt{T} \cdot min( Y_nW_f^TX_n)}{\parallel W_f\parallel  max\parallel X_{n(t)} \parallel  } \\
         &\geq \sqrt{T}  \frac { min( Y_nW_f^TX_n)}{\parallel W_f\parallel  max\parallel X_{n(t)} \parallel  } \\
          &\geq \sqrt{T}  \cdot  C , \qquad C = \frac { min( Y_nW_f^TX_n)}{\parallel W_f\parallel  max\parallel X_{n(t)} \parallel  } \\
         \end{aligned} \\
    $$

   <br>
   
    已知內積中正規化後，乘積最多為 1  
    故可得  

    $$
    \begin{aligned}		
    1 &\geq   \frac{W_f^T}{ \parallel W_f\parallel} \frac{W_T}{ \parallel W_T \parallel}   \\
     & \geq  \sqrt{T} \frac { min( Y_nW_f^TX_n)}{\parallel W_f\parallel  max\parallel X_{n(t)} \parallel  } \\	 
    \end{aligned}
    $$

   <br>
   
    化簡後  

    $$
    \frac{R^2}{\rho^2} = \frac {\parallel W_f\parallel^2  max\parallel X_{n(t)} \parallel^2 } { min( Y_nW_f^TX_n)^2}  \geq T
    $$

   <br>
    
    其中

    $$
    R^2 =  max\parallel X_{n(t)} \parallel^2  \qquad \qquad \rho^2 =  min(Y_n  \frac {W_f^T}{\parallel W_f\parallel }X_n)
    $$

   <br>

    從最後一條式子看來T是有上限的，因此在線性可分的情況下，<mark>PLA 最終會停止</mark>。
 
<br><br>

## Non-Separable Data

PLA 演算法的優缺點相當清楚，優點是簡易實做，可以適用於任何維度，缺點是<mark>資料必須是線性可分的</mark>，可是是否為線性可分通常<mark>無法事前得知</mark>。即使資料是線性可分的，但因為時間複雜度高，執行時間也會耗費相當久。
<br>

另一種情況是 $f$ 產生的資料本身是線性可分，但因雜訊（noise）、輸入錯誤...等因素，最終產生出線性不可分的資料，也會使得 PLA 無法停止。

為避免無窮迴圈的情況發生，因此我們退而求其次，不找不犯錯的線，改找犯錯最少的線：

$$
W_g = argmin \sum_{n=1}^N (Y_n \neq sign(W^TX_n)
$$

<br>

但此類問題已經被證實是一個 **NP-hard 問題**，不可能找到最佳解，只能找到近似解。
<br>

這邊提出了一個近似的演算法 Pocket，它本質是一個貪婪演算法，屬於 PLA 演算法的變形，演算法如下：

1. 初始化權重 $W$ 為 $W_0$，設定 $W_0$ = 0  
2. 隨機遍歷所有資料，找出分類結果與真實標籤不符的資料：  
    1. 若存在，則更新 $W$ 後，並統計新線段存在的錯誤點數。若總數小於所記錄，則更新記錄與對應權重。  
    2. 若不存在，停止執行。  

3. 執行前，請先設定中止條件，如:執行次數、錯誤點少於多少時停止...等。  

<br><br>

## 課堂測驗

**Q1 . Assume that each email is represented by the frequency of keyword occurrence, and output +1 indicates a spam. Which keywords below shall have large positive weights in a**

1. [ ] coffee, tea, hamburger, steak  
2. [x] free, drug, fantastic, deal  
3. [ ] machine, learning, statistics, textbook  
4. [ ] national, Taiwan, university, coursera  
 
<br>

**Q2 . Let  $n = n(t)$, according to the rule of PLA below, which formula is true?**

$$ 
sign(w^T_tx_n)≠y_n  ,\ and\ ,\ ,   w_{t+1}   \leftarrow w_t + y_nx_n
$$

1. [ ] $w^T_{t+1}x_n = y_n$  
2. [ ] $sign(w^T_{t+1}x_n)=y_n$  
3. [x] $y_nw^T_{t+1}x_n \geq  y_nw^T_tx_n$  
4. [ ] $y_nw^T_{t+1}x_n < y_nw^T_tx_n$  

<br>

**Q3 . Define  $R^2 = max_n \parallel x_n\parallel^2$ , $\rho = min_ny_n \frac {w^T_f}{\parallel x_f \parallel} x_n$. We want to show that  $T \leq □$. Express the upper bound $□$ by the two terms above.**

1. [ ] $R/\rho$   
2. [x] $R^2/\rho^2$   
3. [ ] $R/\rho^2$   
4. [ ] $\rho^2/R^2$   

<br>

**Q4 .Since we do not know whether   $D$  is linear separable in advance, we may decide to just go with pocket instead of PLA. If $D$  is actually linear separable, what's the difference between the two?**

1. [x]  pocket on    $D$  is slower than PLA  
2. [ ] pocket on    $D$   is faster than PLA  
3. [ ] pocket on    $D$   returns a better  $g$  in approximating  ff  than PLA  
4. [ ] pocket on    $D$   returns a worse  $g$  in approximating  ff  than PLA  

 

<br><br>

## 其他連結

1. [機器學習基石筆記目錄](/Machine-Learning-Foundations-Study-Notes-Contents/)
    

<br><br>

## 參考資料

1. [感知器｜維基百科](https://zh.wikipedia.org/wiki/%E6%84%9F%E7%9F%A5%E5%99%A8)
2. [FUNcLogs: 感知學習演算法(Perceptron Learning Algorithm)｜白話說明](http://function1122.blogspot.com/2010/10/perceptron-learning-algorithm.html)
3. [[資料分析&機器學習] 第3.2講：線性分類-感知器(Perceptron) 介紹｜Yeh James – Medium](https://medium.com/@yehjames/%E8%B3%87%E6%96%99%E5%88%86%E6%9E%90-%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-%E7%AC%AC3-2%E8%AC%9B-%E7%B7%9A%E6%80%A7%E5%88%86%E9%A1%9E-%E6%84%9F%E7%9F%A5%E5%99%A8-perceptron-%E4%BB%8B%E7%B4%B9-84d8b809f866)
4. [Linear separability｜Wikipedia](https://en.wikipedia.org/wiki/Linear_separability)
5. [投影屏15页的constant如何推导出？｜coursera](https://www.coursera.org/learn/ntumlone-mathematicalfoundations/discussions/weeks/2/threads/2LLNA2XXEeeA2A7nkPb23A)    

<br><br>

## 參考筆記

1. [机器学习基石笔记2——在何时可以使用机器学习(2)｜杜少 - 博客园](http://www.cnblogs.com/ymingjingr/p/4271761.html)
2. [机器学习基石笔记2——在何时可以使用机器学习（2）｜Deribs4 - 博客园](https://www.cnblogs.com/Deribs4/p/5399759.html)
3. [听课笔记（第二讲）： Perceptron-感知机 (台湾国立大学机器学习基石）｜豆瓣](https://www.douban.com/note/319669984/)
4. [Coursera台大机器学习基础课程学习笔记1 -- 机器学习定义及PLA算法｜HappyAngel - 博客园](http://www.cnblogs.com/HappyAngel/p/3456762.html)