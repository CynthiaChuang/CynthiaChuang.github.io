---
title: 【技能樹栽種】實作 CSS 凸顯文字內容： Alert 與 Highlighting
date: 2020-02-09
is_modified: false
categories:
- Blog
- Computer-Language/Framework
tags:
- Github Page
- Jekyll-NextT
- CSS
- 技能樹栽種
--- 

我目前 Github Page 採用的主題是 [jekyll-theme-next](https://github.com/Simpleyyt/jekyll-theme-next) ，我很喜歡這個主題。但唯一讓我不滿意的大概就是沒有凸顯內容文字的手段，只好自食其力了！

<!--more-->
<br><br>  

## Alert
先上張結果圖，實作了六種顏色，分別為 default、primary、info、success、warning、danger。
  
<center> <img src="https://imgur.com/7TPANGR.png" alt="Blog Alert"></center>
<br>

個人倒是很少用  default 與 primary。另外，Alert 左方的 icon 與上方的header，可以由撰寫者自己決定開或不開。
<br>

### 實作
在實作時我參考了 kanboo.github.io 的[程式碼](https://github.com/kanboo/kanboo.github.io/blob/4292e79031c3f8005add53df25e519f9fd4355cf/themes/next/source/css/_common/components/tags/note.styl)，雖然網站外觀看起來很相似，但它是用的是另外一套網誌框架 - [hexo](https://hexo.io/zh-tw/)，所以那份程式碼不能直接移植 So sad :crying_cat_face: 
<br>

不過看了一下沒有很難做，所以仿照這份程式做了一份：
#### 1. 顏色
在開始前，先來設定外觀顏色。在 [jekyll-theme-next](https://github.com/Simpleyyt/jekyll-theme-next) 中客製化的顏色設定會統一放在 `\_sass\_variables\custom.scss` 中。

```sass
// Default
$alert-default-border        : $grey-dim;
$alert-default-bg            : $gainsboro;
$alert-default-text          : $alert-default-border;
$alert-default-icon          : "\f0a9";
```

其中 icon 是使用 [Font Awesome](https://fontawesome.com/) 所提供的。可以到網站中找找自己喜歡的 icon，然後複製它的 Unicode，就可以套用了。

<div class="alert warning">
如果不是使用跟我一樣的主題，在開始前請在檢查是否有引入 <a href="https://fontawesome.com/">Font Awesome</a>。
</div>


#### 2. 本體
接下來決定外觀長怎樣，這部分是寫在 `\_sass\_custom\custom.scss` 中。下面只介紹部分程式碼，完整的程式碼會貼在本節之後。

1. **.post-body .alert**  
    為了限定是在文章中才會出現，所以我這邊用了[多重選擇器](https://pjchender.blogspot.com/2015/03/cssmultiple-selectorsspace.html)來限縮選取。

2. **border、border-left、border-radius**  
    這區塊是為了畫出 Alert。因此先將它初始化後，指定約 4px 的實心線條畫出的左邊的邊框，最後再加上圓角柔和一下線條。

3. **background-color、border-left-color、color**  
    這邊則是指定了本體的底色、左邊框顏色與文字的顏色。不過不過，這邊只是預設的顏色，一旦指定新的選擇器，這邊就會被覆蓋掉。
    
4. **&:not(.no-icon)**  
    這個區塊則是設定 icon 的部分。開頭的`&:`是 sass 語法，對應到的是 css 的[偽類別](https://developer.mozilla.org/en-US/docs/Web/CSS/:not)的語法。當它判斷沒有使用 `no-icon` 這個選擇器時，就會在前面加上 icon ，設定相對並設定相對應位置關係。一樣 content 與 color 會隨著不同的選擇器而變動。
    
    <div class="alert">
    <div class="head">別忘了 font-family！</div>
    就是這行 <b>font-family: 'FontAwesome';</b> 害我找了好久的 bug.
    </div>


5. **&.primary**  
    這邊就是剛剛提過會隨著不同選擇器而邊動的區塊。


```sass
.post-body .alert {
  position:             relative;
  padding:              15px;
  margin-bottom:        20px;

  // commit setting
  border:             initial;
  border-left:        4px solid; 
  border-radius: 4px;
  
  // default seeting
  background-color:   $alert-default-bg;
  border-left-color:  $alert-default-border;
  color:  $alert-default-text;

  a {
    color: $alert-default-text;
    border-bottom-color: $alert-default-text;
    &:hover {
      color: $alert-default-hover  ; 
      border-bottom-color: $alert-default-hover;
    } 
  }
  
  h2, h3, h4, h5, h6 {
      if note_icons {
        margin-top:       3px;
      } else {
        margin-top:       0;
      }
      margin-bottom:      0;
      border-bottom:      initial;
      padding-top:        0 !important;
    }
  
  p, ul, ol, table, pre, blockquote {
      &:first-child {
          margin-top:       0;
      }
      &:last-child {
          margin-bottom:    0;
      }
  }

  .head {
    font-weight:bold;
  }

  &:not(.no-icon) {
      padding-left:     45px;
      &:before {
        // common setting
        position:       absolute;
        font-family:    'FontAwesome';
        font-size:      larger;
        top:            13px;
        left:           15px;

        // default setting
        content: $alert-default-icon;
        color : $alert-default-text;
      }
    }

  &.primary  {
      background-color: $alert-primary-bg;
      border-left-color:  $alert-primary-border;
      color:  $alert-primary-text;
      a {
        color: $alert-primary-text;        
        border-bottom-color: $alert-primary-text;
        &:hover {
          color: $alert-primary-hover   ; 
          border-bottom-color: $alert-primary-hover ;
        } 
      }

      &:not(.no-icon) {
          &:before {
              content: $alert-primary-icon;
              color : $alert-primary-text;
          }
      }
  }
}
```

<br>

### 使用
在文章中需要的地方，插入下列程式碼，就可以看到相對應的效果了。

<div class="alert">
<div class="head">Default With Icon</div>
這是 default 底色、有 icon 、有 header 的版本
</div>

```html
<div class="alert">
<div class="head">Default With Icon</div>
這是 default 底色、有 icon 、有 header 的版本
</div>
```
<br>

<div class="alert primary no-icon">
<div class="head">Primary Without Icon</div>
這是 primary 底色、沒有 icon 、有 header 的版本
</div>

```html
<div class="alert primary no-icon">
<div class="head">Primary Without Icon</div>
這是 primary 底色、沒有 icon 、有 header 的版本
</div>
```


<br><br>  

## Highlighting
另一種凸顯文字的方式就是<span class="highlighting">用螢光筆</span>畫重點啦。

<center> <img src="https://i.imgur.com/ZaF6hhZ.png" alt="Highlighting"></center>
<br>

雖然我準備了四隻螢光筆，但其實我只用過一隻 XDDD 而且這樣一列才發現，一跟二的顏色太相近了。算了，之後要用其他顏色的時候再來調整。
<br>


### 實作
相比 Alert，Highlighting 的實作就簡單多了。

#### 1. 顏色
一樣先到 `\_sass\_variables\custom.scss` 中，設定螢光筆顏色。

```sass
// highlighting  colors
$highlighting-default              : #e6fcf2;
```
<br>

#### 2. 本體
再回到 `\_sass\_custom\custom.scss` 中設定本體。 如果不算 warning 選擇器的部分，程式碼只有四行而已。

其中需要注意的是`white-space`，我之前把它的屬性設成了 `nowrap`，導致當 highlight 較長文句時，該句不會換行，史的文句超出頁面([#5](https://github.com/CynthiaChuang/CynthiaChuang.github.io/issues/5))。

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

### 使用
在文句中需畫螢光筆的地方，插入下列程式碼，就可以看到相對應的效果了。

拜託，請幫我畫<span class="highlighting">重點</span>。

```html
拜託，請幫我畫<span class="highlighting">重點</span>。
```
<br>

拜託，請幫我畫<span class="highlighting warning">重點</span>。

```html
拜託，請幫我畫<span class="highlighting warning">重點</span>。
```

<br>


<div class="alert info">
<div class="head">HTML \<mark\> Tag</div>
後來發現用 HTML \<mark\> Tag 來改效果會更好，所以又做了另外一篇：  <br>
-【技能樹栽種】HTML \<mark\> Tag 實做 Highlighting<br>
P.S. 網誌完成後再來補連結 <br> 
</div>

 
<br><br> 

## 參考資料 
1. [themes note.styl](https://github.com/kanboo/kanboo.github.io/blob/4292e79031c3f8005add53df25e519f9fd4355cf/themes/next/source/css/_common/components/tags/note.styl)。檢自 kanboo/kanboo.github.io (2019-12-16)。
2. PJCHENder (2016-09-07)。[[技術分享] CSS中的多重選擇器（Multiple Selectors）包含空白或逗號](https://pjchender.blogspot.com/2015/03/cssmultiple-selectorsspace.html)。檢自 PJCHENder 那些沒告訴你的小細節 (2020-02-09)。
3. MDN contributors (2019-12-10)。[border-left](https://developer.mozilla.org/en-US/docs/Web/CSS/border-left) 。檢自 MDN web docs (2020-02-09)。 
4. MDN contributors (2019-12-20)。[:not()](https://developer.mozilla.org/en-US/docs/Web/CSS/:not)。檢自 MDN web docs (2020-02-09)。
5. [CSS white-space 属性](https://www.w3school.com.cn/cssref/pr_text_white-space.asp)。檢自 W3school (2020-02-09)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期： 2020-08-14</summary>
  <ul class="timestamp">
    　<li>2020-08-14 更新 新增超連結顏色</li>
    　<li>2020-02-09 發布</li>
    　<li>2020-08-13 完稿</li>
  </ul>
</details>