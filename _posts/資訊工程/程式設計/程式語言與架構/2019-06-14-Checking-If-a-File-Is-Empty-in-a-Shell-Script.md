---
title: 【Shell Script】檢查檔案是否為空白
date: 2019-06-14
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- Shell Script
--- 

在寫 Shell Script 時 ，想確定某檔案是否為空，若為空則想執行其他動作。

<!--more-->
<br> 

## 1. **find** 
直覺想到就是用 find 指令，加上 <mark>empty</mark> ，會回傳為空的檔案名稱，否則回傳空字串。

```bash
result=`find /home/user -type f  -empty -name  file.txt`

if [ "${result}" != "" ]; then
    #do somethig...
fi
```

<br><br> 

## 2. **Shell Script -s 參數**
除了用 find 指令外，另外找到一個 <mark>-s</mark>  參數，它會判斷若檔案存在且內容為空，則回傳 true ，否則回傳 false。

```bash
filename='/home/user/file.txt'

if [ ! -s "${filename}" ]; then
    #do somethig...
fi
```

<br><br>

## 參考資料
1.  [Shell Script 檢查檔案內容是否空白｜LINUX 技術手札](https://www.opencli.com/linux/shell-script-check-file-content-empty)
