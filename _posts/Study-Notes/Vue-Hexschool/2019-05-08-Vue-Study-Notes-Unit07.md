---
title: 【Vue.js 學習筆記】07. JavaScript ES6
date: 2019-05-08
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

1. 使用 let 與 const 宣告變數
2. 展開與其餘參數
4. 縮寫
5. 箭頭函式與傳統函式
6. 字串模板 Template String
7. 常用陣列方法
8. 測驗 2： ES6 小測驗

<!--more-->
<br>

## 使用 let 與 const 宣告變數

### let 與 var 

1. **提升（Hoisting）**  

    <div class="note info">
    <div class="head">提升（Hoisting）</div>
    在執行任何程式碼前，JavaScript 會把函式宣告放進記憶體裡面，這樣做的優點是：可以在程式碼宣告該函式之前使用它。
    </div>
  
    個人一直覺得 hoisting 是一種非常不直覺的特性，因為一般在寫程式的時候，都會事先定義好變數或函式才去使用它，如果在尚未定義的情況下，直接去使用這個變數或函式，通常都會出現錯誤訊息！

    然而，在 JavaScript 中有一個蠻特別的概念是 hoisting，讓你可以在程式碼宣告該變數或函式之前使用它。不過只有用 var 宣告的變數才有 hoisting，而 let 則沒有！  
      
    舉例來說：若你試圖在宣告 var 變數前印出該變數，這是可以的，而且不會報錯，只是會被告知 undefined。但如果是使用 let 宣告的變數，若在宣告前使用，會得到一個 no defined 的 error。

    ```javascript
    console.log(testVar); // undefined  
    console.log(testLet); // no defined
    var testVar = 5 ;
    let testLet = 5 ; 
    ```
    
    <br> 雖然 **undefined** 與 **not defined**，中文都可以翻成是未定義或無定義。不過在 JavaScript 中，代表的意思有些不同。在編譯階段，JavaScript 會預留記憶體給變數，執行時才會將變數內容指進記憶體中。如果是留了記憶體，但是未被賦值，因此會印出變數的值是 undefined。但若得到的是 not defined ，則是代表這個東西並沒有被定義其存在，因此沒有幫它留記憶體的意思。
    <br>

2. **作用域**  
    var 作用域是 **function** scope，而 let 作用域是 **block**。
    
    **用 for loop 和 setTimeout 來看作用域差別**  
        
    ```javascript
    for (var i = 0; i < 10; i++) {
      setTimeout(function () {
        console.log('這執行第' + i + '次');
      }, 10);
    }
    ```
    <br>當使用 **var** 來宣告迴圈變數 i 時，會輸出 10次 **這執行第10次**，而非預期中 **這執行第0~9次**。
    
    這原因主要是<span class='label'>作用域問題</span>，以這個 case 來說，i 並不屬於 for 迴圈的區域變數，而是全域變數（因為外面沒有包 function，因此直接掛到 window 下面了...）。等到 for 迴圈結束後，會從事件佇列中依序執行 setTimeout 中的 function 內容。此時 function 會去找 i 來印出，它只能找到全域變數下 i ，而 i 在迴圈結束後被設成了 10 ，因此會印出 10 次**這執行第10次**。

    而若是用 let 宣告變數，它的作用域是 block，因此每一次 for 迴圈執行 setTimeout 中的 function 都引用到 for 這個 block 作用域下的 i。不過需要注意的是，因為這樣的引用關係，這些變數 i 所佔的記憶體，在 setTimeout 未執行前都不會被釋放。
    
    詳細內容可以看看[這篇文章](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/231181/)。
 <br>
    
3. **重複命名**  
    var 同個區塊上重複命名沒關係，而 let 同個區塊上**不能**重複命名。
 
### const
const 是宣告常數用，一旦宣告就不能更改。

不過若是宣告物件，由於物件本身的紀錄方式是紀錄參考值，因此物件內的屬性是可以被修改，但是重新指定一個新的物件給常數的話還是會跳錯。
<br>

### 額外問題，不使用 let，如何印出正確答案
答：用[立即函式](http://www.victsao.com/blog/81-javascript/287-javascript-function-iife)傳入 i ，製造一個更小的作用域，讓 console.log 引用。

```javascript
for (var  i = 0; i < 10; i++) {
  console.log(i);
  (function(index){
    setTimeout(function () {
      console.log('這執行第' + index + '次');
    }, 10) ;
  }(i))
}
``` 
<br><br>

## 展開與其餘參數 

### **展開語法**  
用 `...` 將陣列中的值一個個取出來再 return 回去

```javascript
let groupA = ['小明', '杰倫', '阿姨'];  
let groupB = ['老媽', '老爸'];

let familyAll5 = groupA.concat(groupB) //ES5合併陣列  
let familyAll6=[...groupA,...groupB] //ES6合併陣列
```
<br>
	
### **淺複製 (Shallow Copy) V.S. 深複製 (Deep Copy)**  
JavaScript 的物件或是陣列的的儲存方式是紀錄記憶體位置，因此當將 groupA 指給 groupB 對 groupB 進行賦值時，是使用<span class='label'>淺複製</span>的方式將 groupA 所紀錄的記憶體位置傳給 groupB，最終會導致<span class='label'>當對 groupB 進行操作時， groupA 也會後受到影響</span>，即下圖左。

```javascript
let groupA = ['小明', '杰倫', '阿姨'];
let groupB = groupA
groupB.push('阿明'); 
console.log(groupA); 
//["小明", "杰倫", "阿姨", "阿明"]
```
    
<br> 為了避免這種影響到互相影響發生，可以改採<span class='label'>深複製</span>的方式（下圖右），借助上一節所提到的<span class='label'>展開語法</span>，將 groupA 值一一取出後，放入一個新的陣列中，處理物件也是相同宣告方法。

```javascript
let groupB = [...groupA]
```

不過我個人偏好使用 lodash 函式庫的 cloneDeep，比較直覺，尤其當你的變數存在物件中包物件的情況時。

<br>

![Shallow and Deep Copying](https://i.imgur.com/wIERqmP.png)
<center class="imgtext"> Shallow and Deep Copying （圖片來源: <a href="https://www.cs.utexas.edu/~scottm/cs307/handouts/deepCopying.htm" class="imgtext">utexas</a>）</center>
<br>
	 
### **類陣列觀念說明**
在使用 [Node.childNodes](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/childNodes) 或 [document.querySelectorAll](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelectorAll ) 時會回傳包含指定節點的子節點的集合，此集合稱為 <span class='label'>Node List</span>，是種類似陣列的資料結構，但是不能使用陣列的部份方法，因此又被稱為 <span class='label'>類陣列</span>。

若要將 Node List 轉成陣列，一樣是使用<span class='label'>展開語法</span>
```javascript
let myArray = [...myNodeList]
```

<br>

### **其餘參數**
就是不定長度參數的宣告，在 Python 中是使用 ``*args``、Hava 中是使用 ``int... nums``，而在 JavaScript 中的宣告有點類似 Java 是使用 ``...nums``，傳入的結果會是一個名為 nums 的陣列。
<br>

跟其他語言的不定參數一樣需遵守兩個規定：
1. 一個 function 的參數列表中最多只有一個不定長度參數。
2. 不定長度參數必須放在參數列表的最後一個。
<br>

JavaScript 還有一個（個人認為）比較神奇的特性，如果你傳入參數多於你宣告的個數，多餘的部份會變成名為 arguments 的類陣列物件，我之前學的語言多直接報錯的說... 
```javascript
function updateEasyCard() {
  let arg = [...arguments];
  let sum = arg.reduce(function (accumulator, currentValue) {
    return accumulator + currentValue;
  }, 0);
  console.log('我有 ' + sum + ' 元');
}

updateEasyCard(0); // 我有 0 元
updateEasyCard(10, 50, 100, 50, 5, 1, 1, 1, 500); // 我有 718 元
```
 
<br><br>

## 解構
解構全名為<span class='label'>解構賦值</span>，其概念是將右方資料<span class='label'>鏡射</span>到左方。下面舉些應用情境：
### 將陣列中的的值，賦予到變數上 
```javascript
let family = ['小明', '杰倫', '阿姨', '老媽', '老爸'];

//ES5  
let ming = family[0];
let jay = family[1];
let auntie = family[2];

//ES6  
let [ming, jay, auntie, mom, dad] = family;
```

<br>若要捨棄或略過陣列中部份的值 

```javascript
// 捨棄尾端的值
let family = ['小明', '杰倫', '阿姨', '老媽', '老爸'];  
let [ming, jay, auntie] = family ;

// 略過中間的值 
let [ming, jay, , mom, dad]  =family; // 略過部份不寫變數，但還是要記得留下空間
```
<br>

### 變數交換
```javascript
let Goku = "悟空";
let Ginyu = "基紐";
[Goku,Ginyu] = [Ginyu,Goku]
```
<br>

### 將字串拆解成字元
```javascript 
let str = "基紐特攻隊";
let [q,a,z,w,s]=str;
```

<br>如果要拆成字元陣列
```javascript 
let str ="基紐特攻隊";
let [...c]=str ;
// 或是
let c = str.split("")
```
<br>

### 從物件中取值並附與新的變數名稱 
```javascript
let GinyuTeam = {  
  Ginyu: "基紐",  
  Jeice: "吉斯",  
  burter: "巴特"
}
// 從物件中取出特定屬性 Ginyu ，並賦予新的變數名稱 leader
let{ Ginyu: leader } = GinyuTeam 

// 若不賦予新的變數名稱，則直接使用Ginyu
let{ Ginyu } = GinyuTeam 
```
<br>

### 預設值
可先給予各變數一個預設值，若在進行解構賦值時，沒有傳入值，會直接採用預設值。

1. 先分別賦予變數一個預設值 ming = '小明', jay = '杰倫'
2. 將後方變數進行解構賦值，將值鏡射到前方變數中
3. 若後方陣列中元素個數少於前方變數個數，多出的變數不會被賦值，因此直接使用預設值
    ```javascript
    let [ming = '小明', jay = '杰倫'] = ['阿明']
    // ming = 阿明, jay = '杰倫'
    ```

<br><br>

##  縮寫

應用情境：
### 合併物件
當屬性，也就是要傳數的<span class='label'>變數名稱，與 key 值名稱相同</span>時可省略，只寫一個即可。

```javascript 
const Frieza = "弗利沙";
const GinyuTeam = {Ginyu: "基紐", Jeice: "吉斯", burter: "巴特",}

//ES5
const allTeam = {  
  Frieza:Frieza,  
  GinyuTeam:GinyuTeam}

//ES6  
const allTeam = {  
  Frieza,  
  GinyuTeam}  
```

<br> 這種做法在使用 CLI 建構 project 時很常被使用到，例如引用 router 時，將 import 命名直接命名 router，在 new Vue 元件時，就不需要寫成 ``router:router``，直接使用 router 即可：
```javascript
import Vue from 'vue'  
import App from './App'  
import router from './router'
new Vue({  
  el: '#app',  
  router,  
  template: '<App/>',  
  components: { App }});
```
<br>

###  物件函式縮寫
下列兩段 function 的是相同的
```javascript
functionName: function(){
  ...
}

functionName(){
  ...
}
``` 

<br><br>

## 箭頭函式與傳統函式

在[展開與其餘參數](#展開與其餘參數)中說明不定長度參數時，有提過一個（個人認為）JS 比較神奇的特性，就是如果你傳入參數多於你宣告的個數，多餘的部份會變成名為 arguments 的類陣列物件。 

不過，這件事情在使用<span class='label'>箭頭函式</span>時是不成立的，你會直接得到一個 error。

> Uncaught ReferenceError: arguments is not defined
	    at updateEasyCard (arrow_function.html:118)
	    at arrow_function.html:120
	    
<br>另外一點需要注意的是，<span class='label'>兩種的函式所綁定的 this 是不同的</span>，
- 傳統函式：依呼叫的方法而定
- 箭頭函式：綁定到其定義時所在的物件

<br>~~超級~~有點難懂，所以後來找了相關的資料來看 [鐵人賽：箭頭函式 (Arrow functions)｜卡斯伯 Blog - 前端，沒有極限](https://wcc723.github.io/javascript/2017/12/21/javascript-es6-arrow-function/)

PS. 我記得這是講師的網誌！？

<br><br>

## 字串模板 Template String

使用反引號加錢字號 \$，就可以在 string 中引用變數，避免做字串串接。
```javascript
// 一般字串  
let originString = "我叫作 " + this.name ;

// ES6
let newString = `我叫作 ${this.name}`;
```

<br>

大括號的範圍內，除了引用變數，也可以直接引用 JavaScript code。
{% raw %}
```javascript
let newString= `<ul>\\
<li>我叫作${people[0].name}</li>\\
<li>我叫作${people[1].name}</li>\\
<li>我叫作${people[2].name}</li>\\</ul>`

// 引用 javascript code
let superNewString= 
 `<ul>\\
 ${people.map((person)=>`<li>我叫作${person.name}</li>`).join('')}\\</ul>`
```
{% endraw %}
<br><br>

## 常用陣列方法 

### **forEach**
forEach 顧名思義是對陣列的每個元素做操作，它是一個 _in-place_ 的方法，不會回傳新的值，直接在原陣列上做修改。是說實做上很少傳那的 array。
```javascript
people.forEach((item, index, array) => {
  ...
})
```

### **map**
這個方法會建立一個新的陣列，其內容為原陣列的每個元素經由函式運算後所回傳的結果之集合。

### **filter**
由原陣列中濾出符合條件的元素所構成的新陣列。

### **find**
只會回傳第一個滿足所提供條件的元素的值。

### **every**
它會檢查陣列中，是否所有元素皆符合所提供條件。

### **some**
它會檢查陣列中，是否存在任意元素符合所提供條件。

### **reduce**
這是一個累加器，它會將陣列中每項元素由左至右送入函數進行運算後回傳單一值。

```javascript
const total = people.reduce((prev,item,idex)=>{
  return prev + item.money
},0)
```
<br> 也可以拿來做比較器
```javascript
const max = people.reduce((prev,item,idex)=>{
  return Math.max(prev,item.money)
},0)
```

<br><br>

## 測驗 2：ES6 小測驗

**問題 1： const 是宣告一個常數，宣告後的變數不能隨意修改。  但是 const 所宣告的如果是物件變數，則物件內的屬性可隨意修改？**  
1. [x] 對  
2. [ ] 不對，就說不能亂改了，還想挑戰我  
<br>

**問題 2： var 與 let 的作用域分別為何？**  
1. [ ] var 是 function; let 是 block "{}"。  
2. [ ] var 是 block "{}"; let 是 function。  
3. [x] var 是全域宣告; let 是 block "{}"。  
<br>

**問題 3：在物件中使用縮寫的 function 如下：  此時的 aa.doSomething() 等同於？**  
```javascript
var aa = {
  doSomething(){ 
    ...
  }  
}
``` 
1. [x] 傳統 function 函式  
2. [ ] 箭頭函式  
3. [ ] 傳統 function 與 箭頭函式 是一樣的，所以是沒差的　  
<br>

**問題 4：使用 ES6 時不需要在意瀏覽器的支援性，用就對了**  
1. [ ] 對。  
2. [x] 錯，還是要注意一下能使用的瀏覽器，或者使用編譯工具(如: Babel)編譯。  
<br>

**問題 5：陣列方法中的 forEach 基本上要搭配 jQuery 或 Vue 才能使用。**  
1. [ ] 對，這是框架所提供的方法。  
2. [x] 不對，現在 JavaScript 已經可以使用這類型的陣列方法。   


<br><br>

## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/study-notes/computer-language/framework/2019/04/18/Vue-Study-Notes-Contents/)


<br><br>

## 參考資料
1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [鐵人賽：ES6 開始的新生活 let, const｜卡斯伯 Blog - 前端，沒有極限](https://wcc723.github.io/javascript/2017/12/20/javascript-es6-let-const/)
3. [Day26 var 與 ES6 let const 差異｜iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10209121)
4. [Day04　undefined與not defined｜iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10190904)
5. [for迴圈 setTimeout 結合一些示例(前端面試題）｜程式前沿](https://codertw.com/前端開發/231181/)
6. [立即函式(IIFE)｜維克的煩惱](http://www.victsao.com/blog/81-javascript/287-javascript-function-iife)
7. [JS基礎：Primitive type v.s Object types｜LeeBoy – Medium](https://medium.com/@jobboy0101/js基礎-primitive-type-v-s-object-types-f88f7c16f225)
8. [Deep vs. Shallow copying.](https://www.cs.utexas.edu/~scottm/cs307/handouts/deepCopying.htm)
9. [鐵人賽：箭頭函式 (Arrow functions)｜卡斯伯 Blog - 前端，沒有極限](https://wcc723.github.io/javascript/2017/12/21/javascript-es6-arrow-function/)