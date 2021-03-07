---
title: 在 Ubuntu 18.04 中檢視隱藏檔案和資料夾
date: 2021-03-07 23:21
is_modified: false
categories:
- 環境設定與指令 
tags:
- Linux/Unix
--- 

前一陣子升級 Ubuntu 18.04 的後遺症，動了我的快捷鍵就算了，連 UI 都大改，害我做事綁手綁腳的 Orz
  
那天我想用圖形界面進入隱藏資料夾，但不得門而入。雖然我可以用終端機指令來操作，但有些操作我覺得用 UI 操作會比較方便，所以還是來找找怎麼辦好了。 

<!--more-->
<br>

## 圖形界面點擊

查到最簡單檢視隱藏內容的方法是，在所要檢視的資料夾中按下 **`Ctrl` + `H`**，就會看到隱藏檔案如雨後春筍冒出來。也可以點擊檔案管理器上方欄位中的選項按鈕，就會在開啟的選單中，看到**顯示隱藏檔**的選項。

<center> <img src="https://i.imgur.com/SfAlceo.png" alt="檢視隱藏內容"></center> 

<br><br>

## 圖形界面直接輸入路徑

原本我在找不到可以透過點擊圖形界面進入隱藏資料夾的方法時，有想在檔案管理器直接輸入路徑。但在之前的版本中，點擊兩下路徑就可以編輯路徑的方法似乎失效...只能另外找方法來編輯路徑。

後來找到點擊快捷鍵 `Ctrl` + `L`，就可以進入編輯狀態，只要在這邊輸入路徑就可以進入隱藏資料夾了：
<center> <img src="https://i.imgur.com/WmpJvfd.png" alt="編輯路徑"></center> 


<br><br>

## 用終端機指令

最後提提用終端機指令的部份：
```bash
$ cd 資料夾
$ ls -a
```

<br><br> 

## 參考資料 
1. 服務端 (2018-12-19)。[如何在Ubuntu檔案管理器中檢視隱藏檔案和資料夾](https://www.itread01.com/content/1545182833.html)。檢自 IT閱讀 (2021-02-04)。
2. 夏华东的博客 (2020-05-07)。[ubuntu 18.04.4 - 显示文件路径](https://blog.csdn.net/weixin_44493841/article/details/105969828)。檢自 夏华东的博客的博客｜CSDN博客 (2021-02-04)。
3. LINUX教程 (2018-10-02)。[linux顯示隱藏資料夾](https://www.itread01.com/p/194469.html)。檢自 IT閱讀 (2021-02-04)。


<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2021-03-07</summary>
  <ul class="timestamp">
    　<li>2021-03-07 發布</li>
    　<li>2021-02-04 完稿</li>
    　<li>2021-02-04 起稿</li>
  </ul>
</details>