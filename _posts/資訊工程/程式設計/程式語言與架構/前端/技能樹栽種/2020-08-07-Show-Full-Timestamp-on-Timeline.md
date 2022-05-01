---
title: 【種樹】在時間軸上顯示完整日期
date: 2020-08-10
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- 技能樹栽種
- Github Page
- Jekyll-NextT
- Jekyll
- Liquid
- 前端
--- 

之前在 [Jekyll-NextT](https://github.com/Simpleyyt/jekyll-theme-next) 上的做的小改進。
  
起因是今年網誌 ~~相較之下~~ 算是**高產**，導致我在翻閱時間軸時，偶而會搞不清楚這篇網誌到底是那一年的，所以興起了幫它加上完整日期的念頭。說做就做！:hammer_and_wrench:

<!--more-->
<br><br> 

只是改改時間格式而已還頗快...至少比寫這篇網誌快 XD，開始前先來看看原本的時間軸這樣：

<center> <img src="https://imgur.com/lSxk5nt.png"></center>

<br><br>

## 在時間軸上顯示完整日期
實做過程可以細分成兩個步驟：

### Step1、更改時間格式

時間軸上的每個物件是定義在 `_includes/_macro/post-collapse.html` 中，找到一個 `post.date`，而後面的 pipleline 所接的就是所顯示的時間格式，把它換成你需要的格式就好。

原本程式碼
{% raw %}
```html
<time class="post-time" itemprop="dateCreated"
      datetime="{{ post.date | date_to_xmlschema }}"
      content="{{ post.date | date: site.date_format }}" >
        {{ post.date | date: '%m-%d' }}
</time>
```
{% endraw %}
換成
{% raw %}
```html
<time class="post-time" itemprop="dateCreated"
      datetime="{{ post.date | date_to_xmlschema }}"
      content="{{ post.date | date: site.date_format }}" >
      {{ post.date | date: '%Y.%m.%d' }}
</time>
```
{% endraw %}
<br>

### Step2、更改字體大小

不過原本顯示的數字從 4 碼變成了 8 碼實在有點擠，思考了一下，決定改字體大小好了，不調整 UI 了。


這邊字體大小是定義在 `_sass/_common/components/post/post-collapse.scss` 的 ` .post-title` 中，我將原本的文字大小由 90px 降到 60 px：


```html
.post-title {
    margin-left: 60px;   
    font-size: 16px;
    font-weight: normal;
    line-height: inherit
    ...
```

改完結果如下
<center> <img src="https://imgur.com/Du36sTn.png" ></center>

<br><br> 

## 參考資料 
<div class="alert info"> 
<div class="head">參考資料</div>
此次修改請參考此 <a href="https://github.com/CynthiaChuang/CynthiaChuang.github.io/commit/5cd5dc871594a5d7a43b971f1bb2e9b6cf889356">commit</a>
</div>

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-08-10</summary>
  <ul>
    <li>2020-08-10 發布</li>
    <li>2020-08-07 完稿</li>
    <li>2020-07-22 起稿</li>
  </ul>
</details>