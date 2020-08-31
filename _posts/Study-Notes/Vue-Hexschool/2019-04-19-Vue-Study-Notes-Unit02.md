---
title: 【Vue.js 學習筆記】02. 基礎 Vue 概述
date: 2019-04-19
is_modified: false
categories:
- Study-Notes
- Computer-Language/Framework
tags:
- Front-end
- Vue.js
- Udemy 
- Vue 出一個電商網站 
--- 

本節內容包含下述子章節：
1. 應用程式建立
2. 雙向綁定的資料 與 MVVM 的概念
4. v-bind 動態屬性指令
5. v-for 動態產生多筆資料於畫面上
6. 使用 v-on 來操作頁面行為
7. 預先定義資料狀態的重要性
8. 透過修飾符，讓 v-on 操作更簡單
9. :class 動態切換 className
10. computed 運算功能
11. Vue 表單與資料的綁定
12. 元件基礎概念

<!--more-->
<br>

## 應用程式建立

{% raw %}
```html
<div id='app'>
  {{ text }}
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    text:  '這是一段話'
  }
})
</script>
```
{% endraw %}
<br>其中 el 是應用程式與 Vue 綁定的方法，綁定時不定要用 id 綁定，可綁定 class :
```html
<div id='app'> 改成 <div class='app'>
el: '.app',    改成  el: '.app',
```
<br>
需注意的是 Vue 一次只能綁定一個元素，因此即便改用 class 重複的 class，也無法顯示相對應的 text。與 id 一樣具有唯一性，因此建議使用 id 。

另外需要特別注意的是，Vue 是不支援巢狀的應用程式，它會直接報錯給你看！

{% raw %}
```html
<div id='app'>
  {{text}}  
  <div  id='app2'>
    {{text2}}
  </div>
</div> 
  
<script>
var app = new Vue({
  el: '#app',
  data: {
    text: '這是一段話'
  }
})

var app2 = new Vue({
  el: '#app2',
  data: {
    text: '這是一段話'
  }
})
</script>
```
{% endraw %}
<br>


會得到下列的錯誤：
> vendor.js:600 [Vue warn]: Property or method "text2" is not defined on the instance but referenced during render. Make sure that this property is reactive, either in the data option, or for class-based components, by initializing the property. See: https://vuejs.org/v2/guide/reactivity.html#Declaring-Reactive-Properties.
>
>(found in Root)

<br>
<div class="alert info">
前面章節介紹的都是<b>直接在網頁上運行</b>，初始化的方法直接用 new Vue 就能夠運行，屬於基本觀念。 <br>

後面章節會介紹透過 <b>Webpack 編譯</b>，才會看到使用 export default 的相關方法進行初始化。
</div>

<br><br>
## 雙向綁定的資料 與 MVVM 的概念

![MVVM](https://i.imgur.com/0b3Lz16.png)
<center class="imgtext">Model View View Model（圖片來源: <a href="https://zh.wikipedia.org/wiki/MVVM" class="imgtext">維基百科</a>）</center>

<br> Vue 是一種受到 [MVVM（Model–view–viewmodel）](https://zh.wikipedia.org/wiki/MVVM) 啟發的架構。實際上寫程式碼時，並不會寫到 VM 的部份，只需要寫 Model 即可。在資料變動的同時 VM 就會去控制視圖的話，反之，若從 UI 更改相關資料， VM 則會通知 Model 改寫資料   

<br> 將 data 中的字串綁定至 DOM 上可使用下列方法：

{% raw %}
1. **Mustache 語法**：  
就是 `{{}}` ，在中間插入顯示資料。例如： `{{text}}`。

2. **v-text**：  
v-text 和 Mustache 語法起到的效果是一樣的。同樣的，動態修改對象名稱的值時，渲染結果也會有對應的變化。例如：`<div v-text="text"></div>`。

3. **v-html**：  
`innerHTML` 的概念，會將包含html的字串解析成 `字符實體`。
{% endraw %} 

<br> 
若想透過 UI 改變資料，則使用 **v-model**， 雙向綁並資料，一旦改變 UI 資料，則後端資料更著改變。
```html
<input type="text" v-model="message">
```

<br> 
<div class="alert warning">
重點說三次！<br>
<b>VUE JS是以資料狀態操作畫面</b>！<br>
<b>VUE JS是以資料狀態操作畫面</b>！<br>
<b>VUE JS是以資料狀態操作畫面</b>！<br>
</div>


<br><br>

## v-bind 動態屬性指令

v-text 與 v-html 也是屬於 Vue 指令的一種，更多的的指令可以看官方的 [API 文件](https://vuejs.org/v2/api/#Directives)，就我個人經驗 **v-** 開頭幾乎都是。

v-bind 是用來更新 HTML 上的屬性使用的，例如：
```html
<div id="app">
  <img v-bind:src="imgSrc" v-bind:class="className" alt="">
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    imgSrc: 'https://images.unsplash.com/photo-1479568933336-ea01829af8de?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=d9926ef56492b20aea8508ed32ec6030&auto=format&fit=crop&w=2250&q=80',
    className: 'img-fluid'
  }
})
</script>
```
其中，img-fluid 是 Bootstrap4 的語法，可以將圖片寬度限制在 100 % 的寬度內。
 
另外在後面章節有提到，<span class='highlighting'>v-bind:src='imgSrc'</span> 可縮寫成  <span class='highlighting'>:src='imgSrc'</span>。

<br><br>

## v-for 動態產生多筆資料於畫面上

v-for 可以基於原始資料多次渲染指定元素，必須搭配特定語法 **alias in expression** 來使用，有點類似 **for each** 的用法。
{% raw %}
```html
<div id="app">
  <ui>
    <li v-for = "(item, idx) in list" 
      v-if ="item.age < 25"> 
      {{idx+1}}:{{item.name}} 年齡是 {{item.age}}
    </li>
  </ui>
</div> 
<script>
var app = new Vue({
  el: '#app',
  data: {
    list: [
      { name: '小明', age: 16 },
      { name: '媽媽', age: 38 },
      { name: '漂亮阿姨', age: 24}
    ]
  }
})
</script>
```
{% endraw %} 
程式碼中的 **v-if** 就是判斷式，會根據表達式子回傳的值來決定是否渲染該元素。若程式中 **v-for** 與 **v-if** 一起使用時，需要特別注意：
1. **v-for** 的優先全會高於 **v-if** 。 

2.  根據 [style guide](https://vuejs.org/v2/style-guide/)，不建議 **v-for** 與 **v-if** 一起使用時，如有這種狀況建議使用 **computed** 屬性，讓其回傳過濾後的列表。
 
 
<br><br>

## 使用 v-on 來操作頁面行為

用於實做互動功能，主要用來綁定事件監聽器，被綁定的方法需宣告於 methods 物件中。需注意的是若想在方法中調用 data 或其他方法時，需要加上 **this** ，this 所指向的則是當前 DOM。

{% raw %}
```html
<div id="app">
  <input type="text" class="form-control" v-model="text">
  <button class="btn btn-primary mt-1" v-on:click="reverseText">反轉字串</button>
  <div class="mt-3">
    {{ newText }}
  </div>
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    text: '',
    newText: ''
  },
  methods: {
    reverseText() {
      this.newText = this.text.split('').reverse().join('');
    }
  },
});
</script>
```
{% endraw %} 


<br>另外在後面章節有提到， <span class='highlighting'>v-on:click="reverseText"</span> 可縮寫成 <span class='highlighting'>@click="reverseText"</span>。

<br><br>

## 預先定義資料狀態的重要性

在 Vue 中若要操作它的資料，必須要<span class='highlighting'>先定義好它的資料結構</span>。若未事先定義，將無法綁定資料內容，會得到該變數 is not define的錯誤。且事前定義資料有利於程式的程式的維護，且有助於提升可讀性。

<br>
不過那天寫程式時被同事提醒，忘了在 data 定義該變數，但卻發現 UI 參考的到，資料也可以進行操作。稍微檢查了一下，發現是因為是在 created 時有進行過一次初始化的動作。

請教老師這是為什麼，助教給了下列的回覆，沒有吃得透測，想說等等上完所有內容再回頭理解這句話：

<div class="alert info">
<div class="head">助教回覆</div>
其實你可以把 data:{ } 看成是一個物件，
而你在 created() 內執行的動作，
就是幫這個物件新增了一個屬性叫 newText 並賦值 newText 。
提供一個範例給你參考，<a href="https://codepen.io/pvt5r486/pen/YMaKZq?editors=1010">codepen</a>
</div>

<br>

<div class="alert info">
<div class="head">2019.05.09 更新</div>
學到後面的時候再回看這個問題，發現我當初的測試有誤，雖然在 UI 中可以顯示第一次 newText 的賦值結果，但之後的 newText 的值得更新都不會反應在 UI 上了。
</div>

<br><br>

## 透過修飾符，讓 v-on 操作更簡單

Vue 的官網中有一句話：<span class='highlighting'>方法只有純粹的數據邏輯，而不是去處理 DOM 事件細節</span>。因此雖然可以在事件處理程序中調用 event 的相關操作，但還是建議使用 <span class='highlighting'>事件修飾符</span> 處理了 DOM 事件的細節，讓事件處理程專注於程式邏輯的撰寫。

關於 v-on 所提供的事件修飾符詳見[官網文件](https://vuejs.org/v2/guide/events.html#Event-Modifiers)，舉例來說，若想移除元素預設行為可用：
```javascript
v-on:click="reverseText"  

reverseText(event) {
  event.preventDefault();
  ...
}
```
<br>或是直接使用事件修飾符
```html
v-on:click.prevent="reverseText"
```
<br>

除了事件修飾符外還有[鍵盤修飾符](https://vuejs.org/v2/guide/events.html#Key-Modifiers)與[滑鼠按鈕修飾符](https://vuejs.org/v2/guide/events.html#Mouse-Button-Modifiers) ... 等。

PS. 看影片時發現一個很酷的功能，在 VSCode 中輸入br * 20 ，就會新增 20 個 \<br\> 。
 
 
<br><br>

## :class 動態切換 className

```html
<div id="app">
  <div class="box" :class= "{'rotate': isTransform }"></div>
  <hr>
  <button class="btn btn-outline-primary"  
          @click="isTransform = !isTransform">
      <!-- 選轉物件 -->
  </button>
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    isTransform:  false
  },
});
</script>

<style>
.box {
  transition: transform .5s;
}
.box.rotate {
  transform: rotate(45deg)
}
</style>
```

<br> 簡單來說就是
```html
v-bind:class="{ '要加入的className': '判斷式'}"
```
 
<br><br>

## computed 運算功能

computed 內的 function 內容，<span class='highlighting'>所相依的資料有產生變動時會被觸發，重新運算結果呈現於畫面上</span>。

這邊 reverseText 一旦偵測到相依 this.text 資料有變動，也就是使用者於輸入框輸入文字時，v-model 就會改變 text 的內容，而 text 一旦改變就會觸發 reverseText 重新計算後並顯示於畫面上，因此可以及時到到反轉的結果。 
{% raw %}
```html
<div id="app">
  <input type="text" class="form-control" v-model="text">
  <button class="btn btn-primary mt-1">反轉字串</button>
  {{ reverseText }}
</div>

<script>
var app = new Vue({
  el: '#app',
  data: {
    text: '',
  },
  computed: {
    reverseText(){
      return this.text.split('').reverse().join('')
    }
  },
});
</script>
```
{% endraw %} 
<br>而在[使用 v-on 來操作頁面行為](#使用-v-on-來操作頁面行為)一章中，我們使用 ``v-on:click="reverseText2"`` ，這邊綁定 reverseText2 則是一個 methods，methods 是互動函式，**需要觸發才會運作**。


就我理解，若希望 UI 顯示隨資料立即更新，就用 computed，若是由其他東西觸發再來更新 UI 的，則是使用 methods。
 
<br><br>

## Vue 表單與資料的綁定

#### 1. **string 與 input、textarea 的雙向綁定**  
在元素直接使用 v-model 綁定 string 。
<br>

#### 2. **boolean 與 （單一）checkbox 的雙向綁定**  
（單一）checkbox 預設值為 boolean，在 checkbox 用 v-model 綁定 boolean 後，checkbox 的勾選狀態改變，綁定 boolean 會隨之變為相對應 true/false 狀態。
<br>

#### 3. **array 與 checkbox-array 的雙向綁定**  
在儲值的 array 與  checkbox-array 相互綁定後，當勾選任意選項後，改選項值會被加入儲值的 array。

PS.1  checkbox-array 中的每個 checkbox 是 v-model  <span class='highlighting'>同一個</span> array。

另外實作時 checkbox-array 的 value 務必設定，且必須為<span class='highlighting'>唯一值</span>，否則 value 無法寫入儲值的 array，且複選顯示會出問題。可以事前準備一個 array 紀錄 value ，整個 checkbox-array 使用 v-for 來來改寫。

```html
checkboxArray = []

<div class="form-check">
<input type="checkbox" class="form-check-input" 
        id="check2" value="雞"  v-model="checkboxArray">
<label class="form-check-label" for="check2">雞</label>
</div>

<div class="form-check">
<input type="checkbox" class="form-check-input"
        id="check3" value="豬"  v-model="checkboxArray">
<label class="form-check-label" for="check3">豬</label>
</div>

<div class="form-check">
<input type="checkbox" class="form-check-input"
        id="check4" value="牛"  v-model="checkboxArray">
<label class="form-check-label" for="check4">牛</label>
</div>
```
<br>

#### 4. **string 與 single radio 的雙向綁定**  
在 string 與  single radio  相互綁定後，string 紀錄目前選定元素的 value。一樣相同 name 的 radio 必須綁定同一個 string。 
<br>

#### 5. **string 與 selected 的雙向綁定**  
在 string 與 selected  相互綁定後，string 紀錄目前選定 option 的 value。option 放在首項，表示這是 default 值，若 default 值在引導使用這下拉後，就不提供使用這選取，可以在 option 的屬性中加上 disabled 。<br>
```html
selected = "" ; 
<select name="" id="" class="form-control" v-model="selected"> 
  <option disabled value="">請選擇</option>
  <option value="小美">小美</option>
  <option value="小妞">可愛小妞</option>
  <option value="阿姨">漂亮阿姨</option>
</select>
```

<br><br>

##  元件基礎概念

每一個 Vue 元件都可以獨立儲存自己的狀態。為避免因使用同一個變數而在造成狀態混淆的狀況，會將它細分成各個元件，在元件獨立控制自己的狀態。
 
來一張經典的：
![Organizing Components](https://i.imgur.com/1a2KVgb.png)
<center class="imgtext">components（圖片來源: <a href="https://vuejs.org/v2/guide/components.html#Organizing-Components" class="imgtext">Vue.js</a>）</center>
　　
<br>定義 component 的方法如下：
```javascript
Vue.component('元件名稱', {
  data: function () {  
    return {  
      count: 0  
    }  
  },
  template: `html 語法`  
```

<br><br>component 的元件名稱，這個元件名稱會是我們在 HTML 中所使用的元素標籤，按官方 style guide 建議元件名稱盡量採用組合字，不使用一個單字，避免原生元素標籤衝突。

而 component 內部的寫法，其實與一般 Vue 元件寫法是一致的，唯一需要特別住要的是 data 在 component 內部是以 **function** 來回傳物件內容。


<br><br>

##  練習測驗 1：基礎章節測驗
**問題 1： v-bind 是用來綁定動態資料，那麼 v-on 指令是用來綁定什麼？**
1. [x]  事件
2. [ ]  資料
3. [ ]  陣列

**問題 2： v-bind 是不是用來綁定 "動態資料" 的屬性？**
1. [x]  是
2. [ ]  不是

**問題 3： Vue 的應用程式資料**
1. [x]  盡量不用預先定義
2. [ ]  盡量可能的預先定義


<br><br>

## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/Vue-Study-Notes-Contents/)


<br><br> 

## 參考資料
1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [MVVM｜維基百科](https://zh.wikipedia.org/wiki/MVVM)
3. [List Rendering｜Vue.js](https://vuejs.org/v2/guide/list.html#v-for-with-v-if)
4. [Style Guide｜Vue.js](https://vuejs.org/v2/style-guide/#Avoid-v-if-with-v-for-essential)
5. [鐵人賽：JavaScript 的 this 到底是誰？｜卡斯伯 Blog - 前端，沒有極限](https://wcc723.github.io/javascript/2017/12/12/javascript-this/)
6. [Vue.js: 計算屬性 Computed｜Summer。桑莫。夏天](https://cythilya.github.io/2017/04/15/vue-computed/)