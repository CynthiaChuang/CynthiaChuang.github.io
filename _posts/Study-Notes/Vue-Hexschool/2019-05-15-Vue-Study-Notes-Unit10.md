---
title: 【Vue.js 學習筆記】10. Vue Router
date: 2019-05-15
modified: 2019-05-15
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
 
1. 使用 Vue Router 及配置路由文件
2. 新增路由路徑及連結
3. 製作巢狀路由頁面
4. 使用動態路由切換頁面 Ajax 結果
5. 命名路由，同一個路徑載入兩個頁面元件
6. Vue Router 參數設定
7. 自定義切換路由方法

<!--more-->
<br>

## 使用 Vue Router 及配置路由文件

Vue Router 是由前端所模擬的網頁路由技術，可以讓使用者透過網址，決定要顯示的頁面。而非傳統網頁，一個網址就是一個頁面。使用時所輸入網址內容，會在前端程式判定所輸入的條件轉譯成搜尋條件，返回相對應的網頁。

<br> 

### 安裝與配置流程

0. **事前準備**  
    請先閱讀[參考文件](https://router.vuejs.org/zh/)

1. **Vue Router**  
    在專案下輸入安裝指令
	```shell
	$ npm install vue-router --save
	```
    
	指令最後的 `-save` 或是 `save-dev` 是指自動將把套件稱和版本號添加到 package.json 文件中 dependencies / devdependencies 部分。
    
    一般來說，為專案添加套件時必須先進行安裝，即在專案下輸入安裝指令，然後連同版本號手動將他們添加到配置文件 package.json 中的 dependencies 裡
    ```shell
    $ npm install vue-router
    ```
    
    若在指令中添加 `-save` 或是 `save-dev` 可以省掉你手動修改的 package.json 文件的步驟。

2.  **路由的配置**  
    添加 **src/router/index.js**，當作前端路由的配置檔案，決定哪個網址讀取哪份檔案。
    ```javascript
    // 引入官方元件
    import Vue from 'vue'
    import VueRouter from 'vue-router'  

    // 引入自定義元件
    import Hello from '@/components/HelloWorld.vue'   

    // 啟用 VueRouter 元件
    Vue.use(VueRouter)

    // 匯出給 entry 使用
    export default new  VueRouter({  

    });
    ```
	<br>
3. **引入Vue Router**  
    在 **entry**（src/main.js）匯入 Vue Router，並在 Vue 元件中引入該物件。
	```javascript
	import Vue from 'vue'
	import App from './App'
	import router from "./router" 
	 
	new Vue({
	  el: '#app',
	  components: { App },
	  template: '<App/>',
	  router,
	})
	```
	<br>我自己在練習的時候在這步驟卡好久，一直得到下面 error

    ![Imgur](https://i.imgur.com/xjcYc56.png)

	<br>找了一陣子才發現，我一開始寫的時候是用 
    ```javascript
    import Router from "./router" 
    ``` 
    
    只要把它改成
    ```javascript
    import router from "./router"
    ``` 
    下面 Vue 元件中引入該物件的地方也把 **Router** 換成 **router** 就好了。  
  
	就是不曉得為什麼這樣就 work 了？ 
    
4. **定義路徑**  
    回到 src/router/index.js 定義路徑，在 VueRouter 元件中新增一個名為 **routes** 的 **陣列** ，裡面包含數的定義好的路徑物件。
	```javascript
	// 匯出給 entry 使用
	export  default  new  VueRouter({
	  routes:[{
	    name:"HomePage", // 元件呈現的名稱
	    path:"/Hello", // 對應的虛擬路徑
	    component:  Hello  // 對應的元件
	  }],
	});
	```
	<br>
	
5. **加上 router-view**  
    回到 **App.vue** 中更改 template，在 template 中加上 router-view 的標籤（可以順便 mark 掉原先的 HelloWorld 標籤）
    ```html
    <template>
      <div id="app">
        <router-view></router-view>
      </div>
    </template>
    ```

6. **測試**
    在網址中加上 Hello，就可連到所定義的元件。<br>
 


<br><br>

## 新增路由路徑及連結

這邊會試著加入一個分頁，並新增一個導覽列來切換兩個分頁

0. **引入 [Bootstrap](https://getbootstrap.com)**  
    在開始之前，先在 index.html 的 header 中引入 [Bootstrap](https://getbootstrap.com)。
    ```html
    <link  rel="stylesheet"  href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"

    integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T"  crossorigin="anonymous">
    ```

    <div class="alert warning">
    <div class="head">注意</div>
    是 Bootstrap 不是 <a herf="https://bootstrap-vue.js.org/">Bootstrap Vue</a>！<br>
    是 Bootstrap 不是 <a herf="https://bootstrap-vue.js.org/">Bootstrap Vue</a>！<br>
    是 Bootstrap 不是 <a herf="https://bootstrap-vue.js.org/">Bootstrap Vue</a>！<br>
    </div>

	<br>

1. **新增頁面**  
    在 components 的資料夾中新增一個新的元件命名為 page.vue，並將它與 HelloWorld 元件做出差異。
    ```html
    <template>
      <div class="hello">
        <div class="card"  style="width: 18rem;">
          <div class="card-body">
            <h5 class="card-title">Card title</h5>
            <p class="card-text">example.</p>
            <a href="#" class="btn btn-primary">Go somewhere</a>
          </div>
        </div>
      </div>
    </template> 

    <script>
    export default {
      data () {
        return {
        }
      }
    }
    </script>
    ```
	<br>這邊用 Bootstrap 的 card 元件做 page.vue，可以直接照[範例](https://getbootstrap.com/docs/4.3/components/card/)貼上就好。如果不想這麼麻煩，直接在 template 裡面塞一段文字也行，分得出 HelloWorld 與 Page 就可以了。

2. **為新頁面配置網址**  
    在 **src/router/index.js** 中添加新增 page.vue 
    ```javascript
    // 引入官方元件
    import Vue from 'vue'
    import VueRouter from 'vue-router'

    // 引入自定義元件
    import HelloWorld from '@/components/HelloWorld.vue'
    import Page from '@/components/page/page.vue'

    // 啟用 VueRouter 元件
    Vue.use(VueRouter) 

    // 匯出給 entry 使用
    export default new VueRouter({
      routes: [
        {
          name: "HelloWorld", // 元件呈現的名稱
          path: "/Hello", // 對應的虛擬路徑
          component: HelloWorld  // 對應的元件
        },
        {
          name: "Page",
          path: "/Page",
          component: Page
        }
      ],
    });
    ```
	<br>
    加上後可以試著在 url 上分別加上 /Hello 或 /Page 確定是否能夠順列切換。
    
	
    PS. page.vue 是放在 components 下一個名為 page資料夾裡面，所以引入路徑中寫成 ``@/components/page/page.vue``。
    	
3. **新增個頁簽**   
    在 App.vue 中加上 Bootstrap 的 [Navbar](https://getbootstrap.com/docs/4.3/components/navbar/)，並新增兩個頁簽。
    ```html
    <template>
      <div id="app">
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
          <a class="navbar-brand" href="#">Navbar</a>
          <div class="collapse navbar-collapse"  id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
              <li class="nav-item">
                <router-link class="nav-link" :to="{name:'HelloWorld'}">Hello</router-link  >
              </li>
              <li class="nav-item">
                <router-link class="nav-link"  to="/Page">Page</router-link>
              </li>
            </ul>
          </div>
        </nav>
        <img src="./assets/logo.png">
        <router-view></router-view>
      </div>
    </template>
    ```
    

    其中程式碼中 **router-link** 是切換簽斷路由的標籤，可以直接給定定義好的完整 url
        
    ```html
    <router-view to="/Page"></router-view>
    ```
    
    或者給定定義的命名好的路由物件，注意傳物件的時候要加上 v-bind。 
            
    ```html
    <router-view:to="{name:'HelloWorld'}"></router-view>
    ```
    
        
    <br>
    
	除了聲明式外，也可在程式中使用 **router.push** 方法 進行跳轉。
    
    ```javascript
    this.$router.push('/Page');
    
    或 
    
    this.$router.push('{name:"HelloWorld"}');
    ``` 
    
    <br>
	比起輸入完整的 url，個人更喜歡傳送路由物件，我覺得可讀性更高，程式維護起來也比較容易，尤其當你還需要傳遞 params 與 query 時，還可以避免做字串串接。
    	
	PS. 是說如果覺得 Navbar 不貼頂很奇怪，可以修改下面的 style，把 **margin-top** 移除掉。
	

<br><br>

## 製作巢狀路由頁面

![巢狀路由（嵌套路由）示意圖](https://i.imgur.com/MUt7P2C.png)
<center class="imgtext"> 巢狀路由（嵌套路由）示意圖 （圖片來源: <a href="https://router.vuejs.org/zh/guide/essentials/nested-routes.html" class="imgtext">Vue Router 官網</a>）</center>
<br>

巢狀路由比較適合用在，只變換主體內容的元件其餘元件（如上方 Navbar）不變的情況。這樣可以專心撰寫主體內容的元件，並大幅減少重複其餘元件的重複撰寫。
<br><br>

1. **實作個頁面**  
    試著在剛剛製作出來 page.vue 中加上 **\<router-view\>** ，並新增另外三個子原元件。 
    - page.vue
        ```html
        <template>
          <div class="hello">
            <div class="card" style="width: 18rem;">
             <router-view></router-view>
            </div>
          </div>
        </template>
        ```

    - card_X.vue

        ```html
        <template>
          <div>
            <div class="card-body">
              <h5 class="card-title">Card X</h5>
              <p class="card-text">Card X.</p>
              <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
          </div>
        </template>
        ```
        <br> 這邊為了做出來漂亮，因此將 card 中的 card-body 抽出來放到各個子元件中，如果偷懶的話，其實可以不再抽，把 \<router-view\> 放在卡片下方，子元件中隨便塞一行字就好，你自己分的出來有切換就好。<br>	
    <br>	

2. **設置路由**  
    在 **src/router/index.js** 中添加新增這幾個元件，並在要配置巢狀路由元件中使用 ``children`` 配置， 

    ```javascript
    import Card1 from '@/components/page/card1.vue'
    import Card2 from '@/components/page/card2.vue'
    import Card3 from '@/components/page/card3.vue' 


    {
      name: "Page",
      path: "/Page",
      component: Page ,
      children:[
      {
        name: "card1",
        path: "", // path 為空，父元件預設帶入
        component: Card1
      },
      {
        name: "card2",
        path: "card2",
        component: Card2
      },
      {
        name: "card3",
        path: "card3",
        component: Card3
       },
     ]
    }
    ```
	<br> 
    
    注意在設定子元件的路徑時，前方不需要再加入 **/** 了。若 children 的路徑為空，代表父元件預設為帶入這個子元件。各個元件詳細路徑如下
	- Card1：/Page
	- Card2：/Page/card2
	- Card3：/Page/card3
		
	

<br><br>

## 使用動態路由切換頁面 Ajax 結果

一種滿常見的應用情境，尤其在如商品、新聞...等，大量使用相同版型的應用場景中最常使用。在使用時會設定特定的匹配模式，將符合該匹配模式的路由，全都導向特定同個元件。

<br>

![博客來商品頁 ](https://i.imgur.com/9JZRISV.png)
<center class="imgtext"> 博客來商品頁 （圖片來源:  <a href="https://www.books.com.tw" class="imgtext">博客來</a>）</center>
<br>


以博客來為例，在商品介紹頁中皆使用了相同版型，並仔細觀察相對應的網址，可以發現網址皆為 https://www.books.com.tw/products/商品ID 的樣式。

在 Vue 中這樣的情況可以採用動態路由來實做，將符合 products/:id 這樣的路由，全導向特定商品介紹的元件，而實際載入的商品資料由 id 來控制。

<br>
這邊以使個人資料卡為例

0. **安裝 vue-axios**  
    開始前先安裝 vue-axios，這是來進行 ajax 請求的套件，住要是為取得一些隨機個人資料
    ```shell
    $ npm install --save axios vue-axios
    ```
    <br> 安裝完成後回到 entry（src/main.js）匯入 vue-axios
    
    ```javascript
    import  axios  from  'axios'
    import  VueAxios  from  'vue-axios'
    Vue.use(VueAxios, axios)
    ```
    
	<br>
	
1. **設定動態路由**  
    在 **src/router/index.js** 中去設定動態路由匹配模式。使用以冒號開頭動態路徑參數，例如：
    ```javascript
     { path: '/user/:id', component: Card3 }
    ```

    去宣告一個路徑參數，當 url 匹配到一個路由時，這個參數值會被設置到 **this.\$route.params**。

    <br>

2. **實作資料讀取**

    回到卡片元件中，使用 vue-axios 去讀取 [user](https://randomuser.me/documentation) 的資料。
    ```javascript
    created(){
      const id = this.$route.params.id;
      this.$http.get(`https://randomuser.me/api/?seed=${id}`).then((res)=>{
        const personalData = res.data.results[0];
        this.name = personalData.name;
        ...
      })
    },
    ```
	<br> `https://randomuser.me/api/?seed=${id}` 這是由 randomuser 所提供的 API，給定一個 seed 後，會回傳特定使用者資料，而所送出的 seed 是取自 **this.\$route.params.id**，也就剛剛所設定的動態路徑參數。
    <br>	
	this.\$http.get 是一個非同步的動作，最終取得一個 Promise ，必須再呼叫 then 將個人資料取出放入變數，最終將變數在 UI 中呈現。		
		
3. **測試**  
    實做完成後可以使用 Page/card/:id 來切換不同的 user 資料。



<br><br>

## 命名路由，同一個路徑載入兩個頁面元件

![website structure](https://i.imgur.com/deMmnkG.png)
<center class="imgtext"> website structure （圖片來源:  <a href="https://commons.wikimedia.org/wiki/File:Plan_html_5.png" class="imgtext">wikimedia</a>）</center>
<br>

如果時候想同時（同級）展示多個元件，例如建置一個 layout 時，會存在 sidebar 、 header 、 footer ...等元件，這時可以使用命名路由來載入，即在 layout 中添加命名 router-view。

一般來說，如果router-view沒有設置名字，那麼預設為 ``default``。

```html
<template>
  <div class="hello">
    <router-view name="menu"></router-view>
    <div class="card" style="width: 18rem;">
      <router-view></router-view>
    </div>
  </div>
</template>
```

<br> 

並在 router 設定相對定的設置。一個路由若要選染多個元件，並需改用 ``components``（有 s）
 
```javascript
{
  name: "Page",
  path: "/Page",
  components: {
    default: Page,
    menu: menuComponent,
  },
  children:[...]
}
```

<br><br>

## Vue Router 參數設定
自己看[文件](https://router.vuejs.org/zh/api/#routes)

<br><br>

## 自定義切換路由方法

除了前面課程所教過使用聲明式 router-link 進行路由切換外，也可以使用[文件](https://router.vuejs.org/zh/api/#router-aftereach)中提供的其他方法進行跳轉。

需注意的是，在實務中要使用文件所提到 method ，如：``router.push``，需加上前綴變成 ``this.$router.push`` 才能成功調用。

<br> 課堂發問區有人提問：**請問老師為什麼我們的router前面還要多補一個\$呢?**
> 講師回覆：因為我們是在 CLI 內運行  
> 前方有先執行了 vue.use(....)
>
>因此外部資源會被掛載到 vue 內  
>就可以使用 this 來呼叫這些資源（包含 router 及接下來的 axios 都是這個概念）


<br><br> 
文件中常用的 method 如下：
- **router.push**  
跳轉到指定路由，並將此動作加到跳轉紀錄中中。如果使用 stack 來想像這個行為，就是一個 Push into the history stack 的概念。
<br>

- **router.back**  
回到上一頁。可想像成是一個 pop from history stack，另外這邊的 pop 方法可以想像成是使用移動 top 標籤的那種實做方式。
<br>

- **router.go**  
回到指定動作。可想像成是一個 top 的移動到指定位置去讀 history stack。
<br>

- **router.forward**  
回到下一頁。可想像成是一個把 top 標籤加 1 後讀取 history stack 。
<br>

- **router.replace**  
跟 router.push  類似一樣會跳轉到到指定路由，但會取代掉最上層的紀錄。可想像成他先 pop history stack 最上層的紀錄後，在 Push 新的紀錄到 the history stack 。
<br>

PS. stack 中用到的名稱可以參考 [Stack: 以Array與Linked list實作](http://alrightchiu.github.io/SecondRound/stack-yi-arrayyu-linked-listshi-zuo.html)


<br><br>

### 其他設定檔
還會出現如：.babelrc 、 .postcssrc ...等設定檔。

<br><br>

## 其他連結
1. [【Vue.js 學習筆記】00. 目錄](/Vue-Study-Notes-Contents/)

<br><br>

## 參考資料

1. [六角學院-Vue 出一個電商網站｜Udemy](https://www.udemy.com/vue-hexschool/)
2. [Vue router 實戰｜《Chris 技術筆記》](https://dwatow.github.io/2018/05-20-vuejs/vue-router-action/)
3. [node.js - npm 安装参数中的 --save-dev 是什么意思｜SegmentFault 思否](https://segmentfault.com/q/1010000000403629)
4. [Stack: 以Array與Linked list實作｜alrightchiu](http://alrightchiu.github.io/SecondRound/stack-yi-arrayyu-linked-listshi-zuo.html)
