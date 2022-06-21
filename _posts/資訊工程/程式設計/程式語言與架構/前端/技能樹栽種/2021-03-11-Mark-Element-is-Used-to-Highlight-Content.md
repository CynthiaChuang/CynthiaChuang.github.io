---
title: "【種樹】HTML Mark Tag 實作 Highlighting"
date: 2021-09-16 15:11
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- Github Page
- Jekyll-NextT
- CSS
- 前端
- 技能樹栽種
--- 

之前為了凸顯文字內容，我自製了 [Highlighting](/Accent-the-Text-by-CSS-Alert-and-Highlighting#Highlighting) 這個 class，但在使用上有點小小的麻煩 - 要 key 的文字有點太長了 XDDD。

<!--more-->


## Highlighting
先來回顧下之前的實做方式。所要做的設定有二，分別是設定**螢光筆顏色**與 **CSS 本體**：

1. **螢光筆顏色**   
    螢光筆顏色會是在 `\_sass\_variables\custom.scss` 中設定。
    
    ```sass
    // highlighting  colors
    $highlighting-default              : #e6fcf2;
    ```

2. **CSS 本體**  
    CSS 本體則是在 `\_sass\_custom\custom.scss` 中設定 `class` 選擇器。

    ```sass
    .post-body .highlighting {
      padding: 0 2px;
      white-space: pre-line;
      border-radius: 2px;
      background-color: $highlighting-default;

      &.warning {
        background-color: $highlighting-warning;
      }
    }
    ```
<br>

完成設定後，在文句中需畫螢光筆的地方，插入下列程式碼，就可以看到使用的效果了。

```html
拜託，請幫我畫<span class="highlighting">重點</span>。
拜託，請幫我畫<span class="highlighting warning">重點</span>。
```

出來的效果會像是這樣：
<p class="illustration">
    <img src="https://i.imgur.com/ZaF6hhZ.png">
</p>



## Mark
但後來發現，其實不用著麼麻煩，在 HTML 用中有個 [`<mark>`](https://www.w3schools.com/tags/tag_mark.asp) 的標籤 (Tag)，而且它實做出來的效果似乎更好，至少它要打的字更少 XDDD

<p class="illustration">
    <img src="https://i.imgur.com/nRHhI77.png">
</p>

```html
拜託，請幫我畫<mark>重點</mark>。
```

<br>

### 顏色
唯一讓我不太滿意的是就是它的顏色，預設的顏色真的很像螢光黃。所以我要做的第一件事，就是先把顏色換掉！

根據上次的經驗，這次我只準備了三支螢光筆，因為預設的顏色與綠色實在太相近了，其他顏色也稍做了調整。
```sass
$mark-default: #d8ebb7;
$mark-warning: #f5deb3;
$mark-danger: #ffb6c1;
```

<br>

### 外觀
好了，搞定顏色後，我另外一件想做的事 - 就是換定螢光筆的外觀。在使用螢光筆的時候，比起粗的那端，我更喜歡使用細的那邊，所以決定來做版細的螢光筆，並將它設為預設的樣式。

<p class="illustration">
    <img src="https://i.imgur.com/anDnWNi.png" alt="螢光筆粗細">
    螢光筆粗細（圖片來源: <a href="https://2ru.co/product/%E6%9C%AC%E8%89%B2%E5%8E%9F%E5%93%81%E4%BA%94%E8%89%B2%E9%9B%99%E9%A0%AD%E7%B2%97%E7%B4%B0%E8%9E%A2%E5%85%89%E7%AD%86%E7%B5%84-1%E5%85%A5/">鉺曘文具</a>）
</p>

#### 粗螢光筆
在做細螢光筆之前，先來做做簡單的粗螢光筆，只要修改 `background-color` 顏色即可。不過因為打算預設樣式是細的，所以我這邊多加了個 `class` 選擇器：

```sass
.post-body mark {
  &.thick {
    padding: 0px;
    border-radius: 0px; 
    background: none;
    background-color: $mark-default;

    &.warning {
      background-color: $mark-warning;
    }

    &.danger {
      background-color: $mark-danger;
    }
  }
}
```

<br>

#### 細螢光筆
而我偏好的細螢光筆的製作則稍微麻煩，必須透過將 `background` 設置成[漸層色](https://wcc723.github.io/css/2013/09/24/css-background/)、並將 `background-color` 設為透明。

漸層色的設置會將上半設置為透明、讓顏色集中在下半。可以透過調整各層的百分比，調整螢光筆的粗細：

<p class="illustration">
    <img src="https://i.imgur.com/sgafaxg.png?1" alt="漸層色">
</p>

```sass
linear-gradient(transparent 40%, rgba(255,255,255,0) 50%, $mark-default  75%, $mark-default 90%, transparent 95%);
```

<br>

完整程式碼如下：
```sass
.post-body mark {
  background-color: transparent;
  padding: 2px 1px;
  border-radius: 5px; 
  color: $text-color ;  
  background: linear-gradient(transparent 40%, rgba(255,255,255,0) 50%, $mark-default  75%, $mark-default 90%, transparent 95%);

  &.warning {
    background: linear-gradient(transparent 40%, rgba(255,255,255,0) 50%,  $mark-warning  75%, $mark-warning 90%, transparent 95%);
  }

  &.danger {
    background: linear-gradient(transparent 40%, rgba(255,255,255,0) 50%,  $mark-danger  75%, $mark-danger 90%, transparent 95%);
  }    
```


#### 簽字筆
以前在做筆記時，除了畫螢光筆外，另外一種作記號的方式就是<mark>劃線</mark>。想說既然要做了，就順便把功能給補上。

與前兩種樣式螢光筆的實做方式不同，簽字筆則是使用了 `border-bottom` 來實做：

```sass
.post-body mark {
  &.under {    
    padding: 0px;  
    border-radius: 0px; 
    background: none;
    background-color: none;
    border-bottom: 3px solid $mark-default; 

    &.warning {
      border-bottom: 3px solid $mark-warning;
    }

    &.danger {
      border-bottom: 3px solid $mark-danger;
    }
  }
}
```


### 使用
在文句中需畫螢光筆的地方，插入下列程式碼，就可以看到相對應的效果了。

#### 細螢光筆
<p class="illustration">
    <img src="https://i.imgur.com/OvvX9zp.png">
</p>

```html
拜託，請幫我畫<mark>重點</mark>。
拜託，請幫我畫<mark class="warning">重點</mark>。
拜託，請幫我畫<mark class="danger">重點</mark>。
```


#### 粗螢光筆
<p class="illustration">
    <img src="https://i.imgur.com/l2Dqlmf.png">
</p>

```html
拜託，請幫我畫<mark class="thick">重點</mark>。
拜託，請幫我畫<mark class="thick warning">重點</mark>。
拜託，請幫我畫<mark class="thick danger">重點</mark>。
```
 

#### 簽字筆
<p class="illustration">
    <img src="https://i.imgur.com/GM3SN8M.png">
</p>


```html
拜託，請幫我畫<mark class="under">重點</mark>。
拜託，請幫我畫<mark class="under warning">重點</mark>。
拜託，請幫我畫<mark class="under danger">重點</mark>。
```



## 參考資料 
1. [HTML mark Tag](https://www.w3schools.com/tags/tag_mark.asp)。檢自 W3Schools (2021-03-10)。
2. 騏騏 (2017-04-01)。[CSS筆記：在網頁上實作螢光筆、簽字筆等重點畫記效果](https://chibaby1231.pixnet.net/blog/post/47152216-css%E7%AD%86%E8%A8%98%EF%BC%9A%E5%9C%A8%E7%B6%B2%E9%A0%81%E4%B8%8A%E5%AF%A6%E4%BD%9C%E8%9E%A2%E5%85%89%E7%AD%86%E3%80%81%E7%B0%BD%E5%AD%97%E7%AD%86%E7%AD%89%E9%87%8D%E9%BB%9E)。檢自 chi0*: 騏格子 (2021-03-10)。
3. 卡斯伯 (2013-09-24)。[CSS沒有極限 - CSS3的漸層](https://wcc723.github.io/css/2013/09/24/css-background/)。檢自 卡斯伯 Blog - 前端，沒有極限 (2021-03-10)。
4. MDN contributors (2020-11-16)。[linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient())。檢自 MDN (2021-03-10)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-16</summary>
  <ul>
    <li>2021-09-16 發布</li>
    <li>2021-03-11 完稿</li>
    <li>2021-03-10 起稿</li>
  </ul>
</details>


