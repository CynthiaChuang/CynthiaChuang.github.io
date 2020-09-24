---
title: 【Shell Script】使用 crontab + script 定期備份並保留30天
date: 2019-06-14
is_modified: false
categories:
- Computer-Language/Framework
tags:
- HackMD/CodiMD
- Shell Script
- crontab
- Linux/Unix
--- 

這是 [CodiMD 安裝](/How-to-Setup-CodiMD/)的後續...，雖然跟 CodiMD 沒啥關係 XD 
這次的主要目的是為 CodiMD 做一個定期備份，另外為了儲存空間上的考量，備份檔案只保留 30 天，一旦超過就刪除資料。

<!--more-->
<br><br> 

不過太久沒重頭寫 Shell Script 了，連起手式都有點忘了 Orz，快速找[鳥哥](http://linux.vbird.org/linux_basic/0340bashshell-scripts.php#script_why)複習一下~~塵封在記憶中的起手式~~
```shell
#!/bin/bash
```

<br>

## 步驟分解
咳，回頭正經的，這篇文章目的可以拆解成三個動作：
1.  備份
2.  刪除 30 天前的檔案
3.  定期執行

另外執行完備份，為了避免這台機器掛掉連同備份一起不見，最後在做一個異地的備份

<br>

### 備份
1. 先準備一個含 timestamp 的資料夾吧，方便做檔案的管理
    ```shell
    foldername=codimd_backup_$(date +%Y%m%d%H%M)
    mkdir -p -- ~/${foldername}
    ```
    <br>


2. 接下來複製整個資料夾，不過因為權限問題，所以我 **cp** 指令前加上了 **sudo**，之後在用 **chown** 更改資料夾權限。
    ```shell
    sudo cp -r -p ~/codimd-container/ ~/${foldername}/
    sudo chown -R $(whoami) ~/${foldername}/codimd-container/
    ```
    建議在使用 cp 指令時加上 **-p**，保留時間、擁有者...等資訊
    
    <br>


3. 因為這是針對 CodiMD 的備份，所以除了一般資料夾的複製外，我也執行了 CodiMD 的 Backup 指令。
    ```shell
    cd ~/codimd-container
    /usr/local/bin/docker-compose exec -T database pg_dump hackmd -U hackmd  > ~/${foldername}/backup.sql
    ```
    PS. 這邊需要特別注意一下在 crontab 中使用 docker-compose 的一些[坑](/Backup-CodiMD-When-Using-Docker-Compose/)...。 恩...，會這麼提醒當然是因為我踩過了 Orz ... 我老是到處踩坑...

    <br>


4. 最後把所有備份的東西壓成一份壓縮檔，然後把壓縮檔統一放到個資料夾下 e.g. ~/codimd_backup，備份階段就完成了
    ```shell
    tar zcvf ${foldername}.tgz ${foldername}
    ```
<br><br>

### 刪除 30 天前的檔案

這邊使用 find 指令來實做。利用 find 找到符合特定規則的檔案後再使用指令參數 <span class='highlighting'>-exec</span> 來刪除。這邊所指的特定規則，當然是指三十天前的壓縮檔：

```shell
# only keep 30 days backup
find ~/codimd_backup -type f -name "*.tgz" -mtime +30 -exec rm -rf {} \;
```
<br>

指令可以分成兩部份：
```shell
# only keep 30 days backup
find ~/codimd_backup -type f -name "*.tgz" -mtime +30
```
1. `~/codimd_backup`  ： 要尋找的目標路徑
2. `-type f`  ： 尋找類型為檔案  
3. `-name "*.tgz"` ： 尋找檔名為 .tgz 結尾
4. `-mtime +30`：檔案的<span class='highlighting'>最後修改時間</span>是 30 天以前的

<br><br> exec 後面的 command 是要執行的指令，這執行刪除指令：
```shell
-exec rm -rf {} \;
```
其中 <span class='highlighting'>{}</span> 指的是 find 指令找到的檔案或目錄，而 <span class='highlighting'>\;</span> 是指令的結束符號。
<br><br>

### 定期執行
Script 寫完之後最後就是讓它定期執行了，這邊直接使用 crontab 指令即可，設定方式還算是單純，只是需要稍微注意一下執行者權限與絕對路徑...等等

```shell
$ crontab -e
```

<br>此時會進入 vi 的編輯畫面，就可以進行工作的排程。注意每一行都是一項工作排程。而每項工作的格式都是由六個欄位所組成：

|**代表意義**|分鐘|小時|日期|月份|週|指令|
|---|---|---|---|---|---|---|
|**數字範圍**|0-59|0-23|1-31|1-12|0-7|就指令啊|

<br>寫起來會像是這樣
```shell
0 0 * * * sh /home/user/codimd_backup.sh
```
意思就是每天 0 點時執行 codimd_backup.sh 這個 Script。
<br><br>

### 異地備份
原本我的想法很簡單，用 scp 將備份的檔案從 CodiMD 的伺服器（簡稱 ServerA）送到備份用的伺服器（簡稱 ServerB）就好。偏偏遇到一個有點尷尬的問題，ServerA 在外網，而 ServerB 在內網，兩邊是互相 ping 不到的。最後沒辦法只好先貢獻我的電腦當跳板 (´_ゝ`)

<br> 所以整體步驟就變成需要先 Download 再 Upload 了：
<br>

#### Download
我一樣先建立一個資料夾，等等放從 CodiMD 的伺服器抓下來的備份
```shell
foldername=codimd_backup
mkdir -p -- ~/${foldername}
```

<br> 然後進行下載
```shell
filename=`ssh -i ~/.ssh/ServerA_key.pem ServerA@HostA ls -t codimd_backup|head -n 1`
scp -p  -i ~/.ssh/ServerA_key.pem ServerA@HostA:~/codimd_backup/${filename} ~/${foldername}/
```
這邊每天下載最新的備份就好，所以這邊使用了 <span class='highlighting'>ls</span> 並使檔案依照修改時間降序排序， 最後使用使用 <span class='highlighting'>head</span> 取出排序中的第一筆資料，也就是最新的資料，最後使用 <span class='highlighting'>scp</span> 將資料抓下來，指令一樣建議加上 <span class='highlighting'>-p</span> 保留 timestamps 資訊。

<br><br>

### Upload
最後再將抓下來資料送到 ServerB 
```shell
expect -c "
spawn scp -r $foldername ServerB@HostB:~/
expect {
    \"*password\" {set timeout 300; send \"myPassword\r\";}
      }
expect eof"
```

這邊麻煩的一點是，ServerB 是用密碼登入的...，所以我還得寫一個自動登入的 Script，只好邊查 expect 的[語法](https://blog.51cto.com/zyp88/1615029)邊湊出一版。
<br><br>

### 刪除 30 天前的檔案
最後我一樣希望 ServerB 備儲檔案別無限制儲存下去，所以我一樣希望它刪除 30 天前（或許 60 天？）的檔案。這邊我稍微猶豫了一下這件事是要在跳板這台機器下 Script ，還是在 ServerB 另外起一個 Script ，最後我決定偷懶跟著目前的 Script 一起做。
<br>

```shell
expect -c "
spawn ssh ServerB@HostB \"find $foldername -type f -name *.tgz -mtime +30 -exec rm -rf {} \\\; \"
expect {
    \"*password\" {set timeout 300; send \"myPassword\r\";}
      }
expect eof"
```

這指令與先前的差不多唯一需要注意的是在 <span class='highlighting'>spawn</span> 中需要注意<span class='highlighting'>字元的跳脫</span>，不然就會跟我一樣 de 了好久的 bug ...

<br> 後來雖然抓完蟲，不過還是一氣之下把 ServerB 的改成了[使用 SSH Key-based 的登入驗證方式](/Configuring-SSH-Key-Based-Authentication-on-a-Linux/)，所以程式可以化簡成：

```shell
# upload
scp -i ~/.ssh/ServerB_key  -r -p ${foldername} ServerB@HostB:~/

# only keep 30 days backup
ssh -i ~/.ssh/ServerB_key ServerB@HostB "find $foldername -type f -name *.tgz -mtime +30 -exec    rm -rf {} \; " 
```
<br><br>

### （補充）找出只存於 ServerA 的檔案並下載
後來想到一件事，我的電腦偶而會關機，所以為了以防異地備份的檔案有少，將 Download 最新的檔案，改成 Download 不在 ServerB 中的所有檔案。
<br>

```shell
diff -q folderA folderB | grep "folderA"| awk 'BEGIN {FS="："}{print $2}' > files.txt
```
 一般來說，如果要找可以 <span class='highlighting'>diff -q</span> ，可以列出哪些檔案只存於 folderA ，哪些檔案又只存於 folderB。取出只存於 folderA 檔案，再用 awk 取出檔名的部份，最後寫檔。


<br> 只是因為目前我的資料夾存在兩台不同的伺服器上，所以必須用到 <span class='highlighting'>process substitution</span> ，但 process substitution 是拿來<span class='highlighting'>暫存檔案</span>用的，如果我用 <span class='highlighting'>diff -q</span> 會行不通，所以稍微換了個方式實做：
<br> 

```shell
diff <(ssh -i ~/.ssh/ServerA_key.pem ServerA@ServerA  "ls codimd_backup|sort") <(ssh -i ~/.ssh/ServerB_key ServerB@ServerB  "ls codimd_backup|sort") | grep "^<" | awk '{print $2}' > files.txt
```
先使用 process substitution 分別列出兩台伺服器上的檔案並排序，然後使用 diff 比較兩份暫存檔的內容，濾出僅存於 ServerA 檔案，再用 awk 取出檔名的部份，最後寫檔。

<br> 最後在依序從檔案中讀出要下載的檔名，並依序下載：
```shell
cat files.txt |  while read line; do 
scp -p -i~/.ssh/ServerA_key.pem ServerA@ServerA:~/codimd_backup/${line} ~/${foldername}/
done
```

<br>對了，使用 process substitution 時，要改用 <span class='highlighting'>bash</span> 來執行 Script ，不能用 sh，不然會一直收到錯誤訊息：
>  Syntax error: "(" unexpected

<br><br>

## 參考資料
1. [第十二章、學習 Shell Scripts｜鳥哥的 Linux 私房菜](http://linux.vbird.org/linux_basic/0340bashshell-scripts.php)
2. [Shell Script簡易教學｜平凡的幸福](https://blog.twtnn.com/2013/12/shell-script.html)
3. [bash - shell script to create folder daily with time-stamp and push time-stamp generated logs｜Stack Overflow](https://stackoverflow.com/questions/14894605/shell-script-to-create-folder-daily-with-time-stamp-and-push-time-stamp-generate)
4. [Linux Script：自動備份或刪除超過特定時間之過期檔案｜符碼記憶](https://www.ewdna.com/2012/04/linux-script.html)
5. [第十五章、例行性工作排程(crontab)｜鳥哥的 Linux 私房菜](http://linux.vbird.org/linux_basic/0430cron.php#whatiscron_type)
6. [bash - How can I assign the output of a command to a shell variable?｜Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/16024/how-can-i-assign-the-output-of-a-command-to-a-shell-variable)
7. [password - Shell Script for logging into a ssh server｜Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/31071/shell-script-for-logging-into-a-ssh-server)
8. [shell 中scp密码输入 --expect｜缪阿布 - 博客园](https://www.cnblogs.com/mologa-jie/p/6064815.html)
9. [Ubuntu使用Spawn和expect实现ssh自动登陆｜Lyndon的专栏 - CSDN博客](https://blog.csdn.net/donglynn/article/details/51536212)
10. [linux expect案例用法｜小k-51CTO博客](https://blog.51cto.com/zyp88/1615029)
11. [bash - Difference between two directories in Linux｜Stack Overflow](https://stackoverflow.com/questions/16787916/difference-between-two-directories-in-linux/29299485)
12. [linux - Bash script process substitution Syntax error: "(" unexpected｜Stack Overflow](https://stackoverflow.com/questions/32038974/bash-script-process-substitution-syntax-error-unexpected)
13. [Why does process substitution result in a file called /dev/fd/63 which is a pipe?｜Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/156084/why-does-process-substitution-result-in-a-file-called-dev-fd-63-which-is-a-pipe)