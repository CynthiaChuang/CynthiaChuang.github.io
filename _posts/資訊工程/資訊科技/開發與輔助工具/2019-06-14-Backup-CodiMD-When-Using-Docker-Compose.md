---
title: 【CodiMD】docker-compose 備份
date: 2019-06-14
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
- "資訊科技 › 系統備援"
tags:
- HackMD/CodiMD/HedgeDoc
- Docker
- Docker-Compose
- Linux/Unix
- 協同合作工具
- 工具安裝與部署
--- 

這是 [安裝完 CodiMD](/How-to-Setup-CodiMD/) 後在執行定期備份時踩到的坑...在使用 [crontab + script 定期備份](/Using-Crontab-and-Shell-Script-to-Regular-Backup-and-Keep-It-Last-30-Days/)時，我百思不得其解，為啥我跑出來的 <mark>backup.sql</mark> 會是空的？ 更讓我不解的是，我手動執行 script 的結果卻是正常的！？？？ 

<!--more-->
<br class="big">

CodiMD 官方給定指令是長這樣，在手動執行它運作的非常好
```shell
$ docker-compose exec database pg_dump hackmd -U hackmd > backup.sql
```

<br class="big"> 

但偏偏 crontab + docker-compose 這兩個加在一起就是跑不起來 Orz。

一開始懷疑過是不是權限問題，使用 crontab 執行時沒有 docker-compose 的執行權限或是寫出的權限。基本上後者可以排除，因為我直接輸出在 home 目錄下，不至於沒有寫出的權限。

所以就先考慮是不是 docker-compose 的執行權限出問題，直接加 sudo ，但還是輸出的 backup.sql 還是空：
```shell
$ sudo docker-compose exec database pg_dump hackmd -U hackmd > backup.sql
```

<br class="big"> 

查了一下相關[討論](https://github.com/docker/compose/issues/2293)，大家是建議最好使用<mark>絕對路徑</mark>，因此將 docker-compose 換成 **/usr/local/bin/docker-compose**，但還沒用 :cry: 
```shell
/usr/local/bin/docker-compose exec database pg_dump hackmd -U hackmd > backup.sql
``` 

<br class="big"> 

執行時多加了兩行，確認一下執行身分與路徑有沒有跑掉，但看起來也一切正常。
```shell
whoami > log.txt
whereis >> log.txt
```

<br class="big"> 

後來D大跟我說他試出來了，在指令上加 <mark class='danger'>-T</mark> 就正常了：
```shell
/usr/local/bin/docker-compose exec -T database pg_dump hackmd -U hackmd > backup.sql
``` 

查了一下 -T 是<mark>禁用 TTY</mark> ，<mark>docker-compose exec</mark> 是預設有啟用 TTY，而 crontab 是沒有預設 TTY（終端設備），所以將 docker-compose exec 加上 -T 就可以。

不過我還是不太明白，即便預設有啟用 TTY，但 Script 在執行過程並有任何輸出到終端機上，我以為這樣應該不會卡死才對 ＠＠？



## 參考資料
1. [Crontab can't execute docker-compose commands｜Issue #2293 · docker/compose](https://github.com/docker/compose/issues/2293)
2. [docker-compose with crontab｜Stack Overflow](https://stackoverflow.com/questions/30905697/docker-compose-with-crontab)
3. [【操作系统】Crontab运行相关问题以及解决方法｜【 无线技术运营 】 (本专栏是http://wireless.qzone.qq.com的镜像) - CSDN博客](https://blog.csdn.net/wireless_tech/article/details/6417996)


