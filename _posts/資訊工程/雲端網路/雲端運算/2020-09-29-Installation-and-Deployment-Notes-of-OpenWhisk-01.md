---
title: 【Survey】OpenWhisk 折騰安裝部署筆記 01
date: 2020-12-31 22:48
is_modified: false
categories:
- "雲端網路 › 雲端運算"
- "資訊科技 › 開發與輔助工具"
tags:
- Fass/Serverless
- OpenWhisk
- Linux/Unix
- 網站與雲端平台
- 工具安裝與部署
--- 


下這個標題的意思不是我折騰 OpenWhisk，而是...OpenWhisk 在折騰我 Orz 
  
之前在[了解 Serverless 的概念與 FaaS 服務](/Serverless-Use-Cases-Study-Notes-Contents)時，就有過想要自己架設 OpenWhisk 來玩玩的打算。剛好最近進來個工作，要我評估 Openwhisk 是否符合需求建議書（RFP）的需求。

<!--more-->
<br>

## 前置閱讀
與 Openwhisk 最相關的條目就是 <mark>Fass/Serverless</mark>，因此在開始前可以先看看 [IBM 微講堂系列 - 《Serverless 應用案例賞析》](/Serverless-Use-Cases-Study-Notes-Contents)，複習下 Fass/Serverless 的概念場景。可以的話，順便玩玩 [IBM Cloud Functions](https://cloud.ibm.com/functions/)，這套是以 Apache OpenWhisk 為基礎所架設 FaaS 平台，可以透過操作 IBM Cloud Functions 對 OpenWhisk 的功能有個概念。

<br>

<div class="alert info"> 
<div class="head">Fass/Serverless 補充說明</div>
1. <a href="https://help.aliyun.com/document_detail/65565.html">Serverless应用场景_Serverless_通用解决方案-阿里云</a><br>
2. <a href="https://www.cnblogs.com/Leo_wl/p/6055252.html">Serverless 架构：用服务代替服务器 - HackerVirus - 博客园</a><br>
</div>


<br><br> 

## 簡介
[Apache OpenWhisk](https://openwhisk.apache.org/) 是由 IBM 和 Adobe 驅動的開源項目，由 IBM 在 2016 年 2 月將此項目貢獻給 Apache 基金會（[GitHub](https://github.com/apache/openwhisk)），並在 2019 年 7 月從孵化器畢業，晉升為 Apache 的頂級項目。

<center> <img src="https://i.imgur.com/dhrBgsq.jpg" alt="OpenWhisk"></center>
<center class="imgtext">OpenWhisk 晉升 Apache 的頂級項目（圖片來源: <a href="https://www.infoq.cn/article/MMwF9fbx-XaAGwwhtT8L" class="imgtext">赵钰莹 | infoq</a>）</center>
<br> 

有人說 AWS Lambda 是 Serverless 的代名詞，我想這不可否認，畢竟 AWS 是最早推出 Serverless，各廠商推出 Serverless 時的競品絕對有它。但它畢竟不是開源的，因此我們無法直接使用並架設在我們自己的環境。

若說到開源的 Serverless 平台，則是 <mark>OpenWhisk</mark> 與 <mark>Kubeless</mark> 天下。OpenWhisk 較為成熟且有許多活躍開發者的支持，但相較之下 OpenWhisk 也比 Kubeless 複雜許多、也重複實現了些 Kubernetes 中已經存在的特性（比如自動擴展機制）。
 
 
<div class="alert info"> 
<div class="head">開源的 Serverless 平台比較</div>
1. <a href="https://juejin.im/post/5d1abf09f265da1bc8544227">【译】Kubernetes Serverless 框架的全面对比（OpennFaas，OpenWhisk，Fission，Kubeless 等）</a>
</div>
<br>

與 AWS Lambda  相同，OpenWhisk 是一個<mark>事件驅動</mark>的無狀態的計算模型。<mark>理論上</mark>它可以支持任何程式語言，使用者僅需上傳程式碼到 OpenWhisk 中，並設定相對應的觸發事件與處理數據流即可，OpenWhisk 負責處理計算資源的擴展。

<br><br> 


## 程式編輯模型
先來看看 OpenWhisk 程式編輯模型，前面提過它是<mark>事件驅動</mark>模型。所謂的事件驅動，顧名思義就是，事件來了才去做一件事情。當事件源觸動觸發器後進入系統，然後在系統中通過規則的匹配去決定要觸發的動作，最終產生所需的結果。


<center> <img src="https://i.imgur.com/a5Wgs5M.png?1" alt="apache OpenWhisk理念"></center>
<center class="imgtext">Apache OpenWhisk理念（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>


在這樣的程式編輯模型中，會有幾個重要的角色：
1. **觸發器（Trigger）**
2. **動作（Action）**
3. **規則（Rule）**
5. **資訊來源/訂閱源 (Feed)** 

<center> <img src="https://i.imgur.com/lZP3G5P.png" alt="Apache OpenWhisk 程式編輯模型"></center>
<center class="imgtext">Apache OpenWhisk 程式編輯模型（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>


<br>

下面就各角色進行說明：


### 觸發器 （Trigger）
而在事件驅動模型中，最先對事件做出反應的是 Trigger，它會對如資料庫的增刪查改、 GitHub 推送新的 commit...等外部事件源進行對應的反應。

不過在 OpenWhisk 中 Trigger 僅是某類別事件的具名頻道，也就是說僅僅是<mark>一個名字</mark>。可以說它是對所要反應特定類型事件的變數宣告，實際上必須透過 Feed 來呼叫 Trigger。關於 Feed 我們等等在說，這邊僅需要先知道 OpenWhisk 中 Trigger 其實只是類似一個函數，它必須經由他人呼叫，才能向下執行。

以資料庫更新為例，在 OpenWhisk 之外會有一個 Event provider，由它監聽資料庫。當 provider 監聽到更新事件時，再由它通知 Trigger 資料更新了。

<center> <img src="https://i.imgur.com/XznGAql.png" alt="Trigger"></center>
<center class="imgtext">Trigger（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>


### 規則 （Rule）  
而當事件源觸動 Trigger 後進入系統流程後，通過 Rule 的匹配去決定要觸發的 Action，最為對 Trigger 的響應。

<center> <img src="https://i.imgur.com/hJtcgqJ.png" alt="Rule"></center>
<center class="imgtext">Rule（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>


一般來說 Trigger 與 Rule 的對應關係是一對多，單一 Trigger 事件可以綁定一或多個 Rule 進而呼叫多個 Action，單一個 Action 也可以回應多個 Rule，但每個 Rule 只能綁定一個 Action：

<center> <img src="https://i.imgur.com/HPLO4vM.png" alt="Rule"></center>
<br>
 
  
### 動作 （Action）
當 Rule 觸發相對應的 Action 後，最終會執行 Action 產生所需的結果。

在 OpenWhisk 中，Action 所指的是執行某個行為的一段程式碼，這些行為可以是計算、資料格式轉換、資料抽取、第三方 API 調用...等，一般來說它輸入與輸出的的資料格式都是 Json。
 
<center> <img src="https://i.imgur.com/URsL9hc.png" alt="Action"></center>
<center class="imgtext">Action（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-00.pdf" class="imgtext">課程講義</a>）</center>
<br>

前面提到過<mark>理論上</mark> OpenWhisk 可以支持任何程式語言，但會什麼會說是理論上呢？因為 OpenWhisk 其實是有明確列出所能直接支援的語言，包含：[.Net](hhttps://github.com/apache/openwhisk-runtime-dotnet)、 [Go](https://github.com/apache/openwhisk-runtime-go)、 [Java](https://github.com/apache/openwhisk-runtime-java)、 [JavaScript](https://github.com/apache/openwhisk-runtime-nodejs)、 [PHP](https://github.com/apache/openwhisk-runtime-php)、 [Python](https://github.com/apache/openwhisk-runtime-python)、 [Ruby](https://github.com/apache/openwhisk-runtime-ruby)、 [Swift](https://github.com/apache/openwhisk-runtime-swift) 與尚在實驗階段的 [Ballerina](https://github.com/apache/openwhisk-runtime-ballerina) 和 [Rust](https://github.com/apache/openwhisk-runtime-rust)。而未列出的語言，如需求建議書中要求的 C++ ，並無直接支援。

但若細看這些語言的支援方式，會發現它其實是採用不同的 [Docker 映像檔](https://hub.docker.com/u/openwhisk)來進行。因此若所慣用的語言不在明列的支援清單中，其實也可以直接提供[自定義 Docker 映像檔](https://github.com/apache/openwhisk-runtime-docker)來達成對該語言使用。
 
雖說程式碼裡面只要你寫的出來什麼都能塞。但有兩點必須注意的是：
1. **無狀態**  
   FaaS 中是無狀態的，不應該存在狀態的紀錄。
   
2. **短時運行**  
    在 OpenWhisk 中是有預設最長執行時間的，因此若超過此限制則不建議使用 Faas 執行，或是需要在切成更小的粒度分開執行。
  
若真需要將 Action 切成更小的粒度，則可以使用**動作序列 / 動作串 （Action Sequence，簡稱 Sequence）**，把一系列的動作鏈結來。顧名思義，它就是一連串的動作，在此序列中的動作會被按順序呼叫，並將一個動作的輸出會傳遞為下一個動作的輸入。如此一來不僅能達到切分的目的，也可讓 Action 被重複使用。 

<br>

### 事件源（Event Sources）/ 事件（Event）/ 訂閱源（Feed）
剛剛一直說到事件驅動，在這邊事件其實存在著三個概念：

1. **事件源（Event Sources）**  
    就是產生事件的<mark>來源</mark>，如：資料庫、 IoT 設備...等。

2. **事件（Event）**  
    事件源所<mark>產生的事件</mark>。例如資料庫的增刪查改、IoT 感應器超出特定溫度、GitHub 推送新的 commit，或是簡單 HTTP 要求...等。

3. **訂閱源（Feed）**  
    前面提到 Trigger 時提到過，在 OpenWhisk 之外會有一個 <mark>Event provider</mark>，由它監聽事件，並通知 Trigger。
    
    而在 OpenWhisk 整個系統中扮演 Event provider 角色的是 Feed。前面提過 Trigger 其實是類似一個函數，外部事件的變動與 OpenWhisk 無關，如果 OpenWhisk 想知道這些變動，必須依賴 Feed 來捕獲這些事件並呼叫，才能得知。
    
    Feed 主要負責從外部事件源來接收事件，不管所接收事件原本的資料結構、格式、傳輸協定為何，它會轉換成 OpenWhisk 所需的協議與格式。更詳細來說，Feed 它是管理外部事件觸發 Trigger 這件事，包括了創建 Trigger、刪除 Trigger、暫停觸發與取消暫停...等生命週期的動作事件。
        
    <center> <img src="https://i.imgur.com/1g4VI6S.png" alt="Feed "></center>
    <center class="imgtext">Feed （圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-02.pdf" class="imgtext">課程講義</a>）</center>
    <br>

    而 Feed 的可以透過下列三種體系結構模式來創建，詳細的說明，可以看看[這篇](/Serverless-Use-Cases-Study-Notes-03#簡介)：
    1. **Hook**
    2. **輪詢（Polling）**
    3. **連接（Connections）**   
 
<br><br>

最後來貼張漂亮的流程圖：

<center> <img src="https://i.imgur.com/LWt3hjl.png" alt="OpenWhisk 流程示例"></center>
<center class="imgtext">OpenWhisk 流程示例（圖片來源: <a href="https://www.cnblogs.com/junjiang3/p/9613698.html" class="imgtext">junjiang3 | 博客园</a>）</center>
<br>

簡單複述一下架構，由 Event Sources 產生 Event，而這些 Event 會被 Feed 所捕獲，而當後 Feed 捕獲 Event 後會觸發 Trigger。一個 Trigger 會綁定一到多個 Rules，而符合條件 Rule 會觸發相對應的 Action 或 Action Sequence 都會被執行，而這些 Action 在執行時，依照需求可能會呼叫第三方程式。Action 除了被 Trigger 觸發外，也可以透過 REST 直接呼叫，當成 API 使用。

<br>

貼下另外一張我也很喜歡的流程圖：

<center> <img src="https://i.imgur.com/JwrVcR4.png" alt="OpenWhisk 流程示例"></center>
<center class="imgtext">OpenWhisk 流程示例（圖片來源: <a href="https://www.oreilly.com/library/view/learning-apache-openwhisk/9781492046158/ch01.html" class="imgtext">Oreilly</a>）</center>


<br><br> 

## 系統架構
前面提到是屬於資料流動的部份，接下來看看系統架構的部份：

<center> <img src="https://i.imgur.com/P4oYa39.png?1" alt="OpenWhisk 系統架構"></center>
<center class="imgtext">OpenWhisk 系統架構</center>
<br> 

會說 OpenWhisk 比 Kubeless 複雜的原因在於，它利用了 CouchDB、 Kafka、 Nginx、 Redis 和 Zookeeper 等許多底層元件，雖說這可以讓使用者清晰地關注於可伸縮和彈性的服務（！？），但這也迫使使用者與開發者必須具備些工具的相關知識，拉高了學習曲線。

附上找到的另外一張流程圖，這張有較為清楚的步驟標號，但有些元件被省略或是被特定指出：

<center> <img src="https://i.imgur.com/qhjWCWy.png?1" alt="OpenWhisk 系統架構流程圖"></center>
<center class="imgtext">OpenWhisk 系統架構（圖片來源: <a href="https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/overview/howitworks.md" class="imgtext">Adobe I/O</a>）</center>
<br>

回頭來看看 OpenWhisk 組成元件扮演著怎樣的角色：

### Step1： Nginx & Redis
Nginx（音：engine X [:speaker:](https://translate.google.com/?hl=zh-TW#view=home&op=translate&sl=en&tl=zh-TW&text=Nginx)）是套是免費的開源軟體，通常用於作為非同步框架的網頁伺服器，也可以用作反向代理、負載平衡器和HTTP快取。

在 OpenWhisk 中是 Nginx 是作為 API Gateway，整體遵守 Restful 設計的架構。當每個進入 OpenWhisk 請求，都會經由它接收後，並將處理後的請求後轉發給 Controller。另外 Nginx 會使用 Redis 來儲存黑名單列表。

<br>

### Step2： Controller
Controller 可以視為真正處理請求的地方。當通過 Nginx 後，會轉而觸發 Controller，由它來為每個請求進行身份驗證與授權，並將操作指派給 Invoker。
 
另外有看到資料寫到 Controller 是使用 Scala 語言編寫的 rest api，並基於 Akka 運行時環境和 Spray REST / HTTP 工具箱構建，用於對 OpenWhisk 內部對象的增刪查改（CRUD）和 Action 的調用。
 
<br>

### Step3、4、8： CouchDB
CouchDB 用於儲存一些系統狀態，憑證、namespace、Action、Trigger、Rule 等定義皆存儲在於此。而這邊除了使用 CouchDB 外，根據[文件](https://github.com/apache/openwhisk-deploy-kube/blob/master/docs/configurationChoices.md#using-an-external-database)也可以使用 [Cloudant](https://www.ibm.com/tw-zh/cloud/cloudant)。只是若是使用 Cloudant 就必須跟 IBM 綁在一起可能會不太適合。


在整個 OpenWhisk 架構中，CouchDB 被用在了三個地方：
1. **身份驗證與授權**   
    當 Nginx 請求轉發給 OpenWhisk 後，Controller 會從 CouchDB 讀取使用者的身份資料，當確認驗證無誤後，Controller 會再進行下一步處理。

2. **取回 Action**  
    身份驗證通過後，Controller 會從 CouchDB 中加載 Action、 Code...等。
    
3. **儲存結果**  
    Action 的執行結果也會存回 CouchDB，就是圖中線 8 的部份。
  

<center> <img src="https://i.imgur.com/FXuX36T.png?1" alt="Lean OpenWhisk Architecture"></center>
<center class="imgtext">Lean OpenWhisk Architecture（圖片來源: <a href="https://medium.com/openwhisk/lean-openwhisk-open-source-faas-for-edge-computing-fb823c6bbb9b" class="imgtext">Medium</a>）</center>
<br>

### Step5： Consul
當 Controller 並將資料指派給 Invoker 前，它會先與 Consul 確認。Consul 擁有可用的 Invoker 及其健康狀況清單。Controller 會決定重新使用現有的「熱」容器（Container），或啟動一個暫停的「暖」容器，或啟動一個新的「冷」容器進行新的調用。


<br>

### Step6： Kafka
Apache Kafka 通常用於構建即時資料管道和 Streaming 應用程式。


OpenWhisk 利用 Kafka 連接 Controller 和 Invoker，所有 Controller 發布的 Action，都會轉換成消息在 Kafka 中排隊等待執行。當 Controller 得到 Kafka 的收到請求的確認消息後，會直接向發出請求的使用者回傳 ActivationId，當使用者收到 ActivationId 後可視為請求已經成功存入 Kafka，在之後可以藉由 ActivationId 向 OpenWhisk 取回運行結果。
 
<br>

### Step7： Invoker
Invoker 算是 OpenWhisk 的最後階段，Invoker 也是用 Scala 實做的，它會執行 Action 的程式碼。


當 Invoker 從 Kafka 中接受了 Controller 傳來的請求後，會生成 Docker Container， 接著從 CouchDB 複製 Action 原始碼並注入容器中。一旦執行完成後，執行結果會被存回 CouchDB 中。


<br>

### Step8：CouchDB
上一階段中的 Invoker 會回傳執行結果到 CouchDB 中儲存。可以藉由在第六階段中取回的 ActivationId 來向 CouchDB 取得結果，看起來會類似這樣：

```bash
{
   "activationId": "31809ddca6f64cfc9de2937ebd44fbb9",
   "response": {
       "statusCode": 0,
       "result": {
           "hello": "world"
       }
   },
   "end": 1474459415621,
   "logs": [
       "2016-09-21T12:03:35.619234386Z stdout: Hello World"
   ],
   "start": 1474459415595,
}
```
 
<br>

### 其他架構圖
在找資料的時候從，其實還有找到其他幾張 OpenWhisk 的架構圖，只是我在前面的敘述，找不到適合的地方插入這幾個張圖，所以這邊就把它列出來：

<center> <img src="https://i.imgur.com/njWWU1M.png" alt="OpenWhisk 系統架構流程圖"></center>
<center class="imgtext">OpenWhisk 系統架構（圖片來源: <a href="https://events19.lfasiallc.com/wp-content/uploads/2017/11/Apache-OpenWhisk-Kubernetes-A-Perfect-Match-for-Your-Serverless-Platform_Ying-Chun-Guo-_-Zhou-Xing.pdf" class="imgtext">Apache OpenWhisk + Kubernetes</a>）</center>

<br> 

<center> <img src="https://i.imgur.com/uzB1iBF.png?1" alt="OpenWhisk 系統架構流程圖"></center>
<center class="imgtext">OpenWhisk 系統架構（圖片來源: <a href="https://www.oreilly.com/library/view/learning-apache-openwhisk/9781492046158/ch01.html#serverless_and_openwhisk_architecture" class="imgtext">Oreilly</a>）</center>

<br><br> 

## 系統限制
OpenWhisk 有一些系統限制，包括一個 Action 可以使用多少記憶體、 Trigger 每分鐘的觸發頻率之類的。


<center> <img src="https://i.imgur.com/5YGKaoY.png?3" alt="OpenWhisk action execution constraints"></center>
<center class="imgtext">OpenWhisk action execution constraints（圖片來源: <a href="https://www.oreilly.com/library/view/learning-apache-openwhisk/9781492046158/ch01.html#serverless_and_openwhisk_architecture" class="imgtext">Oreilly</a>）</center>
<br> 

### Actions
<center> <img src="https://i.imgur.com/MgQ7u6y.png" alt="Default Limits for Actions"></center>
<center class="imgtext">Default Limits for Actions（圖片來源: <a href="https://github.com/apache/openwhisk/blob/master/docs/reference.md#actions" class="imgtext">apache/openwhisk｜GitHub</a>）</center>
<br>

其中 <mark>timeout/memory/logs</mark>，這三個參數使用者可以自行調整。 
 
<br> 

### Triggers
<center> <img src="https://i.imgur.com/MC6VDH5.png" alt="Default Limits for Triggers"></center>
<center class="imgtext">Default Limits for Triggers（圖片來源: <a href="Default Limits for Triggers" class="imgtext">apache/openwhisk ｜ GitHub</a>）</center>
<br><br> 

## 小結
系統的介紹先到這邊好了，下一章節我在試著安裝 OpenWhisk 並評估建議書的需求。


<br><br> 

## 參考資料 
1. [Apache OpenWhisk Documentation](https://openwhisk.apache.org/documentation.html) 。檢自 cheneydc｜简书 (2020-04-08)。
2. cheneydc (2017-08-06)。[折腾Openwhisk - 本地ubuntu部署openwhisk](https://www.jianshu.com/p/8e3a77c28532) 。檢自 cheneydc｜简书 (2020-04-08)。
3. junjiang3 (2018-09-09)。[Apache Openwhisk学习（一）](https://www.cnblogs.com/junjiang3/p/9613698.html) 。檢自 博客园 (2020-07-14)。
4. 零空科技 (2017-06-09)。[Apache OpenWhisk架構概述](https://kknews.cc/zh-tw/tech/j6egn8y.html) 。檢自 每日頭條 (2020-07-14)。
5. 赵钰莹 (2019-07-29)。[无服务器平台Apache OpenWhisk晋升为顶级项目](https://www.infoq.cn/article/MMwF9fbx-XaAGwwhtT8L) 。檢自 InfoQ (2020-07-14)。
6. 腾讯IVWEB团队 (2019-07-02)。[【译】Kubernetes Serverless 框架的全面对比（OpennFaas，OpenWhisk，Fission，Kubeless 等）](https://juejin.im/post/5d1abf09f265da1bc8544227) 。檢自 掘金 (2020-07-14)。
7. Glynn Bird (2016-07-12)。[OpenWhisk 简介：轻松创建微服务](https://developer.ibm.com/zh/articles/os-introducing-openwhisk-microservices-made-easy/) 。檢自 IBM Developer (2020-09-28)。
8. 協同撰寫。[Nginx](https://zh.wikipedia.org/wiki/Nginx) 。檢自 維基百科 (2020-09-28)。
9. 深圳清华大学研究院下一代互联网研发中心 (2019-08-19)。[Openwhisk 概览](https://zhuanlan.zhihu.com/p/78766357) 。檢自 知乎 (2020-09-28)。
10. [How Adobe I/O Runtime Works](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/overview/howitworks.md)。檢自 Adobe I/O (2020-09-28)。
11. Michele Sciabarrà。 [Chapter 1. Serverless and OpenWhisk Architecture](https://www..com/library/view/learning-apache-openwhisk/9781492046158/ch01.html#serverless_and_openwhisk_architecture)。檢自  Learning Apache OpenWhisk (2020-09-29)。
12. 協同撰寫(2019-02-14)。 [Apache/openwhisk reference.md](https://github.com/apache/openwhisk/blob/master/docs/reference.md#system-limits)。檢自 apache/openwhisk ｜ Github (2020-09-29)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-12-31</summary>
  <ul>
    <li>2020-12-31 發布</li>
    <li>2020-09-29 完稿</li>
    <li>2020-04-08 起稿</li>
  </ul>
</details>