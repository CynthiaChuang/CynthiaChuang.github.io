---
title: 解決 Jekyll 將大括號識別成 Liquid 語言
date: 2020-12-31 21:58
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- Jekyll
- Liquid
--- 

在前一陣分享了些前端的程式碼，但發現分享的程式碼只要涉及了大括號，都渲染失敗，無一例外，而且還會從終端機收到 Liquid 的錯誤訊息。

<!--more-->
<br>

## 問題描述
在用 Markdown 寫網誌時，文本的內容只要像下面涉及大括號全都會渲染失敗，還會跳出錯誤訊息：
{% raw %}
```
{{ ... }}
```
{% endraw %}


<br><br> 

## 解決方法
有嘗試過在大括號前後加上 `\` 進行跳脫，但還是不起作用：
{% raw %}
```
\{\{ ... \}\} 
```
{% endraw %}

<br>

<div class="blockquote-center">Liquid 的問題需要用 Liquid 來解</div>

在尋找跳脫方法的過程中，看到有人說了上面這句話，我才恍然大悟，原來我試圖從 Markdown 語法中找到解法，根本是錯誤的方向。最後終於在 Liquid 中找到了  [`raw`](https://shopify.github.io/liquid/tags/raw/) 這個標籤，使用時被這個標籤縮包起來的內容，會被 Liquid 是視為普通文本來處理，而不是按照 Liquid 語法來解析：

{% raw %}
```
{\% raw \%} (移除\)
{{ ... }}
{\% endraw \%} (移除\)
```
{% endraw %}

搞定！成功渲染出來了！

<br><br> 

## 參考資料 
1. Nicolas Molina (2016-07-19)。[markdown — 在Jekyll的markdown代码块中转换双花括号](https://www.it-swarm.asia/zh/markdown/%e5%9c%a8jekyll%e7%9a%84markdown%e4%bb%a3%e7%a0%81%e5%9d%97%e4%b8%ad%e8%bd%ac%e6%8d%a2%e5%8f%8c%e8%8a%b1%e6%8b%ac%e5%8f%b7/1047233674/) 。檢自 it-swarm.asia (2020-09-18)。
2. 吕毅 (2018-08-12)。[转义，解决花括号在 Jekyll 被识别成 Liquid 代码的问题](https://blog.walterlv.com/post/jekyll/raw-in-jekyll.html) 。檢自 walterlv (2020-09-18)。

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-12-31</summary>
  <ul>
    <li>2020-12-31 發布</li>
    <li>2020-09-18 完稿</li>
    <li>2020-09-18 起稿</li>
  </ul>
</details>