---
title: 【Vue.js 學習筆記】09. Vue Cli 的建置與運作原理
date: 2019-05-10
categories:
- Study-Notes
- Computer-Language/Framework
tags:
- Front-end
- Vue.js
- Udemy 
--- 

本節內容包含下述子章節：

 1.  為什麼要學 Vue Cli
 2.  Vue Cli 2.x 與 Vue Cli 3.x 課程說明
 3.  如何使用 Vue Cli
 4.  Vue Cli 所產生的資料夾結構說明
 5.  Webpack 腳本介紹 及 自定義環境變數
 6.  安裝套件在 Vue Webpack 中

<!--more-->
<br>

## 為什麼要學 Vue Cli
Vue Cli 其實是一個指令工具，用來讓我們安裝 Vue + Webpack 的環境。

### 優點
1. 是基於 Webpack 所建置的開發工具。
2. 便於使用各種**第三方套件**（BS4, Vue Router）。
3. 可運行 **Sass, Bebel** 等編譯工具。
4. 便於開發 **SPA（Single Page Application）** 的網頁。
5. 靠簡單設定就可以搭建開發時的常用環境。

### 缺點
不便於開發 **非** SPA 的網頁，若要開非 SPA 的網頁，請使用  CDN 模式。 

<br><br>

## Vue Cli 2.x 與 Vue Cli 3.x 課程說明

課程使用的是 Vue Cli 2.x ，但目前已經有 Vue Cli 3.x 的推出。在課程後面有補上 3.0 的觀念，聽說兩者差異不大！？

Vue Cli 3.x 的補充課程：13. Vue Cli 3.0 <-- 待補充


<br><br>

## 如何使用 Vue Cli
安裝步驟
1. 先安裝 [node.js](https://nodejs.org/en/)。  

2. 安裝 [Vue Cli](https://github.com/vuejs/vue-cli)，安裝確認相依資源的版號。  

3. 安裝完後可以下 Vue，確定是否有無安裝成功  

4. 若安裝成功，可以下 建立專案  
    ```shell
    $ vue init <sampleName> <projectName>
    ```

    建立專案後，可以看到 **package.json**，這是安裝套件的主檔案，會列出相依套件的內容與版號（忽然想到 Android 中 build.gradle 的 dependencies）。

6. 進入專案後，開始安裝相依的套件，這階段所花費的時間會較長。  
    ```shell
    $ npm install
    ```

7. 最後執行專案（看一下剛剛你跑完 Vue init 給的提示）。  
    ```shell
    $ npm run dev 
    ```

 
<br><br>

## Vue Cli 所產生的資料夾結構說明

<div class="note info">
下面幾節的程式架構與檔案，都是使用 webpack 這個 sampleName 所建構出來的。也就說剛剛 init 是下所產生出來的專案。
</div>

```shell
$ vue init webpack my-project
```

### **package.json**

除了在 dependencies 中，有定義相依套件外，檔案的 scripts 處有定義一些指令，例如 dev ，就是剛剛 ``npm run dev`` ，所真正執行的指令。
<br>

### **dist**
這個資料夾是在下 ``npm run dev``  時，他會將所有網頁的的內容打包壓縮而產生的。這個 build 完的檔案必須要運行 http server  才可以正常運作，直接打開資料夾中的 index.html 是不能運作的。
<br>

###  **index.html**
通常不太會動到這支 code 。所有的內容都回從 src 中打包注入到 id 為 app 的 div 下方。
<br>

### **static**
這個資料夾方的是不會被編譯的檔案。
<br>

### **src**
放的是原始碼，這個資料夾的內容是會被編譯的。
- **main.js**  
    src 下有一個 main.js，這份檔案是所有 vue.js 的進入點，也就是注入 index.html 的主檔案。

    main.js 會再 import App.vue 作為 component 使用。

- **App.vue**  
    這份檔案可以分成三個部份，上方 template 是對應先前學過得 x-template 的部份，script 是 JavaScript 的部份，定義這個元件的行文，而 style 則是 css 的部份。

<br>

### **node_modules**
這個資料夾是存放，剛剛透過 npm install 所安裝的套件。
<br>

### **config**
存放一些 vue.js 的設定。

<br>

### 其他設定檔
還會出現如：.babelrc 、 .postcssrc ...等設定檔。

<br><br>

## Webpack 腳本介紹 及 自定義環境變數

![Webpack 流程](https://i.imgur.com/bjYe2nN.png)
<center style="color:Gainsboro;"> Webpack 流程 （圖片來源: <a href="https://www.udemy.com/vue-hexschool/" style="color:Gainsboro;">課程的第 9 節， 68 講座</a>）</center>
<br>

### **Webpack 運行觀念**
Webpack 會有一個 main.js 的主要檔案，它是一個 entry 進入點，這個 main.js 會載入其他相依的內容，如：js、sass、vue。

Webpack 會監控 main.js 與相依的檔案，一旦更動就會進行編譯，而這些編譯的內容就會輸出成 .js、.cdd...等等。

此外，Webpack 有一個 loader 的工具，它會根據不同了類別、設定檔...等，決定相依的檔案該如何成現在 js 裡面。最後透過 output 輸出成最後的檔案。

以上的相關設定可以在 **build/webpack.base.conf.js** 這個 檔案中看到相關的設定。
<br>

### **環境變數**
在一開始的 config 的 folder 中，可以看到 **index.js、dev.env.js 與 prod.env.js** 三支程式。

其中 index.js 是針對整個編譯環境使用，而 dev.env.js 與 prod.env.js 則是在開 Vue 時可以讀取的環境變數，若要讀取變數名稱可以直接在 vue 檔中使用 **process.env.變數名稱** 即可，此外在修改完環境變數後需要重起環境。

<br><br>

## 安裝套件在 Vue Webpack 中

直接在專案的資料夾下，依照套件所提供的安裝指令即可，安裝完後重起環境。若是屬於在 Vue 中使用的元件，請在 main.js 中 import 該元件後，使用 **Vue.use** 宣告使用該套件，之後在程式中就可以直接使用。
 
<br><br>

## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/study-notes/computer-language/framework/2019/04/18/Vue-Study-Notes-Contents/)

<br><br>

## 參考資料
1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/) 