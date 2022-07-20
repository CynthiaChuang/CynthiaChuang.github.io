---
title: CodeFree｜喝一杯咖啡，輕鬆學 HTML
date: 2022-06-28 20:16
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- 前端
- HTML
- 讀書筆記
- Hiskio
--- 

偶然註冊的一個線上課程平台，發現他們有些免費的課程可上。剛好有幾門課當初都是我自學的，現在有機會就來看看系統的學習。

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/ibijtxL.png" alt="Hiskio Codefree">
    Codefree Time（圖片來源: <a href="https://codefree.hiskio.com/">Hiskio Codefree</a>）
</p>

呃...先寫在前面好了。這門課的內容是以初階初學者為導向的，每個小節的知識點不多，能輕鬆理解。

但對於已經有程設基礎的人來說，這門課顯得太簡單，我大概花不到他建議學習時間的一半就完成，這還不算上做筆記的時間，花最久的時間可能是把它寫成網誌！？

~~（這門課的受眾本來就不是我，還硬要來湊熱鬧 XDDD）~~



## 單元 1、HTML 是什麼 ?
網頁構成有　3　大基本要素：
1. **HTML**  
   HyperText Markup Language，超文本標記語言。管理網頁的結構。
2. **CSS**  
   Cascading Style Sheets，階層式樣式表。負責視覺化需求。
3. **JavaScript**   
   通常縮寫成 JS。主管使用者互動與資料內容的處理。
   
嚴格說來，我的網誌分類有點不太精準。因為網頁三兄弟中 HTML 與 CSS 並不屬於程式語言，不過我也懶得重新換個分類了，反正還有架構這個詞勉強兜著。

<p class="illustration">
    <img src="https://i.imgur.com/mYJOoah.png" alt="網頁三兄弟">
    網頁三兄弟（圖片來源: <a href="https://codefree.hiskio.com/courses/25">Codefree - HTML</a>）
</p>

HTML 主要作用在於定義網頁內有什麼內容元素，如文字、照片、影音、超連結...等。

以建構房屋為比喻，建房時大量異種建材來建造地基、牆面，並定義房屋內部的格局與用途。HTML 就像是建材，依照不同用途會使用不同建材與工法。舉例來說，對於書寫文字的部分，就會使用段落標籤 `<p>`；要放置影片，就會使用`<video>` 標籤。

正確的標籤使用不但**方便後續的維護**外，也比較容易在搜尋引擎中取得**較好的排名**，增加網頁曝光率。但若使用了錯誤標籤，就像使用不防水材質來建造浴室，你大概三五時就得抓漏整修，耗時費力不說，還不能根治，除非打掉重建。

<p class="illustration">
    <img src="https://i.imgur.com/rIg09Bc.png" alt="網頁架構與標籤">
    網頁架構與標籤（圖片來源: <a href="https://codefree.hiskio.com/courses/25">Codefree - HTML</a>）
</p>


目前 HTML5 是 HTML 最新的修訂版本，又簡稱 H5。此次更新還滿重大，畢竟是時隔 17 年一次的更新，此次更新新增了 `<video>` 跟 `<audio>`，將影音格式從 Flash 依賴中解耦出來、 `<canvas>` 可以直接在瀏覽器中繪圖。另外移除幾個 CSS 可取代的標籤，如：`<center>` 與 `<font>`。想知道詳細的可以去看看 [W3 官網](https://www.w3.org/TR/html5-diff/)。

<p class="illustration">
    <img src="https://i.imgur.com/vJaf3xq.png" alt="HTML5 更新項目">
    HTML5 更新項目（圖片來源: <a href="https://codefree.hiskio.com/courses/25">Codefree - HTML</a>）
</p>

如果延續建房的比喻，<mark>HTML 是房屋結構，那 CSS 大概是裝潢、JavaScript 就是電器用品</mark>吧？因為 HTML 只是骨架，無法提供漂亮的視覺效果，因此需要其他兩兄弟的支援。


### 單元測驗
1. 選出下列描述正確的選項
    - [x] HTML 是超文本標記語言
    - [ ] HTML 用來決定網頁的外觀
    - [ ] HTML 標籤絕對是不成對存在

2. HTML 主要功能為何呢？
    - [x] 決定網頁的結構
    - [ ] 決定網頁的顏色
    - [ ] 決定與使用者的互動方式



## 單元 2、HTML 語法組成
HTML 是由**標籤（tags）** 與 **內容（content）** 2 個部份所組成。多數情況下標籤是<mark>成對</mark>存在的，可再細分成**開始標記**與**結束標記**：
- **開始標記**：由尖括號（<）以及 尖括號（>）包圍元素名稱。例如： `<p>`。
- **結束標記**：同開始標記但會在元素名稱前多了（/）符號。例如 `</p>`。

目前我所想到的不成對標籤有：`<br>`、`<hr>`、`<img>`。

<p class="illustration">
    <img src="https://i.imgur.com/uIQloIe.png" alt="HTML 語法組成">
    HTML 語法組成（圖片來源: <a href="https://codefree.hiskio.com/courses/25">Codefree - HTML</a>）
</p>


### 單元測驗
1. **何者不是HTML 的組成要素？**
    - [ ]  開始標籤
    - [ ]  結束標籤
    - [ ]  標籤內容物
    - [x]  中間標籤

  是說中間標籤是什麼鬼？是指子標籤嗎？



## 單元 3、修復不成對的 HTML
這邊是個小小的實作題。它給了份有誤的 HTML 標籤，要我們把它改成正確標籤：

```html
<div>咦？好像少了什麼東西<div>  
</p>咦？好像多了什麼東西</p>  
```

<p class="paragraph-spacing"></p>

訂正完後長這樣：
```html
<div>咦？好像少了什麼東西</div>  
<p>咦？好像多了什麼東西</p>  
```



## 單元 4、HTML 巢狀特性
HTML 文件結構是一種**巢狀結構（Nested Structure）**，好像又有人把它稱為階層樹狀結構。簡單來說就是一層包一層吧。彼此互為父子層關係，**外為父、內層為子**。
 
根據的巢狀特性，在實作會因應網頁設計的需求，不斷的拓展很多子層。而這些層次間的關係我們可稱為**層次結構**，不同的層次結構，會呈現不同的結果。

<p class="illustration">
    <img src="https://i.imgur.com/9mQskBh.png" alt="HTML 巢狀與階層性">
    HTML 巢狀與階層性（圖片來源: <a href="https://www.slideshare.net/leolo/html-css-1536489">slideshare</a>）
</p>


###  實作練習
1. **我是子層，正在找一個 div 父層，幫幫我。**
    ```html
    <!-- 將以下標題標籤為變成 div 的子層 -->
    <h3>我是子層，正在找一個 div 父層，幫幫我</h3>
    ```
    
    答：
    ```html
    <!-- 將以下標題標籤為變成 div 的子層 -->
    <div>
        <h3>我是子層，正在找一個 div 父層，幫幫我</h3>
    </div>
    ```
    <p class="paragraph-spacing"></p>

2. **幫 `<h3>` 新增一個子層 `<p>`，並寫下「Hello HiSKIO」。**
    ```html
    <!-- 將以下標題標籤為變成 div 的子層 -->

    <div>
        <h3>我是子層，正在找一個 div 父層，幫幫我</h3>
    </div>
    ```
    
    答：
    ```html
    <!-- 將以下標題標籤為變成 div 的子層 -->

    <div>
        <h3>我是子層，正在找一個 div 父層，幫幫我
            <p>Hello HiSKIO</p>
        </h3>
    </div>
    ```



## 單元 5、網頁基本架構  `<html>` `<head>` `<body>`
網頁的基本結構，應至少具備 `<html>`、`<head>`、`<body>` 這三個標籤元素才能正常顯示運作。一份正常的 HTML 網頁呈現的樣子如下：

<p class="illustration">
    <img src="https://i.imgur.com/kYq6AuG.png" alt="完整 HTML 架構">
    完整 HTML 架構（圖片來源: <a href="http://jinjin.mepopedia.com/~jinjin/webdesign-notes/html.html">WebDesign-網頁設計筆記</a>）
</p>


1. **`<html>...</html>`**  
    此標籤是網頁中**不可或缺**的標籤，且具有**不可重複性**。它是整個網頁**最上（外）層的標籤**，網頁中所有的標籤都被包覆其中。

2. **`<head>...</head>`**	  
    也是**必不可少**且**獨一無二**的標籤，若出現一組以上，可能會造成瀏覽器的混亂。它放置於 `<html>` 下的**第一個標籤**，主要記錄網頁相關基本設置與描述，但內容並不會對使用者顯示。

3. **`<body>...</body>`**  
    也是具備**必要性**與**唯一性**的標籤。用來呈現網頁主要內容給使用者，包括文字、圖片、表單和多媒體物件...等。
    

### 實作練習
1. **試著自己建構網頁的基本結構吧！**   

    ```html 
    <html>
        <head>
            <title>辛西亞的技能樹</title>
        </head>
        <body>
            <h1>Hello, I'm Cynthia。</h1>
        </body>
    </html>
    ```


### 單元測驗
1. **請問下列描述何者正確？**   
    - [ ] `<head>` 裡面的會被在螢幕上顯示給使用者
    - [ ] 網頁結構一定得包含 `<p>` 標籤才算完整
    - [x] Body 主要用來呈現網頁的內容，通常只有一個。
    - [ ] `<html>` 標籤是網頁最內層的標籤



## 單元 6、註解文字  
在電腦語言中，註解（comment）是重要的一個組成部分。

直接可以告訴別人你程式碼的目的，增強程式可讀性、可維護性，還能避免坑害同事（不過，最後到底是避免坑害同事還是把同事推入深淵，那就是另外的故事了 XDDD）。它會在程式碼進入瀏覽器時被忽略，因此不會顯示在網頁上。

在 HTML 中，註解符號是由 `<!--...-->` 組成，可同時用於一行或多行註解：

```html
<!-- 單行註解 -->
<!-- 註解文字只能在純 HTML 文件內裡看到，不顯示在瀏覽器的畫面上 -->

<!-- 多行註解 -->
<!--
多行文字註解
多行文字註解
多行文字註解
-->
```


### 實作練習
1. **把 `<p>` 標籤與內容變成一段註解***

    ```html
    <p>我不想給人看到，把我變成註解！</p>
    ```

    答：
    ```html
    <!-- <p>我不想給人看到，把我變成註解！</p> -->
    ```



## 單元 7、樣式標籤 `<strong>` `<em>` `<mark>`
雖說 CSS 才是美化的主力，但在純文字文件中還是會做些簡單的強調，如：粗體、斜體、刪除線跟文字突顯的功能，這幾個功能可以透過文字樣式標籤來實現。

1. **`<strong>` 粗體字**  
    ```html
    <h3><strong>變成粗體字</strong></h3>
    ```
    效果：<strong>變成粗體字</strong>
    
2. **`<em>` 斜體字** 
    ```html
    <h3><em>變成斜體字</em></h3>
    ```
    效果：<em>變成斜體字</em>
 
3. **`<mark>` 文字突顯**  
    ```html
    <mark>變成突顯字</mark>
    ```
    效果：<mark>變成突顯字</mark>
    
4. **`<del>` 刪除線**
    ```html
    <h3><del>加上刪除線</del></h3>
    ```
    效果：<del>加上刪除線</del>


<p class="paragraph-spacing"></p>

這幾個標籤在 HTML5 可被視為特殊指標。在 HTML5 比過往更強調不同標籤的**語意（Semantic Elements）**，也就是說這些樣式標籤<mark>不僅僅是視覺的記號，也有語意層次的標</mark>記，能幫助搜尋引擎，更理解網站的內容，使搜尋結果更加貼切。

在過往的案例中，這幾個樣式有不同標籤供使用：
1. **粗體字：`<b>` 與 `<strong>`**  
    > <b>bold</b>  與 <strong>strong</strong>  

    `<b>` 這個標籤出自是 bold（粗體）；而 `<strong>` 這個標籤，則是取自 strong（強調 / 重點）。
        
    可以看到兩個標籤的效果在視覺上一模一樣，但 `<b>` 這個標籤，就僅僅是代表『加粗』，純屬視覺上的意義。而 `<strong>` 則有實際上的語意，表示這是一個重要的字串，得要用粗體來提醒大家。  
        
2. **斜體字：`<i>` 與 `<em>`**
    > <i>italic</i> 與 <em>emphasized</em>  

    `<i>` 與 `<em>` 分別出自於 italic（斜體） 和 emphasized （斜體 / 強調）的字首。

    兩個標籤效果如出一轍，並沒有誰多傾斜個 1 度。跟粗體字的標籤一樣，`<i>` 就僅就代表斜體，而 `<em>` 則多了分強調的意味存在。
 
3. **刪除線：`<s>` 與 `<del>`**
    > <s>strikethrough</s> 與 <del>delete</del>  

    `<s>` 與 `<del>` 則是源於 strikethrough（刪除線） 與 delete（刪除）。

    兩者效果毫無二致，邏輯上也跟前面類似，用 `<del>` 標籤時會很明確告知瀏覽器這段文字是我要刪除掉的。

<p class="illustration">
    <img src="https://i.imgur.com/sctpqTa.png" alt="b 和 strong 樣式的分別">
    b 和 strong 樣式的分別（圖片來源: <a href="https://ithelp.ithome.com.tw/articles/10230181">iT 邦幫忙</a>）
</p>


### 實作練習
1. **將芋頭沙拉項目變成斜體字**  
    ```html
    <body>
        <div class="menu1">
            <h3>今日菜單</h3>
            <p><em>芋頭沙拉</em></p>
            <p>高麗菜</p>
            <p>貢丸湯</p>
            <p>本日特色套餐是紅燒獅子頭，來過嘗過的客人都給五顆星評價！</p>
        </div>
        <div class="menu2">
            <h3>明日菜單</h3>
            <p>芋頭牛奶</p>
            <p>空心菜</p>
            <p>酸辣湯</p>
        </div>  
    </body>    
    ```
    <p class="paragraph-spacing"></p>
2. **為芋頭牛奶項目變成粗體字**   
    ```html
    <body>
        <div class="menu1">
            <h3>今日菜單</h3>
            <p><em>芋頭沙拉</em></p>
            <p>高麗菜</p>
            <p>貢丸湯</p>
            <p>本日特色套餐是紅燒獅子頭，來過嘗過的客人都給五顆星評價！</p>
        </div>
        <div class="menu2">
            <h3>明日菜單</h3>
            <p><strong>芋頭牛奶</strong></p>
            <p>空心菜</p>
            <p>酸辣湯</p>
        </div>  
    </body>
    ```
    <p class="paragraph-spacing"></p>
3. **把貢丸湯項目加上`<mark>`**  
    ```html
    <body>
        <div class="menu1">
            <h3>今日菜單</h3>
            <p><em>芋頭沙拉</em></p>
            <p>高麗菜</p>
            <p><mark>貢丸湯</mark></p>
            <p>本日特色套餐是紅燒獅子頭，來過嘗過的客人都給五顆星評價！</p>
        </div>
        <div class="menu2">
            <h3>明日菜單</h3>
            <p><strong>芋頭牛奶</strong></p>
            <p>空心菜</p>
            <p>酸辣湯</p>
        </div>  
    </body>
    ```

 

## 單元 8、HTML 屬性 Attribute
HTML 中的標籤具有屬性，而這些屬性可以藉由各種方式去**設定元素或調整它們的行為**，以符合使用者的期待，例如：high 與 width。不過最常用的非屬 id 與 class，id 具有唯一性，能將網頁某個元素獨立辨識出來，而 class 可將這些元素進行分類，套用不同的效果。

而在這裡，我們將把重點放在添加 id 屬性協助我們將識別內容。
```html
<div id="introA" class="man">
    <h1>自我介紹-人物A</h1>
    <p>大家好，我是王小明，家住在嘉義，喜歡打籃球和講冷笑話。<p>
</div>

<div id="introB" class="man">
    <h1>自我介紹-人物B</h1>
    <p>大家好，我是林大熊，家住在高雄，喜歡打羽球和吃滷肉飯。<p>
</div>
```



### 實作練習
1. **在前端技術、後端技術的 h2 標籤加上不同的 class 屬性**  
    ```html
    <body>
        <div>
            <h2>前端技術</h2>
            <h2>Vue</h2>
            <h3>React</h3>
            <h3>Angular</h3>
        </div>
        <div>
            <h2>後端技術</h2>
            <h3>Laravel</h3>
            <h3>nodeJS</h3>
            <h3>Ruby on rails</h3>
        </div>
        <div>
            <h2>雲端服務</h2>
            <h3>AWS</h3>
            <h3>Azure</h3>
            <h3>GCP</h3>
        </div>
    </body>
    ```

    答：
    ```html
    <body>
        <div>
            <h2 class="frontend">前端技術</h2>
            <h2>Vue</h2>
            <h3>React</h3>
            <h3>Angular</h3>
        </div>
        <div>
            <h2 class="backend">後端技術</h2>
            <h3>Laravel</h3>
            <h3>nodeJS</h3>
            <h3>Ruby on rails</h3>
        </div>
        <div>
            <h2>雲端服務</h2>
            <h3>AWS</h3>
            <h3>Azure</h3>
            <h3>GCP</h3>
        </div>
    </body>
    ```

    阿不是說好把重點 id 屬性？
 


## 單元 9、容器 `<div>` - 常用的容器標籤
在做網頁時 `<div>` 是一個很常用到的標籤元素，是用來當作容器（container），用來包裹其他標籤，如：文字、連結、圖像或影音，將內文**分出不同獨立區塊** （block）。

`<div>` 的使用會便於之後的樣式設計與互動，如：在做網站切版，我們可透過移動 `<div>` ，將該區塊內的元素一併調整；或是利用 CSS 給整個區塊進行整體性樣式設定；或是 JavaScript 做互動操作。
    
```html
<body>
    <div id="number1">
        <h1>div可以做什麼？</h1>
        <p>將元素分組很好用！</p>
    </div>
</body>
```

<p class="paragraph-spacing"></p>

另外一些常用在切版的標籤有 `<div>` `<p>` 與 `<span>`。

- `<div>`：簡單來說就是一個區塊。
- `<p>`：是一個段落。
- `<span>`：通常會用來做標籤內的細部變化。

舉例來說，網頁就是本詩詞、`<div>` 則是其中一首、`<p>` 則是段落、`<span>` 則是針對段落中樣式變化。做出來大概會像是這樣：

<p class="illustration">
    <img src="https://i.imgur.com/xNu8aU3.png" alt="div、p 與 span">
    div、p 與 span
</p>


### 實作練習
1. **將 h2、h3 標籤分別裝入不同的 div 內做區隔**  
    
    ```html
    <!--將以下同樣類別的物品用div做區隔  -->
    <body>
        <h2>蘋果</h2>
        <h3>機車</h3>
        <h3>汽車</h3>
        <h2>香蕉</h2>
        <h3>火車</h3>
        <h3>飛機</h3>
        <h2>梨子</h2>
    </body>
    ```

    答：
    ```html
    <!--將以下同樣類別的物品用div做區隔  -->
    <body>
        <div id="fruit">
            <h2>蘋果</h2>
            <h2>香蕉</h2>
            <h2>梨子</h2>
        </div>

        <div id="transportation">
            <h3>機車</h3>
            <h3>汽車</h3>
            <h3>火車</h3>
            <h3>飛機</h3>
        </div>
    </body>
    ```



## 單元 10、標題（Headings) - 幫你的文章放上適合的標題  
你在看我的網誌時，應該會注意它會有顯眼的大標以及不同的級別的段落標題，方便大家迅速了解每段文字的大意。

這些標題，在 HTML 語法中稱為 Headings。它有 `<h1>–<h6>`  六種元素呈現了不同的級別的標題，`<h1>` 級別最高，而 `<h6>` 級別最低。我通常 `<h1>` 只會用一次，就是文章大標；最常用的則是 `<h2>`；至於 `<h5>` 與 `<h6>` 我幾乎不用。
 
```html
<h1>最重要標題，通常一個頁面一個</h1>
<h2>標題二</h2>
<h3>標題三</h3>
<h4>標題四</h4>
<h5>標題五</h5>
<h6>最不重要的標題</h6>
```


### 實作練習
1. **創建一個 h1 標籤，並寫下「Hello HiSKIO」**    

    ```html
    <h1>Hello HiSKIO</h1>
    ```



## 單元 11、有序列表、無序列表 `<ol>`  `<ul>`   `<li>`
除了以文字段落形式之外，另外一個常用的就是列表。列表可以分成**無序列表（Unordered List）** 與 **有序列表（Ordered List）** 兩種。

1. **無序列表（Unordered List）**：`<ul>` 和 `<li>`   
    `<ul>` 表此列表是無序的，`<li>` 則是用來列表中的每一個項目。需要注意的是 `<ul>` 標籤本身不具有任何效果，需與 `<li>` 同時使用，才能正常顯示。

    ```html
    <ul>
        <li>電腦</li>
        <li>滑鼠</li>
        <li>鍵盤</li>
    </ul>
    ```
    <p class="paragraph-spacing"></p>
2. **有序列表（Ordered List）**：`<ol>` 和 `<li>`  
    如果想要使用有序列表，只需要將 `<ul>` 換成 `<ol>` 即可。

    ```html
    <ol>
        <li>電腦</li>
        <li>滑鼠</li>
        <li>鍵盤</li>
    </ol>
    ```


### 實作練習
1. **新增一個有序列表**  
    ```html
    <ol>
        <li>本壘</li>
        <li>一壘</li>
        <li>二壘</li>
        <li>三壘</li>
    </ol>
    ```
    


## 單元 12、`<img>` - 幫網頁插入一張圖片吧
一圖勝千言，網站怎麼可以沒有圖呢！

前面提過 HTML 的標籤多為成對標籤，而 `<img>` 就是屬於其中的特例。 在 `<img>` 標籤中有兩個重要的預設屬性：
- **src**：
  必備屬性，圖片連結位址。不填是不會出錯拉，但不填了話就不會有圖片了。
- **alt**：
  非必備屬性，圖片說明。如果圖片無法正常載入，則當滑鼠停留在該圖片上，會出現此圖片說明。另外，也可提供視覺障礙用戶藉助螢幕閱讀軟體輔助上網，並更好地理解頁面圖片。

```html
<img src="圖片連結" alt="圖片說明" />
```

 
### 實作練習
1. **在 div 容器內新增一張圖片，且必須包含正確的連結位址，以及說明（alt）**  
    ```html
    <div class="container">
        <!-- 新增一張圖片-->
        <img src="https://bit.ly/3qutOFg" alt="蘋果" />
    </div>
    ```

 
### 單元測驗
1. **下列說明何者正確？**  
    - [ ] 圖片是成對的HTML標籤
    - [ ] alt 的功用在於讓圖片放大或縮小
    - [x] 正常的 img 標籤會帶有 src 屬性 以及 alt 屬性



## 單元 13、`<video>` - 插入影片
除了靜態圖片外，也可以插入動態影音。

`<video>` 標籤用法與 `<img>` 類似，都是藉由 src 屬性輸入來源、 width 和 height 來調整大小。與 `<img>`  標籤不同的是 `<video>` 是個成對標籤，這真奇怪 XDD 另外 `<video>` 標籤有個 controls 屬性可用，它可以顯示影片的控制選項介面。

```html
<video src="影片位址(url)" width="320" height="240" controls>
    Video not supported
</video>
```    

 
### 實作練習
1. **利用 HiSKIO 提供的試閱網址，新增一段帶有控制選項（controls）的 video 吧！**  
    ```html
    <div class="videobox">
        <!-- 新增一段video吧！ -->
        <video src="https://bit.ly/38nUD7Q" width="320" height="240" controls>
        Video not supported
        </video>
    </div>
    ```



## 單元 14、`<iframe>` - 在網頁上插入一段 Youtube 影片吧
插入動態影音的方法除了`<video>` 外，還有 `<iframe>` 可用，它可將影片網站的內容嵌入到自己的網站中。除了嵌入影片網站外，也可以嵌入 Facebook 的粉絲專頁或按讚分享按鈕。

不過有些網頁為了避免點擊劫攻擊，是不允許使用 `<iframe>` 嵌入其他網站的。
    
```html
<iframe src="網址"  width="頁面寬度"  height="頁面高度" >
</iframe>
```  

 
### 實作練習
1. **幫網頁嵌入一個你喜歡的 Youtube 影片 ！**  
    ```html
    <div class="iframebox">
        <!--  加入一個 iframe 吧！ -->
        <iframe width="560" height="315" src="https://www.youtube.com/embed/H0m4XiZrblU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>
    ```



## 單元 15、階段小測驗 
下面有幾題觀念測驗，試著找出解答，完成這階段的小測驗吧！

1. **選出下列對的選項**  
    - [x] `<body>` 標籤用來放置網頁的主要顯示內容
    - [ ] `<html>` 標籤是整個網頁最內層的標籤
    - [ ] `<head>` 標籤是整個網頁最內層的標籤
    - [ ] 網頁並沒有固定的基本結構，有多個 `<body>` `<head>`都是正常的狀況
    
    <p class="paragraph-spacing"></p>

2. **選出下列錯誤的選項**  
    - [ ] 使用錯誤的HTML標籤，容易導致後續網頁製作上出現多問題
    - [ ] HTML 就像網頁的骨架一樣
    - [x] HTML 絕對都是成對的存在
    - [ ] HTML5 可以用來製作網頁遊戲

    <p class="paragraph-spacing"></p>

3. **關於HTML標籤屬性的描述下列何者正確？**
    - [ ] id 與 class 屬性只能用於容器標籤
    - [x] 每種標籤會有不同的屬性可以利用
    - [ ] 標籤屬性會以問號來做屬性賦值 例如：width?30px

    <p class="paragraph-spacing"></p>

4. **選出正確的描述**
    - [ ] 有序列表會使用 `<ul>` 標籤實現
    - [x] `<ol>`要再搭配 `<li>` 才能正常呈現列表的樣貌
    - [ ] 無序列表會使用 `<ol>` 標籤實現

    <p class="paragraph-spacing"></p>
    
5. **選出錯誤的描述**
    - [ ] `<video>` 標籤需要有src 屬性以及連結才能顯示影片
    - [x] `<iframe>` 跟 `<video>` 擁有同樣的功能
    - [ ] `<iframe>` 可以將網頁在另一網頁裡面

<p class="paragraph-spacing"></p><p class="paragraph-spacing"></p>

當你的進度條跑到 100% 後，你就會收到這張圖，恭喜你完成課程啦！

<p class="illustration">
    <img src="https://i.imgur.com/WntBEVF.png" alt="恭喜您完成 HTML 課程">
    嗨！恭喜您完成 HTML 課程，掌握了新的程式技能！（圖片來源: HiSKIO 平台通知）
</p>



## 其他連結
1. 課程內容：[Codefree - HTML](https://codefree.hiskio.com/courses/25)



## 參考資料 
1. JinJinWang。[HTML筆記](http://jinjin.mepopedia.com/~jinjin/webdesign-notes/html.html)。檢自 WebDesign-網頁設計筆記 (2022-06-18)。
2. Mack Chan (2020-02-27)。[ HTML 中的 < b > 和 < strong > 到底有什麼分別？](https://ithelp.ithome.com.tw/articles/10230181)。檢自 iT 邦幫忙 (2022-06-19)。
3. [[HTML5]b,i,s 跟 strong,em,del 這些看起來一樣，但意義不同的標籤們](http://jinjin.mepopedia.com/~jinjin/webdesign-notes/homeworks-3.html)。檢自 五分鐘微閱讀 (2022-06-19)。
4. Weii9 (2016-08-17)。[span,div,p的一些小差别](https://blog.csdn.net/u012884726/article/details/52226404)。檢自 Weii9的博客｜CSDN (2022-06-19)。
5. 樂樂 (2017-12-18)。[Day04 : 常用標籤 ( 二 ) div 、span](https://ithelp.ithome.com.tw/articles/10191914?sc=iThelpR)。檢自 iT 邦幫忙 (2022-06-19)。
6. [p标签和div标签的区别与用法](https://www.html8.com.cn/html/1439.html)。檢自 会学html (2022-06-19)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-06-28</summary>
  <ul>  
    <li>2022-06-28 發布</li>
    <li>2022-06-20 完稿</li>
    <li>2022-06-17 起稿</li>
  </ul>
</details>