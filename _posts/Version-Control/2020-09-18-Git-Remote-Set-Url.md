---
title: 【Git】更換遠端伺服器倉庫網址
date: 2020-12-23 21:41
is_modified: false
categories:
- Version-Control
tags:
- git
--- 

忽然接到要求要我們先將 remote 的網址切換到暫時的網址，原本想撐到他們修好的，不過我今天要上 code 不改不行。

<!--more-->
<br>

## 顯示遠端

開始前先來看看原本的遠端版本庫： 
```bash=
$ git remote -v
origin	https://my.git.com:port/user_name/project.git/ (fetch)
origin	https://my.git.com:port/user_name/project.git/ (push)
```

<br><br> 

## 更改 config

同仁給我的方法是直接修改 config 檔案：
```bash
$ cd project_folder
$ vim .git/config 
```

檔案內容長這樣：
```bash
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://my.git.com:port/user_name/project.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
```
 
直接把檔案中 url 換成新的 `https://my.tmp-git.com:port/user_name/project.git`。


<br><br> 

## git 指令

不過我覺得應該有 git 的指令可以更改才對，所以查了下指令：

```bash
$ git remote set-url origin https://my.tmp-git.com:port/user_name/project.git
```
 
<br><br> 

## 參考資料 
1. fokayx (2015-04-12)。[Git更換遠端伺服器倉庫網址](https://gist.github.com/fokayx/255b228ded2bca1c4f60) 。檢自 fokayx’s gists (2019-01-04)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-12-23</summary>
  <ul class="timestamp">
    　<li>2020-12-23 發布</li>
    　<li>2020-09-18 完稿</li>
    　<li>2020-09-18 起稿</li>
  </ul>
</details>