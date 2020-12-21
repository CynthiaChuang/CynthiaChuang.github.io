---
title: 【Git】使用 git-rebase 修改 Commit 
date: 2019-12-13
is_modified: false
categories:
- Version-Control
tags:
- git
--- 

記錄一下自己常用的 `git-rebase` 的用法。

<!--more-->
<br><br> 

## 情境假設
假設目前 git log 存在多筆 commit ， A → B → C → D → E， e 是目前最新的 commit ，也就是 HEAD。

<br>現在我想對某一筆 commit 進行修改，可以使用 rebase -i 命令選擇要修改的提交
```shell
$ git rebase -i <B 的 commit id>
```

<br>此時文字編輯器會顯示從 HEAD 到 B 的 commit 之間的所有 commit，如下圖顯示：

```shell
pick <C 的 commitId> <C 的 commit 內容>
pick <D 的 commitId> <E 的 commit 內容>
pick <E 的 commitId> <E 的 commit 內容>

# Rebase 326fc9f..0d4a808 onto d286baa
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
 ```

在文字編輯器以看到好幾個指令，需依照目的來挑選指令，這邊列一下我常做的操作：

<br><br> 

## 修改 commits message
這是我最常做的，在要修正的 commit 前的 `pick` 換成 `r` 或 `reword`：
```shell
r <C 的 commitId> <C 的 commit 內容>
```

接著就會回到一般 commit 的編輯畫面，將新的 commits message 打完存檔、退出就行了。

<br><br> 

## 合併 commits
我這比較常用在開發一個比較大的 function 時。我會將每一步測試完後，先用 commit 暫存起來，當整體功能開發完成後，再把合併成一個完整的 commit。

先選擇一個 base，再將要向前合併的 commit 前改為 `s` 或 `squash`。
```shell
pick <C 的 commitId> <C 的 commit 內容>
s <D 的 commitId> <E 的 commit 內容>
```

接著會跳到 commit 編輯畫面，畫面上會同顯示要合併的兩個 commit message：
```shell
# This is a combination of 2 commits.
# The first commit's message is:

<C 的 commit 內容>

# This is the 2nd commit message:

<E 的 commit 內容>
```

把舊的 commit message 註解掉後，輸入新的 commit message 後存檔、退出。此時用 `git log` 就能看到新的 commit 了。

<br><br> 

## 修改 commits 中 tracked 的檔案
這個通常我是用來修正未上傳前 commit 中的檔案，例如錯別字修正的狀況。在這情況下，在要修正的 commit 前改為 `e` 或 `edit`。
```
e <C 的 commitId> <C 的 commit 內容>
```

接下來會收到下列提示，此時就可以去修正檔案，假設我需要修改的檔案為 `sample_C.txt`。
```
You can amend the commit now, with
	git commit --amend 

Once you are satisfied with your changes, run
	git rebase --continue
```

當檔案修改完後，先用 `diff` 檢查一下檔案的差異，再用 `add` 將檔案加進索引，最後按照提示依序輸入 `amend` 與 `continue`。

```shell
$ git diff
$ git add sample_C.txt
$ git commit --amend
$ git rebase --continue
```

回頭使用 `git diff` 檢查下檔案的差異，就會看到原本的 commit 中多了剛剛修改的內容。

<br><br> 

## 修改 commits 的作者資訊
這件之前在《[【Git】修改 Git commits 的作者資訊](/Change-the-Git-Commit-Author-for-the-Specific-Commits/)》，這算是 edit 模式的另一種應用？