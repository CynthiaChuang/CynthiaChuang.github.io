---
title: 【Serverless 應用案例賞析筆記】 04. Serverless 在人工智慧領域的應用
date: 2020-04-07
is_modified: false
categories:
- Study-Notes
- Cloud-Computing
tags:
- Other Online Courses
- Serverless 應用案例賞析
- Fass/Serverless
- OpenWhisk
--- 

 
<center> <img src="https://i.imgur.com/cNBG3YD.png" alt="投影片封面"></center>
<center class="imgtext">投影片封面（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-02.pdf" class="imgtext">課程講義</a>）</center>
<br>
   
最後一堂課來談談 Serverless 在 AI/ML 中的應用。

<!--more-->
<br><br>

## 回顧

老樣子：
<div class="blockquote-center">
無需管理、按需執行、按需擴展、按使用來計費
</div>

細節去看[第二堂](/Serverless-Use-Cases-Study-Notes-02#回顧)與[第三堂](/Serverless-Use-Cases-Study-Notes-03#回顧)的筆記吧。

<br><br>
 

## 人工智慧

人工智慧的簡單定義為：『用於模擬、延伸和擴展人類智慧的理論、方法、技術與應用的一門學科』。常見的應用包含：機器學習、自然語言理解、圖像識別與對話閒聊。

在人工智慧中最重要的東西莫過於資料，資料可以用來訓練出一個模型作為應用，爾後當接到新的請求時，就會去調用這個模型進行預測，進而得到結果。

<br>

### 資料流程

<center> <img src="https://i.imgur.com/tcHUG6o.png" alt="機器學習架構"></center>
<center class="imgtext">機器學習架構（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-03.pdf" class="imgtext">課程講義</a>）</center>
<br>

一個典型的資料流程如下：  
當從不同的來源收集足夠資料後，會對原始資料進行清理、整理、提取、隱私資料遮蔽...等各種應用所需的清洗與前處理。處理過得資料會作為我們的訓練集，而這個訓練集質量會直接影響訓練結果。

當得到訓練集後，會根據資料類型與應用目的去挑選語言框架與訓練模型，根據挑選模型再進行參數挑選、優化，以建立一個精確的 AI 模型。

當 AI 模型訓練好後，就可以部屬進入實用/測試。在此階段若新的請求時，就會使用此模型進行預測，並回傳預測結果，而所接到的請求，也可以做為新的資料加入訓練集，等待下一輪的訓練。

<br><br>

## Serverless 與機器學習


優點
1. **服務隔離**  
    最後培育出來的接口可以使用 RESTful 去公開。對於終端使用者來說，他享受的供應端所提供的服務，毋需關心後端模型架構。且相關應用的升級，也不會影響到終端使用者的使用。

2. **零管理**  
   數據科學家或其他試圖架設 AI 模型的開發者，他們無須去關心底層的基礎設施。
    
3. **無需關心擴展**  
   模型部署在雲端，以服務形式提供 inference，而根據負載自動擴展。
   
4. **不執行不付費**  
    只有當模型被調用，才需要付費。其他時候因為沒有被部屬，所以不佔用資源，也因此無須付費。
    
5. **模型便於重複使用**  
    事件觸發模型調用，不同事件可重複調用模型。
    
6. **以模型為單位擴展**  
    每個模型作為獨立立功能，按需調用、更新、刪除和擴展，不不影響其他模型。模型隔離性。
 
<br>

### 在資料準備階段的應用    

<center> <img src="https://i.imgur.com/0RPgiRz.png" alt="Serverless在資料準備階段的應用"></center>
<center class="imgtext">Serverless在資料準備階段的應用（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-03.pdf" class="imgtext">課程講義</a>）</center>
<br>

前面提過的資料清理、整理、提取、隱私資料遮蔽...等各種清洗與前處理都可以使用 Serverless 來實做。當資料進入資料庫時或是定期去調用 Serverless 。


<br>

### 在模型訓練階段的應用    
<center> <img src="https://i.imgur.com/W9oxOCA.png" alt="Serverless在模型訓練階段的應用"></center>
<center class="imgtext">Serverless在模型訓練階段的應用（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-03.pdf" class="imgtext">課程講義</a>）</center>
<br>

而在模型訓練階段，可以用在當有新資料產生時，Serverless 去觸發重新訓練；或是當有新的模型被訓練出來時，可以用 Serverless 去重新啟動 inference，載入新的模型。

不過注意的是 FaaS 是短時運行的，所以不應該整個訓練過程放在 action 裡面，而是應該讓你的 action 去啟動在另外一台機器上的訓練流程。

<br>

### 在預測階段的應用    
<center> <img src="https://i.imgur.com/UB9rFNu.png" alt="Serverless在預測階段的應用"></center>
<center class="imgtext">Serverless在預測階段的應用（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-03.pdf" class="imgtext">課程講義</a>）</center>
<br>

在使用階段最經典的用法就是，要進行預測時呼叫 Serverless 來部署模型並給出預測結果。

除此之外，呼叫請求進來時，也可以使用 FaaS 去進行資料的前處理，如斷詞、影像大小處理...等。

<br>

### 範例-識別模型

<center> <img src="https://i.imgur.com/ExCDhGM.png
" alt="識別模型"></center>
<center class="imgtext">識別模型（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-03.pdf" class="imgtext">課程講義</a>）</center>
<br>

這邊案例是訓練機器人來辨識圖片：
1. 開發者將影像資料上傳到儲存服務中。
2. 開發者製作一個 docker image，這個 image 中包含了 tensorflow 等訓練所需執行環境。將這個環境在 kubernetes 上執行訓練。
3. 訓練時會從儲存服務取得訓練資料。
4. 當訓練完成後再將模型儲存回儲存服務中。
5. 開發者另外需製作一個 docker image，這個 image 僅須包含執行模型所需的環境即可。並在製作一個 action 以方便調用者進行調用。
6. 調用者會透過 trigger 或是直接調用等方法，去調用這個 action。
7. 當使用者調用這個 action 後，就會從儲存服務中取回模型，建立所需環境後，就執行這個動作。
8. 最後再將執行結果回傳給調用者。

課程是舉了兩個例子，另一個我就跳過了~XD

<br><br>

## 無服務器機器學習 Serverless Machine Learning
在討論機器學習與 Serverless 時，還會看到另外一個詞叫做 <span class="highlighting">Serverless Machine Learning</span>。

它指的是開發人員在第三方雲端平台上訓練與部署模型，並進行模型版本管理...等，而不用考慮基礎設施。

這部份的話，則偏向於之前提過的 BaaS 範疇。

<br><br>

## FaaS 用於機器學習的挑戰

- **程式碼有大小有限制**  
    根據 [Openwhisk System limits](https://github.com/apache/openwhisk/blob/master/docs/reference.md#actions) 預設 action 大小是 48MB，但這個可以在自己架設時修改。如果真的超過設定大小也可透過自訂 docker 的方式來解決。
    
-  **輸入輸出有限制**  
    根據 [Openwhisk System limits](https://github.com/apache/openwhisk/blob/master/docs/reference.md#actions)有傳入參數 1MB、寫出 log 10 MB 限制。可以藉第三方存儲服務來處理。
    
- **執行時間有限制**  
    預設最長 5 分鐘，一樣可以在自己架設時修改。
    
以上的一些限制條件導致機器學習的訓練並不適合在 FaaS 上執行，上億筆的訓練資料、動不動就訓練個一天甚至一周。

雖然有些限制條件在自己架設時可以放寬，但即便放寬了，個人還是覺得跟一開始**短時運行**的定義起衝突


<br><br>

## 實作

### Serverless 函數調⽤ Waston API 實現圖⽚識別

快速紀錄一下，講師所用到的指令，方便之後查詢。

<center> <img src="https://i.imgur.com/3B3C1lH.png
" alt="流程圖"></center>
<center class="imgtext">流程圖（圖片來源: <a href="https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-03.pdf" class="imgtext">課程講義</a>）</center>
<br>


**Package**
```shell
$ wsk	package	bind	/whisk.system/cloudant serverless-pattern-cloudant-package	\	
-p	username	$CLOUDANT_USERNAME	\	
-p	password	$CLOUDANT_PASSWORD	\	
-p	host	${CLOUDANT_USERNAME}.cloudant.com
```
 
**Trigger**
```shell
$ wsk	trigger	create	update-trigger	--feed	serverless-pattern-cloudant-package/changes	\	
--param dbname	images	
```

**Action**
```shell
$ wsk	action	create	update-document-with-watson	actions/updateDocumentWithWatson.js	\	
--kind	nodejs:8	\	
--param	USERNAME	$CLOUDANT_USERNAME	\	
--param	PASSWORD	$CLOUDANT_PASSWORD	\	
--param	DBNAME	$CLOUDANT_IMAGE_DATABASE	\	
--param	DBNAME_PROCESSED	$CLOUDANT_TAGS_DATABASE	\	
--param	WATSON_VR_APIKEY	$WATSON_VISUAL_APIKEY
```

**Rule**
```shell
$ wsk	rule	create	update-trigger-rule	update-trigger	update-document-with-watson
```
 
<br><br> 

## 其他連結
1. [Serverless 應用案例賞析筆記目錄](/Serverless-Use-Cases-Study-Notes-Contents)
2. 課程內容：影片（[IBM片源](https://mediacenter.ibm.com/media/04_Serverless+%E5%9C%A8%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E9%A2%86%E5%9F%9F%E7%9A%84%E5%BA%94%E7%94%A8/1_0c1fmn7k) 、[youku教育片源](https://v.youku.com/v_show/id_XMzg4ODUzNzQwOA==.html)）/ [講義](https://github.com/dWChina/ibm-opentech-ma/blob/master/serverless-use-cases/Serverless-03.pdf) / [Blog](https://mp.weixin.qq.com/s/vlWQISKHktcLC--baAgpoA)

<br><br> 

## 參考資料 
1. 協同撰寫 (2019-02-14)。[OpenWhisk system details](https://github.com/apache/openwhisk/blob/master/docs/reference.md#actions) 。檢自 openwhisk - github (2020-04-15)。
