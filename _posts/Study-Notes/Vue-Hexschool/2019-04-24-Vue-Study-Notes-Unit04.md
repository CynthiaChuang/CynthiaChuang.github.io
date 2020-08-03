---
title: 【Vue.js 學習筆記】04. 進階模板語法介紹
date: 2019-04-24
modified: 2020-04-24
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
1. 模板資料細節說明
2. 動態切換 ClassName 及 Style 多種方法
3. v-for 與其使用細節
4. v-if 與其使用細節
5. Computed 與 Watch
6. 表單細節操作
7. v-on 的頁面操作細節
9. 章節作業
 

<!--more-->
<br>

## 模板資料細節說明
{% raw %}
1. 在 UI 上顯示變數內容，可在直接使用 ``{{}}`` 包覆變數即可顯示。
2. 在 Vue 中無法直接使用 ``{{}}`` 插入一段 **HTML** 語法，該語法會直接轉成純文字後顯示，若要插入 HTML 語法請使用 **v-html**。
    <br> 
    <div class="alert warning">
    <div class="head">注意</div>
    在網站上動態渲染任意 HTML 是非常危險的，因為容易導致 XSS 攻擊。因此只在可信內容上使用 v-html，永不用在使用者提交的內容（例如：使用者留言）上。
    </div>
{% endraw %}


<br><br>

## 動態切換 ClassName 及 Style 多種方法

1. **直接傳入物件**  
    ```html
    :class = "{'className1':判斷式1,'className2':判斷式2}"
    ```

2. **傳入物件變數**  
	將欲傳入 class 的物件，定義為一個物件變數 
    ```javascript
    objectClass: { 'rotate':  false,  'bg-danger':  false, }
    ```
    再將變數傳入 class 中 
    ```html
    :class = objectClass
    ```

    <br>若是想在 html 中變更 objectClass 內容，可以使用下列方式來讀取及修改。
    ```html
    bjectClass.rotate 
    
    或
    
    objectClass['rotate']
    ``` 
    但若 key 值變數中存在 dash，就只能使用中括號的方式讀取，不然會報錯。
    

3. **陣列寫法**  
	適用於 class name 長度比較不確定的狀況。 
    ```html
    class = ['className1','className2']
    ```
    
    <br>也可以把 array 的部份拉出拉獨立成為一個變數 
    ```html
    :class = arrayClass 
    
    與
    
    arrayClass = ['className1','className2']
    ```

    備註：checkbox 與 array 資料的綁定方法，請看第二章節中 [Vue 表單與資料的綁定](/Vue-Study-Notes-Unit02#vue-表單與資料的綁定) 的部份。<br> <br> 

4. **綁定行內樣式**  
	注意原先 CSS 是使用中線命名規則，要改成駝峰式，例如 background-color 改成 backgroundColor，在綁定時可以直接插入物件即可 
    ```html
    :style="{屬性:"值"}"></div>
    ```
    <br>或是將物件抽取出來成為一個獨立變數後在指定給 style，若是傳入物件多於一個可以是用物件陣列的方式來處理：
    ```html
    :style=[{屬性1:"值1"},{屬性2:"值2"}]
    ```

    <br> PS. 關於變數在 HTML 使用時，使用中線命名或駝峰式都可以，只是若是使用中線命名必須要用單引號括起來，backgroundColor → 'background-color'。 

5.  **Prefix**   
	不需要手動加上 Prefix ，它會自動依照每個瀏覽器版本需求自動加上。

<br><br>

## v-for 與其使用細節

1. **陣列與物件（key value pair）**  
	都可以使用 v-for 來執行。但兩者取出索引指略有不同，在陣列取出索引的是 index，而物件的取出索引是 key 值，或稱物件的屬性。
    
2. **Key**  
    根據[文件](https://cn.vuejs.org/v2/guide/list.html#key)表示，v-for 正在更新**已渲染過**的元素列表時，需要特別注意它默認用 **就地複用** 策略。
    <br>
    <div class="alert info">
    <div class="head">就地複用</div>
    所謂的就地複用指的是，如果數據項的順序被改變，Vue 不會移動 DOM 元素來匹配數據項的順序，而是直接重複使用原本位置上的元素。
    </div>

    <br>
    舉例來說，下圖是資料反轉前後的對照圖，可以看到資料反轉，但輸入框不會隨數據項順序改變而一起變動：
    
    ![Imgur](https://i.imgur.com/l5tkc02.png)

    一般來說，這樣的默認模式效能較好，且在資料展示的場景中並不會有這個困擾。但若子元素間存在相依性、存在與使用者互動的場景、或是依賴臨時的 DOM 狀態，則不推薦使用就地複用模式。
    
    例如：上例中可以讓使用對所要輸入的表單進行重新排序，那麼建議在 v-for 為每個節點加上 key 值，它能方便 Vue 跟蹤每個節點，從而重用和重新排序現有元素。


3. **Filter**  
	`Array.Filter()` 顧名思義就是過濾，給定一個的條件式，Filter 為將符合這條件式的元素組合成一個新陣列後回傳。
    

4. **不能運作的狀況**  
     <span class='highlighting'>不要直接更動 array 的內容</span>，即便透過 console.log 和 Vue 工具看資料都是修改過的，但畫面依然顯示舊資料，因為視圖無更新，無效的改動如下：
    ```javascript
    // 情境一：直接修改陣列長度（在一般 js 中，表示刪除所有資料）  
    this.arrayData.length = 0;

    // 情境二：透過索引來取代資料  
    this.arrayData[0] = {  
      name: '阿強',  
      age: 99
    }
    ```

    <br>因為 <span class='highlighting'>Vue 無法探測普通的新增屬性</span>，它必須用於向響應式對像上添加新屬性，因此必須使用 <span class='highlighting'>Vue.set</span> 強制將資料寫入視圖中：

    ```javascript
    Vue.set(this.arrayData, 0, {
      name: '阿強',
      age: 99
    })
    ```

	delete 也是相同情況 <span class='highlighting'>Vue.delete( target, key )</span>。

5.  **純數字的迴圈**  
    {% raw %}
    ```html
    <li v-for=”item in 輸入數字範圍"> 
      {{ item }}  
    </li>
    ```
    {% endraw %}

6. **Template**  
    當輸出的有特定之格式，可利用 template 書寫，template 在最後產生 HTML 時並不會被渲染出來，但它可以用放上 Vue 的指令：
    {% raw %}
    ```html
    <table class=”table”>
      <template v-for=”item in arrayData”> 
        <tr><td>{{item.name}}</td></tr>  
        <tr><td>{{item.age}}</td></tr>  
      <template>
    </table>
    ```
    {% endraw %}


7. **v-for 與 v-if**
    我之前整理過了，請參考： [02. 基礎 Vue 概述 #v-for 動態產生多筆資料於畫面上](/Vue-Study-Notes-Unit02#v-for-動態產生多筆資料於畫面上)
 
 

<br><br>

## v-if 與其使用細節 

1.  **v-if, v-else, v-else-if**  
    這還好不難想像，有 if 就會有 else if 跟 else。通常的是這三個應該是實作在相鄰元素上，如果在程式碼上相隔的太遠，個人建議就要考慮一下是否有需要重構了。

2.  **Key**  
    Vue 會盡可能高效地渲染元素，通常會重複用已有元素而不是從頭開始渲染。但這種方式可能會造成了畫面資訊不一致的狀況，舉例來說：
    ```html
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input class="form-control" placeholder="Enter your username">
    </template>

    <template v-else>
      <label>Email</label>
      <input class="form-control" placeholder="Enter your email address">
    </template>
    ```
	
	<br>因為兩個模板使用了相同的元素，**input** 不會被替換掉，僅僅是替換了它的 **placeholder** 。但是一旦你在輸入框輸入文字，就會出現下面的情況：
	
	![Imgur](https://i.imgur.com/UKtfhdO.png)
	
	<br>為了使完整渲染，可在輸入框元素加上 key ，一旦 key 不同就會重新渲染。注意這邊 key 只加在輸入框上，因此 label  依舊會被重複使用。
	
		
    ```html
    <template v-if="loginType === 'username'">
      <label>Username</label>
      <input class="form-control" placeholder="Enter your username" key="username">
    </template>

    <template v-else>
      <label>Email</label>
      <input class="form-control" placeholder="Enter your email address" key="email">
    </template>
    ```
	<br>
		
3. **v-if v.s v-show**
    根據[教學文章](https://v1-cn.vuejs.org/guide/conditional.html)說明，因為 v-if 是真實的條件渲染，在切換 v-if 區域時，Vue.js 有一個局部編譯/卸載過程，它會確保該區域在切換過程中能合適地銷毀與重建條件塊內的事件監聽器和子組件。

    且 v-if 是<span class='highlighting'>惰性的</span>：如果在初始渲染時條件不成立，就什麼也不做，直到在條件第一次變為成立後才開始局部編譯（編譯會被暫存起來）。

    相比之下，v-show 元素則是全部會被編譯並保留，只是簡單地基於 CSS 切換，設定 style 為 display:none。
    
    ![Imgur](https://i.imgur.com/gJtpP21.png)
    <br>
    一般來說，v-if 有更高的切換消耗而 v-show 有更高的初始渲染消耗。因此，<span class='highlighting'>如果需要頻繁切換，則使用 v-show 較好；如果在運行時條件不大可能改變，則使用 v-if 較好</span>。



<br><br>

## Computed 與 Watch
1. **Method：**  
    這是需要主動觸發，且可以多次重複觸發。
    
2. **Computed：**  
	是針對我們要輸入到畫面的內容，在輸出前需要做前處理。簡單來說，當資料出現變化需要<span class='highlighting'>改變畫面</span>的 function 放這裡 。
    
3. **Watch：**  
	監控特定變數，當該變數產生變化時，可執行特定事件。


<br><br>

## 表單細節操作

修飾符號
1. **v-model.lazy**  
    不是處於監視狀態，而是當輸入框失去 focus 才會觸發。
    
2. **v-model.number**  
    得到的資料型別是 number，而非 string。
    
3. **v-model.trim**  
    自動過濾輸入的首尾空格。

<br><br>

## v-on 的頁面操作細節

### 事件修飾符
1. **@click.stop**  
    可避免<span class='highlighting'>冒泡事件</span>。所謂的冒泡事件是指，點擊事件觸發後在 DOM 裡面傳遞的時會，從子元素不斷往其父元素傳遞，就像泡泡浮上水面一樣。 

    所對應的就是 jQuery 中的 **event.stopPropagation()**。

2. **@click.capture**  
    即元素自身觸發的事件先在此處理，然後才交由內部元素進行處理。

3. **@click.self**  
    只當事件是從偵聽器綁定的元素本身觸發時才觸發回調。


4. **@click.once**  
    只觸發一次回調，第二次就不再觸發。

<br> PS. 稍微找了一下相關資料，弄懂事件傳遞傳遞過程：[【Vue.js】DOM 的事件傳遞機制： capture 與 propagation](/DOM-Phases-of-Event-Capture-and-Propagation/)

<br>

### 按鍵修飾符
1. **.keyCode**  
    只當事件是從特定鍵觸發時才觸發回調。 例如：``@keyup.13``，13 為 enter 的 keyCode。
 

2. **別名修飾**  
    .enter, .tab, .delete, .esc, .space, .up, .down, .left, .right。 例如：``@keyup.enter``。


3. **按下相應按鍵時才觸發鼠標或鍵盤事件的監聽器**  
    .ctrl, .alt, .shift, .meta。 例如：``@keyup.shift.enter``，必須同時按下 shift 及 enter 才會觸發。
<br>

### 滑鼠修飾符
1. **.left**  
    只當點擊鼠標左鍵時觸發。
    
3. **.right**  
    只當點擊鼠標右鍵時觸發。
    
4. **.middle**  
    只當點擊鼠標中鍵時觸發。 


<br><br> 

## 章節作業

交作業：[github](https://github.com/CynthiaChuang/vue-exercise/tree/master/Hw2-Template)、[codepen](https://codepen.io/cynthiachuang/pen/WWaRoo)

<br><br>  

## 其他連結

1. [【Vue.js 學習筆記】00. 目錄](/Vue-Study-Notes-Contents/)


<br><br>

## 參考資料

1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [API (v-html)｜Vue.js](https://vuejs.org/v2/api/index.html#v-html)
3. [List Rendering｜Vue.js](https://vuejs.org/v2/guide/list.html#key)
4. [Vue2.0 中 v-for里面的 “就地复用” 策略 是什么？｜知乎](https://www.zhihu.com/question/61078310/answer/361261031)
5. [Array.prototype.filter() - JavaScript｜MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
6. [Conditional Rendering｜Vue.js](https://vuejs.org/v2/guide/conditional.html#Controlling-Reusable-Elements-with-key)
7. [Vue.js: 條件渲染 v-if、v-show｜Summer。桑莫。夏天](https://cythilya.github.io/2017/04/22/vue-conditional-rendering/)
8. [条件渲染｜vue.js](https://v1-cn.vuejs.org/guide/conditional.html)
9. [DOM 的事件傳遞機制：捕獲與冒泡｜TechBridge 技術共筆部落格](https://blog.techbridge.cc/2017/07/15/javascript-event-SANcjQ) 
