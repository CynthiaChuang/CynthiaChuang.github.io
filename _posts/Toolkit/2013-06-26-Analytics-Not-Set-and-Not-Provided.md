---
title: GA 關鍵字中的 not set 與 not provided 是啥意思？
date: 2013-06-26
modified: 2013-06-26
categories:
- Toolkit
tags:
- 網站分析
- Google Analytics
--- 

如果打開 Google Analytics 應該會有人注意到，<span class='highlighting'>關鍵字</span>，就是了解訪客們是藉由輸入那些字而進到網站的關鍵字，表單上佔據前兩名的會是 <span class='highlighting'>not set</span> 與 <span class='highlighting'>not provided</span>，也許有人已經知道成因了，也許有人還是一頭霧水，恩...我就是一頭霧水的那位，不然也不會有這篇了 XDDDD
  
但無論了解與否，都不妨礙我們知道一件事 — 它已經影響我們的判讀，或許我們可以從其他的指標來做推測，但總歸沒有直接判讀來的簡單。

<!--more-->
<br><br> 

回歸正題，這兩筆資料 **not set** 與 **not provided** 到底是如何形成的呢？

## not set
not set 這個選項在搜尋總覽的關鍵字中占據了榜首（至少我的幾筆資料中都是此），它出現的情況也非常的多樣化，但總歸一句就是在訪問的過程中並<span class='highlighting'>沒有使用到任何的關鍵字</span>，也因此無法提供任何關鍵字資料了。

可能造成此情況的原因有以下幾種:
1.直接拜訪：也就是直接輸入網址，當然就沒有關鍵字資料
2.連結拜訪：就是經由網頁媒介進來的，例如使用超連結進入的也是屬於 not set 的範疇

另外，若是將網頁另存或是加入到我的最愛，再經此拜訪，也會被歸到 not set 的項目中。

<br><br>

## not provided
若說前述是 Google Analytcis 的無心之過，那麼 **not provided** 的出現就是 Google <span class='highlighting'>有意為之</span>的了。

為什麼這麼說？
在解釋之前，我們先看看 Google Analytcis 是如何獲得用戶搜尋關鍵詞的訊息的。

當你在 Google 中，搜尋某個關鍵詞，例如「Google Analytcis」，這個詞和其他很多的訊息會在 URL 參數中以明文的形式，出現在 URL 中。當你點擊了某個搜尋結果，打開網站後，如果此網站中正好有埋 GA，那麼就會紀錄這個使用者搜尋的關鍵詞，並呈現在報告中。

但，一旦登錄了 Google，再進行搜尋的時候，Google 的 URL 變成了 <span class='highlighting'>https</span>，這是經過了加密的，一旦加密，那麼 Google Analytics 就不在紀錄關鍵詞訊息。

理論上，Google 可以讓 Google Analytics 解密然後記錄這些詞，實際上也是可行的，因為聽說 Google AdWords 的用戶仍然可以看到詳細的關鍵字數據。


<br><br> 

## 參考資料 
1. [Google Analytcis的(not set)與(not provided)是什麼？｜跟著工作熊玩賺部落格](http://www.blogfuntw.com/2012/11/analytics-not-set-provided/) 
2. [GA SEO报告中的Not Provided和Not Set｜互联网分析在中国——从基础到前沿](http://www.chinawebanalytics.cn/ga-not-provided-and-not-set/) 