---
title: 【Git】使用 SSH 金鑰與 GitHub 連線
date: 2020-12-23 21:50
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Git
- GitHub
- SSH
--- 

好久沒發網誌，一發網誌立刻收到 GitHub 的通知，說它們即將在明年廢除 https 上 code 的方式，也就是輸入帳密的方式。只好鼻子摸摸來改成使用 SSH 金鑰了。

<!--more-->

> **[GitHub] Deprecation Notice**  
> Basic authentication using a password to Git is deprecated and will soon no longer work. Visit https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information around suggested workarounds and removal dates.



## 配置 GitHub 金鑰
之前在使用 [SSH Key-based 登入](/Configuring-SSH-Key-Based-Authentication-on-a-Linux/)時，有稍微提過 SSH 認證與設置的步驟，在這邊的流程也不例外：

1. 產生金鑰對。
2. 將產生的 Pub key 放到遠端倉庫，也就是我們的 GitHub。

<p class="illustration">
    <img src="https://i.imgur.com/XkG1sE2.png" alt="SSH 連線運作方式">
    SSH 連線運作方式（圖片來源: <a href="https://sebastien.saunier.me/blog/2015/05/10/github-public-key-authentication.html">Sébastien Saunier</a>）
</p>


### 產生金鑰對
我習慣所有的金鑰對收集起放在 `~/.ssh` 這個目錄下。若該目錄不存在，就自己建一個，並設定正確的權限：

```bash
$ mkdir ~/.ssh 
$ chmod 700 ~/.ssh
```
<p class="paragraph-spacing"></p>

然後使用 `ssh-keygen` 這個指令產生金鑰，依照[教學文件](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)的提示，我們選擇 **ED25519** 作為金鑰的加密演算法，並使用電子郵件作為標籤，創建一個新的 SSH 金鑰。 

```bash
$ ssh-keygen -t ed25519 -C "your_email@example.com"
Generating public/private ed25519 key pair.
```

不過，`ED25519` 是後來推出的加密演算法，可能會發生舊系統無法生成的狀況。因此若系統不支援，可以改用 `rsa`，並指定金鑰長度為 `4096`：

```bash
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
<p class="paragraph-spacing"></p>

這邊稍微補充下，在 `ssh-keygen` 中常用參數如下：
- `-t`：指定金鑰的加密演算法，預設使用 `SSH2d` 的 `rsa`。 
- `-f`：指定金鑰的檔名，預設檔名會隨演算法而變動，例如使用 `rsa` 加密時，其檔名預設為 `id_rsa`（私鑰`id_rsa`，公鑰`id_rsa.pub`）。這階段沒改沒關係，等等還會在詢問。
- `-P`：提供舊密碼，空表示不需要密碼（-P ‘’）
- `-N`：提供新密碼，空表示不需要密碼(-N ‘’)
- `-b`：指定金鑰長度（bits）。
- `-C`：提供一個新標籤。

<p class="paragraph-spacing"></p>


在產生金鑰的過程中，會詢問 3 個問題，如果沒有特殊需求可以全部使用預設值（按 Enter）就好：


```bash
Enter file in which to save the key (/home/username/.ssh/id_ed25519): /home/username/.ssh/github_key
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
```

1. **Enter file in which to save the key**  
    第 1 個問題是問你金鑰儲存的位置與檔名，預設檔名是前面所提到 `id_ed25519`。不過，這命名無法表明金鑰的用途，所以習慣上我會更改檔名，例如 `github_key`。

2. **Enter passphrase**  / **Enter same passphrase again**  
    第 2 跟 3 個問題則是詢問是否指定金鑰保護密碼，若有設定密碼的話，之後使用每次使用時，這把金鑰時就要輸入密碼，因此請務必牢記，不然這把金鑰就廢了 XDDD

    如果之後想修改金鑰密碼的話，可以透過 `ssh-keygen` 來設定。
    
    
<p class="paragraph-spacing"></p>

完成上述設定後，會看到 fingerprint 與 randomart ，就代表產生成功了

```bash
Your identification has been saved in /home/username/.ssh/github_key.
Your public key has been saved in /home/username/.ssh/github_key.pub.
The key fingerprint is:
......  your_email@example.com
The key’s randomart image is:
+---[ED25519 256]----+
|                 |
|                 |
...
|                 |
|                 |
+----[SHA256]-----+
```


### 設定金鑰代理
這個步驟是給有設定金鑰密碼的人，如果你有設定密碼，但又不想每次使用金鑰就要輸入一次，你可以考慮設定金鑰代理：
```bash
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/github_key
```

如果你沒有設定密碼，或是像我一樣不想起一個 agent 在背景跑，寧願每次輸入密碼的，可以跳過這個步驟。


### 新增公鑰到你的遠端倉庫
產生的金鑰有兩把，一把是**公鑰（Public Key）**、一把是**私鑰（Private Key）**。
1. **公鑰**：這把是對外公開的金鑰，也就是我們要上傳的金鑰。
2. **私鑰**：這把則是放在自己電腦的金鑰，它等同於你的密碼。

<p class="paragraph-spacing"></p>
 
知道要上傳的是那把鑰匙後，就可以依照下列方式進行設定
1. **檢視並複製生成的公鑰**：  
    ```bash
    $ cat ~/.ssh/github_key.pub 
    ssh-ed25519 ........ your_email@example.com
    ```
    <p class="paragraph-spacing"></p>
    
2. **新增金鑰**：  
    接下來登陸你的 GitHub 帳號，點選你的頭像 → `Settings` 。
    
    <p class="illustration">
        <img src="https://i.imgur.com/jJ6XpEQ.png?1" alt="Settingsl">
    </p>
    
    左欄點選 `SSH and GPG keys`  →  `New SSH key`。
    
    <p class="illustration">
        <img src="https://i.imgur.com/rjIhVFM.png" alt="Settingsl">
    </p>
    
    最後把剛剛複製下來的公鑰，貼在下方，並為這把金鑰取個方便管理的名字。
    <p class="illustration">
        <img src="https://i.imgur.com/pmGdEeE.png" alt="Settingsl">
    </p>
    
3. **連線測試**  
    完成金鑰上傳後，驗證下這組金鑰是不是正常工作，如果一切正常，應該會看到下面的訊息：
     
    ```bash
    $ ssh -T git@github.com
    Hi XXX! You’ve successfully authenticated, but GitHub does not provide shell access.
    ```     
    <p class="paragraph-spacing"></p>
    
    不過，很不幸的我看到是這樣：
    ```bash
    $ ssh -T git@github.com
    Permission denied (publickey).
    ```

    看起來被拒絕了。因為沒有足夠的資訊可供參考，所以我加上的 `-v`，試圖獲取多的資訊：
    ```bash
    $ ssh -vT git@github.com
    debug1: Trying private key: /home/Cynthia_Chuang/.ssh/id_rsa
    debug1: Trying private key: /home/Cynthia_Chuang/.ssh/id_dsa
    debug1: Trying private key: /home/Cynthia_Chuang/.ssh/id_ecdsa
    debug1: Trying private key: /home/Cynthia_Chuang/.ssh/id_ed25519
    debug1: No more authentication methods to try.
    ```
    <p class="paragraph-spacing"></p>
    
    發現它在連線測試的時候，使用的預設的檔名，但我剛剛在建立金鑰時，更改了金鑰的名稱，難怪找不到，這時必須在配置一個 `config` 檔：
    ```bash
    $ vim ~/.ssh/config    
    ```
    
    在檔案中加上下列內容，其中 IdentityFile 是我們剛剛建立的金鑰：
    ```
    Host github.com
     HostName github.com
     User CynthiaChuang
     IdentityFile ~/.ssh/github_key
    ```
    
    保存後重新測試連線，Bingo！
    ```bash
    $ ssh -T git@github.com
    Hi CynthiaChuang! You've successfully authenticated, but GitHub does not provide shell access.
    ```        
    
 

## 更改連線協議
完成金鑰的產生與配置後，必須更改連線的協議，否則即便配置 SSH，還是會走 https 的傳輸協定。

```bash
$ git remote -v
origin  https://github.com/user_name/project.git (fetch)
origin  https://github.com/user_name/project.git (push)
```     

說是更改連線協議，不過這其實就是**更換遠端伺服器倉庫網址**，Github 對於不同的協定走的是不同的伺服器倉庫網址。因此網址會從 https 改成 git 開頭：

```diff
- https://github.com/user_name/project.git
+ git@github.com:user_name/project.git
```


完整的指令如下：
```bash
$ git remote set-url origin git@github.com:user_name/project.git
```

是也可以[修改 config 檔案來達成該換遠端伺服器倉庫](/Git-Remote-Set-Url)，不過個人偏愛指令的設定方式。

<p class="paragraph-spacing"></p>

完成後，可以試著上傳 commit，並查看金鑰的使用狀況，應該會發現金鑰有了使用紀錄：

<p class="illustration">
    <img src="https://i.imgur.com/T0MzThW.png" alt="The Learning Model">
</p>



## 在 Window 使用 SSH 金鑰
基本上所有動作跟在 Linux 一樣，唯一的差異就是在 SSH 金鑰存放的位置是在 `c:/Users/user_name/.ssh`。



## 參考資料 
1. [Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 。檢自 GitHub Docs (2020-12-22)。
2. [Adding a new SSH key to your GitHub accountt](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) 。檢自 GitHub Docs (2020-12-22)。
3. [Switching remote URLs from HTTPS to SSH](https://docs.github.com/en/free-pro-team@latest/github/using-git/changing-a-remotes-url#switching-remote-urls-from-https-to-ssh) 。檢自 GitHub Docs (2020-12-22)。
4. (2018-05-29)。[ssh-keygen常用參數詳解](https://www.itread01.com/content/1527598583.html) 。檢自 IT 閱讀 (2020-12-22)。
5. Mike Mazur (2009-08-06)。[ssh - How do I change my private key passphrase?](https://serverfault.com/questions/50775/how-do-i-change-my-private-key-passphrase) 。檢自 Server Fault (2020-12-22)。
6. 冰海 (2018-01-14)。[github提示Permission denied (publickey)，如何才能解决？ - 冰海的回答](https://www.zhihu.com/question/21402411/answer/295576230)。檢自 知乎 (2020-12-22)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-12-23</summary>
  <ul>
    <li>2020-12-23 發布</li>
    <li>2020-12-23 完稿</li>
    <li>2020-12-22 起稿</li>
  </ul>
</details>