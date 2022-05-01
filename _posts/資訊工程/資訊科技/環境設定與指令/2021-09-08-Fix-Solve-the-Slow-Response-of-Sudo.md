---
title: 解決 sudo 速度反應緩慢
date: 2021-09-16 15:43
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Linux/Unix 
--- 

這都不知道是這個禮拜寫的第幾篇跟 Ubuntu 相關的問題...，一切都是[OS系統的升級](/Upgrade-Ubuntu)錯啦！雖說問題都不大，但遇到了還是很阿雜...
  
因為上次升完級後，發現當我提交 sudo 命令，大概得過 10 秒才會跳出輸入密碼的提示...。

<!--more-->
<br>

# 原因

想了想原因，可能是因為我在升級時順手將 hostname 由 `ubuntu18` 改成了 `ubuntu20`。

而至於更確切的原因？我在網路上找到了 [Kiritow](https://blog.csdn.net/Kiritow/article/details/80687036) 的解說：
> Ubuntu Server 被設計成一種類似於分佈式的操作系統網路結構，允許 `/etc/sudoers` 中的成員不在本機上。因此 sudo 時會先從網路上尋找可能的 sudoer 然後才是本地。 而這 10 秒左右的時間就是整個 DNS 流程的最長時間。


<br><br>

# 解決方案

其實還滿簡單的，修改 `/etc/hosts` 就好了：

```bash
$ sudo nano /etc/hosts
```


然後把新的 hostname，也就是 `ubuntu20`， 加到檔案中：

```diff
- 127.0.0.1 localhost
+ 127.0.0.1 localhost ubuntu20
```

設完後，sudo 速度恢復正常。

<br>

如果沒有恢復正常，可以試試 [Kiritow](https://blog.csdn.net/Kiritow/article/details/80687036) 的方法，他除了加上 hostname 外，他還額外加了 localdomain 變成了：
```diff
- 127.0.0.1 localhost
+ 127.0.0.1 localhost ubuntu20 ubuntu20.localdomain
```


<br>

如果你不知道你的 hostname，可以用指令先查查：
```bash
$ hostname
ubuntu20
```

<br><br>

# 疑問

雖然解決了，但我這邊有一個疑問，之前也出現過[疑似換了 hostname，而產生了錯誤](/@CynthiaChuang/Fix-Sudo-Unable-to-Resolve-Hostname/)，但我那時看到的錯誤訊息是：
```bash
sudo: unable to resolve host ubuntu16-x64
```

且解決方式是一模一樣，都是修改 `/etc/hosts` 。所以到底為啥相同的原因，會得到不同的錯誤訊息？
 

<br><br> 

## 參考資料  
1. Kiritow (2018-06-14)。[Ubuntu下sudo速度很慢原因及解决办法](https://blog.csdn.net/Kiritow/article/details/80687036)。Kiritow的学园｜CSDN博客 (2021-09-08)。
2. hongXkeX (2017-08-11)。[ubuntu - sudo 命令执行速度很慢的解决办法](https://www.jianshu.com/p/3344ee1b9ba0)。檢自 简书 (2021-09-08)。

<br><br>  

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-16</summary>
  <ul>
    <li>2021-09-16 發布</li>
    <li>2021-09-08 完稿</li>
    <li>2021-09-08 起稿</li>
  </ul>
</details>
