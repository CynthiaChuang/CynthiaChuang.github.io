---
title: 【Network】Greedy forwarding together 與 Carry-and-forward
date: 2013-06-26
categories: 
- Network
tags:
- VANETs
- Greedy forwarding together
- Carry-and-forward
--- 

看這篇論文時 [Intersection-based Routing For Urban Vehicular Communications With Traffic-light Considerations](https://ieeexplore.ieee.org/document/6155880)，對文中出現的兩個詞 <span class='highlighting'>Greedy forwarding together</span> 與 <span class='highlighting'>Carry-and-forward</span> 感到一點疑惑。

<!--more-->
<br><br> 

## 疑問
在 Intersection-based Routing For Urban Vehicular Communications With Traffic-light Considerations 一文中所述

<div class="alert info"> 
<b>Greedy forwarding together</b> with <b>carry-and-forward</b> is regarded as a promising solution to conquer the problem of frequent disconnection for packet forwarding in VANETs.
tatic/hanlp.properties <br>
</div>


想知道何為 Greedy forwarding together 與 Carry-and-forward ？

<br><br>

### Greedy Forward Routing  
根據儲楓教授的[說明](https://www.ugc.edu.hk/minisite/rgc_newsletter/rgcnews18/big5/05.htm)，Greedy forwarding routing 是一個簡單的局部地理路由方法，當每一個節點收到一個資料包時，它檢查在其通訊半徑內有沒有其他節點更接近於該資料包的終點。

如果沒有，就將該資料包丟棄；否則，就將該資料包傳輸給在其通訊半徑內最接近該資料包終點的節點。

<br><br>

### Carry and forward
當A、B 其中一方有資料要送往對方，卻發現無法直接傳送時，會先把要送給對方的訊息攜帶在自己身上，繼續往前進。當其進到對方的通訊範圍時，再將此訊息傳送給對方。

不過，要達到以上目的，前提是A、B 兩車事先能透過 GPS 等方式取得對方的位置相關資訊。

<br><br> 

## 參考資料 
1. Jin-jia Chang, Yi-hua Li, Wanjiun Liao, Lng-chau Chang (2012/01), [Intersection-based Routing For Urban Vehicular Communications With Traffic-light Considerations](https://ieeexplore.ieee.org/document/6155880), IEEE Wireless Communications, pp82-pp88
2. [隨機幾何圖形及應用｜香港城市大學](https://www.ugc.edu.hk/minisite/rgc_newsletter/rgcnews18/big5/05.htm)
3. 杜建男（2007）。An efficient data dissemination model for VANETs（碩士論文）。國立中央大學，桃園縣。