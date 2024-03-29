---
title: 【Vue.js】 DOM 的事件傳遞機制： capture 與 propagation
date: 2019-04-24
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- 前端
- Vue.js
- DOM 
--- 

在 [04. 進階模板語法介紹](/Vue-Study-Notes-Unit04/)時，遇到 **@click.stop** 與 **@click.capture** 兩個事件修飾符，讓我有點疑惑事件到底是怎麼傳遞的？

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/81zMIRH.png">
    event flow（圖片來源: <a href="https://www.w3.org/TR/DOM-Level-3-Events/">w3c</a>）
</p>

這張是 [w3c 介紹 event flow](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow) 的圖，我覺得超清楚的，可以看到整體步驟被分成三個階段。



## 第一階段：CAPTURING_PHASE
當你點擊一個 `td`，可以看到事件的傳遞會先從 `window` 開始往下傳，一路傳到 `td` 為止。也就是紅色那條線，這階段被稱為 `CAPTURING_PHASE`，直譯就是捕獲階段。而 **@click.capture** 就是在此階段運作。



## 第二階段：TARGET_PHASE
第二階段就是藍色的 `TARGET_PHASE`， 這階段會觸發目標元件本身所定義的點擊事件，也就是 **@click** 本身。



## 第三階段：BUBBLING_PHASE
最後一個階段則是綠色那一條線 - ``BUBBLING_PHASE``，事件會再往上從子節點一路往其父元素傳遞，直到最後傳回根節點 `window`，就像泡泡浮上水面一樣，也是大家比較常見的冒泡階段了。而 **@click.stop** 就是在阻止此階段的傳遞。
 


## 參考資料
1. [DOM 的事件傳遞機制：捕獲與冒泡｜TechBridge 技術共筆部落格](https://blog.techbridge.cc/2017/07/15/javascript-event-propagation/)
2. [UI Events｜w3c](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow)