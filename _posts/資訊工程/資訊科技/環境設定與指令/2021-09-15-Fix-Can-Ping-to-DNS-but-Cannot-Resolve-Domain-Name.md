---
title: 解決 Ubuntu 能 Ping 到 DNS 卻無法解析域名
date: 2021-09-16 15:50
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Linux/Unix 
--- 

這個還是 Ubuntu 系列，但倒不是因為升級導致的...呃，搞不好還是，好像在我升 Ubuntu 18.04 的時後就出問題的，但因為我有兩張網卡，所以一直沒有發現問題。
  
直到這次我必須通過特定張網卡來使用 VPN，結果發現我的這張網卡能 Ping 到 DNS 卻無法解析域名，因此我才會出不去 Orz

<!--more-->

<center> <img src="https://i.imgur.com/lzj5l3g.png" alt="連線不到的小恐龍"></center>
<center  class="imgtext">離線小恐龍 跑酷遊戲（圖片來源: <a href="chrome://dino/"  class="imgtext">chrome</a>）</center>

<br><br>

## 修改 /etc/resolv.conf  

一開始我們直接修改 `/etc/resolv.conf` ：
```bash
$ sudo vi /etc/resolv.conf
```

在檔案中直接加入 nameserver：

```diff
+ nameserver 8.8.8.8
+ nameserver 1.1.1.1
```

在重新試著 ping 了下果然通了：

```bash
$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=6.70 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=6.67 ms

$ ping google.com
PING google.com (142.251.43.14) 56(84) bytes of data.
64 bytes from tsa03s08-in-f14.1e100.net (142.251.43.14): icmp_seq=1 ttl=57 time=7.54 ms
64 bytes from tsa03s08-in-f14.1e100.net (142.251.43.14): icmp_seq=2 ttl=57 time=8.70 ms
```

但這個方法有個嚴重的問題，這個 DNS 參考檔是由電腦自動產生的，每次**重啟網路**時，DNS 參考檔都會被覆寫。所以這次修改只是暫時的，若不想每次重起都來一次，可能還是得另外找一個方法。

<br>

<div class="alert info"> 
<div class="head">Public DNS</div>
對了補充一下，一些常用 Public DNS <br>
<ul>
<li><b>8.8.8.8</b>:Google Public 慣用 DNS 伺服器</li>
<li><b>8.8.4.4</b>:Google Public 其他 DNS 伺服器</li>
<li><b>1.1.1.1</b>:Cloudflare 免費 DNS</li>
<li><b>1.1.1.2</b>:Cloudflare 免費 DNS（封鎖惡意軟體）</li>
<li><b>1.1.1.3</b>:Cloudflare 免費 DNS（封鎖惡意軟體與成人內容）</li>
</ul>
</div>


<br><br>

## 修改 /etc/resolvconf/resolv.conf.d/head

所以查了下網路，發現應該要更改：
```bash
$ sudo vi /etc/resolvconf/resolv.conf.d/head
```

在這個文件末尾增加上 nameserver：

```diff
nameserver 8.8.8.8
nameserver 1.1.1.1
```

<br>

完成後，重起網路去看看剛剛的 `resolv.conf`：
```bash
$ cat /etc/resolv.conf
```

會看到檔案中多出了 nameserver：

```diff
nameserver 8.8.8.8
nameserver 1.1.1.1
```

<br> 

OK，打完收工！


<br><br> 

## 參考資料 
1. 凍仁翔 (2008-02-24)。[Ubuntu 網路設定 - DNS](http://note.drx.tw/2008/02/ubuntu-dns.html?m=1)。檢自 凍仁的筆記 (2019-01-04)。
2. lengye7 (2019-03-29)。[ubuntu18.04直接更改/etc/resolv.conf修改nameserver重启被重置解决方法](https://blog.csdn.net/lengye7/article/details/88877867)。檢自 lengye7的博客｜CSDN博客 (2019-01-04)。
3. weixin_39832875 (2021-01-17)。[ubuntu 能解析域名但ping不通_Ubuntu 能ping通DNS 地址 无法解析域名](https://blog.csdn.net/weixin_39832875/article/details/113022802)。檢自 weixin_39832875的博客｜CSDN博客 (2019-01-04)。

<br><br>  

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-16</summary>
  <ul>
    <li>2021-09-16 發布</li>
    <li>2021-09-15 完稿</li>
    <li>2021-09-14 起稿</li>
  </ul>
</details>