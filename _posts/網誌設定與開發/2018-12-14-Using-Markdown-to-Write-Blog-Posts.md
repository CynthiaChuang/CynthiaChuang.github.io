---
title: 【Blogger】用 Markdown 寫 Blogger 文章
date: 2018-12-14
is_modified: false
categories:
- 網誌設定與開發
tags:
- Blogger
- Markdown
--- 

好久沒寫網誌了，Blogger 已經雜草叢生了（笑）
 
想當初寫網誌最討厭的一件事情就是<span class='highlighting'>排版</span>，我花在排版的時間可能比寫文章還久阿，我都直接用 html 寫了，它排版還是可以跑掉...真是的...
 
不過這一兩年接觸到了 <span class='highlighting'>[Markdown](https://markdown.tw/)</span>，它的語法可以減少不少排版的時間，所以最近又興起了把網誌撿回來的念頭，希望這次可以持之以恆阿...

<!--more-->
<br> 

## 在 Blogger 寫 Markdown
不過想把網誌撿回來好像也沒有那麼的容易？ <span class='highlighting'>因為 Blogger 不支援 Markdown！</span> 還好網路上有不少人有相同需求，感謝他們的分享，我只需要複製貼上就好 XD。

基本上，就是開個外掛，然後把下面的 script 貼上就是了，實際操作可以參考[卡卡米的記憶體](http://etrex.blogspot.com/2017/03/blogger-code-markdown-prettyprint.html)的說明。

```html
<script
 src="https://cdnjs.cloudflare.com/ajax/libs/showdown/1.6.4/showdown.min.js">
</script>  

<script>  
 var converter = new showdown.Converter();  
 var posts = document.querySelectorAll(".post-body,.snippet-item");
 Array.prototype.forEach.call(posts, function(el, i){  
  if(el.innerHTML.indexOf("markdown") <= 1){ 
   el.innerHTML = converter.makeHtml(el.innerHTML.replace("markdown",""));  
  }});  
 var pres = document.querySelectorAll("pre"); 
 Array.prototype.forEach.call(pres,  function(el, i){ 
  el.classList.add("prettyprint");  
 });  
</script>  

<script  
 src="https://cdn.rawgit.com/google/code-prettify/master/loader/run_prettify.js?skin=sunburst">
</script>
```  

但這份 code 在我這邊有點水土不服，因為我之前改過 CSS，結果出來的效果沒有人家來的好看。而且說真的，在 Blogger 中寫 Markdown 的體驗不是很好...


<br><br>

## 用 Stackedit 發布至 Blogger 
![Stackedit](https://stackedit.io/res-min/img/logo.svg)
<center class="imgtext">   Stackedit Icon （圖片來源: <a href="https://stackedit.io/" class="imgtext">Stackedit官網</a>）</center>

<br> 在 google 的時候，偶然發現有 [StackEdit](https://stackedit.io/)  工程師在 [Stack Exchange](https://webapps.stackexchange.com/questions/40737/markdown-for-blogger) 上~~自薦枕蓆~~。原本它是沒有特別吸引我，因為同為 Markdown 的文字編輯器，目前我 [HackMD](https://hackmd.io/) 使用得正順手，並沒有想換掉它的想法，而且 HackMD 還是繁中界面的哩！ <br><br>


>  Amongst others, you can **post to Blogger.**

但看到這句話，還是抱著嘗試的心態來使用看看。StackEdit 的功能大致與 HackMD 相同，不過它文章的管理方式是我比較喜歡的資料夾，而且寫完後可以直接發布到 Blogger，不需要匯出再轉貼，因此很適合將它作為 Blogger 的編輯器來使用。

雖然，我還滿喜歡 HackMD 的程式碼有支援行數的顯示，但即使沒有顯示也只是可讀行稍微差了點。若沒有要在 Blogger 貼長篇程式碼，這個缺點應該可以忽略不計。
 
關於從 StackEdit 發表的 Blogger的教學，可參考： [使用 StackEdit 發布至 Blogger ~ Open Jiang](http://jeffyon.blogspot.com/2015/05/stackedit-bloggermd.html)

<br><br>

## 定義 Markdown 各標籤的 CSS 文件
前面說過 Blogger 不支援 Markdown，即便 StackEdit 可以將文章轉成 HTML 在 Blogger 上發布，但部份標籤的 CSS 仍需要定義。<br>


### 程式碼高亮標識
StackEdit 的程式區塊預設是使用 [Highlight.js](https://highlightjs.org/) 的函式庫來支援
Highlight 的功能，所以 Blogger 這邊也需要導入相對應的函式庫。

只要在範本的 HTM L的 ``<head>`` 與 ``</head>`` 中間，添加：

```html
<!-- highlight.js Additions START -->
<link href='https://cdn.bootcss.com/highlight.js/9.13.1/styles/androidstudio.min.css' rel='stylesheet'/>
<script src='https://cdn.bootcss.com/highlight.js/9.13.1/highlight.min.js'/>
<script> hljs.initHighlightingOnLoad();</script>
<!-- highlight.js Additions END -->
``` 

<br>

### KaTeX
另外數學符號的顯示部份是使用 [KaTeX](https://khan.github.io/KaTeX/) 來渲染的，據說與 MathJax 相比，它的速度載入數度更快，但犧牲的就是它能支援的符號較少。

原本我是想載入 MathJax 就好，但 Stackedit 轉出的 CSS 標籤中，有部份 MathJax 無法識別，所以還是只能載入 KaTeX。

一樣是在範本的HTML的 ``<head>`` 與 ``</head>`` 中添加：

```html
<!-- KaTeX Additions START-->
<link href='https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.10.0/katex.min.css' rel='stylesheet'/>
<script src='https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.10.0/katex.min.js'/>
<script src='https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.10.0/contrib/auto-render.min.js'/>
<script>
  renderMathInElement(document.body);
  [{left: &quot;$&quot;, right: &quot;$&quot;, display: false}]
	});
</script>
<!-- KaTeX Additions END-->
```

<br>

### Mermaid
如果有製作流程圖的需求，則需要再導入 [Mermaid](https://mermaidjs.github.io/) 的函數庫。但它的黑色文字會被我的深色背景吃掉，導致看不清楚，所以我另外取出它的 [CSS檔](https://cdn.rawgit.com/knsv/mermaid/7.0.0/dist/mermaid.forest.css)，配合我的背景來進行修改。

```html
<!-- Mermaid Additions START-->
<!-- Mermaid 目前版號 8.0.0-rc.8-->
<script src="https://cdn.rawgit.com/knsv/mermaid/0.5.6/dist/mermaid.min.js"></script>
<link rel="stylesheet" href="https://cdn.rawgit.com/knsv/mermaid/0.5.6/dist/mermaid.css">
<script>
  mermaid.initialize({startOnLoad:true, theme: 'forest'});
</script>
<!-- Mermaid Additions END--> 
```

<br>

### 其他標籤
```html
<link href='https://miaochien.github.io/MyBlogger/template/css_template/markDown.css?raw=1' rel='stylesheet'  type='text/css'/>
```

除了上面幾個必須個別調整的標籤外，其他的標籤我本想直接套用在[這篇文章](http://map-testing.blogspot.com/2016/08/hello_23.html)找到的 CSS 文件。但還是因為深色背景的關係，必須修改他的 [CSS 的部份](https://miaochien.github.io/MyBlogger/template/css_template/markDown.css?raw=1)，避免文字被背景吃掉。

但說是修改，其實我只取出所以他的 ``table``、``blockquote`` 兩部份出來改顏色跟文字大小而已，因為我還滿滿意目前這個主體的 CSS 設定的 XD


<br><br> 

## 參考資料 
1. [在 blogger 貼漂亮 code 的方法（使用 markdown 和 prettyprint）｜卡卡米的記憶體](http://etrex.blogspot.com/2017/03/blogger-code-markdown-prettyprint.html)
2. [Markdown for Blogger｜Web Applications Stack Exchange](https://webapps.stackexchange.com/questions/40737/markdown-for-blogger)
3. [使用 Stackedit 發布至 Blogger｜Open Jiang](http://jeffyon.blogspot.com/2015/05/stackedit-bloggermd.html)
4. [使用 Stackedit 編輯 Markdown｜EAGLES VIEW 鳥瞰之眼](http://map-testing.blogspot.com/2016/08/hello_23.html)
5. [如何让你的HEXO博客支持手写流程图？｜慧行说](https://www.liuyude.com/How_to_make_your_HEXO_blog_support_handwriting_flowchart.html)
6. [The need for mermaid.css should be mentioned explicitly in the intro docs... · Issue #273 · knsv/mermaid｜Github](https://github.com/knsv/mermaid/issues/273)
7. [Mermaid Breaking changes｜GitBook](
https://mermaidjs.github.io/breakingChanges.html) 