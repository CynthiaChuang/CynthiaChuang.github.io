---
title: 【Survey】 NVIDIA Clara Deploy SDK
date: 2021-01-18 22:50
is_modified: false
categories:
- 開發與輔助工具
tags:
- Clara
- NVIDIA
- Medical Imaging
--- 

繼上次的 [NVIDIA Clara Train SDK 3.0](/NVIDIA-Clara-Train-SDK-3) 的 Survey 工作後，這次來看看 Clara 組成的另外一個部份 **Deploy SDK**。

<!--more-->
<center> <img src="https://i.imgur.com/XeFIJGt.jpg?2" alt="Clara"></center>

<br><br>

## Overview

快速回顧一下 NVIDIA Clara 的組成，它是由三個面向不同應用的平台所組成，分別是：[**醫療影像**](https://developer.nvidia.com/clara-medical-imaging)、[**基因變異檢測**](https://developer.nvidia.com/clara-parabricks)、[**智慧醫院**](https://developer.nvidia.com/clara-guardian) 。不過一般而言，說到 Clara 通常是代指醫療影像平台。而在醫療影像平台中依照使用的流程，又可以被分成三個部分： 
1. Clara Train SDK
2. Clara Deploy SDK
3. Clara AGX SDK

<center> <img src="https://i.imgur.com/J35fiMU.png" alt="NVIDIA Clara Imaging"></center>
<center class="imgtext">NVIDIA Clara Imaging（圖片來源: <a href="https://developer.nvidia.com/clara" class="imgtext">NVIDIA</a>）</center>

<br>

我們上次有對 [Train SDK](/NVIDIA-Clara-Train-SDK-3) 進行 servery，而這次我們來看看 Clara 組成的另外一個部份 **Deploy SDK**。

近年來，智慧醫院、智慧醫療...等名詞的興起與普遍，象徵著醫療機構逐漸開始運用資訊及通訊科技，並將 AIoT 逐漸導入醫院。在這其中數千種 AI 模型將會被融合進醫院的工作流程中。

但在將這些 AI 應用程式帶入醫院勢必會遇到些挑戰，一是如何利用有限的資料集並在保護病患隱私的情況下，訓練出有效、可擴展的模型；且，即使訓練出有效的深度學習演算法與模型，如何將其部署到臨床工作流程中的集成與可擴展性，也是另一個會遇到的困難。

在此， Clara Deploy SDK 提供了集成到現有醫院架構與流程中所需的框架與工具。

<br>

## Platform

認真來說，Clara Deploy SDK 是一個基於容器（container）開發和部署框架，可以算是一個由容器集合而成的生態系統。SDK 底層使用 Kubernetes 來提供基礎結構並管理各個階段的容器。

目前 Deploy SDK 將平台所提供的各項服務與元件透過 Helm 來統一打包成 chart 的集合，方便使用者快速部署到自己的 Kubernetes 上。整個生態系統可以在不同的雲端服務平台或具備 GPU 的本地端機台上運行。


<center> <img src="https://i.imgur.com/LgA32Ff.png" alt="NVIDIA Clara Deploy Architecture"></center>
<center class="imgtext">DNVIDIA Clara Deploy Architecture（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
<br>

整個生態系統可以從**使用者**與**開發者**的角度出發，將系統分成 Platform 與 Application 兩個部份，在本章節中會先介紹 Platform 。

<br>

### Helm Charts
 
在上圖架構圖中存在著三種不同色階的綠色，其中顏色最深的是容器、次之的 Helm charts、最淺的則是一些腳本。而在 Platform 中，會將這些容器打包成不同用途 chart，方便進行部署與管理。

<center> <img src="https://i.imgur.com/sq9gHk9.png" alt="Role in Architecture"></center>
<center class="imgtext">架構圖元素（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
<br>

可以在安裝並啟動 Deploy SDK 後，藉由 `hlem ls` 指令來觀察目前所啟動的 charts。這些被啟動的 charts 可以與架構圖的中的標註一一對應，除了 Triton Inference Server 之外。

```bash
$ helm ls
NAME                    CHART 
clara                     clara-0.7.1-2008.1   
clara-console             clara-console-0.7.1-2008.1  
clara-dicom-adapter        dicom-adapter-0.7.1-2008.1 
clara-monitor-server        clara-monitor-server-0.7.1-2008.1   
clara-render-server        clara-renderer-0.7.1-2008.1
```
<br>

稍微對應並說明一下每個 chart 的角色與用途：

1. **clara**  
    這對應到的是架構圖中的 Platform Server。它是整個 SDK 的核心部份，負責控制 Workflow、Payloads、Pipelines、Jobs...等方面。另外其中的 Results Service，是用於追蹤所有 Pipelines 產生的所有結果，並負責與 Pipelines 和外部設備的 Services 溝通。  
    
    <center> <img src="https://i.imgur.com/0I4kTlX.png?1" alt="Platform Server"></center>
    <center class="imgtext">Platform Server（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
    <br>
    
2. **clara-monitor-server**  
    對應到的是圖中的 Monitoring Srvcs。它為平台為監督並提供了 GPU、 CPU 和 Disk 指標，一些監控結果可以從 Grafana （port 32000）上看到。 
     
    <center> <img src="https://i.imgur.com/xvtHBVw.png?1" alt="Monitoring Srvcs"></center>
    <center class="imgtext">Monitoring Srvcs（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
    <br>   
3. **clara-render-server**  
    對應到的是負責視覺化的 Render server service，負責提供醫療數據的可視化。
    
   <center> <img src="https://i.imgur.com/i9J8RXU.png" alt="Render server service"></center>
    <center class="imgtext">Render server service（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
    <br>   

4. **clara-console**  
    對應到的 UI 那一塊。主要是允許使用者從網頁去查看 Platform 的功能，不過目前僅能查看 Pipelines 跟 Jobs。
    
    <center> <img src="https://i.imgur.com/HCqoPdH.png" alt="Dashboard"></center>
    <center class="imgtext">Dashboard（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
    <br>   
    
5. **clara-dicom-adapter**  
    這是 DICOM 的資料接口。
    
     <center> <img src="https://i.imgur.com/GfHWnoJ.png" alt="DICOM"></center>
    <center class="imgtext">DICOM（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
    <br>   

 
最後一個無法進行對照的是圖中的 **Triton Inference Server**，它其實 [Tensor RT Inference Server](https://github.com/triton-inference-server/server)，就目前我所理解的應該是在啟動 Job 時，會一併啟動的服務，因此並不是常駐的 chart ，這之後說到 Application 再來談。

<center> <img src="https://imgur.com/vcfC0iU.png" alt="Triton Inference Server"></center>
<center class="imgtext">Triton Inference Server（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
<br>   


<div class="alert info"> 
<div class="head">Triton Inference Server 參考資料</div>

1. <a href="https://docs.nvidia.com/deeplearning/triton-inference-server/user-guide/docs/index.html">Triton Inference Server — Triton Inference Server 2.3.0 documentation</a> <br>
2. <a href="https://blog.csdn.net/zong596568821xp/article/details/108725430">NVIDIA之Triton Inference Server环境部署安装_ZONGXP的博客-CSDN博客</a><br>
3. <a href="https://roychou121.github.io/2020/07/20/nvidia-triton-inference-server/">Triton Inference Server 介紹與範例 | 不務正業工程師的家</a><br>
4. <a href="https://docs.nvidia.com/clara/deploy/sdk/Platform/Pipelines/public/docs/services.html">8.8. Services — Clara Deploy SDK 0.7.1 documentation</a>
</div>
<br>

### Clara Platform API 

而為了能操作 Clara Platform，Deploy SDK 提供了 GRPC based interface，來提供平台開發者進行擴充或應用開發者進行客製，這個 API 可以對 Platform 中的 Payload、 Pipeline 與 Job 進行增、刪、查、啟動...等程式操作與其他的內容交互功能。

例如，前面所提到的 DICOM Adapter，當接收到 DICOM 資料時，它會依靠 Clara Platform API 來啟動和監視 Pipeline Jobs。

<br>

另外一個，使用 Clara Platform API 的代表是 **Clara CLI**，它是 Clara 提供給應用開發者用來管理、進行交互操作的命令行介面。它除了能與 API 進行上述所提到的操作外，它也可以對 Platfrom 本身進行操作，如：`clara platform start`、`clara platform stop`，也可以對 helm chart 進行操作，如：`clara pull dicom`、`clara dicom start`。
 
<center> <img src="https://i.imgur.com/nhhLSp6.png" alt="Clara CLI"></center>
<center class="imgtext">Clara CLI（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">
</a>）</center>
<br> 

若是平台開發者想藉由程式的撰寫來進行操作，也可以透過另外提供的 [Python Client](https://github.com/NVIDIA/clara-platform-python-client) ，也就是 Python 版 Clara CLI，直接對平台進行管理與操控。

<center> <img src="https://i.imgur.com/9jIAc0Y.png" alt="Python Client"></center>
<center class="imgtext">Python Client（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
 

<br><br>

## Application

前一章節的介紹是偏向平台本身的架構與組成，接下讓我們看看 Application 的部分。

對於應用的開發者來說，若想將訓練出來的演算法與模型部署到臨床工作流程中，勢必得自定義一連串的處理與操作流程，並按序執行來處理醫療任務。

下面說明的，就是如何來定義與執行這些操作流程。

<center> <img src="https://i.imgur.com/WnQuxAU.png" alt="Chest Classification"></center>
<center class="imgtext">Chest Classification（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
<br>

### Definition

在 Deploy SDK 中，我們稱這一連串的操作流程為 **Pipeline**。在前面一節中，其實已經反覆出現了幾個 Deploy SDK 中的專有名詞，如 Pipeline、Job...等。因此在這邊我們先就這些專有名詞進行說明：

<center> <img src="https://i.imgur.com/zslS2Bu.png" alt="NVIDIA Clara Deploy Architecture"></center>
<center class="imgtext">Clara Pipelines/Applications（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html" class="imgtext">SDK 0.7.1 documentation</a>）</center>
<br>

1. **Operator**      
    在操作流程中的每個步驟我們稱之為 **Operator**，通常每個 Operator 會對讀入的資料做特定的功能或分析後再輸出，如上圖中的 DICOM READ、LIVER SEGM.。需要特別提醒的是，每個 Operator 其實是一個容器，這也是架構圖中用深綠色來繪製的原因。
    
    <center> <img src="https://i.imgur.com/luHVL4c.png" alt="Operator"></center>
    <center class="imgtext">Operator（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
    <br>

2. **Pipeline**  
    而一連串 Operator 會被一步步接成個**有向無環圖（Directed Acyclic Graph，DAG）**，這意味著資料會循序經由一連串特定操作寫出到特定位置，完成一次的預測。 
    
    <center> <img src="https://i.imgur.com/w0raR0Q.png" alt="Pipeline"></center>
    <center class="imgtext">Pipeline（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
    <br>
    
3. **Service**   
    不過實際使用時，並不是每種操作都適合使用短暫型的容器作為運算資源。若是操作或尋訪太過複雜與昂貴時，此時應改採 Service 作為運算資源。
    
    常見的 Service 有 NVIDIA Tensor RT Inference Server（又名 Triton），它能通過網路連接提供 client-server 推理服務。 
    
    不過[文件上](https://docs.nvidia.com/clara/deploy/sdk/Platform/Pipelines/public/docs/services.html)在介紹 Service 時提到，從 Clara Deploy SDK **v0.7.1** 起，不推薦使用 Service，並且在之後的版本中將不再支援，所以這邊之後要如何使用，可能必須在等他們的文件更新才知道了。     
    
    <center> <img src="https://i.imgur.com/HuJ7oYr.png" alt="Service"></center>
    <center class="imgtext">Service（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
    <br>

4. **Job**  
    為特定資料集所執行的 Pipeline 實例。
 
<br>

完整的 Pipeline 流程如下圖所示：

<center> <img src="https://i.imgur.com/6LQLhhu.png" alt="Clara deploy SDK"></center>
<center class="imgtext">Clara deploy SDK（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
<br>

資料從圖中 Pipeline 左方讀入，最後右方寫出，且相關的狀態也會呈現在 Web UI 上。之前所提到的 Platform Server 會負責 Pipeline 與 Workflow 的管控，完成後其中的 Results Service 會將所產生的結果，送到指定的位置。

<br>

若是從 Operator 微觀的角度來看資料的流動，會如下面所示：

<center> <img src="https://i.imgur.com/OWtoayw.png" alt="Clara deploy Operator"></center>
<center class="imgtext">Clara deploy Operator（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
<br>

Operator 的資料方向與 Pipeline 是一致，或者應該說 Pipeline 的方向與 Operator 一致才對。它會從左方讀入一至數筆資料進行處理，並依需求將資料輸出至一至多個 Operator，以交由下一個 Operator 進行操作。且在 Operator 的運行過程中，可以一次使用一或多項 Service。

如此一來，當每個 Operator 皆完成自身的操作時，整個 Pipeline 的操作也就完成。

<br> 

### Language 

但實際上，使用者該如何定義這一串的處理與操作流程？ 在 Deploy SDK 中，工作流程是由 **YAML** 模板來驅動，而模板的撰寫則是採用了類似 ADSL（Argo Domain Specific Language）的語法來完成。

```yaml
api-version: 0.4.0
name: chestxray-pipeline
operators:
  - name: ai-app-chestxray
    description: Classifying Chest X-ray Images
    container:
      image: clara/ai-chestxray
      tag: latest
    requests:
      gpu: 1
    input:
    - path: /input
    output:
    - path: /output
    services:
    - name: trtis
      container:
        image: nvcr.io/nvidia/tensorrtserver
        tag: 19.08-py3
        command: ["trtserver", "--model-store=$(NVIDIA_CLARA_SERVICE_DATA_PATH)/models"]
      connections:
        http:
        - name: NVIDIA_CLARA_TRTISURI
          port: 8000
```
<br>

這是一份部署胸部 X 光分類的範例，在這份模板中主要可以分成三個區域：
1. **`api-version`**  
2. **`name`**
3. **`operators`**

其中 `api-version` 會直接影響到工作流程引擎的選擇，這個我們稍後在介紹。至於 `name` 就是我們這個 Pipeline 的名字；最後就是我們定義一串的處理與操作流程的地方：**`operators`**。

`operators` 中會包含 1 至數個 Operator，每個 Operator 會包含下列資訊：

1. **`name/container`**  
    Operator 的名稱與描述。
2. **`container`**  
    Operator 所使用的映像檔名稱與版本。
3. **`requests`**  
    所需求的資源，如：GPU、記憶體...等。
4. **`services`**  
    這邊就是前面所介紹的 Tensor RT Inference Server，在使用時還會傳入模型的所在資料夾 (`/clara/common/models/`)。
5. **`input/output`**  
    這邊則是容器資料的輸入與輸出，在新的 **0.5.0** 版 Api 中，可以進一步指定傳入資料的格式與維度。

    ```yaml
    api-version: 0.5.0
    ...
    operators:
      - name: ai-app-chestxray
        input:
        - name: gradient_date
          type: array
          shape: [3, 244, 244, 127]
          element-type: float32
        output:
        - name: dicom_date
          type: stream
          format: dicom
    ```
<br>
    
在上述的範例，是個較為簡單的例子，它所使用的 Operator 只有一個，也就是只有一個步驟，若將它 Workflow 繪出，會如下圖所示：      

<center> <img src="https://i.imgur.com/wO3auUx.png" alt="Clara deploy Operator"></center>
<br>

若是想進行更複雜的資料處理，則需要在 Pipeline 中定義更多的 Operator：

```yaml
operators:
- name: mapper
  description: CUDA mapper
  container:
    image: ${{DOCKER_IMAGE}}
    tag: ${{DOCKER_TAG}}
    command: ["/bin/sh", "-c",
              "mapperWrapper.sh ${{MAPPER_ADDITIONAL_PARAMS}} -i /input -d /mapperOutput/${{JOB_ID}} -o /mapperOutput/${{JOB_ID}}/overlaps.paf -t ${{MAPPER_THREADS}} -k ${{MAPPER_KMER_SIZE}} -w ${{MAPPER_WINDOW_SIZE}} -s ${{MAPPER_INDEX_SIZE}}"]
  requests:
    gpu: 1
  input:
  - path: /input/
  output:
  - path: /mapperOutput

- name: miniasm
  description: Miniasm
  container:
    image: ${{DOCKER_IMAGE}}
    tag: ${{DOCKER_TAG}}
    command: ["/bin/sh", "-c",
              "miniasmWrapper.sh -f /mapperOutput/${{JOB_ID}}/sample.fasta -l /mapperOutput/${{JOB_ID}}/overlaps.paf -o /asmOutput/${{JOB_ID}}/reads.gfa"]
  input:
  - path: /input/
  - from: mapper
    path: /mapperOutput
  output:
  - path: /asmOutput

- name: racon
  description: Polish Assembly using racon
  container:
    image: ${{DOCKER_IMAGE}}
    tag: ${{DOCKER_TAG}}
    command: ["/bin/sh", "-c",
              "raconWrapper.sh ${{RACON_ADDITIONAL_PARAMS}} -r /mapperOutput/${{JOB_ID}}/sample.fasta -t ${{RACON_THREADS}} -l ${{RACON_LOOPS}} -p ${{RACON_POLISH_BATCH_SIZE}} -f /asmOutput/${{JOB_ID}}/reads.gfa -o /raconOutput/${{JOB_ID}} -a /raconOutput/${{JOB_ID}}/polished_assembly.fa"]
  requests:
    gpu: 1
  input:
  - path: /input/
  - from: miniasm
    path: /asmOutput
  - from: mapper
    path: /mapperOutput
  output:
  - path: /raconOutput/
```

<br>

這是 Clara 基因體分析的[部署範例](https://docs.nvidia.com/clara/deploy_archive/R6_2/sdk/Applications/Pipelines/Genomics/public/docs/README.html)。在範例中使用了 3 個 Operator 分別名為 `mapper`、 `miniasm` 與 `racon`，其中 `racon` 同時接收了 `mapper`、 `miniasm` 的輸出作為它自己的輸入，最終可以看到這樣的 Workflow：

<center> <img src="https://i.imgur.com/uXY7PoO.png?1" alt="Multi Input"></center>
<br><br>

目前 NVIDIA 有提供了不少現成的 Pipelines，這對於我這種懶人來說是個福音，因為可以省去撰寫模板的麻煩，或只要做少量的修改就好。

<center> <img src="https://i.imgur.com/P9D3Ybg.png?1" alt="目前支援 Pipeline"></center>
<center class="imgtext">目前支援 Pipelines（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/RunningReferencePipeline.html#select-a-reference-pipeline" class="imgtext">NVIDIA documentation</a>）</center>
<br>
 
也可以依照需求，抽換或增加 Operator 來搭建客製化的 Pipeline。
    
<center> <img src="https://i.imgur.com/mUoDV9m.png" alt="目前支援 operators"></center>
<center class="imgtext">目前支援 Operators（圖片來源: <a href="https://resources.nvidia.com/en-us-r-medical-imaging/scalable-and-modular-ai-deployment-whitepaper?topic=White%20Paper" class="imgtext">Scalable and Modular AI Deployment</a>）</center>
<br>


### User Defined Operators

不過除了使用 NVIDIA 提供的現成 Operators，另一種更常見的情況，可能是我們已經在 Train SDK 中訓練好了自己的網路與模型，想部署在 Deploy SDK 中，這時就必須客製自己的 Operator 了。
 
**Step1. 前處理**   
在準備其他文件前，須先使用 Train SDK 裡的遷移學習工具（Transfer Learning Toolkit, TLT），將訓練好的模型導出到與 TRTIS 兼容的平台。

<div class="alert info"> 
<div class="head">導出工具的介紹</div>

1. <a href="https://docs.nvidia.com/deeplearning/sdk/tensorrt-inference-server-guide/docs/">Triton</a> <br>
2. <a href="https://docs.nvidia.com/clara/index.html">Clara Train SDK</a><br>
3. <a href="https://docs.nvidia.com/clara/tlt-mi/clara-train-sdk-v3.1/nvmidl/appendix/mapping_of_old_to_new.html">Converting from previous TLT</a>
</div>

<br>

**Step2. 下載基礎應用容器的映像檔**  
還記得每個 Operator 其實是一個容器？NVIDIA 提供一版[基礎的映像檔](https://ngc.nvidia.com/catalog/containers/nvidia:clara:app_base_inference)，讓我們可以以此為基礎來擴充自己的映像檔。

```bash
$ docker pull nvcr.io/nvidia/clara/app_base_inference:0.7.3-2011.5
$ docker tag nvcr.io/nvidia/clara/app_base_inference:0.7.3-2011.5 app_base_inference:latest
```

<br>

**Step3. 建立一個 Python 專案**  
為了方便操作，可以建立一個 Python 專案，且專案中應有下列的文件結構

```
my_custom_app
├── config
│   ├── config_inference.json
│   └── model_config.json
├── Dockerfile
└── public
    └── docs
        └── README.md
```


其中 model_config.json 包含了模型屬性，而 config_inference.json 可以直接複製訓練期間中所使用的 MMAR 配置 。

<br>

**Step4. 修改 MMAR 文件**    
文件中的前後處理無須更改。需要更改的是 `name` 、 `inferer` 與 `model_loader`。

```json
"inferer":
{
    "name": "TRTISScanWindowInferer",
    "args": {
        "model_name": "segmentation_liver_v1",
        "ip": "localhost",
        "port": 8000,
        "protocol": "HTTP"
    }
},

"model_loader":
{
    "name": "TRTISModelLoader",
    "args": {
        "model_spec_file_name": "{PBTXT_PATH}"
    }
}
```


<br>

**Step5. Dockerfile**   
最後來撰寫 Dockerfile，並將前述所準備的東西一起放進映像檔中。 

```dockerfile
# Build upon the named base container; version tag can be used if known.
FROM app_base_inference:latest

# This is a well known folder in the base container. Please do not change it.
ENV BASE_NAME="app_base_inference"

# This is the name of the folder containing the config files; same as the app name.
ENV MY_APP_NAME="my_custom_app"

WORKDIR /app

# Copy configuration files to overwrite base defaults
COPY ./$MY_APP_NAME/config/* ./$BASE_NAME/config/
```

最後把將它 build 成映像檔。 

```shell
$ docker build -t ${APP_NAME} -f ${APP_NAME}/Dockerfile .
```
 
<br>

遵循這樣的步驟，就可以準備一份映像檔來作為我們自製的 Operator。當然不同的需求與操作，所需準備的資料不盡相同。若是真有需要，再回頭來 K 文件好了。

<br> 

### Orchestration Mode

前面說過 Deploy SDK 的工作流程是由類似 ADSL 語法撰寫 YAML 模板來驅動。

厄...其實我並沒有找到模板語法的正確名稱，會說是類似 ADSL，主要是與它們使用的工作流程引擎有關。在目前使用的工作流程引擎有兩套分別為 **Argo** 與他們自己自製的 **Clara Orchestration**。

Argo Workflow 是一個基於 Kubernetes CRD 實現開源工作流程引擎，它可以將工作流程中的每個步驟實現為一個容器，並提供了機制來指定工作流程中各步驟之間的約束，並將一個步驟的輸出連接為下一個步驟的輸入。

<center> <img src="https://i.imgur.com/74qOz0P.png" alt="Argo UI（0.3.0 之前）"></center>
<center class="imgtext">Argo UI</center>
<br>

不過 Argo 會將每個 Operator 分別起在不同 Pod 上，而每個 Pod 會需要 2 秒的啟動時間，因此整個 <span class='highlighting'>Pipeline 的啟動時間會取決於 Operator 數目</span>；而在 Clara Orchestration 則是會將所有的 Operator 起在同一個 Pod 上，無論 Operator 的個數，<span class='highlighting'>Pipeline 啟動時間都會是 2 秒</span>。

因此 NVIDIA 在 [文件](https://docs.nvidia.com/clara/deploy/sdk/Platform/Pipelines/public/docs/operators.html#argo-vs-clara-orchestration)上建議，基於效能的考量，在開發與測試時間可使用 Argo，但在正式部署則應使用 Clara 以追求更高的性能。

不過 Clara Orchestration 的 UI 現在似乎還在開發中，顯示有點簡陋：

<center> <img src="https://i.imgur.com/hmPrzZN.png" alt="Clara Orchestration UI（0.4.0+，現行版本）"></center>
<center class="imgtext">Clara Orchestration UI（0.4.0+，現行版本）</center>
<br>


至於如何選擇工作流程引擎？其實前面有稍微提到的，在撰寫 YAML 模板時 `api-version` 的版號會直接選擇工作流程引擎，若你選擇 **0.3.0** 則會使用 Argo；若是 **0.4.0+**，使用的就會是  Clara Orchestration。

最後附上，在演講中 Clara Orchestration UI ，這應該是未來的 UI ?

<center> <img src="https://i.imgur.com/wWJQGIK.png" alt="Clara Orchestration UI（0.4.0+，尚未釋出）"></center>
<center class="imgtext">Clara Orchestration UI（0.4.0+，尚未釋出）（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
<br>

 
<br> 

### Job Priority 

<center> <img src="https://i.imgur.com/37zBqTb.png?1" alt="Job Priority"></center>
<center class="imgtext">Job Priority</center>
<br>

當 Pipleline 被工作流程引擎啟動後，就會成了 Job。而在啟動 Pipleline 時，可以賦予一個優先級，決定啟動順序：

> **JobPriority**：Immediate > High > Normal > Low

- **立即優先級（immediate）**：擁有此優先級的 Job 會在單獨的佇列中排隊，在調度其他優先級的 Job 前必須先清空此佇列。
- **較高優先級（high）**：優先級較高的 Job 會比其他較低的 Job <span class='highlighting'>更常</span>被安排執行。
- **默認優先級（normal）**：默認的優先序。
- **較低優先級（Low）**：執行的<span class='highlighting'>頻率</span>低於較高優先級的 Job。

根據上述的文字看來，除了 immediate 擁有絕對的優先序外，其他的幾個級別都是**執行頻率**的差異。


<br><br> 

## 一個很酷的範例

在 Servery 的過程中看到一個很酷的範例 [Clara Deploy Multi AI Segmentation Pipeline](https://ngc.nvidia.com/catalog/resources/nvidia:clara:clara_multiai_pipeline)，決定分享一下：


<center> <img src="https://i.imgur.com/oMhHXFe.png" alt="Multi AI Segmentation "></center>
<center class="imgtext">Multi AI Segmentation （圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9" class="imgtext">Bringing AI to Hospitalsn</a>）</center>
<br>

但它的 YAML 也超級長的，害我不得不把它縮起來 XDDD
<details><summary><strong>超長的 YAML</strong></summary>
<pre><code class="yaml hljs"><span class="token key atrule">api-version</span><span class="token punctuation">:</span> 0.4.0
<span class="token key atrule">name</span><span class="token punctuation">:</span> multiAI<span class="token punctuation">-</span>pipeline
<span class="token key atrule">pull-secrets</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> ngc<span class="token punctuation">-</span>clara
<span class="token key atrule">operators</span><span class="token punctuation">:</span>
<span class="token comment"># ROI generator operator processing split operation</span>
<span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> roi<span class="token punctuation">-</span>split
  <span class="token key atrule">description</span><span class="token punctuation">:</span> ROI generator for input volume.
  <span class="token key atrule">container</span><span class="token punctuation">:</span>
    <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/clara/roi<span class="token punctuation">-</span>generator
    <span class="token key atrule">tag</span><span class="token punctuation">:</span> 0.7.2<span class="token punctuation">-</span><span class="token number">2010.1</span>
  <span class="token key atrule">requests</span><span class="token punctuation">:</span>
    <span class="token key atrule">gpu</span><span class="token punctuation">:</span> <span class="token number">1</span>
  <span class="token key atrule">input</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/in
  <span class="token comment"># output creates all ROI's and configuration data required by register-volume</span>
  <span class="token comment"># number of output ROI folder names must match the number of ROIs in ROI configuration file</span>
  <span class="token key atrule">output</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> lung
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/out/lung
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> liver
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/out/liver
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> spleen
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/out/spleen
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> colon
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/out/colon
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> config
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/out/config
<span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> lung<span class="token punctuation">-</span>tumor<span class="token punctuation">-</span>segmentation
  <span class="token key atrule">description</span><span class="token punctuation">:</span> Segmentation of lung tumor inferencing using DL trained model.
  <span class="token key atrule">container</span><span class="token punctuation">:</span>
    <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/clara/ai<span class="token punctuation">-</span>lungtumor
    <span class="token key atrule">tag</span><span class="token punctuation">:</span> 0.7.2<span class="token punctuation">-</span><span class="token number">2010.1</span>
  <span class="token key atrule">variables</span><span class="token punctuation">:</span>
    <span class="token key atrule">NVIDIA_CLARA_NII_EXTENSION</span><span class="token punctuation">:</span> <span class="token string">'.nii'</span>
  <span class="token key atrule">requests</span><span class="token punctuation">:</span>
    <span class="token key atrule">gpu</span><span class="token punctuation">:</span> <span class="token number">1</span>
  <span class="token key atrule">input</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> roi<span class="token punctuation">-</span>split
    <span class="token key atrule">name</span><span class="token punctuation">:</span> lung
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /input
  <span class="token key atrule">output</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">path</span><span class="token punctuation">:</span> /output
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationLung
  <span class="token key atrule">services</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> trtis
    <span class="token key atrule">container</span><span class="token punctuation">:</span>
      <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/tensorrtserver
      <span class="token key atrule">tag</span><span class="token punctuation">:</span> 19.08<span class="token punctuation">-</span>py3
      <span class="token key atrule">command</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"trtserver"</span><span class="token punctuation">,</span> <span class="token string">"--model-store=$(NVIDIA_CLARA_SERVICE_DATA_PATH)/models"</span><span class="token punctuation">]</span>
    <span class="token key atrule">connections</span><span class="token punctuation">:</span>
      <span class="token key atrule">http</span><span class="token punctuation">:</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> NVIDIA_CLARA_TRTISURI
        <span class="token key atrule">port</span><span class="token punctuation">:</span> <span class="token number">8000</span>
<span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> colon<span class="token punctuation">-</span>tumor<span class="token punctuation">-</span>segmentation
  <span class="token key atrule">description</span><span class="token punctuation">:</span> Segmentation of colon tumor inferencing using DL trained model.
  <span class="token key atrule">container</span><span class="token punctuation">:</span>
    <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/clara/ai<span class="token punctuation">-</span>colontumor
    <span class="token key atrule">tag</span><span class="token punctuation">:</span> 0.7.2<span class="token punctuation">-</span><span class="token number">2010.1</span>
  <span class="token key atrule">variables</span><span class="token punctuation">:</span>
    <span class="token key atrule">NVIDIA_CLARA_NII_EXTENSION</span><span class="token punctuation">:</span> <span class="token string">'.nii'</span>
  <span class="token key atrule">requests</span><span class="token punctuation">:</span>
    <span class="token key atrule">gpu</span><span class="token punctuation">:</span> <span class="token number">1</span>
  <span class="token key atrule">input</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> roi<span class="token punctuation">-</span>split
    <span class="token key atrule">name</span><span class="token punctuation">:</span> colon
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /input
  <span class="token key atrule">output</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">path</span><span class="token punctuation">:</span> /output
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationColon
  <span class="token key atrule">services</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> trtis
    <span class="token key atrule">container</span><span class="token punctuation">:</span>
      <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/tensorrtserver
      <span class="token key atrule">tag</span><span class="token punctuation">:</span> 19.08<span class="token punctuation">-</span>py3
      <span class="token key atrule">command</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"trtserver"</span><span class="token punctuation">,</span> <span class="token string">"--model-store=$(NVIDIA_CLARA_SERVICE_DATA_PATH)/models"</span><span class="token punctuation">]</span>
    <span class="token key atrule">connections</span><span class="token punctuation">:</span>
      <span class="token key atrule">http</span><span class="token punctuation">:</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> NVIDIA_CLARA_TRTISURI
        <span class="token key atrule">port</span><span class="token punctuation">:</span> <span class="token number">8000</span>
<span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> liver<span class="token punctuation">-</span>tumor<span class="token punctuation">-</span>segmentation
  <span class="token key atrule">description</span><span class="token punctuation">:</span> Segmentation of liver and tumor inferencing using DL trained model.
  <span class="token key atrule">container</span><span class="token punctuation">:</span>
    <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/clara/ai<span class="token punctuation">-</span>livertumor
    <span class="token key atrule">tag</span><span class="token punctuation">:</span> 0.7.2<span class="token punctuation">-</span><span class="token number">2010.1</span>
  <span class="token key atrule">variables</span><span class="token punctuation">:</span>
    <span class="token key atrule">NVIDIA_CLARA_NII_EXTENSION</span><span class="token punctuation">:</span> <span class="token string">'.nii'</span>
  <span class="token key atrule">requests</span><span class="token punctuation">:</span>
    <span class="token key atrule">gpu</span><span class="token punctuation">:</span> <span class="token number">1</span>
  <span class="token key atrule">input</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> roi<span class="token punctuation">-</span>split
    <span class="token key atrule">name</span><span class="token punctuation">:</span> liver
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /input
  <span class="token key atrule">output</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">path</span><span class="token punctuation">:</span> /output
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationLiver
  <span class="token key atrule">services</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> trtis
    <span class="token key atrule">container</span><span class="token punctuation">:</span>
      <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/tensorrtserver
      <span class="token key atrule">tag</span><span class="token punctuation">:</span> 19.08<span class="token punctuation">-</span>py3
      <span class="token key atrule">command</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"trtserver"</span><span class="token punctuation">,</span> <span class="token string">"--model-store=$(NVIDIA_CLARA_SERVICE_DATA_PATH)/models"</span><span class="token punctuation">]</span>
    <span class="token key atrule">connections</span><span class="token punctuation">:</span>
      <span class="token key atrule">http</span><span class="token punctuation">:</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> NVIDIA_CLARA_TRTISURI
        <span class="token key atrule">port</span><span class="token punctuation">:</span> <span class="token number">8000</span>
<span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> spleen<span class="token punctuation">-</span>segmentation
  <span class="token key atrule">description</span><span class="token punctuation">:</span> Segmentation of spleen inferencing using DL trained model.
  <span class="token key atrule">container</span><span class="token punctuation">:</span>
    <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/clara/ai<span class="token punctuation">-</span>spleen
    <span class="token key atrule">tag</span><span class="token punctuation">:</span> 0.7.2<span class="token punctuation">-</span><span class="token number">2010.1</span>
  <span class="token key atrule">variables</span><span class="token punctuation">:</span>
    <span class="token key atrule">NVIDIA_CLARA_NII_EXTENSION</span><span class="token punctuation">:</span> <span class="token string">'.nii'</span>
  <span class="token key atrule">requests</span><span class="token punctuation">:</span>
    <span class="token key atrule">gpu</span><span class="token punctuation">:</span> <span class="token number">1</span>
  <span class="token key atrule">input</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> roi<span class="token punctuation">-</span>split
    <span class="token key atrule">name</span><span class="token punctuation">:</span> spleen
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /input    
  <span class="token key atrule">output</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">path</span><span class="token punctuation">:</span> /output
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationSpleen
  <span class="token key atrule">services</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> trtis
    <span class="token key atrule">container</span><span class="token punctuation">:</span>
      <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/tensorrtserver
      <span class="token key atrule">tag</span><span class="token punctuation">:</span> 19.08<span class="token punctuation">-</span>py3
      <span class="token key atrule">command</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"trtserver"</span><span class="token punctuation">,</span> <span class="token string">"--model-store=$(NVIDIA_CLARA_SERVICE_DATA_PATH)/models"</span><span class="token punctuation">]</span>
    <span class="token key atrule">connections</span><span class="token punctuation">:</span>
      <span class="token key atrule">http</span><span class="token punctuation">:</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> NVIDIA_CLARA_TRTISURI
        <span class="token key atrule">port</span><span class="token punctuation">:</span> <span class="token number">8000</span>
<span class="token comment"># ROI generator operator processing merging operation</span>
<span class="token comment"># takes input from all AI operators and merges them into a single volume</span>
<span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> volume<span class="token punctuation">-</span>merger
  <span class="token key atrule">description</span><span class="token punctuation">:</span> Volume merging for output mask.
  <span class="token key atrule">container</span><span class="token punctuation">:</span>
    <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/clara/roi<span class="token punctuation">-</span>generator
    <span class="token key atrule">tag</span><span class="token punctuation">:</span> 0.7.2<span class="token punctuation">-</span><span class="token number">2010.1</span>
  <span class="token key atrule">variables</span><span class="token punctuation">:</span>
    <span class="token key atrule">ROI_OPERATION</span><span class="token punctuation">:</span> merge
  <span class="token key atrule">requests</span><span class="token punctuation">:</span>
    <span class="token key atrule">gpu</span><span class="token punctuation">:</span> <span class="token number">1</span>
  <span class="token key atrule">input</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> roi<span class="token punctuation">-</span>split
    <span class="token key atrule">name</span><span class="token punctuation">:</span> config
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/in/config
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> lung<span class="token punctuation">-</span>tumor<span class="token punctuation">-</span>segmentation
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationLung
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/in/lung
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> colon<span class="token punctuation">-</span>tumor<span class="token punctuation">-</span>segmentation
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationColon
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/in/colon
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> liver<span class="token punctuation">-</span>tumor<span class="token punctuation">-</span>segmentation
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationLiver
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/in/liver
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> spleen<span class="token punctuation">-</span>segmentation
    <span class="token key atrule">name</span><span class="token punctuation">:</span> segmentationSpleen
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/in/spleen
  <span class="token key atrule">output</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> output
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/out/output
  <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> rendering
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /app/out/publish
<span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> register<span class="token punctuation">-</span>volume<span class="token punctuation">-</span>images<span class="token punctuation">-</span>for<span class="token punctuation">-</span>multi<span class="token punctuation">-</span>organ
  <span class="token key atrule">description</span><span class="token punctuation">:</span> Register volume images<span class="token punctuation">,</span> MHD format<span class="token punctuation">,</span> for rendering.
  <span class="token key atrule">container</span><span class="token punctuation">:</span>
    <span class="token key atrule">image</span><span class="token punctuation">:</span> nvcr.io/nvidia/clara/register<span class="token punctuation">-</span>results
    <span class="token key atrule">tag</span><span class="token punctuation">:</span> 0.7.2<span class="token punctuation">-</span><span class="token number">2010.1</span>
    <span class="token key atrule">command</span><span class="token punctuation">:</span> <span class="token punctuation">[</span><span class="token string">"python"</span><span class="token punctuation">,</span> <span class="token string">"register.py"</span><span class="token punctuation">,</span> <span class="token string">"--agent"</span><span class="token punctuation">,</span> <span class="token string">"renderserver"</span><span class="token punctuation">]</span>
  <span class="token key atrule">input</span><span class="token punctuation">:</span>
  <span class="token punctuation">-</span> <span class="token key atrule">from</span><span class="token punctuation">:</span> volume<span class="token punctuation">-</span>merger
    <span class="token key atrule">name</span><span class="token punctuation">:</span> rendering
    <span class="token key atrule">path</span><span class="token punctuation">:</span> /input
</code></pre>
</details>

    
<br><br> 

## 參考資料 
1. [使用Clara Deploy SDK创建、管理和部署AI增强的临床工作流程](http://www.gpus.cn/gpus_list_page_techno_support_content?id=29)。檢自 技术部(2020-11-09)。
2. Rahul Choudhury, Brad Genereaux, Risto Haukioja and David Bericat  (2020-06-26)。[Deploying Healthcare AI Workflows with the NVIDIA Clara Deploy Application Framework (updated)](https://developer.nvidia.com/blog/deploying-healthcare-ai-workflows-with-clara-deploy-app-framework-updated/) 。檢自 NVIDIA Developer Blog (2020-11-09)。
3. Jesse Tetreault, Rahul Choudhury, Brad Genereaux, Kristopher Kersten andJiahui Guan (2020-04)。[[White Paper] Scalable and Modular AI Deployment](https://resources.nvidia.com/en-us-r-medical-imaging/scalable-and-modular-ai-deployment-whitepaper?topic=White%20Paper) 。檢自 NVIDIA (2020-11-09)。
4. David Bericat Lacima (2020-10-07)。[Bringing AI to Hospitals: How to Design a Clinical Imaging Infrastructure for AI at the Edge [A21253]](https://www.nvidia.com/zh-tw/gtc/session-catalog/?tab.catalogtabfields=1600209910618002Tlxt&search=BRINGING#/session/1597161391443001veQ9) 。檢自 2020 年 GPU 技術大會｜NVIDIA GTC (2020-11-09)。
5. (2020-11-07)。[Clara Deploy Bootstrap](https://ngc.nvidia.com/catalog/resources/nvidia:clara:clara_bootstrap)。檢自 NVIDIA NGC (2020-11-09)。
6. (2020-10-14)。[Clara Deploy Platform](https://ngc.nvidia.com/catalog/collections/nvidia:claradeployplatform)。檢自 NVIDIA NGC (2020-11-09)。
7. (2020-04-04)。[Clara Deploy Load Generator CLI](https://ngc.nvidia.com/catalog/resources/nvidia:clara:load_generator)。檢自 NVIDIA NGC (2020-11-09)。
8. (2020-04-04)。[Clara Deploy Genomics Analysis - De Novo Sequence Assembly](https://ngc.nvidia.com/catalog/resources/nvidia:clara:clara_denovo_assembly_pipeline)。檢自 NVIDIA NGC (2020-11-09)。
9. (2020-04-04)。[Clara Deploy AI Chest X-Ray Classification Pipeline](https://ngc.nvidia.com/catalog/resources/nvidia:clara:clara_ai_chestxray_pipeline)。檢自 NVIDIA NGC (2020-11-09)。
10. [Latest Healthcare/Clara Deploy SDK topics](https://forums.developer.nvidia.com/c/healthcare/clara-deploy-sdk/151)。檢自 NVIDIA Developer Forums (2020-11-09)。
11. [NVIDIA Clara Deploy SDK User Guide — Clara Deploy SDK 0.7.1 documentation](https://docs.nvidia.com/clara/deploy/index.html)。檢自 NVIDIA documentation (2020-11-09)。
12. godleon (2018-12-22)。[[Kubernetes] Package Manager - Helm 簡介](https://godleon.github.io/blog/Kubernetes/k8s-Helm-Introduction/)。檢自 小信豬的原始部落 (2020-11-09)。
13. [9.4. Clara Deploy Base Inference Application](https://docs.nvidia.com/clara/deploy/sdk/Applications/Operators/ai/app_base_inference/public/docs/README.html)。檢自 Clara Deploy SDK 0.7.3 documentation (2021-01-07)。
14. rocdu (2020-07-13)。[使用argo构建云原生workflow](https://cloud.tencent.com/developer/article/1659446)。檢自 云+社区｜腾讯云 (2021-01-07)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2021-01-18</summary>
  <ul class="timestamp">
    　<li>2021-01-18 發布</li>
    　<li>2021-01-08 完稿</li>
    　<li>2020-11-09 起稿</li>
  </ul>
</details>
