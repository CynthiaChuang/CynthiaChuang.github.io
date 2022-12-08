---
title: Windows Powershell | 解決 Git 中文亂碼
date: 2022-07-10
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Windows/DOS
- 工具安裝與部署
- Git
- 終端機
--- 
 
最近用 Windows 10 工作的時間比較多，因為被關在家咩。
 
自從上次安裝 [WSL](https://cynthiachuang.github.io/WSL-Install-and-Use-the-Ubuntu-subsystem-in-Win10/) 時使用了 PowerShell 後，發現它可在 Windows 上下些常用的 Linux 的指令、基本可以滿足我的基本需求後，我幾乎放棄了 Windows 的命令提示字元。 
 
但它有個麻煩的問題...雖然開發過程中少有用到中文，但像是我在寫網誌的時候就會用到，此時就會發現中文變成了亂碼...:cry: 

<!--more-->
<br class="big">

不是我要說...，但 diff 長這樣是要我通靈喔！

<p class="illustration">
    <img src="https://i.imgur.com/kzQg3NE.png" alt="中文 diff 呈現亂碼">
</p>



## 問題確認
先確認下問題，當我建立一份名為 `測試.txt` 的新檔案後，嘗試下達 `git status` 與 `git diff` 會發現，涉及中文的部分都出現了亂碼。

看起來應該是編碼問題？觀察下一個字元對應到了三個編碼，感覺像是 UTF-8 的儲存格式，但直接沒有轉換顯示，直接就用印出來了？

<p class="illustration">
    <img src="https://i.imgur.com/ryKCGxQ.png" alt="中文 status 呈現亂碼">
</p>

<p class="illustration">
    <img src="https://i.imgur.com/8ghsyVP.png.png" alt="中文 diff 呈現亂碼">
</p>

<br class="big">

不過，我如果用 cat 來看內文的話，一切正常：

<p class="illustration">
    <img src="https://imgur.com/DstRxNK.png" alt="cat 指令中文正常呈現">
</p>

<br class="big">

推測兩個可能原因：
1. 終端機的編碼設定
2. git 的問題

第二點我不太確定，因為我在 Linux 上時不需要這種設定，可能需要更多的嘗試來確認問題。所以我決定先來調整終端機的編碼設定，如果失敗了，再來研究 git 這邊。



## 預設使用 UTF-8 編碼
根據 [Stackoverflow](https://stackoverflow.com/questions/57131654/using-utf-8-encoding-chcp-65001-in-command-prompt-windows-powershell-window) 上所提到的，先來確認 `[console]::OutputEncoding`、 `[console]::InputEncoding`、 `$OutputEncoding` 這三個變數：

<br class="big">

可以發現 `[console]::OutputEncoding`、`[console]::InputEncoding` 兩個變數的顯示是使用 Big5：

<p class="illustration">
    <img src="https://imgur.com/cfwWOE6.png" alt="[console]::OutputEncoding 編碼為 950">
</p>

<p class="illustration">
    <img src="https://imgur.com/8EwxQjP.png" alt="[console]::InputEncoding 編碼為 950">
</p>

不過 `$OutputEncoding` 這個變數結果卻呈現 UTF-8。兩個跟 Output 相關的編碼，一個呈現 Big5、一個用 UTF-8，所以現在是歸誰管？

<p class="illustration">
    <img src="https://imgur.com/YWXzqix.png" alt="[console]::InputEncoding 編碼為 65001">
</p>


<br class="big">

最後還是決定先繼續往下做，先把<mark>所有編碼都改成 UTF-8 再說</mark>。所以接下來先來查查，個人設定檔的配置路徑，這個值可以藉由 `$PROFILE` 變數得知：

<p class="illustration">
    <img src="https://i.imgur.com/VBVMCzv.png" alt="個人設定檔的配置路徑">
</p>

根據所查到的路徑前往修改檔案，如果 `Microsoft.PowerShell_profile.ps1` 檔案不存在就直接建立一個，並添增下列內容：

```bash
$OutputEncoding = [console]::InputEncoding = [console]::OutputEncoding = [Text.UTF8Encoding]::UTF8
```

<br class="big">

完成後啟動一個新的視窗，理論上預設編碼就都會呈現 UTF-8 了。

<p class="illustration">
    <img src="https://i.imgur.com/NG0tk2R.png" alt="[console]::OutputEncoding 編碼為 65001">
</p>

<p class="illustration">
    <img src="https://i.imgur.com/MGod6ui.png" alt="[console]::InputEncoding 編碼為 65001">
</p>


<br class="big">

題外話，那天跟同事討論了**踩坑排行榜**，雖然我不像前面兩位同事是踩坑專業戶，該踩得、不該踩得坑全都一個不落；但該掉的坑我似乎也沒有幸運躲過，你看這不就來一個錯誤訊息了嗎？

在我修改完個人設定檔後，重啟 PowerShell 時跳出了下列錯誤訊息：

<p class="illustration">
    <img src="https://i.imgur.com/6DEh2VL.png" alt="因為這個系統上已停用指令碼執行，所以無法載入...">
</p>


<br class="big">

查了下原因，發現是因為是 Windows 執行安全性設置所導致的，在 PowerShell 預設的執行原則是 **Restricted**，也就是限制原則，它**阻止所有 script 的運行**，包含剛剛的 PowerShell 配置文件 (.ps1)。

<p class="illustration">
    <img src="https://i.imgur.com/hA1ourp.png" alt="因為這個系統上已停用指令碼執行，所以無法載入...">
</p>

所以需要藉由 `Set-ExecutionPolicy` 指令重新調整執行原則，相關指令設置說明的說明可以看[這裡](https://docs.microsoft.com/zh-tw/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2#powershell-execution-policies)。這邊我們把執行原則上調到 <mark>RemoteSigned</mark>，它可以**不需要簽署就執行在本機所撰寫的 script**，但若是從下載的 script 就必須經過簽署後才可執行。

喔，對了 `Set-ExecutionPolicy` 這條指令的執行，必須具備**系統管理員身份**。所以我又重起了一個以系統管理員身份執行的 PowerShell，並按 `A` 同意變更執行原則：
<p class="illustration">
    <img src="https://i.imgur.com/ZOZzFmu.png" alt="Set-ExecutionPolicy RemoteSigned">
</p>


<br class="big">

修改完成後，再次看看 diff 結果，結果仍然是亂碼 :cry: 

<p class="illustration">
    <img src="https://i.imgur.com/8ghsyVP.png.png" alt="中文 diff 呈現亂碼">
</p>

不過 cat 指令的結果依舊正常，我還疑這部分應該是 `$OutputEncoding` 所控制的，就是我系統一開始就呈現 UTF-8 的那個變數。

<p class="illustration">
    <img src="https://imgur.com/DstRxNK.png" alt="cat 指令中文正常呈現">
</p>



## 設定 git encoding
查了下，其實我的中心思想沒有錯，設定好 UTF-8 編碼即可。

不過好像 Git for Windows 內建的 git.exe 工具，是走 UNIX-like 作業系統環境下，所以非英語系語言的文字要另外設定。這點 git 可以直接透過指令將編碼調整成 UTF-8


```bash
# encoding of status output 
$ git config --global core.quotepath false

# GUI encoding
$ git config --global gui.encoding utf-8

# encoding of commit messages
$ git config --global i18n.commit.encoding utf-8 

# encoding of log outputs
$ git config --global i18n.logoutputencoding utf-8 
```

<br class="big">

這時看看 status，可以正常顯示檔名了：

<p class="illustration">
    <img src="https://i.imgur.com/cM1IZvm.png" alt="git status 指令中文正常呈現">
</p>


但是涉及 log、 diff 跟使用者名稱的中文還是亂碼：

<p class="illustration">
    <img src="https://i.imgur.com/TvOigL2.png" alt="git 其他指令涉及中文仍呈現亂碼">
</p>


### 設定 `LESSCHARSET` 環境變數
這時需要再設定 `LESSCHARSET` 環境變數，用於設定 character set：
```bash
# Git uses the less pager by default. This command is to convert the encoding of less commands to UTF-8.
$ export LESSCHARSET=utf-8 
```

不過 export 的設法僅顯於目前這個 process，一旦重啟視窗設定就會跳掉，所以如果要永久生效的話，還是要取改剛剛的 `Microsoft.PowerShell_profile.ps1`。直接在最後加上

```bash
$env:LESSCHARSET='utf-8'
```

最後再試試看指令，終於能正常呈現了：

<p class="illustration">
    <img src="https://i.imgur.com/wQNMA0o.png" alt="git 其他指令中文正常呈現">
</p>

<br class="big">


是說，如果是設定 git 指令可以解掉大半，那麼在命令提示字元上效果應該也是可行的？果不其然，在命令提示字元上，status 已經正常顯示，而 log、diff 跟使用者名稱則還是亂碼。因此這個時候，僅需設定命令提示字元上的 `LESSCHARSET` 即可。

命令提示字元的變數的話，就要去改**登錄編輯程式**，這可以直接從搜尋欄輸入 **regedit** 叫出。然後依序尋找 `HKEY_LOCAL_MACHINE` > `Software` > `Microsoft` > `Command Processo`，並新增變數：

<p class="illustration">
    <img src="https://i.imgur.com/vq5gFiJ.png" alt="新增命令提示字元環境變數">
</p>



### 設定 `LC_ALL`
如果不設定 `LESSCHARSET` 環境變數，另一個方法是改設定 `LC_ALL` 語言環境變數，得益於 git 是 UNIX-like 的，所以這方法也可行。

```bash
$ SET LC_ALL=C.UTF-8
```

如果要每次啟動都生效的話，就改用：
```bash
$ SETX LC_ALL C.UTF-8 
```

<br class="big">

完成後，就可以看到顯靈結果啦！再也不用通靈看 diff 了：

<p class="illustration">
    <img src="https://i.imgur.com/wQNMA0o.png" alt="git 其他指令中文正常呈現">
</p>




## 參考資料 
1. Michelle Chen (2021-12-20)。[[Windows] 程式設計教學：使用 PowerShell](https://opensourcedoc.com/windows-programming/powershell/)。檢自 開源技術教學網 (2022-06-24)。
2. 協同撰寫。[PowerShell](https://zh.wikipedia.org/wiki/PowerShell)。檢自 維基百科 (2022-06-24)。
3. Neil Tsai (2020-02-21)。[Command Prompt / Windows Powershell 預設使用 UTF-8 編碼 (Windows 10)](https://www.thinkinmd.com/post/2020/02/21/command-prompt-and-windows-powershell-default-use-utf-8/)。檢自 Thinkin Markdown (2022-06-24)。
4. Ray (2021-01-06)。[解決 Windows 上輸入指令出現「因為這個系統上已停用指令碼執行，所以無法載入...」的問題](https://israynotarray.com/other/20200510/1067127387/)。檢自 是 Ray 不是 Array (2022-06-24)。
5. sysin (2021-11-21)。[Windows 10 环境变量：通过 CMD 和 PowerShell 写入环境变量](https://sysin.org/blog/windows-env/)。檢自 SYStem INside (2022-06-24)。
6. (2021-11-21)。[Setting the Encoding on Windows](https://support.huaweicloud.com/intl/en-us/usermanual-codehub/devcloud_hlp_0954.html)。檢自 HUAWEI CLOUD (2022-06-24)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-07-10</summary>
  <ul>
    <li>2022-07-10 發布</li>
    <li>2022-07-10 完稿</li>
    <li>2022-06-24 起稿</li>
  </ul>
</details>
