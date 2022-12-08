---
title: 【種樹】顯示文章最後修改時間
date: 2020-09-07 00:07
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

那天幫文章做了點更新，卻想到文章沒辦法顯示最後修改時間，因此來研究一下該如何處理。
  
這整個計畫嚴格來說在最後一步失敗了，但還是記錄下...畢竟我都搞這久了，不~~騙一篇網誌~~留下一點紀錄，我實在不甘心  QAQ（附上[傳送門](#首頁)給想先看看失敗地方的人）。但也不算完全失敗，最後用了點 tricy 的作法，也算是符合要求？

<!--more-->


## 顯示修改時間
好了，就按部就班、一步一步來吧，先來試試如何顯示修改時間。

雖然在 [Jekyll-NextT 的使用文件](http://theme-next.simpleyyt.com/)中，沒有看到關於顯示最後修改時間的說明，不過我在程式碼中發現了些端倪。

1. 在 `_config.yml` 的 `post_meta` 中有個 `updated_at` 字段，將這個參數打開（即設為 `true`）。  

    ```yaml
    # Post meta display settings
    post_meta:
      item_text: true
      created_at: true
      updated_at: true
      categories: true
    ```

2. 並在每篇文章的 yaml 區域中加上 `updated` 與時間：  

    ```
    ---
    title: 文章標題
    date: 2013-12-24
    updated: 2020-07-28
    categories:
    - Foo
    tags:
    - Foo
    - Bar
    - Baz
    --- 

    // 以下開始撰寫你的文章
    ```

<br class="big">

出來的效果如下圖。但是我不是很喜歡這效果，原因有二：  
1.  **陳列的資訊太多的**  
    這有違我選這個主題的初衷，當初選擇這個主題很大的原因是因為簡潔，為了更簡潔我還把原先資訊列上的說明文字全拿掉了。現在變這麼長，實在讓我感到很阿雜 :cry: 
    
2.  **排序問題**  
    另一個原因是，當來到首頁與其他出現時間軸頁面時，會發現它們的排序還是照著發布時間來，與我原先預期不同，我期望最後修改的文章會在最前面的說。

<p class="illustration">
    <img src="https://i.imgur.com/mOzmMf5.png" alt="顯示修改時間">
</p>
 


## 時間擇一顯示
雖然沒有符合預期的原生結果，但還好身為一個工程師，我可以自己動手做，~~至於失敗就是另外一回事了~~。

巡視了下網站，發現時間戳會出現在網站的三處：<mark>文章本身</mark>、<mark>首頁</mark> 以及 <mark>時間軸</mark> 三處。不過實際翻看程式碼，會注意到文章與首頁的時間戳其實共用一個 HTML 的，所以需要修改的地方只有兩處。

<div class="alert warning">  
<div class="head">我更改了 yaml 區域中最後修改時間的字段</div>
相比 <code class="language-plaintext highlighter-rouge">updated</code> ，我更喜歡用 <code class="language-plaintext highlighter-rouge">modified</code> 這個單字。所以在我修改後的程式碼當中，都使用了 <code class="language-plaintext highlighter-rouge">post.modified</code> 取代了 <code class="language-plaintext highlighter-rouge">post.updated</code>。若不想隨我更改，請將下列中的 <code class="language-plaintext highlighter-rouge">modified</code> 取代掉。
</div>


### 限制條件
先提提一個限制條件。

在每篇文章的 yaml 區中一定要同時存在 `date` 與 `modified` 兩個字段，即便文章沒有更新過，也必須在 `modified` 字段中填入發布的時間（這也是我喜歡用 `modified` 取代 `updated` 的原因）。否則下面程式碼在執行的時候，會因為 `modified`  為空值，導致時間顯示空白；或依照 `modified` 排序時，因為空值，而導致順序掉到最後面去。

為了解決這問題，我有試著[找過並詢問過](https://stackoverflow.com/questions/63149510/using-liquid-to-sort-posts-by-the-latest-datelatestposted-date-updated-date) Liquid 中是否可以像 python 一樣能自定義 [sort 功能](https://shopify.github.io/liquid/filters/sort/)，讓未修改過的文章不必填寫兩個相同時間，但很不幸的 Liquid 沒有支援這個功能。

所以...還是乖乖填兩個字段吧 :smiley: 


### 文章 & 首頁
回到顯示時間的部分，我們來看看如何修改顯示結果。

在文章與首頁這兩個部分時間戳是共用 `_includes/_macro/post.html` 這份程式碼的，而時間戳則是定義在 `<span class="post-time">...</span>` 的區塊中。


觀察下原先的程式碼會發現，顯示發布時間與最後修改時間的結構非常的相像，兩者相異之處也只有相對應的參數不同。按這概念先試著做了一次變數抽取，可以將原先顯示發布時間程式碼改寫成：

{% raw %}
```html
{% assign icon = "fa fa-calendar-o"  %}
{% assign title_hint = __.post.created  %}
{% assign itemprop_hint = "dateCreated datePublished" %}              
{% assign timestamp = post.date  %}

<span class="post-meta-item-icon">
  <i class="{{ icon }}"></i>
</span>
<time title="{{ title_hint }}" 
      itemprop="{{ itemprop_hint }}"
      datetime="{{ timestamp | date_to_xmlschema }}">
  {{ timestamp | date: site.date_format }}
</time>
```
{% endraw %}

<br class="big">

確認要提取的變數後，就可以開始進行修改：
1. **移除原始程式碼**  
    這邊顯示的邏輯與原本的程式碼不同，因此方便起見我把之前原先顯示兩個時間，也就是 `<span class="post-time">...</span>` 的部分全部移掉。
    
2. **確認顯示邏輯**  
    兩個變數互相搭配的情況下，會有 4 種可能：
    
    | created_at | updated_at | 顯示邏輯                                                             |
    | ---------- | ---------- | ------------------------------------------------------------------- |
    | true       | false      | 顯示發布時間                                                       |
    | true       | true       | 這個比較麻煩，必須再進一步判斷，如果發布時間與修改時間一致，則顯示發布時間；若相異，則顯示修改時間。|
    | false      | true       | ...雖然我覺得應該不會有人這樣設，不過這狀況應該是顯示修改時間？ |
    | false      | false      | 整個區塊不顯示。                                                    |

 
3. **程式撰寫**  
    按照上面的真值表邏輯，就可以開始撰寫了。
    1. **format_modified**  
        考慮到如果 updated_at 為 `false` 的情況， yaml 中可能不會存在該字段，所以先判斷它是否存在，再來套用 Filter。
    
    2. **show**  
        另外用了一個名為 `show` 的變數來當 flag。一來是因為想盡可能遵守<mark>程式碼不重複</mark>的原則，二來是因為 Liquid 的流程控制真的超難寫，有些 Ruby 的語法我在這邊都找不到。:cry:
        
修改後的程式碼如下：

{% raw %}
```html
{% if site.post_meta.created_at or site.post_meta.updated_at %}
  {% assign format_date = post.date | date: site.date_format%}          
  {% assign format_modified = post.modified %}
  {% if format_modified %}
    {% assign format_modified = format_modified | date: site.date_format %}          
  {% endif %}

  {% assign show = "published" %}
  {% if site.post_meta.created_at %}
    {% if site.post_meta.updated_at and format_modified != format_date%}
      {% assign show = "modified" %}    
    {% endif %}
  {% else %} 
    {% assign show = "modified" %}    
  {% endif %}

  {% if show == "published" %}
    {% assign icon = "fa fa-calendar-o"  %}
    {% assign title_hint = __.post.created  %}
    {% assign itemprop_hint = "dateCreated datePublished"  %}              
    {% assign timestamp = post.date  %}
  {% else %} 
    {% assign icon = "fa fa-calendar-check-o"  %}
    {% assign title_hint = __.post.modified  %}
    {% assign itemprop_hint = "dateModified"  %}              
    {% assign timestamp =  post.modified %}        
  {% endif %}

  <span class="post-meta-item-icon">
    <i class="{{ icon }}"></i>
  </span>
  <time title="{{ title_hint }}" 
        itemprop="{{ itemprop_hint }}"
        datetime="{{ timestamp | date_to_xmlschema }}">
    {{ timestamp | date: site.date_format }}
  </time>

{% endif %}
```
{% endraw %}



### 時間軸
時間軸這邊改起來算快，因為之前為了[在時間軸上顯示完整日期](/Show-Full-Timestamp-on-Timeline)，其實已經改過一次了。

一樣到 `_includes/_macro/post-collapse.html` 中，修改時間軸上的 Item 物件。可以看到程式碼中原本是讀取 `post.date` 顯示時間，這邊加個流程控制來決定讀取的變數。

{% raw %}
```html
{% if site.post_meta.updated_at != true%}
  {% assign timestamp = post.date  %}
  {% assign itemprop_hint = "dateCreated"  %}
{% else %}
  {% assign timestamp = post.modified %}
  {% assign itemprop_hint = "dateModified"  %}
{% endif %}

<time class="post-time" itemprop="{{ itemprop_hint }}"
      datetime="{{ timestamp | date_to_xmlschema }}"
      content="{{ timestamp | date: site.date_format }}" >
      {{ timestamp | date: '%Y.%m.%d' }}
</time>  
```
{% endraw %}

另外在時間軸上也藏了一個小小需要改的地方，就是在 archives 頁面上那個年份的分隔。

這邊是定義在 `_includes/archive.html` 中，還滿好找的，上方剛好有一個註解寫 <mark>Show year</mark>。一樣幫那行加上個流程控制：

{% raw %}
```html
{% comment %} Show year {% endcomment %}
{% if site.post_meta.updated_at != true%}
  {% assign timestamp = post.date  %}
{% else %}
  {% assign timestamp = post.modified %}
{% endif %}

{% assign post_year = timestamp | date: '%Y' %}
``` 
{% endraw %}



## 按修改時間排序
搞定時間顯示後，我還希望文章可以按照最後修改的日期來排序，不然我辛苦改完了不就沒人知道嗎？

而與排序有關的有<mark>時間軸</mark>以及<mark>首頁</mark>的部分。


### 時間軸
剛剛頁面都停在了時間軸附近了，所以排序我就從這裡改起了！

跟前面不太一樣，前面顯示時間是時間軸中的 Item 實作的。這邊則是牽扯所有文章的排序，必須在 Item 外就完成，因此會在出現時間軸的頁面，分別是 Categories、 Tags、 Archives ，中各自實作。但三者個改法其實都是相同的。

分別在 `_includes/category.html`、`_includes/archive.html` 與 `_includes/tag.html` 中找到 for 迴圈的位置，並將原先傳入的變數，先用另一個變數暫存，再判斷是否需要按修改時間排序，最後再將結果傳入迴圈中：

{% raw %}
```html
{% assign sorted_list = site.posts %}
{% if site.post_meta.updated_at  %}   
{% assign sorted_list = sorted_list | sort:"modified" | reverse %}        
{% endif %}
  
{% for post in sorted_list %}
```
{% endraw %}



### 首頁
最後是首頁的部分，這邊也是我唯一改不動的部分。也不能說改不動，如果你沒有啟動分頁器功能，其實是可以順利完成，但一旦啟動分頁器就 GG 了。

打開 `_includes/index.html` ，會看到原先兩行程式碼分別對應兩種狀況：有無使用<mark>分頁器</mark>。原本不打算動這邊的邏輯，直接在最後套上判斷式就好，結果如下：

{% raw %}
```html
{% assign posts = site.posts %}
{% if site.paginate > 0 %}
  {% assign posts = paginator.posts %}
{% endif %}

{% if site.post_meta.updated_at %}
  {% assign posts = posts | sort:"modified" | reverse  %}
{% endif %}
```
{% endraw %}

但，越看越不對勁。在分頁器啟動的情況下，它其實是先從分頁器取出了一頁假設 10 篇的文章，而我排序僅僅是這 10 篇文章，而不是對著全部文章排序阿...。這樣子，如果是修改某篇很舊的文中，它還是不會被排到頂端阿！

只好用關鍵字 [jekyll-paginate](https://jekyllrb.com/docs/pagination/) sort by modified date 之類的，下去找找有無相關資訊。但並沒有看到任何有用的建議，倒是發現了這個 [Pull request](https://github.com/jekyll/jekyll-paginate/pull/31/files#)。這個 request 的目的是想要按照更新日期來排序，但很明顯地它還沒有被合併阿阿阿阿...。



## tricy 的作法
因為搞不定在開起分頁器後，全部文章的排序問題，這個計畫原本該宣告失敗的。但我實在不甘心，超想要那個打勾的日曆 icon 阿阿阿阿！

我想到既然它只能照 `date` 這個字段來排，那我就讓它照這個來排吧！但我直接來告訴它何時要顯示 modified 的 icon。 基於這個想法，我把上面寫的程式碼全部 rollback 回去，砍掉重練。

所以現在文章的 yaml 變成了這樣，一旦文章有修改，則更改時間，並把 `is_modified` 設置為 `true`：  

```
---
title: 文章標題
date: 2020-09-01
is_modified: true
categories:
- Foo
tags:
- Foo
- Bar
- Baz
--- 

// 以下開始撰寫你的文章
```

接下來去改 `_includes/_macro/post.html`，一樣動 `<span class="post-time">...</span>` 中間的程式碼，程式碼跟[之前的章節](#文章--首頁)類似，唯一的區別就是我把 `timestamp` 變數抽掉了，全部都只讀 `date` 這個字段，如此我就不參與排序的處理了。

{% raw %}
```html
{% if site.post_meta.created_at or site.post_meta.updated_at %}
  {% assign show = "published" %}
  {% if site.post_meta.created_at %}
    {% if site.post_meta.updated_at and post.is_modified %}
      {% assign show = "modified" %}    
    {% endif %}
  {% else %} 
    {% assign show = "modified" %}    
  {% endif %}

  {% if show == "published" %}
    {% assign icon = "fa fa-calendar-o"  %}
    {% assign title_hint = __.post.created  %}
    {% assign itemprop_hint = "dateCreated datePublished"  %}           
  {% else %} 
    {% assign icon = "fa fa-calendar-check-o"  %}
    {% assign title_hint = __.post.modified  %}
    {% assign itemprop_hint = "dateModified"  %}  
  {% endif %}

  <span class="post-meta-item-icon">
    <i class="{{ icon }}"></i>
  </span>
  {% if site.post_meta.item_text %}
    <span class="post-meta-item-text">{{ __.post.posted }}</span>
  {% endif %}
  <time title="{{ title_hint }}" 
        itemprop="{{ itemprop_hint }}" 
        datetime="{{ post.date | date_to_xmlschema }}">
    {{ post.date | date: site.date_format }}
  </time>
{% endif %}    
```         
{% endraw %}

是說，別忘了 `_config.yml` 中，該開的還是要開。



## 後記
說實話，這個功能開發時程拉了好長一陣子，原本我都已經把它部署到 github page 上了，結果在寫網誌的時候發現有  bug，只好先把功能給 rollback 掉，爾後來回嘗試了些方法，最後只先這樣了。雖然 yaml 的可讀性看起來怪怪的，不過 UI 上顯示一切正常，也算是也成功？

不過說真的，寫網誌真的會促進思考，為了順利寫完網誌，還重新理了理程式邏輯，把原本懶得處理的例外狀況都補上，還順便發了自己好幾條 issue ...



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-09-07</summary>
  <ul>
    <li>2020-09-07 發布</li>
    <li>2020-09-01 完稿</li>
    <li>2020-07-22 起稿</li>
  </ul>
</details>
