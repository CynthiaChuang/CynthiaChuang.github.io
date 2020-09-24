---
title: 【Android】Android SDK 與 API
date: 2016-03-18
is_modified: false
categories:
- Computer-Language/Framework
tags:
- Android
--- 

雖然 API 跟 SDK 還不至於傻傻分不清楚，但也還是迷迷糊糊的，在網路上看到幾個不錯的例子，趁著這個機會一併整理整理 XDDD
 
最後順便整理一下 Android API Level 與 SDK 的版本對照表。
<!--more-->
<br> 

## API (Application Programming Interface，應用程式介面)

API 是一個讓開發者能夠存取 Library 裡面的 functions/methods 的介面，而無須了解 function 是怎麼運作、被實作的。  
      
就我自己舉例，API 是個公司的對外窗口，裡面的 Library 則是不同 team，而 functions / methods 而則是 team 裡面的 RD，每個人可以執行不同工作，當我需要有人幫我寫程式，就去公司的對外窗口（API）去下單，之後只要等著收程式就好，你不管是哪個 team 幫你生出來的。自己舉的例子或許不是很恰當，不過大概就是類似的邏輯概念。  
      
在網路上還看到兩個有趣的例子：

-   在水面上對著湖中女神大喊「我掉的是金斧頭」，湖中女神就會丟出一把金斧頭（如果你沒說謊的話...），你不用知道到底她是怎麼找到你的斧頭的；同時，如果你不知道拿斧頭的規則（說謊），亂喊「我要金斧頭」，湖中女神就不會理你。所以可以說，湖中女神是一個 **"Library"**，有開出 **"拿斧頭的 API"**。  
    
-   API 就是皮卡丘，提供鋼鐵尾巴跟雷電兩種技能給你呼叫。你不用為什麼皮卡丘會發電，也不用研究尾巴為什麼會變鋼鐵，反正你只要說，**"上吧皮卡丘，使用雷電"**。 皮卡丘是 API 提供界面給你呼叫技能的函式庫。 

    ![上吧皮卡丘](https://imgur.com/8Hn06ql.jpg)
    <center class="imgtext"> 上吧皮卡丘，使用雷電（圖片來源: <a href="https://agirls.aotter.net/post/52299" class="imgtext">電獺少女</a>）</center> 

<br><br>

## SDK (Software Development Kit，軟體開發工具包)
是用來幫一個 **產品** 或 **平台** 開發應用程式的工具組，由產品的廠商提供給開發者使用的，通常是某一家廠商針對某一平台、系統或硬體所發布出來用以開發應用程式的工具組，在這個工具包裡面，可能包含了各式各樣的開發工具，模擬器、API 等。  
      
例如：Android 針對不同的版號有不同的 SDK，因此在 Android Studio 進行開發時需要針對你的平台安裝不同的 SDK，而在 SDK 會提供相對應 API 以供使用。  
      
<br><br>

## Android API Level 與 SDK 的版本對照表
目前市面上常見的是 API15 以上的機種，所以這邊就從 15 開始列，N 的部份就先不列了。  

|      API       |版本號                          |版本名稱                     |
|----------------|-------------------------------|-----------------------------|
|API-15          |Android 4.0.3 - 4.0.4          |Ice Cream Sandwich           |
|API-16          |Android 4.1                    |Jelly Bean                   |
|API-17          |Android 4.2                    |Jelly Bean                   |
|API-18          |Android 4.3                    |Jelly Bean                   |
|API-19          |Android 4.4                    |KitKat                       |
|API-20          |Android 4.4W–4.4W.2            |KitKat, with wearable extensions|
|API-21          |Android 5.0-5.0.2              |Lollipop                     |
|API-22          |Android 5.1–5.1.1              |Lollipop                     |    
|API-23          |Android 6.0–6.0.1              |Marshmallow                  |   
       
<br><br>

## 參考資料
1. [[Dev] IDE, API, SDK, Library 基本術語解釋｜Andro Chen 陳俊安的開發筆記](http://androchen.logdown.com/posts/2014/04/13/api-sdk-library)
2. [API ? SDK? 傻傻分清楚｜Jyny Chen Blog](http://blog.jyny.tw/2013/01/api-sdk.html)
3. [Android API Level与sdk版本对照表｜今天又进步了  博客园](http://www.cnblogs.com/wawahaha/p/4916196.html)
 