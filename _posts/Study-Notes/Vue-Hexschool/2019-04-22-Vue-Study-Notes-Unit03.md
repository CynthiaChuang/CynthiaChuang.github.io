---
title: 【Vue.js 學習筆記】03. 製作一個 Todo List 來小試身手吧
date: 2019-04-22
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
1. 套用版型及建立代辦事項列表的資料 
2. 刪除陣列上的特定資料 
3. 製作頁籤分類的功能 
4. 雙擊修改資料內容 
5. 刪除項目補充說明 

<!--more-->
<br>

先來看看成品長怎樣

![Imgur](https://i.imgur.com/UTcNeft.png)

<br><br>
## 套用版型及建立代辦事項列表的資料
作業模板：[傳送門](https://codepen.io/Wcc723/pen/VXRWyg)

1. **新增變數與函式**  
    - newTask：字串，用於儲存輸入框文字 
	- todoList：陣列，紀錄代辦事項，裡面為 key / value ，包含 id、taskName 與完成狀況（completed） 
	- addToList：methods function，當點擊新增按鈕時，會被觸發的函式。 <br>

2. **輸入框與變數雙向綁定**  
將變數 `newTodo` 使用 **v-model** 與 `input` 建立雙向綁定。

3. **按鈕點擊事件與函式綁定**  
將 `addToList` 與 `button` 使用 **v-on:click** / **@click** 進行綁定。

4. **將 todoList 顯示在 UI 上**  
這邊打算用 label 來顯示文字，且因每筆代辦事項之前，想會加入一個核選方塊，因此使用一個區塊標籤 div，包覆住 label 與 checkbox，並在區塊標籤上使用 **v-for='item in todoList'** 進行綁定。

5. **將每筆代辦事項的 label 和 checkbox 綁定**  
將所取出的 item 的 id 與 checkbox 和 label 的 id 屬性使用 **v-bind** 進行綁定。

6. **將 checkbox 狀態與 item 的完成狀況進行綁定**  
將 item.completed 與 checkbox 使用 **v-model** 建立雙向綁定。

7. **將事件內容顯示在 UI 上**  
將 item.taskName 使用 Mustache 語法包覆在 label 中顯示於 UI 上。

8. **實做 addToList 函式**  
取 timestamp 做為 item 的 id，並將 newTask 視為 taskName，預設完成狀況為 false，包成一筆 Object 後，使用 `array.push` 將新 Object 加入陣列尾端，最後記得清空輸入欄。

9. **支援 enter 新增**  
順便為輸入欄加上 ``@keyup.enter="addToList"`` ，讓輸入欄可以支援按 enter 提交。

10. **避免加入空的任務**  
建議可以針對輸入的內容去刪除前後的空格，並判斷是否為空字串，避免加入空字串。

<br><br>

## 刪除陣列上的特定資料

1. **新增函式並與刪除按鈕綁定**  
    建立所需函式 deleteTask 並與 delete icon 使用 `v-on` 進行綁定。使的點擊 delete icon 時，可以觸發該函式。

2. **實做 deleteTask 函式**
    可以使用 **array.splice** 刪除陣列中指定的元素，但其指定方式是使用 index ，因此必須知道欲刪除元素的 index。 Index 可以在進行 v-for 迴圈時取得，並與 item 一併傳入。
    
<br><br>

## 製作頁籤分類的功能

### 刪除線效果
在顯示任務名稱的 label 標籤上添加 class 的動態切換

```html
:class="{'completed':item.completed}"
```
<br>

### 頁簽功能
頁簽功能分成兩部份來實做，一是上方頁簽切換，二是下方資料切換。

<br>先實做上方頁簽切換的部份
1.  **新增變數**  
    新增三個變數 `allItem` 、 `processing` 與 `done`，分別對應到 card-header 的三個分頁全部、進行中、已完成。另外宣告一個變數 `visibilityTab` ，並將該值初始化為 **allItem**，此變數用來紀錄目前使用者正在查看的頁簽，
    
2. **頁簽渲染**  
    為了突顯目前正在查看的頁簽，會將該頁簽進行渲染。因為為各個頁簽加上 class 的動態切換，當 `visibilityTab` 指向該頁時，就加上 active class 的渲染效果。
    ```html
    :class="{'active':visibility == 'allList'}"
    ```
    
3. **頁簽切換效果**  
    為實做為點擊切換的效果，因此當點擊頁簽虛將將 `visibilityTab` 指向自己，例如：
    ```html
    @click="visibility = 'done'"
    ```
    一旦 `visibilityTab` 指向自己，`:class` 的判斷式就會為真，因此就會為該頁簽加上渲染效果。

　　
<br> 下方資料切換
1.  **動態過濾陣列內容**  
    因為我們不希望修改原始 `todoList` 中的資料，因此實做時會希望回傳一個新的過濾後陣列。且因為我們希望在不同頁簽會回傳不同的陣列，因此我們會在 computed 中實作該函數 `filteredList` 。
    
2. **使用回傳陣列取代原始陣列**  
    使用 `filteredList` 取代掉先 v-for 中所使用的 `todoList`。

    PS. 如果為確認 `filteredList` 與 v-for 的搭配是否正常運作，可以在 `filteredList` 中直接回傳 `todoList` 做為測試。 

3. **實作 filteredList 函數**  
    依照 `visibilityTab` 的內容回不同的過濾結果。實作如下，課程影片中老師是用 forEach 的方式，不過個人偏好 filter XD：<br>
    ```javascript
    filteredList: function(){
      if (this.visibility == "allItem"){
        return this.todos;

      }else if(this.visibility == "processing"){
        return this.todos.filter((item) => {
          return !item.completed ;
        });

      }else{
        return this.todos.filter((item)=>{
          return item.completed;
        })
      }
    }
    ```

<br><br>

## 雙擊修改資料內容
1. **在 UI 上實做輸入框架**  
    準備好一個 input text 元件，並與目前顯示內容的元件 - 也就是剛剛包住 label 與 checkbox 的 div - 放置在同一層。

2. **雙擊事件**  
    這邊希望點擊代辦事件兩下後，可以進入修入模式。因此為 **div** 元件建立一個雙擊事件。
    PS. 是加在 div 而不是新增的 text 元件，因為在一般模式下的顯示是 div，所以會收到 dblclick 事件的會是 div。
    ```html
    @dblclick="editTask(item)"
    ```

3. **實做 editTask 函數**  
    宣告三個變數 cacheTask、cacheTaskName，分別紀錄目前編輯的資料項目、與編輯中 taskName。
    
4. **顯示編輯框**  
    在顯示內容的元件 - 也就是剛剛包住 label 與 checkbox 的 div上加入
    ```html
    v-if="item.id !== cacheTask.id"
    ``` 
    並為 text 元件加上 `v-else` ，用以判斷目前應顯示內容或是 text 元件。
    
    PS. 在課程中是為 text 元件加上的也是 `v-if`，只是我覺得用 `v-else` 可讀性比較好。
    

5. **為 text 加上所需屬性**  
    如：使用 `v-model` 綁定 text 與 cacheTaskName 紀錄編輯中的內容、`@keyup.esc` 取消輸入與 `@keyup.enter` 完成輸入。

6. **取消編輯**  
    使用 `cancelEdit` 來實做取消輸入事件，當按下 esc 後，清空 cacheTask、cacheTaskName。

7. **完成編輯並更新資料**  
    使用 `doneEdit(item)` 來實做完成輸入事件，當按下 enter 後，將 item 中的 TaskName 指定為 cacheTaskName 內容，並清空 cacheTask、cacheTaskName。


<br><br>

## 刪除項目補充說明  

之前實做刪除時，是採用傳入陣列 index、並刪除所傳入 index 元素，但在 filtered 後的陣列中內容元素所在 index 與之在原始陣列中的 index 有異。

如果直接刪除所傳入 index 會出現誤刪的情況，因此改利用傳入 id 反查該筆資料在原始陣列中正確的位置，然後在進行刪除。
<br>

課程中在實做時方法有二，一是使用 **array.forEach** ，當遇到 id 同時就紀錄下目前的 index，最後在刪除所紀錄下的 index：
```javascript
removeItem: function(item){
  let removeId = -1 ;
  this.todos.forEach((todo,index)=>{
    if (todo.id === item.id){
      removeId = index;
    }
  })
  this.todos.splice(removeId,1);
},
```

<br>不過個人更偏好第二種方法：
```javascript
removeItem: function(item){
  let removeId = this.todos.findIndex((todo) => {
    return todo.id === item.id
  })
  this.todos.splice(removeId, 1);
},
```


<br><br>

## 其他連結

1. [【Vue.js 學習筆記】00. 目錄](/study-notes/computer-language/framework/2019/04/18/Vue-Study-Notes-Contents/)

2. 實戰體驗：[github](https://github.com/CynthiaChuang/vue-exercise/tree/master/Hw1-TodoList)、[codepen](https://codepen.io/cynthiachuang/pen/dLjPed)

 
<br><br> 

## 參考資料

1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)