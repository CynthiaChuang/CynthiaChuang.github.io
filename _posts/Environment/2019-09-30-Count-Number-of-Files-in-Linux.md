---
title: 【Linux】計算資料夾下的檔案數
date: 2019-07-30
categories:
- Environment
tags:
- Linux/Unix 
--- 

來數一數資料夾裡有多少個檔案？ ❶、❷、❸、❹、❺、❻、❼ 

<!--more-->
<br> 

## 包含子目錄中的檔案與隱藏檔案

```shell
$ find ./ -type f | wc -l
```

<br> 若只想計算某特定副檔名的檔案數，則加上 `-name`

```shell
$ find ./ -type f -name *.py | wc -l
```

<br><br> 

其中 `find ./ -type f` 是指找到此目錄下的所有一般文件。
<br> 而 `wc` 指令則是用於計算文件的 byte 數、字數或列數，`-l` 就是指定輸出列數。由於我們未指定文件名稱，所以指令會從輸入設備，也就是前一個指令的輸出結果，讀取數據。



<br><br>

## 包含子目錄中的檔案，但不包含隱藏檔案
若是不想計算隱藏檔案與目錄的個數，畢竟我用了 git 它真的多到爆，可以用 `ls -lR` 取代 `find`：

```shell
$ ls -lR | grep "^-" | wc -l
```


<br> 計算某特定副檔名，則在 `ls` 指令後加上

```shell
$ ls -lR *.py | grep "^-" | wc -l
```

<br><br>

其中 `ls -l` 就是列出詳細資料，而 `-R` 則是表明，若目錄下有仍有文件，則以下的文皆依序列出。在使用 `ls -l` 時，資訊的展示方式如下，可看到首碼若為 `d` 表目錄、為 `-` 則是文件：
```
drwxr-xr-x  8 Cynthia_Chuang cynthia-chuang 4096 12月 31 16:16 .git/
-rw-r--r--  1 Cynthia_Chuang cynthia-chuang 1315 12月  9 09:49 .gitignore
```

<br> 在 `grep "^-"` 中後面的字串是 [Regular Expression](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Guide/Regular_Expressions#special-caret)，是在宣告開頭字元匹配要`-`，換句話說就是要濾出檔案。反之若是想計算有幾個資料夾，則下

```shell
$ ls -lR | grep "^-d" | wc -l
```

<br><br>

## 不包含子目錄中的檔案，不包含隱藏檔案
如果就單純從想算這一層有多少的檔案，直接用 `-l`

```shell
$ ls -l | grep "^-" | wc -l
```

<br> 想知道檔案+目錄有多少個
```shell
$ ls -l | grep "^[-d]" | wc -l
```

<br><br> 

## 參考資料 
1. [Linux Ubuntu 計算資料夾下的檔案數｜小賴的實戰記錄](https://dotblogs.com.tw/newmonkey48/2012/12/13/85630) 
2. [Linux wc命令｜菜鸟教程](https://www.runoob.com/linux/linux-comm-wc.html)
3. [Linux ls命令｜菜鸟教程](https://www.runoob.com/linux/linux-comm-ls.html)