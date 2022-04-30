---
title: 在 Windows 使用 SSH 金鑰登入 Linux
date: 2021-03-07 23:33
is_modified: false
categories:
- 環境設定與指令
tags:
- SSH
- Linux/Unix
- Windows/DOS
--- 

因為之前的[資安政策升級](/Linux_SSH_Access_Control)，所以伺服器連線都需要使用金鑰登入。不過小尷尬的是，我手邊有一台 OA 設備是 Windows 的...所以我在 Windows 上要怎用金鑰連線？

<!--more-->

因為之前已經在 [《使用 SSH 金鑰與 GitHub 連線》](/Generating-a-Ssh-Key-and-Adding-It-to-the-Github) 與  [《設定 Linux 使用 SSH Key-based 登入驗證方式》](/Configuring-SSH-Key-Based-Authentication-on-a-Linux)兩篇文章中已經介紹過，所以這邊就跳過金鑰的產生，直接來看看如何在 Windows 上設置連線吧。

<br><br>

##  軟體準備

在開始前需要先做一些 SSH 使用的軟體。習慣上，在 Windows 使用 SSH，都會安裝額外軟體，最常見的軟體的大概是 [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)。
    
<center> <img src="https://i.imgur.com/4TszQu2.png" alt="PuTTY 載點"></center>
<center  class="imgtext">PuTTY 載點（圖片來源: <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html"  class="imgtext">PuTTY 官網</a>）</center>
<br>

另外，如果要產生或轉換金鑰，可能需要安裝 [PuTTYgen](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)。

<center> <img src="https://i.imgur.com/DI4HVM4.png" alt="PuTTYgen 載點"></center>
<center  class="imgtext">PuTTYgen 載點（圖片來源: <a href="https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html"  class="imgtext">PuTTY 官網</a>）</center>
<br>

不過，據我所知，Windows 10 好像有內建的 OpenSSH 可以用，如果打算使用 OpenSSH，PuTTY 跟 PuTTYgen 搞不好可以不用安裝了。
 
<br><br>

## 產生私鑰

因為我之前已經在 Linux 產生過金鑰了，所以我直接把私鑰複製過來，放在 `c:/Users/user_name/.ssh` 下。這個路徑跟 Linux 中 `~/.ssh` 的用途類似，OpenSSH 會來這邊金鑰。

但如果是要使用 PuTTY 進行連線，則需要透過 PuTTYgen 將在 Linux 產生的私鑰轉換成 Windows 系統可識別的 `*.ppk` 才可正常使用。

1. **選擇要轉換的金鑰**   
    <center> <img src="https://i.imgur.com/l4zHGl8.png?1" alt="選擇要轉換的金鑰"></center>
    <br>
    
2. **儲存** `*.ppk`   
    <center> <img src="https://i.imgur.com/vMGh6Qm.png?1" alt="儲存 *.ppk"></center>
    <br>

如果是要在 Windows 中產生金鑰，可以考慮使用 PuTTYgen 或是有 OpenSSH 的可以直接使用內建 ssh-keygen 來產生金鑰：
- [PuTTYgen產生金鑰 (Windows使用)](http://blog.faq-book.com/?p=1444)
- [OpenSSH 金鑰管理](https://docs.microsoft.com/zh-tw/windows-server/administration/openssh/openssh_keymanagement)
    
 
<br><br>

## 執行連線

既然前面都提到了 PuTTY 與 OpenSSH 兩種連線方式，因此這邊分兩個部份來介紹：

### PuTTY

1. **設定連線資訊**  
    在開啟 PuTTY 後，選擇 Category (類別) 中的 Session (工作階段)，並填寫指定欄位：
    - **Host Name**：輸入連線目標的使用者帳號與主機名稱。
    - **Port**：預設 Port 是 22。
    - **Connection type**：選取 SSH。

    <br>
    <center> <img src="https://i.imgur.com/V8UNQzK.png?1" alt="選擇要轉換的金鑰"></center>
    <br>
2. **設定 SSH 金鑰**  
    完成資訊設定後，接下來設定 SSH 金鑰，先將 Category (類別) 切換到 Connection (連線)，並展開 SSH 選擇 Auth (身份驗證)。
    
    在右邊窗格中，點擊 Browse，指定選擇 `*.ppk`。最後點擊 Open，進行連線。

    <center> <img src="https://i.imgur.com/KVr1Rh0.png?1" alt="選擇要轉換的金鑰"></center>
    <br>

<br>
    
### OpenSSH

至於 OpenSSH 的連線其實跟在 Linux 上一樣，只要把金鑰放到指定路徑下，也就是 `c:/Users/user_name/.ssh`，並依照需求在 `config` 中，指定 `Host`、 `IdentityFile`。

最後使用 ssh 進行連線：
```bash
$ ssh 帳號@主機
```


<br><br> 

## 參考資料 
1. 腳印哥 (2019-07-06)。[Windows 使用 SSH 金鑰免密碼登入 Linux](https://www.footmark.info/linux/centos/windows-ssh-nopassword-linux/)。檢自 MIS 腳印 (2020-02-01)。
2. [使用 PuTTY 從 Windows 連線至您的 Linux 執行個體](https://docs.aws.amazon.com/zh_tw/AWSEC2/latest/UserGuide/putty.html)。檢自 Amazon Elastic Compute Cloud (2020-02-01)。
3. Derek (2011-05-21)。[PuTTYgen產生金鑰 (Windows使用)](http://blog.faq-book.com/?p=1444) 。檢自 FAQ Book (2020-02-01)。
4. [PuTTYgen 將 .pem 金鑰檔案轉為 .ppk 格式教學](https://officeguide.cc/putty-convert-pem-to-ppk-tutorial/) 。檢自 Office 指南 (2020-02-01)。

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-03-07</summary>
  <ul>
    <li>2021-03-07 發布</li>
    <li>2021-02-01 完稿</li>
    <li>2021-02-01 起稿</li>
  </ul>
</details>