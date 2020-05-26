---
title: 【Git】避免 "fatal:LF would be replaced by CRLF" 的錯誤訊息
date: 2019-12-14
categories:
- Version-Control
tags:
- git
- Windows/DOS
--- 

那天在 Windows 做完筆記，準備上傳到 Github Page 時，發現我無法執行 `git add .`，並出現了 LF / CRLF 轉換的錯誤訊息 - `LF will be replaced by CRLF`，告訴我要把 LF 換行符號轉換成 CRLF。

<!--more-->
<br> 

## 問題描述與釐清
看到錯誤訊息時，就猜到是因為欲提交的文件中存在 LF 的換行符號，但在 Windows 所使用的換行符號是 CRLF，再加上我有設定 **safecrlf 為 true**，是**不允許提交有 LR 與 CRLF 混合的檔案**的，因此 git 直接拒絕我將檔案註冊到索引裡：
```shell
$ git config --global core.safecrlf true
```

不過最讓我疑惑的是，我除了設定 **safecrlf 為 true** 外，我另外還有設定 **autocrlf 為 true**。照理來說 **git 會在檢出將換行符號換成 CRLF** 才對，但為什麼我的文件中還會存在 LR？
```shell
$ git config --global core.autocrlf true
```


<br>

## 解決辦法
為什麼會出現 LR 我還是不得其解，不過對於這條錯誤訊息倒是很好解：

### 1. 關掉 safecrlf
剛剛提過，由於我將 safecrlf 設為 true，是不允許提交有 LR 與 CRLF 混合的檔案的，所以被 git 給拒絕了。因此網路上一派解法是直接把 safecrlf 關掉。

```shell
# 允許有 LR 與 CRLF 混合的檔案
$ git config --global core.safecrlf false

# 允許有 LR 與 CRLF 混合的檔案，但會出 warning
$ git config --global core.safecrlf false
```

不過我個人不希望檔案中有 LR 與 CRLF 混合，這在跨平台合作的情況下會是個悲劇...我就被同事坑過...在提交時會滿江紅一片阿(哭 <br>


### 2. 使用指令或編譯器
第二種方法是用指令或編譯器來替換換行符號。先說指令的部分，在使用 Ubuntu 時，若想取代檔案中字元時，我會使用 tr：
```shell
$ tr "\n\r" "\r" < input.file > output.file
```
這條指令的意思是：「將 input.file 中的 \n\r 字串，轉換成 \r 後，寫到 output.file 去」，在只有一份檔案需要轉換的情況，下這條指令是最迅速的，只是可惜的是...我找不到它在 Dos 中相對應的指令 ~~（好吧！我承認我懶得找）~~ <br>

另一種是使用編譯器，我記得 **NodePad++** 做得到，可是我現在改用 VS Code，原本以為更改它的 **files:eol** 設定就可以了，但 git 還是把我擋下來了 QAQ<br>


### 3. 此用 DOS2UNIX
沒辦法使好用最後一招了，還好在 Windows 中找的到現成的 dos2unix (a.k.a fromdos) / unix2dos (a.k.a todos) tool。不僅方便，而且這個方法還可以批次處理，當你有多個檔案的時候需要轉換時，用這個方法會比較快。

1. 下載 DOS2UNIX and UNIX2DOS for Windows ([載點](http://www.bastet.com/uddu.zip))

2. 將解壓縮出來的檔案放到專案根目錄，由於我是要轉 LF 成 CRLF，所以我只將 UNIX2DOS.exe 和 UNIX2DOS.c 兩隻程式搬進去。

3. 輸入以下指令,其中 **\*.md** 是所要轉換的檔案路徑。
    ```shell
    for /f %f IN ( 'dir /b /s *.md' ) DO @unix2dos %f
    ```
    
4. 處理完成後，再把剛剛搬進去的 UNIX2DOS.exe 和 UNIX2DOS.c 程式刪除，別不小心放進 git 裡面了。


<br><br>

## 補充紀錄

### 1. core.autocrlf 
根據 core.autocrlf 的設置規則與建議。
1. **當在 Window 系統上開發時** <br> 應將 core.autocrlf 設為 true，這樣在提交會將程式碼轉換成 LF，但在檢出則將換行符號換回 CRLF。<br>

2. **當在 Linux 或 Mac 系統上開發時** <br> 應將 core.autocrlf 設為 input，由於 Linux 與 Mac 本身就是使用 LF，因此檢出不需要進行轉換，但提交時依舊得使用 LF。<br>
 
 
3. **當專案只會在 Window 系統上開發時** <br> 如果你確定你的專案只會在 Window 上開發與執行，這時將 core.autocrlf 設為 false，因此你提交時會是 CRLF。不過個人實在不建議，畢竟你不能確認你的交接者是否還是使用相同的系統開法。<br>


### 2. core.safecrlf 
前面有提過 safecrlf 設定的差別：
```shell
# 不允許提交有 LR 與 CRLF 混合的檔案
$ git config --global core.safecrlf true
 
# 允許有 LR 與 CRLF 混合的檔案
$ git config --global core.safecrlf false

# 允許有 LR 與 CRLF 混合的檔案，但會出 warning
$ git config --global core.safecrlf false
```

不過我想到一件事情 core.autocrlf = true 會將 CRLF 轉成 LF，此時我要提交時 git 中應該都是 LF 才對，應該不是混合的檔案了，那為何又會被 safecrlf 擋下來呢？

後來在[Seven是為了紀念賽虎的這篇](https://www.jianshu.com/p/2a46dfd3705a)查到解釋：
> 當 core.autocrlf 為 true 或者 input 時，算激活了 eol，此時如果 core.safecrlf 為 true，git 檢查 crlf 轉換是否正常，比如 Windows 平台，core.autocrlf 設置為 true，如果工作區的文件中含有 LF，git 就會拒絕，因為 true的情況下，git 認為工作區應該都是 CRLF 才對啊。

<br><br>

## 總結
簡單來說，若在 window 環境建議設置
```shell
$ git config --global core.autocrlf true
$ git config --global core.safecrlf true
```
<br>
反之，Linux 或 Mac 建議設置
```shell
$ git config --global core.autocrlf input
$ git config --global core.safecrlf true
```

<br> 若遇到 `fatal:LF would be replaced by CRLF` 或 `fatal:CRLF would be replaced by LF` 直接按照錯誤訊息要求，將檔案中錯誤的換行符號替換掉。


<br><br>

## 參考資料 
1. [通过阅读git-config文档理解Git如何使用autocrlf、safecrlf、eol和.gitattributes处理line-ending｜简书](https://www.jianshu.com/p/2a46dfd3705a)
2. [Git 解決出現 warning: LF will be replaced by CRLF ... The file will have its original line endings in your working directory.｜關於網路那些事...](https://adon988.logdown.com/posts/7642074-git-resolves-to-appear-warninglfll-be-replaced-by-crlf-the-file-would-have-its-original-line-endings-in-your-working-directory) 
3. [使用 Git ADD 指令出現錯誤｜Yowko's Notes](https://blog.yowko.com/git-lf-replace-crlf-error/)
4. [HowTo: UNIX / Linux Convert DOS Newlines CR-LF to Unix/Linux Format｜nixCraft](https://www.cyberciti.biz/faq/howto-unix-linux-convert-dos-newlines-cr-lf-unix-text-format/)