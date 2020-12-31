---
title: 【Git】用 git stash 命令暫存目前的修改工作
date: 2020-01-15
is_modified: false
categories:
- 版本管控
tags:
- Git
--- 

大家應該或多或少遇過這樣的情境：
> 當您開發作到一半時，遇到緊急的 issue 要先修。  
> 或是遇到老闆要插單，要求先完成哪個 function 。  
 
人家是衣食父母也不好不甩人家，只好先擱置手邊的工作啦...
<!--more-->

<br>

此時，有幾個方式可以保留手邊的工作進度。

## 1. commit 之後再 reset
這是我最常使用的的方法，比較是適合用在接下來的工作是在不同的分支上時。

就是別管那多，把所有東西都先存下來再說：
```shell
$ git add --all
$ git commit -m "temporarily"
```

等在其他分支工作完後，再回到這個分支上，把暫存的 commit 解開繼續做：

```shell
$ git reset HEAD^
```

<br><br>
## 2. git stash
除了 commit 之後再拆這個方法外，也可以使用 git 的指令，將檔案放到<span class='highlighting'>暫存區</span>裡面：

<br>

### push / save

```shell
$ git stash

Saved working directory and index state WIP on master: 3a9842d
```
 
<br>

若是你的此次修改有新增加檔案，或是存在 <span class='highlighting'>untracked</span> 狀態的檔案，則應該在指令後 `-u` 指定包含untracked 狀態的檔案：

```shell
$ git stash -u 
```

<br><br>

我在網路上找資料的時候，發現有三個指令，`git stash` / `git stash push` / `git stash save`，都可以達到相同的目的。


其中 `git stash push` 是最標準的用法，也是 `git stash` 預設指令，換句話說，當你調用 `git stash` 不帶任何參數時，其效用等於`git stash push`。

最後一個 `git stash save` 在文件中寫明它不推薦使用該參數，與 `git stash push` 最大的不同是它不支援路徑參數。

<br>

### list / show
當你完成其他工作完後，想把暫存區的工作進度時找回來時，可以先用 `list`，看看暫存區有那工作：

```shell
$ git stash list

stash@{0}: WIP on master: 3a9842d 
stash@{1}: WIP on master: 3a9842d  
```
<br> 

像這樣表示暫存區存在兩個暫存的工作，我們可以使用 `show -p` 來查看指定工作的 diff：
```shell
$ git stash show -p stash@{0}
```

<br>

### pop / drop
若確定所要回復的工作進度時，就可以使用 `pop` 來恢復工作進度
```shell
$ git stash pop stash@{0}
```
<br>

若是某個暫存的工作進度確定不要，則可以使用 `drop` 來丟棄：
```shell
$ git stash drop stash@{1}
```

 
<br><br> 

## 參考資料 
1. [Git｜git-stash Documentation](https://git-scm.com/docs/git-stash)
2. [Stash（暫存）｜連猴子都能懂的Git入門指南｜貝格樂（Backlog）](https://backlog.com/git-tutorial/tw/reference/stash.html)
3. [【狀況題】手邊的工作做到一半，臨時要切換到別的任務｜為你自己學 Git 高見龍網站](https://gitbook.tw/chapters/faq/stash.html) 
4. [[Git] 原來 git stash save 已經被棄用了｜EPH 的程式日記
 ](https://ephrain.net/git-%E5%8E%9F%E4%BE%86-git-stash-save-%E5%B7%B2%E7%B6%93%E8%A2%AB%E6%A3%84%E7%94%A8%E4%BA%86/)