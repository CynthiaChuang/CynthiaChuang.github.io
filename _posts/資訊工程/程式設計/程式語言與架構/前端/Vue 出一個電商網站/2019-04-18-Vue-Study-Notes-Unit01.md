---
title: 【Vue.js 學習筆記】01. 相關資源與工具設置
date: 2019-04-18
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

這邊先列出在第一階段學習過程中，所會使用到相關資源與工具。

<!--more-->



## Vue 官方文件
[官網](https://vuejs.org/index.html)上有提供相關[文件](https://vuejs.org/v2/guide/)可供學習。



## 開發工具
課程中講師使用的是 [Visual Studio Code](https://code.visualstudio.com/) 搭配延展模組 [vue](https://marketplace.visualstudio.com/items?itemName=jcbuisson.vue)、[Vue 2 Snippets](https://marketplace.visualstudio.com/items?itemName=hollowtree.vue-snippets) 與 [Preview on Web Server](https://marketplace.visualstudio.com/items?itemName=yuichinukiyama.vscode-preview-server)。

其中，vue 與 Vue 2 Snippets 是提供 Vue 的 Highlight 與 Snippets 功能，算是第二階段才會使用的的工具主要作用在 .vue 檔上。

而 Preview on Web Server 顧名思義就是在瀏覽器上預覽網頁，不過我在試著啟動這個套件時遇到錯誤訊息：
```shell
command 'extension.launch' not found
```
所以我直接改用另一個套件 [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)。

<br class="big">

是說，關於 Vue 延展模組有發現另一套星星更多的 [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)，它的功能看起來是 vue + Vue 2 Snippets 的合併版，也可以拿來試試看。

不過因為之前是使用 Android Studio 與 IntelliJ IDEA 這 JetBrains 系列的 IDE，所以我開發環境可能會選擇同系列的 [Webstorm](https://www.jetbrains.com/webstorm/)。



## 網頁除錯工具
因為 Vue.js 是以資料驅動的一種架構，因此開發過程需要觀察當時資料是否符合預期。

除了直接透過 console 觀察資料外，也可以透過 [Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd) 來觀察甚至修改資料，相關操作可以看這篇[教學](https://flaviocopes.com/vue-devtools/)。

因為可以直接修改資料，建議網站上線前務必關閉除錯功能。
```javascript
/* 關閉vue-devtools */ 
Vue.config.devtools = false; 

/* 關閉錯誤警告 */
Vue.config.debug = false;
```



## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/Vue-Study-Notes-Contents/)



## 參考資料
1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [Vue.js｜Vue.js](https://vuejs.org/index.html)  
3. [Vue.js DevTools Tutorial](https://flaviocopes.com/vue-devtools/)