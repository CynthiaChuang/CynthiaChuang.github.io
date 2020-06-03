---
title: 使用 Dockerfile 設定 Container 時區
date: 2020-02-18
categories:
- Environment
tags:
- Docker
- Dockerfile
- Linux/Unix
--- 

那天，在看 log 的時候發現我印出來的時間不太對勁，重新看了 Container 的時間，發現我的時區沒設好。

<!--more-->
<br><br> 

## 時區檢查
如果在終端要檢查時間與時區是否正確可以使用：`date` 或是 `timedatectl` 來檢查

```bash
$ date
Fri Feb 21 09:03:40 CST 2020
```

<br> 或是

```bash
$ timedatectl
      Local time: Fri 2020-02-21 09:05:28 CST
  Universal time: Fri 2020-02-21 01:05:28 UTC
        RTC time: Fri 2020-02-21 01:05:28
       Time zone: Asia/Taipei (CST, +0800)
 Network time on: yes
NTP synchronized: yes
 RTC in local TZ: no
```
 
<br> 正常情況下，若設定台北時間（ +8:00 ），時區代碼應該會顯 <span class='highlighting'>CST</span>（Chungyuan Standard Time，中原標準時間），就像上面那樣，但我顯示的卻是 <span class='highlighting'>UTC</span>：
```bash
Fri Feb 21 01:05:28 UTC 2020
``` 

<br> 難怪 log 印出來的時間怎看怎麼彆扭 orz
 
<br><br>

## 設定時區
既然知道時區不對，那就把校正回來，在 Ubuntu 中修改時區的方法有兩種。
<br>

### 1. GUI
第一種方式可以使用 GUI，點擊右上的`關機的圖示` → `系統設定值` → 觀看 `系統` 的區塊  → `時間與日期`，接下來選擇你所在的地區即可。
<br>

### 2. Command Line 
相對於 GUI 的修改方式，就是用 Command Line 啦。

```bash
$ sudo dpkg-reconfigure tzdata
```

說是 Command Line ，不過輸入指令後就會出現互動界面，讓你選擇時區：

<center> <img src="https://i.imgur.com/T53atWB.png" alt="sudo dpkg-reconfigure tzdata"></center>

<br><br>

## 使用 Dockerfile 設定時區
但每次啟動 Container 都要手動改一次時區，這也不是辦法。只好來研究怎麼在 Dockerfile 裡設定時區了。

根據[教學的文件](https://www.itread01.com/p/154107.html)，只要
```dockerfile
RUN apt-get update \
    && apt-get install -y --no-install-recommends tzdata
    
RUN TZ=Asia/Taipei \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata 
```

<br> 但實際在 build 的時候會發現， 跑到 `apt-get install tzdata` 這行時，終端機就會跑出互動訊息要求設定時區。


後來[查到](https://askubuntu.com/a/1013396)，如果不想讓 dpkg 出現互動訊息，要設定環境變數

```
DEBIAN_FRONTEND=noninteractive
```
 
<br> 

只是我覺得將這變數用 `ENV` 來宣告，應該會留個坑，之後不小心又會坑到自己（對於如何坑自己非常的有經驗！

還好在閱讀[文件](https://docs.docker.com/engine/reference/builder/#env)時，注意到在關於 ENV 的最後寫了句：
> **Note**: Environment persistence can cause unexpected side effects. For example, setting ENV DEBIAN_FRONTEND noninteractive may confuse apt-get users on a Debian-based image. To set a value for a single command, use RUN \<key\>=\<value\> \<command\>.

<br>

最後依照提示整個設定改成了：
```dockerfile
RUN apt-get update \
    &&  DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata
    
RUN TZ=Asia/Taipei \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata 
```

<br>

改完後進行 docker image 重 build，注意看的話應該會在過程中閃過：

```bash
Current default time zone: 'Asia/Taipei'
Local time is now:      Fri Feb 21 10:31:05 CST 2020.
Universal Time is now:  Fri Feb 21 02:31:05 UTC 2020.
```

這樣的話，時區就修改完成了。

<br><br> 

## 參考資料 
1. 多人協同創作 (2016-07-13)。[UbuntuTime](https://www.itread01.com/p/154107.html) 。檢自 Community Help  (2020-02-18)。
2. LINUX教程 (2018-10-02)。[Docker容器時區設定與中文字元支援](https://www.itread01.com/p/154107.html) 。檢自 IT閱讀 (2020-02-18)。
3. pyfreyr (2019-05-28)。[Avoiding user interaction with tzdata when installing certbot in a docker container](https://askubuntu.com/a/1129284) 。檢自 Ask Ubuntu (2020-02-18)。
4. [Dockerfile reference - ENV](https://docs.docker.com/engine/reference/builder/#env) 。檢自  Docker Documentation (2020-02-18)。