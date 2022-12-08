---
title: Genymotion 模擬器安裝篇：In ubuntu14.04 LET
date: 2016-03-20
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- IDE/Simulator
- Android
- 工具安裝與部署
--- 

雖然我比較喜歡實機測試，有種詭異的滿足感 (๑ˉ∀ˉ๑)，好啦其實我的 RAM 不太夠，跑模擬器會卡到死，所以這週末就衝去買了一條回來，不然其他螢幕的情況都沒辦法測。
  
只是...買回來後，開模擬器還是好卡 (〒︿〒)，只好安裝其他模擬器試試看，山姆大推薦用 Genymotion 聽說比較好用。

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/TTwPWQc.png">
    genymotion_eyecatch （圖片來源: <a href="https://www.google.com/url?sa=i&url=http%3A%2F%2Fwww.testrisk.com%2F2016%2F03%2Fmobile-application-testing-change-host.html&psig=AOvVaw2T8Xe4HV0w8PKGv0dXUWqA&ust=1597463854717000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCNDayN7mmesCFQAAAAAdAAAAABAD">testrisk</a>）
</p>



## 安裝步驟


### 1. 安裝 Oracle Virtual Box
因為我是在 Ubuntu 上開發，所以必須先安裝 Oracle Virtual Box 才能安裝 Genymotion：

1. **下載 Virtual Box**   
    來到 [Virtual Box 官網]( https://www.virtualbox.org/)後，點選 Downloads 頁面，選擇 VirtualBox 的版號與作業系統，在依照你作業系統的版本選擇適合的 deb 檔下載。

    在 Ubuntu14.04 LET 中，這兩個選項我分別選擇：
    1. VirtualBox 5.0.16 for Linux hosts 
    2. Ubuntu 14.04 ("Trusty") / 14.10 ("Utopic") / 15.04 ("Vivid") AMD64
    <p class="illustration">
    <img src="https://i.imgur.com/FxNSO6O.png">
    </p>    
    
    <p class="illustration">
    <img src="https://i.imgur.com/psZKhX0.png">
    </p>    

<br class="big"> 

2.  **下載 Virtual Box**   
    下載到合適的 deb 檔後，點選右鍵選單中的<mark>以 Ubuntu 軟體中心開啟</mark>來安裝即可。

  
### 2.  安裝 Genymotion
[Genymotion](https://www.genymotion.com/) 有提供免費版的模擬器進行下載，但必須先註冊，所以建議在一開始先註冊再進行其他動作，避免在過程中跳掉。

1. **註冊 Genymotion 帳號**   

2. **下載 Genymotion**   
    註冊完帳號後，先回到首頁，點選首頁正中間的 choose paln 按鈕，選擇個人的免費版本，即 Individual 中 BASIC 標籤，點選 Get started 開始下載。

    <p class="illustration">
        <img src="https://i.imgur.com/87aYyrA.png">
    </p>

    <p class="illustration">
        <img src="https://i.imgur.com/IjSSoLr.png">
    </p>
    
   <br class="big">  
      
2. **安裝 Genymotion**   
    啟動終端機，先切換到下載的檔案（genymotion-2.6.0-linux_x64.bin）所在的目錄，再輸入下列兩條指令：
	```shell
	$ chmod +x genymotion-2.6.0-linux_x64.bin
	$ ./genymotion-2.6.0-linux_x64.bin
	``` 
    <p class="illustration">
    <img src="https://i.imgur.com/TVMBBxb.png">
    </p>
    <br class="big"> 

3.  **執行**   
    當安裝完成後，會在資料夾中多了一個名為 genymotion 的資料夾，進入點擊 genymotion 執行檔，即可執行。


### 3.  設定 Virtual machine
1. **設定新的機器**   
   當點擊進入後會問你要不要新增新的機器，點 Yes。
   
2. **登入**   
   在進行下一步前要先登入剛剛在網站註冊的帳號。
   
3. **選擇機型**   
   選擇你所要的機型，進行安裝。
    
4. **執行**   
   最後讓它 run run 看能不能執行。


### 4. debug 時間...
關於執行時的錯誤訊息，我收過三種，不過嚴格說來我弄掉的只有第一個，其他兩個我也還有點莫名其妙：  

1. **Virtualization technology not enabled in bios**   
    顧名思義就是 Virtualization technology 沒開，在重新開機後進入 BIOS 介面 → Advanced BISO Feature → Virtualization 改成 enable 即可  
    P.S. 我的是 Award 主機板，各個主機板的開啟方法可能會不同。
    <br class="big">    
2.  **The Virtual device got no IP address**   
    這個我在懷疑是因為ㄧ開始 Virtualization 沒開，所以才會取不到 IP address，因為我將 Virtualization 啟動後，就正常了。  
    如果 Virtualization 開啟後還出現這問題，試試這個 [Genymotion, Fix Error "Could Not Obtain An IP Address"](https://www.youtube.com/watch?v=YuJ6ZfudFp8%20%20%20genymotion)
    <br class="big">    
3.  **Unable to configure the network adapter for the virtual device**   
    會出現這條問題最基本原因是，我使用的指令來安裝，但貌似安裝的是舊的？還是因為我忘記多打 update 的指令，我就不得而知了，總之我將 virtualbox 改成自己下載安裝的就算 ok了，接下來會跳出 Virtualization 沒開的問題  
	```shell
	$ sudo apt-get install virtualbox
	```



## 使用方法
安裝完 Genymotion 後，最重要的就是讓它可以在 Android Studio 中使用啦～


### 1. 開啟 plugins 
在 Android Studio中，依序點選 File → Settings lugins→ plugins → Browse repositories

<p class="illustration">
<img src="https://i.imgur.com/jVCII63.png">
</p>


### 2. 安裝 Genymotion plugins
在搜尋攔輸入 Genymotion 後就會跳出，然點選安裝即可。 

<p class="illustration">
<img src="https://i.imgur.com/Ej5Qzlo.png">
</p>

    
### 3. 設定路徑  
安裝完 plugins 後重新啟動 Android Studio 就會發現多了Genymotion 的圖示。

<p class="illustration">
<img src="https://i.imgur.com/vDtPD51.png">
</p>

點選後進入設定路徑，將路徑設定為剛剛 genymotion 執行檔所在的目錄即可，這樣下次執行時就會看到有模擬器可選了。



## 小記  
安裝完後才想到，我用的是免費版，所以公司用途不在授權範圍內 QAQ，所以還是要想辦法處理掉 KVM 問題，不然叫公司買 Business 版？？



## 參考資料
1. [How to Install Genymotion Android Emulator in Ubuntu Linux｜Youtube](https://www.youtube.com/watch?v=k3MSTD9SLy4)
2. [Virtualization not enabled in BIOS?｜Stack Overflow](http://stackoverflow.com/questions/27884846/virtualization-not-enabled-in-bios)
3. [[Android] Android Studio 安裝 Genymotion 模擬器｜阿斌的筆記](http://aiur3908.blogspot.tw/2015/04/android-android-studio-genymotion.html)


