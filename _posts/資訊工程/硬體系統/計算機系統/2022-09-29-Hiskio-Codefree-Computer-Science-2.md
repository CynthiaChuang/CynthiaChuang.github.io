---
title: CodeFree｜喝一杯咖啡，輕鬆學電腦科學 - 下
date: 2022-10-07
is_modified: false
disqus: cynthiahackmd
categories:
- "硬體系統 › 計算機系統"
tags:
- 計算機概論
- 讀書筆記
- Hiskio
--- 


今天又是喝咖啡學習系列，不過不是學程式，而是學電腦科學。筆記內容包括[上](https://codefree.hiskio.com/courses/4)、[中](https://codefree.hiskio.com/courses/5)、[下](https://codefree.hiskio.com/courses/6)三門課。
   
這門課看了下內文其實就是計算機概論，有點令人懷念的一門課…想當初算二進制還算到超挫折的 XDDD 不過怎感覺這篇不是在寫讀書筆記，而是重寫了一次課程？很多東西它沒提到，是我自己忽然想到其他關鍵字就一併寫進去了 XDDD

<!--more-->

<p class="illustration">
	<img src="https://i.imgur.com/ibijtxL.png" alt="hiskio codefree">
    codefree time（圖片來源: <a href="https://codefree.hiskio.com/">hiskio codefree</a>）
</p>



## CH 9｜IT 讓你跟陌生人之間只隔六個人
**六度分隔理論(Six Degrees of Separation)** 是還滿有名的一個現象，這個現象是在說，世界上的任何人與任何人之間，最多僅隔著 6 個人。

不過隨著社群網路的興起，這網路直徑正在逐漸下降。根據 Facebook 2016 年的研究 [〈Three and a half degrees of separation〉](https://research.facebook.com/blog/2016/2/three-and-a-half-degrees-of-separation/)，每個人與其他人間隔約為 4.57 人。


### 9-1｜網站的地址 － IP 位址
IP 位址（Internet Protocol Address，IP Address），可譯為網際協定位址、網際網路協定位址。IP 相當於每戶人家的門牌號，<mark>在網際協定中用於標識傳送或接收資料所示用來帶代表裝置的一串數字</mark>，每個裝置的 IP 都是獨一無二的，不然郵差怎麼把信送到你家？

忽然想到我上次把地址填錯，最後竟然還是送達了 XDDD。不過我那個沒錯得很離譜啦，只是把 Tainan 拼成了 Taiwan 而已，我郵遞區號還是寫對的！ 

<p class="paragraph-spacing"></p>

課程這邊介紹的是 **IPv4**。在 IPv4 中是使用 **32 位元以點區隔的十進位表示法**：簡單來說，就是用 4 個 0 ~ 255 的數字來表示，類似 `172.217.163.36`。另外，含有一個正在取代 IPv4 的是 IPv6，它是使用 **128 位元冒號區隔的十六進位表示法**，例如：`2001:0DB8:02de:0000:0000:0000:0000:0e13`。

2019 年 11 月 25 日，歐洲網路協調中心宣布 IPv4 地址已告罄，而 <mark>IPv6 的使用就是用來解決 IPv4 數量不夠的問題</mark>。不過台灣分配到的 IP 還夠使用，所以大部分都還是使用 IPv4 的網路通訊協定。

<p class="illustration">
	<img src="https://i.imgur.com/JIG2fRB.png" alt="IPv4 與 IPv6 的比較">
    IPv4 與 IPv6 的比較（圖片來源: <a href="https://www.ithome.com.tw/tech/92046">iThome</a>）
</p> 
 
IP 根據用途可以分為 **實體 IP（Public IP）** 及 **虛擬 IP（Private IP）** 兩種。實體 IP，代表可在公開網路中被使用，可以用來連線，就像建築物的門牌；虛擬 IP，則是提供給區域網路使用，不過這邊的 IP 封包是出不了路由器直接連網的，這時需要 IP 分享器的 NAT 來幫忙（其實應該是先有 NAT 才有虛擬 IP 才對 XDDD）。不過在家用環境中，數據機相連的那台通常都是路由器、有線分享器與無線分享器的多合一機器。

虛擬 IP 最常應用於企業中，當公司內部多台電腦並用 IP 分享器連接時，會各自被分配到一個虛擬 IP。不過當公司電腦對外連出去後，對外部網路來說卻都是同一個實體 IP 位址。所以有時會發現某個網站如果鎖 IP，幾乎全辦公室都會慘叫 XDDD

<p class="illustration">
	<img src="https://i.imgur.com/zlaeHle.jpg" alt="虛擬 IP 架構">
    虛擬 IP 架構（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 


不過實務上，我們在拜訪網站時，並不會輸入 `172.217.163.36`，而是直接輸入 `www.google.com`。這就托了**網域名稱伺服器（Domain Name Server，DNS）** 的福，它會轉換讓一串數字變成容易識別的英文字串。
 
 
#### 單元測驗
1. **IP 位址就像是網站的地址。針對 IP 位置的敘述，下列何者錯誤？**
    - [ ] 虛擬 IP 最常用於企業中，當公司內部有多台電腦並用 IP 分享器連接時，會各被分配到一個虛擬 IP，這些虛擬 IP 都是在同一個內部網路裡面
    - [ ] IP 是 Internet Protocol Address 的縮寫，意思是「網際網路協定位址」
    - [x] 網站的 IP 位址有可能重覆


### 9-2｜網站的地標名稱－網域名稱
課程中有個滿有趣的比喻：**IP 位址像是地址，而網域名稱像是地標名稱**。就像是大家會記得台北101，但沒人會記得它的地址，~~除了 Google~~。

<p class="illustration">
	<img src="https://i.imgur.com/AVyJNz4.jpg" alt="網域名稱組成">
    網域名稱組成（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 

網域名稱會由幾個部分組成的，每個部分以 `.` 做為區隔：
1. **https（http）**   
    **通訊協定**。這邊採用超文本傳輸協定（HyperText Transfer Protocol），是一種用於分布式、協作式和超媒體訊息系統的應用層協定。HTTP 是全球資訊網的通訊基礎。 
    
2. **www**   
    www 是可以自行設定的主機名稱，並非註冊域名的主體，或稱子網域及次網域。最常用的主機名稱有 www 及 FTP 等，或是省略不用，它會直接轉向網域名稱。
    
    子網域的其中一種用法是來規劃網站的不同用途。比如，如果你有一個網站 `yoursite.com`，在這個網域你有兩個區塊一個專門用於購物的網站、一個是記錄開發的網誌，那麼就可以使用子網域來區分，例如：`store.yoursite.com` 與 `blog.yoursite.com`。一個網域名稱最多可設定 500 個子網域，也可以為子網域加入多個階層，如`info.blog.yoursite.com`。子網域最長可以設定 255 的字元，但若設定多階層，則每個階層只能使用 63 個字元。

3. **104**  
    這是**次級網域（Second-Level Domain, SLD）**，通常是公司名稱或獨立識別度的文字，如 google、youtube、104...這個名稱會直接影響到使用者識別、搜尋引擎。

4. **com**  
    網址的 `.com` 是屬於 **頂級網域（Top-Level Domains, TLDs / Zone）** 的**通用頂級網域 (gTLD, Generic Top-Level Domain)** ，可以標示出網站的性質。常見的有 `.com`（公司行號或營利單位）、`.edu`（學術機構或教育單位）、`.gov`（政府機關）、`.org`（財團法人組織）。
    
    一般常見的 .com / .net / .org / .edu 等結尾，都是屬於通用頂級網名，這類的域名由網際網路名稱與數字地址分配機構（IANA）管理。

5. **tw**  
    **國家頂級域名 (ccTLD, Country-Code Top-Level Domain)**，也是屬於頂級網域的一種，能讓使用者能迅速辨別出網址申請機構所在的區域。通常是由區域名字縮寫組成，常見是由 ISO-3166 的代碼所組成的國家或地區名稱縮寫。
        
    但 2010 後，IANA 開始分配國際化國家頂級域名之後，就不再遵守這項限制了，除了不再侷限於 2 個字外，還能使用當地文字，例如 `.新加坡`、`.台灣`、`.الجزائر.`（阿爾及利亞）、`.ею`（歐盟）等，都屬於國家頂級域名的一種。
    
    頂級網域跟次級網域合起來就是我們前面提過的**主網域**（Domain）。我另外找了張網域拆得比較清楚的組成，應該會比較容易明白：
    
    <p class="illustration">
        <img src="https://i.imgur.com/YHg8GDQ.png" alt="網域名稱組成">
        網域名稱組成（圖片來源: <a href="https://bettywutalk.com/blog/domains/#3_xin_ding_ji_yu_ming_New_gTLD_New_Generic_Top-Level_Domain">矽谷獨角獸學院</a>）
    </p> 
  
如果我們要拜訪 `www.104.com.tw` 這個的網站，電腦會先去找頂級網域 `.tw` 的 DNS 伺服器找，再找到 `.com.tw` 的次級域 DNS 位置，最後由  `.com.tw` 找到 `www.104.com.tw` 的正確 IP。


#### 單元測驗
1. **可別搞混 IP 位址與網域名稱搞混了！請選出關於網域名稱的正確敘述。**
    - [ ] IP 位址像是地標名稱，而網域名稱像是網站的地址
    - [x] 網域名稱共分為 5 個部分 - 通訊協定、主機名稱、自定義網站名稱、網站性質、申請網址國家機構
    - [ ] 我們常在網址中看到的 http 就是主機名稱，也稱為子網域或次網域


### 9-3｜網站很懂我耶！
應該有注意過，當你搜尋某項商品後，你就會發現這個商品的廣告就會撲天蓋地的向你襲來，無論打開什麼網頁都會看到這些廣告。這是因為你被**小甜餅**給出賣了！

<p class="illustration">
    <img src="https://i.imgur.com/tJd5itd.png" alt="Cookie">
    Cookie（圖片來源: <a href="http://lacovadvidre.cat/es/mes-informacio-sobre-les-cookies/">lacovadvidre</a>）
</p> 

小甜餅的正式名稱是 Cookie(s)，它是一個「小型文字檔案」。是為了克服 HTTP 協定的無狀態性，所採用的「額外手段」之一，以維護使用者跟伺服器對談中的狀態。它會儲存一些瀏覽資訊、個人偏好、偏好語言與位置、內容定制...等資訊，通常不會存儲有關敏感訊息，例如信用卡、身分證等。

<p class="paragraph-spacing"></p>

根據儲存位置，Cookie 可分為**記憶體 Cookie** 和**硬碟 Cookie**。
1. **記憶體 Cookie**  
    由瀏覽器維護，儲存在記憶體中，當瀏覽器被關閉時就消失。存在時間短暫，是臨時的 Cookie。

2. **硬碟 Cookie**  
    當瀏覽器關閉後，尚未到期的 Cookie 會被儲存到硬碟，除非使用者手動清理或過期，否則硬碟 Cookie 不會被刪除。會長期存在，是長期的 Cookie。

<p class="paragraph-spacing"></p>

常見的 Cookie 使用情境有：
1. **網路購物**   
    當你在選購商品時點選了一瓶飲料，而瀏覽器會在傳送網頁時傳送一段 Cookie 給伺服器，讓伺服器知道你選購了飲料；之後如果繼續選購商品， Cookie 就會不斷追加新的資訊。最後結帳時，伺服器讀取 Cookie 就能得知所有要購買的東西品了！

2. **自動登入**   
    當登入時勾選「下次自動登入」，再次回到同一個網站時，無須輸入帳密就可以完成登入。這是因為上次登入時伺服器將其 Cookie 傳送到使用者的硬碟上，當下次登入時，若 Cookie 尚未過期，瀏覽器就會傳送這個帳號密碼的 Cookie，就不必再輸入了。

3. **廣告投放**   
    廣告商會各個網站上投放廣告，而在這些網站上，你連上的每一頁網址都會被 Cookie 所記錄，為第三方 Cookie。因此若把各網站的資料整合，就能追蹤你的網站使用行為。
    
    說到這個忽然想到，之前我們公司會在 APP 裡面埋廣告，結果過沒多久收到使用者的負評說：『你們可不可以不要一直投放色情廣告阿！』（笑翻了我 XD ）


#### 單元測驗
1. **現在你終於知道為什麼網站總是知道你的喜好了吧！全都是因為 Cookie！下列關於 Cookie 的敘述，何者正確？**
    - [ ] 根據儲存位置的不同，可分為控制 Cookie 和記憶體 Cookie
    - [x] 記憶體 Cookie 為非持久 Cookie，由瀏覽器維護，儲存在記憶體中，當瀏覽器被關閉時就消失了，存在時間是很短暫的
    - [ ] 控制 Cookie 為持久 Cookie，若不是用戶手動清理或到了過期時間，Cookie 不會被刪除，存在時間是長期的


### 9-4｜Cookie 出賣了隱私，但你可以這樣做！
基本上，Cookie 不是木馬也不是間諜軟體，網站不可能經由 Cookie 獲得你的其他私人資料，更沒有辦法透過 Cookie 來存取你的電腦。而在 HTTP 中對於 Cookie 的大小有 4 KB 左右的限制，因此也無法儲存複雜資料。但，一旦遭遇惡意的攻擊時，這些資料可能就無法倖免。

如果不想被被盜取 Cookie 可以：
1. 定期清除 Cookie
2. 選擇關閉 Cookie


#### 單元測驗
1. **你擔心你的瀏覽行蹤完全被 Cookie 掌握嗎？其實你可以透過操作來設定 Cookie！關於 Cookie 的特性或設定，何者錯誤呢？**
    - [ ] Cookie 能記載的資料有限，且網站不可能經由 Cookie 獲得你的 email 地址或是其他私人資料
    - [x] 網站可以透過 Cookie 來存取你的電腦
    - [ ] 為避免電腦的擁有者或有心人士竊取你使用的軌跡，在離開公共電腦時，你可以清除 Cookie


### 9-5｜小心你的資訊安全
資訊安全不僅限於網路安全、軟體安全，更明確的說資訊安全是要<mark>保護個人或群體的資料或者資料系統不受到非法的侵入、任意使用，甚至破壞。</mark>


**CIA 三要素**是資訊安全的鐵三角，任何違反三要素的行為都會降低防護強度，而對公司資產或機密造成威脅。

<p class="illustration">
    <img src="https://i.imgur.com/8Fp0fFu.png" alt="CIA 三要素">
    CIA 三要素（圖片來源: <a href="https://www.i-scoop.eu/cybersecurity/cia-confidentiality-integrity-availability-security/">i-scoop</a>）
</p> 

1. **機密性（Confidentiality）**  
    目的在於確保資料傳遞與儲存的隱密性，**任何機密資料未經授權所都不得被閱覽傳遞**。機密資料包含：正在開發的技術、軍事機密、信用卡資料、醫療紀錄...等，只要是不能或不想公開的資料，就是機密資料。

2. **完整性（Integrity）**  
    目的在於**確保資料在傳輸或儲存的生命週期中，保有其正確性與一致性**。因此需維持資料處理過程是安全合規的，不得遭未經授權者修改。
    
3. **可用性（Availability）**  
    與機密性相反，資訊可供授權者不間斷地閱覽或存取使用，並能滿足使用需求。如停電、網路斷線或資料遺失，也都違反此原則。
 

機密性、完整性、可用性三要素是互相牽制的，例如：機密性高會使可用性的降低；高可用性的系統會導致機密性與完整性的降低。如何在有限資源下，讓三者保持平衡當今資訊安全的重要議題。

<p class="illustration">
    <img src="https://i.imgur.com/J2LWQk6.jpg" alt="CIA 三要素">
    CIA 三要素（圖片來源: <a href="https://medium.com/hannah-lin/從零開始學資安-什麼是資訊安全-75a7a208e8db">Hannah Lin｜Medium</a>）
</p> 

<p class="paragraph-spacing"></p>

另外介紹系統安全防護的三個面向（3A，AAA）：

1. **認證（Authentication）**    
    **識別資訊使用者的身分**，並記錄資訊被誰所閱覽使用。常見方法是透過密碼、憑證方式驗證使用者身分，也可透過 OTP、指紋等方式進行識別。

2. **授權（Authorization）**    
    **依照實際需求授予適當權限**，一般建議採最小權限原則（Least privilege，PoLP），即僅給予執行工作職能所需之最低存取權限，避免過度授權可能造成的資訊洩漏。

3. **稽查（Accounting）**    
    **收集使用者與系統之間互動的資料，並留下軌跡紀錄**。紀錄內容包含監控（Monitoring）、報告（Reporting）與日誌(Logging)，以及稽核（Auditing）、計費（Billing）、分析（Analysis）...等資訊提供未來使用。
 
<p class="paragraph-spacing"></p>

最後，提供了些基本的資安意識。
1. **杜絕盜版**：不接觸不在規範內的違法使用行為，可以大幅降低資安風險。
2. **強化密碼**：資安原始意義就是加密，不重複、擁有第二憑證等高強度的密碼絕對是必要的。
3. **隱私控管**：定期清除上網紀錄或 cookie，尤其使用公用電腦。
4. **軟體更新**：不要小看軟體更新，除了提升穩定度也可以適當防堵安全漏洞。
5. **HTTPS**：加密版本的 HTTP，可以為網頁瀏覽行為加上一層保障。


#### 單元測驗
1. **資安有著 CIA、3A 這幾個特性，現在就來複習一下吧！針對資安的特性，下列敘述何者錯誤？**
    - [ ] 3A 分別是認證（Authentication）、授權（Authorization）、稽查（Accounting）
    - [x] C 為機密性（Confidentiality），表示資訊不得向任何人揭露
    - [ ] I 為完整性（Integrity），表示資訊不得遭非經授權者更改
    - [ ] A 為可用性（Availability），表示資訊可供已授權者使用與存取


### 9-6｜駭客到底是好是壞？
這章來介紹在電影中超神秘的駭客（Hacker），在電影中他們可以劃分成正派與反派陣營，還各個能文又能武。在現實生活中，這些駭客們是不是文武雙全我不知道，但他們真的有分陣營，其中有好有壞，也有不成氣候的半調子。
 
<p class="illustration">
    <img src="https://i.imgur.com/LFOUDFC.jpg" alt="駭客分類">
    駭客分類（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p>


1. **白帽駭客（White Hat）**   
    又名為**道德駭客**。他們通常是電腦安全專業人士，會幫助企業找出系統漏洞進而修改，或幫助受害企業移除病毒；或者以個人工作者的身份發現系統漏洞來賺取獎金。但總結來說，白帽駭客的行為是合法的，因為他們在公司的許可下入侵系統。
 
2. **黑帽駭客（Black Hat）**   
    也被稱作**劊客或怪客（Cracker）**，是真正的網路犯罪者。會為了金錢或純粹惡意蓄意破壞、攻擊、非法入侵盜取...等。
    
3. **灰帽駭客（Grey Hat）**   
    他們是中間的灰色地帶，遊走於正邪之間。他們可能會非法入侵、散播病毒，以竊取金錢或資訊；有時會在事後通知目標企業注意漏洞，並象徵式收取協助修復系統費用；有時則是為了炫耀技術或是宣揚某目標進行攻擊。灰帽駭客通常會有是不道德和非法的行為，但他們不參與犯罪活動，與黑帽駭客專門從事犯罪活動不同。
    
4. **腳本小子（Script Kiddie）**    
    沒有技術的劊客，算是個貶義詞。他不像真正的駭客那樣會發現系統漏洞，通常使用別人開發的程式來惡意破壞他人系統，因而稱之為腳本小子。


<p class="paragraph-spacing"></p>


除了上述的幾個分類外，我還聽過紅帽，知名組織匿名者（Anonymous）記憶中就屬於紅帽。會說記憶中是因為我現在找不到資料出處，不過真的有紅帽這個分類啦，雖然不算主流分類。


1. **紅帽駭客（Red Hat）**   
    從文字敘述看來紅帽駭客仍然是屬於白帽或灰帽範疇的，或者更偏向灰帽！？
    
    他們也會像白帽一樣制止黑帽的行動，但卻是採用上載病毒、阻斷服務攻擊（DoS attack）等進攻的方法，以入侵並關閉對方的電腦，也因有手段較為激進，有資料將其稱為激進駭客（Hacktivism）。更多時候他們會利用攻擊來表達政治、意識型態、社會或宗教訊息、或是宣揚某些目的（例如宣傳資訊自由、反戰申明），但這些目的通常都是不是金錢或個人利益。
    
    著名的紅帽有維基解密（WikiLeaks）和匿名者。

2. **藍帽駭客（Blue Hat）**  
    這是我在查資料時看到的，不過在不同的資料中我看到了不同的定義。
    
    有一說他們是報復者，僅入侵激怒他們的對象，除非被招惹，否則不會主動攻擊他人，感覺上比較偏向灰帽；另一說則是，軟體公司會將安全性測試委外給外部的藍帽駭客執行，以在軟體發布之前，找軟體的安全性漏洞，若從這個角度看來，他們比較偏向白帽。
 
3. **綠帽駭客（Green Hat）**  
    這個是個有趣的抬頭，簡單來說這是他們是駭客中的新人，不過可能是那種白目新人，問題多到被其他駭客翻眼的那種。不過總體來說算是個熱衷學習、聽命而為的駭客。
    

是說，我在查資料的時候注意到，中國那邊將駭客（Hacker）稱為黑客、將劊客（Cracker）稱為駭客。找資料的時候稍微注意一下。


#### 單元測驗
1. **這個章節是不是打破你對駭客的既定印象呢？你還記得每一種駭客的特性嗎？請你選出錯誤的敘述喔！**
    - [ ] 白帽駭客的行為是為了找出系統漏洞進而修改
    - [x] 灰帽駭客通常是電腦安全專業人士
    - [ ] 黑帽駭客的行為是蓄意破壞或攻擊，也是真正的網路犯罪者
    - [ ] 腳本小子是沒有技術的劊客


## CH 10｜程式語言中有社會階級嗎？
程式語言之中也有好壞優劣之分？是沒有，但有鄙視鏈，這是個人人都鄙視 PHP 的世界 XDDD

<p class="illustration">
    <img src="https://i.imgur.com/EEs4SSE.png" alt="駭客分類">
    人人鄙視 PHP（圖片來源: <a href="https://blog.csdn.net/qq470603823/article/details/122204409">Python导师大白的博客｜CSDN博客</a>）
</p> 


### 10-1｜低階語言（Low-level language）
高低階語言指的不是程式語言的優劣，而是以**易讀性**做為區分依據。高階語言接近於人類使用的語言，也越容易學習，但執行效率比低階語言差；反之，低階語言較接近電腦執行的動作，執行效率高，但不易學習。

<p class="illustration">
    <img src="https://i.imgur.com/U3omHfK.jpg" alt="高低階語言差別">
    高低階語言差別（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 

這章節先介紹**低階語言**。一般來說，低階語言與電腦硬體相依行高，具備**機器依存（Machine Dependent）**、**可攜性（Portability）低**的特性。舉例來說，在 ASUS 上能執行的程式，通常不能在 ACER 的執行，必須經過修改才能在不同電腦正確執行。因它與硬體運作的密切關係，所以設備商多用低階語言來開發設備的驅動程式。

而常見的低階語言有「機器語言」及「組合語言」：
  
1. **機器語言（Machine Language）**  
    機器語言是以**二進制**，也就是 0 和 1 與電腦直接溝通，不需經過任何翻譯便可以執行，且佔用**記憶體少**，所以**執行速度時間最短**。<mark>它也是電腦唯一懂的語言</mark>。
    
    但因整份程式碼只用 0 和 1 兩個數字來撰寫，所以撰寫程式碼程式時需要強記每個指令對應的機器碼，難學難懂，且不易發現除錯。
    
    <p class="illustration">
        <img src="https://i.imgur.com/Ch5zydL.png" alt="二整數相加的機器語言碼">
        二整數相加的機器語言碼（圖片來源: <a href="https://shopee.tw/【欣明】Foundations-of-computer-science-2e-計算機概論-原文書-i.66892677.8437790606">《計算機概論 2/e》</a>）
    </p>  

2. **組合語言（Assembly language）**  
    因二進制符號的難讀難寫，因此衍生出較接近人類符號的助憶碼來取代機器語言，方便撰寫與除錯。但即便如此還是超級難寫...之前寫個加法器的作業就搞掉我快一週時間...。

    <p class="illustration">
        <img src="https://i.imgur.com/PCgqOSW.png" alt="二整數相加的組合語言程式碼">
        二整數相加的組合語言程式碼（圖片來源: <a href="https://shopee.tw/【欣明】Foundations-of-computer-science-2e-計算機概論-原文書-i.66892677.8437790606">《計算機概論 2/e》</a>）
    </p> 
    
    
    此外組合語言無法電腦直接溝通，因此必須通過**組譯器**（Assembler）將助憶碼轉譯成為機器碼。 
    
    <p class="illustration">
    <img src="https://i.imgur.com/Z6x3mB5.jpg" alt="組合語言需通過組譯器轉譯">
    組合語言需通過組譯器轉譯（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
    </p> 


#### 單元測驗
1. **低階語言可不是比高階語言低下的語言喔！請從選項中選出關於低階語言的正確敘述吧！**
    - [x] 低階語言又分為機器語言、組合語言
    - [ ] 低階語言與高階語言相比，較貼近人類使用的語言，因此較容易理解
    - [ ] 相較於高階語言，低階語言在程式撰寫、維護方面都較為容易


### 10-2｜高階語言（High-level language）
相較低階語言，高階語言會**更接近人類英文語法**，因此容易學習也易於維護；此外高階語言不受電腦的限制，可使用在不同的電腦系統執行，**可攜性高**，...當然前提是你寫的好，不然還是得改程式。

而高階語言也具組合語言的特徵，一樣必須透過**直譯或編譯**將高階語言轉譯成機器語言。也因為這樣的轉譯處理，執行效率會比低階語言差。


高階語言主要分為「程序導向語言」及「物件導向語言」：

1. **程序導向語言（Procedure-Oriented Programming，POP）**  
    指的是一般結構化的高階程式語言，這些程式語言都是**依照程式敘述的「先後指令及邏輯順序」**，一步一步向下執行。
    
    常見的程序導向語言有 Basic、Fortran、Cobol 及 C 語言..等。


2. **物件導向語言（Object-Oriented Programming，OOP）**  
    而物件導向語言則是先設計類別與物件，再利用多個物件組合出程式。物件導向語言具備 3 大特性：
     1. **封裝（Encapsulation）**  
        將特定功能的處理程序及資料包裝在物件裡面，使用者不需要了解內部設計就可以使用。
     2. **繼承（Inheritance）**  
        新類別物件可以承襲既有類別的功能及屬性，可以省去撰寫相同程式碼的時間。
     3. **多型（Polymorphism）**   
        不同物件對於同樣的事件，可以有不同的表示法。

    目前較知名且流行的程式語言，大多都是物件導向語言，像是 Python、C++、Objective-C、Swift、Java、Ruby 及 PHP 等。


<p class="illustration">
    <img src="https://i.imgur.com/XqUi0yL.jpg" alt="機器語言、組合語言與高階語言的比較">
    機器語言、組合語言與高階語言的比較（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 


#### 單元測驗
1. **可別搞混低階語言、高階語言的特性了喔！請問關於「高階語言」的敘述，何者錯誤？**
    - [ ] 高階語言又分為「程序導向語言」及「物件導向語言」
    - [x] 程序導向語言是依照程式敘述的「先後順序及流程」來告知電腦需要完成什麼工作及它的流程，而 Python、Java 都屬於此種語言
    - [ ] 高階語言的執行速度較低階語言慢


### 10-3｜高階語言的內建翻譯機
前面提過，機器語言是電腦唯一懂的語言，連同為低階語言的組合語言都需透過組譯器將其轉譯成機器語言，而高階語言更是此。在標題中所提到的內建翻譯機，指得就是前面所提過的**直譯**、**編譯**以及**即時編譯**。


1. **編譯語言（Compiled language）**   

    <p class="illustration">
    <img src="https://i.imgur.com/PDAhI9u.jpg" alt="編譯流程">
    編譯流程（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
    </p> 

    這種程式語言，會在程式執行前先透過編譯器（compiler）將程式碼編譯成機器語言（machine language）。而這些機器語言會再透過連結器（linker）將一個或多個目的檔（.obj）與靜態函式庫（.lib）連結，產生可執行檔（.exe）。最終要執行程式時，只需要透過載入器（loader）將可執行檔載入記憶體，並與動態函式庫（.dll）連結，不需重新翻譯。
    
    編譯式語言多半以 **靜態語言（static language）** 為主，如：C、C++、bjective-C、Visual Basic ...等等。它們會事先定義的型別、型別檢查，能夠在程式編譯時期檢查中型別錯誤。此語言的優點因已經先預先編譯，因此具備較高的執行速度，反之程式語法繁瑣、彈性不足，因此在執行前只能出檢查的簡單錯誤，程式開發、除錯速度會較慢。
 
2. **直譯語言（Interpreted language）**   

    <p class="illustration">
    <img src="https://imgur.com/rgNeUZd.jpg" alt="直譯流程">
    直譯流程（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
    </p> 
    
    
    不同於編譯語言，直譯語言在執行時會一行一行的動態將程式碼直譯成機器語言；直譯器在此擔任的角色就像一位「中間人」，它會邊轉譯邊執行。直譯語言多半以**動態語言**為主，包含：JavaScript、Python、Ruby...等等，具有靈活的型別處理，動態生成與程式彈性，但執行速度會比編譯式語言要慢一些。
    
    使用直譯器的好處是，它不需要在每次程式更新後重新編譯整個程式，程式開發與除錯速度比較快，較適合新手。而缺點在於動態語言的型別錯誤要到執行時期才會呈現出來，效能較不理想。此外不同於編譯語言的可執行檔可獨立執行；直譯語言則是必須依賴一個執行環境，語言可用的功能由這個執行環境提供，例如 JavaScript 只能使用瀏覽器提供的功能、Python 必須安裝相關 Package。
    
    <p class="illustration">
    <img src="https://i.imgur.com/3VfMp0x.png" alt="編譯與直譯語言比較">
    編譯與直譯語言比較（圖片來源: <a href="https://cg2010studio.com/2021/01/25/圖解-編譯-vs-直譯/">逍遙文工作室</a>）
    </p> 
     

3. **即時編譯（just-in-time compilation，JIT）**  
 
    <p class="illustration">
    <img src="https://i.imgur.com/Q9r28tA.jpg" alt="即時編譯">
    即時編譯（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
    </p> 
    
    為了改善編譯語言以及直譯語言的缺點，因而發展出即時編譯的技術，混合了兩者的優點。如同編譯語言，會一句一句編譯原始碼，但翻譯過的程式碼會快取起來成為中介碼（Bytecode）。到執行期時，再將中介碼直譯，之後執行。使用即時編譯技術的語言會比純編譯語言來的慢一些，但是卻又擁有直譯語言的特性。此類語言的代表語言有 Java、C#。


#### 單元測驗
1. **高階語言的翻譯機分為「編譯」、「直譯」、「即時編譯」，下列關於其敘述，何者錯誤？**
    - [ ] 編譯語言有 C、C++ 等，優點是有高效能的執行速度，但缺點是程式語法繁瑣、彈性不足，也只能檢查出執行前的簡單錯誤
    - [x] 直譯式語言有 Objective-C，其語法簡潔且有較高的彈性，且在初期嘗試的階段可以立即執行並除錯
    - [ ] 即時編譯是為了改善編譯語言及直譯語言的缺點，所以其混合了兩者的優點


## CH 11｜成為軟體工程師第一步：選擇一個語言並練熟
這個章節是從學習目標分析挑選的入門語言。
    
<p class="illustration">
<img src="https://i.imgur.com/JczesKA.png" alt="程式語言合集雲">
</p> 


### 11-1｜初學者可以從網頁開發開始
沒啥特定目標的話，可以考慮從開發網頁開始，因為相較其他程式，網頁不但能立即看到成果，更能直接開始使用；如果再搭配線上編輯器，更可以即時看到效果。對於初學者來說，可以即時看到成果，能得到較高成就感。

其他原因如**簡單易懂，資源龐大**，在網路上，網頁學習相關資源既多又廣，可以說整個網路世界都是你的老師。
- **相關工具**
    - [w3schools](https://www.w3schools.com/)：網頁開發的字典
    - [coolors](https://coolors.co/)：網頁設計的調色盤/配色寶典
    - [awwwards](https://www.awwwards.com/)：各種免費的現成優秀案例可以
- **線上課程網站**
    - [Coursera](https://zh-tw.coursera.org/)
    - [Udemy](https://www.udemy.com/?utm=3ba75b10bfa8ef986d88743c19573aab&track=1&pt=2)
    - [Hahow](https://hahow.in/)
    - [Yotta](https://www.yottau.com.tw/home)
    - [The Odin Project](https://www.theodinproject.com/)
    - 程式相關社團，如：[六角學院](https://www.hexschool.com/?utm_source=google&utm_medium=ads&utm_campaign=hexschool-course-web-searchads220425&gclid=Cj0KCQjwpeaYBhDXARIsAEzItbE39xzr7wFi2HjcR6OBgnv6og2-Gv_eJB7z8aojYe7-nivC0iNUQ50aAj3KEALw_wcB)
- **線上編譯器**
    - [CodePen](https://codepen.io/)
    - [JSFiddle](https://jsfiddle.net/)

<p class="paragraph-spacing"></p>

嗚，課程中還有稍微提了下網頁三巨頭 － HTML、CSS、JavaScript，不過我這邊就不提了，有需要的話去看看 [〈CodeFree｜喝一杯咖啡，輕鬆學 HTML〉](https://cynthiachuang.github.io/Hiskio-Codefree-HTML)這篇。


#### 單元測驗
1. 如果你想學程式，但還沒有方向，那麼建議你可以從網頁開始。關於「網頁三巨頭」－HTML、CSS、JavaScript 的敘述何者錯誤？
    - [x] HTML 負責管理使用者的操作行為
    - [ ] CSS 負責改變網頁外觀
    - [ ] JavaScript 負責網頁的動態效果


### 11-2｜網頁通通交給我！
這邊介紹的是除了 HTML 與 CSS 外的語言。


1. **JavaScript**   
    <p class="illustration">
    <img src="https://imgur.com/fcO9N7r.png" alt="JavaScript">
    JavaScript（圖片來源: <a href="https://papan01.com/archives/2020-01-01-you-dont-know-js-yet-1">Papan01's Blog</a>）
    </p> 
     
    JavaScript 與 HTML 及 CSS 並稱為網頁前端的三大巨頭，主要是網頁前端的程式語言，負責決定網頁與使用者的互動及瀏覽器的行為。除應用於網頁前端外，也會用後端的資料庫系統。

    JavaScript 屬於物件導向及直譯式語言，對於新手而言上手較為容易，雖然...我學的第一門語言不是這個，這好像是我第七還八個學得語言！？
     
    相關學習資源有 [MDN的JS手冊](https://developer.mozilla.org/zh-TW/docs/Learn/JavaScript) 與 [w3schools](https://www.w3schools.com/js/default.asp)。

2. **Go（又稱 Golang）**  
    <p class="illustration">
    <img src="https://imgur.com/KzbF7OA.png" alt="Golang">
    Golang（圖片來源: <a href="https://www.kevinwu0904.top/blogs/golang-error/">极客熊生</a>）
    </p> 

    Golang 是由 Google 所開發出來的程式語言，以語法靈活、簡潔、清晰、高效為特徵，最特別的是它有垃圾回收功能。
    
    Golang 常應用伺服器端，用來開發大型軟體；除此之外，也廣泛應用在網路上，Google、Facebook、騰訊及百度等，都是 Go 的使用者。這個語言還滿容易學習的，雖然我沒學完 XDDD，學到一半跑去學前端 Vue。

    相關學習資源有 [Go技術手冊](http://www.golang-book.com/books/intro) 與 [Go練功好所在](https://tour.golang.org/welcome/1)。

3. **PHP**  
    <p class="illustration">
    <img src="https://imgur.com/97CkgB7.png" alt="PHP">
    PHP（圖片來源: <a href="https://laravel-news.com/php-releases-on-hold-for-two-weeks">Laravel News</a>）
    </p> 
    
    對，就是人人鄙視、位於鄙視鏈最末端的 PHP XDDD
    
    PHP 多用在網頁後端開發，對新手來說上手快，但因沒有清晰的設計哲學而遭人詬病，且地雷較多，讓人又愛又恨 XDDD 話雖如此，在後端語言蓬勃崛起之際，PHP 仍佔穩市場的第一，諸如：Facebook、 WordPress 與市面上超過 27 % 的部落格（CMS）也是以 PHP 為架構。
    
    相關學習資源有 [PHP慕課網](http://www.imooc.com/learn/54)。
    
    <p class="illustration">
    <img src="https://i.imgur.com/NHnWQ0D.png" alt="即時更新的後端排名">
    即時更新的後端排名（圖片來源: <a href="https://w3techs.com/technologies/overview/programming_language/all">w3techs</a>）
    </p>    

4. **Ruby**  
    <p class="illustration">
    <img src="https://imgur.com/kpQbWY9.png" alt="Ruby">
    Ruby（圖片來源: <a href="https://www.geeksforgeeks.org/interesting-facts-about-ruby-programming-language/">GeeksforGeeks</a>）
    </p> 
    
    Ruby 是個靈巧且方便又實用的程式語言，而專攻網頁後端 Ruby on Rails 正是 Ruby 爆發成長的催化劑。使用 Ruby on rails 的著名例子有 Github、Airbnb...等。
    
    相關學習資源有 [笨方法學Ruby](http://lrthw.github.io/)。


#### 單元測驗
1. **想必你應該了解哪些是常見的製作網站的程式語言了吧！請選出下列不是常用來製作網頁的語言喔！**
    - [x] Swift
    - [ ] JavaScript
    - [ ] PHP


### 11-3｜想學 C 語言請找我！
必須說 C 語言幾乎是所有語言的實現基礎，如果精通了，再去學其他的語言也很容易上手，這也是資工系第一個學會的語言。

<p class="illustration">
    <img src="https://i.imgur.com/HDwg1qB.jpg" alt="C語言家族">
    C語言家族（圖片來源: <a href="https://www.pcschool.com.tw/blog/it/c-cplusplus-csharp">巨匠電腦</a>）
</p> 
    
1. **C 語言**  
    C 語言是非常強大且重要的程式語言，其編碼方式和邏輯運影響了後來的眾多程式語言，例如 C++、C#、Objective-C、Java、JavaScript...等。因此有人說：「學程式就從 C 語言開始」，從它開始學習可以打好基礎，並使其它程式語言的學習會更輕鬆！
    
    它最常被用在作業系統的編譯器中，擅長處理低階語言，例如 Microsoft Windows、macOS、Linux、Unix 等。

    相關學習資源有 [C語言技術](https://openhome.cc/Gossip/CGossip/index.html)、[美麗C世界](http://dhcp.tcgs.tc.edu.tw/c/index.htm)。

2. **C++**  
    C++ 和 C 語言一樣都是我們資工系必修課，它承襲 C 語言的優點及特性，並導入了 C 語言所沒有的物件導向特性。C++ 可用於軟體開發、搜尋引擎及操作系統上，常見的 Office 系列軟體與 Google 就是以 C++ 撰而寫成的。
    
    相關學習資源有 [C++台大開放式課程](http://ocw.aca.ntu.edu.tw/ntu-ocw/index.php/ocw/cou/101S112)、[給新手的C++練習](https://codingsimplifylife.blogspot.com/2016/04/c.html)。

3. **C# （發音為 C sharp）**  
    C# 則是微軟開發的程式語言，採用全物件導向設計的高階語言。C# 多用於開發網頁、服務平台及 Windows 應用上，如 Evernote。除此之外，C# 還能夠在 Unity 裡面寫遊戲！
    
    相關學習資源有 [微軟C#學習](https://www.microsoft.com/net/tutorials/csharp/getting-started)。


#### 單元測驗
1. **你掌握 C 語言這個大家族的特性了嗎？請選出錯誤的敘述喔！**
    - [x] C ++ 語言是低階語言
    - [ ] C 最常使用在作業系統的編譯器中，它擅長處理低階語言
    - [ ] C# 多用於開發網頁


### 11-4｜寫 APP 選我就對了！
基本上分成兩大支派 - Android 跟 IPhone 系列，不同的開發方向會選擇不同的程式語言。

<p class="illustration">
	<img src="https://i.imgur.com/uAXfZdj.png?1" alt="Android vs Mac">
    Android vs Mac（圖片來源: <a href="https://news.xfastest.com/apple/96118/ios-android/">XFastest News</a>）
</p> 


1. **Objective-C**  
    Objective-C 是 C 語言家族的延伸，也是物件導向的程式語言。
    
    Objective-C 在實務上就是用來寫 IOS 和 Mac 應用程式的程式語言，不太適合做為通用型語言來學。如果真要學習程式設計，不建議從 Objective-C 入手，建議先學過一門通用型語言後再來學習，除非真要專門開發 Mac 相關程式。但若要針對 Mac 開發，我會建議先學 Swift。

    相關學習資源有 [Objective-C官方文件](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)。
    
2. **Swift**  
    Swift 也是由 Apple 發布的，根據 Apple 聲稱 Swift 的效能優於 Objective-C，並具備快速、現代、安全及互動特點，且相較於 Objective-C 它的語法清晰度高，也更加簡單，更多[學習Swift的理由](https://www.pcschool.com.tw/blog/it/ios-app-engineer)。
    
    目前 Apple 有意讓 Swift 和 Objective-C 共存於公司的作業系統上。因我不開發 Mac 不太確定它的開發現況，但根據課程所說，目前的 iOS APP 仍多以 Objective-C 開發，且工作職缺仍以 Objective-C 為主；不過就看到一些排名與報導，Swift 的排名似乎在 Objective-C 之上？
    
    相關學習資源有 [Swift技術手冊](https://numbbbbb.gitbooks.io/-the-swift-programming-language-/)。

3. **Java**  
    Java 好像是我學的第三個語言？它屬於即時編譯語言，並具備跨平台及物件導向的特性，是一個應用廣泛的程式語言。它可以用來開發 Andorid 的 APP、跨平台的桌面應用程式及遊戲開發，知名的案例有 Gmail 及 Minecraft。
    
    相關學習資源有 [Java技術手冊](https://caterpillar.gitbooks.io/javase6tutorial/content/c1.html) 與 [Java語言技術](https://openhome.cc/Gossip/Java/)。


#### 單元測驗
1. **可以製作 APP 的程式語言是不是比你想像中的多呢？針對製作 APP 常用的程式語言，請選出錯誤的敘述。**
    - [ ] Swift、Objective-C 都是常用來開發 iOS APP 的程式語言
    - [x] Objective-C 也是程序導向語言的程式語言
    - [ ] Objective-C 只能用來開發 Apple 相關產品


### 11-5｜資料分析請找我！
1. **R**  
    R 是資料科學界的巨星，市佔率高達 61%。它簡單易上手，可以將複雜的資料做出數據分析、統計和圖形模型...等資料視覺化呈現。
   
    相關學習資源有 [R的學習歷程](https://liao961120.github.io/2018/01/31/RlearningPath.html)。

2. **Python**  
    Python 是近年來最流行的程式語言，還被譽為「最容易學習的程式語言」。Python 除了廣為人知的資料分析外，還能夠應用於網頁、遊戲及應用程式開發。Instagram、Youtube、Spotify 都是用 Python 所寫的；不過要我說最熱門的用法莫過於機器學習。

    若是沒有明確目標的初學者，也可以將 Python 列入第一考量，它的易懂性高，能快速入門！相關學習資源有 [Python基礎教程](http://www.runoob.com/python/python-tutorial.html) 與 [Python新手學習](https://pala.tw/begin-to-learn-python/#resources)。
 

<p class="illustration">
	<img src="https://i.imgur.com/V4WLoex.png" alt="Python 與 R 語言比較">
    Python 與 R 語言比較（圖片來源: <a href="https://medium.com/marketingdatascience/python與r語言-行銷資料科學家必備技能之一-40878e0a5de9">Marketingdatascience｜Medium</a>）
</p> 


#### 單元測驗
1. **原來分析資料不只可以用 Excel！針對常見的資料分析語言，請選出正確的敘述。**
    - [x] Python、R 都是常用來分析資料的程式語言
    - [ ] Swift 是最近竄起的分析資料的程式語言，可以用來做出數據分析、統計和圖形模型
    - [ ] JavaScript 不只可以應用於資料分析，還能用於網頁、遊戲及應用程式開發


### 11-6｜如果你還不知道要選什麼，或許你可以參考這篇
從前面看下來可以發現程式語言的面向不盡相同，這邊將面向大致分成了五大類：**網頁（Web）**、**行動裝置（Mobile）**、**電腦（PC）**、**積體電路（IC）**及**資料分析（Data Analytics）**，並依面向整理出相關的語言建議：

<p class="illustration">
    <img src="https://i.imgur.com/yTq4GWT.jpg" alt="即時編譯">
    即時編譯（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 
    

如果完全沒目標，只是想寫寫看程式，會見從從 Python 或是網頁前端開始入手；如果要專業學習，可以考慮從 C/C++ 開始打好基礎。另外課中推薦了一篇文章以供參考：[〈新手學程式－我該學哪種語言？新手常見毛病我中招了嗎？〉](https://blog.hiskio.com/the-problems-for-freshman-of-coding/)


#### 單元測驗
1. **認識那麼多程式語言，你是不是快搞混了呢？現在就讓我們來釐清一下！請選出正確的敘述。**
    - [ ] PHP 除了可以開發網站，也可以用來分析數據
    - [x] Java 應用面向廣泛，可以開發網站、APP，也可以開發桌面應用程式
    - [ ] JavaScript 應用面向也非常廣泛，可以開發網站、APP還能分析數據


## CH 12｜程式技術，放眼未來
這個章節有點雜，從前端談到了 APP。

### 12-1｜CMS 跟 CVS 一樣好方便
內容管理系統（Content Management System，CMS），是一種網站**後台管理系統**，會將網站內的資訊組織並加以整合。

CMS 的使用更偏向管理者，而非開發者。它通常會提供一特定網址，使用者毋需撰寫任何程式，僅需登入管理網站，並準備好欲發布的文字與圖片就能把資料更新到網站。這項技術可使使用者將精力專注在內容上，著名的 WordPress 其本質上就是一款內容管理系統。

一般來說內容管理系統會具備下列優點：
1. **有效管理文章**   
    通常有個視覺化界面的後台，讓使用者可以更輕鬆的利用套件來完成，不需要時也能將它關閉。
2. **輕鬆更新外觀**  
    通常是外觀與內容分離的模式，就像紙娃娃一樣，可以很能容易地為它換上新衣服。
3. **活躍社群支援**  
    是開源程式碼，當使用者越多時，便擁有越多的相關程式可以使用，更不用害怕委託的工程師跑了或公司倒了！

<p class="illustration">
    <img src="https://i.imgur.com/aDShZHU.jpg" alt="CMS">
    CMS（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 


#### 單元測驗
1. **想要迅速架設網站，記得先搞懂 CMS！下列哪些不是 CMS 的特性呢？**
    - [ ] 中文為內容管理系統，是一種網站後台管理系統，會將網站內的資訊組織並加以整合
    - [x] 通常有一個後台可以登入，只要利用瀏覽器便能輕鬆編輯網站，但需要撰寫程式語言
    - [ ] CMS 是開源程式碼，當使用者越多時，你便擁有越多的相關程式可以使用


### 12-2｜熱門的 3 大 CMS
承前節，這邊介紹 3 個家喻戶曉的 CMS 。

1. **Wordpress**  
    Wordpress 是其中最知名的 CMS 工具，目前全球有三成的網站都是使用 WordPress 製作的，且在 CMS 系統的市佔率更接近六成。
    
    WordPress 的優勢在於其學習曲線的平緩與多語系的支援；另外 WordPress 為開源免費的，得益於此發展出龐大的社群，而衍生許多主題、外掛、網站架構版型可供選擇。
    
    是說，我當初有想說用 WordPress 來放置這些筆記，但它需要額外負擔**個人網址**和**虛擬主機費用**，我承認我懶得管這些，所以後來才選擇依附在平台的 Github Pages。  

2. **Joomla!**  
    雖說是市佔率第二名，但其市占率僅有 6 %，與 WordPress 的 60 % 相比只能是望塵莫及。

    Joomla! 學習曲線相對平緩，上手難度僅次於 WordPress。而其特色在於具備與多特別商業布景，適用於中小型企業的場域，此外它的操作後台，可將常用的功能做成下拉式及按鈕，畫面漂亮且易理解。

3. **Drupal**  
    市占率有近 4 %。相較於上面兩者來說，它的學習曲線較陡，需要學習大量的知識，像是 HTML、PHP 及物件導向...等。
    
    但它的優勢則在於它是一個功能特殊又可以無限擴充的網站，常被用來架設大型、多用戶的網站像是競選網站、交友網站及共筆媒體...等。

<p class="illustration">
    <img src="https://i.imgur.com/Tvv7GbD.jpg" alt="熱門的 3 大 CMS">
    熱門的 3 大 CMS（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 


#### 單元測驗
1. **你了解熱門 CMS 的特性嗎？請選出它們錯誤的敘述喔！**
    - [ ] Wordpress 最初是設計為部落格架站的工具，現在慢慢變成 CMS
    - [ ] 使用 Drupal 需要學習大量的知識，像是 HTML、PHP 及物件導向等，學習曲線較陡
    - [x] WordPress 不適開源的程式碼，這也是其相關資源稀少的原因之一


### 12-3｜軟體工程師也有分前後嗎？
這邊簡單介紹下，**前端（front-end）**、 **後端（back-end）** 與 **全端（full-stack）** 工程師，三者間各自負責的工作內容：

1. **前端**  
    前端工程師負責**造房子**，<mark>負責整體網頁畫面的呈現與點擊按鈕或區塊的互動</mark>，要熟悉 HTML、CSS/Sass、JavaScript 以及其他相關開發框架等。
    
    簡單來說，在使用上**看得到**的部分通常就是由前端開發者負責。


- **後端**  
    ==後端工程師則是負責資料傳遞與網站的溝通層面==，也就是負責**打地基**的，它會將前端的動作傳遞到後端並從資料庫撈取相對應的資料，最後呈現在頁面上。除了熟悉後端程式語言與演算法，也要了解前端相關知識、網路通訊協定、大資料處理，甚至是伺服器的設定...等。

    簡單來說，使用上**看不見**的部分通常是由後端開發者負責。


- **全端**  
    簡單來說就是**前、後端都要會**！說好聽一點是**通才**的概念，更形象點的說明就是工具人啦 XDDD 它需要工程師必須兼具網站的的多項知識，包含伺服器、資料庫系統維護、版面調整、使用者介面與體驗、客戶與商業需求...等，根據不同任務性質投入前或後端的開發與維護。
    
    至於全端這個概念，出自 2012 年開源技術大會（open source convention，OSCON）上一位 Facebook 員工說出「他們只雇用全端開發者」。在場的軟體工程師 Laurence Gellert，因而寫下 [〈What is a Full Stack developer?〉](http://goo.gl/EVMa32)一文，提及一位全端開發者應該具備的條件。
    
    課程中建議全端工程師可在進一步掌握資訊安全方面的知識。

<p class="illustration">
    <img src="https://i.imgur.com/tiGrDK2.png" alt="三種軟體工程師工作內容與職務比較">
    三種軟體工程師工作內容與職務比較（圖片來源: <a href="https://www.worker360.com.tw/blog/developer">窩課360</a>）
</p> 


#### 單元測驗
1. **前端與後端工程師的職責可是天差地遠，別搞錯囉！請選出正確的工程師職位與其相對應的職責。**
    - [ ] 前端工程師：負責「打地基」，包含伺服器與資料庫的應用，例如儲存使用者登入的資料與行為記錄
    - [x] 前端工程師：負責整體畫面的呈現與點擊按鈕、區塊的互動行為
    - [ ] 後端工程師：負責「造房子」，包含軟體（應用程式或網頁等）架構、樣貌與互動功能
    - [ ] 後端工程師：負責改變網頁外觀，如：顏色、排列等視覺元素


### 12-4｜什麼？還有全端設計師？
全端設計師並不是在說全端工程師，而是在說**寫程式的設計師**。這...就別斜槓來跟我們搶飯碗了，拜託...

目前多數公司會分成 UI/UX 跟前端工程師兩種職務，但為精簡人力、省下溝通成本、或是為了增加團隊的合作性...等原因，而衍生出全端設計師一職。設計師除了原先 UI/UX 的設計外，還會參與甚至包辦前端開發，有時還會身兼 PM 並參與行銷...等，協助整個專案的執行。

在這邊的全端偏向，**具備且熟悉多樣技能，來解決更多問題**的意義，而不是一開始前後兼顧的意思。不過說實話無論是全端工程師或是全端設計師，在職場上的定位較為模糊，這職位可能會是領導者、顧問，不過更有可能是工具人，哪裡有需要就去哪裡支援 XDDD

 
<p class="illustration">
    <img src="https://i.imgur.com/WM2AvfE.jpg" alt="全端設計師職能範圍">
    全端設計師職能範圍（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 


#### 單元測驗
1. **沒想到設計師也有「全端」的吧！下列關於全端設計，何者正確呢？**
    - [ ] 全端設計師需要會「前端工程師」、「後端工程師」、「UI、UX 設計師」的工作內容
    - [x] 當一個設計師學會程式，開始可以參與甚至包辦前端開發，協助整個專案的執行，就稱作「全端設計師」
    - [ ] 能包辦「UI 設計」、「UX 設計」的設計師稱作「全端設計師」


### 12-5｜響應式設計
響應式設計（Responsive web design，RWD），又稱適應性網頁、響應式網頁設計、回應式網頁設計、多螢幕網頁設計，為一種網頁設計技術。

最大特色在於：<mark>在不同解析度下改變網頁頁面的布局排版</mark>，讓同一個網站在不同裝置上自動調整，無論電腦、平板或手機，都能呈現最好閱讀的畫面，是因應移動裝置的使用者數量大增而發展出的方法。

<p class="illustration">
    <img src="https://i.imgur.com/CcBEaFa.jpg" alt="在不同解析度下改變網頁頁面的布局排版">
    在不同解析度下改變網頁頁面的布局排版（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 


RWD 是目前最受歡迎的網頁設計之一，其優點如下：

1. **方便閱讀，提升使用者體驗**  
    這是 RWD 最大的優點，也是它的最重要目的。隨著移動裝置使用的興起，其尺寸與規格如雨後春筍般大量地涌現出來，而RWD 採用液態排版技術（Liquid Layout）可克服這問題。
    
    液態排版技術（Liquid Layout），可讓網站內容，包含圖片、LOGO、主選單、文字內容、網頁標尾...等，自動隨螢幕的尺寸來進行縮放、移動...等重新排列，以找到最合適的呈現方式。如：在電腦寬螢幕上圖片、文字內容以橫向水平排列、遇到手機螢幕圖片改以縱向垂直呈現，以避免呈現過於密麻的字、或避免需要縮放，方便使用者可以輕鬆瀏覽。
 
 
2. **輕鬆管理，大幅節省成本**   
    對管理者來說，僅維護單一版本，會是更加輕鬆，能節省不少時間和人力成本！此外僅製作單一版本，當然能節省開發費用。
 
 
3. **提升排名，有效加速行銷**  
    如果行動版具有獨立版的網址，那麼就很有可能會分散網站在搜尋排名上的力道，因為同樣的內容卻存在著兩個網址，那麼搜尋排名計分也就被分攤掉了。
    
    因此會推薦採用 RWD 整合成單一網址，使瀏覽次數不分散，能直接優化排名（SEO）、帶動行銷效益。針對這一點，Google 在官方網誌[〈Recommendations for building smartphone-optimized websites〉](https://developers.google.com/search/blog/2012/06/recommendations-for-building-smartphone)一文中也推薦此用法：
    
    > Sites that use responsive web design, that is, sites that serve all devices on the same set of URLs, with each URL serving the same HTML to all devices and using just CSS to change how the page is rendered on the device. **This is Google's recommended configuration.**


<p class="illustration">
    <img src="https://i.imgur.com/cZaIVoH.png" alt="圖解傳統網頁與RWD網頁製作方式">
    圖解傳統網頁與RWD網頁製作方式（圖片來源: <a href="https://www.ibest.tw/page01.php">愛貝斯</a>）
</p> 


#### 單元測驗
1. **原來同一個畫面可以在手機、電腦等裝置都完美呈現全都靠 RWD！請選出錯誤的敘述。**
    - [ ] RWD 中文為響應式設計，它可以讓同一個網站在不同裝置上自動調整畫面
    - [x] RWD 雖然方便，但採用 C 語言的技術，所以製作門檻較高
    - [ ] RWD 提升了行動友善度，而且採用單一網址，瀏覽次數不分散，因此能直接優化網頁排名（SEO）


### 12-6｜來認識 APP 吧！
應用程式（application, App）廣義上來說電腦上的各種軟體都算；但由於行動裝置普及，現在提到 App 多半是指行動裝置的應用程式，也就是 Mobile App。

課程這邊針對 Mobile App 進行分類：
1. **原生型 Native App**  
    目前最常見的應用程式，就像是在電腦上安裝使用的軟體一樣。原生 App 是為特定作業系統所開發的應用程式，因此無法跨平台操作，例如同為 Facebook Android 版本的就無法在 iOS 上使用，必須針對不同的作業系統開發相應的 App，因此開發與維護都比較麻煩。

2. **網頁型 （Mobile）Web App**  
    基本上就是 RWD 的網頁，因透過瀏覽器開啟應用程式，故具備跨平台操作的特性，也不需在意更新問題，但也因要透過需瀏覽器才能使用，運行速度與整體效能比較受限。

    <p class="illustration">
        <img src="https://i.imgur.com/NnCpWxg.jpg" alt="原生型與網頁型 Facebook App 畫面比較">
        原生型與網頁型 Facebook App 畫面比較（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
    </p> 
    
    撇除使用體驗與運行速度，原生型與網頁型兩者的差異在於<mark>原生型「不依賴瀏覽器運作」</mark>，所以最上面不會出現網址列。

3. **混合型 Hybrid App**  
    混合型 App 是前兩者的綜合發展體，結合原生型與網頁型的優點，可以用 HTML5 技術方便開發，又不用依賴瀏覽器使用，跨平台開發特性也方便維護，運行流暢度與成本則介於 Native App 和 Web App 之間。
   
    開發者可以降低開發成本，又能透過審核上架至 App 商店，觀察使用者下載情形與偏好，可以說是同時也具備方便開發與商業考量的意義。


<p class="illustration">
    <img src="https://i.imgur.com/ksBCuCN.jpg" alt="原生型與網頁型 Facebook App 畫面比較">
    原生型與網頁型 Facebook App 畫面比較（圖片來源: <a href="https://codefree.hiskio.com/courses/5">課程</a>）
</p> 


#### 單元測驗
1. **那麼多種 APP，千萬別搞混了！請選出下列正確的選項。**
    - [ ] 網頁型 App 是目前最常見的應用程式，是專為特定作業系統（手機目前以 iOS、Android 為主）所開發的應用程式
    - [ ] 原生型 App 與電腦上的網頁並沒有太大的區別，只不過到了行動裝置上多了 RWD
    - [x] 混和型 APP 結合了原生、網頁型 APP 的優點，且可以用 HTML5 技術開發，又不用依賴瀏覽器使用


### 12-7｜認識結對程式設計
結對程式設計（Pair programming），是一種敏捷開發的方式。

望文生義，就是開發者兩兩「結對」，他們會從設計構想到執行的過程都在同台電腦上共同工作。兩人中一人負責輸入程式碼，稱之為**駕駛員（driver）**，另一人則負責審查駕駛員所輸入的每一行程式碼，稱之為**觀察員（observer）** 或 **領航員（navigator）**，兩者可隨時互換角色。如此兩兩交互討論、切磋並共事，再相互交換角色的工作模式，就稱之為結對程式設計。

<p class="illustration">
    <img src="https://i.imgur.com/lNfkNpS.png" alt="結對程式設計">
    結對程式設計（圖片來源: <a href="https://zh.wikipedia.org/zh-tw/结对编程">維基百科</a>）
</p> 

這種工作模式我看過我前主管跟另外一位組員做過幾次，不過他們不會整個專案都這們幹，因為太耗時了！通常都是在開發新功能或重構才會這麼做 XDDD

<p class="paragraph-spacing"></p>

它這邊提了 3 個結對程式設計的優點，有點懶直接把課程內容整個貼過來：
 
1. **知識傳播**  
    結對程式設計是一個**很直接的技術和經驗傳授管道**，類似一對一家教，能對實作上的進行意見反饋。透過這樣的方式，是經驗共享和知識共享的效果最大化。
    
    因此，有些公司會採取這樣的模式，例如建立自己的培訓系統，或是聘請專家來培育新生代。
 
2. **個人提升**  
    在擔任不同角色時，能夠**透過不同的思考層面與角度去衡量程式的不同設計面向**。

3. **產出質量**  
    因結對時，會有觀察員負責審查所輸入的程式碼。因此錯漏與 bug 能在第一時間及時修正，如此便能在成果初次呈現之時就提升其質量並且省去最後 debug 步驟的精力和時間。

<p class="paragraph-spacing"></p>

至於結對程式設計的效率則與結對的人選有關：

1. **若是強強組合**  
    在雙方都是有經驗有技術的前提之下，都各自固有寫程式習慣與思維，其實也沒有什麼討論的必要。如此一來，便形成人才閒置，如果拆開來工作，便能獨自完成兩個項目，提升了產出效率。

2. **若是弱弱搭配**  
    確實能夠達到討論互助及增進團體凝聚力的效果，但比較難要求短時間的生產效率。

3. **若是強弱搭配**  
    普遍來說，經驗和能力上較缺乏的初級工程師搭配一個資深工程師，經合作後能夠培養獨當一面的能力。不僅能提升技術，更能精進從不同角度的問題思考與架構。
    
    但這種模式對於初學者來說獲益匪淺，但對於資深開發者卻只是付出遠大於收穫，難免出現失衡的。因此，對於結對程式設計一直以來都有著兩極的評價和看法。


對於結對程式設計究竟適不適用，須考量到的層面包含每個人的個性、習慣與工作模式，甚至是環境和產出內容的難易程度和所預設的目標都會影響到運作效果！


#### 單元測驗
1. **下列關於結對程式設計，何者敘述錯誤呢？**
    - [ ] 工作模式為程式開發者兩兩「結對」，使用同一台電腦工作，從設計構想一路到執行編碼的過程中都是共同合作
    - [x] 兩人會先分配誰要開發哪一部分，接著分頭撰寫程式
    - [ ] 其中一人負責輸入程式碼，另外一人則負責審查輸入的每一行程式碼


### 12-8｜雲端的 3 大服務
雲端運算（cloud computing），是一種基於網際網路的運算方式，通過這種方式，將共享的軟硬體資源與資訊可以按需求提供給各種終端和裝置，使用服務商提供的基礎設施進行運算。雲端運算最終的目標：讓資源透過網路變得像是日常生活的水、電一樣唾手可得。

課程接下來介紹了，3大服務模式：**IaaS、PaaS、SaaS**，我這邊就偷懶不做筆記了，要看的話就參考之前的筆記：[〈雲端計算 IaaS、PaaS、SaaS 與 FaaS〉](https://cynthiachuang.github.io/Difference-between-IaaS-PaaS-SaaS-and-FaaS)。


#### 單元測驗
1. **原來平常使用的雲端有分那麼多種服務模式！請選出下列正確的敘述喔！**
    - [x] 雲端共有 IaaS、PaaS、SaaS 這 3 種服務模式
    - [ ] 我們常用的 Google 雲端硬碟屬於 PaaS
    - [ ] SaaS 將硬體資源虛擬化，提供虛擬伺服器、虛擬主機等。它可以大幅省去硬體採購投資、部署設備的成本


### 12-9｜雲端的 4 種部署方式
上一節提過雲端運算提供運算資源的 3 種服務型式，而這節則介紹 4 種雲端運算部署模式：

1. **公有雲（Public Cloud）**  
    簡而言之，公有雲是由銷售雲端服務的廠商所成立，針對大眾提供的雲端運算服務，一般用戶或企業都能使用。一般耳熟能詳的雲端運算服務，絕大多數都屬於公有雲的模式，例如針對企業用戶的 AWS、 Azure、 Salesforce.com...等，或是針對個人的 Dropbox、 Evernote...等，都是公有雲服務。
    
    公有雲使取得運算資源的門檻降低，使任何人都可以輕易取得龐大的運算資源。但這系統通常以帳號密碼登入作為存取控制機制，安全疑慮較高，容易衍生出其他的安全問題。
 
2. **私有雲（Private Cloud）**  
    與公用雲相反，只服務企業內部或受信任的人員，可以是由該組織自己管理，或由第三方廠商管理，它可以部署在企業內，也可部署在企業外。
    
    由於私有雲主要是建立在企業管控的網路，可以擁有絕對的網路控制權，比起公有雲的安全性高；但也因是企業自建，經濟效益與規模比較受限，運算資源較難快速地擴張。
 
3. **社群雲（Community Cloud）**  
    這種架構是由目標或利益相仿的組織，例如有共同的任務、安全要求、政策與法規，共同成立，以服務擁有相同需求的群體，例如電子病歷交換雲端平臺。

4. **混合雲（Hybrid Cloud）**  
    此種架構架構，是結合多個獨立的雲端運算架，藉由結合公、私兩端的優點，以彈性利用不同架構間安全、規模等優勢。混合雲是一種較新的概念，主要是為了因應單一架構無法滿足人們需求。舉例來說，企業會建立私有雲，但受限於規模可能會將較無安全疑慮的服務交由公有雲。
 

<p class="illustration">
    <img src="https://i.imgur.com/rSjme8l.png" alt="雲端的 4 種部署方式">
    雲端的 4 種部署方式（圖片來源: <a href="https://www.ithome.com.tw/article/93013">iThome</a>）
</p> 


#### 單元測驗
1. **下列雲端部署方式與其敘述，何者有誤？**
- [x] 私有雲是最常見的部署方式，使用上彈性空間大但安全疑慮較高
- [ ] 混合雲結合公、私兩端的優點
- [ ] 公用雲一般用戶或企業都能使用，但通常僅以帳號密碼登入作為存取控制機制


### 12-10｜雲端的 5 種運算特徵
雲端運算除了有 3 種服務模式、 4 種部署模式外，還有 5 種運算特徵：

1. **隨選所需自助服務（On-gemand self-service）**  
    我覺得有點類似之前[講師介紹 Serverless 架構](https://cynthiachuang.github.io/Serverless-Use-Cases-Study-Notes-01/)所提到─**按需執行**。
    
    使用者無須經由供應商，可以直接按照需求（如：伺服器運作時間、網路儲存空間、計算資源...等）取得服務以執行。此特點能滿足**隨用隨付（Pay-As-You-Go,PAYG）模型**中資源配置的需求。

2. **網路存取方式多樣化（Broad network access）**  
    因為網路資源變得容易取得，使用者隨時都可以透過各種裝置（如電腦、平板、手機等）隨時隨地存取資訊。

3. **共用資源池（Resource pooling）**    
    這個特徵是在說明雲端運算的**共享性**，它會將運算資源整合為一個共用資源池，並能根據使用量自動調配虛擬與實體主機的資源，以提供給多位使用者。

4. **快速、彈性地調整服務（Rapid elasticity）**    
    就是**按需擴展**的概念，這是指雲端運算服務可以快速、彈性部署及釋出運算資源。

5. **可行量與計價的服務（Measured service）**  
    就是**按使用來計費**的概念。使用者或服務供應商能夠直接取得、管理資源的使用情況，並按照使用資源計費。 
    
    
#### 單元測驗
1. **針對雲端的運算特徵，下列敘述何者錯誤？**
    - [ ] 用戶能在不需要供應商介入的情況下自行配置像是伺服器運作時間、網路儲存空間等運算資源
    - [ ] 配置運算資源時，架構既可快速擴增，亦可快速縮小，在一些情況下甚至能夠自動化運作
    - [x] 無法自動控制運算資源的使用（儲存空間、處理器、頻寬與使用人數等）但能使資源最佳化



## CH 13｜換一顆軟體工程師的邏輯腦：演算法

### 13-1｜演算法是什麼？如何演算？
演算法是**一連串解決問題的邏輯步驟**。當我們將邏輯步驟用程式撰寫後，電腦就會計算給出答案，從輸入到輸出結果的過程，就是演算法的範圍。而過程會是：輸入（input）→ 演算（algorithm）→ 輸出（output）。

<p class="illustration">
<img src="https://i.imgur.com/b192fuE.png" alt="演算法基本概念">
演算法基本概念（圖片來源: <a href="https://www.eisland.com.tw/Main.php?stat=a_yg2Y8Hs">英語島</a>）
</p> 

舉例來說，當上班肚子餓時（輸入 input），我們依序確認時間點，以及適不適合出門（演算法 algorithm），再根據結果，決定要如何解決（輸出 output），這中間的起承轉合，就是演算法的過程。

<p class="illustration">
<img src="https://i.imgur.com/vp4PAhZ.jpg" alt="演算法與流程圖">
演算法與流程圖（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 

演算法必須完全滿足下列 5 項特性：
1. **輸入（input）**  
    **0 個或多個輸入資料**，這些輸入必須有清楚的描述或定義。
2. **輸出（output）**  
    **至少會有一個輸出結果**，不可以沒有輸出結果。
3. **明確（definiteness）**  
    每個指令或步驟必須是**明確的指令**。
4. **有限（finiteness）**  
    必須在**有限**步驟後必產生結果，不然會產生無窮迴圈。  
5. **有效（effectiveness）**  
    步驟清楚可行，阿不然要怎麼算？


#### 單元測驗
1. **經過圖解後，你是不是更了解演算法了呢？請選出下列關於演算法錯誤的特性吧！**
    - [x] 演算法有 5 種特性，分別是輸入、輸出、時效性、有限、有效
    - [ ] 必須完全滿足演算法 5 種特性，才稱得上邏輯運算
    - [ ] 演算法的運作過程為輸入→ 演算→ 輸出


### 13-2｜常用的演算法表示法
常用來表示演算法的方法有兩種，簡單來說不是畫出來就是寫出來：

<p class="illustration">
<img src="https://i.imgur.com/8PELm5K.png" alt="構思演算法">
構思演算法（圖片來源: <a href="http://ms2.ctjh.ntpc.edu.tw/~luti/107-2week04_1.htm">初探演算法</a>）
</p> 


1. **畫出來：流程圖（flow chart）表示法**  
    流程圖會運用各種方塊、圖形、線條和箭頭，精簡表達演算的順序和步驟。且圖示本身，就表達資料性質、處理方式...等等。常用的流程圖符號如下：

    <p class="illustration">
    <img src="https://i.imgur.com/vBFQ1wX.jpg" alt="常用的流程圖符號">
    常用的流程圖符號（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
    </p> 

2.  **寫出來：虛擬碼（pseudo code）**  
    另一個常用的方法就是透過虛擬碼。它是介於自然語言與程式語言之間的描述語言，結構上與程式接近，但並無嚴格的語法規則，一般只要結構清晰、方便閱讀，就可視為虛擬碼。

恩，我找到了個範例：輸入一個正整數N，判定是否能被 2 整除，如果 N 能被 2 整除是偶數，否則 N 是奇數。將這敘述分別使用兩種方法撰寫：

<p class="illustration">
<img src="https://i.imgur.com/kAZGb6q.jpg" alt="流程圖與虛擬碼">
流程圖與虛擬碼（圖片來源: <a href="https://sites.google.com/cycu.org.tw/python">Python 程式設計與邏輯思維</a>）
</p> 


#### 單元測驗
1. **原來演算法的標示方法不只有一種！你了解這些標示方法了嗎？請選出正確的敘述吧！**
    - [x] 常見的標示法有「流程圖標示法」、「文字敘述表示法」兩種
    - [ ] 流程圖運用各種方塊、圖形、線條和箭頭，精簡表達演算的順序和步驟。例如：菱形就表示起訖符號
    - [ ] 文字敘述法透過圖案本身，就能表達出資料性質、處理方式



### 13-3｜打造演算法的鋼筋水泥
程式語言，或說演算法的控制結構共分為：**循序**、**條件**及**重複結構**。是說，之前在寫 [Python 的流程控制](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#ch-4流程控制)時好像提過類似的事情？


#### 循序結構（sequence structure）  
**依其先後順序逐一完成執行**。最常見的結構，一般程式撰寫時沒有特別設定，多是這個結構。例如：
```python=
使用自動販賣機，選好飲料 → 選擇付款方式 → 付錢 → 獲得飲料
```

<p class="illustration">
<img src="https://i.imgur.com/5eDB3FV.jpg" alt="循序結構">
循序結構（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 

 
#### 條件結構（conditional structure）
**依條件是否成立**，來決定執行的動作。

<p class="illustration">
<img src="https://i.imgur.com/1kZuSKd.jpg" alt="條件結構">
條件結構（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 


#### 重複結構（repetition structure）
**在條件成立時，會反覆執行動作**。例如：

<p class="illustration">
<img src="https://i.imgur.com/6IdDqDI.jpg" alt="重複結構">
重複結構（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 


#### 單元測驗
1. **演算法有「循序、條件、重複」三種基本結構，請選出下列正確的配對。**
    - [x] 循序結構：由上至下依序執行，用於需要依序執行多個動作時。結構為：指令 1 → 指令 2 → 指令 3 →......
    - [ ] 條件結構：依條件是否成立，來決定執行的動作，例如：while....則執行某動作
    - [ ] 重複條件：在條件成立時，會反覆執行動作。例如：if...then...else



## CH 14｜認識軟體工程師的開發工具

### 14-1｜一應俱全的整合開發環境 IDE
整合開發環境（Integrated Development Environment，IDE），不過習慣上大家都直接稱呼 IDE。
 
IDE 是一種輔助程式開發人員開發軟體的應用軟體。通常包含程式語言編輯器、編譯器/直譯器、除錯器、還有使用者圖形界面...等，IDE 會將各自獨立的工具集結在一起，方便一站式使用（課程說像是三合一咖啡 XDDD）。
 
過往的 IDE 會針對特定的程式語言量身打造（如：Visual Basic），但現今的 IDE 往支持多種語言的方向在推動（如：Eclipse 及 Microsoft Visual Studio）。 All-In-One 的設計雖然方便，但並非所有工具的使用都能遂心如願，所以有些開源的 IDE，可以讓工程師混搭，以創造更合適的 IDE。


<p class="illustration">
<img src="https://i.imgur.com/GfC5mzr.jpg" alt="IDE 整合開發環境">
IDE 整合開發環境（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 


- **IDE 的優點**  
    1. **適合新手**：不需在終端機下安裝及設定各種前置作業，能節省建立開發環境的成本，並加速對開發環境的了解。
    2. **執行程式碼**：可以在 IDE 內直接執行一段程式碼，不需離開編輯器。
    3. **高亮程式碼**：將原始碼以不同顏色顯現，讓我們閱讀更方便。
    4. **自動排版**：當輸入 if 或 while 時，編輯器知道下一行要縮進。 


#### 單元測驗
1. **如果你平常有在接觸程式領域，那麼你一定很常聽到 IDE 吧？相信經過我們的講解，你應該有了初步的了解。請選出下列正確的敘述吧！**
     - [x] IDE 中文為整合開發環境，它將程式開發所需的軟體工具集結在一起，通常包含程式語言編輯器、編譯器 / 直譯器、除錯器、還有圖形使用者界面
     - [ ] 它最大的缺點就是不能在 IDE 內直接執行一段程式碼，而是需要離開編輯器
     - [ ] IDE 的 ALL IN ONE 設計相當方便，完全沒有任何缺點


### 14-2｜熱門的整合開發環境
這節介紹些熱門的 IDE：

1. **Visual Studio**  
    > 完整的 [Visual Studio介紹及載點](https://visualstudio.microsoft.com/zh-hant/vs/)

    簡稱 VS，是由微軟開發的 IDE，是目前應用廣泛的 IDE 之一。它同時能在 Windows、Linux 和 macOS 作業系統上執行，還能支援 C++、C#、.NET、F#、JavaScript、Ruby、Go 及 Python 等程式。

    我記得這是我大學教授指定的 IDE，不過我大學之後就在也沒用過了 XDDD 不過需要注意的是，這邊介紹的 Visual Studio 而不是 Visual Studio Code，兩者的定位並不一樣。VS 是 IDE，而 VS Code 則僅是單純的程式碼編輯器，不過說真的最新的 VS Code 加上那龐大的擴充套件，幾乎可以取代 IDE 來用。

2. **Eclipse**  
    > [Eclipse 下載教學](https://www.kjnotes.com/devtools/80)

    Eclipse 由 IBM 開發，本身是一個框架平台，但因為外掛程式的支援，因此許多軟體開發商以 Eclipse 為框架，開發自己的 IDE。它最初是用來開發 Java，後來也有人透過外掛程式，來開發 C/C++、PHP、Ruby、JavaScript、R 及 Python。
    
    這個是我曾經最愛的 IDE :heart_decoration: ，我通常拿它來開發 Java，不過我讀完研究所後就在也沒裝過它了 XDDD

3. **Xcode**  
    > [Xcode 下載教學](https://medium.com/彼得潘的-swift-ios-app-開發問題解答集/安裝xcode-8需要macos-10-11-5以上-2e0f2d5557f3)

    這個我就沒有用過了。它只能運行在 Mac OS X 作業系統上，是提供開發人員用來開發蘋果的相關應用程式的 IDE，能支援 C、C++、Fortran、Objective-C、Java、Python、Ruby 和 Swift 等程式語言。

4. **IntelliJ**  
    > [IntelliJ 下載](https://www.jetbrains.com/idea/download/#section=linux)

    這是我目前慣用的 IDE，是由 JetBrains 所釋出。它有提供社區版本以及商業版本，像是 Android Studio，就是 Google 基於 IntelliJ IDEA 的社區版本發展而來。IntelliJ 本身主要是針對 Java 的 IDE，但針對其他語言，如： JavaScript、TypeScript、Ruby、Go 及 Python...等，可由外掛程式載入 IntelliJ 使用。
    
    
5. **雲端開發環境**  
    另外一種我常用的開發方式，是用 SaaS，也就是[雲端開發環境](https://www.inside.com.tw/article/4177-best-online-ides-let-you-code-anywhere)，它可以免除繁瑣複雜的前置作業。不過也因為它是 SaaS，所以程式碼會儲存在供應商的儲存服務中，所以我通常是用來快速驗證，或是臨時寫程式時才會用。

<p class="paragraph-spacing"></p>

<p class="illustration">
<img src="https://i.imgur.com/RP7vuw2.png" alt="IDE 整合開發環境">
熱門的 IDE 比較（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 


是說看到這些 IDE 害我又想到傳說中的**鄙視鍊**，Vim 或 text editor 的鄙視用 IDE 的工程師、 Android Studio 或 IntelliJ IDEA 的鄙視用 Eclipse 的工程師...等。這些各式各樣的鄙視鍊還滿有趣的，有興趣的可以去查查。

 
<p class="illustration">
<img src="https://i.imgur.com/FES6dqv.png" alt="工具版鄙視鍊">
工具版鄙視鍊（圖片來源: <a href="https://segmentfault.com/a/1190000012510841">SegmentFault 思否</a>）
</p>


#### 單元測驗
1. **關於下列熱門的 IDE，請選出錯誤的敘述。**
    - [x] Xcode 是 Windows 開發人員所提供的 IDE，用來開發微軟的相關應用程式，且僅能運行於 Windows 作業系統上
    - [ ] Visual Studio 簡稱 VS，是一款全功能整合開發平台，不過僅相容 Windows 和 macOS X 系統
    - [ ] elipse 由 IBM 開發，本身是一個框架平台，但因為外掛程式支援，所以許多軟體開發商以 Eclipse 為框架，開發自己的 IDE


### 14-3｜程式語言的框架
課程說的有點文言，先別管它，我用自己的理解寫一次好了。

框架，也就是 Framework，它像是個積木，開發者已經建構出大多數的元件了，甚至建築好大致的骨架後，一旦你有應用需求（Application）就只需在框架之上，依照需求去做修改與實踐，以實現客製化。如：PyTroch 跟 Tensorflow，就是機器學習中兩大框架，他們有撰寫公開的骨幹網路模型、損失函數、優化函數及衡量指標，當你要實做你的網路架構時，你只需要從中挑選並組成所需網路即可。


<p class="illustration">
<img src="https://i.imgur.com/sSJvh5p.png" alt="程式語言的框架">
程式語言的框架（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 

 
現階段框架為程式領域中公認前瞻性的技術，並且因應 IT 轉型的需求，成為近年來技術潮流的新秀。框架的出現產生了最主要提供了以下的便利性：

1. 讓開發者省去初始編寫架構的時間和精力，得以專注於核心開發上。
2. 透過自動化系統矯正錯誤，不再需要為繁雜的語法費心力。


#### 單元測驗
1. **其實不是所有的語法都要一個個字輸入，而是有更快的方法及工具－框架，來提升效率喔！針對框架的特性，請選出錯誤的敘述。**
    - [ ] 它可以讓開發者省去編寫架構的時間與精力
    - [ ] 由一連串代碼庫所組成，功用為簡化特定輸入的程式語言語法
    - [x] 無法透過系統自動化矯正錯誤


### 14-4｜JavaScript 的網頁開發框架
可能之前提到前端，因此在這章介紹網頁前端開發的框架。

這邊介紹的前端框架，多基於 JavaScript 所開發的，因 JavaScript 的開發痛點在於各瀏覽器的相容性，而透過 JavaScript 框架除了具備上述框架的優點外，還能輕鬆地克服跨瀏覽器問題。

框架跟程式語言的發展史一樣，都會依照開發用途、使用者體驗...等因素，逐漸進行迭代，發展至今 JavaScript 框架大多皆已發展地相當完整且成熟。下列介紹幾個普遍使用的 JavaScript 框架：
 
1. **JQUERY UI**  
    這是是一套 JavaScript 函式庫，它提供抽象化、可自訂主題的 GUI 控制項與動畫效果。課程提到了幾個特點：
    1. 現今各大主流跨瀏覽兼容
    2. 鏈式操作及選擇器使用方便，語法更簡潔
    3. 前端網頁的基本插件種類豐富
    不過鏈式操作應該不算是 JQUERY UI 的特點？而是 JQUERY 本身的特點？

2. **Vue.js**  
    我之前學過得一套框架，可以去翻翻之前的[筆記](https://cynthiachuang.github.io/Vue-Study-Notes-Contents/)。它具備：
    1. 學習容易，可快速上手
    2. 加入 Laravel 中，可支援些許項目
    3. 快速啟動便利性高的命令列介面（CLI）工具

3. **React**  
    React 主要是在做前端的渲染的，課程介紹它優點包含：
    1. 使用者滿意度最高的 Javascript 框架
    2. 充分學習 React.js 後，可轉移至 React Native 開發客戶端
    3. 可支援 JSX 的新語法
  
    這邊有個問題，雖然習慣上在介紹前端框架時候都會提到 React，但它其實並**不是框架而是函式庫**。 嚴格說來，React 和 react-router、react-redux、redux-saga 等結合起來才是個框架，React 本身只一個前端渲染的函式庫而已。
    
    <p class="illustration">
    <img src="https://i.imgur.com/wKcjkLv.png" alt="React 是函式庫不是框架">
    React 是函式庫不是框架（圖片來源: <a href="https://zh-hant.reactjs.org/">React 官網</a>）
    </p> 


在未來的趨勢裡，框架扮演著規範和定義的角色，它能以更為簡潔的語法取代許多繁瑣的步驟，避免錯誤設定，讓開發者投入更核心、更重要的開發工作。


#### 單元測驗
1. **使用 JavaScript 開發最大的困難點在於各個瀏覽器的相容性，而使用框架就能輕鬆解決跨瀏覽器的相容問題。請選出下列關於 React 的正確描述**
    - [ ] 可加入 Laravel 中，支援些許項目
    - [x] 充分學習 React.js 後，可轉移至 React Native 開發客戶端
    - [ ] 現今各大主流跨瀏覽器兼容，且前端網頁的基本插件種類豐富
    - [ ] 不可支援JSX的新語法


### 14-5｜認識函式庫－動態與靜態函式庫
這邊開始介紹函式庫 library，它是一個由許多程式碼集結而成的程式碼集合體，負責幫助程式設計實現各種不同的功能，就像是名副其實圖書館一樣，它內建豐富典藏以供查詢取用。 

[之前在編譯語言](#10-3高階語言的內建翻譯機)的時後有提過：
> 編譯語言會透過連結器將一個或多個目的檔與**靜態函式庫**連結，產生可執行檔。最終要執行程式時，只需要透過載入器將可執行檔載入記憶體，並與**動態函式庫**連結，不需重新翻譯。

其中所提到的靜態函式庫與動態函式庫，就是本節所要介紹，它們依<mark>協助運行程式的方式</mark>不同而有不同的特性：

1. **靜態函式庫（Static library）**  
    又稱靜態連結函式庫（Statically-linked library），在編譯期間由編譯器與連結器將它整合至應用程式內，並製作成目的檔以及可以獨立運作的執行檔，因此執行檔毋須考慮電腦上函式庫檔案是否存在及版本問題。
   
    但在編譯階段載入多個函式庫，會導致執行檔肥大，而且需要更新時要整個重新編譯再重新連結，會大量消耗記憶體與系統資源。 

2. **動態函式庫**  
    這東西在 Windows 上稱為動態連結函式庫（Dynamic-link library，DLL），而在 UNIX 或 Linux 上則稱為共享函式庫（Shared Library）。函式庫會獨立於執行檔之外，只有當執行程式時才被統載入使用，可以節省記憶體資源、提升開發效率；但有能遇到執行端所存在的函式庫與程式要求版本不相容的狀況，導致程式無法正常執行，也就是所謂的 DLL 地獄。

<p class="illustration">
<img src="https://i.imgur.com/8T1oDFa.jpg" alt="兩種函式庫比較">
兩種函式庫比較（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 


#### 單元測驗
1. **若依協助運行程式的方式區分，可分為動態、靜態函式庫，可別搞混它們的特性了！請選出下列哪些為靜態函式庫的特性**
    - [x] 即使在不同環境下無需函式庫也可以執行檔案
    - [ ] 在 UNIX 或 Linux 系統上則是稱作共享函式庫
    - [ ] 載入多個函式庫時，執行檔會很小，且需更新程式時，要整個重新編譯再重新連結，只會稍微消耗記憶體與系統資源
    - [ ] 函式庫檔獨立於執行檔，在執行程式時才被作業系統載入使用，可以節省記憶體資源


### 14-6｜認識函式庫－標準與第三方函式庫
函式庫除了依照協助運行程式來區分函式庫外，還可以依==輔助程式語言相關使用==來區分：

1. **標準函式庫（Standard Library）**  
    像是<mark>程式語言基本補充包</mark>，在某些情況下，程式語言規格說明中會直接提及該函式庫；通常與程式語言一起合稱為「ＯＯＯ標準函式庫」，例如：Ｃ標準函式庫、Ｃ++標準函式庫等。

2. **第三方函式庫（Third-party Library）**  
    可以想像成是<mark>程式語言進階擴充包</mark>，含許多可以實現其他延伸功能的外部函式的庫。例如：jQuery 是 JavaScript 非常受歡迎的第三方函式庫。


#### 單元測驗
1. **若依輔助程式語言相關使用區分，可分為標準、第三方函式庫！請選出下列哪些為第三方函式庫的特性**
    - [ ] 像是「程式語言基本補充包」，通常與程式語言一起合稱為「ＯＯＯ標準函式庫」，例如：Ｃ標準函式庫
    - [x] 像是「程式語言進階擴充包」，含許多可以實現其他延伸功能的外部函式的庫


### 14-7｜帶來無限便利的 API
之前，寫過一篇關於 [API 的網誌](https://cynthiachuang.github.io/Android-SDK-and-API/#api-application-programming-interface應用程式介面)可以先去看看。

在課程中則是用了餐廳點餐的過程為例：
1. 進入餐廳看菜單
2. 向服務生點餐（提出需求）
3. 服務生向廚房點餐（要求）
4. 服務生送餐點過來（回應）  

當你點完餐，服務生會到後台傳遞這個訊息，餐點製作完成後，再由服務生送上餐點。在這個過程中，服務生的角色，既非生產者，也非需求方，而是在於傳達，好比 API 傳遞訊息、回傳資料的概念。

<p class="illustration">
<img src="https://i.imgur.com/qufvInY.jpg" alt="兩種函式庫比較">
兩種函式庫比較（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 

由於 API 經濟的興起，越來越多的企業通過 API 將自己企業的功能和數據開放提供第三方人員有償或是無償使用，這樣的開放可以直接對企業利潤帶來引響，或是擴大自身在生態圈影響力，並且很快的完成創新。

舉例來說，在 Facebook 可以看 Youtube 影片、在部落格可以嵌入 Google map，都是拜 API 之賜。因為 Youtube、Google map 提供 API 服務，所以工程師不必自己架設影音網站，也不用自己創造全新地圖，就能串接 API 擴充，享用對方的服務。


#### 單元測驗
1. **身為工程師，你一定得認識 API！請選出關於 API 正確的敘述吧！**
    - [x] 為 application programming interface 的縮寫，中文為「應用程式介面」
    - [ ] 主要功用，在於對外獲得雙方的資訊



## CH 15｜程式語言的資料型態


### 15-1｜Python 的資料型態
這章我整個跳過，因為在[〈CodeFree｜喝一杯咖啡，輕鬆學 Python〉](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#ch-3資料型態)一節中已經做過相同的筆記了...我不想在做一次 Orz


#### 單元測驗
1. **如果你想學習程式，那麼你一定要懂資料型態。下列哪些是正確的配對呢？**
    - [ ] bool：分為 True、False、and、or
    - [x] float：指有小數點的數值
    - [ ] int：字串
    - [ ] tuple：整數


### 15-2｜資料運算
這節就講**算術運算子**、**邏輯運算子**、**賦值運算子**，不過多東西我在〈CodeFree｜喝一杯咖啡，輕鬆學 Python〉那篇有整理過了，所以就跳過一部分囉～

> 想了解算術運算子，點[這裡](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#3-3資料型態數字)；想了解邏輯運算子，點[這裡](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#4-6符號-and-與-or)

<p class="paragraph-spacing"></p>

而 **賦值運算子（Assignment operators）** 之前沒有專門整理過，這邊在稍微整理下。

賦值運算子是種比較特別的運算子，通常是用來將已存在物件賦予給其他相同類別的物件。在大多數的程式語言中，賦值運算子是以**等號（=）** 來表示。不過雖說是等於，但跟數學上的等於略有不同，在計算時他們會將右邊的運算完成後，賦值給左邊的變數，左右邊跟數學上是相反過來的。除了基礎的賦值外，還有衍生出複合的賦值運算子：

<p class="illustration">
<img src="https://i.imgur.com/00eqHzx.jpg" alt="兩種函式庫比較">
兩種函式庫比較（圖片來源: <a href="https://codefree.hiskio.com/courses/6">課程</a>）
</p> 
 
 
#### 單元測驗
1. **運算元與運算子在寫程式時也非常常用。請選出下列正確的配對與敘述。**
    - [ ] 邏輯判斷運算子：用於計算加減乘除
    - [ ] 賦值運算子：又稱為比較運算子，用於比較大小關係
    - [ ] 計算運算子：用於賦與運算元數值
    - [x] 運算元（Operant）就是運算資料，可以是數字或算式


### 15-3｜流程控制－條件判斷控制
在章節 [13-3](https://cynthiachuang.github.io/Hiskio-Codefree-Computer-Science-2/#13-3打造演算法的鋼筋水泥) 時，提過演算法的控制結構共分為：循序、條件及重複結構。

在這節會來說明條件判斷控制...恩，我又要來跳過...有需要的還是去看看〈CodeFree｜喝一杯咖啡，輕鬆學 Python〉那篇，我把兩邊對應的章節給列在下面了：
1. 單向選擇 if > [傳送門](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#4-2if-流程控制)
2. 雙向選擇 if···else > [傳送門](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#4-3else-例外處理)
3. 多向選擇 if···elif···else > [傳送門](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#4-4elif-多重流程控制判斷)
4. 巢狀 if > [傳送門](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#4-5巢狀-if-流程控制)


#### 單元測驗
1. **請選出錯誤的配對。**　
    - [ ] 單向選擇 if：當判斷式成立時即執行該程式區塊
    - [ ] 雙向選擇 if···else：當條件成立時可以執行 if 下的程式區塊，而當不成立時就執行 else 下的程式區塊
    - [x] 巢狀 if：當第一個條件成立時可以執行 if 下的程式區塊，不然就看第二個條件是否成立，成立時就執行該 elif 下的程式區塊


### 15-4｜流程控制－迴圈控制
繼續跳過，還是上對照連結：
1. for 迴圈 > [傳送門](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#ch-5for-迴圈)
2. while 迴圈 > [傳送門](https://cynthiachuang.github.io/Hiskio-Codefree-Python/#ch-6while-迴圈)


#### 單元測驗
1. **請選出正確的配對。**
    - [ ] for 迴圈：以「for」這個字組成，在兩字之間會放入我們自訂的變數
    - [x] 巢狀for迴圈：巢狀迴圈也就是迴圈裡又包著迴圈，外層迴圈每動作一次，內層迴圈就會把自身的整個迴圈執行一遍，執行完畢後才跳回外層迴圈
    - [ ] break：不會讓迴圈結束，只跳過迴圈內 continue 後面的剩餘敘述，再回到迴圈的判斷式


### 15-5｜函式
還是跳過，直接上[傳送門](https://cynthiachuang.github.io/Hiskio-Codefree-Python#ch-9函數-function)。


#### 單元測驗
1. **為了避免落落長的程式碼，你就會使用到函式！請選出下列正確的敘述。**
    - [ ] 宣告函式：函式在使用前不需要宣告，但會先定義出函式的名稱、參數及它要做的事情
    - [x] 呼叫函式：在寫完函式後，我們要使用函式時必須「呼叫（call）」函式，才能讓函式開始運作
    - [ ] 函式可以避免不同的東西一直複製貼上在你的程式碼


## CH 16｜過去三十年人工智慧剛起步 v.s. 未來三十年人工智慧 IQ 10000

### 16-1｜人工智慧的巨量資料學習法
害我想到我的草稿夾，有一篇關於人工智慧、機器學習與深度學習之間的網誌，我決定等那邊篇寫完再來給連結...恩...希望有吧 XDDD


#### 單元測驗
1. **現在你終於搞懂人工智慧、機器學習、深度學習之間的關係了！讓我們來複習一下吧！請選出下列正確的選項**
    - [ ] 人工智慧：機器學習的分支，目標在於以電腦解決問題，發展至今日，舉凡醫療、交通等領域，都已經廣泛運用
    - [x] 機器學習：簡稱 ML，人工智慧的一個分支，它能讓電腦從海量的資料中自行學習，並且自己逐步精進
    - [ ] 深度學習：人工智慧的分支。以人工神經網路為架構，讓機器朝人類更進一步發展，把機器學習推向實際運用


### 16-2｜生活中的人工智慧
這節在介紹目前人工智慧的應用範疇，並提人工智慧是未來科技的趨勢：「未來只剩兩種人，告訴電腦該怎麼做的人，以及被電腦告知該怎麼做的人」。是說，我這邊偷懶幾乎是用課程原文：


#### 專家系統（Expert System）
專家系統採用事前做足應對措施的方式，也可以說是人工智慧系統中的知識庫。以針對性的問題作為出發，進而發想出他的解決方法。不過一個很大的缺點是，只限制於專家有預先設想過的答案或應對，侷限性相較比較大，有尚未考慮過的問題是專家系統中的一個缺漏。

 

#### 電腦視覺（Computer Vision）
電腦視覺以開啟電腦及攝影機的視覺為目的，也就是讓電腦學會「看」，以便進行資料的圖像化。並且在人工智慧系統中，期望以電腦視覺去取得圖像中的更多資訊。常見的應用有：無人駕駛、圖像監測、工業機器人和圖像資料庫等。

 

#### 語音辨識（Speech Understanding）
語音辨識，也可稱做自動語音辨識（Automatic Speech Recognition，簡稱 ASR），以人類所釋出的語音轉為對應文字的系統，生活中常見的手機語音輸入法就是一個應用的例子。其他應用包含：語音撥號、語音導航、室內裝置控制及語音文件檢索...等。近期更是出現了語音轉語音的翻譯系統，也是仰賴語音辨識的智慧系統。

 

#### 機器人應用（Robotic Application）
機器人取代人力的運作模式從數十年前就已開啟。目前機器人的應用在許多工業領域中發光發熱，包含倉儲物流、航太產業、汽車製造，以及木工與建築...等。許多搬運及組裝的工作透過機器人組成的自動化流程，不但能省時省力，更能使得流程穩定且順暢。

 

#### 類神經網路（Artificial Neural Network）
自 2010 年代後興起的人工智慧浪潮中，其中一個就是機器學習（Machine Learning）的模式。透過深度學習後，電腦以創造了數學及計算模型來處理人工智慧種種複雜行為。類神經網路系統架構就如同人類的神經元和受器的建構關係，在接收指令與執行指令之間完成行為模式。而電腦最基本的數學運算及統計系統就是建構在類神經網路的應用上。

 

#### 智慧型代理人（Intelligent Agent）
人工智慧中的智慧型代理能利用快速且精確的運算能力達成目標。同時具有三大特性：自主能力（autonomous）、反應能力（reactivity）和交互作用能力（interactive）。智慧型代理人藉由他的自主主體性在進行決策時，能針對事項進行反應，並且各個代理人之間得以溝通、合作來達成最終目的。常見的例子像是蘋果手機的 Siri、聊天機器人 Chatbox 都是智慧型代理人的一種。

 

#### 自然語言處理（Natural Language Understanding）
所謂的自然語言相對應程式語言，是人類為了溝通所形成具結構性語法的語言。而自然語言的處理主要透過演算法讓電腦理解自然語言，並實際執行指令。運作方式是將語法架構拆解以便理解並生成語言。從簡單普遍的拼字檢查、關鍵字搜尋，到複雜的機器翻譯和語意分析都是依賴自然語言處理的功能達成的。



## 其他連結
1. 課程內容：
    1. [Codefree - 電腦科學（上）](https://codefree.hiskio.com/courses/4)
    2. [Codefree - 電腦科學（中）](https://codefree.hiskio.com/courses/5)
    3. [Codefree - 電腦科學（下）](https://codefree.hiskio.com/courses/6)
2. 課程筆記：
    1. [CodeFree｜喝一杯咖啡，輕鬆學電腦科學 - 上](https://cynthiachuang.github.io/Hiskio-Codefree-Computer-Science-1)
    2. [CodeFree｜喝一杯咖啡，輕鬆學電腦科學 - 下](https://cynthiachuang.github.io/Hiskio-Codefree-Computer-Science-2)



## 參考資料 
1. 卞哲琛, Et al.。[煞氣a精靈部隊之期末 ALL PASS 版本計算機概論重點整理](https://sites.google.com/site/nutncsie11037/home)。檢自 nutncsie11037 (2022-07-12)。   
2. 陳思翰 (2011-03-04)。[認識IPv4與IPv6的差異](https://www.ithome.com.tw/tech/92046)。檢自 iThome (2022-07-29)。
3. 協同撰寫。[超文本傳輸協定](https://zh.wikipedia.org/zh-tw/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE) 。檢自 維基百科 (2022-07-29)。   
4. bettywutalk。[主網域、子網域、網域寄放和附加網域是什麼？對SEO有何影響？](https://bettywutalk.com/blog/domains/#3_xin_ding_ji_yu_ming_New_gTLD_New_Generic_Top-Level_Domain)。檢自 矽谷獨角獸學院 (2022-07-29)。   
5. [什麼是子網域？](https://tw.godaddy.com/help/what-is-a-subdomain-296)。檢自 GoDaddy TW (2022-07-29)。 
6. Hannah Lin (2021-05-22)。[從零開始學資安 — 什麼是資訊安全?. 網路上的資訊安全文章普遍深澀所以想寫簡單易懂的一系列文章。](https://medium.com/hannah-lin/從零開始學資安-什麼是資訊安全-75a7a208e8db)。檢自 Medium (2022-08-02)。 
7. Ugnė Mikalajūnaitė (2020-10-25)。[駭客是什麼](https://nordvpn.com/zh-tw/blog/heike-shi-shenme/)。檢自 NordVPN (2022-08-02)。 
8. Po-Ching Liu (2017-12-28)。[編譯語言 VS 直譯語言](https://totoroliu.medium.com/編譯語言-vs-直譯語言-5f34e6bae051)。檢自 Medium (2022-09-02)。 
9. 吳其勳 (2011-06-21)。[徹底了解Cloud Computing｜部署模式](https://www.ithome.com.tw/article/93013)。檢自 iThome (2022-09-16)。 
10. 呂同塵 (2018-11)。[圖解演算法：演算法是什麼？](https://www.eisland.com.tw/Main.php?stat=a_yg2Y8Hs)。檢自 英語島 (2022-09-20)。 
11. 莊泉福, 黃慶福 (2018-11)。[Python 程式設計與邏輯思維](https://sites.google.com/cycu.org.tw/python/ch-5-演算法虛擬碼與流程圖)。檢自 自然組 (2022-09-22)。 



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-10-07</summary>
  <ul>
    <li>2022-10-07 發布</li>
    <li>2022-09-29 完稿</li>
    <li>2022-07-29 起稿</li>
  </ul>
</details>
