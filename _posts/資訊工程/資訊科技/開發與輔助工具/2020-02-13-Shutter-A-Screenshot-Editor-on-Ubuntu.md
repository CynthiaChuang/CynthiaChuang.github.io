---
title: Ubuntu 上的螢幕截圖編輯軟體 - Shutter
date: 2021-09-14
is_modified: true
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Linux/Unix
- 編修與繪圖工具
- 工具安裝與部署
---

找到一款 Linux 的截圖軟體，重點是它有圖片編輯器阿！GIMP 好麻煩，我的要求不多能畫條線、畫個箭頭再加畫個螢光筆就好了。

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/xqJC4QV.png" alt="Shutter">
</p>



## 安裝
1. **Ubuntu 16.04**    
  安裝超級簡單在軟體中心就找的到了！

  應用程式 → Ubuntu 軟體中心 → 搜尋 Shutter → 點擊安裝了。

  <p class="illustration">
  <img src="https://i.imgur.com/vHvLaNL.jpg" alt="在 Ubuntu 軟體中心中安裝 Shutter">
  </p>

  最後長這樣：

  <p class="illustration">
  <img src="https://i.imgur.com/GW0aBea.png" alt="Shutter">
  </p>

<br>

2. **Ubuntu 20.04**      
  但，在 Ubuntu 20.04 時，我從軟體中心卻找不到這個軟體了，無論是搜尋 **Shutter** 或是**快門**，只好改由終端機安裝了。
  
  因為不確定系統預設的 repository 有沒有收錄 Shutter，所以我先加入了官方的 PPA：
  ```bash
  $ sudo add-apt-repository ppa:shutter/ppa
  ```

  <br>
  
  然後用 apt-get 來安裝：
  ```bash
  $ sudo apt-get update
  $ sudo apt-get install shutter
  ```    



## 問題排除：無法編輯圖片
是說，我每次更新系統 shutter 都會出問題，之前從 Ubuntu 16.04 升級到 Ubuntu 18.04 時，我的 Shutter 就不見過一次！不過這還不是最大的問題，畢竟可以重裝。

不過當從軟體中心重裝後（對！那時軟體中心還找的到），發現我沒辦法編輯圖片，更確切的說應該是**編輯按鈕不能按**，如果不能編輯圖片我還要你幹麻（翻桌） 

<p class="illustration">
    <img src="https://i.imgur.com/7AYQ6Va.png" alt="翻桌啦">
    翻桌啦（圖片來源: <a href="https://zi.media/@seawater/post/jQnhZP">Zi 字媒體</a>）
</p>

只好森七七地去拜大神了，據大神們說這是因會缺少必要套件，只要把這些套件裝回就行了，不過這些套件已經被從 Ubuntu 18.04 的官方鏡像<mark>移除</mark>了，因此必須先手動下載：

```bash
$ wget https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas-common_1.0.0-1_all.deb
$ wget https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas3_1.0.0-1_amd64.deb
$ wget https://launchpad.net/ubuntu/+archive/primary/+files/libgoo-canvas-perl_0.06-2ubuntu3_amd64.deb
```
<br>

之後才能進行安裝，安裝過程或許會需要 `apt-get install -f` 指令來處理相依套件衝突問題：

```bash
$ sudo dpkg -i libgoocanvas-common_1.0.0-1_all.deb
$ sudo dpkg -i libgoocanvas3_1.0.0-1_amd64.deb
$ sudo dpkg -i libgoo-canvas-perl_0.06-2ubuntu3_amd64.deb
$ sudo apt -f install
```
<br>

安裝結束後就可以把下載的 deb 刪掉了

```bash
$ sudo rm libgoocanvas-common_1.0.0-1_all.deb
$ sudo rm libgoocanvas3_1.0.0-1_amd64.deb
$ sudo rm libgoo-canvas-perl_0.06-2ubuntu3_amd64.deb
```

最後啟動 Shutter 可以發現我的編輯又可以用了（開心

<br>

補充一下，是說我發現在這版的截圖在使用時變得超級糊。找了下原因，發現它的影像品質預設只有 9，滿級是 100，難怪超級糊。

影像品質可以在 `編輯` → `偏好設定` → `主要` → `影像格式` 來調整：

<p class="illustration">
    <img src="https://i.imgur.com/H6CZEJU.png?1" alt="編輯">
</p>



## 參考資料
1. yunol (2010-08-22)。[好用的 Ubuntu 螢幕截圖軟體 Shutter](http://elesson.tc.edu.tw/~yunol/shutter/)。檢自 台中式教育局網路中心數位教學平台 (2020-02-13)。
2. Chi Thuc Nguyen (2019-02-19)。[How to enable "Edit" option in Shutter on Ubuntu 18.04](https://thucnc.medium.com/how-to-enable-edit-option-in-shutter-on-ubuntu-18-04-e8b2c8dcc58)。檢自 Chi Thuc Nguyen ｜ Medium (2021-01-26)。
3. Peter279k (2019-02-01)。[在 Ubuntu 18.04 上安裝 Shutte r 擷取圖片工具](https://peterli.website/%E5%9C%A8ubuntu-18-04%E4%B8%8A%E5%AE%89%E8%A3%9Dshutter%E6%93%B7%E5%8F%96%E5%9C%96%E7%89%87%E5%B7%A5%E5%85%B7/)。檢自 Peter 工程日誌 (2021-01-26)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-14</summary>
  <ul>
    <li>2021-09-14 更新：指令安裝方式、各節標題更改</li>
    <li>2021-01-27 更新：新增 在 Ubuntu 18.04 +上安裝</li>
    <li>2020-02-13 發布</li>
    <li>2020-02-13 完稿</li>
  </ul>
</details>
