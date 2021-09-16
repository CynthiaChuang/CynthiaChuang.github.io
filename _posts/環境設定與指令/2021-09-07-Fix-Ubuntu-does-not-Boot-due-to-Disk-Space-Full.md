---
title: 解決 Ubuntu 系統硬碟滿載，導致系統無法啟動
date: 2021-09-16 15:58
is_modified: false
categories:
- 環境設定與指令
tags:
- Linux/Unix 
--- 

那天將 OS 由 [Ubunt 18.04 升級 Ubuntu 20.04](/Upgrade-Ubuntu) 後，配置完慣用的快捷件與 UI 後，順手重新開了機。卻結果發現開不了機了，一旦輸入開機密碼就跳黑屏，螢幕上顯示這錯誤訊息：
  
```bash
/dev/sda6:clean ***/*** files, ***/***blocks
```
<!--more-->

<br>

雖然在網路上找到很多可能的原因，但我依稀記得開機前有看到 Chrome 給了我一條提示：**說我的空間已滿，建議我刪除一些網頁暫存紀錄**，尋思是不是因為硬碟空間滿了導致開不了機？雖然覺得不可思議，但還是決定先朝這方向來試試。

- **Step1：確認硬碟是否滿載**  
    不過我也不確定這是不是 root cause，但系統又進不去了，所以決定先進 Recovery Mode。
    <center> <img src="https://i.imgur.com/8knMlcs.png" alt="進入 Recovery Mode"></center>
    <br>
    
    選擇 root 選項並輸入密碼：
    <center> <img src="https://i.imgur.com/l55nGN7.png" alt="root 選項"></center>
    <br>
    
    看了下硬碟的佔用情況：
    ```bash
    $ df –lh 
    ```
    <center> <img src="https://i.imgur.com/BtWNLaj.png" alt="硬碟的佔用情況"></center>
    <br>
    
    果然滿載了，雖然百思不得其解，不過既然是硬碟滿了，那就刪掉冗餘的軟體與檔案就好啦～！
    
    <br>
    
- **Step2：釋放硬碟空間**  
    先清掉一些快取檔案、安裝檔、舊版本檔案和相依性軟體，放掉硬碟空間。 
    ```bash
    $ sudo apt-get autoclean
    $ sudo apt-get clean
    $ sudo apt-get autoremove
    ```
    <br>
    
    不想下指令的話，剛剛 UI 上有一個 clean 的選項也能達到類似的效果：
    <center> <img src="https://i.imgur.com/WtjAZvG.png" alt="clean 的選項"></center>
       
    <br>
    
- **Step3：刪除大文件**   
    可透過 `du` 指令，如：`du -h max-depth=1  /usr/` 或 `du -shx /*`，不然也可以土法煉鋼用 `ls -lhS` 將檔案由從大到小順序，一層一層地去找出大檔案。
    
    不過有找到一個比較快速的方法，直接找硬碟上大於 400MB 的文件，一般通常都 log 檔，可以直接刪除： 
    ```bash
    $ find / -size +400000k -exec rm -rf {} \;
    ```
        
    <br>
    
    如果擔心誤刪的的話，可以先將大於 400MB 的文件列出在手動刪除：
    ```bash
    $ find / -size +400000k -exec ls -lhS {} \;
    ```
        
    <br>
       
    搞定後，再次檢查硬碟空間，OK 的話，就可以下重新開機：
    ```
    $ reboot
    ```
    <br>
    
...這才是我的正常應得使用量咩，剛剛到底發生了啥事阿！
<center> <img src="https://i.imgur.com/Hrv4vHV.png" alt="清理後硬碟的佔用情況"></center>



<br><br> 

## 參考資料 
1. cskywit (2019-07-24)。[【问题解决】/dev/sda6:clean xxx/xxx files, xxx/xxx blocks_寸先生的AI道路-程序员宝宝_ubuntu强制关机后无法进入](https://www.cxybb.com/article/cskywit/97142880)。檢自 寸先生的AI道路｜CSDN博客 (2021-09-07)。
2. jmq (2014-05-07)。[Ubuntu does not boot due to disk space full](https://superuser.com/questions/750782/ubuntu-does-not-boot-due-to-disk-space-full)。檢自 Super User (2021-09-07)。
3. davefighting (2018-07-01)。[ubuntu系统磁盘已满，导致系统无法启动](https://blog.csdn.net/zhouxiaowei1120/article/details/80872905?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.essearch_pc_relevant&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.essearch_pc_relevant)。檢自 zhouxiaowei1120的博客｜CSDN博客 (2021-09-07)。
4. LIEYz (2020-10-15)。[ubuntu磁盘满导致无法开机](https://blog.csdn.net/qq_18998145/article/details/109091091)。檢自 LIEY｜CSDN博客 (2021-09-07)。
5. 三铜钱 (2021-05-16)。[Linux系统盘满了无法启动系统,linux 系统运行久了，硬盘满了如何处理呢？](https://blog.csdn.net/weixin_33402252/article/details/116985928?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-7.essearch_pc_relevant&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-7.essearch_pc_relevant)。檢自 三铜钱的博客｜CSDN博客 (2021-09-07)。
6. 吴小白呢 (2021-08-09)。[Linux之解决磁盘耗尽导致系统无法启动](https://blog.csdn.net/xiaobai316/article/details/119535325?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-1.essearch_pc_relevant&spm=1001.2101.3001.4242)。檢自 xiaobai316的博客｜CSDN博客 (2021-09-07)。
 


<br><br>  

## 更新紀錄
<details>
  <summary>最後更新日期：2021-09-16</summary>
  <ul class="timestamp">
    　<li>2021-09-16 發布</li>
    　<li>2021-09-07 完稿</li>
    　<li>2021-09-07 起稿</li>
  </ul>
</details>