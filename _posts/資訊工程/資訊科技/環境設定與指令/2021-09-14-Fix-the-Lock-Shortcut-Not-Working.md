---
title: 解決 Ubuntu 20.04 鎖定快捷鍵無反應
date: 2021-09-16 15:33
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Linux/Unix 
--- 

還是 Ubuntu 升級後搞出來的問題 ←_←  
  
每天下班前我會順手按下 `Super` + `L` 把螢幕給鎖定。不過那天下班前忽然發現快捷鍵不管用了，雖然可以透過右上角的按鍵來所鎖定，但是實在有點麻煩，所以隔天上班時還是決定花點間調整。

<!--more-->


## 確認鎖定功能是否設定  
在網路上找了下資料，發現可能的原因五花八門，所以我決定先檢查下鎖定功能的設定是否如預期般設定：

1. **查看功能設定**  
    ```bash
    $ gsettings get org.gnome.desktop.lockdown disable-lock-screen
    ```
    
    理論上會回覆個 boolean，預期回覆是 `false`。
    
2. **設定鎖定功能**  
    如果得到的回覆是 `true`，則先設定鎖定功能：
    ```bash
    $ gsettings set org.gnome.desktop.lockdown disable-lock-screen false
    ```


## 更改 X 顯示管理器    
不過，我重新設定鎖定功能後仍然不能運作，只能進一步修改設定，考慮了下決定選擇用<mark>更改 X 顯示管理器</mark>的方法。

1. **檢查套用的管理器**  
    雖然文件中是說 Ubuntu 20.04 是使用 GDM3 作為默認顯示管理器，但目前的管理器應該是 **LightDM**，或許是因為我一路從 Ubuntu 14.04 升上來的關係！？  
    
    一個簡單的指令可以確定目前所使用的管理器是否為 LightDM：    
    ```bash
    $ dm-tool lock
    ```
    如果你的螢幕立刻被鎖定，代表使用的 LightDM；若沒有反應的話，則代表否。
    
2. **更改管理器**   
    接下來把管理器改回預設的 GDM3：
    ```bash
    $ sudo dpkg-reconfigure gdm3
    ```
    
    <br>
    
    輸入完密碼後，會出互動介面，選擇上面的 GDM3：
    <p class="illustration">
    <img src="https://i.imgur.com/qv27MZB.png" alt="設定 GDM3">
    </p>
    
    完成後，重新開機就可以套用新的 X 顯示管理器了：
    <p class="illustration">
    <img src="https://i.imgur.com/pOVPMeq.png" alt="GDM3">
    </p>
    
    對了，原本的 LightDM 登入界面長這樣：
    <p class="illustration">
    <img src="https://i.imgur.com/NrTbmF0.png" alt="LightDM">
    </p>
   
<br><br>   

既然 LightDM 有指令可以鎖定螢幕，GDM3 當然也有：    
```bash
$ gnome-screensaver-command --lock
```
不過我應該沒有機會用到這條指令了，因為我的快捷鍵可以用了，哈哈哈 :metal:



## 參考資料 
1. 旧人赋荒年 (2018-08-15)。[Ubuntu锁屏突然失效](https://blog.csdn.net/yangziluomu/article/details/81702000)。檢自 旧人赋荒年｜CSDN博客 (2021-09-09)。
2. Cris_Hu (2021-08-12)。[Ubuntu20.04 锁屏快捷键无反应的解决方法](https://blog.csdn.net/Cris_Hu/article/details/119639189)。檢自 Cris_Hu的博客｜CSDN博客 (2021-09-09)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-16</summary>
  <ul>
    <li>2021-09-16 發布</li>
    <li>2021-09-14 完稿</li>
    <li>2021-09-09 起稿</li>
  </ul>
</details>