---
title: "Docker Container 執行錯誤 windows:docker standard_init_linux.go:228: exec user process caused: no such file or directory"
date: 2022-11-02
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Windows/DOS
- Linux/Unix
- WSL
- 工具安裝與部署
- Docker
--- 

整理草稿夾時，注意到之前留下的一些錯誤紀錄。雖然問題不複雜，不過值得再次提醒我自己注意，所以我還是決定把它整理程網誌，~~絕對不是為了騙一篇~~ XDDD
<!--more-->


## 問題回報

1. **How to reprocedure the problem**  
	每次啟動 container 時此問題必現 [100%]
	```bash
	$ docker run demo:tag  	
	```
2. **Actual result**  
	出現錯誤訊息
	```bash
	standard_init_linux.go:228: exec user process caused: no such file or directory
	```
3. **Expected result**   
	能順利啟動此 container。


## 錯誤分析與排除

1. **排查啟動 script**  
	應該是啟動 script 的問題，所以首先排查 script，先直接別跑好了：
	```bash
	$> docker run demo:tag bash 	
	```
	
	恩，果然順利啟動了，是 script 的問題。
	
2. **檢查 script 內文**  
	雖然不可能，但還是檢查下 script 裡面的路徑有沒有下錯，畢竟錯誤訊息是寫說 `no such file or directory`。

	但文中路徑看起來一切正常、相對應的檔案有都存在，而且這些檔案內容也都不涉及路徑，所以應該都沒問題。
	
3. **檢查換行符號**  
	這是在網路上看到的，我也覺得應該是這原因。因為這次的 script 在 Windows 下撰寫，但我的 container 是 Ubuntu 的，如果 script 的換行符號是 CRLF 應該會跑不起來。
	
	果然檢查了下 VS Code，我的文件設定不知道什麼時候跑掉 XDDD 我明明有把它改成 LF 阿（搔臉
	
	把它調整回 LF 後，重新 build 一份新的 image 並執行，果然順利啟動了。

    
    <div class="alert info"> 
    <div class="head">VS Code 設定換行符號</div>
    <ol>
        <li>檔案 > 喜好設定 > 設定</li>
        <li>搜尋 eol</li>
        <li>將預設行尾字元設定成\n</li>
    </ol>
    </div>



## 參考資料 
1. Ryan Chou (2020-01-04)。[VS Code 設定換行符號，使MacOS、Windows 雙平台開發有一致的版本](https://blog.typeart.cc/VS%20Code%20設定換行符號，使MacOS、Windows%20雙平台開發有一致的版本/)。檢自 只是個打字的 (2022-11-03)。
2. CyrusZhou (2021-10-05)。[windows:docker standard_init_linux.go:228: exec user process caused: no such file or directory 错误解决](https://blog.csdn.net/lsqtzj/article/details/120619305)。檢自 CyrusZhou的博客｜CSDN博客 (2022-11-03)。
3. 可缺不可滥 (2020-12-10)。[vscode解决 windows换行crlf与lf冲突](https://blog.csdn.net/glorydx/article/details/110958739)。檢自 可缺不可滥的博客｜CSDN博客 (2022-11-03)。
 


## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-11-03</summary>
  <ul>
    <li>2022-11-03 發布</li>
    <li>2022-11-03 完稿</li>
    <li>2022-11-03 起稿</li>
  </ul>
</details>