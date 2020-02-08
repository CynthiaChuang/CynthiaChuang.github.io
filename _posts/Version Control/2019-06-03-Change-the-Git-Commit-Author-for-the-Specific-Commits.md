---
title: 【Git】修改 Git commits 的作者資訊
date: 2019-06-03
categories:
- Version-Control
tags:
- git
--- 

前一陣子，用公司電腦寫自己的code，結果…啊啊啊啊，<span class="label">git 的作者資訊</span>顯示的是公司帳號阿 QAQ
  
原本想說回家後，用 `git cherry-pick` 一個 commit 一個 commit 搬移後，再重新 commit。結果發現...靠！有將近 20 個 commit ，這樣搬會瘋掉的。

<!--more-->
<br>

還好找到了這篇 [修改 Git commits 的作者資訊](https://yulun.me/2014/git-tips-change-author-and-email-in-previous-commits/) ，雖然情境不太一樣，人家是回家加班寫 code，我是上班時間~~摸魚~~進修，不過目的都是一樣的都是修改 Git commits 的作者資訊。
<br>

## 修改 Git commits 的作者資訊
### 1. 情境
假設目前 git log 存在多筆 commit ， A -> B -> c -> d -> e， e 是目前最新的 commit ，也就是 HEAD。
其中大小寫字母屬於不同作者提交的 commit。以我的案例來說，大寫是我私人帳號、小寫的則是我公司帳號。<br>


### 2. 指令
若想修改 c、d、e 三筆 commits 的作者資訊，依序執行下列步驟：

1. 在專案目錄下 rebase 指令 
    ```shell
    $ git rebase -i <B 的 commit id>
    ```
    

2. 此時，終端器上會出現類似下面的
	```shell
	pick <c 的 commitId> <c 的 commit 內容>
	pick <d 的 commitId> <d 的 commit 內容>
	pick <e 的 commitId> <e 的 commit 內容>
	```
	  將要更改的 commit 的 command 由 pick 改成 **<span class="label">e 或 edit</span>** 後，儲存離開進入 rebase 流程。
      

3. 此時它會提醒你目前停在 c 的 commit，顯示如下：
    ```shell
    Stopped at <c 的 commitId>  <c 的 commit 內容>
    You can amend the commit now, with

      git commit --amend

    Once you are satisfied with your changes, run

      git rebase --continue
    ```
    
4. 接著輸入正確的作者姓名與 E-mail。
    ```shell
    $ git commit --amend --author="Author Name <email@address.com>"
    ```

5. 修改完成後，就告知 git 並存檔繼續。
    ```shell
    $ git rebase --continue
    ```

6. 接下來會進入下一個被修改 commit ，也就是 d，重覆進行**<span class="label">步驟 3、4、5</span>** ，直到所有 commit 被修改完成。


<br><br>

## 修改專案的作者資訊
不過每次這樣改有點麻煩，畢竟我剛剛是重覆執行了<span class="label">二十次</span>阿！

與其事後修補，倒不如防範未然。所以我想直接修改專案的作者資訊，由於指令不帶 `--global` 參數，因此它的作用域只有在這個專案，不會影響整個 git 環境：
```shell
$ git config user.name "YOUR NAME"  
$ git config user.email "YOUR EMAIL"  
```


<br><br> 

## 參考資料 
1. [修改 Git commits 的作者資訊｜Somewhere I Belong](https://yulun.me/2014/git-tips-change-author-and-email-in-previous-commits/) 
2. [How to change the commit author for one specific commit?｜Stack Overflow](https://stackoverflow.com/questions/3042437/how-to-change-the-commit-author-for-one-specific-commit)