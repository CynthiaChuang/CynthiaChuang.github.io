---
title: 【Survey】NVIDIA Clara Train SDK 3.0
date: 2021-01-12 23:22
is_modified: false
categories:
- "智慧計算 › 人工智慧"
- "資訊科技 › 開發與輔助工具"
tags:
- AI/ML
- Clara
- NVIDIA
- 醫學影像
- 工具安裝與部署
--- 

真的很想說，**Clara 我真的跟你很不熟阿**，但老大有令，不敢不從，只能硬著頭皮上了。
  
這篇筆記主要是對近期改版的功能做了些整理，如文章標題所說我關注的功能主要集中在 Train SDK 的部分。至於近期指的是？恩...其實就是我上次看過它之後到目前的這段期間 XDDD
<!--more-->

<center> <img src="https://i.imgur.com/XeFIJGt.jpg?2" alt="Clara"></center>

<br><br>

## Overview

我記得上次看到 Clara 好像是 1.0 版吧？那時平台剛發布不久，對於 Clara 的印象就是個<mark>醫療影像平台</mark>，它提供研究者一套醫療影像標注工具進行資料標注，並制定了一套標準結構用來使用影像資料進行 AI 訓練。

不過從近期的改版與報導看來，他們企圖將所有醫療相關的解決方案都整合進 Clara，這是想把 Clara 打造成類似一個**百寶袋**的概念？

<center> <img src="https://i.imgur.com/eFZs2e3.png" alt="哆啦a夢 百寶袋"></center>
<center class="imgtext">一個百寶袋的概念?（圖片來源: <a href="https://www.dora-world.com.tw/character.php" class="imgtext">哆啦A夢台灣官網</a>）</center>

<br>

這次重新造訪 Clara，發現除了原先的醫療影像平台外，還多了兩個平台：
1. **Clara for Medical Imaging**：傳送門 [link1](https://developer.nvidia.com/clara-medical-imaging)、[link2](https://www.nvidia.com/zh-tw/healthcare/clara-imaging/)
2. **Clara for Genomics**：傳送門 [link1](https://developer.nvidia.com/clara-parabricks)、[link2](https://www.nvidia.com/zh-tw/healthcare/clara-parabricks/)
3. **Clara for Smart Hospitals**：傳送門 [link1](https://developer.nvidia.com/clara-guardian)、[link2](https://www.nvidia.com/zh-tw/healthcare/clara-guardian/)

<br>

<center> <img src="https://i.imgur.com/PbZbzYS.png" alt="NVIDIA Clara application framework"></center>
<center class="imgtext">NVIDIA Clara application framework
（圖片來源: <a href="https://developer.nvidia.com/clara" class="imgtext">NVIDIA</a>）</center>

<br>

三個平台中，NVIDIA Clara Imaging 指的是他們的立業之本**醫療影像**，這部分我在本節未才介紹。這邊則先稍微提一下新增 **Genomics** 與 **Smart Hospitals**。

<br>

### Clara for Genomics  

由於基因定序成本不斷下降，從 1,000 萬美元降至 1,000 美元，甚至可望降至 100 美元，其下降速度已經超越摩爾定律（Moore's Law）所預期，但定序後所產生的基因體是個非常龐大的資料來源，需要處理到億級的鹼基對，才能對定序資料進行分析與試驗。這麼龐大資料量處理，使得基因體分析成為定序流程的瓶頸，若想突破這瓶頸，必須找到合適且充裕的算力。

為了吃下這塊大餅，NVIDIA 收購以基因體分析工具（Genome Analysis Toolkit，GATK）為基礎的 Parabricks，它可透過 CUDA 加速基因資料的讀取、序列比對，將相關計算速度提升 <mark>30~50</mark> 倍，以降低分析成本，並導入深度學習進行基因變異檢測。

<center> <img src="https://i.imgur.com/cg8zG9b.png" alt="基因定序流程"></center>
<center class="imgtext">基因定序流程（圖片來源: <a href="https://developer.nvidia.com/clara" class="imgtext">NVIDIA</a>）</center>

<br>

<div class="alert info"> 
<div class="head">補充資料</div>
<a href="https://www.youtube.com/watch?v=r5iWLqguRLk">How to get satrted with NVIDIA Parabricks｜YouTube</a>
</div>

<br>

### Clara for Smart Hospitals 

是說這個項目我月初看到它的時候，分頁名稱還寫著**嵌入式 Clara（NVIDIA Clara AGX Developer Kit）**，不過月中為了寫網誌回去翻資料的時候，分頁名稱已經改成了**智慧醫院（Clara for Smart Hospitals）** 了。不過不管名字換哪個企圖都是一樣的，想在醫院的邊緣設備上部署 AI。

但兩者還是有些微不同，[Clara AGX™](https://developer.nvidia.com/clara-agx-devkit) 是嵌入式人工智慧運算平台，旨在將計算功能帶到了邊緣設備上，適用在需要執行即時 AI 與高級影像、影片或訊號處理的醫療設備，例如：醫療儀器、手術室、病患監護和智慧攝影機...等，可為醫療物聯網（The Internet of Medical Things, IoMT）提供即時、可更新的人工智慧技術。

而智慧醫院則是個整合 **智慧影像分析（IVA）** 與 **對話式人工智慧（Conversational AI）** 的 IoMT 架構，並試圖將其導入醫療照護產業。其中所推出的 Clara Guardian 比較像是一個程式框架，它可以結合智慧感測器與人工智慧，方便醫院環境中的感測器的開發與部屬。

但不論是 Clara AGX 或是 Clara Guardian 都會與 NVIDIA EGX™ 邊緣人工智慧平台結合，方便安全管理大批裝置和協調針對醫療設備或邊緣節點的 AI 應用程式部署。

<center> <img src="https://i.imgur.com/lVlG2Yt.png" alt="智慧醫院"></center>
<center class="imgtext">智慧醫院（圖片來源: <a href="https://developer.nvidia.com/clara" class="imgtext">NVIDIA</a>）</center>

<br>


### Clara for Medical Imaging

<center> <img src="https://i.imgur.com/WIf1Tcv.png" alt="Clara Train SDK Flow"></center>
<center class="imgtext">Clara Train SDK Flow（圖片來源: <a href="https://developer.nvidia.com/blog/deploying-healthcare-ai-workflows-with-the-nvidia-clara-deploy-application-framework/" class="imgtext">NVIDIA Developer Blog</a>）</center>

<br>

最後我們回來看看 Clara 起家立業的醫療影像，這邊先簡單敘述下該項目的工作流程與目的。若在醫療機構中，如果想引入 AI 深度學習演算法，並將其投入應用，大致可分成 3 個步驟：

1. **資料獲取與標注**  
    資料是 AI 學習所不可或缺的養分。因此如何獲取資料，並就這些資料進行所需的標注，是建構整體系統的關鍵第一步。
    
2. **將資料導入選定的模型訓練**  
    有了標注完成的資料後，開發者便須將資料導入開發的模型，從無到有進行訓練；或者選擇在該領域或是其他領域的模型進行 Fine-Tune 或遷移學習（Transfer Learning）。當訓練完成後，使用測試集進行檢驗，若是訓練結果無法在測試集上有良好的表現，可能需要調整參數重新訓練。

3. **模型部屬**  
    若是在測試集中取得了不俗的表現，則可以進一步部署在設備端，運用於真實診療環境中進行推斷。若是 AI 在設備端運行良好，可以在進一步將其部署到其他的邊緣設備上。
    
    
而 Clara for Medical Imaging 所期望的目的則是<mark>簡化</mark>與<mark>標準化</mark>上述三個步驟，讓開發者更專注於醫學本身的研究。

<br><br>

 
## Introduction for Medical Imaging
 
這一章節，讓我們再來細說醫療影像部分，在[上一小節](#Clara-for-Medical-Imaging)所提到的流程，不啻是醫療機構引入深度學習演算法的步驟，更是整個 AI 開發通用的流程。而 NVIDIA 依照這個流程，在近期的更新中將整個平台分成三個部分，分別是： 

1. Clara Train SDK
2. Clara Deploy SDK
3. Clara AGX SDK

<center> <img src="https://i.imgur.com/J35fiMU.png" alt="NVIDIA Clara Imaging"></center>
<center class="imgtext">NVIDIA Clara Imaging（圖片來源: <a href="https://developer.nvidia.com/clara" class="imgtext">NVIDIA</a>）</center>

<br>

### Clara AGX SDK  
我們由右向左來介紹這張架構圖，圖中最右邊的 **Clara AGX SDK** 就是前面在 [Clara for Smart Hospitals](#Clara-for-Smart-Hospitals) 中提到嵌入式人工智慧運算平台，可以進一步將 AI 其部署到其他的邊緣設備上，不過因為前面說過了就略過不談了。在月中回來後只有在這邊可以看到嵌入式 AI 的項目。

<br>

### Clara Deploy SDK

在架構圖中間區塊的是 Clara Deploy SDK。簡單闡述這個模組的目的，就是將訓練後產生的最佳模型，使用 Clara Deploy SDK 部署到設備端，為下一位病患掃描結果提供進階的服務，可以用來整合臨床與研究工作流程。
 
而從工程面來說，Clara Deploy SDK 則是提供了，一個能整合到現有醫院架構中的 work stream 所需的框架與工具。

<center> <img src="https://i.imgur.com/BJMFLiV.png" alt="The Clara Deploy SDK architecture."></center>
<center class="imgtext">The Clara Deploy SDK architecture.（圖片來源: <a href="https://developer.nvidia.com/blog/deploying-healthcare-ai-workflows-with-the-nvidia-clara-deploy-application-framework/" class="imgtext">NVIDIA Developer Blog</a>）</center>

<br>

<div class="alert info"> 
<div class="head">補充資料</div>
如果對 Clara Deploy SDK 內容與近期個更新有興趣可以看看<br>  
1. <a href="http://www.gpus.cn/gpus_list_page_techno_support_content?id=29">使用Clara Deploy SDK创建、管理和部署AI增强的临床工作流程</a><br>  
2. <a href="https://developer.nvidia.com/blog/deploying-healthcare-ai-workflows-with-the-nvidia-clara-deploy-application-framework/">Deploying Healthcare AI Workflows with the NVIDIA Clara Deploy Application Framework</a><br>  
</div>

<br>

### Clara Train SDK

最後則是我較為側重的 Clara Train SDK，且在前面所提到的引入深度學習演算法的步驟中，它也占了了不輕的比例。它的功能主要是著重在<mark>資料的標注</mark>與<mark>模型訓練</mark>。

在資料標注的方面，由於醫療的專業需求，導致標注人員的專業門檻較高，且因為工作的精細要求，一組資料標注依精細度不同，需耗費 20-40 分鐘甚至 1-2 小時不等，導致標注成本非常昂貴。（找醫生來標能不貴嗎？:money_with_wings: ）

有鑑於此， NVIDIA 提供了由預先訓練模型、用戶端 API 及伺服器所組成的 <mark>AI 輔助標注機制（AI Assisted Annotation， AIAA）</mark>，開發者可以直接藉由此工具對醫療影像進行標注，並降低標注所需的時間。

而在訓練方面，NVIDIA 定義了一種名為 <mark>醫學模型檔案（Medical Model Archive， MMAR）</mark> 標準結構。它可用於安排在模型開發生命週期中產生的所有工件，以協助開發者快速利用現有的演算法架構與預訓練模型進行遷移訓練或 Fine-Tune。

在近期上線的功能中，多了不少有趣的新功能，不僅能加速標記速度，更引入了聯盟學習（Federated Learning）與 AutoML，降低對資料的需求與參數/模型調整的負荷，下面的章節來看看這些多出來的新功能。

<center> <img src="https://i.imgur.com/excybq5.png" alt="Clara Train SDK"></center>
<center class="imgtext">Clara Train SDK（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>

<br><br>


## Key Features for AIAA

Clara Train SDK 為了因應手動標注資料的缺點：<mark>速度慢</mark>、<mark>繁瑣</mark>與<mark>成本高</mark>，而推出了 AI assisted Annotation（AIAA），它是由三個部分所組成：
1. Annotation Client
2. Annotation Server
3. Pre-trained / BYOM（Bring Your Own Model）

<center> <img src="https://i.imgur.com/WIPLz1J.png" alt="AIAA"></center>
<center class="imgtext">AIAA（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>

<br>

Clara Train SDK 針對常見的的醫療影像工具，如：MITK、3D Slicer，有開發支援的 Plug-in。此外，還提供了一個 AIAA Client API，能與其他醫療影像顯示進行整合。這些 AIAA Client API 已具備 2D/3D Annotation，並能支援 Deep Extreme cut in 3D（DEXTR3D）的功能，讓使用者可藉由僅標注6個點（每個軸上 2 個），再透過 restful api 方式送至 Annotation Server 生成切割結果。
 
<center> <img src="https://i.imgur.com/xnr20ZN.png" alt="DEXTR3D"></center>
<center class="imgtext">DEXTR3D（圖片來源: <a href="https://docs.nvidia.com/clara/tlt-mi/clara-train-sdk-v3.0/aiaa/overview.html" class="imgtext">Clara Train Application Framework v3.0 documentation</a>）</center>

而 Annotation Server 在收到客戶端的標注結果，會藉由預訓練的模型，或是自定義的模型，進行輔助標記的功能。


<br>

#### DeepGrow

> [Interactive segmentation of medical images through fully convolutional neural networks](https://arxiv.org/pdf/1903.08205.pdf)  
> Tomas Sakinis, Fausto Milletari, Holger Roth, Panagiotis Korfiatis, Petro Kostandy, Kenneth Philbrick, Zeynettin
Akkus, Ziyue Xu, Daguang Xu, Bradley J. Erickson
<br>

除了 DEXTR3D Model 外，本次 AIAA 新增了 DeepGrow，它可透過 CNN 接收標注前後景的方式，生成切割結果，以協助器官標注。
  
<center> <img src="https://i.imgur.com/R43JgN2.png" alt="DeepGrow"></center>
<center class="imgtext">DeepGrow（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>

<br>

感覺有點像是 Photoshop 的魔術棒選取與反向選取的功能。如果用魔術棒選取得結果太大，就用反向選取去扣掉一些範圍；反之若不夠大，就在向外繼續點取，直到框住所要選取的目標為止。

在該篇論文中，可以看到些標注結果。在每一直行為一次標記的操作，左邊四行是對已知器官的標注、右邊兩行則是對未知器官標注；又綠色箭號是標注前景、黃色標注背景。

可以看到它透過前景標注來擴大選取範圍，或使用背景標注來去掉部分選取。不過在截圖中，它的背景標注的操作的範圍都有點小，可能要把圖放大一點才能看到它去掉的部分。
  
<center> <img src="https://i.imgur.com/RkLJnEh.png" alt="Example of interactive segmentation"></center>
<center class="imgtext">Example of interactive segmentation（圖片來源: <a href="https://arxiv.org/pdf/1903.08205.pdf" class="imgtext">論文</a>）</center>

<br> 
 
另外貼個器官名稱圖對照一下，不過似乎不是同一個切面，因為胃不見了 XDDDDD
<center> <img src="https://i.imgur.com/AqzF6Ct.jpg" alt="腹部橫切面-肝臟位置圖"></center>
<center class="imgtext">腹部橫切面-肝臟位置圖（圖片來源: <a href="https://802.mnd.gov.tw/ListP0003102.ShowItemListState.do?StateEvent=InitEvent&QueryRecord.ArticleId=2006-02-09%2012:47:42" class="imgtext">國軍高雄總醫院-院刊內容</a>）</center>

<br>

使用這個模型的好處在於可以標注未曾見過器官。以 3D Slicer 為例，在進行標注時，必須先選擇相對應的器官輔助模型，才有辦法協助切割；一旦今天所要標注的器官沒有輔助模型，是無法進行有效的協助的。不過如果有了~~魔術棒~~ DeepGrow，就可標注沒有模型輔助的器官了。

在 3D Slicer 操作效果如下：
<center> <img src="https://i.imgur.com/WuntBl0.png" alt="DeepGrow 在 3D Slicer 操作效果"></center>
<center class="imgtext">DeepGrow 在 3D Slicer 操作效果（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>

<br>

官網中，還貼了張效能的示意圖，號稱可以提升 10x 的標注速度：

<center> <img src="https://i.imgur.com/UXZLQgk.png" alt="標注速度的提升"></center>
<center class="imgtext">標注速度的提升（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>

<br>

是說每次說到效能提升，我都會想到這兩張圖 XDDD

<center> <img src="https://i.imgur.com/mwWwsQB.png" alt="效能提昇"></center>
<center class="imgtext">（圖片來源: <a href="https://www.facebook.com/photo?fbid=10209456134591075&set=pcb.10209456134991085" class="imgtext">Facebook</a>）</center>


<br><br>

##  Key Features for Training 

在 Train SDK 中主要負責訓練的是 Clara Training Framework。在近期的更新中它也多了不少~~不~~有趣的功能。不過在細看這些功能前，還是先快速回復下 MMAR 結構的記憶，這會有助於後續的敘述。


### Medical Model Archive， MMAR 

MMAR 是 Clara Train SDK 用來安排在開發生命週期中工作的資料結構，資料結構如下：

```
./Project
├── commands
├── config
├── docs
├── eval
├── models
└── resources
```

1. `commands`：這會包含所需要用到的 scripts，通常會有 training、 多 GPU training、 validation、 inference和 TensorRT 轉換...等。    
2. `config`：包含每次訓練所需的training、 validation、 AIAA 部署和環境...等配置 JSON 文件。
3. `docs`：顧名思義，放文件的地方。    
4.  `eval`：eval 預設結果輸出的目錄。    
5.  `models`：tensorflow checkpoint 儲存的目錄。
  
<br>
  
不過，比較重要的就三個目錄`commands`、`config` 和 `models`  
<center> <img src="https://i.imgur.com/yGTGYiE.png" alt="MMAR 目錄格式"></center>

<br>

而在這些文件中最核心的檔案莫過於 `config` 下 `config_train.json`。  
在這個 JSON 中，包含定義神經網路所需的所有參數、網路模型的搭建、激勵函數、優化器、圖像的前後處理功能...等。

<center> <img src="https://i.imgur.com/50ebmEr.png" alt="DeepGrow 在 3D Slicer 操作效果"></center>
<center class="imgtext">DeepGrow 在 3D Slicer 操作效果（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>

<br>

這些模型、優化器、圖像增強...等功能，除了可以使用 NVIDIA 所提供的模組外，也可以繼承指定的父類別搭建自己的網路。

這樣的好處不僅可以將開發環境從容器（container）中抽離出來，連開發需要的文件、訓練時所產生的資料以及環境設定都被獨立出來，這樣在抽換積木也會方便很多，需要時藉由 MMAR 掛入容器中，即可進行模型的訓練。

不過麻煩的地方，在於必須將現有的模型，依照 MMAR 所定義的網路、激勵函數、優化器、前後處理...等架構，拆解到對應的 API 中，不僅需要了解 Clara Train SDK 設定檔設定方式與 API 使用方法外，目前 Framework 僅支援 TensorFlow，並不支援如：Keras、 Pytorch...等 Framework，因此必須熟悉 TensorFlow 與 SDK 和 API 的使用方法，才有能力進行轉移。




<br>

### Features Overview

對 MMAR 與 `config_train.json` 格式有大致了解後，再回來看看近期的主要更新項目：
1. <mark>AutoML</mark>
2. Use negative loss value as the key model validation metric
3. Automatic Mixed Precision
4. Determinism
5. <mark>Smart Cache</mark>
6. <mark>Federated learning</mark>
7. Loss functions and models
8. Novograd

其中較為重要的有<mark>AutoML</mark>、 <mark>Smart Cache</mark> 與 <mark>Federated learning</mark>，這接下來三節會在說明。而其餘的幾個項目，我們先在這邊快速帶過：
1. **Use negative loss value as the key model validation metric**  
    添加 NegLoss  作為 minimal loss 的 metric value。
2. **Automatic Mixed Precision**  
    自動混合精度，可以使用半精度進行訓練，但保持以單精度實現的網路精確度。可以減少記憶體的需求，並提升了 3 倍的訓練速度（某些模型）。
3. **Determinism**  
    感覺這個功能是為了 debug 用的， deterministic 一詞指的是<mark>輸入相同的輸入會得到相同結果</mark>，兩者之間是不變的。Determinism 用在 training 是希望得到相同的模型權重和相同的inference results。
4. **Loss functions and models ＆ Novograd**  
    就新增了些 losses、 models and optimizer


<br>

### AutoML

大概自從 Google 發表 Cloud AutoML 後，AutoML 算是進入了百家爭鳴時代，現在連 NVDIA 都要來湊熱鬧。

一般來說，網路的訓練會包含幾個重要元件，如資料的前後處理、神經網路主體以及損失函數...等。一個最佳的模型是由這些元件中選出最合適的積木堆砌而成，在加上挑選適合參數來優化這些設置。

<center> <img src="https://i.imgur.com/rvvdPpG.png" alt="Components of deep learning medical imaging pipeline"></center>
<center class="imgtext">Components of deep learning medical imaging pipeline（圖片來源: <a href="https://developer.nvidia.com/blog/powering-automl-enabled-ai-model-training-with-clara-train/" class="imgtext">NVIDIA Developer Blog</a>）</center>

<br>

而 AutoML 的目的就在於：<mark>如何在最少干預人工下尋找最合適的架構與最優參數</mark>。主流的 AutoML 架構中，會有一個 controller 管理整個搜索過程，它會用不同的參數來啟動訓練流程，並收集歷史資料，為下一輪訓練提供進一步的參數建議。在 controller 實做可以透過 RL、進化演算法或是貪婪搜索算法...等來實現，端看實際應用來選擇。

<center> <img src="https://i.imgur.com/bhihV9V.jpg" alt="AutoML controller and network training flow."></center>
<center class="imgtext">AutoML controller and network training flow.（圖片來源: <a href="https://developer.nvidia.com/blog/powering-automl-enabled-ai-model-training-with-clara-train/" class="imgtext">NVIDIA Developer Blog</a>）</center>

<br>

而 Training Framework 所提供的 AutoML 也是遵循這樣的架構，詳細的步驟如下：

1. **Generate search space definition， 生成搜索空間的定義**  
    Executor 會根據 config 的設置，定義要搜索的空間範圍。
2. **Initialize the controller， 初始化 Controller**  
    Controller 根據搜索空間的定義制搜索策略。
3. **Generate initial recommendations， 產生初步建議集**   
    根據搜索策略 Controller 會給定一或多個參數的初始設置，交由 Scheduler 進行嘗試。 
4. **Schedule jobs， 安排工作**  
    Scheduler 會根據建議集與可用資源，為每組初始設置（Job）安排一個 Workers 平行處理。
5. **Execute jobs， 執行工作**  
    每個 Job 都會被分配給一個 Workers 執行， Workers 會調用 Executor 執行該項作業，最終會產生一個分數，用來衡量此次設置的指標。 
6. **Refine recommendations， 調整建議集**  
    當 Job 被執行完後，會得到相對應的分數，Controller 會根據這個分數調整並產生新的建議集，並重複步驟 4 到 6，直到 Controller 不再提供任何建議，或是達到 EarlyStop 門檻。

另外圖中的 Engine 是負責元件間的互相溝通，Controller 與 Executor 之間的溝通就必須藉由 Engine 來進行。
 
<center> <img src="https://i.imgur.com/NpQNNSc.png" alt="Clara Train v3.0 AutoML workflow"></center>
<center class="imgtext">Clara Train v3.0 AutoML workflow（圖片來源: <a href="https://developer.nvidia.com/blog/powering-automl-enabled-ai-model-training-with-clara-train/" class="imgtext">NVIDIA Developer Blog</a>）</center>

<br>

在搜索空間定義中，它可針對以下方面進行參數的搜索：
1. 網路架構選擇（Network architecture selection）
2. 網路參數設置（Network parameter settings）
3. 學習率設置（Learning rate settings）
4. 資料轉換設定（Transform settings）

並接受給定搜索參數的列舉、參數的搜索範圍或 components 類型生成參數設置建議集。

以列舉的方式為例，可以透過列舉的方式來測試參數或是 Loss and optimizer
<center> <img src="https://i.imgur.com/qB4dtoC.png" alt="Hyper Parameter Enum Search"></center>
<center class="imgtext">左：Network Parameter Search、右：Loss and optimizer Search（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>


<br>

或者使用 [RL 對浮點數參數進行搜尋](https://arxiv.org/abs/2006.05847v1)

<center> <img src="https://i.imgur.com/U6JstCN.png" alt="Using Auto ML for float parameter using RL"></center>
<center class="imgtext">Using Auto ML for float parameter using RL（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">NVIDIA</a>）</center>


<br><br> 

針對 Clara Training Framework 所提出的 RL 自動搜索方法。該方法適合用於超參數調整，並設置機率以挑選資料增強的方法。該方法在 3D medical image segmentation 進行驗證，搜索後可以有效提升模型基礎性能，並且可與其他手動調整參數的 SOTA 切割演算法相比擬。

比較使用的資料集是 [Medical Segmentation Decathlon](http://medicaldecathlon.com/) 中三個任務所得的 Dice Score (DSC) ，其中：
- Task06：Lung 肺
- Task07：Pancreas 胰腺
- Task09：Spleen 脾臟

<center> <img src="https://i.imgur.com/BUSxRMF.png" alt="A comparison of validation accuracy"></center>
<center class="imgtext">A comparison of validation accuracy（圖片來源: <a href="https://developer.nvidia.com/blog/powering-automl-enabled-ai-model-training-with-clara-train/" class="imgtext">NVIDIA Developer Blog</a>）</center>

<br>


### Federated learning

對於深度學習來說，資料當然是多多益善。但近年來各國越來越重視個人隱私資料保護議題，因而紛紛制定了嚴格的隱私條款，更遑論病患資料這種極度隱私的資訊。

因此 Google 在 2016 年提出了聯盟式學習（Federated Learning），其目的是為了在保護用戶隱私下進行機器學習模型訓練。在
聯盟式學習中主推資料不需要離開設備，各個節點可在各自的設備訓練模型，並且通過特定的加密的機制在伺服器上進行參數的整合後，在取回更新各自的模型。

看到這裡有種分散式訓練的即視感，像是多節點訓練中，每個人讀取資料集中不同部分，i.e. 資料集中的子資料集。但話說回來資料不離開設備就算是遵守了隱私條款嗎？這些訓練所得權重也算是資料的一部分吧？算了，先別戰這個問題，我們先假設這滿足了隱私的需求吧。

<br>

在聯盟式學習可以根據資料型態有可在細分成：
1. **橫向聯盟式學習（horizontal federated learning）**  
    這是用適用於特徵重疊性高且樣本重疊少時的情境，例如這邊所談及的醫療影像，在影像中他們有著相似的特中，但卻是來自不同的病患，網路架構相較之下較為簡單。
    
2. **縱向聯盟式學習（vertical federated learning）**  
    這適用於樣本重疊多且特徵重疊少的情境，舉例來說像是同一社區不同診別的診所，他們病患重疊性比較高，但資料中的特徵卻不相同。
 
但在於縱向聯盟中有個大麻煩，就是如果參與者如果越多，會使的它的流程架構就會越複雜，不僅難以執行也不適合推廣。因此在實務上比較常見的會是<mark>橫向聯盟</mark>，NVIDIA 所提供的也是這個。

<center> <img src="https://i.imgur.com/N1LmQQD.png" alt="horizontal federated learning"></center>
<center class="imgtext">horizontal federated learning（圖片來源: <a href="https://developer.nvidia.com/blog/federated-learning-clara/" class="imgtext">NVIDIA Developer Blog</a>）</center>


<br>

找了張有標注詳細步驟的架構圖來說明流程，在聯盟式學習基本上就是通過不斷迭代下列步驟來訓練模型：
1. 各節點使用各自訓練資料改進目前模型後將更新參數加密發送到伺服器
2. 伺服器將接收到的參數與其他節點的更新整合參數
3. 發送模型更新參數到各節點
4. 各節點更新用戶端模型

<center> <img src="https://i.imgur.com/VuN0PlG.png" alt="Federated Learning 步驟"></center>
<center class="imgtext">Federated Learning 步驟（圖片來源: <a href="https://secbuzzer.co/post/70" class="imgtext">secbuzzer</a>）</center>

<br>
 
而在 NVIDIA 的聯盟式學習中，每個節點可以對自己的權重有進一步的隱私控制，它可以決定要發送多少百分比的模型權重到伺服器。
 
一旦伺服器從節點接收到最新一輪的模型，伺服器就可以依照自己的演算法定如何合併這些參數，可以是各節點權重的平均值，也可以根據歷史貢獻的來調整權重。



<center> <img src="https://i.imgur.com/yRGDvYX.png" alt="Federated Learning 步驟"></center>
<center class="imgtext">Federated Learning 步驟（圖片來源: <a href="https://docs.nvidia.com/clara/tlt-mi/clara-train-sdk-v3.0/nvmidl/additional_features/federated_learning.html" class="imgtext"> documentationg</a>）</center>

<br>

在運行聯盟式學習模型訓練時，必須先啟動伺服器訓練服務。伺服器 session 會控制每次訓練所需最少與最多的節點數量。若是一個節點要加入訓練則必須先發送請求。伺服器會檢查節點的憑證，並執行身分驗證過程以驗證節點。若驗證成功，則伺服器將會發送 token 到節點；否則，則拒絕。
 
完成驗證後，節點會發送另一個請求從伺服器取得目前訓練模型，以開始訓練。節點決定在每個聯盟式學習的 step 中，要進行多少 epoch 訓練，當完成該輪訓練後，就把訓練後的參數送往伺服器。當伺服器從所有參與的節點接收到所有更新的參數後，會根據權重演算法執行模型聚合，並得到更新後的整體訓練模型。聯盟式學習會達到 config 中配置 `num_rounds` 後才停止訓練。  
 
<br>

這張圖把剛剛說到節點的訓練過中詳細標了出來。

<center> <img src="https://i.imgur.com/lVev5PF.png" alt="Federated learning client-side workflow"></center>
<center class="imgtext">Federated learning client-side workflow（圖片來源: <a href="https://docs.nvidia.com/clara/tlt-mi/clara-train-sdk-v3.0/nvmidl/additional_features/federated_learning.html" class="imgtext">Documentationg</a>）</center>
 
<br>

關於聯盟式學習模型配置，有使用另外的 `config_fed_server.json`/ `config_fed_client.json` 來定義，不是定義在 `config_train.json` 裡面。


<center> <img src="https://i.imgur.com/0RtdwBO.png" alt="Federated learning client-side workflow"></center>
<center class="imgtext">Federated learning client-side workflow（圖片來源: <a href="https://docs.nvidia.com/clara/tlt-mi/clara-train-sdk-v3.0/nvmidl/additional_features/federated_learning.html" class="imgtext">Documentationg</a>）</center>
 
<br>

### Batch By Transform and Smart Cache


這部分主要是涉及效能的優化。在一般訓練機器學習模型時，主要會使用到的資源有三種，分別是：
1. **IO**：用在資料讀取與載入的部分。  
2. **CPU**：用在資料前處理的部分。  
3. **GPU**：用在資料處理，也就是訓練的部分。

就處理速度而言 IO 最慢、GPU 最快。但 GPU 雖具備強大的並行能力，可以加快資料處理的過程，可是它的記憶體有限，所以資料必須從外部導入，這也讓 IO 有了可趁之機得以拖慢整體訓練速度。

此外，在模型的訓練過程中，其實是對同一個資料集反覆讀取。且在訓練過程的每一步中，都必須從資料集讀入一組圖片，然通過一系列的 Data Transformation，才會將其放入訓練流程由 GPU 進行加速。而這些 Transformation 的在 CPU 進行，這也使的它成為訓練速度的另一個瓶頸，降低了 GPU 使用率。

<center> <img src="https://i.imgur.com/C5L2RYk.png" alt="GPU 使用率"></center>
<center class="imgtext">一般訓練過程中，GPU 使用率會因為等待資料讀取而降低（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">Nvidia</a>）</center>

<br>

因此 Train SDK 為了改進 GPU 使用率，提供了兩種方式，希望能將資料保留在 GPU 記憶體中重複使用，以減少了對 IO 和 CPU 資源的需求。
1. Batch By Transform
2. Smart Cache

<br>

#### Batch By Transform

一般在進行 crop 時，它會從硬碟讀取一張圖片進入記憶體後，從中裁取需要的範圍後，就將原圖丟棄。但為了減少從 IO 讀入圖片的次數，這邊引入 Batch By Transform，能夠從同一張圖片，進行多次的裁取。

<center> <img src="https://i.imgur.com/u5tAnwB.png" alt="Batch By Transform"></center>
<center class="imgtext">Batch By Transform（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">Nvidia</a>）</center>

<br>

在程式碼設定的部分，需針對兩個部分進行設定：
1. **ImagePipeline**   
    將 `batched_by_transforms` 開啟，並把 `output_batch_size` 設為 0
    ``` 
    "output_batch_size": 0,
    "batched_by_transforms": true,
    ```

2. **Crop**   
    選擇一個 batching transforms：CropByPosNegRatio、 FastCropByPosNegRatio, CropByPosNegRatioLabelOnly，並設定 `batches_to_gen_at_once`，而這個必須大於 1 才有意義。 
    ```
    {
        "name": "FastCropByPosNegRatio",
        "args": {
        "size": [96,96,96],
        "fields": "image",
        "label_field": "label",
        "pos": 1,"neg": 1,
        "batch_size": 12,
        "batches_to_gen_at_once": 48
    }
    ```

<center> <img src="https://i.imgur.com/TMGy9FP.png?1" alt="Batch By Transform"></center>
<center class="imgtext">Batch By Transform（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">Nvidia</a>）</center>

<br>

不過可以看到它把 `output_batch_size` 設為 0，輸出的數量全由 `batches_to_gen_at_once` 進行控制了，可想而知一個 batch 所產生來的資料會是同一個病人裁減出來的結果。


<br>

#### Smart Cache

若 Batch By Transform 想減少的是 IO 的載入次數，那麼 Smart Cache 想減少則是 **Transformation** 的執行次數，以降低對 IO 和 CPU 資源的需求。

為了減少轉換所造成的開銷，Smart Cache 會將轉換的結果存入記憶體中，提供下一個 step 的使用。不過需要注意的是，有些轉換是具備隨機效果，這部分的轉換不應該存入記憶體中，以免降低訓練效果；唯有相同的輸入恆等於相同結果輸出的轉換步驟，才適合使用放入記憶體。

最有效率的存放方式是存入是由第一個轉換開始算起的 <mark>the longest transformation chain</mark>。如此一來在下個 step 中，這些前期的會產生相同結果轉換會被繞過，直接從記憶體讀取，以降低對 IO 和 CPU 的使用。
 
<center> <img src="https://i.imgur.com/ulwBNXx.png" alt="Smart Cache"></center>
<center class="imgtext">Smart Cache（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">Nvidia</a>）</center>

<br>

但實際上訓練所使用的訓練集是非常的龐大，根本不可能將所有圖片的轉換結果，因此我們必須決定要<mark>保留多少圖片</mark>在記憶體內，也就是在實際上記憶體中只保留一個完整資料集的子集，而這個子集會投入此次的 epoch 訓練，這可確保訓練所需的資料隨時可用，從而使 GPU 資源保持繁忙。但若是保留的圖片太少，不僅訓練速度仍受到 IO 速度的影響，訓練效果也會受到影響。

為了能夠遍歷訓練集中的所有圖片，會在制定一個替換比例，逐步替換記憶體中的內容。不過須注意的是，再將記憶體的內容在送入 GPU 之前，仍必須先經過 non-deterministic 轉換序列，將剩下的轉換執行完畢。因此，在訓練進行中，會有另一個 thread 為替換項目進行剩下轉換。一旦一個 epoch 完成，Smart Cache 就會用轉換完成的替換項目替換相同數量的項目。

由於這樣的訓練方式是遍歷記憶體中的所有資料而不是完整資料集，因此在訓練時應該增加 epoch 進行補償。此外，這兩個數字的設置可能會很棘手：
1. 保留數若設置太高，則 OS 可能會使用硬碟上的虛擬記憶體來進行交換，導致速度大大降低；但若太低，則會使得訓練速度仍受到 IO 速度的影響。
2. 而替換比例若設置太高，可能在一個 epoch 完成前來不及完成 non-deterministic 轉換序列；但若太低，替換速率緩慢，可能需要更多的 epoch 數已進行收斂。


<br>

要實現 Smart Cache，只需使用 ImagePipelineWithCache 的子類，並：
1. 關閉 `batched_by_transforms`
2. 設置 `num_cache_objects` 與 `replace_percent`  
    設置 `num_cache_objects` 應該被 `output_batch_size` 整除，否則最後一個 batch 會變小並降低效率。


<center> <img src="https://i.imgur.com/yW2VBcO.jpg" alt="Smart Cache Code"></center>
<center class="imgtext">Smart Cache Code（圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">Nvidia</a>）</center>


<br>

#### Batch By Transform + Smart Cache

Batch By Transform 和 Smart Cache 也可同時開啟，將時間進一步的。

在 V2 版本中，可透過 `batched_by_transforms`  的開取與否，來決定兩者是否共用。但這樣的設置方式有些問題：
1. 整個 batch 都是同一個病人的圖片。
2. 且一般來說，一張圖片上的病徵可能不會超過四處，從同一張圖片擷取這麼多的意義可能不大。

<center> <img src="https://i.imgur.com/huolGlN.png" alt="BT + Smart Cache V2"></center>
<center class="imgtext">BT + Smart Cache V2圖片來源: <a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">Nvidia</a>）</center>

<br>

因次在 V3 版本中做了些改進，能夠在同一 batch 中從不同病人身上擷取圖片，能使每次學習資料更為的多樣化，並加速擬合。

<center> <img src="https://i.imgur.com/YkYjKsE.png" alt="BT + Smart Cache V3"></center>

<br>

若要啟動 V3 版本的 BT + Smart Cache，請將 `batched_by_transforms` 關閉，並透過 `BatchDims` 來將不同病人所產生資料進行合併。

<center> <img src="https://i.imgur.com/jWZhBrS.png" alt="BT + Smart Cache V3"></center>


<br>

#### 效果

最後來看看開了效能優化的結果。先來看看 <mark>GPU 使用率</mark>，下圖中各線條所對應的優化調整分別是：
1. 黑色：BT+cache
2. 綠色：cache
3. 紅色：BT
4. 藍色：base

有點太五彩繽紛了 XDDDD，我們單看黑線與藍線就好。可以看到黑色線段落在 GPU 低谷的次數比藍色少很多，表示同時使用 Batch By Transform + Smart Cache 確實能有效提高 GPU 使用率。    
<center> <img src="https://i.imgur.com/za1YD85.png" alt=" GPU 使用率"></center>

<br>

而在運算速度方面，這邊在不同的設置上，使用了同一個脾臟的資料集（[Medical Segmentation Decathlon](http://medicaldecathlon.com/) 中的  Task09_Spleen.tar）進行訓練，依照圖表最高的效率可以提昇 54x。

不過我懷疑實務上不會這高，因為那個 replace 是 0，其實不受保留數與替換數設置的影響。而且拿單 GPU 跟 4-GPU 這樣比，這也太...
<center> <img src="https://i.imgur.com/xpULeE9.png" alt="Spleen Segentation Performance Comparision"></center>
<center class="imgtext">Spleen Segentation Performance Comparision<a href="https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started" class="imgtext">Nvidia</a>）</center>

<br> 

Dice 是目標檢測中，比較常用到的衡量指標，範圍從 0 到 1 ，越接近 1 表示模型性能越好。另一個常見的指標是 IoU。是說，我來開一篇著兩個指標的說明好了。



<br><br> 


## 參考資料 
1. 赵泓维 (2020-04-03)。[NVIDIA Clara 能让医疗AI开发变得有多容易？](https://vcbeat.top/OGIxZmRiMzM0NzhmYzZmN2NiYzNiNWEwYzYyZThhNTA=) 。檢自 动脉网 (2020-08-21)。
2. 伊喬伶 (2015-01-17)。[淺談2017至2024年全球次世代定序市場](https://yourgene.pixnet.net/blog/post/118072452-%E6%B7%BA%E8%AB%872017%E8%87%B32024%E5%B9%B4%E5%85%A8%E7%90%83%E6%AC%A1%E4%B8%96%E4%BB%A3%E5%AE%9A%E5%BA%8F%E5%B8%82%E5%A0%B4) 。檢自 有勁的基因資訊｜痞客邦 (2020-08-21)。
3. [NVIDIA Clara Guardian](https://www.nvidia.com/zh-tw/healthcare/clara-guardian/) 。檢自 NVIDIA Developer (2020-08-21)。
4. Rahul Choudhury, Brad Genereaux and Risto Haukioja (2020-04-07)。[Deploying Healthcare AI Workflows with the NVIDIA Clara Deploy Application Framework](https://developer.nvidia.com/blog/deploying-healthcare-ai-workflows-with-the-nvidia-clara-deploy-application-framework/) 。檢自 NVIDIA Developer (2020-08-21)。
5. 技术部 (2019-04-30)。[使用Clara Deploy SDK创建、管理和部署AI增强的临床工作流程](http://www.gpus.cn/gpus_list_page_techno_support_content?id=29)。檢自 GPUS - 吉浦迅科技有限公司 (2020-08-21)。
6. [Intro to Clara Train SDK](https://ngc.nvidia.com/catalog/resources/nvidia:med:clara:getting_started)。檢自 NVIDIA NGC (2020-08-21)。 
7. NVIDIA。[Smart Cache](https://docs.nvidia.com/clara/tlt-mi/clara-train-sdk-v3.0/nvmidl/additional_features/smart_cache.html#how-does-it-work) 。檢自 Clara Train Application Framework v3.0 documentation (2020-08-21)。
8. Yan Cheng, Dong Yang, Holger Roth and Daguang Xu (2020-04-15)。[Powering AutoML-enabled AI Model Training with Clara Train](https://developer.nvidia.com/blog/powering-automl-enabled-ai-model-training-with-clara-train/) 。檢自 NVIDIA Developer Blog (2020-08-21)。
9. By Yuhong Wen, Wenqi Li, Holger Roth and Prerna Dogra (2019-12-01)。 [Federated Learning powered by NVIDIA Clara](https://developer.nvidia.com/blog/federated-learning-clara/) 。檢自 NVIDIA Developer Blog (2020-08-21)。
10. 黃威人 (2019-07-17)。 [【概念篇】Federated Learning：無需集中訓練資料的機器學習](https://secbuzzer.co/post/70)。檢自 secbuzzer (2020-08-21)。
11. Sherry Su (2019-02-04)。 [聯盟式學習 (Federated Learning). 突破隱私權的重圍](https://medium.com/sherry-ai/%E8%81%AF%E7%9B%9F%E5%BC%8F%E5%AD%B8%E7%BF%92-federated-learning-b4cc5af7a9c0)。檢自 Sherry.AI｜Medium (2020-08-21)。
12. nzsfw (2019-10-18)。 [怎么理解图像识别里的dice系数？](http://sofasofa.io/forum_main_post.php?postid=1005897)。檢自 SofaSofa (2020-08-21)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-11-12</summary>
  <ul>
    <li>2021-01-12 發布</li>
    <li>2020-08-24 完稿</li>
    <li>2020-08-20 起稿</li>
  </ul>
</details>
