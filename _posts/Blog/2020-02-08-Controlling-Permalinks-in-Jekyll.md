---
title: "【技能樹栽種】修改 Jekyll 生成的靜態網址"
date: 2020-02-08
categories:
- Blog
tags:
- Github Page
- Jekyll
- 技能樹栽種
--- 

那天想要修改某篇文章的分類。結果發現一旦改了，所產生的網址也跟著變了，導致我有參考到這篇文章的全部都要修改參考...Orz
 
所以在猶豫完改不改分類之後，我決定搞個大的：直接<span class='highlighting'>移除網址中的類別</span>，還順便把<span class='highlighting'>時間也移了</span>！

<!--more-->
<br><br>  

## 如何修改網址：Permalink
在 Jekyll 中，靜態網址的格式制定是由 <span class='highlighting'>Permalink</span> 這個參數所控制，可以直接在 `_config.yml` 檔案中修改，也可以在每篇文章的 `yaml metadata block` 中配置。

我這邊為了一勞永逸所以是採用修改 `_config.yml` 檔案的方式。

<br><br> 

## Permalink 的參數
若按照我先前的網址格式，在 config 中會是這樣的呈現：

```yaml
permalink: pretty
```
或是
```yaml
permalink: /:categories/:year/:month/:day/:title/
```

<br> permalink 的模板格式中，是用 `:關鍵字` 的格式引用標記的動態內容。在先前的網址格式中，所使用到的關鍵詞所代表的意義如下：

|關鍵詞|意義|
|---|---|
|categories| 對應到文章 yaml 中 categories。如果一篇文章含有多個類別，這些類別會被用斜線串接，如：`/category1/category2`|
|year| 對應文章 yaml 中 date 的年份。格式為 4 碼，如：`2020`|
|month| 對應文章 yaml 中 date 的月份。格式為 2 碼，如：`02`|
|day| 對應文章 yaml 中 date 的日期。格式為 2 碼，如：`08`|

至於[其他的關鍵字](https://jekyllrb.com/docs/permalinks/#placeholders)，因為舊網址沒用，而新網址也不需要，所以這邊我就不列出來了。

<br> 應該有注意到 `pretty` 並不再上述列出的關鍵詞中，而且它前面也沒有用冒號做前綴。

因為 pretty 是屬於 Jekyll <span class='highlighting'>內建模板變數</span>，它會指到事前定義好的靜態網址，在這邊 `pretty` 與 `/:categories/:year/:month/:day/:title/` 兩個是等價的。雖然...我不覺得這串網址到底哪裡漂亮了？

<br> 按一開始的規畫，我打算移掉類別與時間，所以最後網址就只剩下：
```yaml
permalink: /:title/
```

<br> 好了，到這邊就修改完全部文章的網址了。剩下的工作就是把之前引用的舊網址全部改掉了!

<br><br> 

## 參考資料 
1. Permalinks。檢自 [Jekyll • Simple, blog-aware, static sites](https://jekyllrb.com/docs/permalinks/) (2020-02-07)。