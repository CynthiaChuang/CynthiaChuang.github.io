---
title: 存取其他電腦的方法
date: 2020-02-27
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
- "資訊科技 › 開發與輔助工具"
tags:
- Linux/Unix
- SSH
- 協同合作工具
- 工具安裝與部署
--- 

最近為了因應疫情變化，HR 提了一項新政策，要考慮分批上班，簡單來說就是 <mark>work from home</mark>。  
  
我手邊的 OA 設備是可以應付短期需求，但時間一長還是不夠用...，所以把主意打到我的桌機看能不能連回來工作...。

<!--more-->
## 啟動目標電腦 SSH service
我的桌機一般 SSH 是關著的，所以要讓另外一台電腦連進來，要先把 service 打開

```shell
$ service ssh start
```



## 連線目標電腦
試著用另外的電腦連進來，連進來後再輸入目標電腦的開機密碼即可。

```shell
$ ssh <目標電腦 user name>@<目標電腦 ip> 
```

<p class="paragraph-spacing"></p> 

理論上是這樣就可行了...但我忘了我的電腦是在內網，且還是屬於使用 VPN 也尋訪不到的部分...只好先用 VPM 進到內網，再找台機器當跳板連回來...



## 遠端桌面
另外記錄一下，遠端桌面的軟體有 Chrome、anydesk 與 TeamViewer，不過這些軟體兩邊都要安裝，而且若想直接拜訪目標電腦，必須先設定好密碼，不然就得一個人坐在目標電腦前幫你按同意...有點蠢...

如果不想隨時都有人可以連到目標電腦的話，可以先把 service 關掉，要用的時候在透過 SSH 連到目標電腦開啟 service。

```shell
$ systemctl stop anydesk.service
$ systemctl start anydesk.service
```
 
<p class="paragraph-spacing"></p> 

不過如果機台在內網或是公司有阻擋 port，遠端桌面也不能用就是了...
 


## 參考資料 
1. Vivek Gite (2019-07-31) 。[How To Restart SSH Service under Linux / UNIX](https://www.cyberciti.biz/faq/howto-restart-ssh/)。檢自 nixCraft  (2020-02-27)。
2. 精心出精品 (2019-07-04)。[解惑：如何使得寝室的电脑和实验室的电脑远程相互访问（Linux和Windows）](https://www.cnblogs.com/zyrblog/p/11133091.html) 。檢自 精心出精品 - 博客园 (2020-02-27)。