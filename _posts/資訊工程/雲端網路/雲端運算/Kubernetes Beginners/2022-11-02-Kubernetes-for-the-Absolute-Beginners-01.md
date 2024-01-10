---
title: "【K8S Beginners 筆記】CH 1~3 Introduction、Overview and Setup"
date: 2022-11-09 
is_modified: true
categories:
- "雲端網路 › 雲端運算"
tags:
- "Udemy"
- "Docker"
- "K8S"
- "Kubernetes Beginners"
- "讀書筆記"
--- 


最近公司在流行考 CKA 與 CKAD 的證照，所以大家最近在都在買課程，說真的 Udemy 應該給我們團購價的 XDDD 
  
不過 Beginners 這門課程並不是針對 CKA 或 CKAD，這門課算是它們的前置課程，算是給 K8S 初學者的，正適合我這個一知半解者。

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/2F0W0aE.png" alt="課程縮圖">
    課程縮圖（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

## CH 1｜Introduction
這個章節只是做一個大概的介紹，稍微介紹下本課程的內容：

<p class="illustration">
    <img src="https://i.imgur.com/75iwkSv.png" alt="本課程的內容">
    本課程的內容（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

另外，課程還提供了個[永久的 Lab](https://kodekloud.com/courses/labs-kubernetes-for-the-absolute-beginners-hands-on/) 是個可以用來練習 K8S 環境，這個也是當初吸引購買這系列課程的主因 XDDD


## CH 2｜Kubernetes Overview
Kubernetes，簡稱為 K8S，是用於<mark>自動部署、擴充和管理「容器化（containerized）應用程式」</mark> 的開源系統。該系統由 Google 設計並捐贈給 Linux 基金會。

在這章節先介紹兩個術語： **Container** 與 **Orchestration**
<p class="illustration">
    <img src="https://i.imgur.com/HlgeMVJ.png" alt="兩個基本術語術語">
    兩個基本術語術語（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


### 2-1｜Container Overview
這節說是介紹 container ，倒不如說是介紹 Docker 這個容器技術，雖然我們常常把 Docker 和 container 劃上等號，不過市面上確實有其他容器技術可以取代 Docker，如：LXC、LXD、LXCFS。

先留個連結之後再來研究其他技術的特點：
- [〈再見 Docker，8 款容器替代方案｜閱坊〉](https://www.readfog.com/a/1640200187123175424)
- [〈2022 年要考虑的 7 种 Docker 替代方案｜InfoQ〉](https://www.infoq.cn/article/ggimepmcvdqw4eos2cig)


#### 解決痛點
1. **相依性地獄（Dependency Hell）**  
    在未導入容器化技術前，若需要開發一個涵蓋全端的服務，需要各種不同的技術，如 NodeJS 的前端、MongoDB 的資料庫、Kafka 的消息系統與 Ansible 的自動化編排工具...等。

    除必需一一從頭部署這些服務元件外，還必需解決它們的相依性問題，包含服務元件與操作系統的相依性、元件與元件之間共用函式庫的依賴衝突...等，且每次元件升級這些組件，這些相依性檢查就必須再來一次。

    課程中把這樣的現象稱之為地獄矩陣，維基百科則稱之為[相依性地獄](https://zh.wikipedia.org/zh-tw/相依性地狱)：

    <p class="illustration">
        <img src="https://i.imgur.com/xCIDXVQ.png" alt="地獄矩陣-相依性問題">
        地獄矩陣-相依性問題（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
    </p>

2. **環境設定困難**  
    每次新開發人員的加入都必須下達大量指令來設置他們的環境，但說實話，設置過系統的人都知道即使照著教學，會踩坑的人還是會踩坑，不然我也不會寫出這麼多的踩坑紀錄 XDDD
    
    除開發環境（development）外，可能還會存在整合環境（integration）、測試環境（testing）、驗證環境（Quality Assurance)、模擬環境（staging）、生產環境（production）...等環境，每個環境的建立就得重新設置一次，耗時耗力，且難以維持所有環境的一致性。


為了避免相依性地獄並提升每次環境設置效率，其中一種實做方式是導入容器化技術，它能允許我們<mark>在不影響其他元件的情況下修改元件，甚至根據需求修改底層操作系統</mark>。

在導入容器技術後，可在**同一個虛擬機器（VM）或作業系統（OS）** 執行不同的元件，每個元件可以存在單獨的容器，並擁有專用的函式庫與依賴項目。且建構時只需要執行 `docker run` 即可完成配置，可大幅提升工作效率。

<p class="illustration">
    <img src="https://i.imgur.com/AIiXBDV.png" alt="服務中的每個元件可以會存在單獨的容器">
    服務中的每個元件可以會存在單獨的容器（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>
    

#### Container 基本概念
與 VM 一樣，<mark>container 是個隔離的環境</mark>，它們擁有自己的 processes 或 service、自己的 network interface 和自己的 mounts；但與 VM 不同的是<mark>它們會共享的 OS Kernel</mark>。

<p class="illustration">
    <img src="https://i.imgur.com/TEXr2kI.png" alt="什麼是 Container">
    什麼是 Container（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

在理解 Docker 如何運作前，先了解下 Linux 系列的 OS。這些 OS 是由 Kernel 與一系列軟體所組成，其中 OS Kernel 負責操作底層硬體，而軟體則可能是不同的使用者界面、驅動程式、編譯器、文件管理器、開發工具...等。

因此市面上常見的 Ubuntu、Fedora、SUSE 與 CentOS ...等 OS，他們其實是使用通用的 Linux Kernel，並根據系統特色提供自定義的使用者界面與軟體。
 
<p class="illustration">
    <img src="https://i.imgur.com/6wJImxX.png" alt="Linux 系列的 OS 是通用的 Linux Kernel">
    Linux 系列的 OS 是通用的 Linux Kernel（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

而這概念遇前面的所提到的 **Container 會共享的 OS Kernel** 相呼應。因此在 Linux 系統上可以執行基於另一個發行版的 Container，因為這些 Container 是利用了 Docker 主機的底層 Kernel，並與其中所包含操作系統配合使用。

換句話說，如果若主機的 OS 與 Container 的 OS 無法共享 Kernel 的話，Container 是無法啟動的！在說誰...就 **Windows** 阿 XDDD 不過 MacOS 的話我不太確定，我覺得應該可以跑？因為 Darwin 也是種類 Unix 的作業系統，我在 [Docker Hub](https://hub.docker.com/r/joseluisq/rust-linux-darwin-builder) 上有看到個專案，所以應該跑得起來吧 XDDD

<p class="illustration">
    <img src="https://i.imgur.com/QyUfQEP.png" alt="主機的 OS 與 Container 的 OS 無法共享 Kernel">
    主機的 OS 與 Container 的 OS 無法共享 Kernel（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


至於為何在 Windows 上可以跑 Linux Container 的？根據我查到的資料，在 Windows 10 Pro 要透過 Hyper-V 提供 hypervisor 功能，Docker Desktop for Windows 則包含薄薄一層 Linux 與 Docker 角色。

<p class="illustration">
    <img src="https://i.imgur.com/I5POzgT.png" alt="Docker Desktop for Windows">
    Docker Desktop for Windows（圖片來源: <a href="https://functional.style/docker/general/overview/">點燈坊</a>）
</p>


是說無法執行其他 Kernel 的 Container 算缺點嗎？我認為應該**不算**，因為 Docker 主要用途是將應用程式 Container，方便部署與管理。其他 Kernel 的元件本來就無法部署在這，更遑論還要提升其方便性了。 


#### VM 和 Container 之間的差異
從圖中可以看出兩者系統架構最大的差別在於：<mark>每個 VM 都有自己的 OS，基於此 OS 執行應用程式；而 Container 會依賴主機的 OS 在其上執行應用程式</mark>。

也因為 VM 擁有自己的 OS 和 Kernel，所以**會佔用更多的硬體空間**，至少都是 GB 起跳，且對於底層的**資源利用率也會更吃重**；反之，Container 是輕量級的，故相比 VM ，Container 在啟動時會更加的迅速。

另外一點需要注意的是，雖然 Container 也是個隔離的環境，但並不像 VM 那樣完全隔離。這是因為 Container 需要共享主機 OS 與 Kernel，導致其隔離性比 VM 低，**安全性也較差**。

 
<p class="illustration">
    <img src="https://i.imgur.com/M0A8xcS.png" alt="虛擬機和容器之間的差異">
    虛擬機和容器之間的差異（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

這邊節錄部分 [Microsoft 對於容器與虛擬機器](https://learn.microsoft.com/zh-tw/virtualization/windowscontainers/about/containers-vs-vm)的比較：


| 功能       | 虛擬機器                                                                      | 容器                                                                                           |
| ---------- | ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| 隔離       | 可與主機 OS 和其他 VM 徹底隔離。如果重視安全性可以用這個。                    | 一般可提供與主機和其他容器的輕量型隔離功能，但無法提供和 VM 一樣的強式安全性界限。             |
| OS         | 會執行包含 Kernel 在內的完整 OS，因此需要更多系統資源 (CPU、記憶體和儲存體)。 | 執行 OS 的使用者模式部分，而且可以量身打造而只包含應用程式所需的服務，因此使用的系統資源較少。 |
| 客體相容性 | 幾乎可以在虛擬機器內執行任何作業系統。                                        | 只能執行與主機的 OS 相同的容器。                                                               |
|部署|使用 Windows Admin Center 或 Hyper-V 管理員部署個別 VM；使用 PowerShell 或 System Center Virtual Machine Manager 部署多個 VM。|透過命令列使用 Docker 部署個別容器；使用協調器 (例如 Azure Kubernetes Service) 部署多個容器。|


#### Image、Container 與 Registry
在介紹容器化技術時，或說是 Docker 時，會提到三個基本詞：Image、Container 與 Registry。

- **Image**  
    常見翻譯為映像檔或是鏡像。是個**唯讀的完整操作系統環境**，在整個容器化技術中扮演了<mark>模板</mark>的角色，可用於創建一個或多個 Container。
        
    如果沒有符合需求的現成 Image，我們可以基於現有的 Image，向上疊加所需要的操作。需要注意的是，每一層在建成之後就不會再改變，所以才會說 Image 是唯讀的環境。之前寫過一篇[〈客製化自己的 Docker 映像檔流程〉](https://cynthiachuang.github.io/The-Workflow-for-Creating-Your-Own-Custom-Image/)就是在做這件事。
    
     <p class="illustration">
        <img src="https://i.imgur.com/3agtqA9.png" alt="Image 可用於創建一個或多個 Container">
        Image 可用於創建一個或多個 Container（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
    </p>


- **Container**      
    如果說 Image 是模板，而 <mark>Container 就是基於此模板所創建出來的實體（Instance）</mark>，用物件導向來說明就是 Class 和 Instance 之間的關係。
    
    就像課程的圖片所示，這些 Container 可以想像成一個個獨立的環境，彼此並不互相干涉，對任何一個 Container 都不會對其他 Container 造成影響...除非它們共用同一個 Volume？


- **Registry**  
    課程中並沒有出現這個字，不過課程提到的 Dockerhub 其實就是這概念了，所以我還是把它寫出來了。在課程提到可以將 Image 推到 Dockerhub，這概念就如同 **Github 跟 Repository 是一樣的概念**，它們一樣可以 pull 跟 push，只是 Git Repository 是存放 source code，而 Docker Repository 是存放 Docker images。
  
<br class="big">

找到一張圖我覺得還滿清楚說明三者間的關係，雖然有些 keyword 還沒介紹，但應該不影響理解這張流程圖：

<p class="illustration">
    <img src="https://i.imgur.com/0vIF2co.png" alt="Docker 架構圖">
    Docker 架構圖（圖片來源: <a href="https://ithelp.ithome.com.tw/articles/10197758">iT 邦幫忙</a>）
</p>


這三者在開發與維運都佔據了重要的角色，過往交付開發時，運營團隊並沒有設置應用的經驗，必須與開發團隊合作完成設置。當導入 Docker 後，建立基礎設施所涉及的大部分工作都可以藉由 Docker 來完成。

開發團隊將 Image 上傳至公開的映像檔倉庫（Public Image Registry）或是組織建置的私有倉庫（Private），運營團隊就可從中取得 Image，並藉由 Image 在各平台上部署應用程式的 Container。這不僅能快速部署出生產環境，還能確保與開發環境的一致性。


### 2-2｜Container Orchestration
當完成 Image 打包後，這章節介紹如何部署與 **Container Orchestration**。當在部署 Container 時，Container 間可能互有依賴，如：資料庫、訊息系統...等，且當負載出現異動時，必須要能向上或向下擴展，以符合需求。

這些需求可由 Container Orchestration（容器編排）來達成，它<mark>具備自動部署和管理容器的功能</mark>，會協調 Container 間的連接，並實現自動擴展或縮減（Auto Scaling Out/In）。

<p class="illustration">
    <img src="https://i.imgur.com/dAjxzMt.png" alt="Container Orchestration">
    Container Orchestration（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


#### Orchestration Technologies
本堂課程的主角 <mark>Kubernetes（K8S）就是一種 Container Orchestration 技術</mark>，也是目前最流行的調度工具，在所有公有雲服務，如 GCP、Azure 和 AWS，上都有提供此技術，但它絕非是唯一的 Container Orchestration 技術。

除了 Google 的 Kubernetes 外，還有 Docker 自己的 **Swarm** 和 Apache 的 **MESOS**...等。不過各有各的有優缺點，像 Swarm 雖容易設置和入門，但無法應付複雜應用場景；而 MESOS 則是學習曲線陡峭，且其複雜度對於小叢集（Cluster）來說太過複雜，不過對成百上千個 node 的大型系統到是游刃有餘；至於 K8S 則是介於兩者之間。

是說，下圖中的統計中有出現一個 OpenShift，這個也是個 Container Orchestration 技術，它是 Red Hat 紅帽所推出的一個 Kubernetes 發行版。更明確地說，OpenShift 則是圍繞著 Kubernetes 建構了更多功能，換句話說，Kubernetes 是 OpenShift 不可或缺的一部分。

<p class="illustration">
    <img src="https://i.imgur.com/O6WHEvq.png" alt="Orchestration Technologies 2020 市占率">
    Orchestration Technologies 2020 市占率（圖片來源: <a href="https://www.t4.ai/industries/container-platform-market-share">T4</a>）
</p>


#### Orchestration 優點
1. **高可用性**  
    因為服務會有多個實體在不同的 node 上執行，即便發生硬體故障也不會導致服務停擺。此外，Orchestration 所帶來的自動化方法，能減少人為操作錯誤的機會。    
2. **彈性**  
    Orchestration 可以自動重啟或擴展 Container 或 Cluster。當使用者需求增加時，可以無縫部署更多實體，以負荷增加的需求；若需求減少，也可以在不關閉服務的情況下，減少底層 node 的數量。    
3. **簡化操作**  
    只需通過一組配置文件的文件輕鬆完成所有操作，如：負載平衡、Container 管理。


### 2-3｜Kubernetes Architecture
在開始設置 Kubernetes Cluster 前，先介紹一些專有名詞語概念。

#### 專有名詞
1. **節點（Nodes/Minions）**  
    Node 節點，在過去又被稱為 Minions（小小兵 XDDD），不過這個叫法現在應該比較少見？至少我身邊的人都叫 node。
    
    廣義上的 node 可以是<mark>一個物理機器或者虛擬機</mark>，取決於所在 cluster 的配置。而 K8S 會將 container 放入其中啟動來執行工作負載。狹義上的 node 則是特指 worker node，是指實際的運算資源。
    
 
    <p class="illustration">
        <img src="https://i.imgur.com/2ZiYkvn.png" alt="節點（Nodes/Minions）">
        節點（Nodes/Minions）（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
    </p>

2. **叢集（Cluster）**   
    **Cluster 就是一組 node**。Cluster 的好處在於，若單一 node 失效時，仍然可以訪問其他 node，以**維持服務的可用性**。此外，多 nodes 也有助於**分擔負載**。

    <p class="illustration">
        <img src="https://i.imgur.com/rI1qHTs.png" alt="叢集（Cluster）">
        叢集（Cluster）（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
    </p>

3. **主節點（Master）**  
    Master 本身就是一個 node，只是被賦予了**特殊的角色任務**。它是 <mark>K8S cluster 的管理者</mark>，負責管理並記錄 cluster 與相關資料、監視 cluster 中的 nodes，並負責 nodes 上容器的實際 orchestration，如：將故障 node 的工作負載轉移到另一個 node。 

    <p class="illustration">
        <img src="https://i.imgur.com/wgf7qXB.png" alt="主節點（Master）">
        主節點（Master）（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
    </p>


#### Components
實際安裝 Kubernetes，會裝入下列 **6 種元件**：

<p class="illustration">
    <img src="https://i.imgur.com/7pXH0Fe.png" alt="Kube Components">
    Kube Components（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


1. **API server**  
    充當 K8S 的前端。使用者、管理設備、CLI 都會與 API server 進行通訊，以與 K8S cluster 進行交流。 
    
2. **etcd service**  
    K8S 系統中使用 etcd 存儲所有用於管理 cluster 資訊，如：pod、service deployments...等。etcd 會以 etcd lock 的技術，將這些資訊以 key-value pair 的格式分散儲存。
    
3. **kubelet service**  
    Kubelet 相當於 node 上管理者，它會確保每個 container 都如期在 node 上執行。 
    
4. **Container Runtime**  
    Container Runtime 是執行 container 的基礎軟體，在這門課程中使用的是 Docker Engine。當然也有其他的選擇，我記得看過有人嘗試用 RKT 來作為 Runtime，雖然我不記得他最後為什麼放棄了 XDDD 
    
5. **Controllers**  
    Controllers 則是 orchestration 背後的大腦，當 node、container 或 endpoint 出現故障時，controllers 會進行通知與回應，通常情況下，controllers 會啟動新的 container。
    
    <br class="big">

    是說找資料時有注意到，controllers 裡的 s 真的有很多控制器，後面的課程應該會提到？
    
6. **Schedulers**  
    Schedulers 工作調度器，主要負責資源調度，例如 node 間的負載與 container 安排。
    

#### Master vs Worker Nodes
這 <mark>6 種元件與前面提到的 Master-Workers 架構中的 2 種 node 共同組成 K8S 系統</mark>。雖說如此，但在單一 node 上並非此 6 種元件全都存在，依照角色的不同會部署不同元件。 

<p class="illustration">
    <img src="https://i.imgur.com/WvBnRoC.png" alt="Master vs Worker Nodes">
    Master vs Worker Nodes（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>
 
Workers node，也就是我們的負責執行 container 的小小兵（minion），因此為了執行 container，node 上必須具備 **container runtime**。除此之外，在 node 中還會有一個管理者 **kubelet**，它會確保 container 的執行符合 master node 的要求；除此之外，kubelet 還負責與 master 互動，並接收來自 master 請求與提供健康訊息。

與 kubelet 相對的是 **kube-apiserver**，這是身為 <mark>master 的標誌性象徵</mark>，也是 K8S cluster 中最重要的元件，因為對內它是 cluster 中各個 node 的溝通橋樑，因為 node 彼此間是無法直接溝通的；對外，它也充當 K8S 的前端負責與使用者溝通，並將使用者需求經由 controllers 與 schedulers 判斷後，將需求發送給其他 node。另外，kubelet 除充當溝通大師外，K8S 中的身份認證與授權也是由它負責。

而在 master 中其中一個與 kube-apiserver 直接互動的元件是 **etcd**，它會存放 K8S cluster 的資料作為備份，並記錄整個 cluster 的狀態及資料。Master 之中還有 controller 與 scheduler 等元件，不過這章節先暫時停在這，其他元件的詳細說明應該會在後面課程說明（？）

 
#### kubectl
這節介紹 kubectl，又稱 kube command line tool 或 kube control。它是 K8S 所提供的 API 命令工具集，能與 K8S cluster 進行溝通，能部署與管理在 K8S 上的應用、並取得 cluster 或 node 的狀態與訊息。是說有人跟我一樣 kubectl 跟 kubelet 傻傻分不清楚嗎 XDDD

<p class="illustration">
    <img src="https://i.imgur.com/xVxABOG.png" alt="常用 kubectl 指令">
    常用 kubectl 指令（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>
 
基礎常用指令有 run、cluster-info 和 get nodes 。
- **`kubectl run`**：用於在 cluster 中部署應用。
- **`kubectl cluster-info`**：查看有關集 cluster 的資訊。
- **`kubectl get nodes/pods`**： 列出 cluster 中的所有 node 資訊。 

<br class="big">

這邊忽然冒出了一個專有名詞 pod，所以我去查查相關定義：
1. **Pod**  
    它是 K8S 中**最小的運作單位**，一個 pod 對應到一個應用服務，會裝載一個或多個 container。
2. **Node**  
    它是 K8S 中**最小硬體運作單位**，通常一個 node 對應到一個物理機器或者虛擬機。
    
所以結合[前面](#專有名詞)所說，可以得知 cluster 中有許多 node，而 K8S 會依據請求在 pod 中啟動一或多個 container，最後會以 pod 為單位將 container 放入到 node 上執行。
  
<p class="illustration">
    <img src="https://i.imgur.com/kUbliM1.png" alt="nodes 與 pods">
    nodes 與 pods（圖片來源: <a href="https://fabriciosanchez.azurewebsites.net/3/kubernetes-pods/">Fabrício Sanchez</a>）
</p>


### 2-4｜測驗 1：Architecture

1. **What is a worker machine in Kubernetes known as?**
    - [ ] Cluster
    - [ ] Node
    - [ ] Minion
    - [x] Node or Minion
2. **A Node in Kubernetes can only be a physical machine and can never be a virtual machine.**
    - [ ] True
    - [x] False
3. **Multiple Nodes together form a**
    - [ ] POD
    - [ ] Group
    - [x] Cluster
    - [ ] Swarm
4. **Which of the following processes runs on Kubernetes Master Node**
    - [ ] Kubelet
    - [x] Kube-apiserver
    - [ ] Kube-proxy
5. **Which of the following is a distributed reliable key-value store used by kubernetes to store all data used to manage the cluster**
    - [ ] Kube-apiserver
    - [ ] Kubelet
    - [ ] scheduler
    - [x] etcd
    - [ ] controller
6. **Which of the following services is responsible for distributing work or containers across multiple nodes.**
    - [ ] kube-apiserver
    - [ ] kubelet
    - [x] scheduler
    - [ ] etcd
    - [ ] controller
7. **Which of the following is the underlying framework that is responsible for running application in containers like Docker?**
    - [ ] kube-apiserver
    - [ ] kubelet
    - [x] container runtime
    - [ ] scheduler
    - [ ] controller
8. **Which is the command line utility used to manage a kubernetes cluster?**
    - [ ] kube-api
    - [ ] kubelet
    - [ ] kubectrl
    - [x] kubectl
    - [ ] docker


### 2-5｜補充資料

- [Kubernetes Concepts](https://kubernetes.io/docs/concepts/) 



## CH 3｜Setup Kubernetes 
快速設置 K8S 的 N 個方法。


### 3-1｜Introduction and Minikube

<p class="illustration">
    <img src="https://i.imgur.com/GpryRLk.png" alt="Setup Kubernetes">
    Setup Kubernetes（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

這邊介紹幾個快速構建 K8S cluster 的方法：
1. **本地端**  
    想在筆記本電腦或虛擬機上自行設置，可以考慮 minikube 和 kubeadmin ...等。
2. **託管**  
    直接在 GCP 和 AWS ...等雲端環境中設置。
3. **課程提供的 Lab**  
    它這邊是貼 [Play with Kubernetes](https://labs.play-with-k8s.com/)，不過好像跟第一章的[永久的 Lab](https://kodekloud.com/courses/labs-kubernetes-for-the-absolute-beginners-hands-on/) 不太一樣？



### 3-2｜測驗 2：Setup Kubernetes

1. **Which of the below is an option to run Kubernetes in a Local Environment?**
    - [ ] kubectl
    - [ ] kube-server
    - [x] minikube
    - [ ] play-with-k8s

2. **Which of the below is an instant way of setting up a permanent kubernetes cluster on the cloud?**
	- [ ] kubeadm tool
	- [ ] minikube
	- [x] Google Container Engine (GKE)
	- [ ] play-with-k8s  
			This is instant, however the sessions are not permanent. They get deleted after 4 hours.
        
3. **Which of the below solution is used for setting up a multi-node Kubernetes cluster in a local environment?**
	- [ ] minikube
	- [x] kubeadm
	- [ ] Google Container Engine (GKE)
	- [ ] play-with-k8s       


### 3-3｜Demo：Minikube
這章節 Demo Minikube 的安裝，不過因為我之前安裝過了我就跳過了。但根據影片可以總結成 4 個步驟：
1. **安裝 kubectl**  
	影片中看來是用二進制檔安裝，不過安裝的指令看起來[安裝教學](https://kubernetes.io/docs/tasks/tools/)中所羅列的有差異，所以還是得重翻文件。另外，我在文件中看到可以用 `apt-get` 的方式來安裝
	
2. **確認本地端是否已安裝 VirtualBox**  
    因為 minikube 的特性是會在本地端起一個 VM，所以需要而外安裝 [VirtualBox](https://www.virtualbox.org/wiki/Downloads)。對了還要確定是否啟動虛擬化！

3. **安裝 Minikube**  
    一樣還是看[教學](https://minikube.sigs.k8s.io/docs/start/)吧！目前的安裝步驟看起來已經跟影片中有所差異了。

4. **執行測試**  
    在 minikube 上執行 hello-minikube app。

<br class="big">

是說，在教學文件中除了 minikube 外，還有 **kubeadmin** 的教學。不過我在影片中找不到這部分，但我想即便有應該也跟 minikube 一樣很多都不適用了，因此如果有要相關教學就看看這篇吧 - [〈Kubeadm｜Kubernetes〉](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)

在這被消失的課程中值得一提是它比較了 minikube 跟 kubeadmin 兩者間的差異，<mark>minikube 只能設置單一個 node 的 K8S cluster， kubeadmin 則是建立一個多node 的 cluster</mark>。

<p class="illustration">
    <img src="https://i.imgur.com/LgyrJVh.png" alt="minikube 跟 kubeadmin">
    minikube 跟 kubeadmin（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


### 3-4｜Google Kubernetes Engine (GKE)
是說，我沒有打算在 local 端安裝 minikube，但我也需要環境測試課程中的指令，所以我是去申請 [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine?hl=zh-tw)，基本上新客戶地一個月有 300 美元的扣打，夠這堂課程用了。

<p class="illustration">
    <img src="https://i.imgur.com/xYBglvz.png" alt="領取 $300 美元的免費抵免額立即試用">
    領取 $300 美元的免費抵免額立即試用
</p>

基本上進去後，點選 Kubernetes Engine 就能從 UI 上來建立 cluster，不然啟用 cloud shell 也可以藉由指令來啟動 cluster：

```shell
## 設定預設區域
$ gcloud config set compute/zone us-west1-a

## 建立 cluster，需花點時間
$ gcloud container clusters create kuar-cluster                                                                         
Default change: VPC-native is the default mode during cluster creation for versions greater than 1.21.0-gke.1500. To create advanced routes based clusters, please pass the `--no-enable-ip-alias` flag
Default change: During creation of nodepools or autoscaling configuration changes for cluster versions greater than 1.24.1-gke.800 a default location policy is applied. For Spot and PVM it defaults to ANY, and for all other VM kinds a BALANCED policy is used. To change the default values use the `--location-policy` flag.
Note: Your Pod address range (`--cluster-ipv4-cidr`) can accommodate at most 1008 node(s).Creating cluster kuar-cluster in us-west1-a... Cluster is being configured...working.

## 取得權限操作 cluster 的權限
$ gcloud container clusters get-credentials kuar-cluster --zone us-west1-a --project your-project-id
Fetching cluster endpoint and auth data.
kubeconfig entry generated for kuar-cluster.
```

<br>

建立完之後的 UI 長這樣：

<p class="illustration">
    <img src="https://i.imgur.com/lJNe5Vz.png" alt="GKE UI">
    GKE UI
</p>



## 其他連結
1. 課程內容：[Kubernetes for the Absolute Beginners - Hands-on](https://www.udemy.com/course/learn-kubernetes/)
2. 目錄： [【K8S Beginners 筆記】目錄](https://cynthiachuang.github.io/Kubernetes-for-the-Absolute-Beginners-Contents)



## 參考資料 
- 作者 (年代)。[文章文稱](網址)。檢自 網站 (2021-09-14)。
1. 協同撰寫。[Kubernetes](https://zh.wikipedia.org/zh-tw/Kubernetes)。檢自 維基百科 (2022-10-06)。
2. (2021-10-30)。[Docker 基本觀念與原理介紹](https://functional.style/docker/general/overview/)。檢自 點燈坊 (2022-10-06)。
3. JasonGerend, v-susbo, Et al.(2022-09-22)。[容器與虛擬機器](https://learn.microsoft.com/zh-tw/virtualization/windowscontainers/about/containers-vs-vm)。檢自 Microsoft Learn (2022-10-07)。
4. Jack in the world (2019-10-15)。[[CS] Docker的三個基本概念: Image, Container, 和Registry](https://mailtojacklai.medium.com/cs-docker的三個基本概念-image-container-和registry-89844595a7a6)。檢自 Jack in the world｜Medium (2022-10-07)。
5. Red Hat (2019-12-02)。[什么是容器编排？](https://www.redhat.com/zh/topics/containers/what-is-container-orchestration)。檢自 Red Hat (2022-10-14)。
6. The Kubernetes Authors (2022-10-02)。[Viewing Pods and Nodes](https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/)。檢自 Kubernetes  (2022-11-02)。
8. 被蛇咬到的魯卡 (2020-11-11)。[Kubernetes 教學 — 什麼是 Pod？什麼 Node ？搞的我好亂呀！](https://medium.com/starbugs/kubernetes-教學-一-概念與架構-954caa9b1558)。檢自 Starbugs Weekly 星巴哥技術專欄｜Medium (2022-11-02)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-11-09</summary>
  <ul>
    <li>2022-11-09 更新：新增 GKE、K8S 補充資料</li>
    <li>2022-11-03 發布</li>
    <li>2022-11-02 完稿</li>
    <li>2022-10-05 起稿</li>
  </ul>
</details>