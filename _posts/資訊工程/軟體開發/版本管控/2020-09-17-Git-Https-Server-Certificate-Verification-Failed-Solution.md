---
title: '【Git】解決 "server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none" '
date: 2020-12-30 22:03
is_modified: false
categories:
- "軟體開發 › 版本管控"
tags:
- Git
- Linux/Unix
--- 

最近在上 code 老是遇到憑證問題，雖然可以加環境變數來跳過，但三不五時就來一次實在有點討厭，來找找有沒有一勞永逸的辦法。

<!--more-->


## 問題
```bash
$ git push -u origin master
fatal: unable to access 'https://my.git.com:port/user_name/project.git/':
server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none
```

這主要是系統不信任我 git 伺服器的憑證，因此過不了系統的安全認證。查到的解決方法有二：
1. 略過或關閉 SSL 憑證檢查。
2. 告訴系統該伺服器可以信任。



## 略過或關閉 SSL 憑證檢查
如果你信任該伺服器，可以直接設定環境變數，讓 git 略過憑證的驗證：

```bash
export GIT_SSL_NO_VERIFY=1 
```

但這方法並不能永久略過，每隔一陣子，它就又會來報個錯刷刷存在感。若要讓 git 永久略過 SSL 憑證檢查，必須直接將設定寫入 git 的設定檔：

```bash
git config --global http.sslVerify false
```

這種解法比較像是閉上眼不管了，憑證問題依然沒有解決，而且像這樣永久略過會讓 SSL 少一層防護，因此不是很想這麼做。



## 匯入伺服器憑證
比較標準的作法應該是要匯入伺服器憑證，告訴系統該伺服器可以信任。在 [Office 指南](https://officeguide.cc/git-https-server-certificate-verification-failed-solution/)這個部落格，找到了份 script，可以直接做到這件事。

建立一個 script
```bash
$ touch import_ssl.sh
```

然後用 `vim` 對該檔案進行編輯，進入編輯模式後，輸入下面的內容，當然 `hostname` 跟 `port` 記得要換成自己的。
 
```bash
# Git 伺服器資訊
hostname=my.git.com
port=port_num

# 取得自己 OS 中放置 CA 的位置
trust_cert_file_location=`curl-config --ca`

# 匯入 Git 伺服器憑證
sudo bash -c "echo -n | openssl s_client -showcerts -connect $hostname:$port \
    2>/dev/null  | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p'  \
    >> $trust_cert_file_location"
``` 


最後執行 script
```bash
$ bash import_ssl.sh
```

理論上之後對同一台 git 伺服器的憑證應該不會再出現相同的錯誤訊息了，至少我到目前為止暫時沒出現過了。



## 參考資料 
1. EdmundChen (2017-12-11)。[git错误error: server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none](https://www.jianshu.com/p/7d599bdf370a) 。檢自 简书 (2020-09-17)。
2. [Git 透過 HTTPS 連線失敗：server certificate verification failed 解決方式](https://officeguide.cc/git-https-server-certificate-verification-failed-solution/) 。檢自 Office 指南 (2020-09-17)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-12-30</summary>
  <ul>
    <li>2020-12-30 發布</li>
    <li>2020-09-17 完稿</li>
    <li>2020-09-17 起稿</li>
  </ul>
</details>