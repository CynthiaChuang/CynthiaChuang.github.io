---
title: 數學中常見的 arg 和 arg max/ min 是什麼？
date: 2013-12-21
is_modified: false
categories:
- 研讀筆記
tags:
- 論文與文件
- 數學
--- 

上了研究所開始啃論文，數學式再也不能跳過去了（允悲
  
在看論文的過程中發現，arg / argmax 在數學式中反覆出現，為了能順利看下去，只好來拜大神。
<!--more-->
<br><br> 

## 常見的數學表示

1. $y = f(x)$  
    一般的函式寫法，表給定一輸入值 $x$，將該值傳入函式 $f$，會得使用 $x$ 運算後的值 $y$。
    
2. $y = max f(x)$  
    通常輸入值 $x$ 為一集合，該集合中的值個別傳入函式 $f$，最終傳回運算結果集合中的最大值。
    
    舉例來說，存在一個集合 $x={0,1,2}$，並將 $x$ 中元素分別帶入函數 $f$ 計算得：
      
    $$
    \begin{aligned}		
        f(0) = 10 \\
        f(1) = 20 \\
        f(2) = 7 \\
    \end{aligned}
    $$

    則回傳最大值 20，故 $y=20$。
    
    而 $min$ 也是類似的計算過程，但回傳的是運算結果集合中的最小值。

3. $y = arg max f(x)$  
    跟 $y = max f(x)$ 類似，但在 $max$ 中回傳的是最大值，而 $arg max$ 回傳的則是該值對應的輸入值。
    
    以上個例子為例，產生最大值 20 的輸入值為 1，因此回傳該值，故 $y=1$。

    同理 $arg min$ 會回傳最小值所對應的輸入值。


<br><br> 

## 參考資料 
1. Unknown (2009-04-21)。[arg 和 arg max](https://kevingo75.blogspot.com/2009/04/arg-arg-max.html) 。檢自 I am here. (2013-12-21)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2013-12-21</summary>
  <ul class="timestamp">
    　<li>2013-12-21 發布</li>
    　<li>2013-12-21 完稿</li>
  </ul>
</details>