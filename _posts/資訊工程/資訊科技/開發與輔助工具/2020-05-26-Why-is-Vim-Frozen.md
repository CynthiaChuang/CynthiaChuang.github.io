---
title: Vim 卡死動不了的解決方法
date: 2020-05-26
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- IDE/Simulator
- Linux/Unix
- 工具介紹與操作
--- 

Vim 是大部分系統都有內建的編輯器，有時在陌生環境不想安裝額外編輯器時，它絕對是你的好幫手。     
但在使用 vim 時偶爾會卡死，無法輸出任何文字，有點像當掉了。偏偏終端機中其他頁籤可以輸出...

<!--more-->
<br><br> 

## Q：Vim 卡死動不了的解決方法
### 狀況描述
使用 vim 時偶爾會卡死，無法輸出任何文字，有點像當掉了。但終端機中其他頁籤可以輸出，不像是 Linux 系統當機。


### 懷疑原因
可能是不小心按到 `Ctrl + s`，因此將螢幕鎖定了。


### 解決方法
可以藉由 `Ctrl + q` 解除鎖定。



## 參考資料 
1. 施靜樺 (2019-04-19)。[每個開發者都應該要會用的編輯器–Vim](https://medium.com/@jinghua.shih/%E6%AF%8F%E5%80%8B%E9%96%8B%E7%99%BC%E8%80%85%E9%83%BD%E6%87%89%E8%A9%B2%E8%A6%81%E6%9C%83%E7%94%A8%E7%9A%84%E7%B7%A8%E8%BC%AF%E5%99%A8-vim-5f83349973a3) 。檢自 施靜樺 - Medium (2020-04-08)。
2. Bruce_You (2017-05-23)。[vim 卡住 死机_开发工具](https://blog.csdn.net/Bruce_You/article/details/72633717) 。檢自 Bruce_You的博客 - CSDN博客 (2020-04-08)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期： 2020-05-26</summary>
  <ul>
    <li>2020-05-26 發布</li>
    <li>2020-04-08 完稿</li>
  </ul>
</details>