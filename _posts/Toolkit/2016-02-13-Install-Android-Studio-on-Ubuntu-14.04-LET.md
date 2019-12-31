---
title: 【IDE】Android Studio的安裝篇：In ubuntu14.04 LET
date: 2016-02-13
categories:
- Toolkit
tags:
- IDE/Simulator
- IntelliJ系列
- Android
--- 

最近開始碰 Android，而且開發平台又換 Android Studio 而不是慣用的 eclipse，更慘的是作業系統還換到 Linux，簡直是雪上加霜。所以最近真的累到快翻掉　(；ﾟДﾟ；)

老樣子，紀錄一下這次的安裝過程<br>
↑ 其實一點也不是老樣子，已經很久沒有打部落格了，最近愛上手寫ＸＤＤＤ

<!--more-->
<br> 

## 安裝步驟
基本上安裝還頗簡單，而且 Ubuntu 有圖型化介面，真的不行還可以下載安裝XDDD <br>

### 1. 安裝 JDK
雖然 Ubuntu 有內建 Openjdk，但聽說 Android Studio 比較喜歡(?) Oracle 的 JDK，所以我還是另外裝了 JDK8
```shell
$ sudo add-apt-repository ppa:webupd8team/java 
$ sudo apt-get update 
$ sudo apt-get install oracle-java8-installer oracle-java8-set-default;
```

<br>安裝完成後，檢查是否有安裝成功，若一切順利，會看到 JDK 的版號會是8
```shell
$ java  -version
```
<br>

### 2. 安裝Android Studio
```shell
$ sudo apt-add-repository ppa:paolorotolo/android-studio
$ sudo apt-get update
$ sudo apt-get install android-studio
```
<br>

### 3.執行初始化安裝設定
```shell
$ /opt/android-studio/bin/studio.sh
```
不過，我嫌每次都要跑這行有點麻煩，所以後來做了把它加到 Launcher 裡面去捷徑...至於這篇何時可以生出來，((看看塞到爆的草稿夾...，恩...總有一天吧...

<br><br>
## 再次重灌時...
我這次重新安裝不幸遇到了點小問題，稍微紀錄一下，這次再安裝的時候跳出了 <span class="label warning">Unable to install Android Studio</span> 的錯誤訊息，有點晴天霹靂阿，不幸得重灌就算了，竟然還出狀況！！！！

查了一下與作業系統位元數有關，上次安裝的是 Ubuntu 32 位元的，這次則是 64 位元的，所以上次的 Android Studio 安裝步驟，有點水土不服XDDDD

需要安裝點額外的 3jpolib
```shell
$ sudo dpkg --add-architecture i386
$ sudo apt-get update
$ sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386
```

安裝完成後，重新執行初始化安裝設定，讓它重新啟動安裝程序，就OK了！！！

<br><br>
## 參考資料
- [How to install Android Studio on Ubuntu 15.04 / CentOS 7∥B N Poornima｜LinOxide](https://linoxide.com/tools/install-android-studio-ubuntu-15-04-centos-7/)
- [[UBUNTU 14.04.1]_在UBUNTU 14.04 安裝ANDROID STUDIO ∥thkaw｜NTL-NETWORK](http://www.ntex.tw/wordpress/2073.html)
- [Unable to install Android Studio in Ubuntu [duplicate]｜Stack Overflow](https://stackoverflow.com/questions/28847151/unable-to-install-android-studio-in-ubuntu)