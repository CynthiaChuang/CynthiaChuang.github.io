---
title: 【Git】救回遺失 commit 
date: 2021-03-07 23:25
is_modified: false
categories:
- "軟體開發 › 版本管控"
tags:
- Git
--- 

那天在社團看到一篇[求救文](https://www.facebook.com/groups/1403852566495675/permalink/2823371291210455/)，內容主旨是這樣的，求救者把花費六小時所撰寫的 commit，在還沒 push 前就把它給 drop 掉了（允悲 :joy: 
  
不過幫他哀弔完之後，我也想知道怎麼把 commit，因為我很怕下一個做這蠢事就是我 XDDD
<!--more-->
<br>

<center> <img src="https://i.imgur.com/Rq9W4dg.png" alt="求救文內文"></center>
<center  class="imgtext">求救文（圖片來源: <a href="https://www.facebook.com/groups/1403852566495675/permalink/2823371291210455/"  class="imgtext">Taiwan 程式語言讀書會｜Facebook</a>）</center>
<br><br>

## 所有的操作紀錄都保留下來

求救文在下方的留言中，有一位[米一粒](https://www.facebook.com/groups/1403852566495675/permalink/2823371291210455/?comment_id=2823410504539867)的留言，他說當你製作提交紀錄時，就已經讓紀錄進入版本管控中，而 Push 這動作只是將紀錄推送遠端數據庫而已。因此，只要有提交過記紀錄、產生過 Hash，就有機會藉由這筆 Hash 拿回這筆紀錄？

<br>

不過問題來了，我都把 commit 給丟了，我要怎查詢 commit 的 Hash code？這時可以用 `git reflog` 指令，來顯示每個指令的 SHA-1 值：
```bash
$ git reflog
fc22a64 (HEAD -> master) HEAD@{0}: rebase -i (finish): returning to refs/heads/master
fc22a64 (HEAD -> master) HEAD@{1}: rebase -i (start): checkout 807e915898cd6cbf337512e83a0ccff1a1551df9
45f3987 HEAD@{2}: commit: commit3
fc22a64 (HEAD -> master) HEAD@{3}: commit: commit2
807e915 HEAD@{4}: commit (initial): commit1
```

<br>

從紀錄看來，依序製作了三個 commit，接下來下了 `rebase` 指令，最後把 HEAD 移動到了 commit2 的位置，也就是我把 commit3 給丟了。好了，重點是第四行，那邊有被刪除的 commit3 的 Hash code！

<br><br>

## 恢復 commit

在得知被刪除的 commit 的 Hash code 後，就可以透過 `git reset` 來還原了：

```bash
$ git reset 45f3987 --hard
HEAD is now at 45f3987 commit3
```

<br>

再用 `git reflog` 看看目前 HEAD　的狀況：

```bash
$ git reflog
45f3987 (HEAD -> master) HEAD@{0}: reset: moving to 45f3987
fc22a64 HEAD@{1}: rebase -i (finish): returning to refs/heads/master
fc22a64 HEAD@{2}: rebase -i (start): checkout 807e915898cd6cbf337512e83a0ccff1a1551df9
45f3987 (HEAD -> master) HEAD@{3}: commit: commit3
fc22a64 HEAD@{4}: commit: commit2
807e915 HEAD@{5}: commit (initial): commit1
```

歐耶～！把 commit 找回來了！

<br><br> 

## 參考資料 
1. Calos。[GIT 遺失 commit 後的恢復方法](https://caloskao.org/git-recovery-lost-commit/) 。檢自 Calos's Blog (2021-02-04)。
2. 洧杰 (2019-11-17)。[git reflog - 還原大招](https://w3c.hexschool.com/git/10bf7677) 。檢自 W3HexSchool (2021-02-04)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-03-07</summary>
  <ul>
    <li>2021-03-07 發布</li>
    <li>2021-02-04 完稿</li>
    <li>2021-02-04 起稿</li>
  </ul>
</details>