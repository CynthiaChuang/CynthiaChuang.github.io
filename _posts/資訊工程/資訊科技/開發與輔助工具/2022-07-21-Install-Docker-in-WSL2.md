---
title: WSL｜在 WSL2 中安装 Docker 
date: 2022-07-20 01:59
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Windows/DOS
- Linux/Unix
- WSL
- 工具安裝與部署
- Docker
--- 

前幾天寫了份 Docker 的文件要給客戶，交稿前請 QA 幫忙校稿跟測試...原本一切順利，但突發奇想用了 WSL2 來安裝...結果就 :cry: 

<!--more-->


<p class="illustration">
    <img src="https://imgur.com/nMa3Drb.png"  alt="適用於 Linux 的 Windows 子系統">
    適用於 Linux 的 Windows 子系統（圖片來源: <a href="https://linuxiac.com/windows-subsystem-for-linux-explained-wsl-wsl2/">Linuxiac</a>）
</p>



## 問題描述
故事是這樣的，如果按照我提供的 Docker 安裝步驟，理論上坑品不錯的話應該能一次過。但... QA 的職業技能就是踩坑阿...阿不！是跳坑才對，這不馬上就開始發動技能了：

<p class="illustration">
    <img src="https://i.imgur.com/ANgeQmq.png"  alt="Docker is not running">
</p>

這個好解決，用 `service docker start` 把 service 起起來就好了：

<p class="illustration">
    <img src="https://i.imgur.com/4gxrHf9.png"  alt="service docker start">
</p>

但緊接著 QA 又丟給了我張截圖：

<p class="illustration">
    <img src="https://imgur.com/gKwlCVC.png"  alt="Cannot connect to the Docker daemon at unix:///var/run/docker.sock">
</p>

請她再次確認看一下目前 docker daemon 狀態，結果依舊顯示 not running。再排除系統重啟、service 重啟...等一些常見狀況與排除方法後，我們將目光放在了 WSL 這個環境上了...


## 安裝 WSL2
之前在曾經試著 [Win10 上安裝和使用 WSL](https://cynthiachuang.github.io/WSL-Install-and-Use-the-Ubuntu-subsystem-in-Win10)，所以這邊就跳過一些介紹，直接開始來裝 Docker 啦！恩...原本是這麼打算的啦，想說可以直接從 Docker 的安裝開始。但進入 Ubuntu 後，就看到這個：

<p class="illustration">
    <img src="https://imgur.com/BVTMZXb.png" alt="Docker 在 WSL1 中不支援需升級成 WSL2">
</p>

所以我還是先重新安裝 WSL2 吧。

### Step1｜確認 Windows 版本
若要安裝 WSL2，Windows 必須滿足一定的版本需求，對於 Windows 10 需符合：
- **x64**： 版本 1903 或更新版本，使用組建（Build Number） 18362 或更新版本。
- **ARM64**： 版本 2004 或更新版本，使用組建 19041 或更新版本。

或者直接使用 Windows 11。

<br class="big">

想要確認版本與組建號碼的話，按 `Win 鍵 + R` 並輸入 `winver`，就可以看到相關數字了：
 
<p class="illustration">
    <img src="https://i.imgur.com/v93FIjJ.png" alt="確認版本與組建號碼">
</p>

### Step2｜啟用 Windows 子系統 Linux 版和啟用虛擬機器功能
啟用 Windows 子系統 Linux 版，才能在 Windows 上裝任何 Linux 版本；而要升級到 WSL2 ，則必須多啟用虛擬機器平台功能。


在上次的步驟中，所用到的下列指令就是啟用 Windows 子系統 Linux 版：

```bash
$ Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

這次在官網上看到另外一種啟用指令，也跟啟用虛擬機器平台的指令一並貼出來：

```bash
# 啟用 Windows 子系統 Linux 版
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 啟用虛擬機器平台
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

<br class="big">

另外也可以藉由 UI 來啟動。開啟 `控制台 → 程式集 → 開啟或關閉 Windows 功能`，勾選其中「Windows 子系統 Linux 版」與「虛擬機器平台」。
 
<p class="illustration">
    <img src="https://i.imgur.com/3pyD5A3.png" alt="開啟或關閉 Windows 功能">
</p>

設定完後，得重新開機，才能套用設定。

<br class="big">

是說，我在許多教學中發現都會**啟用 Hyper-V**，因為 WSL2 是使用 Hyper-V 架構來實現虛擬化的，但在官方的教學過程中好像並沒有啟動 Hyper-V 的步驟？

剛好在知乎上有人提出[相關的討論](https://www.zhihu.com/question/439585675)，得到結論是<mark>不需要開啟 Hyper-V，但要啟用虛擬機器平台功能，因為這個虛擬機器平台就是一個精簡版的 Hyper-V</mark>。再根據[木頭龍的回答](https://www.zhihu.com/question/439585675/answer/2144365789)詳細來看，Hyper-V 可分成 2 部分：底層的虛擬機器平台及上層的虛擬機器管理軟體，這兩個功能在之前是**合併在一起的**，我想這有可能就是部分教學文章會說開啟 Hyper-V 的原因？


### Step3｜下載 Linux 核心更新套件
接下來更新 WSL2 Linux 核心套件，這步驟比較簡單[下載最新套件](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)後，點擊安裝就好。

不過，有一次我載完後忘記安裝就繼續往下做，等我要開起 Linux 時就得到了 `WslRegisterDistribution failed with error` :sweat_smile: ...

<p class="illustration">
    <img src="https://i.imgur.com/VBu3Ual.png" alt="忘了更新套件">
</p>


### Step4｜將 WSL2 設定為預設版本
開啟 PowerShell，將 WSL2 設為預設版本：

```bash
$ wsl --set-default-version 2
```

<p class="illustration">
    <img src="https://i.imgur.com/xPTqL2W.png" alt="成功將 WSL2 設為預設版本">
</p>


### Step5｜安裝並啟動 Linux
這邊就不提了，跟[之前](https://cynthiachuang.github.io/WSL-Install-and-Use-the-Ubuntu-subsystem-in-Win10)的下載與執行步驟一樣。如果只是要從 WSL1 升到 WSL2 應該是不用重裝才對，我單純只是為了網誌截圖才重裝的。


## 在 WSL 中安装 Docker 
我找到了兩種方式，可以在 WSL 中安装 Docker：


### 方法1｜安裝 Docker 搭配 Docker Desktop
在 WSL2 中安裝 Docker 最常見的方法是採用 Docker Desktop for Windows，這種方式是利用了 Docker 的 Client/Server 架構，由 Windows 這邊負責 docker.sock，WSL2 中則是只有 Client 端，並利用 Windows 的 Docker Server 端進行通訊。

#### ⎚ Step1｜安裝 Docker
因為我使用的 Ubunut 20.04 的 發行版本，所以它實際上已經有內建 `docker.io` 可以不用安裝了。

直接輸入 `docker` 指令，就會看到提示訊息建議我們到 Docker Desktop 設定 **WSL instegration**。

<p class="illustration">
    <img src="https://i.imgur.com/wLnXIoo.png" alt="建議我們到 Docker Desktop 設定 WSL instegration">
</p>

<br class="big">

不過，因為我要復現 QA 的操作，所以我還是把預載的 `docker.io` 給移除，重新按照[文件]( https://docs.docker.com/engine/install/ubuntu/)裝新版的 Docker。安裝好後，則是會直接看到 QA 反應的狀況：

<p class="illustration">
    <img src="https://imgur.com/gKwlCVC.png"  alt="Cannot connect to the Docker daemon at unix:///var/run/docker.sock">
</p>

基本上兩個問題都得去 Docker Desktop 做額外設定。


#### ⎚ Step2｜在 Docker Desktop 設定 WSL instegration
因為我的環境中已經裝有 Docker Desktop，所以就不截圖安裝步驟了，有需要的可以看看[文件](https://docs.docker.com/desktop/install/windows-install/)的說明。印象中還挺簡單，至少...我沒有踩到奇怪的坑 XDDD

<br class="big">

首先，開啟 Docker Desktop 上的  setting，並且在 General 中開啟 `Expose daemon on tcp://localhost:2375 without TLS`。

<p class="illustration">
    <img src="https://imgur.com/Z9khlXF.png" alt="開啟 Expose daemon on tcp://localhost:2375 without TLS">
</p>


接下來回到 WSL2 中設定環境變數，為了讓每次打開 bash 時，此變數都會生效，所以我把它寫到 `~/.bashrc` 並更新：
```bash
# 單次設定
$ export DOCKER_HOST=tcp://127.0.0.1:2375

# 寫入 bashrc 中，之後每次啟動都會被設定
$ echo DOCKER_HOST=127.0.0.1:2375 >> ~/.bashrc  
$ source ~/.bashrc
```


再次執行 `docker run`。理論上這一步應該就可以了，因為同事說他做到這步就 OK，但....

<p class="illustration">
    <img src="https://imgur.com/M7Kubev.png" alt="Cannot connect to the Docker daemon at tcp://127.0.0.1:2375">
</p>

<br class="big">

我不得要領，只能先按照[論壇上](https://stackoverflow.com/questions/48047810/cannot-connect-to-the-docker-daemon-on-bash-on-ubuntu-windows)的解法一一試過去：

1. 檢查 `Use the WSL 2 based engine`：這個我有開沒問題。
2. 關 Hyper-V 重新啟動，再次開啟 Hyper-V 並重啟：也失敗，因為我不確定他的版本，所以虛擬機器平台的啟動我也來一次，但一樣失敗。

就在我還懷疑我是不是 export 下錯位置，想說難道它其實不是 WSL2 的指令而是 Windows 的時候（雖然不太可能），我在[某層回覆](https://stackoverflow.com/a/70502511/9661744)看到了線曙光，他標題大大寫著 <mark>DO NOT USER OTHER METHODS IN LATEST DOCKER DESKTOP</mark>，並在內文提及了 **WSL Integration**。

難不成跟 Docker Desktop 的版號還有關？雖然不曉得發文者的版號是多少，但我很確定我的是最新的，因為我才剛被它拐騙更新而已。

<p class="illustration">
    <img src="https://imgur.com/Y4UuIl3.png" alt="Docker Desktop 的版號">
</p>

<br class="big">

我照著提示 `Settings > Resources > WSL Integration` 來到一個全新世界（誤）！在這我開啟了跟 Ubuntu 20.04 的連線，並重啟 Docker Desktop。

<p class="illustration">
    <img src="https://i.imgur.com/pZdTTH9.png" alt="WSL Integration">
</p>


然後回到 WSL2 再次測試 `docker run` 指令，然後我就看到令人感動的結果啦~！開心的我立刻登入 Stack Overflow 幫他推一個，這個答案包含我才兩個人推而已 XDDD
<p class="illustration">
    <img src="https://i.imgur.com/4KdNtUd.png" alt="docker run 成功執行">
</p>

對了，如果是新的 Docker Desktop，那麼 `Expose daemon on tcp://localhost:2375 without TLS` 不用開、環境變數也不用設定。


### 方法2｜使用便捷腳本安裝 Docker
這是我在 [〈Does WSL2 docker need windows docker desktop?〉](https://superuser.com/questions/1583786/does-wsl2-docker-need-windows-docker-desktop) 這個提問中看到的，原本是想了解為何需要依賴 Windows 的 Docker Desktop，結果沒想到發現另外一個解法。

在之前安裝 [Docker 的文件](https://docs.docker.com/engine/install/ubuntu/)中有提到，安裝 Docker 有 repository、 package 跟 convenience script 3 種方法，習慣上我是使用 repository 來安裝的。不過它這邊用 convenience script 來安裝：

```bash
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
$ sudo service docker start
```

測試 `docker run` 可以順利進行耶！


<br class="big">

回到一開始的問題**為何需要依賴 Windows 的 Docker Desktop**？[Survey 得到結論](https://ithelp.ithome.com.tw/articles/10235035)是因為<mark>有了 Linux Core 所以 Windows Docker Desktop 可以不需要，但跟 Windows 的整合就會差一點</mark>。



## 參考資料 
1. (2022-06-28)。[舊版 WSL 的手動安裝步驟](https://docs.microsoft.com/zh-tw/windows/wsl/install-manual)。檢自 Microsoft Docs (2022-07-19)。
2. (2022-06-28)。[Install Windows Subsystem for Linux (WSL) on Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install)。檢自 Microsoft Docs (2022-07-19)。
3. (2021-02)。[wsl 2 是否需要启用 Hyper-V？](https://www.zhihu.com/question/439585675)。檢自 知乎 (2022-07-19)。
4. Les Lee (2019-04-26)。[在wsl-中安裝-docker](https://medium.com/一個小小工程師的隨手筆記/wsl-與-windows-的完美雙結合-在wsl-中安裝-docker-e722e87ffa3b)。檢自 一個小小工程師的隨手筆記｜Medium (2022-07-19)。
5. sam-liaw (2020)。[WSL 安裝 Docker](https://hackmd.io/@sam-liaw/ByiB16T0S)。檢自 HackMD (2022-07-19)。
6. shreyansp (2021-12-28)。[Cannot connect to the Docker daemon on bash on Ubuntu windows](https://stackoverflow.com/a/70502511/9661744)。檢自 Stack Overflow (2022-07-19)。
7. kdm.J (2020-09-08)。[Does WSL2 docker need windows docker desktop?](https://superuser.com/questions/1583786/does-wsl2-docker-need-windows-docker-desktop)。檢自 Super User (2022-07-19)。
8. 我思故我在 (2020-09-08)。[5天-搞清楚為何 WSL2 需不需要 Windows Docker Desktop](https://ithelp.ithome.com.tw/articles/10235035)。檢自 iT 邦幫忙 (2022-07-19)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-07-20</summary>
  <ul>
    <li>2022-07-20 發布</li>
    <li>2022-07-20 完稿</li>
    <li>2022-07-18 起稿</li>
  </ul>
</details>