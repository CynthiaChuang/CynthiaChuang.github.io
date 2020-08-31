---
title: 【Windows】Asus win8 筆電使用 USB 開機
date: 2016-02-27
is_modified: false
categories:
- Environment
tags:
- Windows/DOS
--- 

之前我將 Ubuntu 灌到我的 Toshiba 的行動硬碟中，在用桌電進行開機時還沒有問題，但筆電的 BIOS 完全進不去阿~!! F2 都快按壞了，超崩 ( 皿 ) 潰。
 
查了一下華碩的官網發現因為它們家的 Win8 因為導入了快速開機功能，導致 F2 失效，還好他們有記得留教學如何進去 BIOS，再加上網路上的資料，終於設定完成了
ヽ(✿ﾟ▽ﾟ)ノ

<!--more-->
<br> 

## 1. 進入BIOS介面
1.  開機後  
    點選選單中的設定 (Settings) → 變更電腦設定 (Change PC settings)
    
2.  接下來再點選  
    更新與復原 (Update and recovery)→ 進階啟動 (Advanced startup)
    
3.  之後會進入一個藍藍的選單，選擇  
    疑難排解 (Troubleshoot) → 進階啟動 (Advanced startup) → 進階選項 (Advanced options) → UEFI 韌體設定 (UEFI Firmware Settings) → 重新啟動 (Restart)
    
4.  開機後就直接進入 BIOS 介面了  
    
 
<br><br>

## USB開機設定 
華碩官網是有教如何修改開機優先序啦，只是我在選單中始終找不到我的 Toshiba 只好改用網路上的另一個方法

1.  切到 Boot 選單內將  
    Lunch CSM → Enabled
2.  再切換到 "Security"  
    Secure Boot Control → Disabled
3.  按 F10 儲存
4.  重開後一直按 "Esc" 會出現開機選單，就會出現選硬碟及光碟了

<br><br> 

## 參考資料 
1. [Windows 8-如何進入BIOS設定頁面｜ASUS官網常見問題](http://www.asus.com/tw/support/FAQ/1008329/)
2. [華碩win8筆電如何用USB開機｜PC救星開機隨身碟](http://www.pcsavior.com.tw/zh/qa/165.html)
3. [Windows 8-如何由隨身碟或光碟機開機｜ASUS官網常見問題](https://www.asus.com/tw/support/faq/1008277)
 