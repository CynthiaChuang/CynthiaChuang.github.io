---
title: 【Serverless 應用案例賞析筆記】 03. Serverless 在物聯網領域中的應用
date: 2020-04-07
modified: 2020-04-07
categories:
- Study-Notes
- Cloud-Computing
tags:
- Other Online Courses
- Serverless 應用案例賞析
- Fass/Serverless
- OpenWhisk
- IoT
--- 

<center> <img src="https://i.imgur.com/I9XGCWZ.png" alt="投影片封面"></center>
<center class="imgtext">投影片封面（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-02.pdf" class="imgtext">課程講義</a>）</center>
<br>
  
這星期介紹 serverless 在物聯網的應用。

<!--more-->
<br>

<div class="alert warning">
<div class="head">文中的 IBM 文件連結</div>
一樣文件中出現的 IBM 文件連結若無法顯示或不存在，可以試著把 doc 語系換成英文後，再重新連結一次試試看。
</div>

<br><br>
 
## 回顧


發現講師很厲害，每次的回顧都講得很雷同，應該有寫稿？所以細節直接[第二堂課程筆記](/Serverless-Use-Cases-Study-Notes-02#回顧)就好，我就不再複製貼上了。


看不過這也算是強化了記憶，每次看到這裡就自動浮現了：

<div class="blockquote-center">
無需管理、按需執行、按需擴展、按使用來計費
</div>

<br>

在上一堂課集中講解了 API 的部份，這一堂則會專注在事件驅動。
 
 
以人類來比喻事件驅動，是指通過嗅覺、視覺、味覺、聽覺或者觸覺感知事物，傳遞給大腦，指揮身體作出反應。而在電腦世界中則是，各種設備、感知器，甚至應用程式與系統都會產生各種事件，這些事件交由電腦處理，並引發後續響應動作。

其實當我了解這概念的時候發現它真的很適合用來在 IoT 中應用。在 IoT 中，各類感知器會收集大量的資料，這些資料會被送往某個 Sink/Coordinator 或後端儲存。當資料達到特定條件或閥值時，會被認定為產生特定動作或事件，而這樣的事件又觸發程式進行處理，操控設備做出響應。IoT 整個過程與 Serverless 的事件驅動過程不謀而合。

<center> <img  height="100" src="https://i.imgur.com/S5xaGmF.jpg" alt="IoT 應用"></center>
<center class="imgtext">IoT （圖片來源: <a href="https://pixy.org/97299/f" class="imgtext">pixy</a>）</center>
<br>

在將 Serverless 使用到 IoT 應用中，我們可以定義事件、規則和動作，使得事件被及時處理，設備及時響應，還可以使用 FaaS 對收集上來的資料進行格式轉化和索引，使得存儲資料可以更容易被獲取和分析，也可以使用定時呼叫對一段時間內收集的資料做分析和歸檔，甚至可以將 Serverless 平台可以延伸到邊緣計算環境中，這種方式將使得在雲端和邊緣使用統一的方法進行動態數據的處理成為可能。
 
以上偷懶直接節錄自[官方的 Blog](https://mp.weixin.qq.com/s/mDjTDcV-V25YRXbSFLNYyg) XD


 
<br><br>
 
## Feed (資訊來源/訂閱源)

雖然講師認為這是回顧，但其實之前只在說明[觸發器 (Trigger)](/Serverless-Use-Cases-Study-Notes-01#OpenWhisk-程式編輯模型)提過 Event provider 的概念。

那時說到，在 OpenWhisk 中觸發器僅某類別事件的具名頻道，在 OpenWhisk 之外會有一個 Event provider，由它監聽事件。當 provider 監聽到更新事件時，再由它通知 Trigger。
 
而根據 IBM 的[Cloud Functions 術語](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-about#cloud-functions-)文件，對於 Feed 的描述是：   

> **Feed**  
> A feed is a convenient way to configure an external event source to fire trigger events that can be consumed by Cloud Functions. For example, a Git feed might fire a trigger event for every commit to a Git repository.
> 
>**資訊來源/訂閱源 (Feed)**  
>資訊來源是一種簡便的方法，可配置外部事件來源來發動 Cloud Functions 所使用的觸發程式事件。例如，Git 資訊來源可能在每次確定至 Git 儲存庫時，都會發動觸發程式事件。
 
個人覺得是提到的 Event provider 與這次說的 Feed 是類似的啦 XD

<br>

### 簡介
<center> <img src="https://i.imgur.com/1g4VI6S.png" alt="Feed "></center>
<center class="imgtext">Feed （圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-02.pdf" class="imgtext">課程講義</a>）</center>
<br>

回到正題，Feed 它是管理外部事件觸發 Trigger 這件事，包括了創建 Trigger、刪除 Trigger、暫停觸發與取消暫停...等生命週期的動作事件。

在自然界或在限縮到 IT 系統中存在著各式各樣的事件，但嚴格來說，<span class="highlighting">外部事件的變動與 Serverless 無關</span>，如果 Serverless 想知道這些變動進而觸發觸發器，必須依賴 Feed 來捕獲這些事件。

Feed 主要負責從外部事件來源來接收事件，不管所接收事件原本的資料結構、格式、傳輸協定為何，它最終都能轉換成 Serverless 平台所需的協議與格式。

<br>

可以使用以下三種體系結構模式來創建 Feed：<span class="highlighting">Hook</span>、<span class="highlighting">輪詢（Polling）</span> 和 <span class="highlighting">連接（Connections）</span>。 

1. **Hook**  
    顧名思義就是鉤子，讓你的服務與另一個服務有掛勾，通過使用另一個服務公開的 Webhook 設定 Feed。在這種結構模式中，Webhook 會被配置在外部伺服器上，它會直接用 URL 來執行 POST 操作觸發器。
    
    舉例來說，可以通過制定 Webhook 來監聽 Github 上的各種事件，例如：push。假如你希望有人上 code 時，本地端會自動拉下來，那麼你可以一個監聽 push 事件的 Webhook，此後每個這個項目有任提交新的 commit，都會被這個 Webhook 捕獲，這時就會發送一個 HTTP POST 觸發事前定義觸發器與動作，這個動作可以是 pull 的指令。
 
     這個方法的好處大概是不需要在外部維持任何一個長期的服務，而且設置還挺容易的，但就是有些系統並沒有提供 hook，可能需要單獨寫段程式去長時間運行，來監測系統是否有事件產生。 
     
    <div class="alert info">
    <div class="head">紀錄一下 Github 的設置文章</div>
     1. <a href="https://developer.github.com/v3/orgs/hooks/#receiving-webhooks">Organization Webhooks | GitHub Developer Guide</a> <br> 
     2. <a href="https://kknews.cc/code/l3qen3e.html">用github的webhooks實現項目自動化構建</a> <br> 
    </div>
    <br>

 
2. **Polling（輪詢）**  
    如果沒有 hook 或者你也可以，使用一個 Polling 方法，在 Serverless 上起一個定時的動作，定期發發送一次 request 來獲取新數據。這種模式相對容易建立，但是事件的頻率受到輪詢間隔的限制。若間隔時間太短，會造成 server 端的負荷。若間隔時間太長，也會同時造成收到的資訊有不即時的問題發生。

    <div class="alert info">
    <div class="head">Webhook 與 Polling</div>
     1. <br> 
     可以看看<a href="https://medium.com/@justinlee_78563/line-bot-%E7%B3%BB%E5%88%97%E6%96%87-%E4%BB%80%E9%BA%BC%E6%98%AF-webhook-d0ab0bb192be">這篇</a>餐廳與外送員的例子。
    </div>   
    <br> 
    

3. **Connections（連接）**  
    總覺的翻成連接有點詞不達意。不過它的意思是，若上面兩種都無法達成你的需求，例如你每次 Pooling 的時間較長不符合 FaaS 短暫的需求，那就可能需要起一個單獨的服務用於保持與 Feed 的持續連接。

    不過這個做起來感覺會非常麻煩，IBM 說它們 Cloudant changes 就是用這個做，晚點再來找找他們實做方式好了，先知道有這麼一個結構模式可以用來創建 Feed 就好。 
 
 
<br>

### Feed and Trigger 的差異
     
在找資料了時候[看到的](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-triggers#triggers_difference)，貼過來補充一下。

這邊寫到 Feed 和 Trigger 雖是緊密相關的，但在技術上是不同的概念。

**Cloud Functions**  
Cloud Functions 處理流入系統的**事件**。不過 Cloud Functions 是基於 Apache OpenWhisk 實做的企業版本，所以這句話應該可以理解成 OpenWhisk 處理流入系統的**事件**（！？）

<br>

**Trigger**  
指某類事件的名稱。每個事件屬於恰好一個 Trigger；若用比喻的方式來說明，Trigger 類似於基於主題的發布訂閱系統中的主題。規則則用來指示每當來自 Trigger 的事件到達時，就觸發 Trigger 所搭載的 action。

<br>

**Feed**  
Feed 是一種可以配置外部事件源來觸發 OpenWhisk 訂製事件的便捷方法。Feed 是全部屬於某個 Trigger 的事件的 stream。預先安裝的套件、可安裝的套件以及你自己定義的套件可能包含 Feed。Feed 通過 Feed 操作進行控制，該操作用於處理組成 Feed 的事件流的創建、刪除、暫停和恢復組成。Feed 操作通常通過使用管理通知的 REST API 與產生事件的外部服務進行交互。
 


<br>

## Internet of Things（IoT,物聯網）

 
<center> <img src="https://i.imgur.com/VBmiORr.png" alt="IoT"></center>
<center class="imgtext">IoT（圖片來源: <a href="https://www..com/cht/xcdoc/cont?xsmsid=0J178110000554951149&sid=0J182351851717589046" class="imgtext">昕奇雲端科技</a>）</center>
<br>

套句研究所教授說的物聯網就是<span class="highlighting">物物相連的網路</span>，周遭的物理設備可以通過網路（通常是無線網路）相連，比較常被討論的設備包含：
1. 環境感知器（無線感測網路）
2. 穿戴式設備
3. 工業界中油電腦控制的設備（智慧工廠）
4. 個人導航設備（車載網路、行動通訊網路、自動駕駛）

依照[維基百科](https://zh.wikipedia.org/wiki/%E7%89%A9%E8%81%94%E7%BD%91)所提到的，物聯網的應用領域主要包括：運輸和物流領域、工業製造、健康醫療領域範圍、智慧環境（家庭、辦公、工廠）領域、個人和社會領域等。

<br>

### IoT 應用特點

1. **異構的大數據**  
    由於感測器數量繁多，且種類不盡相同，例如在環境感知應用中，可能會灑上成千上萬個感測器用於收集溫濕度...等資料，由於感測器會不間斷的收集環境資料，且每種設備回報的資料結構又都不盡相同。
    
    因此一種應用可能會得到資料結構各異的大數據資料，在使用會彙整時需要針對不同的設備進行不同的處理。
    
2. **設備通常由 Gateway 接入**  
    雖說物聯網中有主從模式（Master-Slave）架構形成星狀拓墣（Star Topology），也有從點對點（Peer-to-Peer）衍生出的網狀拓樸（Mesh Networking Topology）、叢集拓樸（Cluster Networking Topology）。但這些感測器通常不會直接接入網際網路，畢竟通訊協定不一樣？
    
    感測器通常會將資料交由 sink / coordinator，再由它們轉交給 router ，進行資料聚合以及一些簡單的處理後，傳入互聯網送往存儲平台。
    
3. **資料通常在 Gateway 做初步處理**  
    有可能因為網路延遲、封包遺失等原因，導致資料亂序呈現，所以會在這邊做重新排序處理，也有可能會在這邊做一些隱私資料的遮蔽。

4. **資料通常被傳輸到雲端做存儲和分析**  
    在物聯網中邊緣節點因為成本的考量，恩...畢竟很多感測器灑出去就拿不回來，想想那荒山野嶺要拿的回還來還真難，更別說還有些感測器是放水流的...，因此它們計算能力通常不強，儲存空間也不大，因此繁複的運算通常送到後端來進行。


<br>

### 典型應用架構

<center> <img src="https://i.imgur.com/opaee1R.png" alt="IoT 典型應用架構"></center>
<center class="imgtext">IoT 典型應用架構（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-0.pdf" class="imgtext">課程講義</a>）</center>
<br>

稍微解釋這張圖，圖片最左端的是終端設備與 Gateway，它們最終透過 MQTT 將資料送往 IoT 平台，這個 IoT 平台會轉手資料送往消息中樞上去， 通過這個消息中樞會將資料路由到不同的系統與平台上去，例如：後端存儲平台、Stream分析平台...等。而送往後端存儲平台可以再串接數據分析格式化...等等。

<br>

而在這樣應用架構中，Serverless 可以應用在圖中的 ABCD 四處，這邊就這四種場景進行介紹。


**A. 輸入資料處理**
<center> <img src="https://i.imgur.com/W7QJKgk.png" alt="輸入數據處理"></center>
<center class="imgtext">輸入數據處理（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-0.pdf" class="imgtext">課程講義</a>）</center>
<br>

在消息進入 IoT 平台後，被消息中樞獲取後，Feed 可以去監聽 kafka 上的消息取得新進入的原始數據，轉交進入 OpenWhisk 平台進行如格式轉換、資料過濾、保存...等操作，處理完畢後在放到消息中樞的另一個 topic 上，讓它將資料轉交不同的系統與平台。

簡單來說，就是資料在進入後端系統前的前處理。

<br>

**B. 資料存儲後的處理**
<center> <img src="https://i.imgur.com/7SN8fWE.png" alt="資料存儲後的處理"></center>
<center class="imgtext">資料存儲後的處理（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-0.pdf" class="imgtext">課程講義</a>）</center>
<br>

也可以應用在，當 Feed 監聽到資料已經進入實體儲存時，我們可以去調用第三方服務去做分析、資料增強、與外部資料合併，也可以使用一個定期任務進行匯總，或把資料倒到機器學習的訓練集裡面去，並進行 re-train。

<br>

**C. Data stream 檢測**
<center> <img src="https://i.imgur.com/FyK3nRn.png" alt="Data stream 檢測"></center>
<center class="imgtext">Data stream 檢測（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-0.pdf" class="imgtext">課程講義</a>）</center>
<br>

在另外一個也可以放到 Data stream 裡面，在這邊資料流會不斷的通過，可以當它通過時去檢查資料是否錯誤，或是當通過流數或資料超過了閥值，則觸發相對應的動作，例如在車載的應用中，某個時間資料流數目超過閥值，表車輛數目繁多可能造成壅塞，所以把號誌改為綠燈希望舒緩阻塞情況。

<br>

**D. 邊緣計算上的 Serverless能力**
<center> <img src="https://i.imgur.com/r1Jjqyc.png" alt="Data stream 檢測"></center>
<center class="imgtext">Data stream 檢測（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-0.pdf" class="imgtext">課程講義</a>）</center>
<br>

最後這是一個待發展的趨勢，針對邊緣計算環境如 Gateway，提供基礎的資料處理能力，使之擁有統一的事件驅動能力。
 

<br><br>

## 真實案例與實做

真實案例這邊我就跳過了，感覺跟前面有些雷同，我就不重複寫了。
 
另外 Demo 的部份，可以直接看看這兩份文件
- [让智慧冰箱能够请求替换件 - IBM Developer](https://developer.ibm.com/cn/patterns/power-smart-fridge/)
- [通过 OpenWhisk 和 Watson IoT 提供主动客户服务](https://github.com/IBM/ibm-cloud-functions-serverless-iot-openfridge/blob/master/README-cn.md)

在這個範例中，他們其實是用一個 action 來監聽 mqtt 當作 Feed，其中 action 的 annotation 設為 feed true。


 
<br><br> 

## 其他連結
1. [Serverless 應用案例賞析筆記目錄](/Serverless-Use-Cases-Study-Notes-Contents)
2. 課程內容：影片（[IBM片源](https://mediacenter.ibm.com/media/03_Serverless+%E5%9C%A8%E7%89%A9%E8%81%94%E7%BD%91%E9%A2%86%E5%9F%9F%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8/1_9u4hn9xd) 、[youku教育片源](https://v.youku.com/v_show/id_XMzg4MTM4MDEzNg==.html?spm=a2hzp.8253869.0.0)）/ [講義](https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-02.pdf) / [Blog](https://mp.weixin.qq.com/s/mDjTDcV-V25YRXbSFLNYyg)

<br><br> 

## 參考資料 
1. IBM开源技术 (2018-10-16)。[Serverless应用场景之二：物联网](https://mp.weixin.qq.com/s/mDjTDcV-V25YRXbSFLNYyg) 。檢自 IBM开源技术 微信(2020-04-09)。
2. (2019-07-12)。[Cloud Functions 的運作方式 - Cloud Functions 術語](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-about#cloud-functions-terminology) 。檢自 IBM Cloud (2020-03-25)。
3. (2020-03-09)。[Creating custom event provider feeds](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-feeds_custom) 。檢自 IBM Cloud (2020-03-25)。
4. Justin Lee (2019-09-24)。[LINE Bot 系列文 — 什麼是 Webhook?](https://medium.com/@justinlee_78563/line-bot-%E7%B3%BB%E5%88%97%E6%96%87-%E4%BB%80%E9%BA%BC%E6%98%AF-webhook-d0ab0bb192be) 。檢自 Justin Lee - Medium (2020-04-09)。
5. (2020-03-09)。[Creating triggers for events](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-triggers) 。檢自 IBM Cloud (2020-03-25)。
6. (2020-03-09)。[Cloud Functions CLI](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create&locale=en) 。檢自 IBM Cloud (2020-03-25)。


