---
title: 在 Linux 系统安裝 Openconnect
date: 2020-08-10 
is_modified: false
categories:
- Toolkit
tags:
- VPN
- Openconnect
- Linux/Unix
--- 

從草稿夾清出了草稿，拖了有點久，都去年的事情了 XD
  
起因是這樣的，去年去支援了 UI team 的工作，需要用到 postman 做些 API 資料的測試，順便接回前端。但這個案子的開發環境有點麻煩，我又是在外點工作，所以會用到好幾個不同的 VPN 連到不同環境。而這次的 API 又需要用另外一個 VPN 連到其他地方去...真是有夠麻煩的 :broken_heart: 
  
厄...我先承認，我全部丟給我搭檔去處理 VPN 的部份，我先去前端刻 UI 了，所以細節處紀錄的不是很清楚 XD

<!--more-->

<center> <img src="https://i.imgur.com/GcpptAh.jpg?1" alt="VPN"></center>
<center class="imgtext">VPN（圖片來源: <a href="https://www.pexels.com/zh-tw/photo/nordvpn-vpn-vpn-2063636/" class="imgtext">Gem Fortune - pexels</a>）</center>
<br><br>

## VPN 踩坑流程

在安裝 Openconnect 之前，搭檔還先嘗試了好幾套 VPN 軟體，不過都悲劇了。

### GlobalProtect

第一個是台北同仁給的則是  **GlobalProtect**，還很好心的給了安裝文件，可以手把手教你連上需要的機台，但依舊悲劇的是 - <span class="highlighting">沒有 Linux</span>。...奇怪了，台北同仁都不用 Linux 嗎？

我搭檔只好開始啃 [GlobalProtect Linux 的安裝文檔](https://docs.paloaltonetworks.com/globalprotect/4-1/globalprotect-app-user-guide/globalprotect-app-for-linux.html)，只是他裝好後，試圖進行連接，
 
```bash 
$ globalprotect 
                                                                       
>> connect --portal IP
Retrieving configuration...  
```

但一直處在 Retrieving configuration...，即使整個完整卸除安裝後，再重新安裝，還是一樣問題，他只好改試用 SoftEther VPN 試試。
<br>

###  SoftEther

他們說要來試試看，在 [Linux 上的安裝](https://linuxconfig.org/setting-up-softether-vpn-server-on-ubuntu-16-04-xenial-xerus-linux)，沒有看到他們說成功的消息，反而開始討論起其他的，應該是失敗了？
<br>

### network-manager-l2tp

後來看到他們在嘗試使用 l2tp，參考了這兩個連結：
- [Setup L2TP over IPsec VPN client on Ubuntu 18.04 using GNOME](https://20notes.net/linux/setup-l2tp-over-ipsec-client-on-ubuntu-18-04-using-gnome/)
- [Configure IPsec/L2TP VPN Clients：Linux](https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients.md#linux)

搭檔回報 failed，有一個 Enter `Your VPN IPsec PSK` for the **Pre-shared key**，看起來是必要欄位，但他沒法填。

<br><br>

## Openconnect

後來搭檔參考了，[這篇](https://askubuntu.com/questions/686186/how-to-connect-my-ubuntu-to-my-workplace-globalprotect-vpn-using-win-7-vm)使用 IPsec/ESP modes 來連 GlobalProtect，看來是成功了。

總結一下安裝步驟：

1. **安裝 CA**
    ```bash
    $ sudo cp ca.crt /usr/local/share/ca-certificates
    $ sudo update-ca-certificates
    ```

2. **安裝 openconnect**
    ```bash
    $ sudo apt-get install \
        build-essential gettext autoconf automake libproxy-dev \
        libxml2-dev libtool vpnc-scripts pkg-config \
        libgnutls-dev
    $ git clone https://github.com/dlenski/openconnect.git
    $ cd openconnect
    $ ./autogen.sh
    $ ./configure
    $ make
    $ sudo make install && sudo ldconfig
    ```

    or

    ```bash
    $ sudo apt-get update
    $ sudo apt-get install openconnect
    ```

3. **連線到目標 VPN**
    ```bash
    $ sudo ./openconnect --protocol=gp IP
    ```

<br><br> 

## 後記
說實話，我從頭到尾搞不清楚他們搞什麼 XDDD

不過按照他們總結給我的流程進行安裝，可以順利從 postman 要到資料了。剛好前端刻的差不多了，可以直接接下去繼續串資料流程了 XD

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-10</summary>
  <ul class="timestamp">
    　<li>2020-08-10 發布</li>
    　<li>2020-06-04 完稿</li>
    　<li>2019-09-24 起稿</li>
  </ul>
</details>
