---
title: 【實用 Photoshop 入門到大師】01. 界面介紹
date: 2021-02-01 21:38
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
1. 打開 Photoshop 的方式
2. 打開新檔和開起舊檔
3. 界面介紹
4. 不同種的影像顯示方式

<!--more-->


## 打開 Photoshop 的方式
有兩種打開的方式：
1. 點擊兩下桌面圖示。
2. 將要開啟的檔案（圖檔或是 psd 檔皆可）拖拉到 Photoshop 圖示上方後放開滑鼠。

<p class="illustration">
    <img src="https://i.imgur.com/BzGHgpf.jpg" alt="打開 Photoshop 的方式">
</p>



## 打開新檔和開起舊檔
1. **開新檔案**  
    點選 `檔案` → `開啟新檔`，會跳出新增的對話框。

    <p class="illustration">
    <img src="https://i.imgur.com/DjxiuuH.jpg" alt="開新檔案">
    </p>

    幾個需要特別注意的設定：
    1. **寬/高單位**：基本上，螢幕顯示多是以像素為單位。
    2. **解析度**：如成品是用於<mark>螢幕顯示</mark>，解析度應設定為 **72**；若是用於<mark>印刷</mark>，則解析度需設定為 **300** 到 **350**。
    3. **色彩模式**：一樣分成螢幕顯示與印刷，螢幕顯示設為 **RGB**，印刷設為 **CMYK**。而位元的部分，越高的位元，儲存顏色的能力越高。
    4. **背景內容**：可填黑色或者是白色，一般都是白色。

    <p class="paragraph-spacing"></p>

2. **開啟舊檔**      
    點選 `檔案` → `開啟舊檔`，接著點選你想要開啟的檔案。
 
 
### 解析度
大學在電腦影像處理這門課裡，是有學過關於解析度的定義，不過不太確定在不同的領域中，所使用的定義及單位是否有所不同，因此還是稍微找一下相關的說明。

<p class="paragraph-spacing"></p>

在開新檔案時，可以注意到預設的單位為 **Pixels/Inch**，也就是每英寸像素（Pixels Per Inch，**PPI**），又被稱為像素密度，用來表示<mark>每英寸可以有多少個像素值</mark>。一般來說，解析度會直接影響圖形的細緻程度。

另外還有一個詞是每英寸點數（Dots Per Inch，**DPI**），之前在做 APP 的時候常用來描述設備的解析度。不過在繪圖與平面設計的領域，這個詞使用來描述<mark>每英寸有多少個列印點數</mark>。

<p class="paragraph-spacing"></p>

從定義上來說，**像素**這個詞只存在電腦螢幕顯示領域，而**點**只用於印刷領域。不過在這領域中， PPI 和 DPI 似乎出現了混用的情況，在灣得文創的[介紹](https://www.wonder-product.com/pages/digital-image)中曾經提到：

> 現在數位影像領域中，像素（pixel）輸出後被稱之為點（dot），所以解析度縮寫成PPI 或 DPI 都可以。


這現象其實在 Photoshop 建立新檔的過程，或許也是可見一斑。在建立過程中只有兩個單位： **Pixels/Inch** 與 **Pixels/Centimeter**。所以即便是以印刷為目的的設定，解析度的選擇依然是以像素為單位。

<p class="paragraph-spacing"></p>

另外附上，找到的一些常用解析度設定：
- **預設的解析度**： 通常設置 72 PPI，這能滿足一般顯示器的解析度。
- **網頁圖像**： 通常設置 72 PPI 或 96PPI。
- **報紙圖像**： 通常設置 120 PPI 或 150 PPI。
- **彩版印刷圖像**： 通常設置 300 PPI。
- **大型燈箱圖像**：一般不低於 30 PPI。
- **一些特大的牆面廣告**：可設定在 30 PPI 以下。


### 色彩模式：RGB & CMYK
這邊是以講師放出來的補充資料為主，補上我一些自己查的資料。

<p class="illustration">
    <img src="https://i.imgur.com/0t8zwy1.jpg" alt="RGB & CMYK">    
</p>

1. **RGB**   
    RGB 一般稱為**色光三原色**，這是在電腦圖學領域中，非常常見的顏色模型，像之前做 CV 時，引入的資訊就是 RGB。在做前端時，我們常用的 Hex （十六進位色碼標示法）也是基於 RGB 的運作原理。
    
    厄...其實前端的顏色表示用 RGB 也是合理，因為只要最後的成品是**呈現在電子螢幕**上，色彩模式都要調成 RGB，這與電子螢幕的成像原理有關。
    
    RGB 的成色原理是由 <mark>紅（**R**ed）、綠（**G**reen）、藍（**B**lue）</mark> 三種顏色進行疊加，由於這些顏色是色光，所以疊加越多層顏色越<mark>亮</mark>，當三色光都相疊在一起就會成為白（**W**hite）光，這樣的疊加模式稱之為**加法混合**原理。
     
    提到白光，恩...最近忙的快看到白光了（泣 :sob:
    
    <p class="illustration">
        <img src="https://i.imgur.com/Ta0PCW7.png" alt="看到白光了">
        看到白光了（圖片來源: <a href="https://es.123rf.com/photo_55649087_3d-hombre-muerto-en-el-ata%C3%BAd-.html">123RF</a>）
    </p>
       
    在此色彩模式中，每個顏色可以 <mark>0 ~ 255</mark> 來表示各自的強度，當紅綠藍三個顏色數值皆是 255 時，就會呈現白色；反之，若三個顏色值都是 0 時，就會變成黑色（blac**K**）。另外從上圖的 RGB 模式圖來看：紅＋綠會得到黃（**Y**ellow）、紅＋藍得洋紅（**M**agenta）、藍＋綠得青（**C**yan）。    
    <p class="paragraph-spacing"></p>
    
    回到 Photoshop 這邊，在檔案新增時會看到相對應的色彩模式顯示在文件上：
    
    <p class="illustration">
        <img src="https://i.imgur.com/zAqh90y.jpg" alt="RGB">
    </p>
     
2. **CMYK**    
   CMYK 一般俗稱**印刷四色**，主要應用於彩色印刷。它的成色原理是由青色、洋紅色、黃色與黑色四種色料構成，與 RGB 的混合原理不同，它是屬於**减法混合**原理，也就是疊加越多層顏色越<mark>暗</mark>，有點類似水彩的調色。

   根據上圖的 CMYK 模式圖來看，CMY 三種色料疊加會出現黑色，從 Google 的頁面上，設定 `CMYK(100%,100%,100%,0%)` 時也會看到黑色。
   
   不過據我查到的[資料](https://blog.eprint.com.tw/what-is-the-difference-in-rgb-vs-cmyk/)說，這三種色料等量疊加之後會只變成深灰色或深褐色，必須加上黑色色料，才能印出全黑色圖像。所以在[維基百科](https://zh.wikipedia.org/wiki/%E5%8D%B0%E5%88%B7%E5%9B%9B%E5%88%86%E8%89%B2%E6%A8%A1%E5%BC%8F)上，對 CMYK 描述是：
   > 利用色料的三原色混色原理，加上黑色油墨，共計四種顏色混合疊加。
   
   在 Google 的頁面上，設定 `CMYK(0%,0%,0%,100%)` 時也會看到黑色。
   
   <p class="illustration">
   <img src="https://i.imgur.com/iNVcfyx.png" alt="Google">
   </p>
 
    在此色彩模式中，每個顏色是以 <mark>0 ~ 100%</mark> 來表示各自的強度，它的與 RGB 的主、次色剛好反過來，在 CMYK 中是由青色、洋紅色、黃色三個主色，當三個主色都是 0 時，會呈現白色；而這些主色可以疊加出次色：紅色（紅色＋黃色）、綠色（青色＋黃色）、藍色（青色＋紅色）。 
    
    不過產品最後的顏色可能會有色偏，實際色偏程度會依廠牌色料配方而有不同差異。...但是說 RGB 的產出也會有色差，**在不同的螢幕上看到的顏色有色差**，熟悉嗎？因為每次網購都會見到這句話 XDDD
        
   <p class="illustration">
   <img src="https://i.imgur.com/5A12mAP.jpg?3" alt="傻眼的商品色差">
   傻眼的商品色差（圖片來源: <a href="https://www.dcard.tw/f/buyonline/p/230686256">網路購物板｜Dcard</a>）
   </p>
     
    因此在新增 Photoshop 檔案時，請依照成品的使用目的，選擇相對應的色彩模式，這個模式也會顯示在文件上： 

    <p class="illustration">
    <img src="https://i.imgur.com/4dRnEYZ.jpg" alt="CMYK">
    </p> 
    
    如果要更改色彩模式，可在 `主選單` → `影像` → `模式` 中進行修改。

    <p class="illustration">
    <img src="https://i.imgur.com/X22XM0b.jpg?1" alt="更改色彩模式">
    </p> 
    
<p class="paragraph-spacing"></p><p class="paragraph-spacing"></p>

**RGB 與 YMCK 比較**

|          | RGB                 | YMCK                                                   |
| -------- | ------------------- | ------------------------------------------------------ |
| 顯示載體 | 電子螢幕            | 印刷輸出                                               |
| 顏色組成 | 紅（R）、綠（G）、藍（B） | 青色（Cyan）、洋紅色（Magenta）、黃色（Yellow）、黑色（blacK） |
| 數值範圍 | 0-255               | 0-100                                                  | 

<p class="paragraph-spacing"></p>

好奇兩個色彩模式在表現上會有什樣的差異，因此找了些資料來看。總結來說，相同顏色在 RGB 顯示上看起來會比較亮、彩度感覺比較高；CMYK 相較之下有點灰樸樸的，看起來有點像最近流行的莫蘭迪色！？

<p class="illustration">
    <img src="https://i.imgur.com/CvFzOnd.png" alt="RGB 與 YMCK 色差">
    RGB 與 YMCK 色差（圖片來源: <a href="https://blog.eprint.com.tw/what-is-the-difference-in-rgb-vs-cmyk/">易普印 e知識百科</a>）
</p> 



## 界面介紹
在這節課中，講師對於 Photoshop 的介面介紹：

<p class="illustration">
    <img src="https://imgur.com/NuQB708.jpg" alt="Photoshop 介面">
</p> 

在 Photoshop 右上方的文字選項，稱之**主選單**。點擊其中一個選單，相關的功能就會在下方展開。不過對於老手（講師）而言，除了：濾鏡、檢視與說明，其它功能幾乎都可以透過快速鍵來啟用，很少手動點選。

<div class="alert success"> 
<div class="head">老師建議</div>
Photoshop 的功能很多，但依照工作性質的不同，會需要使用的工具也不盡相同，你不需要、也不可能每個工具都熟稔，但需要知道那個工具怎麼運用，來創作你想要的作品。
</div>

<p class="paragraph-spacing"></p>

而在左邊會有一欄**工具列**，當選取工具列中其中一個功能時，在主選單下方會出現該工具的屬性。若是不小心將工具列關掉後，可以到 `主選單`→`視窗`→`工具`，勾選即可。

講師特別介紹的是最下面兩個黑白兩色的框框的，也就是我上圖中藍色框的位置，他們分別是前後景色：
- **前景色**：左上角的白色框，是前景色。前景色是在圖上作畫的顏色，當使用地方筆刷、油漆筒...等工具，使用的就是這個顏色。
- **背景色**：右下角的黑色框，稱為後景色或背景色。背景色相當於，一張圖的底色，當使用橡皮擦工具擦除時，會顯示出這個顏色。
 
<p class="paragraph-spacing"></p>
 
當點擊前景色/背景色的方框時，會打開檢色器，可以藉由更改鮮豔度、明暗度或是色相，來更改顏色，不過對我來說更熟悉的可能是直接輸入右方的 RGB 跟 Hex XDDD

<p class="illustration">
    <img src="https://i.imgur.com/VKiTbJy.png" alt="Photoshop 介面">
</p> 

另外，在前/背景色按鈕的左上方，有一個按鈕可以恢復成預設的前/背景色，也就是紫色圈起來的地方，只要一點擊前/背景色就會恢復成黑/白。


<p class="paragraph-spacing"></p>

畫面的右邊則是面版/工作區，講師建議先保持**圖層**、**色版**、**路徑**與**內容**四個面板，另外網友建議可以保留**色票**，之後其他面板就可以關掉了，有需要在開啟，否則：
<div class="blockquote-center">
儘量保持你的工作區域越乾淨越好。
</div>

<p class="illustration">
    <img src="https://i.imgur.com/6pY7yBI.png" alt="Photoshop 介面">
</p> 

若是不小心將工作區的面板關掉後，可以到 `主選單`→`視窗`→`內容面版/圖層面版`。通常開啟圖層面板時，會將色版跟路徑一併導入。



## 不同種的影像顯示方式
這節課我們要來講不同檢視影像的方式，以及一堆快速鍵的記憶。

1. **放大縮小圖像**  
    按下快速鍵 `Ctrl` + `+` 與  `Ctrl` + `-`。另外發現 `Alt` + `滑鼠滾輪`，也可以縮放達到效果。

2. **移動影像**   
    當縮放比例超過 100% 或太大時，可能會有局部影像不在範圍內時，此時按下`空白鍵`，會出現手形工具的符號，這時就可以隨意移動。
    
    不過，我按空白鍵要使用手形工具來要移動時，雖然指標會變成手形工具，但卻是無法移動圖像；可是若是按快捷鍵 `H` 或直接點選工具列的手形工具，卻是可以移動的。
    
    找了下人家的[解決方法](http://ask.zol.com.cn/x/2859591.html)：
    1. 快捷鍵 `H` ，但壞處是用完之後得要再換回原本工具。
    2. 按住空白鍵不放，此時按一次 `Ctrl`(記得放開)，再次嘗試拖曳圖片，就可以了。壞處是比較麻煩，但暫時也只能用這個辦法了。
    
    <p class="paragraph-spacing"></p>

    另外，記錄一個不小心玩出來的移動影像的方法：
    1. 垂直移動：`PageUP`/ `PageDown`
    2. 平滑垂直移動：`Shift` + `PageUP` /  `Shift` + `PageDown`
    3. 水平移動： `Ctrl` + `PageUP`/  `Ctrl` + `PageDown`
    4. 平滑水平移動：：`Shift` + `Ctrl` + `PageUP` /  `Shift` + `Ctrl` + `PageDown`
  
  <p class="paragraph-spacing"></p>
 
3. **檢視方式**  
    按下快速鍵 `F` 可以在不同檢視方式間迅速切換。玩了一下大概有三種不同的檢視方式，要形容的話，大概就是大中小三種。
    
4. **隱藏面板**  
    快速鍵 `TAB`，可以顯示/隱藏主選單外所有面板。如果是 `Shift` + `TAB`，則會顯示/隱藏主選單與工具列以外的所有面板。



## 其他連結
1. 實用 Photoshop 入門到大師筆記：[目錄](/Practical-Photoshop-from-Beginner-to-Master-Contents)



## 參考資料
1. 設計嵐 (2010-01-19)。[『解析度』的名詞解釋](https://kav68795.pixnet.net/blog/post/25680483)。檢自 痞客邦 (2021-01-13)。 
2. 協同撰寫。[解析度](https://zh.wikipedia.org/wiki/%E5%88%86%E8%BE%A8%E7%8E%87)。檢自 維基百科 (2021-01-13)。
3. 協同撰寫。[每英寸像素](https://zh.wikipedia.org/wiki/%E6%AF%8F%E8%8B%B1%E5%AF%B8%E5%83%8F%E7%B4%A0)。檢自 維基百科 (2021-01-13)。
5. Han (2019-06-24)。[什麼是像素？解析度？一次搞懂，數位影像名詞剖析與簡介](https://www.wonder-product.com/pages/digital-image)。檢自 灣得文創 (2021-01-13)。
6. 起源趙老師 (2018-01-27)。[什麼是像素？什麼是解析度](https://kknews.cc/zh-tw/digital/9xrk82j.html)。檢自 每日頭條 (2021-01-13)。
7. JessieLAB。[RGB和CMYK的色彩模式差異，關於設計重要的小事](https://jessielab.com/rgb%E5%92%8Ccmyk%E7%9A%84%E8%89%B2%E5%BD%A9%E6%A8%A1%E5%BC%8F%E5%B7%AE%E7%95%B0%EF%BC%8C%E9%97%9C%E6%96%BC%E8%A8%AD%E8%A8%88%E9%87%8D%E8%A6%81%E7%9A%84%E5%B0%8F%E4%BA%8B/)。檢自 JessieLAB 潔西實驗室 (2021-01-13)。
8. 小宜 (2016-10-21)。[RGB 與 CMYK 色彩模式差異](https://blog.eprint.com.tw/what-is-the-difference-in-rgb-vs-cmyk/)。檢自 易普印 e知識百科 (2021-01-13)。
9. 奧米加 (2008-11-03)。[色彩觀念-簡述RGB與CMYK](https://ayu6628.pixnet.net/blog/post/4807180)。檢自 痞客邦 (2021-01-13)。
11. 協同撰寫。[印刷四分色模式](https://zh.wikipedia.org/wiki/%E5%8D%B0%E5%88%B7%E5%9B%9B%E5%88%86%E8%89%B2%E6%A8%A1%E5%BC%8F)。檢自 維基百科 (2021-01-13)。
12. [为何我的电脑photoshop里的空格抓手不能用](http://ask.zol.com.cn/x/2859591.html)。檢自 ZOL问答 (2021-01-13)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-02-01</summary>
  <ul>
    <li>2021-02-01 發布</li>
    <li>2021-01-14 完稿</li>
    <li>2020-12-13 起稿</li>
  </ul>
</details>
