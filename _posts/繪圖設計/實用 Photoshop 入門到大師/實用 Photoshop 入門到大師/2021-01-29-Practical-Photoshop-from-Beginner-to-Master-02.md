---
title: 【實用 Photoshop 入門到大師】02. 圖層裡的奧秘
date: 2021-03-08 00:05
is_modified: false
categories:
- "繪圖設計與藝術 › 平面設計"
- "資訊科技 › 開發與輔助工具"
tags:
- 課程
- Hahow
- 實用 Photoshop 入門到大師
- Photoshop
- 讀書筆記
- 編修與繪圖工具
- 工具介紹與操作
--- 

本節內容包含下述子章節：
1. 圖層的原理
2. 圖層的操作
3. 圖層管理的方法 & 快速選取圖層

<!--more-->
<p class="paragraph-spacing"></p>

在課程開始前我看了 [Adobe Photoshop 使用手冊](https://helpx.adobe.com/tw/photoshop/using/layer-basics.html)中關於**圖層**介紹。

在這份文件中，它說到圖層就像一張張**透明片**堆疊在一起，我們可以透過圖層的透明區域，看到下面的圖層。且，也可以透過抽換這些透明片的前後順序，還改變物件排列與可視的範圍。

這麼做的好處，是可以分開管理每一層的物件，彼此是互不干擾的，最終呈現出一個完整的作品。
 
<p class="illustration">
    <img src="https://i.imgur.com/raHCeJX.png" alt="Photoshop 圖層圖示">
    Photoshop 圖層圖示（圖片來源: <a href="https://helpx.adobe.com/tw/photoshop/using/layer-basics.html">Adobe Photoshop 使用手冊</a>）
</p>


 
## 圖層原理 
就像前面所說的透明片例子一樣，每個圖層是以**疊加**的方式來呈現。

在老師給的練習檔當中可以看到，一個圖像會由數個的圖層疊加而成：

<p class="illustration">
    <img src="https://i.imgur.com/mY6hy1u.png" alt="練習檔截圖">
</p>

而圖層間的關係由 3D 立體圖來看會是這樣的呈現：

<p class="illustration">
    <img src="https://i.imgur.com/oViVVvI.png" alt="圖層原理_3D">
    圖層原理_3D（圖片來源: <a href="https://hahow.in/courses/5b26587f67ff51001e25cf0a/assignments?item=5b34d81791606b001eb9d6e7&panelMode=PLAY_LIST">課程教材</a>）
</p>

除了疊加的規則外，圖層中另一件重要的規則是，當圖層中的圖像若出現**交集**，上方圖層會<mark>覆蓋</mark>下方圖層。

以上面的練習檔為例，因為月亮圖層覆蓋在漸層圖層之上，所以才會在漸層色板呈現月亮，而未交集處仍呈現漸層顏色，如下圖左；若是將圖層反過來，只會看到漸層色板，因為它可以遮擋掉整個月亮，如下圖右。

<p class="illustration">
    <img src="https://i.imgur.com/h6epxP1.png" alt="交集">
</p>

若是兩圖層間沒有任何交集，則圖層順序可以隨意擺放，並不會對最終成像產生影響：
 
<p class="illustration">
    <img src="https://i.imgur.com/VRLFWn1.png" alt="不交集">
</p>



## 圖層的操作
本節介紹圖層的新增、複製、刪除、合併、選取、取消選取與移動圖層中的物件等功能。

1. **新增**  
    可透過快捷鍵：`Ctrl` + `Shift` + `N` ，或是點擊圖層面版的最下方的圖示來新增圖層。
    <p class="illustration">
        <img src="https://i.imgur.com/IbtjPhL.png" alt="點擊新增圖層圖示">
    </p>
    
    一般來說，新增圖層面版中的**顏色**通常不會在此階段就做顏色標示，而**模式**與**透明度**一般常見的初始值為正常與一百。
    <p class="illustration">
        <img src="https://i.imgur.com/GXh6bn7.png" alt="新增圖層面版">
    </p>
    
2. **複製**  
    它的快捷鍵有點麻煩，需要配合滑鼠動作，按下 `Alt` 並用滑鼠左鍵點擊圖層不放，同時向上拖拉，這時應該會出現一個殘影，表示已經進入複製的狀態，將複製的圖層拖曳至想要的順序。
    
    或者也可以點選圖層後按右鍵開啟選單，選擇複製圖層。不過，這樣複製出來的圖層，預設是在被複製圖層的上方，若要改變順序，還是需要再次拖曳。
    
3. **刪除**  
    點選欲刪除的圖層後，按下 `delete`，或是點擊圖層面版的最下方的垃圾桶圖示。
    <p class="illustration">
        <img src="https://i.imgur.com/r2r1Vtl.png" alt="刪除圖層面版">
    </p>

    另一種刪除法是用拖曳的，點擊欲刪除的圖層不放，將它向下拉到這個垃圾桶的圖示上。不過，自己覺得都是點擊、向下找垃圾桶的動作，我寧願用點擊垃圾桶圖示來刪除，至少不用按著不放。
    
    另外，在右鍵選單中也有刪除圖層的選項。

4. **合併**  
    如果要合併的圖層是連續的，先點選第一個想要的圖層後，再按下 `Shift` 不放，並點選最後一個想要的圖層，放開 `Shift` 後按下 `Ctrl` + `E` 合併圖層；或是點選右鍵選單中合併圖層選項。
    
    如果要合併的塗層不是連續的，則在選取圖層的過程中，按著 `Ctrl` 不放，再依序想要圖層，再將塗層合併。
    
    是說，這應該是一般選檔時就會用到的按鍵？
    
5. **選取圖層物件**  
    按著 `Ctrl` 不放，並點擊**圖層物件縮圖**，就可以選取到圖層中的物件。

    <div class="alert danger">
    <div class="head">注意</div>
    是點取圖層物件的縮圖，就是下圖中的紅色區域，不是圖層喔。如果點選圖層，就會變成剛剛的選取不連續圖層了。      
    <p class="illustration">
        <img src="https://i.imgur.com/4OxLxQO.png" alt="圖層物件的縮圖">
    </p>
    </div>

    <p class="paragraph-spacing"></p>
    
6. **取消選取圖層物件**  
    若要取消選取，則按下 `Ctrl` + `D`。
    
7. **移動圖層中的物件**  
    若要移動圖層中的物件，點選該圖層後後，再點擊右方工具列中的**移動工具**，就可以移動該圖層中的物件。反之，若物件不在該圖層中，是無法移動的。
    
    <p class="illustration">
        <img src="https://i.imgur.com/m7qu401.png" alt="移動工具">
    </p>



## 圖層管理的方法 & 快速選取圖層
一個完整的作品會由許多的圖層所構成，一旦作品越複雜使用的圖層會越多，因此如何管理與快速選取所需圖層就越發重要。


### 圖層管理
圖層管理的方法有兩種：
1. **群組管理**  
    先使用上一節提到選取圖層的方法，將要歸類在一起的圖層選取起來，再使用快捷鍵 `Ctrl` + `G`，就會將所選取的圖層放入一個資料夾群組。點擊群組兩下，就可以針對群組命名，效果如下：
    <p class="illustration">
        <img src="https://i.imgur.com/lyoYatw.png" alt="資料夾群組">
    </p>
    
    是說，我也試著找過如何不用快捷鍵來建立資料夾，目前只找在在圖層面板的下方有建立群組的按鈕，但發現它只能幫你建立資料夾，圖層必須靠拖曳的方式手動分類。
    <p class="illustration">
        <img src="https://i.imgur.com/sBSfiOl.png" alt="建立群組的按鈕">
    </p>
        
    如果想解開群組，則按下 `Ctrl` + `Shift` + `G`。可以點選群組後按右鍵，選擇刪除群組，不過用這方法刪除時要小心，它會問你要刪除群組及其內容，或是只刪除群組，別選錯了。
 
2. **顏色標示區分**  
    如果不想使用群組管理，或是想在群組進一步分類，則可以使用顏色來標示。
    
    一樣先將要歸類在一起的圖層選取起來，在選取的任一圖層前方點擊右鍵選取顏色：
    <p class="illustration">
        <img src="https://i.imgur.com/7Kg3zxq.png" alt="顏色標示">
    </p>


### 快速選取圖層
之前是透過圖層選取物件，這邊則是透過物件選取圖層。在物件上，按下右鍵，此時會出現所有的圖層名稱，直接點選所要的圖層名稱，一旁的圖層面板就會選取相對應的圖層。
<p class="illustration">
    <img src="https://i.imgur.com/aM0ruBe.png?1" alt="快速選取圖層">
</p>



## 其他連結
1. 實用 Photoshop 入門到大師筆記：[目錄](/Practical-Photoshop-from-Beginner-to-Master-Contents) / [另開書籍模式閱讀](/Practical-Photoshop-from-Beginner-to-Master)



## 參考資料 
1. Adobe Photoshop (2020-06-23)。[了解 Photoshop 的圖層基礎概念](https://helpx.adobe.com/tw/photoshop/using/layer-basics.html) 。檢自 Adobe Photoshop 使用手冊 (2021-01-15)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-03-08</summary>
  <ul>
    <li>2021-03-08 發布</li>
    <li>2021-01-29 完稿</li>
    <li>2021-01-15 起稿</li>
  </ul>
</details>

