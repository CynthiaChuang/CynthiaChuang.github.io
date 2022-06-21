---
title: 【Vue.js】Vue Router 之透過路由組件傳參數給元件
date: 2019-12-30
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- 前端
- Vue.js
--- 

一般來說，想透過路由傳遞參數給元件，多採用[**動態路由**](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#%E5%93%8D%E5%BA%94%E8%B7%AF%E7%94%B1%E5%8F%82%E6%95%B0%E7%9A%84%E5%8F%98%E5%8C%96)的方式來實作，但這種方式一則容易使得元件高度耦合，導致只能在特定 URL 上使用；二則是你永遠別低估使用者亂輸入的能力，他們常常會輸入一些超乎設計者預期的字串 XD
 
因此，我希望可以找到其他方式傳參數給元件，最後終於在文件找到[**路由組件傳參**](https://router.vuejs.org/zh/guide/essentials/passing-props.html)的相關說明。不過它的說明有些簡略，還是稍微嘗試了下才找到我要的用法。

<!--more-->

## 情境說明
嚴格來說，是因為我同時實作<mark>巢狀路由</mark>與<mark><a href="https://router.vuejs.org/zh/guide/essentials/passing-props.htmlg">路由組件傳參</a></mark>兩件事情，才使得事情變得有些複雜。

<p class="illustration">
    <img src="https://i.imgur.com/MUt7P2C.png" alt="巢狀路由（嵌套路由）示意圖">
     巢狀路由示意圖 （圖片來源: <a href="https://router.vuejs.org/zh/guide/essentials/nested-routes.html">Vue Router 官網</a>）
</p>

我的情境有點像上圖，一個頁面中存在兩個元件，分別是外層的的 menu，與主體內容的 content 元件。

只是在我的應用中，兩個主體內容的 content 元件實作方式除了 Breadcrumbs 與 Title 不同外，其餘幾乎一樣。因此將他們合而為一，再使用一個標記來區分要顯示的 Breadcrumbs 與 Title，似乎是比較好的管理方法。

但因為是有限的標記選擇—不是 Profile 就是 Posts，再加上我也不希望讓使用者修改標記，畢竟若輸入不是這兩個標記時，也算<mark>標題與內文不符</mark>了 XDD，因此我直接排除常用的動態路由做法。



## 路由組件傳參
因為剔除動態路由的關係，使得我碰到了點瓶頸。還好後來在文件找到[路由組件傳參](https://router.vuejs.org/zh/guide/essentials/passing-props.html)，它可以通過 props 解耦，這正是我需要的。

<br>

根據文件，有三種方式可以透過 props 解耦

### Boolean mode 布林模式 
布林模式的用法是跟<mark>動態路由</mark>一併使用的，首先建立一個用來顯示的元件，並設定所需的 props：

```html
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>I form {{from}}</h2>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    from: {
      type: String,
      default: ""
    }
  },
  data () {
    return {
      msg: 'Welcome'
    }
  }
}
</script>
```

<br> 

接下來再 router/index.js 的檔案中，定義相關路由與一個動態路徑參數，最重要的是<mark> 為你的命名視圖添加 `props` 選項並設置為True</mark>。

此路徑參數將直接傳遞給給該元件，需注意的是，此路徑參數需與元件中所要使用的 props 變數名稱相同，才能順利傳遞。

```javascript
export default new Router({
  routes: [
    {
      path: '/:from',
      name: 'HelloWorld',
      props: true,
      component: HelloWorld
    }
  ]
})
```
<br> 

若你的路徑參數與 props 變數名稱不相同，或是路徑參數多於 props 所定義的變數，這些<mark>無法被元件的 prop 所識別且獲取的特性</mark>，則可以在 `vm.$attrs` 中查看。

<br> 


如果你的主視圖是巢狀路由架構，則為命名視圖所添加的 `props` 需配合 `components` 一同改成物件，其 key 值則對應到在主視圖中 router-view 的標籤所定義的名字。

當主視圖 template 如下：
```htmlmixed
<template>
  <div id="app">
    <router-view name="menu"/>
    <router-view name="content"/>
  </div>
</template>
```

<br> 則路由定義如下，在路由中定義的所有動態路徑參數，將會全數傳遞這兩個元件，若參數可被該元件的 prop 所識別且獲取，則會對應到 props 變數反之則會出現 `vm.$attrs` 中。


```javascript
export default new Router({
  routes: [
    {
      path: '/:from/:id',
      name: 'HelloWorld',
      props: { menu: true, content: true},
      components: {
        menu: Menu,
        content: HelloWorld
      }
    }
  ]
})
```

<br> 若僅其中一個元件不需要接收 props，則可將將其布林模式設定為 False。

```javascript
props: { menu: false, content: true},
```


### Object mode 物件模式
若是你的應用情境不需要設定動態路由，或是像我一樣僅需傳送固定參數，則建議使用<mark>物件模式</mark>。

物件模式，顧名思義就是使用命名視圖中添加 `props` 選項來傳遞物件。且與布林模式一樣，若所傳遞的物件可被 prop 所識別，則會對應到 props 變數反之則會出現在 `vm.$attrs` 中：
```javascript
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      props: { from: "Taiwan"},
      component: HelloWorld
    }  
  ]
})
```
<br>

若是巢狀路由架構，則與布林模式一樣使用物件來為不同的 router-view 元件傳遞物件。

```javascript
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      props: { menu: {id:1}, content: { from: "Taiwan"}},
      components: {
        menu: Menu,
        content: HelloWorld
      }
    }
  ]
})
```
<br>

若是其中一個元件不需要接收 props，則可將其布林模式設定為 False，或是不將其加入 key 值。
```javascript
props: { menu:false, content: { from: "Taiwan"}}

or

props: { content: { from: "Taiwan"}}
```


### Function mode 函式模式
最後一個函式模式，顧名思義就是建立一個函式傳回 props。如此一來就可以將參數轉成你所需要的型態與名稱。

因為布林模式中 **query** 的值，是無法透過參數被傳入元件中的，因此這邊建立一個函式將 query 轉成物件傳入元件中。

```javascript
export default new Router({
  routes: [
    {
      path: '/from',
      name: 'HelloWorld',
      props: (route) => ({ from: route.query.w }),
      component: HelloWorld
    }
  ]
})
```

以這組code 為例，如過在 URL 中輸入 `/from?w=Taiwan`，將會傳入 `{from: 'Taiwan'}`，交由元件的 prop 進行識別。

<br>

也可以透過函式將參數轉成你所需要的名稱
```javascript
export default new Router({
  routes: [
    {
      path: '/:id',
      name: 'HelloWorld',
      props: (route) => ({ from: route.params.id }),
      component: HelloWorld
    }
    
```
<br>

巢狀路由架構的部分，一樣使用物件來為不同的 router-view 元件傳遞函式
```javascript
export default new Router({
  routes: [
    {
      path: '/from',
      name: 'HelloWorld',
      props: { menu: (route) => ({ id: route.query.w }),
               content:  (route) => ({ from: route.query.w })
      },
      components: {
        menu: Menu,
        content: HelloWorld
      }
    }
  ]
})
```
<br>

關閉其中一個元件的 props，與前兩個模式一樣則將它設定為 False，或是不將其加入 key 值。
```javascript
props: { menu:false, content: { from: "Taiwan"}}

or

props: { content: { from: "Taiwan"}}
```


## 參考資料 
1. [路由组件传参｜Vue Router](https://router.vuejs.org/zh/guide/essentials/passing-props.html) 
2. [API｜Vue.js](https://cn.vuejs.org/v2/api/#vm-attrs)