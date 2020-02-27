---
title: 在 HTML 中插入 Emoji 
date: 2020-02-27
categories:
- Blog
tags:
- HTML
--- 

做個筆記 :memo:
  
因為自製的[Alert 與 Highlighting](/Accent_the_Text_by_CSS_Alert_and_Highlighting/)，沒辦法用 Shortcodes 插入，只好另找解法了。

<!--more-->
<br><br> 

## 使用圖片插入
看了一下使用 Shortcodes 所編譯出來的 HTML 程式，發現最終是變成了圖片，像這樣：

```html
<img alt=":mask:" 
     class="emoji"
     src="https://cdnjs.cloudflare.com/ajax/libs/emojify.js/1.1.0/images/basic/mask.png" 
     title=":mask:">
```

<br> 出來的效果長這樣：  
<br> <img alt=":mask:" class="emoji" src="https://cdnjs.cloudflare.com/ajax/libs/emojify.js/1.1.0/images/basic/mask.png" title=":mask:">  

<br>這種方式出來的圖片較固定，不像其他的方式有可能隨瀏覽器變化。

<br><br> 

## 使用 Unicode
記得 Emoji 本來就是種 Unicode，直接輸入使用應該可以才對~賓果 🎉
 
```html
&#x1F637;

或

<span style="font-size:100px">&#x1F637;</span>
```

<br> 出來的效果長這樣：  
&#x1F637; <span style="font-size:100px">&#x1F637;</span>

<br>好處是可以使用 style 調整 Emoji 大小，又不像圖片會失真，但這種方式回隨作業系統與瀏覽器而變化 XDDD

<br>


**備註一下 Unicode 的插入**  
查到的 Unicode 應該會長 `U+<十六進制數值>` 這樣，但在 HTML 中使用要把它轉換成 <span class="highlighting">HTML Encode</span>。

HTML Encode 它會用 `&` 開始 `;` 結束，然後使用 `#x` 表明是十六進制，因此完整 Unicode 使用 HTML Encode 編碼會變成 `&#x<十六進制數值>;`。當然也可以用十進制來表示，例如：`&#x1F637;` 十進制就成了 `&#128567;`，出來的效果都一樣。


<br><br> 

## Copy and Paste
最後一招大絕招就是<span class="highlighting">複製貼上</span>啦，直接去查表後複製貼上過來就是了 XD


<br><br> 

## 參考資料 
1. [📙 Emojipedia — 😃 Home of Emoji Meanings 💁👌🎍😍](https://emojipedia.org/)。檢自 Emojipedia (2020-02-27)。 
2. [Full Emoji List, v13.0](https://unicode.org/emoji/charts/full-emoji-list.html#1f618) 。檢自 Emoji Charts (2020-02-27)。
3. [HTML Smiley Emoji](https://www.w3schools.com/charsets/ref_emoji_smileys.asp) 。檢自 w3schools (2020-02-27)。


