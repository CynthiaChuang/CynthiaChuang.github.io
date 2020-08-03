---
title: 【Vue.js 學習筆記】06. Vue.js 元件
date: 2019-05-07
modified: 2019-05-07
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
 
1.  元件概念介紹
2.  使用 x-template 建立元件
3.  使用 function return 建構資料格式
4.  props 基本觀念
5.  props 使用上的注意事項
6.  props 型別及預設值
7.  emit 向外層傳遞事件
8.  Slot 元件插槽
9.  使用 is 動態切換元件
10. 元件 章節作業
11. 測驗：Vue 元件測驗

<!--more-->
<br>

## 元件概念介紹
1. 元件可獨立運作且可重複使用。

2. 元件中的資料皆為<span class='highlighting'>獨立</span>的。

3. 父元件傳遞資料給子元件，使用 <span class='highlighting'>props</span> 傳遞。父元件更新資料時，props 會<span class='highlighting'>即時</span>將資料更新到內層元件。

4. 子元件若要傳遞給父元件，是使用 <span class='highlighting'>emit</span>，必須透過<span class='highlighting'>e觸發事件mit</span>才會能將資料傳到父元件，相對 props 來說它並非即時的。

5. SPA（single page application）也是透過元件製作。


<br><br>

## 使用 x-template 建立元件

在 [02. 基礎 Vue 概述](/Vue-Study-Notes-Unit02) 時，介紹過 component 的註冊方法，如下：

```html
Vue.component('row-comp', {
  props: ['person'],
  template: `<tr>
    <td>{{ person.name }}</td>
    <td>{{ person.cash }}</td>
    <td>{{ person.icash }}</td>
    </tr>
}	`
```
<br>

若是 template 中的語法太過複雜，會使用 x-template 的方式來改善程式的可讀性。 

在宣告 x-template 時，可使用 \<script\> 標籤來封裝我們的 HTML 模板，並在標籤帶上 **text/x-template** 的類型，然後通過一個 id 將模板進行引用。 

```html
<script type="text/x-template"  id="rowCompTemp">
 <tr>
  <td>{‌{ person.name }}</td>
  <td>{‌{ person.cash }}</td>
  <td>{‌{ person.icash }}</td>
 </tr>
</script>

Vue.component('row-comp', {
  props: ['person'],
  emplate: '#rowCompTemp'
})
```
<br>

其中，component 裡的 props 是提供給父元件傳值給子元件使用。完整 component 使用如下：

```html
<div id="app">
  <table class="table">
  <thead></thead>
    <tbody>
      <row-comp v-for="(item, key) in data"
                :person="item" 
                :key="key"></row-comp>
    </tbody>
  </table>
</div>
```
<br> 

不過在使用時 component 需要特別注意 HTML 的結構。

有些 HTML 元素，例如： **\<ul\>**、**\<o\>**、**\<table\>** 和 **\<select\>**，對於哪些元素可以出現在其內部是有嚴格限制的。而有些元素，例如：**\<li\>**、**\<tr\>** 和 、**\<option\>**，也被限定出現在特定的元素內部。 

以上例來說，**\<tbody\>** 的標籤中正常來說只能放 **\<tr\>**，如果放了自製的 **\<row-comp\>**，會被視作無效的內容提升到外部，並導致最終渲染結果出錯。

<span class='highlighting'>簡單來說就是資料顯示的出來，但 UI 卻會不合預期。</span>
<br> 

這種情況可以使用 Vue 的 **[is](https://cn.vuejs.org/v2/api/#is)** 來動態切換模板。在一開始載入 HTML 模板會正確，但在開始運行以後才動態的方式使用該屬性的標籤替換成指定的 component 。

```html
<div id="app">
  <table class="table">
  <thead></thead>
    <tbody>
      <tr v-for="(item, key) in data" 
          is="row-comp" 
          :person="item" :key="key"></tr>
    </tbody>
  </table>
</div>
```

使用下列方式註冊的 component 方式屬於 <span class='highlighting'>全域註冊 （Registration）</span>。
```javascript
Vue.component(tagName, options)
```
<br>

若要限定 component 在某個 Vue app 下才使用，應使用 <span class='highlighting'>局部註冊（Local Registration）</span>：

```javascript
<script>
  var child = {
    props: ['person'],
    template: '#rowCompTemp'
  }

  var app = new Vue({
    el: '#app',
    data: {...},
    components:{ 
      'row-comp':child
    }
  });
</script>
```


<br><br>

## 使用 function return 建構資料格式 

在 component 的 data 必須是一個<span class='highlighting'>函數</span>，因此必須使用 function return 才能正常運作。

這是為了確保每個 instance 可以維護一份<span class='highlighting'>獨立的拷貝物件</span>，避免修改一個 instance 的數值後影響到其他所有的 instance。

```javascript
Vue.component(‘counter-component’, {
  data: function(){
    return {counter: 0}
  },
  template: '#counter-component'
})  
```


<br><br>

## props 基本觀念
props 為<span class='highlighting'>父元件向子元件</span>傳遞資料的方式。在子元件中設定所要傳入的變數名稱，在 HTML 中使用該元件時使用該變數名稱即可將資料傳入。

```html
<photo img-url='圖片 url'></photo>

<script>
  Vue.component('photo', {
    props:['imgUrl'],
    template:  '#photo'
  })
<script>
```

<br>

需要注意一點的是，JavaScript 中習慣是用小駝峰命名，但在 HTML 中因為 HTML 對大小寫不敏感，小駝峰式命名需要轉換成使用中線命名，如這邊的 **imgUrl**，在 HTML 中是使用 **img-url** 。

<br>

傳遞資料的方式又可分成兩種：**靜態傳遞**、**動態傳遞**。

 - **靜態傳遞**：就是直接傳入字串。
 - **動態傳遞**：就是用 v-bind 的方式傳入父元件中的 data。



<br><br>

## props 使用上的注意事項

1. **單向數據流**  
    Vue 在不同 component 間強制使用<span class='highlighting'>單向數據流</span>，父元件可以向子元件傳遞數據，但是子元件<span class='highlighting'>不能直接修改</span>父元件的狀態。不然你會得到下列的錯誤訊息：

	>vendor.js:600 [Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "imgUrl"
    
	它這邊是建議若要針對數據進行修改，請在 data 中 mutated 一份，否則若直接修改 props，當父元件傳入新的 props 時，你的變動就會被覆蓋掉了。
    
2. **物件傳參考特性 與 尚未宣告的變數**  
    若資料匯入有時間差，可以使用 v-if 讓元件的產生時間往後移，可以判斷資料中的某個特性尚未讀入就不要顯示該元件之類的，避免 error / warn 產生。

    另外，JavaScript 在傳遞 **物件** 時 call by reference，所以在子元件內部的物件資料變動會影響外部物件。

 
3. **維持狀態與生命週期**  
    如果你在不想每次都呼叫 create ，就使用在 [05. Vue 的生命週期](/study-notes/computer-language/framework/2019/04/24/Vue-Study-Notes-Unit05/) 提到的 **\<keep-alive\>**。

 
<br><br>

## props 型別及預設值

```javascript
props: {     
  propA: Number,
  propB: [String, Number],
  propC: {
    type: String,
    required: true
  },
  propD: {
    type: Number,
    default: 100
  }
}
```
<br> 為 props 定義資料型別，可以避免傳入錯誤的資料內容...至少傳錯會跳 Warnning ！？
> [Vue warn]: Invalid prop: type check failed for prop "cash". Expected Number, got String.

<br> 另外需注意的是，使用靜態屬性傳入時，所傳入的一定會是**字串**，若是使用動態屬性，則
- 傳入變數：傳入資料的型態依照變數而定。
- 傳入值：若是直接 hard code 值，傳入轉成宣告的型態，如果轉得過去的話...如果轉不過去，它就會傳入系統所判定的型態，然後...再丟 Warnning 給你。
 

<br><br>

## emit 向外層傳遞事件
資料由子元件向外層傳遞，先看個完整程式碼，程式的目的是透過子元件 button-counter 去累加父元件的變數 cash 。

```html
<div id="app">
  <h2>透過 emit 向外傳遞資訊</h2>
    我透過元件儲值了 {{ cash }} 元
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>

<script>
  Vue.component('buttonCounter', {
    template:  
      `<div>
        <button @click="incrementCounter" class="btn btn-outline-primary">
          增加 {{ counter }} 元
        </button>
    <input type="number" class="form-control mt-2" v-model="counter">
      </div>`,
    data: function() {
      return {
        counter:  1
      }
    },
    methods: {
      incrementCounter(){
        this.$emit("increment", Number(this.counter))
      }
    }
  });

  var app = new Vue({
    el:  '#app',
    data: {
      cash:  300
    },
    methods: {
      incrementTotal(addCount){
        this.cash += addCount;
      }
    }
  });
</script>
```

<br> 實作步驟如下：

1. **實作父元件函式**  
    先在父元件中定義一個名為 incrementTotal 的 method，incrementTotal 所執行的動作就是 this.cash 做累加。

2. **傳入函式作為監聽器**  
    在 HTML 使用子元件標籤時，自訂一個觸發事件，並將在父元件的 method - 也就是先前定義 incrementTotal，傳入作為監聽器。
    
    兩者間用 **v-on** 進行綁定。
    ```html
    v-on:increment="incrementTotal"
    ```

    其中：increment 是自訂事件名稱，incrementTotal 則是父元件中的方法。

3. **由子元件傳值給父元件**  
    為子元件的 button 加上 click 事件 incrementCounter。而在該方法的實作中，我們會再使用 **emit** 去觸發自訂事件 **increment**
    ```javascript
    incrementCounter(){
      this.$emit("increment")
    }
    ```
    <br>
    若有需要做參數傳遞，則 

    ```javascript
    incrementCounter(){
      this.$emit("increment", Number(this.counter))
    }
    ```



<br><br>

## Slot 元件插槽

在一般情況下若直接在子元件標籤內新增內容的話會被模版直接替換，而看不到輸入的內容。因此若想讓父元件替換子元件部分內容，或想讓元件可以依各頁面需求進行客製化，應該使用 <span class='highlighting'>slot</span>。
<br>

### 單個插槽（Single Slot）
若 component 中只有一個位置可供替換，那麼使用 Single Slot 即可。使用方式只需在 component 的 template 中使用 ``<slot>...</slot>`` 定義可被替換的段落。

使用時直接在子元件標籤內填入想替換的內容即可，例如：

```html
<!--在父元件中，直接在子元件標籤內新增內容-->
<single-slot-component>  
  <p>使用這段取代原本的 Slot。</p>
</single-slot-component>

<!--子元件中實作-->
<script type="text/x-template" id="singleSlotComponent">  
  <slot><p>slot中的預設文字</p></slot>
</script>  
```

component 的 template 中設置的預設內容，只有在子元件標籤內沒有提供內容時才會被渲染。
<br>

### 具名插槽（Named Slots）
若 component 中提供多個位置可以替換，應使用具名插槽（Named Slots），template 中的 slot 需使用 **name** 屬性決定配置的內容。沒有 name 的插槽將成為預設插槽，匹配不到的內容會放置在此；若沒有默認插槽，匹配不到的內容會被捨棄。

而要資料放入該插槽時，只需在子元件標籤內宣告 slot 屬性並指定插入位置的 name，如：
```html
<!--在父元件中，直接在子元件標籤內新增內容-->
<named-slot-component>  
  <header slot="header">替換的 Header</header>  
  <footer slot="footer">替換的 Footer</footer>  
</named-slot-component>

<!--子元件中實作-->
<script type="text/x-template" id="namedSlotComponent">  
  <slot name="header">這段是預設的文字</slot>  
  <slot name="footer">這是預設的 Footer</slot>  
</script>
```
<br> 備註：一个不帶 **name** 的 **\<slot\>**，其 name 的預設值為 default。
<br>

> 在2.6.0中，Vue 為具名插槽和作用域插槽引入了一個新的統一的語法（即 v-slot 指令）。它取代了slot 和slot-scope 這兩個目前已被廢棄但未被移除且仍在文檔中的特性。 

> 依照[文件](https://cn.vuejs.org/v2/guide/components-slots.html)看來應該只是將要插入的方式由 **slot="slotName"** 改成 **v-slot:slotName** 即可，但需要特別注意下 Vue 版本必須為 2.6 版以後才可以順利使用新語法。（千萬別像我拿舊的 Vue來測試新版語法...蠢死了QAQ）

<br><br>

## 使用 is 動態切換元件

個人覺得這跟 :class 的用法類似，唯一不同的是 :class 是切換使用的 class，而 :is 則是切換使用的 component。


<br><br>

## 元件 章節作業
[codepen](https://codepen.io/cynthiachuang/pen/jobbaN)

<br><br>

## 測驗：Vue 元件測驗

**問題 1：Vue 的元件中，外部資料要傳入到內層，要使用何種方式？**  
1. [x] props  
2. [ ] emit  
3. [ ] on  
<br>

**問題 2： Vue 的元件中，外部資料要傳入到內層的字串、數值，內層可以再次修改它？**  
1. [ ] 對，它都傳進來了，怎麼做都可以。  
2. [x] 不對，這樣會有錯誤提示。  
<br>

**問題 3：Vue 的元件中，資料建構需額外使用 function return 的方式？**  
```javascript
data: function(){
return {}
}
```
1. [x] 對。  
2. [ ] 錯，沒有那個必要。  
<br>

**問題 4：Vue 的元件，如果外層有使用 keep-alive 包起來，在 created 生命週期加上 Ajax 則？**
1. [ ] 每次切換元件時，資料都會重新載入。  
2. [x] 只有第一次會載入資料，重新切換則會維持就有狀態。  
3. [ ] 元件將無法被 v-if 切換。  
<br>

**問題 5：內層的元件，如何傳遞數值到外層？**  
1. [x] 內層元件使用 emit 向外做事件傳遞。  
2. [ ] 外層使用 props 就能雙向綁定。  
3. [ ] 沒有辦法，元件資料是單向數據流。  




<br><br>

## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/study-notes/computer-language/framework/2019/04/18/Vue-Study-Notes-Contents/)

<br><br>

## 參考資料
1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [VueJS 元件載入模板 (template) 的幾種方式｜Kuro's Blog](https://kuro.tw/posts/2017/09/21/VueJS-%E5%85%83%E4%BB%B6%E8%BC%89%E5%85%A5%E6%A8%A1%E6%9D%BF-template-%E7%9A%84%E5%B9%BE%E7%A8%AE%E6%96%B9%E5%BC%8F/)
3. [Vue.js: 元件 Components 簡介 - 註冊與使用｜Summer。桑莫。夏天](https://cythilya.github.io/2017/05/11/vue-component-intro/)
4. [DOM Template Parsing Caveats｜Vue.js](https://vuejs.org/v2/guide/components.html#DOM-Template-Parsing-Caveats)
5. [API - is｜Vue.js](https://vuejs.org/v2/api/index.html#is)
6. [Components Basics｜Vue.js](https://vuejs.org/v2/guide/components.html)
7. [Vue.js: Slot｜Summer。桑莫。夏天](https://cythilya.github.io/2017/10/11/vue-component-slot/)

