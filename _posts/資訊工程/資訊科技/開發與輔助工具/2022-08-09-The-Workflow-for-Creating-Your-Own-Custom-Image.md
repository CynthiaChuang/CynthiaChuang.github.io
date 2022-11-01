---
title: "客製化自己的 Docker 映像檔流程"
date: 2022-10-15
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Docker
- Linux/Unix
--- 

這篇網誌中，我們會從無到有逐步建立一份客製映像檔，內容包括：Docker 安裝與配置、基礎映像檔下載、撰寫 Dockerfile，再透過 `docker build` 建立成映像檔，最後將映像檔上傳至倉庫。

這篇算是 [〈WSL｜在 WSL2 中安装 Docker〉](https://cynthiachuang.github.io/Install-Docker-in-WSL2/)的前篇？原本是要寫給客戶的教學文件，不過我覺得拿出來當網誌好像也還行？所以就稍微擴寫、改述並刪除了些會被我主管請去喝茶東西，~~看來應該還能騙到一篇！？~~
<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/mlrB0F5.png" alt="Docker 三元素：Dockerfile、Docker 映像檔以及 Docker 容器">
    Docker 三元素：Dockerfile、Docker 映像檔以及 Docker 容器（圖片來源: <a href="https://medium.com/platformer-blog/practical-guide-on-writing-a-dockerfile-for-your-application-89376f88b3b5">Platformer — A WSO2 Company｜Medium</a>）
</p> 


<div class="alert warning">
<div class="head">環境確認</div>
本說明採用的環境是 <strong>Ubuntu 20.04</strong>，與 docker 相關之指令應可在多數環境中執行。但考慮到環境可能性眾多，可能遇到的情況族繁不及備載，因此執行本說明時有遇到問題請查閱 Docker 官方文件。
</div>



## Step1｜Docker 環境確認
既然是客製化 Docker 映像檔，當然得先裝有 Docker 啦！可以先確定版號的指令，來確認開發環境是否安裝有 Docker：
```bash
$ docker version
```

若果正確安裝的話，應該看到相關 Client 與 Server 的版號資訊，類似這樣：
```bash
Client:
 Version:           20.10.12
 API version:       1.41
 Go version:        go1.16.2
 Git commit:        20.10.12-0ubuntu2~20.04.1
 Built:             Wed Apr  6 02:14:38 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.12
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.2
  Git commit:       20.10.12-0ubuntu2~20.04.1
  Built:            Thu Feb 10 15:03:35 2022
  OS/Arch:          linux/amd64
  Experimental:     false
```

<br>

否則會得到 docker 指令不存在的錯誤訊息： 
```bash
$ docker version
bash: docker: command not found
```

如果已經安裝的話，可以直接跳到 [1-2 章節](#step1-2確認服務是否啟動)來確認服務是否啟動；若是沒有安裝的話，就跟我一起來裝 Docker 吧！
 
 
### Step1-1｜安裝 Docker
我的開發環境是 Ubuntu 20.04 ，裡面已經有安裝 Docker 了，但我發現 Docker 似乎有新版本，所以打算順便升級 Docker 來玩玩。

在這邊我先移除了舊版的的相關套件，再用**設置 Docker 的儲存庫（Repository）** 的方式來安裝。這種安裝法會方便於安裝與升級，這也是 Docker 官方推薦的安裝方式。

1. **移除舊版 Docker**  
    ```bash
     sudo apt-get remove docker docker-engine docker.io containerd runc
    ```

2. **設置 Docker 的儲存庫**  
    相比以前一行安裝 docker.io，新的版本的多了許多的前置步驟：
    - 更新 apt-get 索引，並安裝用 HTTPS 傳輸的套件以及 CA 證書
        ```bash
        $ sudo apt-get update
        $ sudo apt-get install \
                       ca-certificates \
                       curl \
                       gnupg \
                       lsb-release
        ```    
    - 添加 Docker 的官方 GPG key
        ```bash
        $ sudo mkdir -p /etc/apt/keyrings
        $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg 
        $ sudo chmod a+r /etc/apt/keyrings/docker.gpg
        ```     
    - 然後，我們需要向 sources.list 中添加 Docker 儲存庫  
        ```bash
        $ echo \
          "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
          $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
        ```
        
3. **安裝 Docker Engine**  
    最後再次更新 apt-get 索引，並才能安裝最新版本的 Docker Engine、containerd 和 Docker Compose。
    ```bash
    $ sudo apt-get update
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    ```


### Step1-2｜確認服務是否啟動
安裝完成後，查看一下 Docker 服務是否有正常啟動：
```bash
$ sudo service docker status
```
或是
```bash
$ sudo systemctl status docker 
```

<br>

如果正常啟動的話，可以看到綠色的 active 狀態：

<p class="illustration">
    <img src="https://i.imgur.com/7TCVsZr.png" alt="正常啟動會顯示綠色的 active">
    正常啟動會顯示綠色的 active
</p> 

若是未啟動，則啟動 service：
```bash
$ sudo service docker start
```
或是
```bash
$ sudo systemctl start docker
```

<br>

最後，確認是否能夠下載測試映像檔並在容器中運行它：

```bash
$ sudo docker run hello-world
```
 
若是成功執行會印出 `Hello from Docker` 的訊息並關閉容器：

<p class="illustration">
    <img src="https://i.imgur.com/4Xf0bop.png" alt="Hello from Docker">
    Hello from Docker～
</p> 


### Step1-3｜以非 root 身份管理 Docker 
在一般情況下，`docker` 指令需要 root 權限，其他使用者則必須使用 `sudo` 才能操作 `docker` 指令。因此如果不想每次都得在 `docker` 指令前加上 `sudo`，就得將使用者添加一個名為 docker 的群組中。


首先先建立一個名為 docker 的群組，並將使用者添加到其中。當 Docker 守護進程啟動時，它會建立一個可供 docker 群組成員訪問的 Unix domain socket。
```bash
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
```

更新使用者群組並重啟 docker：
```bash
$ newgrp docker 
$ service docker restart    
```

最後，試試看可不可以在沒有 `sudo` 下執行 docker：
```bash
$ docker run hello-world
```



## Step2｜下載基礎映像檔
若要客製自己的映像檔時，我們必須先選定一個**基礎映像檔（Base Image）**，再依此映像檔，加入所需開發套件和功能，層層堆疊逐步形成特定用途的客製映像檔。

該基礎映像檔的來源，可以是任何公開的映像檔倉庫（Public Image Registry），如 [Docker Hub](https://hub.docker.com/) 或是 [NVIDIA NGC](https://catalog.ngc.nvidia.com/?filters=&orderBy=scoreDESC&query=container)，裡面存放了數量龐大的映像檔可供使用者下載；當然，也可以是自己或是組織建置的私有倉庫（Private）。


### Step2-1｜從公開倉庫取得映像檔
從公開倉庫取得映像檔是較常見的方式，通常無須額外權限與設定，就可直接使用下載指令。

以下載 [Jupyter Notebook](https://hub.docker.com/r/jupyter/datascience-notebook) 為例，我們從先在 Docker Hub 上確認所要下載的映像檔與標籤後，就直接使用指令將映像檔下載到本地端。

```bash
$ docker pull jupyter/datascience-notebook:latest
```

下載完成後會出現摘要與狀態提示下載成功：

<p class="illustration">
    <img src="https://i.imgur.com/1v8yKkF.png" alt="提示下載成功">
    提示下載成功
</p> 


### Step2-2｜從私人倉庫取得映像檔
若要從私有倉庫取得映像檔，則必須先**登入**該倉庫。

```bash
$ docker login -u {Username} -p {Password} {Registry}
Login Succeeded
```

`docker login` 可以登入到指定倉庫，如果未指定倉庫位址，則會預設登入官方的公開倉庫 Docker Hub。

<br>

完成登入後，後續的操作可能會與公開倉庫取的存取步驟有所差異，但整體流程差不多：
1. 確認所要下載的映像檔與標籤
2. 使用指令將映像檔下載到本地端


## Step3｜撰寫 Dockerfile
挑選完基礎映像檔後，就可根據自身需求撰寫 Dockerfile 加入所需套件和功能。

在撰寫 Dockerfile 時，是以 **FROM 指令**為開頭，往下根據每一行定義會製作出的一個唯讀資料層，當啟動執行成容器時，才會再疊上寫入層。因此建議撰寫時盡可能刪除每層不必要的資料，以降低映像檔大小。

我這邊就以剛剛從取得映像檔為基礎，向上安裝了 <mark>h5py</mark>、 <mark>pandas</mark> 與 <mark>gensim</mark>...等 Python 套件，以提供一個簡單的 Dockerfile 範例，方便後續說明的進行。

想要詳細了解如何撰寫 Dockerfile 的話，還是建議去看看官方文件 -  [〈Best practices for writing Dockerfiles〉](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)，絕對不是因為我不想寫文件了 XDDD

```dockerfile
From jupyter/datascience-notebook:latest

RUN pip install --no-cache-dir  --upgrade pip \
    && pip install --no-cache-dir \
        "h5py==3.7.0" \
        "pandas==1.4.3" \
        "gensim==4.2.0" \
    && rm -rf ~/.cache/pip
```

撰寫完成後，將其進行存檔並命名為 `Dockerfile`。


## Step4｜建立映像檔
完成 Dockerfile 的撰寫後，就可以用 `docker build` 的指令將映像檔建立起來。

```bash
$ docker build -t demo-push:v1 .
```

其中，`-t` 是為映像檔進行命名 **`demo-push`** 為倉庫與映像檔名稱、 **`v1`** 則是標籤，請依照習慣自行命名。指令執行時，它會找尋目前資料夾中名為 Dockerfile 的建置文件；但如果你的建置文件並非取名為 Dockerfile，這時需要在指令中加上 `-f` 指定建置文件：

```bash
$ docker build -t demo-push:v1 -f MyDockerfile .
```

如果需要更多關於 docker build 的介紹，請閱讀 [Docker 官方文件](https://docs.docker.com/engine/reference/commandline/build/)。


<br>

<div class="alert warning">
<div class="head">符號注意</div>
注意！兩個命令的末尾都有一個點 `.`
</div>

<br>
 
當建立成功後可在終端機上看到下列訊息：
```bash
Removing intermediate container cc2418f30dff
 ---> 2c7464ff7613
Successfully built 2c7464ff7613
Successfully tagged demo-push:v1
```

<br>

<div class="alert info">
<div class="head">補充說明｜docker build 命令後的點號（.）</div>
它並不是用於指定 Dockerfile 文件所在的位置的！  <br>
它並不是用於指定 Dockerfile 文件所在的位置的！  <br>
它並不是用於指定 Dockerfile 文件所在的位置的！  <br><br>

重點說三次！在這邊 <code>.</code> 確實是用來表示目前目錄，但卻不是用在指定 Dockerfile 所在路徑，因為那是 <code>-f</code> 的守備範圍！  

<br><br>

那麼指定這目錄的目的？這與 Docker 的運行架構有關：

<p class="illustration">
    <img src="https://i.imgur.com/i2do1xG.png" alt="Docker daemon 與 client 的交互運作">
    Docker daemon 與 client 的交互運作（圖片來源: <a href="https://ithelp.ithome.com.tw/articles/10236066">iT 邦幫忙</a>）
</p> 

在 Docker 的運行架構中，存在 Docker Daemon（服務端守護進程）和 Client 兩個部分。Docker Daemon 會提供一組名為 Docker Remote API 的 REST API；而 Client 的代表就是我們常用的指令，指令則會透過 Docker Remote API 與 Docker Daemon 互動，從而完成各種功能。<br><br>
 
簡而言之， Docker 的運行架構就是個標準的 C/S 架構。<br>

<br>

那麼這邊又產生了一個問題，如果在建構映像檔時有涉及 <code>COPY</code>、 <code>ADD</code>...等操作文件的指令時，因為並非在本地建構，而是在服務端建構的，那麼在 C/S 架構中， Docker Daemon 該如何獲得本地檔案？<br>

<br>
 
這邊就有一個 <strong>映像檔建構上下文（Context）</strong>的概念。當建構的時候，使用者會指定建構映像檔上下文的路徑，而 docker build 指令再得知這路徑後，會將這路徑下的所有內容打包上傳給 Docker Daemon。而 Docker Daemon 在收到打包的內容後，會將它展開就能獲得建構映像檔所需的一切檔案。<br>

<br>

那回到一開始的問題 <strong>docker build 命令後點號（.）的意思</strong>，指得就是將目前的目錄下的所有內容打包上傳給 Docker Daemon。
</div>
 
 
## Step5｜上傳映像檔
當成功建構映像檔後，若要將它發布出去，可以將其上傳至 Docker Hub。

<p class="illustration">
    <img src="https://i.imgur.com/vRaJFcV.png?1" alt="Docker Hub Icon">
    Docker Hub Icon
</p>


### Step5-1｜註冊與登入
雖先前從 Docker Hub 下載映像檔到本地端時，並不需要登入即可下載；但若要上傳映像檔必須完成登入：

```bash
$ docker login -u {Username} -p {Password}
Login Succeeded
```

如果沒有帳號則必須先前往官網註冊。


### Step5-2｜創建儲存庫（Create Repository）

<p class="illustration">
    <img src="https://i.imgur.com/ZBsYe54.png" alt="Create Repository">
    Create Repository
</p>

如果沒有現有的儲存庫，可藉由 UI 的填寫建立一個新的儲存庫：

<p class="illustration">
    <img src="https://i.imgur.com/7cDFwlx.png" alt="填寫資料建立儲存庫">
    填寫資料建立儲存庫
</p>

在建立完成後可以得到一個 push 的指令，其中 tagname 需換成自定義的標籤，我們這邊使用 v1：
```bash
$ docker push cynthia/demo-push:v1
```


### Step5-3｜本地端映像檔重命名
在上一步驟可以發現從倉庫取得的指令中，其：
- **倉庫與映像檔名稱**： `{DockerHub UserName}/{ImageName}`
    - 倉庫路徑（RegistryURL）: `cynthia`
    - 映像檔名稱（ImageName）: `demo-push`
- **標籤**：`v1` 

若此時直接使用指令上傳，會得到映像檔不存在錯誤。

```bash
$ docker push cynthia/demo-push:v1
The push refers to repository [cynthia/demo-push]
An image does not exist locally with the tag: cynthia/demo-push
```

<br>

這是因為剛剛在建立映像檔時，所使用的倉庫與映像檔名稱為 `demo-push`，而非 `cynthia/demo-push`，因此需要先重命名：
```bash
# docker tag {local_name}:{local_tag} {remote_name}:{remote_tag}
$ docker tag demo-push:v1 cynthia/demo-push:v1
```   

### Step5-4｜上傳至倉庫

重新命名後，再使用上傳指令進行上傳：
```bash
docker push cynthia/demo-push:v1
```

<br>

上傳完成後，在終端機與平台上都可以看到相對應的提示：


<p class="illustration">
    <img src="https://i.imgur.com/bj21oeR.png" alt="">
</p>

<p class="illustration">
    <img src="https://i.imgur.com/mW7xBoS.png" alt="">
</p>



## 參考資料 
1. [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)。檢自 Docker Documentation (2022-07-13)。
2. Xu Sheng (2018-03-09)。[docker build命令后 . 号的意思](https://www.xuxusheng.com/post/docker-build命令后-号的意思)。檢自 XuSheng's Blog (2022-07-13)。
3. Baohua Yang (2022-05-12)。[使用 Dockerfile 定制镜像](https://vuepress.mirror.docker-practice.com/image/build/#镜像构建上下文-context)。檢自 Docker 从入门到实践 (2022-07-13)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-10-15</summary>
  <ul>
    <li>2022-10-15 發布</li>
    <li>2022-08-09 完稿</li>
    <li>2022-07-13 起稿</li>
  </ul>
</details>