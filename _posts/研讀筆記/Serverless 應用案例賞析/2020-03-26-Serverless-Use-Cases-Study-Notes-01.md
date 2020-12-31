---
title: 【Serverless 應用案例賞析筆記】 01. Serverless 架構與 Apache OpenWhisk
date: 2020-03-26
is_modified: false
categories:
- 研讀筆記
- 雲端運算
tags:
- 其他課程平台
- 《Serverless 應用案例賞析》
- Fass/Serverless
- OpenWhisk
--- 

<center> <img src="https://i.imgur.com/Wn0R9Lh.png" alt="投影片封面"></center>
<center class="imgtext">投影片封面（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>
  
本堂課程作為此系列的準備課程，針對不了解 Serverless 的人，介紹 Serverless 概念、特點以及 Open Source Serverless 平台 Apache OpenWhisk 和基於它實做的企業版本 IBM Functions 的概況。

<!--more-->
<br><br>

## 雲端計算的發展趨勢
在課程一開始先帶我們來回顧雲端計算的發展趨勢。

<center> <img src="https://i.imgur.com/kB72EtC.png" alt="雲端計算的發展趨勢"></center>
<center class="imgtext">雲端計算的發展趨勢（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>

<br>

由圖中可以發現架構演變的趨勢，是朝著<span class="highlighting">簡單</span>、<span class="highlighting">便捷</span>與<span class="highlighting">低成本</span>實現<span class="highlighting">商業邏輯</span>（Business Logic）的方向前進。

從最開始裸機的配置與開發；接著進入 IaaS 飛速發展，在這期間以 OpenStack 為代表的社區逐漸的成長，發展出許多的雲端平台公司；到之後發展出以 docker 為代表的 containers，而使得 Kubernetes 和 Mesos 等容器調度工具廣為人知。


<div class="alert info">
<div class="head">docker、Swarm、Kubernetes 和 Mesos</div>

文章中所說的 docker 指的是 docker 技術本身。而一般看到的 Docker、 Kubernetes 與 Mesos 的比較文章，更有可能是比較與 Docker Swarm 比較，三者皆為容器調度工具。
<br> 
<br> 三者較可閱讀：<br> 
1. <a href="https://zhuanlan.zhihu.com/p/28301108">Docker、Kubernetes 和 Apache Mesos 对比中的一些误区</a> <br> 
2. <a href="http://jolestar.com/container-ecosystem/">2016年容器技术思考：Docker, Kubernetes, Mesos 将走向何方？</a> <br> 
</div>

由發展史看可以看出，隨著一系列的發展，對底層的屏蔽是日趨增加。

在裸機的時代，開發人員還需要操心系統的設定與維護；而到了 container 階段，則是已經將執行環境抽離，消除底層系統和基礎架構差異性所帶來的問題。

換句話說，隨著對底層屏蔽的增加，使的開發人員能<span class="highlighting">專注於功能或程式本身的開發設計</span>，加速整個開發流程的推進。

<br> 

而在最近幾年的技術中，又將粒度再次縮小，提出了所謂的 Function as a Service（FaaS），也就是 Serverless，試圖讓開發人員程從系統、運行環境、軟體相依性等複雜的環境中解脫。


<br> 

### 什是 Serverless?
Serverless 是雲端計算 IaaS 、 PaaS 的下一個方向，但這像技術並非實現字面意上的『無伺服器』，而是由第三方雲端計算平台供應商負責後端基礎設施的維護，以服務（aaS）的方式為開發者提供所需功能，例如：認證、授權與資料庫服務等。

簡單地說，這裡所強調的『無伺服器』，指的是我們的<span class="highlighting">程式碼不會明確地被部署在某些特定的軟體或者硬體的伺服器上</span>，不需要在伺服器上持續執行行程以等待 HTTP 請求或 API 調用；是通過某種事件機制來觸發程式碼的執行，此時供應商才會將此服務進行部屬。這樣的架構可以讓開發人員更專注程式碼的運行，而不需要分心管理任何的基礎設施。

<br>

目前已有有些這樣的服務：<span class="highlighting">由第三方供應，且無須安裝、管理和維護，只在需要的時候使用，並按照使用資源計費<span>。

例如實務上，有些開發人員為了更專注於商業邏輯相關程式碼的開發，會將與商業邏輯邏輯無關的部份委託給第三方，像是上述所提到的認證、授權與資料庫服務等，這些都是 **Backend-as-a-Service（BaaS）**。

也有的將整個程式運行環境給託管出去的，將開發出來的函式直接放動雲端去當作服務在調用，也就是 **Function-as-a-Service（FaaS）**。

這兩類型的服務都被稱作 Serverless，也都是雲端計算的發展方向。

<br>


這邊整理兩種服務的特性：

- **Function-as-a-Service（FaaS）**  
    - 片段程式碼，按需執行、按需擴展、無需管理任何基礎設施相關部份。	
    - 事件驅動行計算。函式被事件觸發或是被 HTTP 請求調用。	

- **Backend-as-a-Service（BaaS）**  
    - 第三方基於 API 的服務，實現應用開發中的基礎功能模塊。
    - 這些 API 像服務一樣，自動擴展，無需管理。


兩者都強調：<span class="highlighting">按需執行、按需擴展、無需管理</span>。這樣的模式是將軟體開發中的複雜性全部抽出，給開發人員更多的自由，並降低開發門檻；但從另一方面來說，這是將複雜性全留給雲端計算平台供應商。


<br> 

### Serverless 要點
Serverless 僅是一個概念，目前還沒有一個普遍公認的權威的定義，但此概念所要實現的目標表達就是：
<div class="blockquote-center"> 無需管理、按需執行、按需擴展、按使用來計費 </div>

P.S. 對於按使用來計費，我有兩種理解，按調用次數或使用資源來收費，不過兩種的核心的使用了才付費，就看平台供應商的計價方式了。

<br>

至於所使用究竟是 FaaS 或 BaaS 則是次要的。但若採用的 FaaS ，對於函式還是有些要求：
1. 為事件驅動的調用方式。
2. 函式是無狀態、短暫的、且是有限制的，例如：所需記憶體的限制、執行時間的限制。  
3. 另外，因為 FaaS 可被 HTTP 調用，因此 API Gateway 設定也是其中的要點。

<br>

另外，講師提到一件事 Serverless 雖然包含了兩種服務概念 FaaS 與 BaaS，不過在講解中多會集中在 FaaS，這點在多數的文章中也是。因為 BaaS 這邊複雜度在提供此服務的開發者那邊，對於我們來說會用就好。因此在提到 Serverless 時大家多著墨在 FaaS 這塊。

<br> 
<center> <img src="https://i.imgur.com/D1N30gv.jpg?1" alt="感應式水龍頭"></center>
<center class="imgtext">感應式水龍頭（圖片來源: <a href="https://www.amazon.com/-/es/inoxidable-lavabo-el%C3%A9ctrico-autom%C3%A1tico-dom%C3%A9stico/dp/B07SZMPPBH" class="imgtext">Amazon</a>）</center>
<br>


課程中講師提到關於水龍頭與計算資源的比喻：
<div class="blockquote-center"> 雲端計算的的目標是：期望計算資源如同水資源一樣的便捷，打開就可以享用。 </div>

按她的比喻來理解，現行的 VM 或是 containers 像是一般水龍頭，打開後除非主動關閉，不然就會 24 小時運行，即便此時無人使用，但還是會佔用資源。

而 Serverless 則像是使用了感應式水龍頭，手伸出去了，資源才會被調用執行，結束後資源就自動停用。
 
<br>

### FaaS 與 PaaS 的比較  
在第一次看到 FaaS 的觀念時，覺得它像是更專注提供某種功能的 PaaS，所以查了一下兩者的比較。

在[維基百科上](https://zh.wikipedia.org/wiki/%E7%84%A1%E4%BC%BA%E6%9C%8D%E5%99%A8%E8%A8%88%E7%AE%97)提到：
> 以平台即服務（PaaS）為基礎，無伺服器運算提供一個微型的架構 ...

所以可以推測 FaaS 是 PaaS 的上層，但還不到 SaaS。

<br>

而在 [AWS 的官方博客](https://aws.amazon.com/cn/blogs/china/iaas-faas-serverless/) 上，則是有說到些兩者的差異：  

1. **啟動和停止**  
    大部分 PaaS 應用無法針對每個請求啟動和停止整個應用，而 FaaS 生來就是為了實現這樣的目的。
    
2. **縮放能力**  
    兩者在維運方面最大的差異在於<span class="highlighting">縮放能力</span>。對於大部分 PaaS 平台，租戶依然需要考慮縮放，就算將它設置為自動縮放，依然無法在具體請求的層面上進行縮放。但是對於 FaaS 應用，這種問題完全是透明的。
    
<br>

文章比較末端，使用 AWS 雲端架構戰略副總裁 Adrian Cockcroft 給的簡單定義來收尾：

> 如果你的 PaaS 能夠有效地在 20 毫秒內啟動實例並運行半秒，那麼就可以稱之為 Serverless
    
 
<br>

最後附上一篇兩者的比較： [FaaS、PaaS 和无服务器体系结构的优势](https://www.infoq.cn/article/2016/06/faas-serverless-architecture)

<br>

### Serverless 架構的優點  
1. **快速開發/快速響應**  
    對於使用者來說最重要的是快速響應，因為底層全部被屏蔽了，只需專注商業邏輯的開發，無需煩心操作系統、運行環境、版本相容...等問題。 借助 Serverless，或許只需要幾個小時，來完成新的實做邏輯即即可，不用再去重新配置環境。
    
2. **無限擴展**  
   在 Serverless 架構的中，<span class="highlighting">橫向擴展是完全自動的、有彈性的、且由平台供應商所管理</span>，使用者僅需支付您所需要的計算能力。

3. **『綠色』計算**  
    想想剛剛水龍頭的例子，就可以理解了。

4. **對開發者友好**  
    恩...不用管硬體配置，只要寫程式就好。這對我這個討厭配置環境的人來說，真的是非常友好 XDDD 每次配環境都會在坑裡掙扎超久的 Orz

5. **費用用低**  
    在降低成本上包含了兩個方面，即基礎設施的成本和人員（運營/開發）的成本。

  
<br><br> 
 

## Opensource Serverless 平台 - Apache OpenWhisk

<center> <img src="https://i.imgur.com/uAXvRH6.png" alt="Apache OpenWhisk Icon"></center>
<center class="imgtext">Apache OpenWhisk（圖片來源: <a href="https://landscape.cncf.io/license=open-source" class="imgtext">CNCF Cloud</a>）</center>
<br>


OpenWhisk 是一個開源 FaaS 平台，現在已經脫離孵化器，是 Apache 的一個頂級項目，也在 IBM 公有雲上有做了驗證。它是一個典型的<span class="highlighting">事件驅動型</span>的程式編輯模型。

 
<br> 

事件驅動顧名思義就是，事件來了才去做一件事情。以講師給的流程圖為例：  
當事件源觸動觸發器後進入系統，然後在系統中通過規則的匹配去決定要觸發的動作，最終產生所需的結果。通常來說，輸入與輸出的的資料格式都是 Json。

<center> <img src="https://i.imgur.com/a5Wgs5M.png?1" alt="apache OpenWhisk理念"></center>
<center class="imgtext">Apache OpenWhisk理念（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>

在這邊出現了幾個詞，<span class="highlighting">觸發器 (Trigger)</span>、<span class="highlighting">動作 (Action)</span> 與 <span class="highlighting">規則 (Rule)</span>，這些詞我跟下一節的筆記整理在一起。
 
<br> 
 

### OpenWhisk 程式編輯模型

<center> <img src="https://i.imgur.com/lZP3G5P.png" alt="Apache OpenWhisk 程式編輯模型"></center>
<center class="imgtext">Apache OpenWhisk 程式編輯模型（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>



**觸發器 (Trigger)**  

觸發器可以對外界數據源，如資料庫的增刪查改、 github 上的 push、issue...等，進行對應的反應。

不過在 OpenWhisk 中觸發器僅某類別事件的具名頻道，換句話說僅是<span class="highlighting">一個名字</span>。觸發器要對反應特定類型事件的宣告，再藉由使用者或是透過事件來源觸發觸發器。

以資料庫更新為例，若是事件來源觸發，在 OpenWhisk 之外會有一個 Event provider，由它監聽資料庫。當 provider 監聽到更新事件時，再由它通知 Trigger 資料更新了。

<center> <img src="https://i.imgur.com/XznGAql.png" alt="Trigger"></center>
<center class="imgtext">Trigger（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>


**動作 (Action)**  

動作是執行某個特定動作的一段程式碼，它將會在 openwhisk 中執行。使用所選擇的語言來撰寫 Action，或者直接將 Docker 以映像檔提供執行。

Action 中什麼都可以放，諸如：計算、資料格式轉換、資料抽取、第三方 API 調用...等，程式寫的出來的都行。但必須注意的是，在 FaaS 中是<span class="highlighting">無狀態</span>的，不應該存在狀態的記錄，而且<span class="highlighting">短時運行</span>的。在 openwhisk 中預設最長執行時間為是 5 分鐘，若超過 5 分鐘則不建議使用 Faas 執行，或是必須在要切成更小的粒度分開執行。

<center> <img src="https://i.imgur.com/URsL9hc.png" alt="Action"></center>
<center class="imgtext">Action（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>

**動作序列 / 動作串 (Sequence)**  
全稱是 Action Sequence，這並不在講師的講義中，僅出現在講解過程中出現。

動作序列顧名思義，就是把一系列的動作鏈結來。它是一串按順序呼叫的動作，其中某個動作的輸出會傳遞為下一個動作的輸入，達到動作重複使用的目的。 

<br>


**規則 (Rule)**  
規則會建立觸發器與動作（序列）的關聯。每次觸發觸發器時，規則都會使用觸發事件作為輸入，並呼叫關聯的動作（序列）。使用適當的規則集時，單一觸發器事件可能會呼叫多個動作，也可能會呼叫動作以作為多個觸發程式的事件的回應。

 
<center> <img src="https://i.imgur.com/hJtcgqJ.png" alt="Rule"></center>
<center class="imgtext">Rule（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>

**套件 (Package)**

就是一個集合，可能包含 action、trigger、rule...等，可以利用套件來新增與服務及事件提供者的整合。
 
這邊講師沒多說，不過 [IBM 的名詞解釋](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-about#cloud-functions-) 舉了個例子：
> 使用 IBM Cloudant 變更資訊來源所建立的觸發程式，會將服務配置成在每次修改文件或將其新增至 IBM Cloudant 資料庫時發動觸發程式。套件中的動作代表服務提供者可設為可用的可重複使用邏輯，如此，開發人員可以使用服務作為事件來源，以及呼叫該服務的 API。

<br>

<center> <img src="https://i.imgur.com/rherSKn.png" alt="Package "></center>
<center class="imgtext">Package （圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>

<br>

最後來貼張漂亮的流程圖：

<center> <img src="https://i.imgur.com/bdt17VM.png" alt="OpenWhisk 流程示例"></center>
<center class="imgtext">OpenWhisk 流程示例（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>


<br>

### Apache OpenWhisk 的部署方式

Apache OpenWhisk 的部屬看理來很簡單，只要支援 docker 的設備都可以安裝，Apache 已將相關內容都把包成 docker image。相關配置步驟可以看這兩份官方的說明文件：[文件1](https://openwhisk.apache.org/documentation.html#openwhisk_deployment)、[文件2](https://github.com/apache/openwhisk#quick-start)

...不過有沒有真的這麼簡單，可能要頭洗下去才知道。


<center> <img src="https://i.imgur.com/JtiWH57.png" alt="Apache OpenWhisk的部署方式"></center>
<center class="imgtext">Apache OpenWhisk的部署方式（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>

<br><br>

## IBM Functions

先前提過 OpenWhisk 有在 [IBM 公有雲](https://cloud.ibm.com/functions/)上有做了驗證，所以這邊講師就示範了在公有雲上的簡單操作。

<br>

### 建立 Action

1. 從入口處點選**開始建立**  
    <center> <img src="https://i.imgur.com/Pl7SdIK.png" alt="開始建立"></center>
    <br>

2. 選擇**建立動作**  
    <center> <img src="https://i.imgur.com/CIqK3tZ.jpg" alt="建立 Action"></center>
    <br>

3. 建立**套件**  
    這步驟非必須，這邊只是試著操作。
    <center> <img src="https://i.imgur.com/LACIoTX.jpg" alt="建立 Package"></center>
    <br>
    

4. 為動作命名，並引入剛剛所創建的套件  
    這邊為了測試方便，運行環境直接選擇 **Node.js**
    <center> <img src="https://i.imgur.com/b3gprKB.jpg" alt="建立 Package"></center>
    <br>
    
5. 建立後會出現 Node.js 的程式碼作為動作的執行細節  
    再依照實際要執行的動作進行修改，完成後可以試著點選**呼叫**，查看動作執行後的結果。
    <center> <img src="https://i.imgur.com/5EheU6x.jpg" alt="定義動作細節"></center>
    <br>
    
    也可以透過 curl 來呼叫
    ```shell
    $ curl -u API-KEY -X POST https://us-south.functions.cloud.ibm.com/api/v1/namespaces/id%40dgmail.com/actions/test/hello?blocking=true
    ```

    
    
<br>

### 建立 Trigger

1. 從入口處點選**開始建立**  
    <center> <img src="https://i.imgur.com/Pl7SdIK.png" alt="開始建立"></center>
    <br>
    

2. 選擇**建立觸發程式**  
    <center> <img src="https://i.imgur.com/8X3Iavi.png" alt="建立 Trigger"></center>
    <br>    

3. 設定定時式的觸發器  
    這邊先簡單設定一個定時觸發的觸發器  
    <center> <img src="https://i.imgur.com/ynmDXKW.jpg" alt="建立定時觸發器"></center>
    <br>    
    
4. 設定觸發時間  
    這邊使用 cron 的方式設定每 20 秒觸發  
    <center> <img src="https://i.imgur.com/HlLkZXe.jpg" alt="建立每 20 秒觸發"></center>
    <br>    
    
    
5. 查看  
    完成與 action 的連接後，可到監視查看。
        <center> <img src="https://i.imgur.com/2gfcm2n.jpg" alt="監視"></center>
    <br>
    
    應該會看到 trigger 每 20 秒被觸發，接著執行 action 。  
    P.S. Oops，我現在才發現 every20 打錯了，不過也來不及了...


<br><br> 

## 其他連結
1. [Serverless 應用案例賞析筆記目錄](/Serverless-Use-Cases-Study-Notes-Contents)
2. 課程內容：影片（[IBM片源](https://mediacenter.ibm.com/media/1_7hu9nbk8) 、[youku教育片源](https://v.youku.com/v_show/id_XMzg0MTI3NTE3Ng==.html)）/ [講義](https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf) / [Blog](https://mp.weixin.qq.com/s/p0bImKuYSz2FPfdMvTt06Q)

<br><br> 

## 參考資料 
1. 费良宏 (2017-01-18)。[从IaaS到FaaS—— Serverless架构的前世今生](https://aws.amazon.com/cn/blogs/china/iaas-faas-serverless/) 。檢自 亚马逊AWS官方博客 (2020-03-25)。
2. 協同撰寫。[無伺服器計算](https://zh.wikipedia.org/wiki/%E7%84%A1%E4%BC%BA%E6%9C%8D%E5%99%A8%E8%A8%88%E7%AE%97)。檢自 維基百科 (2020-03-25)。
3. (2018-11)。 [IBM Cloud Functions](https://www-03.ibm.com/software/sla/sladb.nsf/8bd55c6b9fa8039c86256c6800578854/c9c99a9fc9731d3f86258340006f9704/$FILE/i126-7607-03_11-2018_zh_TW.pdf)。檢自 IBM Cloud 其他服務說明 (2020-03-25)。
4. Shashikanh (2019-07-25)。[“Hello, World” in IBM Cloud Functions vs Express Serverless Platform — A Developer’s Perspective](https://medium.com/ibm-cloud/hello-world-in-ibm-cloud-functions-vs-express-serverless-platform-a-developers-perspective-4d5e391653e) 。檢自 IBM Cloud - Medium (2020-03-25)。
5. (2019-07-12)。[Cloud Functions 的運作方式 - Cloud Functions 術語](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-about#cloud-functions-terminology) 。檢自 IBM Cloud (2020-03-25)。
6. Linux中国 (2017-08-04)。[Docker、Kubernetes 和 Apache Mesos 对比中的一些误区](https://zhuanlan.zhihu.com/p/28301108)。檢自 Linux 开源评论 - 知乎 (2020-03-25)。
7. jolestar (2017-02-17)。[2016年容器技术思考：Docker, Kubernetes, Mesos 将走向何方？](http://jolestar.com/container-ecosystem/)。檢自 午夜咖啡 (2020-03-25)。
8. Abel Avram，大愚若智 譯 (2016-06-26)。[FaaS、PaaS 和无服务器体系结构的优势](https://www.infoq.cn/article/2016/06/faas-serverless-architecture)。檢自 infoq (2020-03-27)。
9. [Get hands-on experience with Serverless Computing using IBM Cloud Functions](https://developer.ibm.com/series/get-hands-on-with-serverless-series-page/)。檢自 IBM Developer (2020-03-27)。



