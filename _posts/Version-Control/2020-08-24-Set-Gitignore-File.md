---
title: 【Git】設定 .gitignore 忽略清單
date: 2020-09-06 23:58
is_modified: false
categories:
- Version-Control
tags:
- git
--- 

開發過程中，有些檔案不想也不應該放在 git 裡面一起備份，例如：密碼、大型的預訓練模型或者一些編譯過程所產生的的暫存檔...等。
  
這時可以設定忽略的規則來過濾上傳的文件。

<!--more-->
<br><br> 


## 新增過濾腳本

如果有不想放進去的檔案，只需要可以在專案中的 `.gitignore` 設置忽略規則即可。

檔案不存在？那就新增它吧！

```bash
$ touch .gitignore
```

<br><br> 

## 內容寫法

有了檔案就可以開始編輯內容了：

```bash
$ vim .gitignore
```

不過如果你是全新專案或者剛開始編輯 `.gitignore`，可以考慮直接用 [github 的模板](https://github.com/github/gitignore)，至少一些基本的都可以過濾掉，之後再加上你自己的客製化就好。


例如，我會進一步過濾掉下列文件與資料夾：
```
# ide
.idea/
*.iml

# tmp data
*.swp
*~
temporary/

# data
dataset/
output/
models/
```

<br>

基本上語法跟 <span class='highlighting'>Regular Expression</span>  相似，不清楚的可以查查[這篇網誌](https://atedev.wordpress.com/2007/11/23/%E6%AD%A3%E8%A6%8F%E8%A1%A8%E7%A4%BA%E5%BC%8F-regular-expression/)，這篇網誌可是我 Regular Expression 的入門呢，至於我自己的筆記？還躺在草稿夾呢 :joy: 


- `#` : 表註解
- `/` 結尾：表目錄
- `*` : 表示匹配 0 或多個字元
- `?` : 表示匹配 0 或 1 個字元
- `[]` : 表示匹配中括弧內的任一個字元
- `!` : 則是用來表示追蹤特定文件，有可能在前面規則中過濾掉了某個資料夾，但該資料夾下某個文件卻是要追蹤的，就可以用 `!`
 

<br><br> 

 
## 過濾已追蹤的檔案

`.gitignore` 的過濾效果只會在 Untracked 的檔案上顯示效果，若你這檔案已經列入版本控制，也就是之前被 commit 過了，則不受 `.gitignore` 檔案控制。

如果想讓這已經列管的檔案被忽略，可以先解除版制的追踪

```bash
$ git rm -r --cached .
```

然後再 commit 忽略掉這檔案。


<br><br> 

## 參考資料 
1. Fleschier (2018-10-30)。[gitignore的使用及写法](https://fleschier.github.io/2018/10/27/gitignore-learning/) 。檢自 Fleschier 渐寒๑ (2020-07-21)。
2. 高見龍。[【狀況題】有些檔案我不想放在 Git 裡面…](https://gitbook.tw/chapters/using-git/ignore.html) 。檢自 為你自己學 Git (2020-07-21)。
2. github。[A collection of useful .gitignore templates](https://github.com/github/gitignore) 。檢自 gitignore (2020-07-21)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-09-06</summary>
  <ul class="timestamp">
    　<li>2020-09-06 發布</li>
    　<li>2020-08-24 完稿</li>
    　<li>2020-07-21 起稿</li>
  </ul>
</details>