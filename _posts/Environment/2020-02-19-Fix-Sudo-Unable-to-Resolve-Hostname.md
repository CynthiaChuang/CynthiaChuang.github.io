---
title: '解決 sudo: unable to resolve hostname'
date: 2020-02-19
is_modified: false
categories:
- Environment
tags:
- Linux/Unix
--- 

有夠莫名其妙的，一覺醒來不僅 docker 壞了，連 sudo 都爛掉了 QAQ

<!--more-->
<br><br> 

## 問題描述與釐清
每次執行 sudo 就出現這個警告訊息，也是屬於不影響...但超級煩的那種 orz

```bash
sudo: unable to resolve host ubuntu16-x64
```

其中 ubuntu16-x64 是我的 hostname。我在懷疑是我之前換過 hostname 的緣故造成它無法解析，但...我換 hostname 是快 2 個多月前的事情吧...，我還是搞不懂會啥忽然會跳這錯誤訊息了？

<br><br> 

## 解決辦法
其實還滿簡單的，修改 `/etc/hosts` 就好了

```bash
$ vim /etc/hosts
```

<br> 然後把新的 hostname，也就是 ubuntu16-x64 加到檔案中

```diff 
- 127.0.0.1 localhost
+ 127.0.0.1 localhost ubuntu16-x64
```
 
<br> 設完後, 使用 sudo 就不會出現那條錯誤訊息了~

<br><br> 

## 參考資料 
1. Tsung (2008/11/20)。[Ubuntu / Debian: sudo 出現 unable to resolve host 錯誤解法](https://blog.longwin.com.tw/2008/11/linux-sudo-unable-to-resolve-host-2008/) 。檢自 Tsung's Blog (2020-02-18)。