---
title:  WSL：在 Windows 10 中安裝和使用 Ubuntu 子系統
date: 2022-05-06 02:00
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Windows/DOS
- Linux/Unix 
- WSL
- 工具安裝與部署
--- 

實在不習慣在 Windows 開發的一些設定，而之前安裝的雙系統又被我洗掉了，因此打算來安裝 VM。不過就在此時我同事說或許可以安裝 WSL？

<!--more-->
<br>
<center> <img src="https://i.imgur.com/5PJDakX.png" alt="適用於 Linux 的 Windows 子系統"></center>
<center  class="imgtext">適用於 Linux 的 Windows 子系統（圖片來源: <a href="https://docs.microsoft.com/zh-tw/windows/wsl/"  class="imgtext">Microsoft Docs</a>）</center>
<br> 

## WSL 

WSL 是 Windows Subsystem for Linux 的縮寫，是由 Microsoft 與 Canonical 公司開發的相容 Linux 底層核心的接口，不細究其原理的話，使用上的感覺像是就好像是在 Docker 而且還是把整台電腦所有儲存空間都掛載進去的那種 XDDD

相較傳統 VM 跟 Docker，它不需要額外安裝虛擬機與容器，資源也沒那麼吃緊。這對 Linux 的初學者以及像我這種輕量使用的情境，它算是個超棒的工具。

另外一個對我來說的優點，就是它的終端機是用 Unix 指令，我再也不用擔心記不住 DOS 指令了XDDD
 
 
<br><br> 

## 在 Windows 安裝 Ubuntu 

有 Window 的介面輔助下，安裝起來其實相當方便快速：

1. **用系統管理員身分執行 Windows PowerShell**  
    在 Win10 中已經有內建 Windows PowerShell，你可以從開始選單旁的搜尋欄輸入 **Windows PowerShell** 找到它，再以系統管理員身分執行它。

    <center> <img src="https://i.imgur.com/qNTogRI.png" alt="Windows PowerShell 在開始選單"></center>
    <br> 
    
    是說 PowerShell 的預設顏色真的有夠醜，字也很小。所以我開啟後的第一件事就是幫它換個顏色，順便把字體調大。
    <center> <img src="https://i.imgur.com/wcyaqj0.png" alt="Windows PowerShell"></center>
    <br> 
   
    應該會注意到，打開 PowerShell 時會上面有一行字：
    > Copyright (C) Microsoft Corporation. 著作權所有，並保留一切權利。  
    > 請嘗試新的跨平台 PowerShell https://aka.ms/pscore6  

    看起內建的 PowerShell 不是最新版的，雖說不升級不會怎像，但若真的很想弄到這行字的話，可以從 GitHub 下載安裝套件，詳細說明可參考[文件](https://docs.microsoft.com/zh-tw/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.1)：

    - [v7.2.0 Release of PowerShell](https://github.com/PowerShell/powershell/releases)
 
<br> 

2. **安裝 WSL**  
    在終端機中輸入下列指令：
    ```
    $ Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
    ```
    <br> 

    進度條跑完後請按照指令提示重新啟動電腦：
    ```    
    是否要立即重新啟動電腦以完成此操作?  
    > [Y] Yes  [N] No  [?] Help (default is "Y"):    
    ```
    <br>
    
    重啟後，再次啟動 PowerShell，測試是否安裝成功：
    ```    
    $ wsl  
    Windows 子系統 Linux 版沒有任何已安裝的發行版本。  
    您可以透過瀏覽 Microsoft Store 來安裝各種發行版本:  
    https://aka.ms/wslstore       
    如果看到提示訊息代表安裝成功，可以開始來安裝所需要德 Linux 發行。
    ```
<br> 

3. **安裝 Linux 發行**  
    在這邊遇到了點小問題，如果我按照剛剛的提示訊息連接到 [https://aka.ms/wslstore](https://aka.ms/wslstore)，會被向 Microsoft Store，但卻會得到下列訊息：
    ```   
    Wait a while, then try again. Something happened on our end.
    ```
    <br>

    所以我改在 Microsoft Store 中直接輸入 **Ubuntu**，不過如果直接輸入搜尋的話，需要注意 WSL 相容問題；或者考以直接使用網友提供的[連結](ms-windows-store://search/?query=WSL)，找到與 WSL 相容的 Ubuntu 版本，我這邊選擇了了目前正在使用的 Ubuntu20.04：

    <center> <img src="https://i.imgur.com/DP2Ls0Y.png" alt="Ubuntu 20.02"></center>
    <br> 

<br><br> 

## 執行 Ubuntu

在安裝好發行版本之後，就可以準備第一次的啟動，可以在 Microsoft Store 中找到剛剛安裝的 Ubuntu20.04，點擊畫面右上角的「啟動」按鈕。
<center> <img src="https://i.imgur.com/wGNVSc9.png" alt="Ubuntu 20.02 啟動"></center>
<br> 

或者你也可以在開始選單中，點擊剛剛安裝的 Ubuntu20.04：
<center> <img src="https://i.imgur.com/Jr6PicO.png" alt="Ubuntu 20.02 啟動2"></center>
<br> 

<br>

第一次啟動大概會花上幾分鐘的時間進行初始化設定，系統會要求提供 username 與 passward：
<center> <img src="https://i.imgur.com/GMIWpQG.png" alt="Ubuntu"></center>
<br> 
<br>

安裝完成後，可以打開 Windows PowerShell，輸入：
```
 $ wsl --list 
 Windows 子系統 Linux 版發佈:
 Ubuntu-20.04 (預設值)         
```
你可以看到已經完成安裝的子系統。

<br>

下次再次執行時，除了從開始選單點擊 Ubuntu20.04 的 icon 外，你也可以在 PowerShell 中輸入：
```
$ wsl 
```


<br><br> 

## 重設與解除安裝 Ubuntu

如果你再需要使用子系統了，只需要前往「應用程式與功能」把 Ubuntu 解除安裝即可，就像是一般的軟體的解除安裝：
<center> <img src="https://i.imgur.com/CjBhdGB.png" alt="Ubuntu 解除安裝"></center>
<br> 

<br> 

如果你還想要使用子系統，但嫌環境太髒想要重裝，也可以幫它「回復成出廠預設值」。一樣前往「應用程式與功能」，選擇 Ubuntu，會注意到有一個進階選項。
<center> <img src="https://i.imgur.com/pltdetH.png" alt="Ubuntu 進階選項"></center>
<br>

點選進階選項，找到裡面的重設，如此它就會將整個安裝給清掉了：
<center> <img src="https://i.imgur.com/CjBhdGB.png" alt="Ubuntu 重設"></center>
<br> 

用 PowerShell 檢查下，可以發現子系統被清掉了，
```
$ wsl  -l  --all
Windows 子系統 Linux 版沒有任何已安裝的發行版本。
您可以透過瀏覽 Microsoft Store 來安裝各種發行版本:
https://aka.ms/wslstore        
```
接下來只要重複[前一章](#執行-ubuntu)的執行步驟重來一次就好了。

<br>

另外還有一個方法可以重設，雖然文件中說這叫「取消登錄發佈」，但我目前看不出兩者的差異，所以還是稱之為重設吧 XDDD 這個方法就不用去設定了，在 PowerShell 就可以了：

```
$ wsl --unregister Ubuntu20.04   
```

<br><br>

## 儲存路徑

在使用時我有兩個問題：
1. 如何在存取 Windows 中的檔案？
2. Home 目錄下的資料又會存在 Windows 中的哪個位置？

<br>

找了下，Windows 的檔案會被掛載在 `/mnt/` 下，例如 D 槽就會被掛載在 `/mnt/d`。Home 目錄中的資料則存在 C 槽的 user 資料中，完整路徑如下：
> C:\Users\%UserName%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\rootfs

不過 `CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc` 這個會依照 Linux 系統版本而有所不同，所以可能要注意找一下。

<br>

在打這段的時候忽然想到，我能不能將 Home 目錄資料放到 D 槽？說道立刻就去查，不過看起來好像不行，因為[文件](https://docs.microsoft.com/zh-tw/windows/wsl/troubleshooting)上提到：「<mark>適用於 Linux 的 Windows 子系統只會在系統磁碟機(通常是 C)上執行</mark>」，因此看來是不能換了。


<br><br> 

## 錯誤

為了寫網誌的時候，為了截圖看錯誤訊息，所以我反覆重設又啟動，結果在某次重起的時候就遇到了這個錯誤：
> WslRegisterDistribution failed with error: 0x800700b7
> The distribution installation has become corrupted.
> Please select Reset from App Settings or uninstall and reinstall the app.
> Error: 0x800700b7 ????????????????

不過這問題還滿好處理的了，訊息都寫了，卸掉重裝就好了。

<br><br> 


## 後續

執行到現在，其實就已經差不多了，就像是一個 Ubuntu 的 Docker。不過如果在找資料的時候有發現 WSL + Linux GUI 的資料，所以我下次應該會來看看能不能安裝成功，就靜待我的好消息吧 XDDD

<br><br> 

## 參考資料 
1. Chen Do (2020-06-23)。[【工具】win10 安裝 ubuntu 子系統（WSL） 並安裝圖形化界面](https://www.twblogs.net/a/5ef12eb211be511c3893ecee)。檢自 台部落 (2021-11-05)。 
2. 辛比 (2019-07-28)。[[推薦] WSL (Windows Subsystem for Linux) 安裝與使用教學](https://xenby.com/b/226-%E6%8E%A8%E8%96%A6-wsl-windows-subsystem-for-linux-%E5%AE%89%E8%A3%9D%E8%88%87%E4%BD%BF%E7%94%A8%E6%95%99%E5%AD%B8)。檢自 辛比誌 (2021-11-05)。 
3. Will 保哥 (2019-02-01)。[介紹好用工具：WSL (Windows Subsystem for Linux)](https://blog.miniasp.com/post/2019/02/01/Useful-tool-WSL-Windows-Subsystem-for-Linux)。檢自 The Will Will Web (2021-11-05)。
4. Craig Loewen/olprod (2021-09-28)。[適用於 Linux 的 Windows 子系統文件](https://docs.microsoft.com/zh-tw/windows/wsl/)。檢自 Microsoft Docs (2021-11-05)。
5. 顧武雄 (2020-05-18)。[Win10內執行Linux程式　實戰WSL子系統安裝](https://www.netadmin.com.tw/netadmin/zh-tw/technology/163029C8DF994FE9BD51678CA04A5B42)。檢自 網管人 (2021-11-05)。
6. 玩轉Win10的MS酋長（2019-03-20）。[Win10重置/註銷Linux子系統教程](https://kknews.cc/zh-tw/code/69yoqnq.html)。檢自 每日頭條 (2021-11-05)。
7. 辛比（2019-07-28）。[[推薦] WSL (Windows Subsystem for Linux) 安裝與使用教學](https://xenby.com/b/226-%E6%8E%A8%E8%96%A6-wsl-windows-subsystem-for-linux-%E5%AE%89%E8%A3%9D%E8%88%87%E4%BD%BF%E7%94%A8%E6%95%99%E5%AD%B8)。檢自 辛比誌 (2021-11-05)。
8. Craig Loewen、olprod、Ticiana Ciminari、Bruce Chen（2021-10-06）。[針對適用於 Linux 的 Windows 子系統進行疑難排解](https://docs.microsoft.com/zh-tw/windows/wsl/troubleshooting)。檢自 Microsoft Docs (2021-11-12)。

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-05-06</summary>
  <ul>
    <li>2022-05-06 發布</li>
    <li>2021-11-09 完稿</li>
    <li>2021-11-05 起稿</li>
  </ul>
</details>
