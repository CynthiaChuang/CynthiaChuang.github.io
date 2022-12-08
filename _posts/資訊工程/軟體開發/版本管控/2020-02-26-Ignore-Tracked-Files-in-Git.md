---
title: 【Git】取消追蹤檔案
date: 2020-02-26
is_modified: false
categories:
- "軟體開發 › 版本管控"
tags:
- Git
--- 

不小心把 log 給 commit 進去了，來想辦法把它移掉。

<!--more-->
## 解除追蹤
當檔案已經被追蹤後，即便在 `.gitignore` 內加入新的規則，也無法排除。所以首先先來將檔案解除追蹤：

如果要解除單一檔案或是資料夾，可以用：
```bash
$ git rm --cached filename    
$ git rm -r --cached foldername   
```

<br class="big"> 

若是要解除多個的話，則可以直接把所有檔案追蹤，這樣比較快：
```bash
$ git rm -r --cached .
```



## 新增 `.gitignore` 規則
接下來在 `.gitignore` 內加入新的規則，像我是要忽略 log 檔案，所以就在 `.gitignore` 下加入：
```bash
*.log
```



## 最後重新 commit 
```bash
$ git add .
$ git commit -m '[remove] remove log and update .gitignore'
```



## 參考資料 
1. David Ma (2017-04-13)。[在 git 中取消追蹤檔案 Ignore tracked files in git](http://blog.ma.beibeilab.com/ignore-tracked-files/) 。檢自  Bunun Engineer's Blog (2020-02-26)。