---
title: 常見評價指標：Accuracy、Precision、Recall、F1、ROC-AUC 與 PR-AUC
date: 2020-01-09
is_modified: false
categories:
- AI/ML
tags:
- Evaluation Criteria 
- 煉丹常識
--- 

當評估（Evaluation）一個模型的好壞時，不能總是依靠體感來挑選，因此需要一些量化指標去判定它的好壞。常見的量化指標有 **Accuracy**、**Precision**、**Recall** 與 **F1-Measure**。有時也會使用 **ROC-AUC** 與 **PR-AUC** 還評估在相同資料集下的表現結果。

<!--more-->
<br> 

## 名詞定義
假定一個具體場景作為例子：
> 假如現在有 100 筆檢體進行流感快篩，其中已知 70 位為正常人、30 位為流感病患。快篩目的是找出所有的流感病患，現在快篩結果顯示 50 人罹患流感，但這 50 人之中僅有 16 位真的罹患流感，其他 34 位屬於正常人但被誤判的情況。

<br> 依照此情境我們可以繪製出下列表格：

|       | 被診出罹患流感 | 未被診出罹患流感 | |
|-----| ------------ | ------------  | --- |
| **病人** | 16 人 （TP） | 14 人 （FN）  | 30 人 | 
| **正常人** | 34 人  （FP） | 36 人  （TN） | 70 人 | 
|       | 50 人 | 50 人 |

<br> 依此假定情境，我們可以先來定義 **True Positives**、**True Negatives**、**False Positives**、**False Negatives** 這四個名詞解釋。
<br>

### **TP，True Positives**   
其中第一個 True，表示分類正確，第二個 Positives 表示在此次篩檢將此檢體歸為 Positives，也就是被診出罹患流感的狀況。因此，True Positives 就是**診斷正確，且此人被診斷為有病**，簡單來說就是，<mark>病人被診出罹患流感</mark>。在本情境中共有 16 人。
<br>

### **TN，True Negatives**   
同理，第二個 Negatives 表示此次篩檢將此檢體<mark>未</mark>被診出罹患流感的狀況。因此，True Negatives 就是**診斷正確，且此人被診斷為正常**，換句話說就是，<mark>正常人沒有診出患病</mark>。在本情境中共有 36 人。
<br>

### **FP，False Positives**
其中第一個 False，表示分類錯誤。因此，False Positives 就是**診斷錯誤，且此人被診斷為有病**，也就是，<mark>正常人被誤診成罹患流感</mark>。在本情境中共有 34 人。
<br>


### **FN，False Negatives**
**診斷錯誤，且此人被診斷為正常**，即，<mark>病人但並沒有被檢出罹患流感</mark>。在本情境中共有 14 人。

<br><br>

接下來各個指標的計算，都是基於 TP、TN、FP、FN 這四種狀況進行排列組合：
<br>

## Accuracy
中文譯作<mark>準確率</mark>，其定義是：對於給定的測試資料集，分類器正確分類的樣本數與總樣本數之比。白話來說就是，<mark>模型預測正確數量所佔整體的比例</mark>。

其公式為： 

$$
Accuracy =\cfrac{TP+TN}{TP+TN+FP+FN}
$$


<br>

依照假定場景我們可計算：  

$$
Accuracy =\cfrac{16+36}{16+36+34+14} = 0.52
$$

<br>

雖然這是最常見的衡量指標，但對於<mark>資料正反例不平衡</mark>的狀況下，如：肺炎、癌症檢測...等，Accuracy 指標幾乎不具參考價值。

以罕見疾病檢測來說，有 100 個樣本，有 99 位是 Negative（未患病），只有 1 個 Positive（患病）。假設現在有一模型，針對所有樣本預測輸出全是 Negative，則此模型的 Accuracy 高達 $\frac{99}{100} = 0.99$。Accuracy 雖高，但這個模型幾乎報廢不能使用的。


<br><br>

## Precision
先來張經典圖，方便大家想像：
<center> <img src="https://i.imgur.com/1MKvczU.png" alt="Precision and recall"></center>
<center class="imgtext">Precision and recall（圖片來源: <a href="https://en.wikipedia.org/wiki/Precision_and_recall" class="imgtext">維基百科</a>）</center>
<br>

在 Accuracy 不具參考價值的狀況下，就會採用 Precision 和 Recall 兩個指標，這兩個指標都是專注被預測為 Positive 的資料，但卻又各有所好。

以 Precision 來說，中文譯名為<mark>精確率</mark>。顧名思義，這個指標更在意的是在預測為 Positive 的結果中，其精確性是多少。白話來說就是，<mark>被預測為 Positive 的資料中，有多少是真的 Positive</mark>。

其公式為：  

$$
Precision =\cfrac{TP}{TP+FP}
$$

<br>

依照假定場景我們可計算，<mark>被診斷成有病的人中，有多少位是真的病人</mark>： 

$$
Precision =\cfrac{16}{16+34} = 0.32
$$

<br>

在一般的應用中，如果更在乎的是預測出來的結果，就可以採用這個指標。看到的一個例子是<mark>門禁系統</mark>，它更在乎的是有否讓未經許可的人進去（也就是 FP 越小越好）

<br><br>

## Recall
以 Recall 則被譯為<mark>召回率</mark>。我的理解是它是原本是 Positive 的資料，它能夠召回多少，也就是說<mark>在原本 Positive 的資料中被預測出多少</mark>。

其公式為：  

$$
Recall =\cfrac{TP}{TP+FN}
$$

<br>

依照假定場景我們可計算，<mark>在病人中真的被診斷出來的比例</mark>： 

$$
Recall =\cfrac{16}{16+14} = 0.53
$$

<br>

如果你的應用場景更偏向在意是否觸及了所有的 Positive case，例如：廣告投放它更在意的是你是否觸及了所有的潛在客戶（Positive case），寧可錯殺也不放過 XDDD

<br><br>

## F1-score
不過一般來說，這兩個指標通常不可兼得。如果你的模型夠貪婪，想預測出更多的  Ground Truth 為 Positive 的例子，那們它可能會發生誤判，雖然產生較高的 Recall，但也導致 Precision 下降。反之，若是模型都保守地預測，那麼 Precision 或許會很高，但 Recall 就會相對較低。


在兩者難以取捨的情況下，衍伸出另一個指標 F1-score，它 Precision 與 Recall 調和平均數，其公式為：

$$
\cfrac{2}{F_1} = \cfrac{1}{P} + \cfrac{1}{R}
$$

<br>
調整後：

$$
F_1 = \cfrac{2PR}{P+R} = \cfrac{2TP}{2TP+FP+FN} 
$$

<br>

依照假定場景我們可計算 F1-score：

$$
F_1 = \cfrac{2*16}{2*16+34+14} = 0.4
$$

<br>

F1 中，認為 Precision 與 Recall 的權重是一樣，因此有人將其一般化，列出了這樣的公式：

$$
F_\beta = (\beta^2 + 1) * \cfrac{PR}{\beta^2*P+R}
$$

<br><br>

## ROC-AUC （Area Under Curve） 
也就是指 ROC 曲線下方的面積，在說明 AUC 前我們先看看 ROC 空間，這個空間就是以 False Positive Rate 為 X 軸，True Positive Rate 為 Y 軸，其公式定義如下：

$$
FPR = \cfrac{FP}{FP+TN}，  TPR = \cfrac{TP}{TP+FN}
$$


<br> 因此在空間中若存在一點離<mark>左上角越近</mark>的點預測準確率越高。反之，離右下角越近的點，預測越不準。

<center> <img src="https://i.imgur.com/G5CdWgG.png" alt="ROC Space"></center>
<center class="imgtext">ROC空間的4個例子（圖片來源: <a href="https://zh.m.wikipedia.org/zh-tw/ROC%E6%9B%B2%E7%BA%BF" class="imgtext">維基百科</a>）</center>
<br>

以這張圖為例，用 A、B 與 C 三不同的模型進行預測，並將結果繪製在 ROC 上，依此圖判斷最好的結果是 A 模型，反之，最糟糕的預測是 C 模型甚至劣於隨機分類。


<br> 上述 ROC 空間裡的單點，是指給定特定分類模型一個閾值後得出的預測結果。但對於同一個分類模型來說給定不同的閾值，會影響到模型的準確率，進而得到不同的 FPR 和 TPR。因此 ROC 曲線指的是，將同一模型不同閾值所得到的結果一一繪製在 ROC 空間中，所得到的曲線。

關於這曲線的好壞與空間中單點一樣，若曲線越靠近左上角表示其效能越好。除了肉眼判斷外，也可以引入 AUC （Area Under Curve，曲線下面積）做為模型優劣的指標。
 
一般來說，AUC 必在 0~1 之間，<mark>AUC 值越大的分類器，正確率越高</mark>。
- **AUC = 1**  
若線下面積為 1，代表這是一個完美分類器。當你採用這個模型時，你可以找到至少一個閾值能得出完美預測。不過絕大多數情況下完美分類器是不存在。

- **0.5 < AUC < 1**  
表示這個模型妥善設定閾值的話，能有預測價值。

- **AUC = 0.5**  
跟隨機猜測沒兩樣，模型沒有是預測價值的。

- **AUC < 0.5**  
比隨機猜測還差；但只要總是反預測而行，就優於隨機猜測。


一般使用 ROC-AUC 是為了克服資料不平衡所採用的指標，因為在繪製 ROC 空間所採用的 X 軸與 Y 軸是以實際表現來衡量。在網路上看到一個例子：在所有資料中，90 % 為 Negative（未患病）、10 % 為 Positive（患病）。在計算 FPR 時只關心 90 %  Negative 有多少是被錯誤預測，與那 10 % 毫不相關。同理，計算 TPR 時，只關心 10 % 病患是否有被正確診斷，也那 90% 毫不相關。也因為是由實際表現出發，可以避免樣本不平衡的問題。

不過雖說 ROC-AUC 可以不受資料變動或是不平衡的影響，但不平衡的資料可本身就有較高的初始值，在使用時要特別注意。


<br><br>
## PR-AUC
另外還有一個曲線，就是以 Recall 為 X 軸，Pecision 為 Y 軸，曲不同的閾值所畫的曲線。

同樣的，也是線下面積越大越好，或是說曲線越靠近<mark>右上角</mark>越好。

<br><br> 

## 參考資料 
1. [Precision and recall｜Wikipedia](https://en.wikipedia.org/wiki/Precision_and_recall)
2. [ROC曲線｜維基百科](https://zh.m.wikipedia.org/zh-tw/ROC%E6%9B%B2%E7%BA%BF)
3. [准确率(Accuracy), 精确率(Precision), 召回率(Recall)和F1-Measure｜ArgCV](https://blog.argcv.com/articles/1036.c)
5. [精确率、召回率、F1 值、ROC、AUC 各自的优缺点是什么？｜知乎](https://www.zhihu.com/question/30643044)
6. [ROC曲线和PR(Precision-Recall)曲线的联系｜SEAN是一只程序猿](http://www.fullstackdevel.com/computer-tec/data-mining-machine-learning/501.html) 
7. [Precision and recall｜Wikipedia](https://en.wikipedia.org/wiki/Precision_and_recall#F-measure)
8. [【机器学习笔记】：一文让你彻底理解准确率，精准率，召回率，真正率，假正率，ROC/AUC｜知乎](https://zhuanlan.zhihu.com/p/46714763)