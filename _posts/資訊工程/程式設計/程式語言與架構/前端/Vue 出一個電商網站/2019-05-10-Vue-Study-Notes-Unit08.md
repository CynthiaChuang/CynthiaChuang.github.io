---
title: 【Vue.js 學習筆記】08. Vue 常用 API
date: 2019-05-10
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- 讀書筆記
- Udemy 
- 前端
- Vue.js
- Vue 出一個電商網站 
--- 

本節內容包含下述子章節：
1. 使用 Extend 避免重複造輪子
2. Filter 自訂畫面資料呈現格式
3. 無法寫入的資料，用 set 搞定他
4. Mixin 混合其它的元件內容
5. 使用 Directive 開發自己的互動 UI

<!--more-->



## 使用 Extend 避免重複造輪子
這邊跟 Java 一樣中使用 _extends_ 作為其擴充父類別的關鍵字，不過它雖然關鍵字是 _extends_ 有加 _s_ ，但實質是<mark>不支援</mark>多重繼承的。
 
實做方式如下，先使用 <mark>Vue.extend</mark> 實做要被擴充父類別：
```javascript
// extend  
var newExtends = Vue.extend({   
   data: function () {   
      return {   
         data: {},   
         extendData: '這段文字是 extend 得到'
   }}}),
   mounted: function () {
      console.log('Extend:', this)
   },
   computed: {
      getValue(){
         console.log('getValue Extend:', this)
   }
},
```

<p class="paragraph-spacing"></p> 

在子類別中，去繼承並客製化所需求的部分：

```javascript
var childOne = {
   extends: newExtend,
   computed: {
      getValue(){
         console.log('getValue childOne:', this)
   }
}   

var childTwo = {
   extends: newExtend,
   template: '#row-component-two',
   mounted: function () {
      console.log('childTwo:', this)
   }
}
```

<p class="paragraph-spacing"></p>

就我自己理解當使用 Extend 擴充父類別時，Object 會被繼承，（如：mounted，computed）。若在子類別的 Object 中宣告與父類別相同名稱的 <mark>變數</mark> 或 <mark>函式</mark> 名稱，則會 Override 父類別的。是說不曉得有 super 可以用？

<p class="paragraph-spacing"></p>

<div class="alert info">
<div class="head">2019.05.10 補充</div>
在 <a herf="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super">mozilla</a> 有看到 super 的用法，但在使用時它一直報錯，懷疑與 Vue 本身有關係，詢問講師後得到的回答是：<b>「沒辦法使用super，這邊 extend 是 VUE API，所以與 ES6 Class Extend 是沒有關聯的」</b>。
</div>



## Filter 自訂畫面資料呈現格式
Vue.js 還有提供 Filter（過濾器）的功能，主要用於處理格式化文字等狀況。filter 可重複使用，一個值可以套用多個 filter。

<p class="paragraph-spacing"></p> 

filter 一樣可以分為 <mark>全域</mark> 與 <mark>局部</mark>，全域的宣告是在外層使用 <mark>Vue.filter</mark>

```javascript
Vue.filter(filterName,function(n){
   ...
})
```

<p class="paragraph-spacing"></p> 

而局部宣告是在元件中加入 <mark>filters</mark> 的物件
```javascript
var child = {
   data:function() {
      return {
         ...
      }
   },
   filters: {
      filterName:function (n) {
         ...
      },
   }
}
```

<p class="paragraph-spacing"></p>

如要使用 filter 過濾器，則在 Mustache 中在變數後方使用 <mark>「|」（pipe）</mark>符號聯集多個 filter，執行時由左向右執行，它會將 pipe 符號左方的結果當作參數傳數入右方的 filter ，所以在撰寫 filter function 時記得宣告參數。

{% raw %}
```javascript
<td>
   {{ item.icash | separator | dollarSign}}
</td>

filters: {
   dollarSign: function (n) {
      return `$ ${n}`
   },

   separator: function (n) {
      return n.toFixed(2).replace(/./g, function (c, i, a) {
         return   i && c !== "." && ((a.length - i) % 3 === 0) ? ',' + c : c;
      });
   }
},
```
{% endraw %}



## 無法寫入的資料，用 set 搞定它
在第四章的進階模板語法介紹中，有提過 [Vue.set()](https://myskilltree.blogspot.com/2019/04/vuejs-04.html#v-for-與其使用細節)，那時說過 Vue 無法探測普通的新增屬性，因此必須使用 **Vue.set( target, key, value )** 強制將資料寫入視圖中，它才能在 UI 上反應。

如過要確認物件的內容是否有被綁定在 DOM 中，可用 console.log 印出 this 確認 Vue 的內容，如果 <mark>物件的元素中不包含 get 與 set 時</mark>，下圖上框，代表<mark>資料結構並未被綁定在 DOM 中</mark>。
 
<p class="illustration">
    <img src="https://i.imgur.com/vrR6i8A.png">
</p>

<p class="paragraph-spacing"></p> 

未被綁定在 DOM 的資料，雖然 log 或 tool 上看到的資料內容可能正確，但並不會觸發 UI 的更新。

不過 get 與 set 似乎只有物件才有，其他變數型態如 String、boolean 並沒有，似乎沒有其他方式可以觀察。...不過好像也不用觀察...直接看有沒有在 data 裡面就好了 XD



## Mixin 混合其它的元件內容
如果說 extend 是繼承的概念，它會繼承父類別的所有行為。而 <mark>mixin 有點類似 Util 或 Helper</mark>，在一旁用物件將元件的功能寫好，讓使用者可以直接程式中宣告 <mark>mixins</mark> 後傳入使用（這邊的 _s_ 總算加對了...），而一個元件可以傳入多的物件，需注意傳入的時，需以陣列方式傳入，即：

```javascript
mixins:[object1, object2]
```
<p class="paragraph-spacing"></p> 

完整程式碼如下：

```javascript
// 用物件將元件的功能寫好
var mixinFilter = { 
   template: '#row-component',
   filters: {
      dollarSign: function(n) {
         return `$ ${n}`
      },
      separator: function (n) {
         return n.toFixed(2).replace(/./g, function (c, i, a) {
            return   i && c !== "." && ((a.length - i) % 3 === 0) ? ',' + c : c;
         });
      }
   },
            
var mixinMounted = {
   mounted() {
      console.log('這段是 Mixin 產生')
   }
}

Vue.component('row-component', {
   props: ['item'],
   data: function() {
      return { 
         aAata: {}, 
      }
   },
   mixins:[mixinFilter, mixinMounted],
);
```



## 使用 Directive 開發自己的互動 UI
先把[文件](https://vuejs.org/v2/guide/custom-directive.html)啃一啃，尤其 [Hook Functions](https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions "Hook Functions") 與 [Directive Hook Arguments](https://vuejs.org/v2/guide/custom-directive.html#Directive-Hook-Arguments "Directive Hook Arguments") 的部分。

先看一個簡單的範例先
```html
<div id="app">
   // v-focus = "v-" + directive name
   <input type="email" v-model="email" v-focus>
</div>   

<script>
// focus 為 directive name， html 中會使用 "v-" + directive name，來呼叫這個指令
Vue.directive('focus', {
   // 定義生命週期該執行的動作，function 中可以傳入的參數在文件中 Directive Hook Arguments
   // 有定義。
   inserted: function(el) {
      el.focus()
   }
})
</script>
```
 
<p class="paragraph-spacing"></p> 

通常 function 常用的參數有
- el：指令所绑定的元素，可以用来直接操作 HTML
- binding：directive 所自帶的一些屬性  
- vnode：Vue 的虛擬節點 

這邊需要搭配 console.log 印出 this，才會比較清楚要對這些參數下的哪些元素做操作。



## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/Vue-Study-Notes-Contents/)



## 參考資料
1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [Vue.js: Filter 過濾器｜Summer。桑莫。夏天](https://cythilya.github.io/2017/05/23/vue-filter/)
3. [Vue.js: Mixins｜Summer。桑莫。夏天](https://cythilya.github.io/2017/09/24/vue-mixins/)