---
title: 【Git】commit message 的寫法
date: 2019-03-21
is_modified: false
categories:
- 版本管控
tags:
- Git
--- 

記錄一下，別人的 git commit 都怎麼寫的，我自己的寫法太放飛自我了，每次在翻閱歷史紀錄時都要盯著它看很久 Orz ...
  
之後 commit 如果沒有特殊要求（就是團隊已經定下的書寫規則），盡量就照這份來寫吧 >///<

<!--more-->
<br>

## 必要元素
一個有效的 Git Commit Message 應包含三件事：_What、Why_ 以及 _How_ <br>

### 做了什麼事情（What）
通常是 commit 的第一行，**簡述此次變更的內容、達成的目的**，例如：修了什麼 bug 或新增了什麼功能...之類的。這是 commit 最重要的一部分，通常大家只會看這個，所以必須直白地告訴其他開發者提交這個 commit 的目的，因此若是 commit 裡面包含多的目的，那麼你就應該分開提交。

~~P.S. 還記得之前S老大看到我的 commit 的臉黑的跟什麼一樣，因為我包了太多事情了 XD~~

習慣上，我會為此次變更加上標籤，方便下次閱讀時快速了解這次變更的性質。<br>

### 為什麼要有變更（Why）
記錄變更的動機與原因是什麼了，例如此次改版原因或是 issue 編號之類的，方便之後追朔。<br>

### 使用什麼方法（How）
有需要特別補充的才記錄在訊息中，不用寫的太細節，那是程式碼註解或是程式碼本身該做的事情。

<br><br>
## 書寫原則
基本上依照以下的格式來書寫 commit。

- **第一行（標題）**<br> 
這次變更概要（What）
- **第二行**<br> 
留空
- **第三行**<br> 
這次變更的詳細內容與理由等（Why + How）

<br>

### 第一行
依照 commit 的類別，在開頭加上標籤，大概會長的像下面這樣，有人建議限制文字長度在 **50 字元**左右：
```
[標籤] 這次變更概要

ex. [fix] 修正文字大小
```
<br>

標籤的部份常用的有：
- **fix**：顧名思義，修bug用的
- **add**：新增檔案或功能
- **change**：功能或規格變更。類似 function 的 in/output 不變，但中間可能由於 API 更改導致實作方式變更...之類的。
- **refactor**：整理或重構程式碼
- **remove**：刪除檔案
- **revert**：取消變更
- **merge**：合併 branch
- **update**：更新文件（非程式碼）、db、 json、 md

<br>

另外，這些是我看[這篇](https://github.com/oracle-design/guides/wiki/Git-commit-message-的寫法)有用到的：
- **hotfix**：緊急嚴重的 bug 修正
- **disable**：拿掉某功能
- **upgrade**：版本更新

<br> 個人習慣標籤之前會再加上 branch 或 module 的識別標籤，方便進行功能辨識：

```
[branch/module][標籤] 這次變更概要

ex. [Portal][fix] 修正文字大小
```

<br>

### 第三行
這邊就是寫 Why + How 了，如果是修 issue，順便把 issue 號碼加上去（如果有的話）；加功能的話，則會在這邊備註是哪份文件那一版所要增加的功能。
例如：```TT-1985 follow Common UI v4.2 修改文字大小```

所以整筆 commit 會變成這樣：
```
[fix] 修正文字大小

TT-1985 follow Common UI v4.2 修改文字大小
```
 
<br><br>

## 參考資料
1. [Git commit message 的寫法｜Github](https://github.com/oracle-design/guides/wiki/Git-commit-message-的寫法)
2. [撰寫有效的 Git Commit Message｜Fourdesire Blog](http://blog.fourdesire.com/2018/07/03/%E6%92%B0%E5%AF%AB%E6%9C%89%E6%95%88%E7%9A%84-git-commit-message/)
