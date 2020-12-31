---
title: 【Vue.js 學習筆記】05. Vue 的生命週期
date: 2019-04-24
is_modified: false
categories:
- 研讀筆記
- 程式語言與架構
tags:
- Front-end
- Vue.js
- Udemy 
- 《Vue 出一個電商網站》 
--- 

本節內容包含下述子章節：
1. Vue 生命週期
2. 練習測驗 2：生命週期章節小測驗

<!--more-->
<br>

## Vue 生命週期

![Imgur](https://i.imgur.com/vDihCVx.png)
<center class="imgtext"> Lifecycle-Diagram（圖片來源: <a href="https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram" class="imgtext">Vue.js</a>）</center>

<br>

每個 Vue instance 在被創建時都要經過一系列的初始化過程，在這個過程中 Vue 也會運行  <span class='highlighting'>lifecycle hooks</span> 的函數，提供使用者在不同階段添加自己的操作。


上圖中，紅色圓角框的文字，都屬於 Vue 所提供的 lifecycle hooks，呼叫方式如下：
```javascript
data: function(){
  return {...}
},
beforeCreate(){
  console.log('beforeCreate! ${this.text}');
},
created(){
  alert('created! ${this.text}');
},
beforeMount(){
  alert('beforeMount! ${this.text}');
},
```
<div class="alert danger">
<div class="head">注意</div>
不要在選項屬性上使用<b>箭頭函數</b>！<br>
不要在選項屬性上使用<b>箭頭函數</b>！<br>
不要在選項屬性上使用<b>箭頭函數</b>！<br>
</div>

<br>例如：

```javascript
created：() => console.log(this.a)

或

vm.$watch('a'，newValue => this.myMethod())
```

因為箭頭函數並沒有 **this** ， **this**  會作為變數一直向上找尋，直到找到，因此經常導致 
```
Uncaught TypeError: Cannot read property of undefined

或 

Uncaught TypeError: this.myMethod is not a function`
```
之類的錯誤。

<br>

### 各階段介紹

#### **beforeCreate**
剛完成初始化，此階段資料尚未產生，理論上此階段別進行操作資料，但也不是沒辦法啦 → [vue怎么在beforeCreate里获取data](https://segmentfault.com/q/1010000012331476)。不過人家最後也說了...**實際情況中從來沒遇到過需要在組件還沒初始化就去拿 data 的**...

#### **created**
數據觀測後所產生的 hook，從此階段開始才能對資料做操作。

#### **beforeMount**
編譯完模板後會被觸發，但此階段尚未將模板掛載到 HTML 的 DOM 元素上。

#### **mounted**
直到此階段才將 template 整個掛載到 HTML 上，這時候才能進行一些 HTML 的操作。假如有載入 jQuery，要到這步驟才能操作 HTML 的 DOM 元素。

#### **beforeUpdate**
在元件建立起來後，它會因為資料變動的關係而觸發 beforeUpdate 並進行重新繪製。

#### **updated**
直到資料重新繪製完成後，會再觸發 updated。

#### **beforeDestroy** 與 **destroyed**
分別會在銷毀元件前後觸發。

#### **deactivated** 與 **activated**
一般元件，如：v-if，不想每次切換條件判斷式就被 destroy 並摧毀該元件上所記錄的資料，導致下次更改判斷式後就必須重新走一次 created 流程。

此時就可以使用 **\<keep-alive\>** 來維持資料狀態，避免被 destroy。當組件在 \<keep-alive\>  內被切換，它的 deactivated 和 activated 這兩個 lifecycle hooks 函數將會被對應執行。

<br><br>

## 練習測驗 2：生命週期章節小測驗
**問題 1：生命週期中，如果我們要透過 Ajax 讀取資料，至少到哪個階段才能正確運作？**  
1. [ ] beforeCreate  
2. [ ] init  
3. [x] Created  

<br>

**問題 2： keep-alive 標籤內的元件描述，以下何者為對？**  
1. [ ] v-if 判斷隱藏後，一樣會觸發 destroyed 的生命週期。  
2. [x] 在 v-if 判斷隱藏後，資料也不會消失。  
3. [ ] 重複切換出現的時候，一樣會進入 create  的生命週期。  

<br><br>  

## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/Vue-Study-Notes-Contents/)


<br><br>

## 參考資料
1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [The Vue Instance｜Vue.js](https://vuejs.org/v2/guide/instance.html)
3. [vue.js - vue怎么在beforeCreate里获取data｜SegmentFault 思否](https://segmentfault.com/q/1010000012331476)
