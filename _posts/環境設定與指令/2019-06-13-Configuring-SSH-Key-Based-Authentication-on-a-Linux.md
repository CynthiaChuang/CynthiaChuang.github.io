---
title: 設定 Linux 使用 SSH Key-based 登入驗證方式
date: 2021-01-12 21:26
is_modified: true  
categories:
- 環境設定與指令
tags:
- Linux/Unix 
- SSH
--- 

在寫 Shell Script 時被 expect 跳脫搞到快瘋掉，還是來 Sign Key 比較快... Orz 
 
不過本來就應該避免把密碼 Hard Code 在程式碼當中啦，當初有確定這份 Script 只有我要用，不會出我電腦，才放心用 Hard Code 的。但現在既然有可能出我電腦，還是用金鑰做認證登錄比較保險。

<!--more-->
<br> 

## SSH Key-based 登入驗證

SSH Key-based 登入驗證方式的正式名稱應該叫 — **SSH 公開金鑰認證（Public Key Authentication）**，設定完成後就可以免密碼登入了。

基本上就兩個步驟：
1. 產生 SSH 登入用的金鑰
2. 將產生的 Pub key 放到伺服器的 `authorized_keys` 清單中

<br>

### 1. 產生金鑰
先切換到 `~/.ssh` 目錄下，通常金鑰都會收集起放在這個目錄下。若該目錄不存在，就自己建一個，並設定正確的權限：
```bash
$ mkdir ~/.ssh 
$ chmod 700 ~/.ssh
```

然後以 `ssh-keygen` 產生金鑰：
```bash
$ ssh-keygen
```
<br>

在產生金鑰的過程中，會詢問一些問題，如果沒有特殊需求可以全部使用預設值（按 Enter）就好：

```bash
1. Enter file in which to save the key (/home/username/.ssh/id_rsa): /home/username/.ssh/tn_key

2. Enter passphrase (empty for no passphrase): 

3. Enter same passphrase again: 
```
第 1 個問題是問你金鑰儲存的位置與檔名，預設檔名是 id_rsa ，不過這實在看不出這把金鑰的用途，所以習慣上我會更改檔名。

第 2 跟 3 個問題則是指定金鑰保護密碼。若有設定密碼的話，之後每次使用這把金鑰時就要輸入密碼，因此請務必牢記密碼。別像我一樣，我有一次設完密碼後，數個月沒登入就忘記密碼了，只好在重新產生一把金鑰惹...
(っ╥╯﹏╰╥c)

<br>

最後看到 fingerprint 與 randomart ，就代表產生成功了
```bash
Your identification has been saved in /home/username/.ssh/tn_key. 
Your public key has been saved in /home/username/.ssh/tn_key.pub.
The key fingerprint is:
.....
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|                 |
...
|                 |
|                 |
+----[SHA256]-----+
```

<br>

###  2. 將公開金鑰上傳

產生的金鑰有兩把一把是公開金鑰（Public Key）、一把是私密金鑰（Private Key）。
1. **公開金鑰（Public Key）**：這把是對外公開的金鑰，之後把它<mark>上傳到伺服器上使用</mark>。
2. **私密金鑰（Private Key）**：這把則是<mark>放在自己電腦</mark>的金鑰，它等同於你的密碼，請務必保護好。此外，私鑰在使用時，權限僅能是 `400`、 `600` 或 `700`，否則會出現下列的錯誤訊息：
    
    > Permissions for {{filename}}  are too open.<br>
    > It is required that your private key files are NOT accessible by others.<br>
    > This private key will be ignored.

<br>

上傳金鑰的方法有幾種，個人偏好第 1 種與第 2 種，因為一條指令就搞定了：

```bash
$ ssh user@host 'mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys' <  /home/username/.ssh/tn_key.pub
```
<br>

也可以用 `ssh-copy-id` ，指令最短：
```bash
$ ssh-copy-id -i ~/.ssh/tn_key.pub user@host
```
<br>

真的不想記指令的話，就把金鑰傳上去，在寫過去 `authorized_keys` 。不過說真的這條就是把第 1 種上傳方式拆開下而已：
```bash
$ scp ~/.ssh/tn_key.pub user@host:~/.ssh/
$ ssh user@host
$ cat .ssh/tn_key.pub >> .ssh/authorized_keys
```
 
<br> 

最懶的大概是直接複製貼上！？
```bash
$ cat ~/.ssh/tn_key.pub  #把出現的 ssh-rsa 複製起來
$ ssh user@host
$ vi .ssh/authorized_keys #把複製的 ssh-rsa 貼上
```

<br>

### 3. 停用密碼認證登入

如果公開金鑰認證設定完成後，想關閉密碼登入的方式，可以修改伺服器上的  `/etc/ssh/sshd_config` 的設定
```diff
# Change to no to disable tunnelled clear text passwords
PubkeyAuthentication yes
- # PasswordAuthentication yes
+ PasswordAuthentication no
```
把 **PasswordAuthentication** 註解打開並將值改成 `no`，另外檢查 **PubkeyAuthentication** 是否為  `yes`，修改完記得重起 sshd 。

```bash
$ systemctl restart sshd
```
<br> 

是說我有點好奇，如果把 PasswordAuthentication 跟 PubkeyAuthentication 同時設成 no 會出現什麼狀況？但我慫（台語），而且機台在遠端，所以沒敢嘗試，要是搞壞就真的 GG 了  (つ﹏⊂)

<br><br> 

## 參考資料 
1. Tsung (2005-12-28)。[ssh keygen 免輸入密碼](https://blog.longwin.com.tw/2005/12/ssh_keygen_no_passwd/)。檢自 Tsung's Blog (2019-06-13)。
2. G. T. Wang (2014-05-18)。[SSH 公開金鑰認證：不用打密碼登入 Linux 設定教學，安全又方便](https://blog.gtwang.org/linux/linux-ssh-public-key-authentication/)。檢自 G. T. Wang (2019-06-13)。
5. 程序新视界 (2018-01-18)。[Linux ssh 重启无效_程序新视界](https://blog.csdn.net/wo541075754/article/details/79092281)。檢自 CSDN博客 (2021-01-08)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2021-01-12</summary>
  <ul class="timestamp">
    　<li>2021-01-12 更新：補上重起 sshd 指令</li>
    　<li>2019-06-13 發布</li>
  </ul>
</details>