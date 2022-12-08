---
title: Ubuntu 系統版本升級
date: 2021-09-16 16:02
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Linux/Unix 
--- 

我需要我的無線網路阿，但我只能看到 WIFI 列表，但沒有任何一個熱點可以連線的，試過各種方法都只能得到`啟動失敗`的錯誤訊息。無奈之下，只能將作業系統從 Ubuntu 18.04 升級成 Ubuntu 20.04 :cry: 
業系統從 Ubuntu 18.04 升級成 Ubuntu 20.04 :cry: 

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/wTWEMpR.jpg" alt="Ubuntu 20-04 Official Logo">
    Ubuntu 20-04 Official Logo（圖片來源: <a href="https://www.ubuntufree.com/download-ubuntu-20-04-lts/ubuntu-20-04-official-logo/">Ubuntu Free</a>）
</p>



## 升級做法
- **Step1：更新伺服器**  
    首先先從取得更新伺服器的套件檔案清單來檢查可以升級的版本，並更新套件。
    ```bash
    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo apt-get -y dist-upgrade
    ```

- **Step2：配置更新管理器**  
    在 Ubuntu 有一個名為 [Update Manager](https://zh.wikipedia.org/wiki/%E8%BD%AF%E4%BB%B6%E6%9B%B4%E6%96%B0%E5%99%A8) 的工具，可以協助將系統升級到新版本。不過正常情況下，管理器應該已經安裝了。
    ```bash
    $ sudo apt install update-manager-core
    ```
    
-  **Step3：釋放硬碟空間**  
    在進行更新前，我習慣性會清掉一些快取檔案，放掉硬碟空間。其中 `autoclean` 與 `clean` ，會分別將已刪除和已安裝的軟體安裝檔給刪除。而 `autoremove` 會清除更新後用不到的舊版本檔案，如舊的 kernel，與不再被使用的相依性軟體。
    ```bash
    $ sudo apt-get autoclean
    $ sudo apt-get clean
    $ sudo apt-get autoremove
    ```
       
-  **Step4：執行升級**    
    升級前可以先查看一下可升級的版本：
    ```bash
    $ sudo do-release-upgrade -c
    ```

    確認好版本，也下定決心要升級的話，就按下這指令吧：
    ```bash
    sudo do-release-upgrade
    ```
    幸運的話，就會開始升級了。


## 錯誤排除
不過想當然爾，安裝過程中怎麼可能不出包 XDDD 當我按下後立刻跳出錯誤訊息：
> Checking for a new Ubuntu release Please install all available updates for your release before upgrading

<br class="big">

它告訴必須把所有的更新先做完才可以，只好回頭處理剛剛的錯誤訊息：

```bash
處理時發生錯誤：/var/cache/apt/archives/docker.io_20.10.7-0ubuntu1~18.04.1_amd64.deb
```

<br class="big">

我還以為是安裝檔損壞了，所以先嘗試刪除損壞的安裝檔後，重新更新系統：
```bash
$ sudo dpkg --remove --force-remove-reinstreq docker.io_20.10.7-0ubuntu1~18.04.1_amd64
$ sudo apt-get update
```

<br class="big">

不過依舊跳出錯誤訊息，但細細研究錯誤訊息發現解法就寫在第一行了，不過我忘了複製訊息，大意是某個資料夾在 20 時不再使用，請確認無容器在使用後將它刪除，所以直接照做即可，最後更新後直接升級，大約等 2~3 小時，就可以享受 Ubuntu 20.04 了！ :+1:



## 參考資料 
1. ethan (2019-03-16)。[Ubuntu 更新與升級](https://project.zhps.tp.edu.tw/ethan/2019/03/ubuntu-%E6%9B%B4%E6%96%B0%E8%88%87%E5%8D%87%E7%B4%9A/)。檢自 Ethan's 學習筆記 (2021-09-03)。
2. Clay (2019-11-01)。[[已解決][Linux] Please install all available updates for your release before upgrading](https://clay-atlas.com/blog/2019/11/01/please-install-all-available-updates/)。檢自 Clay-Technology World (2021-09-03)。
3. Peter279k (2019-02-01)。[從Ubuntu 16.04升級到Ubuntu 18.04之心得(持續更新)](https://peterli.website/%E5%BE%9Eubuntu-16-04%E5%8D%87%E7%B4%9A%E5%88%B0ubuntu-18-04%E4%B9%8B%E5%BF%83%E5%BE%97/)。檢自 Peter 工程日誌 (2021-09-03)。
4. Linux學習教程 (2020-05-06)。[如何將Ubuntu從18.04升級到20.04版本](https://kknews.cc/zh-tw/code/g42eqp8.html)。檢自 每日頭條 (2021-09-03)。
5. flydream0 (2013-02-28)。[apt-get指令的autoclean,clean,autoremove的区别](https://blog.csdn.net/flydream0/article/details/8620396)。檢自 放飞梦想，成就未来｜CSDN博客 (2021-09-04)。
6. Ubuntu問答 (2020-06-26)。[apt – 如何刪除Ubuntu中損壞的軟件包](https://ubuntuqa.com/zh-tw/article/10754.html)。檢自 Ubuntu問答 (2021-09-04)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-16</summary>
  <ul>
    <li>2021-09-16 發布</li>
    <li>2021-09-04 完稿</li>
    <li>2021-09-03 起稿</li>
  </ul>
</details>