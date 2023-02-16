---
title: CSS 漸層背景
date: 2023-02-15 16:46:00
is_modified: false
image: https://i.imgur.com/ehVOu8l.png
categories:
- "程式設計 › 程式語言與架構"
tags:
- CSS/SCSS
- 前端
--- 

我還以為我這篇已經寫完了，結果一切都是我幻想 XDDD
  
這主要是我當初用在用 [Mark Tag 實作細螢光筆](/Mark-Element-is-Used-to-Highlight-Content/)的技術，這邊稍微整理一下相關資料。

<!--more-->
<p class="illustration">
	<img src="https://i.imgur.com/ehVOu8l.png" alt="Basic gradients">
	Basic gradients（範例來源: <a href="https://easylogic.medium.com/gradient-tool-conic-gradient-e9dd198921cc">easylogic｜Medium</a>）
</p>



## Gradient
漸層基本上就是一個顏色的漸變過程。既然是漸變，那至少需具備**兩種顏色**，或者說<mark>需要兩種以上的顏色</mark>，並從其中挑出兩種顏色分別設置為是起始色與終點色。

設定完成後，瀏覽器會計算出中間漸變的過程，進而產生一種流動感。除了顏色設定外，還可設定漸變角度、方向與尺寸...等，形成不同的流動感。

不過漸層這東西，如果顏色挑選的好的話，這顏色會非常漂亮，但...如果挑選的不好那會是場災難 XDDD
 
在 CSS 裡有 6 種漸層，分別是：
1. linear-gradient：線性漸層
2. radial-gradient：徑向漸層
3. conic-gradient：錐形漸層
4. repeating-linear-gradient：線性重複漸層	 
5. repeating-radial-gradient：放射重複漸層
6. repeating-conic-gradient：重複錐形漸層

 
## Linear Gradient 線性漸層
線性漸層大概是大家第一印象會浮出的樣式。線性漸層的特色就是沿著指定角度上放置數個顏色，並以直線的方式依照此角度逐漸漸變：

<p class="illustration">
    <img src="https://i.imgur.com/HEvHg0w.png" alt="紫色和黃色之間呈 45 度角的線性漸變。">
    紫色和黃色之間呈 45 度角的線性漸變。（圖片來源: <a href="https://www.quackit.com/css/functions/css_linear-gradient_function.cfm">Quackit</a>）
</p>


```css
background:linear-gradient(direction, color-stop1, color-stop2, ...);  
```

1. **direction**  
	漸變的方向或角度，如果沒有設定，預設會是由上至下進行漸層；若是設定角度，則是以左下角為圓心，與正 y 軸的夾角作為角度的來設定。這細節我們等等來實驗會比較清楚。
2. **color-stop**  
	它會依照依方向使用 color 1 → color 2 → ... 依序漸變過去。在 color 後方會僅跟著定位參數，將每個顏色放置在漸層線上的特定位置。若沒有指定定位參數，則預設為將第一個顏色放置在 0%、最後一個顏色放置在 100%，其餘顏色則會自動等比例分配。其中 0% 表示起始邊界，100% 表示結束邊界。


### 用關鍵字設置漸變方向
接下來我們就漸變方向的設置方式的不同來實驗。

若想用關鍵字設置漸變方向，這邊有八組關鍵字可用，剛好對應八個方位：**上、下、左、右、上左、下左、上右、下右**，分別為 **to top、to bottom、to left、to right、to top left、to bottom left、to top right、to bottom right**。需特別注意的是 <mark>to</mark> 是必須的，別省略了。

這邊設置柔紫色 `#c5d5fa` 為起始色、柔綠色 `#c3dc99` 為終點色，並依序設置漸層方向為上、下、左、右，其效果如下：

```css
background: linear-gradient(to top, #c5d5fa, #c3dc99);
background: linear-gradient(to bottom, #c5d5fa, #c3dc99);
background: linear-gradient(to left, #c5d5fa, #c3dc99);
background: linear-gradient(to right, #c5d5fa, #c3dc99);;
```

<p class="illustration">
    <img src="https://i.imgur.com/KRyoNCw.png" alt="漸層方向依序為上、下、左、右">
    漸層方向依序為上、下、左、右
</p>


若依序設置漸層方向為上左、下左、上右、下右，其效果如下：

```css
background: linear-gradient(to top left, #c5d5fa, #c3dc99);
background: linear-gradient(to bottom left, #c5d5fa, #c3dc99);
background: linear-gradient(to top right, #c5d5fa, #c3dc99);
background: linear-gradient(to bottom right , #c5d5fa, #c3dc99);
```
<p class="illustration">
    <img src="https://i.imgur.com/TDOTHk5.png" alt="漸層方向依序為上左、下左、上右、下右">
    漸層方向依序為上左、下左、上右、下右
</p>


### 用度數設置漸變方向
不過因為用關鍵字只能設定特定方向，若想設置其他角度只能用度數來設定。設定時是**以左下角為圓心，夾角以 12 點鐘方向順時鐘轉動來表示**。

這邊一樣設置柔紫色 `#c5d5fa` 為起始色、柔綠色 `#c3dc99` 為終點色，並依序設置漸層不同的度數，其效果如下：
```css
background: linear-gradient(0deg, #c5d5fa, #c3dc99);
background: linear-gradient(30deg, #c5d5fa, #c3dc99);
background: linear-gradient(45deg, #c5d5fa, #c3dc99);
background: linear-gradient(60deg, #c5d5fa, #c3dc99);
background: linear-gradient(90deg, #c5d5fa, #c3dc99);
```
  
<p class="illustration">
    <img src="https://i.imgur.com/e911Iad.png" alt="漸層方向依序為0度、30度、45度、60度、90度">
    漸層方向依序為0度、30度、45度、60度、90度
</p>

仔細觀察，可以看到漸層依順時鐘的方向在轉動。其中 45 倍數角度的渲染效果基本就跟關鍵字的渲染效果一致。這邊試著依每 30 度為一步旋轉 360 度，其效果如下：

<p class="illustration">
    <img src="https://i.imgur.com/125pGOh.png" alt="漸層方向依順時鐘旋轉一圈的效果">
    漸層方向依順時鐘旋轉一圈的效果
</p>


### 多種顏色漸層
除了兩色外，漸層的顏色還可以使用多色漸層。以三色為例 `#c5d5fa`、`#fcdef4`、`#c3dc99` ，因為沒有設定定位參數，所以三種顏色平均分配漸層位置，分別是 0%、50%、100%。


```css
background: linear-gradient(#c5d5fa, #fcdef4,  #c3dc99);
```
<p class="illustration">
    <img src="https://imgur.com/2cWDQex.png" alt="預設分配位置的三色漸層">
    預設分配位置的三色漸層
</p>

這邊試著把粉紅色 `#fcdef4` 的位置由預設的 50%，上提到 15% 的位置。可以看到因為粉紅色 `#fcdef4` 的位置上提，導致柔紫色 `#c5d5fa` 的位置被壓縮。

```css
background: linear-gradient(#c5d5fa 0%, #fcdef4 15%, #c3dc99 100%);
```
<p class="illustration">
    <img src="https://i.imgur.com/jx2ihat.png" alt="自訂分配位置的三色漸層">
    自訂分配位置的三色漸層
</p>

<br class="big">

如果多一點顏色，還能做出彩虹的效果，就是出來的效果有點傷眼睛 XDDD

```css
background: linear-gradient(red, orange, yellow, green, 
                            blue, indigo, purple);
```
<p class="illustration">
    <img src="https://i.imgur.com/EkYuUvI.png" alt="彩虹漸層">
    彩虹漸層
</p>

這時可以搭配定位參數的使用，畫出銳利的直線：
```css
background: linear-gradient(red 0%, red 15%, 
                            orange 15%, orange 30%,
                            yellow 30%, yellow 45%,
                            green 45%, green 60%,
                            blue 60%, blue 75%,
                            indigo 75%, indigo 90%,
                            purple 90%, purple 100%);
```

<p class="illustration">
    <img src="https://i.imgur.com/wOh7IbF.png" alt="彩虹">
    彩虹
</p>

這效果主要是藉由將兩種顏色位置重疊，最終會直接畫出一條壁壘分明直線：

```css
background: linear-gradient(#c5d5fa 50% ,#c3dc99 50%);
```
<p class="illustration">
    <img src="https://i.imgur.com/wtIhDIB.png" alt="壁壘分明直線">
    壁壘分明直線
</p>


### Patterns with Gradients
如果能熟練的使用線性漸層能拼出一些有趣的花樣。像是在 [oxxo](https://www.oxxostudio.tw/articles/202008/css-gradient.html) 就弄出了個拼磚：

<p class="illustration">
    <img src="https://i.imgur.com/ZvmN70e.png" alt="彩色拼磚">
    彩色拼磚（範例來源: <a href="https://www.oxxostudio.tw/articles/202008/css-gradient.html">OXXO.STUDIO</a>）
</p>

不過這個需要按照尺寸客製化去調整。使用上有點小麻煩。

<br class="big">

到是在 [Quackitn](https://www.quackit.com/css/codes/patterns/) 跟 [卡斯伯](https://www.casper.tw/css/2013/09/24/css-background/) 這邊找到些有趣的花樣：
 
 <p class="illustration">
    <img src="https://i.imgur.com/D2yeUdU.png" alt="grid Patterns">	
    <img src="https://i.imgur.com/BubAmOp.png" alt="grid Patterns">		
	<img src="https://i.imgur.com/uTdE9Wu.png" alt="Zig-Zag Patterns">
    <img src="https://i.imgur.com/1aQ6wSG.png" alt="Box Patterns">
    <img src="https://i.imgur.com/xlHu2L0.png" alt="Zig-Zag Patterns">
	<img src="https://i.imgur.com/FodellA.png" alt="Pyramid">	
	<img src="https://i.imgur.com/7TUeJIm.png" alt="Half-Rombes">	
    <img src="https://i.imgur.com/7ABhKhK.png" alt="Zig-Zag Patterns">
	Box and Zig-Zag Background Patterns（範例來源: <a href="https://www.quackit.com/css/codes/patterns/">Quackitn</a>、<a href="https://www.casper.tw/css/2013/09/24/css-background/">卡斯伯 Blog</a>、<a href="https://medium.com/@lavanyaratnabala/css-patterns-870d65192f40">Medium</a>、<a href="https://css-tricks.com/background-patterns-simplified-by-conic-gradients/">CSS-Tricks</a>）
	
	
</p>

分享個我比較喜歡樣式，也就是上面第一張圖的 CSS：
```css
background: 
    linear-gradient(135deg, #ccc 25%, transparent 25%) -50px 0,
    linear-gradient(225deg, #eee 25%, transparent 25%) -50px 0,
    linear-gradient(315deg, #ccc 25%, transparent 25%),
    linear-gradient(45deg, #eee 25%, transparent 25%);	
background-size: 100px 100px;
```



## Radial Gradient 徑向漸層
除了基礎的線性漸層，另一個常見的漸層方式徑向漸層。它與線性漸層一樣，會用設定的顏色填滿整個區域，但與之不同的是，徑向漸層是從單點為顏色起始點，並依圓形或橢圓的方式向外放射延伸。

```css
background: radial-gradient(shape size at position, color-stop1, color-stop2, ...);  
```

1. **shape size at position**  
	其實這邊有三個參數 shape、size 與 position。
	- **shape**   
		指的是向外放射延伸的形狀，**預設是橢圓（ellipse）**，另一個選項則是圓形（circle）。
	- **size**  
		指的是橢圓/圓形的半徑，**預設是以圓心到最遠角（farthest-corner）** 為半徑進行漸變，其餘的值有最近邊（closest-side）、最近角（closest-corner）、最遠邊（farthest-side）。這邊一樣等等來做實驗。
	- **position**  
		預設會將圓心設在中心點（center）。
2. **color-stop**  
	跟線性漸層一樣，有顏色與定位參數兩個參數，寫在前的顏色越接近圓心。
	
	
### 設置放射延伸形狀
剛剛提過延伸的形狀有兩種：<mark>橢圓（ellipse）與圓形（circle）兩種</mark>。這邊一樣柔紫色 `#c5d5fa` 為起始色、柔綠色 `#c3dc99` 為終點色，來觀察兩種形狀的漸層效果：

```css
background: radial-gradient(circle, #c5d5fa, #c3dc99);
background: radial-gradient(ellipse, #c5d5fa, #c3dc99);
```

<p class="illustration">
    <img src="https://i.imgur.com/xvr87iW.png" alt="觀察圓形與橢圓的漸層效果">
    觀察圓形與橢圓的漸層效果
</p>

一樣可以透過定位參數來調整它的渲染效果：

```css
background: radial-gradient(circle, #c5d5fa 25%, #c3dc99);
background: radial-gradient(circle, #c5d5fa 70%, #c3dc99);
```

<p class="illustration">
    <img src="https://i.imgur.com/orkcNCD.png" alt="不同定位參數的圓形漸層效果">
    不同定位參數的圓形漸層效果
</p>


### 設置放射中心
無論是橢圓還是圓形其放射中心，都是預設在區域中心。如果想更改放射中心位置有 3 種方式：
1. **絕對位置**：直接使用長度值來定義位置（如：10px）
2. **百分比值**：使用百分比（例：10%）來定義位置
3. **關鍵字**：使用 top、 bottom、 left、 right、 center 關鍵字來指明位置，也可將關鍵字使用，就是前面提到的 top left、 top right...等。
 
對了關鍵字 **at** 別忘了：
```css
background: radial-gradient(circle, #c5d5fa, #c3dc99);
background: radial-gradient(circle at 30px 50px, #c5d5fa, #c3dc99);
background: radial-gradient(circle at 80% 20%, #c5d5fa, #c3dc99);
background: radial-gradient(circle at left bottom, #c5d5fa, #c3dc99);
```

<p class="illustration">
    <img src="https://i.imgur.com/niOVS4G.png" alt="不同放射中心的圓形漸層效果">
    不同放射中心的圓形漸層效果
</p>


### 設置放射半徑
若依照參數順序，這小節應該放在設置放射延伸形狀與設置放射中心的中間章節比較合適，不過因為效果圖必須搭配放射中心的調整來看會比較清楚，所以我把它移到這邊來看效果。

這邊總共有 4 個值可選最遠角（farthest-corner）、最近邊（closest-side）、最近角（closest-corner）、最遠邊（farthest-side）：

```css
background:radial-gradient(circle closest-side at center, #c5d5fa, #c3dc99);
background:radial-gradient(circle farthest-side at center, #c5d5fa, #c3dc99);
background:radial-gradient(circle closest-corner at left, #c5d5fa, #c3dc99);
background:radial-gradient(circle farthest-corner at left, #c5d5fa, #c3dc99);
```  

<p class="illustration">
	<img src="https://i.imgur.com/zeLzbT8.png" alt="不同放射半徑的圓形漸層效果">
	不同放射半徑的圓形漸層效果
</p>


### 多種顏色漸層
至於多種顏色的設置跟線性漸層的一致，如果沒有設置定位參數，所有顏色會平均分配：

```css
background:radial-gradient(circle, red, orange, yellow, 
                           green, blue, indigo, purple);
```  

<p class="illustration">
	<img src="https://i.imgur.com/GrePbTF.png" alt="徑向彩虹漸層">
	徑向彩虹漸層
</p>
 
一樣透過設置定位參數，可以調整顏色的分配狀況，若直接將顏色重疊，會做出同心圓效果...雖然我的圖被截掉...不過應該還看得出是同心圓啦 XDDD
 
```css
background:radial-gradient(circle, 
                            red 0%, red 15%, 
                            orange 15%, orange 30%,
                            yellow 30%, yellow 45%,
                            green 45%, green 60%,
                            blue 60%, blue 75%,
                            indigo 75%, indigo 90%,
                            purple 90%, purple 100%);
```  

<p class="illustration">
	<img src="https://i.imgur.com/nPp3Pv2.png" alt="徑向彩虹漸層">
	彩虹同心圓
</p>


### Patterns with Gradients
每次看人家的範例，就覺得就覺得人家真超厲害，一個簡單的漸層都可以完出個花來：

<p class="illustration">
	<img src="https://i.imgur.com/asXOzkV.png" alt="圓圈背景">
	<img src="https://i.imgur.com/RmT8uXv.png" alt="圓圈背景">
	<img src="https://i.imgur.com/F5Lxk7c.png" alt="圓圈背景">
	<img src="https://i.imgur.com/xSpvlTa.png" alt="圓圈背景">
	圓圈背景（範例來源: <a href="https://www.quackit.com/css/codes/patterns/">Quackitn</a>、<a href="https://www.casper.tw/css/2013/09/24/css-background/">卡斯伯 Blog</a>、<a href="https://medium.com/@lavanyaratnabala/css-patterns-870d65192f40">Medium</a>）
</p>

[oxxo](https://www.oxxostudio.tw/articles/202008/css-gradient.html) 還能畫出個球體：
<p class="illustration">
	<img src="https://i.imgur.com/nzHhNQy.png" alt="球體">
	球體（範例來源: <a href="https://www.oxxostudio.tw/articles/202008/css-gradient.html">OXXO.STUDIO</a>）
</p>


橘黃色點點那個還滿漂亮的，留一下 CSS 好了：

```css
background-color: orange;
background-image: 
    radial-gradient(gold 15%, transparent 15%),
    radial-gradient(gold 40%, transparent 40%);
background-size: 60px 60px;
background-position: 0 0, 30px 30px;
```  


## Conic Gradient 錐形漸層
錐形漸層語法有點類似徑向漸層，但漸變的方向確不太一樣。徑向漸層是由圓心向外漸變，而錐形漸層則是<mark>順時針繞圓心漸變</mark>：

<p class="illustration">
	<img src="https://i.imgur.com/FZQT7D8.png" alt="線性漸層、徑向漸層與錐形漸層的漸變差異">
	線性漸層、徑向漸層與錐形漸層的漸變差異（範例來源: <a href="https://followandrew.dev/css-conic-gradient-effects-tutorial/">FollowAndrew</a>）
</p>


```css
background-image: 
    conic-gradient(from angle at position, color-stop1, color-stop2, ...); 
```

1. **from angle**  
	開始旋轉的角度，預設是從 **0 度** 開始順時鐘漸變。
2. **at position**  
	錐形漸層的圓心，預設圓心是在圖片中心。

<br class="big">

自己上點顏色來看看，

```css
background: conic-gradient(#c5d5fa, #fcdef4, #c3dc99);
background: radial-gradient(circle, #c5d5fa, #fcdef4, #c3dc99);
```
<p class="illustration">
	<img src="https://i.imgur.com/3zlx7Bq.png" alt="徑向漸層與錐形漸層的漸變差異">
	徑向漸層與錐形漸層的漸變差異
</p>


### 設置起始旋轉的角度
這邊要特別注意的大概就是關鍵字 <mark>from</mark> 別丟了？

```css
background: conic-gradient(from 0deg, #c5d5fa, #fcdef4, #c3dc99);
background: conic-gradient(from 90deg, #c5d5fa, #fcdef4, #c3dc99);
background: conic-gradient(from 180deg, #c5d5fa, #fcdef4, #c3dc99);
background: conic-gradient(from 270deg, #c5d5fa, #fcdef4, #c3dc99);
```

<p class="illustration">
	<img src="https://i.imgur.com/fEyVauI.png" alt="不同起始旋轉的角度的錐形漸層效果">
	不同起始旋轉的角度的錐形漸層效果
</p>


### 設置錐形的圓心
跟徑向漸層的放射中心設置方式一樣，可以透過設置**絕對位置**、**百分比值**與**關鍵字**的 3 種方式，一樣 <mark>at</mark> 別忘了：

```css
background: conic-gradient(#c5d5fa, #fcdef4, #c3dc99);
background: conic-gradient(at 30px 50px, #c5d5fa, #fcdef4, #c3dc99);
background: conic-gradient(at 80% 20%, #c5d5fa, #fcdef4, #c3dc99);
background: conic-gradient(at left bottom, #c5d5fa, #fcdef4, #c3dc99);
```


<p class="illustration">
	<img src="https://i.imgur.com/2tpVgCd.png" alt="不同圓心的錐形漸層效果">
	不同圓心的錐形漸層效果
</p>


### 多種顏色漸層
其實我前面已經用了三個顏色了，但...我想畫彩虹 :rainbow: 所以我畫了兩版，一版是漸層變色，一版是色塊版：

```css
width:200px;
height:200px;
border-radius:50%;     
background: conic-gradient(red, orange, yellow, green, blue, indigo, purple, red);
background: conic-gradient(red 0, red 25deg, 
                           orange 25deg, orange 75deg , 
                           yellow 75deg, yellow 125deg,
                           green 125deg, green 175deg, 
                           blue 175deg, blue 225deg, 
                           indigo 225deg, indigo 275deg, 
                           purple 275deg, purple 325deg, 
                           red 325deg, red 360deg);
```


<p class="illustration">
	<img src="https://i.imgur.com/uvHHuaL.png" alt="彩虹錐形漸變">
	彩虹錐形漸變
</p>



### Patterns with Gradients
錐形好像比較少見到各種花裡胡哨的圖案，到是比較常見各種圓餅圖：
<p class="illustration">
	<img src="https://i.imgur.com/gnT6EZJ.png" alt="圓餅圖">
	圓餅圖（範例來源: <a href="https://lenadesign.org/2021/05/15/css-conic-gradient/">Lena Design</a>）
</p>

另一種比較常見的則是這種環形色票：

<p class="illustration">
	<img src="https://i.imgur.com/hXxMXQv.png" alt="色票">
	色票（範例來源: <a href="https://lenadesign.org/2021/05/15/css-conic-gradient/">Lena Design</a>）
</p>




## Repeating Linear Gradients 重複線性漸層
前面的範例中，如果要做出重複的漸層效果，都會透過 `background-size` 與 `background-repeat` 來實現。但除此之外，還可以透過 `repeating-linear-gradient` 輕鬆實現重複效果：

```css
background:repeating-linear-gradient(direction, color-stop1, color-stop2, ...);  
```

基本使用方法，與 `linear-gradient` 基本一致，但需要指定定位參數，瀏覽器會將需要重複的顏色放置在指定位置，其餘部份會自動計算補滿：
```css
background:repeating-linear-gradient(#c5d5fa, #c3dc99  15%);
```

<p class="illustration">
	<img src="https://i.imgur.com/O2uC0Yl.png" alt="repeating-linear-gradient 漸層效果">
	repeating-linear-gradient 漸層效果
</p>

以這個例子為例，會將 `#c5d5fa` 與 `#c3dc99` 分別擺放在 0% 和 15%，其餘部份就會重複填滿了。不過我發現這個重複的交界處的過渡太過明顯，所以我在 30% 的地方加上起始色 `#c5d5fa`，讓它的產生平滑過渡的效果：

```css
background:repeating-linear-gradient(#c5d5fa, #c3dc99  15%, #c5d5fa 30%);
```

<p class="illustration">
	<img src="https://i.imgur.com/Uka8hxp.png" alt="repeating-linear-gradient 漸層效果">
	repeating-linear-gradient 漸層效果
</p>

若沒有指定定位參數，使用起來的效果基本跟 `linear-gradient` 相同：
<p class="illustration">
	<img src="https://i.imgur.com/ZTFefJ9.png" alt="不給定定位參數的 repeating-linear-gradient 漸層效果">
	不給定定位參數的 repeating-linear-gradient 漸層效果
</p>


### Patterns with Gradients
一樣找了些有趣花樣：

<p class="illustration">
	<img src="https://i.imgur.com/SdRNmr2.png" alt="網狀線">
	<img src="https://i.imgur.com/xEVPr40.png" alt="斑馬線">
	<img src="https://i.imgur.com/Duhv4Zt.png" alt="警戒線">
	網格與線狀（範例來源: <a href="https://www.quackit.com/css/codes/patterns/">Quackitn</a>、<a href="https://www.casper.tw/css/2013/09/24/css-background/">卡斯伯 Blog</a>、<a href="https://medium.com/@lavanyaratnabala/css-patterns-870d65192f40">Medium</a>）
</p>

那個橘黑色的配色，有點像道路工程的警示色 XDDD
```css
background: repeating-linear-gradient(45deg, 
                                      black 0, black 5%, 
                                      orange 5%, orange 10%);
```  



## Repeating Radial Gradient 重複徑向漸層
除線性漸層有重複效果外，徑向漸層也有：

```css
background:repeating-radial-gradient(direction, color-stop1, color-stop2, ...);  
```


基本使用方法，與 radial-gradient 基本一致，但需要指定定位參數，剩下交由瀏覽器自動補滿：
```css
background: repeating-radial-gradient(circle, #c5d5fa 5%, #c3dc99 10%);
```

<p class="illustration">
	<img src="https://i.imgur.com/KHL7UqV.png" alt="重複徑向漸層">
	重複徑向漸層
</p>

不曉得為啥，好像出現類似陰影的效果，我換個顏色感覺會更明顯：
<p class="illustration">
	<img src="https://i.imgur.com/Ukz4lEF.png" alt="重複徑向漸層有自帶陰影效果">
	重複徑向漸層有自帶陰影效果
</p>

一樣若想讓過渡效果平滑點，可將起始色設為新的終點色，就是眼睛看起來有點花：
<p class="illustration">
	<img src="https://i.imgur.com/rbu1CdL.png" alt="平滑過渡效果的重複徑向漸層">
	平滑過渡效果的重複徑向漸層
</p>


### Patterns with Gradients
這個我就不找太多範例了，我眼睛好花阿 @@ 

```css
background: repeating-radial-gradient(closest-side at 25px 35px, orange 15%, gold 40%);
background-size:60px 60px;
```

<p class="illustration">
	<img src="https://i.imgur.com/iS4ABKV.png" alt="圈圈">
	圈圈（範例來源: <a href="https://www.quackit.com/css/codes/patterns/">Quackitn</a>）
</p>



## Repeating Conic Gradient 重複錐形漸層
如果用重複錐形漸層則可做出放射線的背景效果：

```css
background: repeating-conic-gradient(from angle at position, color-stop1, color-stop2, ...);  
```

基本使用方法，與 conic-gradient 基本一致，一樣別忘了定位參數：
```css
background: repeating-conic-gradient(red 0, yellow 15deg);      
background: repeating-conic-gradient(red 0, red 15deg, yellow 15deg,  yellow 30deg);     
```

<p class="illustration">
	<img src="https://i.imgur.com/v5mf4wi.png" alt="重複錐形漸層">
	重複錐形漸層
</p>


### Patterns with Gradients
如果能熟練是用漸層的話，可以試著疊加不同的漸層效果，例如漸淡融入的效果；也能取代之前一些像是棋盤實做，它的程式碼會更加的簡短。

<p class="illustration">
	<img src="https://i.imgur.com/j0H2hRP.png" alt="重複錐形漸層花樣">
	重複錐形漸層花樣（範例來源: <a href="https://www.oxxostudio.tw/articles/202008/css-gradient.html">OXXO.STUDIO</a>、<a href="https://css-tricks.com/background-patterns-simplified-by-conic-gradients/">CSS-Tricks</a>）
</p>


最後記錄下棋盤的 CSS
```css
border:7px solid #000;
background: repeating-conic-gradient(black 0% 25%, white 0% 50%) 43%/45px 50px; 
```

## Combine background image with gradient overlay
原本我以為圖片也有漸層屬性可用，不過看來我多想了，它也是利用漸層效果疊加做出來的：

```css
background-image:
    linear-gradient(to bottom, rgba(195, 220, 153, 0.32), rgba(255, 255, 255, 0.73) 80%),
    url('https://i.imgur.com/OzTiFeI.png');
```

<p class="illustration">
	<img src="https://i.imgur.com/b9FkiTd.png" alt="圖片漸層效果">
	圖片漸層效果（原圖片來源: <a href="https://699pic.com/tupian-401782443.html">摄图网</a>）
</p>



## Color Palettes Generator and Color Gradient Tool
這部份跟 CSS 比較沒關係，純粹就是給配色苦手用的參考資料。這些網站有現成的配色方案，能配出許多還不錯的視覺效果 XDDD 

1. [Fresh Background Gradients](https://webgradients.com/) **推** :star2: 
2. [UI Gradients](https://uigradients.com/)
3. [Colorzilla Gradients](http://www.colorzilla.com/gradient-editor/)
4. [ColorSpace](https://mycolor.space/)
5. [Grabient]( http://grabient.com)
6. [coolhue](http://webkul.github.io/coolhue)
7. [CSS GEARS]( https://gradients.cssgears.com/)



## 參考資料 
1. 卡斯伯 (2013-09-24)。[CSS沒有極限 - CSS3的漸層](https://www.casper.tw/css/2013/09/24/css-background/)。檢自 卡斯伯 Blog (2022-12-22)。
2. oxxo (2022-08-30)。[深入理解 CSS 漸層 ( CSS Gradient ) ](https://www.oxxostudio.tw/articles/202008/css-gradient.html)。檢自 OXXO.STUDIO (2022-12-22)。
3. 小艾 (2018-01-14)。[Day26：小事之 多重背景與漸層背景 CSS3 Gradients](https://ithelp.ithome.com.tw/articles/10197136)。檢自 iT 邦幫忙 (2022-12-22)。
4. bdp (2017-12-07)。[CSS：background 雙色漸層](https://ithelp.ithome.com.tw/articles/10190867)。檢自 iT 邦幫忙 (2022-12-22)。
5. 逗點人 (2017-10-27)。[三種絕美CSS 背景漸層語法產生器(FRESH BACKGROUND GRADIENTS）-適用FIREFOX、IE](https://blog.7netic.com/2017/10/27/三種絕美css-背景漸層語法產生器fresh-background-gradients）-適用firefox、ie/)。檢自 逗點人7netic設計插畫誌 (2022-12-22)。
6. [CSS linear-gradient() Function](https://www.quackit.com/css/functions/css_linear-gradient_function.cfm)。檢自 Quackit (2022-12-22)。
7. [CSS radial-gradient() function](https://www.w3schools.com/cssref/func_radial-gradient.php)。檢自 W3schools (2022-12-22)。
8. Lena Stanley (2021-05-15)。[CSS Conic-Gradient](https://lenadesign.org/2021/05/15/css-conic-gradient/)。檢自 Lena Design (2022-12-22)。
9. Ana Tudor (May 28, 2020-05-28)。[Background Patterns, Simplified by Conic Gradients](https://css-tricks.com/background-patterns-simplified-by-conic-gradients/)。檢自 CSS-Tricks (2022-12-26)。
10. (2021-06-16)。[How to add a gradient overlay to a background image using just CSS and HTML](https://webdevetc.com/blog/how-to-add-a-gradient-overlay-to-a-background-image-using-just-css-and-html/)。檢自 Web dev etc (2022-12-26)。
11. 沾醬油(2018-04-19)。[色票-好用的9個漸層配色網站](https://spencer581026.pixnet.net/blog/post/402900452-色票-好用的9個漸層配色網站)。檢自 沾醬油的部落格｜痞客邦 (2022-12-26)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-02-15</summary>
  <ul>  
    <li>2023-02-15 發布</li>
    <li>2022-12-26 完稿</li>
    <li>2022-12-22 起稿</li>
  </ul>
</details>
