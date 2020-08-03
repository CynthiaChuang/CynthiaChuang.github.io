---
title: 【Coding Style】常用註解標籤 - TODO、FixMe、XXX
date: 2019-04-15
modified: 2019-04-15
categories:
- Computer-Language/Framework
tags:
- Coding Style
--- 

這篇有點難以分類阿，最後把它放到了 Coding Style 的分類，但總覺得不太對@@
 
這篇主要紀錄一下，我在寫 code 時常用的註解標籤，這個習慣是以前用 Eclipse 時留下的 - **TODO、FixMe、XXX**，不過最近才發現似乎不同語言，對於這些標籤也有不同的解釋？

<!--more-->
<br>

就我自己的解釋
 1. **TODO：**  
 表示待實現的功能。

 2. **FixMe：**  
 有問題或者不能運作的程式碼，需要修正。
 
 3. **XXX：**  
 雖然功能已經實作，但實作方法有待商榷，希望能進行改進。

<br><br>

不過我在[其他的地方](http://www.cnblogs.com/syfwhu/p/4814435.html)看到其他幾個不同的標籤與解釋：
1. **TODO 與 FixMe：**  
這兩個我比較沒有疑惑，就是代辦事項與 bug fix 的意思。

2. **XXX：**  
這個標籤就跟我所知道完全不一樣了，它在這個體系下是類似於 **HotFix** 的標籤，優先序高於 **FixMe**。 

3. **HACK：**  
有點不是很理解文件中的解釋 - *表明代碼實現走了一個捷徑* @@???
就我從[維基百科的理解](https://en.wikipedia.org/wiki/Kludge#In_computer_science)，HACK 指的是一種問題的解法，但這方法可能有點醜甚至有點莫名其妙，但卻是有用的時候，所以在我認知中這個標籤是對應到我的 **XXX**。

 4. **Review：**  
這個標籤則是說明任何改動都需要經過評審。

所以在使用註解標籤時，可能需要與你的協同者確定相關的註解機制，或者 follow 該語言的註解規則？ 

<br><br>

## 參考資料
1. [代码中特殊的注释技术——TODO、FIXME和XXX的用处](https://blog.csdn.net/mantis_1984/article/details/42550395)
2. [代码中特殊注释——TODO、FIXME、XXX、HACK｜编程之路](https://blog.csdn.net/diehuang3426/article/details/79725745)
3. [JavaScript编码风格指南(中文版)｜默语 - 博客园](https://www.cnblogs.com/syfwhu/p/4814435.html)
