---
title: Ubuntu 上的螢幕錄影工具 - Kazam
date: 2020-11-12 21:01
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Linux/Unix
- 影音播放工具
- 工具安裝與部署
--- 

那天被 Designer 要求錄製相關操作以做為製作 Demo 影片的素材。但之前錄影的工具都只適用在 Windows 上，我還沒試過在 Ubuntu 上錄影呢。 

<!--more-->


## 安裝
我先試著在網路上找了適用於 Ubuntu 上錄影工具，最後找到了這篇 [15 個最佳的螢幕錄製工具推薦](https://kknews.cc/zh-tw/code/gm4bj5e.html)，文中列出了 15 種工具，有需求的人可以逐一試試。

從這列表中，我相中了 Kazam，因為它可以透過 apt-get 或 apt 來安裝：

```bash
$ sudo apt-get install kazam
```

<br class="big"> 

驚奇的發現，我電腦竟然裝過！所以它是內建的嗎？還是我夢遊安裝的？不過既然裝過了，我就試著升級一下：
```bash
$ sudo apt-get update
$ sudo apt-get upgrade kazam
``` 

升級完版本顯示為 `1.4.5` ，這[看起來](https://launchpad.net/kazam)是他們的最新版本沒錯，不過開發團隊看起來很久沒有更新了，最後 release 時間停在 2014.08.18。

<p class="illustration">
    <img src="https://i.imgur.com/StG6H1x.png" alt="Kazam timeline">
    timeline（圖片來源: <a href="https://launchpad.net/kazam/+series">Kazam Screencaster</a>）
</p>

但我這在 [Fygul Hether](http://fygul.blogspot.com/2019/04/kazam.html) 的介紹文章中發現，如果用[非官方的 PPA](https://launchpad.net/~sylvain-pineau/+archive/ubuntu/kazam)，版本似乎會到 1.5.3，並多出了些新功能，有需要的可以試試看，這邊只做紀錄：

```bash
$ sudo add-apt-repository ppa:sylvain-pineau/kazam
$ sudo apt update
$ sudo apt install kazam
```



## 使用操作
打開軟體會發現它有提供錄影與截圖的功能，不過之前有找到個還滿順手的軟體 - [Shutter](/Shutter-A-Screenshot-Editor-on-Ubuntu)，所以就暫時不先玩截圖的功能了，僅專注在錄影功能上。

<p class="illustration">
    <img src="https://i.imgur.com/woxvwPh.png" alt="Kazam">
</p>

它的使用方式相當的簡單，基本上選定錄影範圍後，按下 `Capture` 就可以直接錄影了。不過建議在第一次使用前，先做好基本設定再開始。

依序點選 "檔案" → "偏好設定"，開啟偏好設定視窗。主要要調整或確認的地方有二：General 以及 Screencast，分別調整<mark>聲音</mark>與<mark>影像幀數和編碼格式</mark>：

<p class="illustration">
    <img src="https://i.imgur.com/ycKcj8u.png" alt="Kazam">
</p>

像音源的部分，一般來說應該會是選擇內部音效，如果另外有裝音效卡的需要換成對應的卡片。像我內建音效卡的輸出是壞的，所以聲音是走另一個 USB 簡易型音效卡出來，所以選了 USB 這個。

另外幀數的部分，幀數直接影響的就是流暢度，我開啟時預設幀數是 15，但查到的網誌是建議幀數至少要 <mark>30</mark>。但幀數會與檔案大小成正比。



## 快捷鍵
基本上啟動後，icon 會出現在右上角，錄影途中可以點擊 icon 展開選單，來暫停或是中止錄影。

<p class="illustration">
    <img src="https://i.imgur.com/XBR0Ues.png" alt="Kazam">
</p>

因為我是在錄製操作流程，所以有把滑鼠游標給錄進來，而且為了縮短片長，每當需要長等待的時候我都會按下暫停。這導致游標在右上與畫面中間來滑動，實在讓人眼花瞭亂...。

所以只好上網拜大神，看有沒有<mark>快捷鍵</mark>可用，幸運的是還真的讓我找到了：

- **開始錄影 (Recording)**：Super+Ctrl+R  
- **暫停、恢復錄影 (Pause)**：Super+Ctrl+P  
- **完成錄影 (Finish)**：Super+Ctrl+F  
- **結束 Kazam (Quit)**：Super+Ctrl+Q



## 無法在 Windows 上播放
另外一個遇到的麻煩是，我錄出來的 mp4 影片在 Windows 上不能播放，也不能說是完全不能播放，如果是用 Chrome 可以播，但若改用其他的播放器就 GG 了。

查了一下好像是解碼器的問題，[xueliang2007](https://blog.csdn.net/qq_31806429/article/details/78832902) 說可以透過 [HandBrake](https://handbrake.fr/downloads.php) 這套軟體來轉檔。不過這是套 Windows 的軟體，我不可能直接把影片丟給 Designer 叫他們自己安裝軟體轉檔，他們應該會直接退件給我。（雖然這個想法讓我很心動！）

<br class="big">

後來找到 StackExchange 發現相同的[問題](https://video.stackexchange.com/questions/20162/convert-kazam-video-file-to-a-file-playable-in-windows-media-player)，根據下面的回答，可以使用 <mark>fmpeg</mark> 來轉檔，指令如下：

```bash
$ sudo apt install ffmpeg
$ ffmpeg -i in.mp4 -pix_fmt yuv420p -c:a copy -movflags +faststart out.mp4
```

這次再請用 Windows 的同仁幫我試試看不能播放，終於得到可以順利播放的回覆了。

<br class="big">

不過如果你的影片帶有音源，上面的指令可能會跳出錯誤訊息：
```bash
Error while opening encoder for output stream #0.0 - maybe incorrect parameters such as bit_rate, rate, width or height
```

因此，重新找了下[相關問題](https://stackoverflow.com/questions/13877031/error-while-opening-encoder-for-output-stream-0-0-maybe-incorrect-parameters)，有找到一條可以 work 可指令，不過...沒有研究那些參數代表的意思，不太確定是否會有啥差別。

```bash
$ ffmpeg -i input.mp4 -c:a copy -movflags +faststart output.mp4
or
$ ffmpeg -i input.mp4 -c:v copy -crf 19 output.mkv
```

<br class="big">

是說，ffmpeg 好像也能用來錄影，但是我找不到只錄製特定區域或單一視窗的功能，只好放棄用這套了。



## 參考資料 
1. Linux學習教程 (2019-08-04)。[ubuntu 15個最佳的螢幕錄製工具推薦](https://kknews.cc/zh-tw/code/gm4bj5e.html) 。檢自 每日頭條 (2020-09-23)。
2.  Fygul Hether (2019-04-27)。[Linux螢幕錄影軟體Kazam](http://fygul.blogspot.com/2019/04/kazam.html)。檢自 荒天翔鷗的天地 (2020-09-23)。
3.  Magic Len (2014-09-24)。[Linux上的螢幕錄影工具－Kazam](https://magiclen.org/kazam/) 。檢自 MagicLen (2020-09-23)。
4. xueliang2007 (2017-12-18)。[linux下用Kazam录屏视频在windows不能播放解决](https://blog.csdn.net/qq_31806429/article/details/78832902) 。檢自 xueliang2007的博客｜CSDN博客 (2020-09-23)。
5. Gyan (2016-12-17)。[screen casting - convert KAZAM video file to a file, playable in windows media player](https://video.stackexchange.com/a/20164) 。檢自 Stack Exchange (2020-09-23)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-11-12</summary>
  <ul>
    <li>2020-11-12 發布</li>
    <li>2020-09-23 完稿</li>
    <li>2020-09-23 起稿</li>
  </ul>
</details>