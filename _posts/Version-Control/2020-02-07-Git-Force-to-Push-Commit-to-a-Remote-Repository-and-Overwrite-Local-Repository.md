---
title: 【Git】強制更新遠端分支 與 強制覆蓋本地的分支
date: 2020-02-07
is_modified: false
categories:
- Version-Control
tags:
- git
--- 

記錄一個壞習慣 XD 我這處女座的個性，大概每隔一陣子就會對專案的 commit 的歷史紀錄感到不順眼，想要對它大刀闊斧地診整頓一番。
 
如果只有整理 local 端的 commit 倒是簡單，只要用`git-rebase`就好，但是如果 commit 已經上遠端了的呢？
 
**只能來硬的了 XDDD**

<!--more-->
<br><br>

## 強制更新遠端分支
其實只要一條指令就可以將`git-rebase`後的 commit 給送上遠端。

```shell
$ git push -f origin <rbranch>:<lbranch> 
```

<center> <img src="https://miro.medium.com/max/500/0*XaLzNzYkA6PZjbl9.jpg" alt="危險的 force 指令"></center>
<center class="imgtext">危險的 force 指令（圖片來源: <a href="https://estl.tech/a-gentler-force-push-on-git-force-with-lease-fb15701218df" class="imgtext">Engineering Tomorrow’s Systems</a>）</center>
<br>

**使用前請注意！**`-f` 是個非常危險的指令，它可以無視一切先來後到的規則，讓你的 commit，直接取代線上所有內容。不是讓團隊進度付之一炬，不然就是像這些 Jenkins 的開發人員一樣[不小心強制更新了 150 多個 github repos](https://groups.google.com/forum/#!searchin/jenkinsci-dev/force$20push/jenkinsci-dev/-myjRIPcVwU/mrwn8VkyXagJ)。真的做了你應該會被組員拖去套麻布袋 XD
<br>

<div class="alert info"> 
<div class="head">強烈建議</div>
1. 用在只有自己一個人的 Project 或 Branch。<br>
2. 務必確認送出的內容與遠端分支。<br>
3. 務必頭腦清晰確認自己在做啥，不清晰，<b>回去睡飽</b>再回來。<br>
</div>

<br><br> 

做完才想到一件事。我是一個人開發沒錯，但我在兩台機器上都有程式碼，所以另一台的 repo 怎麼辦？

## 強制覆蓋本地的分支
在開始抓程式碼覆蓋前，請先確認沒有要保存的程式或 commit，一旦執行了這些都會不見的！
<br>

### git pull
對相應`git push`，`pull`當然也有相對應的強制指令：
```shell
$ git pull --force origin <rbranch>:<lbranch> 
```

<br> 不過一開始我並沒下完整分支，所以一直跳出 merge 的訊息。所以我回頭看了文件，發現它寫到：

> -f, --force <br>
> When git fetch is used with \<rbranch\>:\<lbranch\> refspec, it refuses to update the local  branch \<lbranch\> unless the remote branch \<rbranch\> it fetches is a descendant of  \<lbranch\>. This option overrides that check.

似乎是需要完全指定分支才行。

<br>

### git reset
如過上面一招不行還有另外一招組合技，先取回遠端數據庫的最新歷史紀錄
```shell
$ git fetch --all 
```   

然後放棄目前所有檔案與 commit，還原成遠端版本
```shell
$ git reset --hard origin/master
```

最後重新拉回來
```shell
$ git pull origin master
```

<br><br> 

## 參考資料 
1. 高見龍 (2017)。[【狀況題】聽說 git push -f 這個指令很可怕，什麼情況可以用它呢？](https://gitbook.tw/chapters/github/using-force-push.html) 。檢自 為你自己學 Git (2020-02-07)。
2. Toh Weiqing (2016-12-01)。[A gentler force push on git: Force-with-lease](https://estl.tech/a-gentler-force-push-on-git-force-with-lease-fb15701218df)。檢自 Engineering Tomorrow’s Systems (2020-02-07)。
3. 10km (2018-11-30)。[git:pull --force 強制覆蓋本地的分支](https://blog.csdn.net/10km/article/details/84669270) 。檢自 10km的专栏-CSDN博客 (2020-02-07)。
4. Mike (2019-11-04)。[[Git] Git 自學筆記 : 單一檔案(checkout) ,退版(reset) ,重拉(pull), 強推(push)](https://dotblogs.com.tw/michaelfang/2016/09/07/git-reset-log-reflog)。檢自 Mike's開發瘋 - 點部落 (2020-02-07)。