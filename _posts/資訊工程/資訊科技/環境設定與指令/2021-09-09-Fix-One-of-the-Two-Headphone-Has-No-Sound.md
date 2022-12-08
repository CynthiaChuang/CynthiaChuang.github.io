---
title: 解決 Ubuntu 一隻耳機沒有聲音的問題
date: 2021-09-16 15:30
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Linux/Unix 
--- 

一樣升級後遇到的小問題，我的耳機剩下右耳有聲音...有試過調整平衡但沒啥效果。

<!--more-->
<br class="big">

開始前我先將耳機拔去插了手機試試，確定耳機沒壞...耳朵也沒壞 XDDD 既然都沒壞，那應該是哪裡設定出了問題了，希望啦，不然真的壞音效卡會很麻煩的。

<p class="illustration">
    <img src="https://i.imgur.com/m0FmKgk.png" alt="Headphone and smartphone">
    Headphone and smartphone（圖片來源: <a href="https://www.pikrepo.com/fyndm/black-and-yellow-canalbuds-plug-in-black-android-smartphone">Pikrepo</a>）
</p>



# 解決方案
1. **安裝 Alsa-utils**  
    不確定大家的電腦裡面是不是都有 alsamixer，如果沒有的話可能要先裝一下：
    ```bash
    $ sudo apt-get update
    $ sudo apt-get install alsa-utils
    ```

2. **執行 alsamixer**  
    ```bash
    $ alsamixer
    ```    
    
    <br class="big">
    
    此時會跳出 UI 界面，一看果然發現我的耳機音量的其中一耳幾乎貼底了：
    <p class="illustration">
    <img src="https://i.imgur.com/VCmuGyH.png" alt="調整前 alsamixer 界面">
    </p>
    
    但是說，為啥是 Speaker 而不是 Headphone？  
    
    
3. **調整音量**  
    透過**左右**鍵選擇裝置，然後長按上方鍵把兩耳聲音填滿，此時左耳應該就會有聲音了！修復後，再透過上下鍵調整回較為舒適的音量。
    
    <p class="illustration">
    <img src="https://i.imgur.com/Zdo7Bj2.png" alt="調整後 alsamixer 界面">
    </p>



## 參考資料 
1. Whywait_1 (2020-05-29)。[解决ubuntu插入耳机，两只耳机有一只没有声音或者声音偏小的问题](https://blog.csdn.net/AuthurWhywat/article/details/106431574)。檢自 AuthurWhywait的博客｜CSDN博客 (2021-09-08)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-16</summary>
  <ul>
    <li>2021-09-16 發布</li>
    <li>2021-09-09 完稿</li>
    <li>2021-09-09 完稿</li>
  </ul>
</details>