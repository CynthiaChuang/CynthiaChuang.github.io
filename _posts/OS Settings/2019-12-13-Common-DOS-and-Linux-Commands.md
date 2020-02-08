---
title: 【Windows】常用 Unix 與 DOS 的指令
date: 2019-12-13
categories:
- OS-Settings
tags:
- Windows/DOS
- Linux/Unix
--- 

每次回到 Windows 的 DOS command，都習慣性直接打 <span class='label'>cd d:\\</span> 要切換磁槽、使用 <span class='label'>ll</span> 要來顯示檔案，想當然每次都沒反應 XD
 
每每遇用到這狀況就要找 google，所以還是決定自己做一份對照表...至少下次還需要 search 時是為自己貢獻流量 XDDD 
<!--more-->
<br><br>  

## 常用對照表

| Unix | DOS | MS-DOS 說明 |  
| --- | --- | --- |
| cd d:\ | d: | 切換磁槽  |
| cd [相對路徑或絕對路徑] | cd [相對路徑或絕對路徑] |切換資料夾目錄 |
| mkdir [目錄名稱] | md [目錄名稱] | 建立目錄 |
| pwd | cd | 顯示目前所在的目錄 |
| rm [目錄/檔案名稱] | del [目錄/檔案名稱] | 刪除目錄/檔案 |
| cat [檔案名稱] | type [檔案名稱] | 檔案內容查閱 |
| ls | dir | 檔案與目錄的檢視 |
| cp [來源] [目的] | copy  [來源] [目的] | 複製檔案或目錄 | 
| mv [來源] [目的] | ren [來源] [目的] | 移動檔案與目錄或更名 | 


<br><br> 

## 參考資料 
1. [Unix 與 MS-DOS 指令對照表](http://163.28.10.78/content/primary/info_edu/cy_sa/LinuxY/cmd/dos2unixcmd.htm)
2. [DOS的指令切換至硬碟的D槽｜Ann's雜誌](https://blog.xuite.net/an224/twblog/128339418-DOS%E7%9A%84%E6%8C%87%E4%BB%A4%E5%88%87%E6%8F%9B%E8%87%B3%E7%A1%AC%E7%A2%9F%E7%9A%84D%E6%A7%BD)