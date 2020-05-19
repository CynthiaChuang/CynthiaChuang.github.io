---
title: 【Serverless 應用案例賞析筆記】 02. Serverless 在 API 經濟中的應用
date: 2020-04-02
categories:
- Study-Notes
- Cloud-Computing
tags:
- Other-Online-Courses
- Serverless 應用案例賞析
- Fass/Serverless
- OpenWhisk
--- 

<center> <img src="https://i.imgur.com/EXCq2WM.png" alt="投影片封面"></center>
<center class="imgtext">投影片封面（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-01.pdf" class="imgtext">課程講義</a>）</center>
<br>
  
本堂課程為此系列的第一講，主要來探討 Serverless 在 API 經濟中可以產生怎樣的作用。

<!--more-->
<br>

<div class="alert warning">
<div class="head">文中的 IBM 文件連結</div>
如果文中附上的 IBM 文件連結無法顯示或不存在，試著把 doc 語系換成英文後，再重新連結一次試試看。
<br>
發現 IBM 的多語系文件沒做好，如果沒有相對應的翻譯內容，不會直接導向英文頁面，而是直接顯示不存在。
</div>


<br><br>

## 回顧

開始前先來複習一下 **Serverless要點** 與 **OpenWhisk 程式編輯模型**。

<br>

### Serverless 要點

- [第一堂課程筆記 - Serverless](/Serverless-Use-Cases-Study-Notes-01/#serverless-要點)
  
上一堂課有介紹到 Serverless 存在兩種類型的服務： Function-as-a-Service（FaaS） 與 Backend-as-a-Service（BaaS）。

兩種類型的服務存在著相同特點：  
<div class="blockquote-center">
無需管理、按需執行、按需擴展、按使用來計費
</div>

依照講師的說明，只要其實只要滿足上述特點的的服務都可以被視為 Serverless 。

不過如同上一堂課所說的，Serverless 雖然包含了兩種服務概念，但因為實做複雜度的偏向，因此一般在討論 Serverless 時多集中在 FaaS。
 
<br>

Function-as-a-Service FaaS，中文翻譯為**函式即服務**，...恩...函式就是一般會在程式碼裡面寫的函式。不過需要注意的是這些函式是
<span class="highlighting">無狀態</span>、<span class="highlighting">短暫的</span>、且是<span class="highlighting">有限制的</span>。

除此之外，FaaS 的另外一個設定要點是 <span class="highlighting">API Gateway</span>，它可以將 Serverless 中的函式做為服務暴露出來，讓客戶端可以直接調用這個服務。

<br>

### OpenWhisk 程式編輯模型

在本堂課主要會利用 Serverless 無狀態、短暫的、有限制的 特點，來看看它在 API 經濟中的作用。
- [第一堂課程筆記 - OpenWhisk 程式編輯模型](/Serverless-Use-Cases-Study-Notes-01/#openwhisk-%E7%A8%8B%E5%BC%8F%E7%B7%A8%E8%BC%AF%E6%A8%A1%E5%9E%8B)
 
<center> <img src="https://i.imgur.com/lZP3G5P.png" alt="Apache OpenWhisk 程式編輯模型"></center>
<center class="imgtext">Apache OpenWhisk 程式編輯模型（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-0.pdf" class="imgtext">課程講義</a>）</center>
<br>


另外在上一講中還提到了OpenWhisk 程式編輯模型。


OpenWhisk　是一個典型的<span class="highlighting">事件驅動型</span>的程式編輯模型，其中有幾個主要的概念：觸發器（Trigger）、動作（Action）、規則（Rule）與套件（Package）。

我們可以定義規則，通過規則將觸發器與動作關聯起來，因此當某個事件被觸發時，就會執行相對應的動作。

而套件就是把一系列相關的觸發器、動作和規則關聯在一起。例如：Apache OpenWhisk 的 [alarms package](https://github.com/apache/openwhisk-package-alarms) 就是一個還滿好用的套件。

<br><br>

##  典型應用場景
由於它的技術特點，因此存在一些典型的應用場景：

1. **資料庫的增刪查改的事件響應**  
    對於資料的變更，做出相應的反應與更新。

2. **感測器的資料分析**  
    在 IoT 的應用中，可對收回的資料做分析，又或者是資料更新後的路徑重新規劃等。

    應該在下一章 [Serverless 在物聯網領域中的應用](/Serverless-Use-Cases-Study-Notes-03)會有更仔細的討論，先把連結建立起來。
    
3. **定時任務**  
    定期回收資料、定期運算...等。
    
4.  **後端 API**  
	本堂課重點！！ 可以去實現大量的後端 API，並將這些功能曝露出來供使用者使用。
    
5. **系統之間交互的橋樑**  
   因為其輕量的特性，可以用來作為系統間的溝通橋樑。例如：程式更新去觸發 Jekyll 的更新。
 
<br><br>

##  API

既然本堂課的副標題叫 **《Serverless 在 API 經濟中的應用》**，當然得稍微介紹下 API。

<br>

###  API 經濟 

什麼是 API 經濟？

講師提到，現在開發功能有種用<span class="highlighting">搭積木</span>來實現功能趨勢。換句話說，自己寫的程式越來越少，更多是採用第三方服務。

這樣的開發趨勢，會使得開發人員直接面向整個生態圈打交道 - 不是需要用調用別人的 API ，就是我們實做的 API 被調用。

而所謂 API 經濟，指的是通過 Open API 把後臺的資料、資源...等能力，開放提供第三方人員有償或是無償使用，這樣的開放可以直接對企業利潤帶來引響，或是擴大自身在生態圈影響力，並且很快的完成創新。


 <br>

###  開放 API
 
開放 API　＝　公開 API　＝　Open API　＝　共通性資料存取應用程式介面

上面四個名詞我最熟悉的是第三個，不過它們指得同一件事：

<div class="alert info">
<div class="head">Open API</div>
服務的提供者將專有軟體、應用程式或是 web...等服務，通過網路，向服務使用者提供訪問的interface，且這樣的服務必須是安全且穩定的。
</div>

<br> 在 API 經濟之下，越來越多的企業通過 API 將自己企業的功能和數據開放出去供別人調用，從而給企業本身帶來直接的價值。可是很多情況下，企業已有的功能不適合直接暴露給外部使用戶，需要考慮相關認證的機制以及隱私資料遮蔽，做一定的限制和修改。



<br>

### Serverless 平台實現 Open API

若不採用 Serverless 來實現 Open API，必須先分配個 VM 或 container，然後設定操作系統、執行環境，並上傳程式碼，接下來 7 * 24 等待著。等待前端調用，如果前端調用來了，處理資料丟到後端，再等待後端返回後，增加處裡丟還給前端。
 
除機器必須 7 * 24 運轉外，上線必須花費時間設定相關作業，這些都造成相關服務的提供者開發及維運上的困難。

<br>
<center> <img src="https://i.imgur.com/8GXYW5F.png" alt="Serverless 平台實現 Open API  架構"></center>
<center class="imgtext">Serverless 平台實現 Open API 架構（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-1.pdf" class="imgtext">課程講義</a>）</center>
<br>

上圖是使用 Serverless 平台實現 Open API 的架構，從左向右看，分別是互聯網、防火牆、API Gateway、 FaaS 與要存取的內部系統。

互聯網的部份代表的是使用者的存取，而因為要將 interface 暴露給使用者，所以中間在架個防火牆，防火牆後面才是 API Gateway。

API Gateway 這邊主要負責安全性、流量管理、拜訪與響應的策略及監控、統計、分析...等。 
 
而在 API Gateway 後面則是接　Serverless 中的 FaaS 服務，在這邊就依照你的開發需求去撰寫程式，例如：輸⼊輸出格式轉化、隱私數據屏蔽、加密與解密...等。


<br>

簡單來說 FaaS 這邊放的開放給外部使用時所有需要使用程式來處理的內容。這部份的內容可用任何語言實作。

接下來，通過 API Gateway 曝露出來一個 URL，再將這個 URL 發佈出去就可以了。

使用 Serverless 用的話，可以節省了資源（至少不用 7 * 24 運轉？）


<br>

## 場景示例

<center> <img src="https://i.imgur.com/4rc6ZLU.png" alt="數據訪問 API"></center>
<center class="imgtext">數據訪問 API（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-1.pdf" class="imgtext">課程講義</a>）</center>
<br>

課程這邊講師舉了好幾個場景示例，不過有點懶沒有將它們一一打出來。

不過就是基本上就使用這藉由網路通過 API Gateway 去請求 FaaS，而 FaaS 接到請求後執行相對應的程式，再將結果返回給使用者。

<!--more-->
<br><br>

## 操作示範

這節我能做的操作跟講師不太一樣，她在這章節所使用的是 **OpenWhisk CLI**，但這部份在 IBM 上的操作文件整個不見了！

因此就算從 github 把 [OpenWhisk CLI](https://github.com/apache/incubator-openwhisk-cli/releases) 下載下來，我也不清楚該如何將 wsk CLI 跟 IBM	Function 做連接，再加上有考慮自己架個 OpenWhisk 來試試看，所以 wsk CLI 的使用就決定向後推了 XD

<br>

### 環境準備

1. IBM cloud 帳號
2. 安裝 IBM Cloud Developer Tools 
3. 配置 CLI

<br>

**IBM Cloud Developer Tools**  
前面提過課程中安裝的是 wsk CLI，但因為我在 IBM 找不到文件的關係，所以先改用 IBM 的 CLI。

為了快速試用，所以選擇現成的 Docker Container 來使用，[安裝步驟](https://cloud.ibm.com/docs/cli/reference/bluemix_cli?topic=cloud-cli-using-idt-from-docker#idt-docker-prereq)如下：

1. **從 Docker hub 拉回 Image**  
    ```shell
    $ docker pull ibmcom/ibm-cloud-developer-tools-amd64
    ```

2. **啟動並進入 Docker Container** 
    ```shell
    $ docker run -ti ibmcom/ibm-cloud-developer-tools-amd64
    ```
 
<br>

<div class="alert warning">
<div class="head">Docker Container 環境</div>

IBM 所提供的 Container 作業系統環境是 Alpine Linux。所以一些操作指令需要注意一下。
</div>

<br>

紀錄一下，在 Docker Container 中我需要反覆查詢的指令：
1. **安裝**  
    如要安裝其他程式，如 vim ，用的指令不是 `apt-get`
    
    ```shell
    $ apk update
    $ apk add vim 
    ```  
    
2. **重新進入 Container**  
    如果按照往常慣例，使用
    ```shell
    $ docker exec -ti [ID]  bash
    ```
    
    但會出現下列錯誤訊息：
    ```shell
    OCI runtime exec failed: exec failed: container_linux.go:345: starting container process caused "exec: \"bash\": executable file not found in $PATH": unknown
    ```
    
    依照 [appleboy](https://github.com/appleboy/gorush/issues/319#issuecomment-353721167) 的建議改用 `alpine` tag 
    ```shell
    $ docker exec -ti [ID] /bin/sh
    ```
      
    

<br>

**配置 CLI**  
依照[教學文件](
 https://cloud.ibm.com/functions/learn/cli)配置就行了 
 
1. **登入 IBM Cloud**  
    ```shell
    $ ibmcloud login -a cloud.ibm.com -o [名稱空間] -s "dev"
    ```
    
    不過用這個方式登入的話它會請你輸入帳號密碼，所以我會用右上角頭像中的**登入 CLI 及 API**，裡面有個一次性密碼的指令
    
    ```shell
    $ ibmcloud login -a https://cloud.ibm.com -u passcode -p [一次性密碼]
    ```

    在此步驟之後，會請你設置區域之類的，我記得我是設**美南**（我知道有美中、美西、美東這些說法，但有美南這個說法嗎?）就是 us-south。


2. **安裝 Cloud Functions 外掛程式**  
    ```shell
    $ ibmcloud plugin install cloud-functions
    ```
    
    安裝的時候忘了紀錄，我記得會出現一些安裝與升級的提示，依照提示的指令裝一裝就好了。


3. **目標名稱空間**  

    ```shell
    $ ibmcloud target -o [名稱空間] -s dev
    ```

    再列出可用的資源群組
    ```shell
    $ ibmcloud resource groups
    ```
    
    將此名稱空間的資源群組設定為目標
    ```shell
    $ ibmcloud target -g [name of resource group] 
    ```
 
4. **測試**  
    執行清單指令以顯示現行目標名稱空間中的所有實體。
    ```shell
    $ ibmcloud fn list
    ```
 
 

<br>

### CLI 操作

這堂課主要是在說 API 操作的部份，所以我也依照講師的授課內容，先找相對應的指令。至於其他的指令，我在酌情要貼測試結果還是文件就好。
 
<br>

**action**  

在開始前先準備一段 action 的程式，課程中的範例是關於資料庫的操作，不過我這邊為了偷懶直接印出來假裝有 insert XD

另外，因為我不想預先編譯，所以挑了<span class="highlighting">直譯式語言</span>來實做，而在 Python 與 JavaScript 兩者之間，我挑了比較熟悉的 Python。


```python
def main(args):
    name = args.get("name", "stranger")
    color = args.get("color", "white")
    command = "INSERT INTO catdb VALUES( {} , {} )".format(name,color)
    print(command)
    return {"command":command}  
``` 

<br> 

依照 [教學文件](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-actions) 來 Creating actions    


1. **建立 action**  
    ```shell
    $ ibmcloud fn action create catapi catapi.py --kind python:3.7 --web true
    ok: created action catapi
    ``` 
    <br>

    **kind**  
    因為我是挑 python 所以 `kind` 參數需要設置為 `python:3.7` ，如果是其他語言撰寫請查詢相對應的 [tag](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create)。
    
    如果你的 python 檔案必須依賴第三方套件或是有多份 Python 檔案請查詢  [Packaging 的方法](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-prep#how_to_package_python)。
    
    **web**  
    因為我要直接使用 url 呼叫所以，這邊將 web 設為 true 方便後面直接呼叫。
    
    
2. **刪除 action**   
    恩，第一次建立的時候打錯字了，先把錯誤的 action 刪掉 
    ```shell
    $ ibmcloud fn action delete cayapi
    ok: deleted action cayapi

    ```   
    
3. **查詢 action 細節**  
    如要查詢 action 的細節可以用 `action get` 來查看。
    ```shell
    $ ibmcloud fn action get catapi
    ok: got action catapi
    {
        "namespace": "[名稱空間]_dev",
        "name": "catapi",
        "version": "0.0.1",
        "exec": {
            "kind": "python:3.7",
            "binary": false
        },
        "annotations": [
            {
                "key": "web-export",
                "value": true
            },
            {
                "key": "raw-http",
                "value": false
            },
            {
                "key": "final",
                "value": true
            },
            {
                "key": "exec",
                "value": "python:3.7"
            }
        ],
        "limits": {
            "timeout": 60000,
            "memory": 256,
            "logs": 10,
            "concurrency": 1
        },
        "publish": false,
        "updated": 1586324682689
    }
    ```

 
4. **試著調用 action**
    ```shell
    $ ibmcloud fn action invoke catapi --blocking -r --param name Cynthia --param color black
    {
        "command": "INSERT INTO catdb VALUES( Cynthia , black )"
    }
    ```

    如果想看調用紀錄可以下 `list` 來查看
    ```shell
    $ ibmcloud fn activation list catapi
    Datetime            Activation ID                    Kind       Start Duration   Status          Entity
    2020-04-08 06:07:21 e07e2378302245f1be2378302255f1f5 python:3.7 warm  2ms        success         Cynthia_Ch...com_dev/catapi:0.0.1
    2020-04-08 06:07:11 5ce7fafa63364e64a7fafa6336ce64d3 python:3.7 warm  1ms        success         Cynthia_Ch...com_dev/catapi:0.0.1
    2020-04-08 06:06:53 440e81cfb43a455d8e81cfb43a955d07 python:3.7 warm  2ms        success         Cynthia_Ch...com_dev/catapi:0.0.1
    2020-04-08 06:06:36 47fb4b61be884b62bb4b61be88ab62c1 python:3.7 cold  183ms      success         Cynthia_Ch...com_dev/catapi:0.0.1
    2020-04-08 05:44:20 aa2b3be4990f479bab3be4990fb79b1f python:3.7 warm  1ms        success         Cynthia_Ch...com_dev/catapi:0.0.1
    2020-04-08 05:44:01 4b698055804240ada98055804270aded python:3.7 cold  175ms      success         Cynthia_Ch...com_dev/catapi:0.0.1
    2020-04-08 05:43:36 741d9ac2f43d448d9d9ac2f43d848dd7 python:3.7 cold  162ms      developer error Cynthia_Ch...com_dev/catapi:0.0.1
    ```
    
5. **api 設定**
    ```shell
    $ ibmcloud fn api create /catapi /insert post catapi --response-type json
    ok: created API /catapi/insert POST for action /_/catapi
    https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/b0aa51c2e269fe5f451e1a50345b0bb86f711854511ee59e834395c2f375e57b/catapi/insert
    ```
    
    如此會拿到 api 的 url，就可以從外部使用 curl 呼叫 API 了！

    ```shell 
    $ curl --request POST \
     --url https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/b0aa51c2e269fe5f451e1a50345b0bb86f711854511ee59e834395c2f375e57b/catapi/insert \
    --header "Content-Type: application/json" \
    --data '{"name":"CC", "color":"black"}'

    {
      "command": "INSERT INTO catdb VALUES( CC , black )"
    }
    ```
 
<br>

### 高級 API gateway

不過使用 Openwisk 沒有流量控制、安全性...等功能，必須依賴安裝其他第三方 API 來控管。

不過 IBM Function 倒是另外提供這些功能，但必須從 UI 來控制就是了。
<center> <img src="https://i.imgur.com/9MZkFFQ.png" alt="高級 API gateway"></center>


<br><br> 

## 其他連結
1. [Serverless 應用案例賞析筆記目錄](/Serverless-Use-Cases-Study-Notes-Contents)
2. 課程內容：影片（[IBM片源](https://mediacenter.ibm.com/media/1_u1ucwcoh) 、[youku教育片源](http://v.youku.com/v_show/id_XMzg2MzU5MDY4NA==.html)）/ [講義](https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-01.pdf) / [Blog](https://mp.weixin.qq.com/s/XElPa20WYxdXnprh3ygEiQ)

<br><br> 

## 參考資料 
1. (2020-03-26)。[Using IBM Cloud Developer Tools from a Docker Container](https://cloud.ibm.com/docs/cli/reference/bluemix_cli?topic=cloud-cli-using-idt-from-docker) 。檢自 IBM Cloud Docs (2020-04-08)。
2. appleboy (2017-12-23)。[not able to use docker exec shell · Issue #319](https://github.com/appleboy/gorush/issues/319#issuecomment-353721167) 。檢自 Github (2020-04-08)。 
3. [IBM Cloud Functions - CLI](https://console.bluemix.net/openwhisk/learn/cli) 。檢自 IBM Cloud (2020-04-08)。 
4. (2020-01-17)。[Preparing apps for actions](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-prep#how_to_package_python)。檢自 IBM Cloud Docs (2020-04-08)。
5. (2020-01-15)。[Creating actions](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-actions)。檢自 IBM Cloud Docs (2020-04-08)。
6. (2020-03-09)。[Cloud Functions CLI](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-cli-plugin-functions-cli#cli_action_create)。檢自 IBM Cloud Docs (2020-04-08)。