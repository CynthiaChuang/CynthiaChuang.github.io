---
title: "【技能樹栽種】新增版權訊息"
date: 2022-04-30 00:07 
is_modified: true
categories:
- 網誌設定與開發
- 程式語言與架構
tags:
- Github Page
- Jekyll-NextT
- Jekyll
- CSS
- 技能樹栽種
--- 

在閱讀別人網誌中時看到[創用 CC 授權條款（Creative Commons license）](https://zh.wikipedia.org/zh-tw/%E7%9F%A5%E8%AF%86%E5%85%B1%E4%BA%AB%E8%AE%B8%E5%8F%AF%E5%8D%8F%E8%AE%AE)的宣告，讓我看了有點心動，我決定也來加一個！！ 開始前，先從[授權條款](https://zh.wikipedia.org/zh-tw/%E7%9F%A5%E8%AF%86%E5%85%B1%E4%BA%AB%E8%AE%B8%E5%8F%AF%E5%8D%8F%E8%AE%AE#%E4%B8%83%E4%B8%AA%E5%B8%B8%E8%A7%84%E8%AE%B8%E5%8F%AF%E8%AF%81%E7%9A%84%E4%BD%BF%E7%94%A8)中的選出所要使用授權方式，我這邊挑選的是**姓名標示-非商業性-相同方式分享（BY-NC-SA）**：

<!--more-->

<center> <img src="https://i.imgur.com/0x8qdEq.png" alt="姓名標示-非商業性-相同方式分享"></center>
<center  class="imgtext">姓名標示-非商業性-相同方式分享（BY-NC-SA）（圖片來源: <a href="https://zh.wikipedia.org/zh-tw/%E7%9F%A5%E8%AF%86%E5%85%B1%E4%BA%AB%E8%AE%B8%E5%8F%AF%E5%8D%8F%E8%AE%AE#%E4%B8%83%E4%B8%AA%E5%B8%B8%E8%A7%84%E8%AE%B8%E5%8F%AF%E8%AF%81%E7%9A%84%E4%BD%BF%E7%94%A8"  class="imgtext">維基百科</a>）</center>

<br><br>


## 設計思考

首先先決定放置的位置與期待的外觀：

1. **位置**  
    位置的部份倒是很快就決定，因為這並不屬於本文的一部分，而我又不想將它放在開頭喧賓奪主，所以就決定將它放置在 footer 裡。

2. **外觀**  
    我希望這段宣告可以被突顯出來。如果單是用粗體或是 Highlight 似乎又不夠顯眼，所以決定用<mark>方框</mark>或是<mark>色塊</mark>來突顯文字，但方框似乎與我內文的風格似乎不相同，所以就決定用色塊了！
    
    是有考慮過複用之前實做的 [Alert](https://cynthiachuang.github.io/Accent-the-Text-by-CSS-Alert-and-Highlighting/)，但相較之下它風格有點偏華麗，我是說圓角之類、Icon之類的效果，所以被我 Pass。但單純的色塊我又不愛，所以就決定用 blockquote 來實做了！
    
    題外話，總覺的我的 Alert 的配色有點飽滿，找個時間來把改成最近很紅的莫蘭迪色系好了。

<br><br>


## 實做

呃...前面雖然稍微勾勒下設計，但開始實作時我才發現 Jekyll-NextT，原本就有提供 copyright 的 UI。雖然 Jekyll-NextT 所提供的 UI 雖然與我勾勒的實作方式不同，但呈現的風格卻有點類似 blockquote。

<center> <img src="https://i.imgur.com/Ih0GOQk.png" alt="Jekyll-NextT 的 blockquote"></center>


<br>

### 開啟版權訊息功能    
不過細節上我沒有很喜歡這個版本，所以我決定順著原本的實做架構重新實做一次。但如果不介意的話，只要完成這個步驟，就可以直接啟用 Jekyll-NextT 所提供 UI。


1. **設定 config.yml**  
    先找到 `/_config.yml` 中的 `post_copyright`，先將 `enable` 改成 `true`，並將 `license` 填入所選擇的授權方式：
    ```yaml
    post_copyright:
      enable: true
      license: CC BY-NC-SA 4.0
      license_url: https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en
    ```
    
    <br>
    
2. **多語系設定**  
    預設文字會被放在 `_data/languages/default.yml` 中，不過實際顯示文字通常會根據你在 `/_config.yml` 中所設定的 `language` 而有所不同。
    
    舉例來說，若你所設定的語系為 `zh-tw`，則顯示的會以 `_data/languages/zh-tw.yml` 為主，若在該文件找不到才會去找 `default.yml`。
    
    因此，請根據你所設定的語系，修改相對應的文件，修改項目如下：
    ```yaml
    copyright:
      author: 本文作者
      link: 本文連結
      license_title: 版權聲明
      license_content: '除非另有標注，部落格中所有文章，均採用
        <a href="%s" rel="external nofollow" target="_blank">%s</a> 許可協議。
        轉載請標明作者、連結與出處！'
    ```        
 <br>

### 修改外觀       

1. **移動版權訊息位置**   
    這算是我自己的小執著！？我始終不認為它應該被放在本文當中，所以我找到 `_includes/_macro/post.html` 中原本`post-copyright.html` 的位置，把它移動到 footer 裡面：

    刪除：  
    {% raw %}
    ```html
    <div>
      {% unless is_index %}
        {% include _macro/post-copyright.html %}
      {% endunless %}
    </div>
    ```
    {% endraw %}

    新增：  
    {% raw %}
    ```html
    <footer class="post-footer">
      {% unless is_index %}
        {% include _macro/post-copyright.html %}
      ...
      {% endunless %}
      ...
    </footer>
    ```
    {% endraw %}
     
    <br>
    
2.  **修改 HTML**   
    我不喜歡原本的 style，所以我完整複寫 `include _macro/post-copyright.html` 中所有的程式碼：
    
    {% raw %}
    ```html
    {% if site.post_copyright.enable %}
      <blockquote class="post-copyright">
        <p> 
          <b>{{ __.post.copyright.author | append: __.symbol.colon }}</b>&ensp;
          {{site.author}} <br>
          <b>{{ __.post.copyright.link | append: __.symbol.colon }}</b>&ensp;
          <a href="{{ post.url | absolute_url }}" title="{{ post.title }}">{{ post.url | absolute_url }}</a><br>

          <b>{{ __.post.copyright.license_title | append: __.symbol.colon }} </b>&ensp;
          {{ __.post.copyright.license_content | replace_first: '%s', site.post_copyright.license_url | replace_first: '%s', site.post_copyright.license }}
        </p>
      </blockquote>
    {% endif %}
    ```
    {% endraw %}
       
    <br>
    
3.  **修改 CSS**  
    根據原本的的實做架構，CSS 的設定分別散落在 `_sass/_common/components/post/post-copyright.scss` 與 `_sass/_variables/base.scss` 兩隻程式中。

    其中 `base.scss` 主要是變數設定：
    ```sass
    // Posts Expand
    // --------------------------------------------------
    $posts-expand-title-font-weight : $font-weight-normal;
    $post-copyright : (
      margin: 30px 0 0,
      border-left-color:red,
      bg: white,
    );
    ```
    
    而 `post-copyright.scss` 則是套用變數：

    ```sass
    @mixin post-copyright {
      .post-copyright {
        margin: map-get($post-copyright, margin);
        border-left-color: map-get($post-copyright, border-left-color);
        background-color: map-get($post-copyright, bg);
      }
    }  
    ```
    
    完成基礎設定後，為了配合網站整體配色，我在 `_sass/_variables/custom.scss` 中重新配了色：

    ```sass
    $post-copyright : (
        border-left-color: $green-light,
        bg: $greendark-lighter ,    
        color: $greendark-ligh ,
    );
    ```
<br> 

最後出來的結果長這樣：
<center> <img src="https://i.imgur.com/AQNyyJ9.png" alt="自製 Copyright block"></center>

<br> 

### 增加版權訊息開關選項  

因為我在聲明寫到：「除非另有標注...」，既然寫說另有標注代表可能會有不加訊息的狀況，因此決定加個開關可以決定是否放置版權訊息：

1. **修改 HTLM 檔**  
    先回到 `_includes/_macro/post.html` 中找到剛剛引入 `post-copyright.html` 的位置，並在 include 外加上條件式。
    
    在這邊我設定了一個名為 `copyright` 的 flag，且僅希望在 `copyright` 設定為 false 時才會隱藏該區域；若未設定或設定為 ture 時，則正常顯示該區，因此我必須明確寫出它的條件式：
    {% raw %}
    ```html
    + {% if page.copyright != false %}
        {% include _macro/post-copyright.html %}   
    + {% endif %}
    ``` 
    {% endraw %}
    
    <br>
    
2. **ymal 設定**  
    搞定後，當你某篇文章不放置版權訊息時，就在文章上方的 yaml 區，設定的 `copyright` 的 flag：
    
    ```yaml
    ---
    title: "【技能樹栽種】新增版權訊息"
    date: 2021-09-02
    + copyright: false
    ---
    ```


<br><br>

## 下一步

發現有些網站除了顯示版權訊息外，還會在你複製網頁內容時，自動加上版權聲明、作者與網址等資訊。下一步如果有空的話，我應該會來研究這個該怎實現，等搞定了再來發網誌嘿～！ 

<div class="alert info"> 
🤟 喔耶～我完成了 → <a href="https://cynthiachuang.github.io/Copy-Text-to-Clipboard-and-Append-Source-Hyperlink">【技能樹栽種】複製網頁文字時，加上網站的作者與網址</a>
</div>


<br><br> 

## 參考資料 
1. 協同撰寫。[創用CC授權條款](https://zh.wikipedia.org/wiki/%E7%9F%A5%E8%AF%86%E5%85%B1%E4%BA%AB%E8%AE%B8%E5%8F%AF%E5%8D%8F%E8%AE%AE)。檢自 維基百科 (2021-09-06)。
2. Riemann (2019-08-01）。[新增文章底部版權訊息](https://riemann.blog/posts/2aaa1d55/)。檢自 Riemann's Blog (2021-09-09)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2022-04-30</summary>
  <ul class="timestamp">
    　<li>2022-04-30 更新：複製網頁文字時，加上網站的作者與網址</li>
    　<li>2021-09-13 發布</li>
    　<li>2021-09-13 完稿</li>
    　<li>2021-09-02 起稿</li>
  </ul>
</details>
