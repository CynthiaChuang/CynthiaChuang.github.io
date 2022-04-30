---
title: "【技能樹栽種】複製網頁文字時，加上網站的作者與網址"
date: 2022-04-29 10:16
is_modified: false
categories:
- 網誌設定與開發
- 程式語言與架構 
tags:
- Github Page 
- Jekyll-NextT 
- Jekyll 
- 技能樹栽種
- Javascript
--- 

之前興起實作了[版權訊息的顯示](https://cynthiachuang.github.io/Add-Post-Copyright/)時，有發現有些網站除了顯示版權訊息外，還會在複製網頁內容時，自動加上版權聲明、作者與網址等資訊。當時我覺得頗有趣，決定要來研究看看如何實作。

<!--more-->
老樣子我把採坑的過程都寫了下來，如果只想知道最後的結果就直接拉到底吧。

<br>

## 剪貼簿 DOM 事件

既然行為牽扯到複製與貼上，我第一個反應就是翻翻跟剪貼簿相關的 DOM 事件類型，果然找到一些可以用事件分別是：`copy` 跟 `paste`。想了下我的應用情境，我應該是要在它複製時，加上版權訊息，畢竟執行貼上時應該已經離開這個網站了，不會再次觸發 DOM 事件。

這簡單用 EventListenter 就好： 

```
document.addEventListener('copy', function (evt) {
    // ToDo: append source hyperlink
});
```

另外, 這樣寫法也行：
```
document.body.oncopy = function () {
    setTimeout( function () {
        // ToDo: append source hyperlink
    }, 100)
}
```
        
<br><br>

## 取得複製並修改文字

### 取得文字
根據找到的資料，剪貼簿的資料儲存在 `clipboardData` 物件中，可以藉由 `getData()` 方法並傳入指定資料格式以取得數據：

	 
```
document.addEventListener('copy', function (evt) {
    let text = clipboardData.getData("text/plain");
    console.log(text);
});
```

<br>

不過，這樣直接硬幹似乎有點問題，我遲遲沒看到應該出現的 log，反而出現 Error：

<center> <img src="https://i.imgur.com/TXuMHLr.png" alt="Error"></center>
<br>

細看了下[介紹](https://www.cnblogs.com/xiaohuochai/p/5882902.html)，原來是瀏覽器的問題。文件中有提到，`clipboardData` 這個物件，在 IE 中是屬於 window 物件的屬性；但對於其他瀏覽器來說，此物件是屬於 event 的屬性。所以我嘗試改寫了剛剛程式，多了個判斷式：
```
document.addEventListener('copy', function (evt) {
    let clipdata = evt.clipboardData || window.clipboardData;
    let text = clipdata.getData("text/plain");
    console.log(text);
});
```
<br>

不過，這樣出來的值卻是 null 的，或許跟瀏覽器的版本有關？所以我又翻了些資料發現相關的寫法五花八門，有諸如：  
`event.originalEvent.clipboardData.getData('text');`、   
`window.event.clipboardData.getData('text');`  
...等寫法，不過這些寫法不是 Error 就是 null（沮喪 😔）。

<br>

最後終於找到一個可用的寫法：
```
document.addEventListener('copy', function (evt) {
    let text = navigator.clipboard.readText();
    console.log(text);
});
```
但這個只能在 Chrome 上運作，而且即便在 Chrome 上也必須先得到使用者允許授權才能讀取：
<center> <img src="https://i.imgur.com/kUxMiy7.png" alt="使用者允許授權"></center>

<br><br>

所以我換了個想法，改用了 `document.getSelection`。不同於前面的實作方法，是從剪貼簿中取值，在進行複製前有一個必備的前行動作–<mark>反白</mark>，這個方法就是將反白的文字取出。
```
document.addEventListener('copy', function (evt) {
    let text = document.getSelection().toString();
    console.log(text);
});
```

<br>

### 修改文字

搞定文字取出後，下一步就是對文字動手腳後回填。這部分就真的得仰賴剪貼簿 `clipboardData` 了，還好這個時候它沒有跟我鬧脾氣，順利回填了：

```
document.addEventListener('copy', function (evt) {
    let text = document.getSelection().toString();
    if (text) {
        text = text + "\n\n" 
        + "=========================================\n"
        + "{網站資訊}"
    }  
    let clipdata = evt.clipboardData || window.clipboardData;        
    clipdata.setData("text",text);
    evt.preventDefault();
});
```

就此，功能終於搞定了！(灑花)

<br><br>

## 參考資料 
1. 小火柴的蓝色理想 (2016-09-18)。[深入理解DOM事件类型系列第四篇——剪贴板事件](https://www.cnblogs.com/xiaohuochai/p/5882902.html)。檢自 博客园 (2021-11-23)。
2. JS教程 (2018-10-07)。[複製網頁內容，貼上之後自動加上網址的實現方法(指令碼之家特別整理)Script](https://www.itread01.com/p/1070933.html)。檢自 IT閱讀 (2021-11-23)。
3. (2018-06-28)。[js 剪下板應用clipboardData詳細解析](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/287472/)。檢自 程式前沿 (2021-11-23)。
4. GiorgosK (2018-04-14)。[window.clipboardData.getData("Text") doesnt work in chrome](https://stackoverflow.com/a/49830762)。檢自 Stack Overflow (2021-11-23)。
5. [Element: copy event](https://developer.mozilla.org/en-US/docs/Web/API/Element/copy_event)。檢自  Web APIs｜MDN (2021-11-23)。
6. pj2452 (2015-01-02)。[javascript - "Unable to get property 'getData' of undefined or null reference" in IE but not Chrome](https://stackoverflow.com/questions/27738155/unable-to-get-property-getdata-of-undefined-or-null-reference-in-ie-but-not)。檢自 Stack Overflow (2021-12-02)。
7.  MGA (2017-01-16)。[javascript - Uncaught TypeError: Cannot read property 'getData' of undefined](https://stackoverflow.com/questions/41680895/uncaught-typeerror-cannot-read-property-getdata-of-undefined)。檢自 Stack Overflow (2021-12-02)。
8. MaxLeeBK (2021-09-25)。[那些被忽略但很好用的 Web API / Clipboard](https://ithelp.ithome.com.tw/articles/10271977?sc=iThomeR)。檢自 iT 邦幫忙 (2021-12-02)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-04-29</summary>
  <ul>
    <li>2022-04-29 發布</li>
    <li>2021-12-02 完稿</li>
    <li>2021-11-23 起稿</li>
  </ul>
</details>