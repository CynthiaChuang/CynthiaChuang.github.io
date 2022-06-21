---
title: 限制 Linux 的 SSH 連線設定
date: 2021-01-18 22:40
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Linux/Unix
- SSH
--- 

最近部門為了落實更高強度的資安政策，因此針對伺服器的管理多做了限制。

<!--more-->
<p class="illustration">
<img src="https://i.imgur.com/9bjRUwP.png" alt="close">
close（圖片來源: <a href="https://kknews.cc/zh-hk/news/5n5rpb8.html">每日頭條</a>）
</p>

一般來說，若要連線至遠端的伺服器，可以使用 SSH 這個指令：

```bash
$ ssh 帳號@主機
```

建立連線時，輸入相對應的密碼，以完成認證。

<br> 

因為有些是開發用的伺服器，為了方便大家隨時使用，過往僅用密碼設置進行認證。不過根據這次的資安政策，需要針對伺服器最下列的設置：

1. 限制登入網段
2. 改用公開金鑰認證
3. 禁止 root 管理者以 SSH 登入

當然不只這些限制啦，還有 Linux 系統的密碼策略要設定，不過我這個打算另外開一篇，這篇就只專注在與 SSH 連線設定的部分。
   


## 限制登入網段
適當的限制網段是有其必要性的，因此這邊來限制能夠登入伺服器的網段！  
這邊我們是打算用設置白名單的方式，也就是除了明文允許的網段外，其他的一律禁止。

1. **允許連線的網段(/etc/hosts.allow)**  
    假設要放行的網段為 `192.168.1.0/24` 與 `192.168.99.10/32`，則在 `/etc/hosts.allow` 中新增

    ```bash
    sshd:192.168.1.0/24,192.168.99.10/32
    ```

2. **禁止連線的網段(/etc/hosts.deny)**  
    而除了上述放行的網段外，其他的網段則把他們擋下來。因此在 `/etc/hosts.deny` 中設定
    
    ```bash
    sshd:ALL
    ```
        
    正常情況下，allow 的優先權會<mark>高於</mark> deny。所以如果在 `hosts.allow` 這個檔案中有允許放行，並不會被 `hosts.deny` 的設定擋下來。 

    這樣一來，就完成基本的防護設定了！



## 改用公開金鑰認證 
關於改用公開金鑰認證這件事，之前在[《設定 Linux 使用 SSH Key-based 登入驗證方式》](/Configuring-SSH-Key-Based-Authentication-on-a-Linux)，已經寫過了這邊就不再重複了，總之可以分成 4 個步驟：

1. 本地端產生金鑰
2. 將公開金鑰上傳至伺服器
3. 啟用金鑰登入並停用密碼認證登入
4. 最後重起 sshd
 


## 禁止 root 管理者以 SSH 登入
會禁止 root 的原因有幾個：
1. `root` 這個帳號名稱是眾所皆知的，在帳號已知的情況下，只要破解密碼即可登入了，如此一來很容易遭受暴力法嘗試破解。
2. 無法追蹤與稽核相關修改與操作，因為在系統紀錄上都是 root。

如果真有需要進行 root 的操作，最好的方法還是先使用其他帳號登入後，再以 `sudo` 等方式切換成 `root` 進行管理，此作法能多一道防線，也能對哪些帳號可以具備 `sudo` 權限進行管理。

Linux 伺服器的預設是允許 root 管理者帳號遠端登入的，若要關閉該設定，一樣是修改 `/etc/ssh/sshd_config`，將其中的 **PermitRootLogin** 由 yes 改成 no：
```diff
- PermitRootLogin yes
+ PermitRootLogin no
```
<br>

在修改了 `sshd_config` 文件之後，一樣必須重起 sshd，以套用變更。
```bash
$ systemctl restart sshd
```



## 參考資料 
1. G. T. Wang (2018-02-03)。[Linux 的 SSH 安全加密連線指令使用教學、設定檔配置範例](https://blog.gtwang.org/linux/ssh-command-tutorial-and-script-examples/)。檢自 G. T. Wang (2021-01-08)。
2. 奔騰兔 (2006-02-02)。[SSH 連線設定](https://pentiumto.pixnet.net/blog/post/47370617-ssh-%E9%80%A3%E7%B7%9A%E8%A8%AD%E5%AE%9A) 。檢自 奔騰兔的部落格 (2021-01-08)。
3. 鳥哥 (2001/10/02)。[限制連接上您 Linux 主機的電腦網域](http://linux.vbird.org/linux_security/old/04_2remove_services.php) 。檢自 鳥哥的 Linux 私房菜 (2021-01-08)。
4. (2018-07-07)。 [Linux SSH 安全策略 限制 IP 登入方法](https://codertw.com/%E4%BC%BA%E6%9C%8D%E5%99%A8/382291/)。檢自 程式前沿 (2021-01-08)。
5. Sam Tang (2019-03-12)。[/etc/hosts.allow 及 /etc/hosts.deny 限制 IP 連線](https://www.opencli.com/linux/etc-hosts-allow-and-etc-hosts-deny)。檢自 LINUX 技術手札 (2021-01-08)。
6. (2017-05-15)。[免密碼登入 SSH Server](http://blog.ilc.edu.tw/blog/index.php?op=printView&articleId=688203&blogId=25793)。檢自 (2021-01-08)。
7. G. T. Wang (2017-12-27)。[Linux 禁止 root 管理者以 SSH 登入，強化系統安全性](https://blog.gtwang.org/linux/howto-disable-ssh-root-login-in-linux/)。檢自 G. T. Wang (2021-01-08)。
8. Weithenn (2017-10-24)。[CentOS 7.4 基礎設定 (6) - 禁止 Root 帳號本機及 SSH 遠端登入](https://www.weithenn.org/2017/10/centos-74-journey-part06.html)。檢自 不自量力 の Weithenn (2021-01-08)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-01-18</summary>
  <ul>
    <li>2021-01-18 發布</li>
    <li>2021-01-08 完稿</li>
    <li>2021-01-08 起稿</li>
  </ul>
</details>
