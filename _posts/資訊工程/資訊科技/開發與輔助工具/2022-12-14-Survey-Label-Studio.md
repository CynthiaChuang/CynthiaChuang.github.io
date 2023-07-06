---
title: Survey｜Label Studio
date: 2023-02-16
is_modified: false
image: https://i.imgur.com/rlEl5Z6.png
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- 工具安裝與部署
- 工具介紹與操作
- Label Studio
--- 


我竟然還是從草稿夾中把這篇給翻出來填坑...。上次搞這個都是一年前了，但最近又被重新 assign 了這個 project，也只能重新去把記憶給找回來了。嘖...要回想一年多前做了什麼事情真是難倒我了＝＝
  
上次看的時候時候版本還是 1.0，現在都 1.6 啦...不過筆記應該沒有差多少？應該...

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/rlEl5Z6.png" alt="Label Studio">
    Label Studio（圖片來源: <a href="https://github.com/heartexlabs/label-studio">GitHub</a>）
</p>


## Overview
故事的開始好像是從 CVAT 開始，它是套是開源的影像標注工具，功能雖然完善但主要專注於影像方面，界面也稍嫌複雜。如果要標注文本或是 NLP 就需要另尋它法...雖然我拿 excel 也能標啦 XDDD

在找標注工具時，大概考慮了幾個點：
1. **開源**：~~大概是因為老闆不想多付錢 XDDD~~
2. **友好界面，有 UI 支援更加**：都得標文字資料，再不來點 UI 洗洗眼睛，應該很快就膩了。
3. **協作功能**：相信我不會想要一個人標完整個資料集。
4. **管理機制**：主要是看訪控權限跟 project 管理機制，免得有天兵砍我資料（也不是沒發生過＝＝。
 
我是有看到一套我還滿喜歡的 [Prodigy](https://demo.prodi.gy/?=null&view_id=ner_manual)，不過它看起來只能標注文本，重點是它不是開源的。Survey 到最後上位的就是 Label Studio 啦～！

<p class="illustration">
    <img src="https://i.imgur.com/toUwkBX.png" alt="What is Label Studio?">
    What is Label Studio?（圖片來源: <a href="https://labelstud.io/">Label Studio 官網</a>）
</p>

忽然意識到他們有換配色耶，尾巴的顏色也不太一樣了...不過好像舊版的顏色比較合我胃口？
 

### Labeling Workflow
開始看 Label Studio 之前，先看下標注流程，因為無論是要進行人工標注（Human Labeling）或是半監督學習（Semi-supervised Labeling），第一步都是資料標注。

其中人機協作是常見的資料標注流程：
1. **資料收集**：  
	通過爬蟲、程式或者其它工具，將資料保存到資料庫中。
2. **資料前處理/資料標注**：  
	因為收集回來的資料多是雜亂無章的，若要標注、甚至進行訓練前，都必需先將資料進行清洗。清洗完的資料可以直接用於無監督學習（Unsupervised Labeling）做預訓練模型，也可以將整理後的資料進行標注，變成有標籤資料。<br><br>	
	標注的方式與目標，會依照訓練目的而有所不同。例如對於文本的標注有：文本分類、命名實體識別、文本摘要...等；對於音訊可以做講者分類、情緒識別或轉錄...等；對於圖片則可能有圖像分類、物件檢測、語義分割...等。<br><br>	
	是說，如果是多人協作標注資料，最好在開始前確定標注原則...至少...確定要使用的標籤，別出現一人標貓、一人標 cat。
3. **模型訓練**：  
    有了標注完成的資料後，即可將資料導入，從無到有進行訓練；或者選擇在該領域或是其他領域的模型進行 Fine-Tune 或遷移學習（Transfer Learning）。
4. **模型評估**：  
	當訓練完成後，使用測試集或是新收集的資料進行檢驗，若是訓練結果無法有良好的表現，可能需要調整參數重新訓練。
5. **重新訓練 or 模型部署**：  
 	不斷重複前 1～4 步，以優化模型和資料，提高模型性能。一旦取得合乎預期的表現後，就可將模型進一步部署在設備端或伺服器上。 
	<p class="illustration">
		<img src="https://i.imgur.com/V7KOD9m.png" alt="Usually Labeling Workflow">
		Usually Labeling Workflow（圖片來源: <a href="https://towardsdatascience.com/introducing-label-studio-a-swiss-army-knife-of-data-labeling-140c1be92881">Towards Data Science</a>）
	</p>
	
### What is Label Studio?
通常完成上述的步驟需耗時數週到數月的時間，所以 Heartex 的首席技術官希望能整合相關功能到自動化的平台，從而減少技術團隊的產品、機器學習的開發、實驗時間學習生命週期。

<br class="big">

Heartex 就是推出 Label Studio 的公司，而 Label Studio 是一套開源的標注工具，它可應用範圍很廣，包含圖片、音訊、文字、影片...等格式的資料。此外它還提供簡明 UI 可快速配置多種資料，能在 10 分鐘內準備好工具，用以標注文本、音訊或圖像...等資料格式。

它另外一個宣傳點則是可將 Label Studio 與機器學習模型進行集成。機器學習可以用來協助標注，以提供標籤預測（預標籤），並可執行持續的主動學習。

不過有些我希望的功能，例如：訪控權限，並不是在開源版本中，不過我這還能接受 XDDD 
 
<p class="illustration">
	<img src="https://i.imgur.com/CzmobYV.png" alt="Usually Labeling Workflow">
	Usually Labeling Workflow（圖片來源: <a href="https://towardsdatascience.com/introducing-label-studio-a-swiss-army-knife-of-data-labeling-140c1be92881">Towards Data Science</a>）
</p>


### Components and Architecture
根據 Label Studio 的軟體架構圖，系統可以大致分成：**Frontend**、**Backend**、**Task**、**ML Backend** 4 個部份：

<p class="illustration">
	<img src="https://i.imgur.com/sE306vT.png" alt="Label Studio Architecture">
Label Studio Architecture（圖片來源: <a href="https://labelstud.io/guide/index.html#Components-and-architecture">Label Studio Documentation</a>）
</p>

顧名思義，各元件負責前端、執行標注、標籤資料與任務管理、以及串接 ML Backend 的部份。各個元件基本都有提供 [Source Code](https://labelstud.io/guide/index.html#Components-and-architecture)，方便開發者自行客製化。

從表中也可以看到各元件的實做方法中，其中大概除了 [MST](https://github.com/mobxjs/mobx-state-tree) 我可能找不到替罪同事外，其他應該都可以？而且授權看來都是 **Apache 2.0 LICENSE**，所以可以放心改 XDDD

<p class="illustration">
	<img src="https://i.imgur.com/wZU025g.png" alt="Label Studio Components">
	Label Studio Components（圖片來源: <a href="https://labelstud.io/guide/index.html#Components-and-architecture">Label Studio Documentation</a>）
</p>


#### Label Studio Backend
- [heartexlabs/label-studio](https://github.com/heartexlabs/label-studio/)  

基本上，Backend 是它主體了 XDDD 在這份 Source Code 中，你可以找到 frontend 跟 data_manager。安裝所用的 docker 與 docker-compose 都出自於這一份。


#### Label Studio Frontend
- A. [heartexlabs / label_studio/frontend](https://github.com/heartexlabs/label-studio/tree/master/label_studio/frontend)
- B. [heartexlabs / label-studio-frontend](https://github.com/heartexlabs/label-studio-frontend)

A 混在 Backend 的 project 內，安裝 Backend 時會順便起起來，如果要直接起則是 B。但目前從 commit 看來兩邊專案似乎不齊頭？

這個專案所使用到的技術，就是我們在上表看到的 React 跟 MST（mobx-state-tree）。


#### Data Manager
- [heartexlabs/dm2](https://github.com/heartexlabs/dm2)


<p class="illustration">
	<img src="https://i.imgur.com/tTP9yoD.png" alt="Data exploration tool for Label Studio.">
	Data exploration tool for Label Studio.（圖片來源: <a href="https://github.com/heartexlabs/dm2">GitHub</a>）
</p>

這專案可讓使用者輕鬆探索資料集，可以選擇如何查看與顯示的資料，如：網格和列表視圖...等。當然這個專案必須跟前端集成，因為 DataManager 使用 LabelStudio API 進行操作。

<p class="illustration">
	<img src="https://i.imgur.com/mIWHD8y.png" alt="LabelStudio API Workflow">
	LabelStudio API Workflow（圖片來源: 拍謝，一年前留的圖片我忘記來源了，知道的麻煩告訴我一聲，謝謝！）
</p>


#### Machine Learning Backends
- [heartexlabs/label-studio-ml-backend](https://github.com/heartexlabs/label-studio-ml-backend)

Machine Learning Backends 是一個 SDK，可讓使用者封裝機器學習程式碼並將其變成換為 Web 服務器。最後將該服務器連接到 Label Studio Instance 以執行 2 個任務：

1. 基於模型推理結果動態預標注資料
2. 根據最近注釋的資料重新訓練或微調模型


#### Label Studio Enterprise Edition
官網是建議，在客製化之前先參考下 Label Studio Enterprise Edition 咩，有了它就可以不用自己造輪子了呦～！不過我沒研究過他的[收費標準](https://labelstud.io/guide/billing.html)，有考慮的可能得查一下。

<p class="illustration">
	<img src="https://i.imgur.com/MqJnisw.png" alt="Label Studio Features">
	Label Studio Features（圖片來源: <a href="https://labelstud.io/guide/index.html#Components-and-architecture">Label Studio Documentation</a>）
</p>

是說...那個訪控權限跟標注 review 的功能，看起來就很棒！


### Data Type
它們能支援的格式或說標注動作還不少，以資料類型來說大致包含了 <mark>Images、 Audio、 Text、 Time Series、 Multi-Domain、 Video</mark>：

1. **Images**  
	- 支援圖片分類、物體檢測、語義分割。
2. **Audio**  
	- 支援音源分類、講者分類、情緒識別、轉錄。	
3. **Text**  
	- 支援文檔分類、NER、問答、情緒分析。
4. **Time Series**  
	- 支援時間序列分類、分割、事件識別。
5. **Multi-Domain**  
	- 支援對話處理（呼叫中心錄音可以同時轉錄和處理為文本）、OCR、將影像或音檔分割時間序列數。
9. **Video**  
	- 支援影像分類、物件追蹤、輔助標記。

<p class="illustration">
	<img src="https://i.imgur.com/1re3sxj.gif" alt="Label every data type.">
	Label every data type.（圖片來源: <a href="https://labelstud.io/">Label Studio</a>）
</p>

各資料所能接受的資料格式如下，不過我好像沒看到 video 的資料格式？是還沒更新上去嗎（video 是 1.6 版新增的）？


<p class="illustration">
	<img src="https://i.imgur.com/ALRMGe9.png" alt="Types of data you can import">
	Types of data you can import（圖片來源: <a href="https://labelstud.io/guide/tasks.html#Types-of-data-you-can-import-into-Label-Studio">Label Studio Documentation</a>）
</p>


### Pros and Cons
稍微玩了下這套工具，簡單列些優缺點。不過在開始前先看看官方給的優點：
1. **簡單**：  
	沒有複雜的配置，並且易於集成到機器學習管道中。

2. **可針對多種資料類型快速配置**：  
	可在 10 分鐘內準備好工具，以在標記文本、音源與圖像之間切換。甚至可以同時標注三種類型，不過這種複合式的用法得自製 template 才可用。
 
	<p class="illustration">
	<img src="https://i.imgur.com/lvZrPku.png" alt="同時標注三種類型">
	同時標注三種類型
	</p>

3. **機器學習集成**   
	它能與所有眾多的機器學習框架和模型集成。ML 有許多不同的約束條件應用程序，Label Studio 必須足夠靈活以處理它們並提供幫助，而不是使其複雜化。

<br class="big">

玩完後，它確實可以稱得上它所宣稱的優點：
1. **界面相對漂亮**  
	這套算是我玩過工具中，界面比較漂亮的。
2. **部署方便**  
	它提供了 2～3 種的部署方式，且每種方式的部署也都還滿簡單的。
3. **配置方便，且有多種內置模板與支持多種資料類型**  
	這前面說過了，就不再提了。
4. **機器學習集成**  
	可以串接訓練好的推論服務，達到輔助標注的目的。
	
	
至於缺點的部份，除了權限機制跟 plug-in 的部份（好吧，這其實也不太算是缺點，是閹割的結果）外，我覺得比較麻煩的是的是它的**錯誤訊息難以閱讀**，這蟲要抓會很麻煩 XDDD

<p class="illustration">
<img src="https://i.imgur.com/rmaUE5O.png" alt="Label Studio 界面">
Label Studio 界面（圖片來源: <a href="https://labelstud.io/guide/tasks.html#Types-of-data-you-can-import-into-Label-Studio">Label Studio Documentation</a>）
</p>


## Quick start
OK，來試著安裝它吧！先貼傳送門，等等應該用的上：
- [Install and upgrade Label Studio](https://labelstud.io/guide/install.html)
- [Start Label Studio](https://labelstud.io/guide/start.html)
- [Database setup](https://labelstud.io/guide/storedata.html)


### System requirements
安裝前先確定下安裝環境，主要是儲存空間的部份，特別是如果是要作為 production 那空間需要特別注意：

1. **軟體要求**  
	- Python 3.6 +（不過我覺得應該要 **Python 3.7** + 才對？）
	- Linux、Windows or MacOSX
	- PostgreSQL 11.5 版 or  SQLite 3.35 +

2. **空間**   
	- 使用 SQLite 資料庫時，100 萬筆的標注任務佔用大約 2.3GB 的硬碟空間。若是要作為 production，建議使用 **50GB** 的硬碟空間。
	- 至於記憶體的部份至少使用 8GB RAM，但**建議使用 16GB RAM**。


### Install Label Studio & Start Label Studio
他的安裝方式有 4 種，可以依照需要的方式來安裝：

1. **pip**  
	這大概是最簡單的安裝方式，不過需要注意下 Python 版本，文件中有說需要 **Python 3.7 +** 的版本：
	
	```bash
	$ pip install -U label-studio
	$ label-studio
	```
	
	<br class="big">
	
	啟動時，默認 Web 瀏覽器會在 `http://localhost:8080`。 這個方式在啟動時是資料庫是使用 SQLite，如果要用 PostgreSQL 可以在啟動時設定：
	
	```bash	
	$ label-studio start my_project --init -db postgresql
	```
	
	不過還需要設定點環境變數：
	
	```bash	
	DJANGO_DB=default
	POSTGRE_NAME=postgres
	POSTGRE_USER=postgres
	POSTGRE_PASSWORD=
	POSTGRE_PORT=5432
	POSTGRE_HOST=db
	```
	
	<br class="big">
	
	另外，還有個參數我覺得應該也能派上用場： `LABEL_STUDIO_BASE_DATA_DIR` / `--data-dir`，前者用在環境變數、後者用在命令列，不過目的是一樣，從文件中看來這個值應該可以讓每次啟動都讀到相同的資料庫...不然每次啟動都要重標資料？怎麼可能啦 XDDD
	
	

2. **Docker**  
 	這個大概是我比較常用的方法：
	```bash
	$ docker run -it -p 8080:8080 -v `pwd`/mydata:/label-studio/data heartexlabs/label-studio:latest
	```

	在 mount 進去的 mydata 資料夾後，會看到存放資料的資料夾 media 跟與標籤存放的 `label_studio.sqlite3`。若要換成 PostgreSQL 或是指定資料庫的話可以用 `-e` 的參數搭配 `LABEL_STUDIO_DATABASE`、`LABEL_STUDIO_BASE_DATA_DIR` 這兩個環境變數使用。

	<br class="big">
    
	不過我這次在玩 1.6 時候會遇到，一連串 `OpenBLAS blas_thread_init` 的 error：
	
	<p class="illustration">
	<img src="https://i.imgur.com/qUsoNyU.png" alt="Label Studio 界面">
	Label Studio 界面（圖片來源: <a href="https://labelstud.io/guide/tasks.html#Types-of-data-you-can-import-into-Label-Studio">Label Studio Documentation</a>）
	</p>

	對於這個我有點莫名奇妙，想想看當你上一秒用 1.0 跑得很愉快的時候，下一秒發現有更新，就順手換到 1.6 版，然後就 GG 了...你能想像這有多崩潰阿 XDDD
	
	而且最悲劇的是，我在 [Label Studio Documentation](https://labelstud.io/guide/index.html) 找不到相關的訊息，還好最後 [git 的 docs](https://github.com/heartexlabs/label-studio/blob/6efe3b21aad7ba69ed04c28aa68314174c78bfdf/docs/source/guide/install.md#openblas-blas_thread_init-pthread_create-failed-for-thread-x-of-y-operation-not-permitted) 有找到，謝天謝地！
		
	簡而言之，就是 Docker Engine 的版本太低了，文件中要求 Docker Engine 需要 >= `20.10.12`。不過我實驗用的環境是 Ubuntu 16.04，Docker Engine 升不上 `20.10.12`  (艹皿艹 )
	 
 

3.  **Docker Compose**   
	是說如果真要部署 production ，應該用 Docker Compose 比較適合

	```bash
	$ git clone https://github.com/heartexlabs/label-studio.git
	$ cd label-studio
	$ docker-compose up -d
	Creating network "label-studio_default" with the default driver
	Creating volume "label-studio_static" with default driver
	...
	Creating label-studio_db_1 ... done 
	Creating label-studio_app_1 ... done
	Creating label-studio_nginx_1 ... done
	```
	
	完成後可以看到有 3 容器被啟動：
	- **Label Studio**：就是本體阿。
	- **Nginx**：proxy web server 是用於加載各種靜態資料，包括上傳的音檔、圖像...等。
	- **PostgreSQL**：這邊是用於替代性能較低的 SQLite3。
	
	各容器的詳細設定就直接看看 [docker-compose.yml](https://github.com/heartexlabs/label-studio/blob/develop/docker-compose.yml) 吧。
	
	```bash
	$ docker ps
	CONTAINER ID        IMAGE                                         COMMAND                  CREATED              STATUS              PORTS                    NAMES
	fdd6ea6bb8b8        nginx:latest                                  "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp     label-studio_nginx_1
	de66bb2f5bfc        heartexlabs/label-studio:latest               "./deploy/docker-ent…"   About a minute ago   Up About a minute   8080/tcp                 label-studio_app_1
	b4108f4d947f        postgres:11.5                                 "docker-entrypoint.s…"   About a minute ago   Up About a minute   5432/tcp                 label-studio_db_1
	```


4.  **Source Code**  
	這邊通常用有需要改動程式碼的時候會用到。例如若要更改前端，可以進入修改 `frontend/` 文件夾，不過改完後後，記得重 build：

	```bash
	cd label_studio/frontend/
	npm ci
	npx webpack
	cd ../..
	python label_studio/manage.py collectstatic --no-input
	```
	
	是說，如果文件建議改這份前端的話，所以前端是以這份為主？
	
	
	<br class="big">
	
	改完後有兩種用法：
	1. **直接運行**：  
		```bash
		git clone https://github.com/heartexlabs/label-studio.git
		cd label-studio
		# Install all package dependencies
		pip install -e .
		# Run database migrations
		python label_studio/manage.py migrate
		# Start the server in development mode at http://localhost:8080
		python label_studio/manage.py runserver
		```
		
	2. **包成 Docker image**：  
		因為我要部署上伺服器，所以我傾向會把它包成 image。在 source code 中有現成的 dockerfile，所以可以直接 build 了，不過需要注意的是，跟 Docker 一樣 Docker Engine 太低會 build 不起來，不然你[會發現它 apt update 會一直 fail](https://stackoverflow.com/questions/71941032/why-i-cannot-run-apt-update-inside-a-fresh-ubuntu22-04 )。

<br class="big">

啟動完成後就可以看倒吊的...老鼠（！？）

<p class="illustration">
<img src="https://i.imgur.com/6GL45uv.png" alt="Label Studio 界面">
Label Studio 界面（圖片來源: <a href="https://labelstud.io/guide/tasks.html#Types-of-data-you-can-import-into-Label-Studio">Label Studio Documentation</a>）
</p>

<br class="big">

喔，[查了一下](https://labelstud.io/blog/what-s-with-the-label-studio-opossums/)原來牠是負鼠（opossums），而且人家是女孩子，名叫海蒂（Heidi）XDDD

<p class="illustration">
<img src="https://i.imgur.com/5myGKmv.png" alt="Hi, Heidi">
Hi, Heidi（圖片來源: <a href="https://labelstud.io/blog/what-s-with-the-label-studio-opossums/">Label Studio</a>）
</p>


### Labeling workflow 
安裝完成並啟動 Label Studio 後，就可以開始嘗試標注：

#### **Step1: Create accounts for Label Studio.**
- [Set up user accounts](https://labelstud.io/guide/signup.html)

開始前須先註冊並建立一個帳戶，以開始標記資料和設置專案。目前有兩種方式可以註冊：
1. **UI**  
	就會看到剛剛那隻爬爬走的負鼠，在頁面中有一個 sing up 的選項。

2. **terminal**  
	另一種是在啟動 Label Studio 時在指令順便建立一個帳號：
    ```bash
	$ label-studio start --username <username> --password <password> [--user-token <token-at-least-5-chars>]
    ```
	
	
	如果是用 docker 則是加上 `LABEL_STUDIO_USERNAME` 與 `LABEL_STUDIO_PASSWORD` 兩個環境變數：
    ```bash
	$ docker run -it -p 8080:8080 -v `pwd`/mydata:/label-studio/data -e LABEL_STUDIO_USERNAME=<my-email>  -e LABEL_STUDIO_PASSWORD=<my-password> heartexlabs/label-studio:latest
    ```
	
	<br class="big">	
	
	如果要禁用註冊頁面，讓僅有人使用邀請鏈接人使用，在啟動 Label Studio 前加上環境變數：
    ```bash
	$ export LABEL_STUDIO_DISABLE_SIGNUP_WITHOUT_LINK=true
	```
	
	再用指令啟動並建立一個基礎帳戶，以用於登入 Label Studio：
	
	```bash
	$ label-studio start --username <username> --password <password> [--user-token <token-at-least-5-chars>]
    ```
	
	或是直接在 docker 啟動指令上加上環境變數：
	
	```bash
	$ docker run -it -p 8080:8080 -v `pwd`/mydata:/label-studio/data -e LABEL_STUDIO_DISABLE_SIGNUP_WITHOUT_LINK=true -e LABEL_STUDIO_USERNAME=<my-email>  -e LABEL_STUDIO_PASSWORD=<my-password> heartexlabs/label-studio:latest
	```

	登入 Label Studio 後，再邀請協作者。



#### **Step2: Set up the labeling project/Set up the labeling interface.**
- [Set up your labeling project](https://labelstud.io/guide/setup_project.html)
- [Set up your labeling interface](https://labelstud.io/guide/setup.html)
- [Get data into Label Studio](https://labelstud.io/guide/tasks.html)
- [Label and annotate data](https://labelstud.io/guide/labeling.html)

先針對要標記的資料，定義標記類型並配置專案設置：
1. 建立專案：  
	<p class="illustration">
	<img src="https://i.imgur.com/a0Ox5mG.png" alt="建立專案">
	</p>

2. 導入資料以進行標注任務：
    - [Sync data from external storage](https://labelstud.io/guide/storage.html)
    
	從文件看來資料來源可以是：**雲端儲存體**，如：Amazon S3、 Google Cloud Storage、 Microsoft Azure Blob storage...等， **Redis database**、 **URL** 與 **Local storage**。

	其中從 Local 端上傳資料是比較簡單的做法：
	
	<p class="illustration">
	<img src="https://i.imgur.com/GOniSBJ.png" alt="從 Local 端上傳資料">

	<img src="https://i.imgur.com/NkS4Dwf.png" alt="從 Local 端上傳資料">
	</p>

 
3. labeling interface  
    - [Templates](https://labelstud.io/templates/)

	上傳完資料後，就可以開始配置標注模板，制定模板的方式有兩種。一是使用現成的模板，能選擇的方式還頗多，基本上就是參考 [Data Type](#Data-Type) 的支援。
	
	因為我手邊的是貓狗資料集，所以我這邊選擇影像分類：
	
	<p class="illustration">
	<img src="https://i.imgur.com/HsEWU5w.png" alt="labeling interface">
	</p>
	
	<p class="illustration">
	<img src="https://i.imgur.com/GXYAQd4.png" alt="labeling interface">
	</p>
	
	
	<br class="big">	
	
	另一種起相同標注模板的方式是使用 Template 撰寫：
	
	<p class="illustration">
	<img src="https://i.imgur.com/aLyHdLH.png" alt="labeling interface">	
	</p>
	
	<p class="illustration">
	<img src="https://i.imgur.com/45uZECY.png" alt="labeling interface">
	</p>
	
		
	<br class="big">	
	
	如果現有的標注模板不符合使用需求，可以[自定義符合需求的 template](https://labelstud.io/guide/setup.html#Customize-a-template)，像是之前提過的標注三種類型的 template：

	<details class="details-2">
	<summary>自定義 template：</summary>
	<pre>
	<span>&lt;<span>View</span>&gt;</span>
	<span class="hljs-comment">&lt;!-- Image with Polygons --&gt;</span>
	<span>&lt;<span>View</span> <span>style</span>=<span>"padding: 25px;
				box-shadow: 2px 2px 8px #AAA"</span>&gt;</span>
	<span>&lt;<span>Header</span> <span>value</span>=<span>"Label the image with polygons"</span>/&gt;</span>
	<span>&lt;<span>Image</span> <span>name</span>=<span>"img"</span> <span>value</span>=<span>"$image"</span>/&gt;</span>
	<span>&lt;<span>Text</span> <span>name</span>=<span>"text1"</span>
			<span>value</span>=<span>"Select label, start to click on image"</span>/&gt;</span>

	<span>&lt;<span>PolygonLabels</span> <span>name</span>=<span>"tag"</span> <span>toName</span>=<span>"img"</span>&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Airbus"</span> <span>background</span>=<span>"blue"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Boeing"</span> <span>background</span>=<span>"red"</span>/&gt;</span>
	<span>&lt;/<span>PolygonLabels</span>&gt;</span>
	<span>&lt;/<span>View</span>&gt;</span>
	<span class="hljs-comment">&lt;!-- Text with multi-choices --&gt;</span>
	<span>&lt;<span>View</span> <span>style</span>=<span>"margin-top: 20px; padding: 25px;
				box-shadow: 2px 2px 8px #AAA;"</span>&gt;</span>
	<span>&lt;<span>Header</span> <span>value</span>=<span>"Classify the text"</span>/&gt;</span>
	<span>&lt;<span>Text</span> <span>name</span>=<span>"text2"</span> <span>value</span>=<span>"$text"</span>/&gt;</span>
	<span>&lt;<span>Choices</span> <span>name</span>=<span>"choices1"</span> <span>toName</span>=<span>"img"</span> <span>choice</span>=<span>"multiple"</span>&gt;</span>
		<span>&lt;<span>Choice</span> <span>alias</span>=<span>"wisdom"</span> <span>value</span>=<span>"Wisdom"</span>/&gt;</span>
		<span>&lt;<span>Choice</span> <span>alias</span>=<span>"long"</span> <span>value</span>=<span>"Long"</span>/&gt;</span>
	<span>&lt;/<span>Choices</span>&gt;</span>
	<span>&lt;/<span>View</span>&gt;</span>

	<span>&lt;<span>View</span> <span>style</span>=<span>"margin-top: 20px; padding: 25px;
				box-shadow: 2px 2px 8px #AAA;"</span>&gt;</span>
	<span>&lt;<span>Header</span> <span>value</span>=<span>"Named entity"</span>/&gt;</span>
	<span>&lt;<span>Labels</span> <span>name</span>=<span>"label"</span> <span>toName</span>=<span>"text"</span>&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Person"</span> <span>background</span>=<span>"red"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Organization"</span> <span>background</span>=<span>"darkorange"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Fact"</span> <span>background</span>=<span>"orange"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Money"</span> <span>background</span>=<span>"green"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Date"</span> <span>background</span>=<span>"darkblue"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Time"</span> <span>background</span>=<span>"blue"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Ordinal"</span> <span>background</span>=<span>"purple"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Percent"</span> <span>background</span>=<span>"#842"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Product"</span> <span>background</span>=<span>"#428"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Language"</span> <span>background</span>=<span>"#482"</span>/&gt;</span>
		<span>&lt;<span>Label</span> <span>value</span>=<span>"Location"</span> <span>background</span>=<span>"rgba(0,0,0,0.8)"</span>/&gt;</span>
	<span>&lt;/<span>Labels</span>&gt;</span>

	<span>&lt;<span>Text</span> <span>name</span>=<span>"text"</span> <span>value</span>=<span>"$text"</span>/&gt;</span>
	<span>&lt;/<span>View</span>&gt;</span>
	<span>&lt;/<span>View</span>&gt;</span>
	</pre>
	</details>


 
	<p class="illustration">
	<img src="https://i.imgur.com/lvZrPku.png" alt="同時標注三種類型">
	同時標注三種類型
	</p> 
	
	
4. Label and annotate the data.    
	設定完後就可以嚕貓了 XDDD
	<p class="illustration">
	<img src="https://i.imgur.com/nIFNsm8.png" alt="貓狗影像分類">
	</p> 
		
	<br class="big">	
	
	順便試試物件偵測：	
	
	<p class="illustration">
	<img src="https://i.imgur.com/UKbrhZp.png" alt="物件偵測">
	</p> 
		
	<br class="big">	
	
	跟車牌標注：	
	<p class="illustration">
	<img src="https://i.imgur.com/anuaKtP.png" alt="貓狗影像分類">
	</p> 		


    
5. Export the labeled data or the annotations   
	- [Export annotations and data from Label Studio](https://labelstud.io/guide/export.html)  
	
	因為只有企業版本才能從 UI 會出標注結果，所以我們只能用指令來匯出：


   ```bash
   $label-studio export <project-id> <export-format> --path=<output-path>
   ```


### Machine Learning Backends
這邊試著設置 ML Backends。[文件中](https://labelstud.io/guide/ml.html)提供了兩種安裝方法：`docker-compose` 或是指令。

我這邊使用指令方式安裝：


1. 首先先 Clone the repo
    ```bash
    $ git clone https://github.com/heartexlabs/label-studio-ml-backend  
    ```

2. Setup environment。建議使用venv，不過我在 label studio 的容器內執行，所以我就沒管它了。
    ```bash
    $ cd label-studio-ml-backend
    $ pip install -U -e .
    $ pip install -r label_studio_ml/examples/requirements.txt
    ```

3. 根據範例腳本初始化 ML 後端。
    ```bash
    $ label-studio-ml init my_ml_backend --script label_studio_ml/examples/simple_text_classifier.py
    ```
    此 ML 後端是 Label Studio 提供的 [example](https://github.com/heartexlabs/label-studio-ml-backend/blob/master/label_studio_ml/examples/simple_text_classifier/simple_text_classifier.py)。
    
4. Start ML 後端 server
    ```bash
    $ label-studio-ml start my_ml_backend
    ```
	
5. 最後啟動 Label Studio 後，前往在專案設置頁面的機器學習部分，添加指向 `http://localhost:9090` 機器學習模型後端的連結。
	<p class="illustration">
	<img src="https://i.imgur.com/R7QpvJl.png" alt="連接 ML 後端">
	</p> 		
 
 
### Demo 畫面
最後附上幾張看起來超級酷的 demo 畫面。

<p class="illustration">
<img src="https://i.imgur.com/LW3JD4E.gif" alt="Demo1">
</p> 		
 
<p class="illustration">
<img src="https://i.imgur.com/cPPynxt.gif" alt="Demo1">
</p> 		
 
<p class="illustration">
<img src="https://i.imgur.com/MV1naEb.gif" alt="Demo1">
</p> 		
 
<p class="illustration">
<img src="https://i.imgur.com/drT44AA.gif" alt="Demo1">
</p> 


 
## 題外話
是說...我真的覺得綠色那隻比較好看欸？你們覺得呢？

 
<p class="illustration">
<img src="https://i.imgur.com/rLSg2Ef.gif" alt="題外話">
</p> 

<br class="big">	

海蒂小姐真的還滿可愛的 XDDD

<p class="illustration">
<img src="https://i.imgur.com/xtFkeBt.png" alt="題外話">
</p> 

## 參考資料 
1. secsilm (2020-11-10)。[试用开源标注平台 Label Studio_Alan Lee](https://blog.csdn.net/u010099080/article/details/104881167)。檢自 CSDN博客 (2021-08-04)。
2. 協同撰寫 (2021-07-28)。[heartexlabs/label-studio](https://github.com/heartexlabs/label-studio)。檢自 GitHub (2021-08-04)。
3. Nikolai Liubimov (2020-01-28)。[Introducing Label Studio, a swiss army knife of data labeling](https://towardsdatascience.com/introducing-label-studio-a-swiss-army-knife-of-data-labeling-140c1be92881)。檢自 Towards Data Science (2021-08-04)。
4. Johnson7788 (2020-12-28)。[安利一个开源的好工具Label Studio, 闭环数据标注和模型训练](https://zhuanlan.zhihu.com/p/339567115)。檢自 知乎 (2021-08-04)。
5. [Components and Architecture](https://labelstud.io/guide/index.html#Components-and-architecture)。檢自 Label Studio Documentation (2022-12-09)。
6. [Label Studio features](https://labelstud.io/guide/label_studio_compare.html)。檢自 Label Studio Documentation (2022-12-09)。


## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-02-16</summary>
  <ul>
    <li>2023-02-16 發布</li>
    <li>2022-12-14 完稿</li>
    <li>2021-08-04 起稿</li>
  </ul>
</details>


<style>
.details-2 summary {
	padding: 5px;
	background-color: #f0f0f0;
}
.details-2 content {
	display: block;
	padding: 5px;
	background-color: #f5f7f7;
}

.details-2 summary::after {
	content: '';
	position: absolute;
	width: 1em; height: 1em;
	margin: .2em 0 0 .5ch;
	background: "^"
	background-size: 100% 100%;
	transition: transform .2s;
}
.details-2:not([open]) summary::after {
	margin-top: .25em;
	transform: rotate(90deg);    
}

.details-2 ::-webkit-details-marker {
	display: none;
}
.details-2 ::-moz-list-bullet {
	font-size: 0;
}
</style>