---
title: 【CodiMD】安裝踩雷筆記...
date: 2021-01-26
is_modified: true
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Linux/Unix
- HackMD/CodiMD/HedgeDoc
- 協同合作工具
- 工具安裝與部署
--- 

說實話我還是搞不懂為何需要建立 CodiMD，不過上面說要建那就來建吧～！

<!--more-->


## 先來聊聊 HackMD 與 CodiMD
<p class="illustration">
<img src="https://i.imgur.com/lobcWpo.png" alt="HackMD">
HackMD 官網截圖 （圖片來源: <a href="https://hackmd.io/"> HackMD 官網</a>）   
</p>

之前提過 [HackMD](https://hackmd.io/) 是我用過最順手 Markdown 的文字編輯器。而 [CodiMD](https://demo.codimd.org) 其實就是 Open Source 的 HackMD Community Edition。

CodiMD 之前好像叫做 HackMD CE ，但為了避免命名混搖的困擾，因此中間修改過專案名稱（看 [這裡](https://github.com/hackmdio/codimd/issues/1170) 與 [這裡](https://github.com/hackmdio/codimd/issues/720)），由 HackMD CE 改成了 CodiMD。

不過，但說實話還是很混亂，而且是讓人更加的困惑！除了在一些文件與專案兩個名字會輪流出現外，還有另外一個讓人更困惑的是，名為 CodiMD 的 repository 其實是有兩個的，分別由 HackMD EE 與社群所維護的。


### 程式碼
由於上述原因它的程式碼是有兩個 repository。

1. **HackMD EE 維護**
    - Source code：[hackmdio/codimd](https://github.com/hackmdio/codimd)
    - Container： [hackmdio/docker-hackmd](https://github.com/hackmdio/docker-hackmd) / [Docker Deployment](https://hackmd.io/c/codimd-documentation/%2Fs%2Fcodimd-docker-deployment)


2. **社群維護**
    - Source code：[codimd/server](https://github.com/codimd/server)
    - Container： [codimd/container](https://github.com/codimd/container)



## 安裝步驟
這邊安裝了社群維護 CodiMD

```shell
$ git clone https://github.com/codimd/container.git codimd-container
$ cd codimd-container
$ docker-compose up
```

文件上寫的安裝指令相當簡單只有 3 條指令，執行完後，畫面上會出現  <mark>HTTP Server listening at port 3000</mark>，此時就算啟動完成了。
 
但是事情通常都是沒有這麼簡單的，我有那一次安裝環境照著指令輸完就過的？每次都會踩到一堆坑 QAQ
 

### 問題1：docker-compose 未安裝
一開始的問題是因為我沒有裝過 **docker-compose**，因此一開始會出現未安裝的提示，所以照著 Ubuntu 的提示安裝了 **docker-compose** 

```shell
$ sudo apt install docker-compose
```

<br class="big">

原本以為這樣就安裝完成了，但重新執行時跳出了下面的 Error
> ERROR: Version in “./docker-compose.yml” is unsupported. You might be seeing this error because you’re using the wrong Compose file version. Either specify a version of “2” (or “2.0”) and place your service definitions under the `services` key, or omit the `version` key and place your service definitions at the root of the file to use version 1.  
For more on the Compose file format versions, see [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)

<br class="big">

查了一下，根據 [issue #58](https://github.com/10up/wp-local-docker/issues/58#issuecomment-337963951) 的 commit，發現與 docker-compose 版號有關：
> docker-compose v3 syntax is not supported by docker-compose until version 1.10.0. You’ll need to update docker-compose to a newer version to get rid of that error. Current version is 1.16.1

<br class="big">

果然下了 `docker-compose --version` 後，發現的版號只有 **1.8.0**，所以現在有兩個選擇：
1. 修改 docker-compose.yml 的版號，由 version: '3' 降成 version: '2'。
2. 是升級 docker-compose 版本。


<br class="big">

但目前 docker-compose [版本](https://github.com/docker/compose/tags) 已經來到 1.25.0-rc1，版本差的有點多，還是升級一下好了。

```shell
$ sudo curl -L https://github.com/docker/compose/releases/download/1.25.0-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose 
$ sudo chmod +x /usr/local/bin/docker-compose 
$ docker-compose --version
```

<br class="big">

在執行 `docker-compose --version` 時 ，可能會遇到 `/usr/bin/docker-compose: 沒有此一檔案或目錄`，可以直接把先直接把檔案從 /usr/local/bin 搬到 /usr/bin，不然就是就是建立連結指令：

```shell
$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
   
<br class="big">

再次查詢版號，此時就會顯示正確版號了，到這邊 docker-compose 算是安裝完成。

```shell
$ docker-compose --version
docker-compose version 1.25.0-rc1, build 8552e8e2
```


### 問題2： could not find an available, non-overlapping IPv4 address
不過 docker-compose 安裝完，我還是不能跑 Orz，這次錯誤訊息換成了：

```shell
ERROR: could not find an available, non-overlapping IPv4 address pool among the defaults to assign to the network
```

<br class="big">

根據大家的[踩雷心得](https://stackoverflow.com/questions/43720339/docker-error-could-not-find-an-available-non-overlapping-ipv4-address-pool-am)（誤），大抵可分成三種狀況：
 1. openvpn 執行中 running，這個狀況下
	```shell
	$ service openvpn stop
	```

 2. docker network 滿了，則下
	```shell
	$ docker network prune
	```
	
3. 前兩條搞不定就試試看重起網路
	```shell
	$ sudo service network-manager restart
	```
 
<br class="big">

我自己是屬於第三種狀況，重起網路就 OK 了｡:.ﾟヽ(*´∀`)ﾉﾟ.:｡

不過我同事試過前面的指令都不行。但他後來發現 disable 掉某張網卡就行了，懷疑是與那張網卡的網段衝到了，因此他根據[這篇](http://www.itmuch.com/docker/24-docker-compose-network/)，建立一個網路然後在 yaml 中設定名稱。
```shell
networks:
 # Internal network for communication with PostgreSQL/MySQL
 backend:
   ipam:
     config:
     - subnet: 172.20.0.0/16
```

如果有人裝雙網卡且試過前面的指令都不行的，或許也可以試試看著個方法？



## 維護


### 備份與還原
若要執行備份指令的，直接下
```shell
$ docker-compose exec database pg_dump hackmd -U hackmd  > backup.sql
```

<br class="big">

根據 README 文件，而若要還原資料則是下：
```shell
$ cat backup.sql | docker exec -i $(docker-compose ps -q database) psql -U hackmd
```

<br class="big"> 

但我在還原資料時，直接得到一個 error

```shell
SET
SET
SET
SET
CREATE EXTENSION
COMMENT
ALTER TYPE
ERROR:  type "enum_Notes_permission" already exists
SET
SET
ERROR:  relation "Authors" already exists
ALTER TABLE
ERROR:  relation "Authors_id_seq" already exists
ALTER TABLE
ALTER SEQUENCE
ERROR:  relation "Notes" already exists
ALTER TABLE
ERROR:  relation "Revisions" already exists
ALTER TABLE
ERROR:  relation "SequelizeMeta" already exists
ALTER TABLE
ERROR:  relation "Sessions" already exists
ALTER TABLE
ERROR:  relation "Temp" already exists
ALTER TABLE
ERROR:  relation "Temps" already exists
ALTER TABLE
ERROR:  relation "Users" already exists
ALTER TABLE
ALTER TABLE
ERROR:  duplicate key value violates unique constraint "Authors_pkey"
DETAIL:  Key (id)=(1) already exists.
CONTEXT:  COPY Authors, line 1
 setval 
--------
      2
(1 row)

ERROR:  duplicate key value violates unique constraint "Notes_pkey"
DETAIL:  Key (id)=(139cc586-bbe4-4e08-87b5-c77dab9c2d67) already exists.
CONTEXT:  COPY Notes, line 1
ERROR:  duplicate key value violates unique constraint "Revisions_pkey"
DETAIL:  Key (id)=(d3a30eb1-24b1-4bd2-a08d-0b79a17b0d7e) already exists.
CONTEXT:  COPY Revisions, line 1
ERROR:  duplicate key value violates unique constraint "SequelizeMeta_pkey"
DETAIL:  Key (name)=(20150504155329-create-users.js) already exists.
CONTEXT:  COPY SequelizeMeta, line 1
ERROR:  duplicate key value violates unique constraint "Sessions_pkey"
DETAIL:  Key (sid)=(YU3T4MbAOIL0zoKwI1SWUphyroxHm0PS) already exists.
CONTEXT:  COPY Sessions, line 1
COPY 0
COPY 0
ERROR:  duplicate key value violates unique constraint "Users_pkey"
DETAIL:  Key (id)=(8758f908-b87f-4376-aaff-507a16ba73ff) already exists.
CONTEXT:  COPY Users, line 1
ERROR:  multiple primary keys for table "Authors" are not allowed
ERROR:  multiple primary keys for table "Notes" are not allowed
ERROR:  multiple primary keys for table "Revisions" are not allowed
ERROR:  multiple primary keys for table "SequelizeMeta" are not allowed
ERROR:  multiple primary keys for table "Sessions" are not allowed
ERROR:  multiple primary keys for table "Temp" are not allowed
ERROR:  multiple primary keys for table "Temps" are not allowed
ERROR:  multiple primary keys for table "Users" are not allowed
ERROR:  relation "Users_profileid_key" already exists
ERROR:  relation "authors_note_id_user_id" already exists
ERROR:  relation "notes_alias" already exists
ERROR:  relation "notes_shortid" already exists
```

<br class="big">

[問了一下](https://community.codimd.org/t/i-got-a-error-when-i-restored/42)，才知道不能在 CodiMD 執行時恢復資料庫...，想想也合理，我為啥會在運行去恢復資料庫阿，白痴...。還好管理者沒有嫌棄我 ~~（反正他嫌棄了我也看不到）~~，還很熱心的告訴我還原的詳細步驟：

1.  先停止所有服務，下 `docker-compose down` 或 `docker-compose down -v`，-v 會直接刪除 database。
2.  接下來單獨運行 database container，`docker-compose up -d database`。
3.  隨後執行 `cat backup.sql | docker-compose exec database psql -U hackmd`，從備份中還原。
4.  完成後，重新運行服務 `docker-compose up -d` 。

不過這恢復步驟有個前提就是：  <mark class="danger">nothing meaningful in the database yet</mark>。

<br class="big">

如果是用預設那份 yml 檔，mount 的那個資料夾應該會對應到 **/var/lib/docker/volumes/codimd-container_database**，原本想說能不能用 `docker volume create`，並複製資料裡面的東西到新的 volume 中，事實證明可以，但不確定這樣的複製方法與文件的備份方法差了多少資料。

需要注意的是，在複製 volume 必須確保 database 已經停止，不然資料可能會不一致，因此管理者還是推薦用文件的備份方法 。


### 軟體升級
```shell
$ cd codimd-container ## enter the directory
$ git pull ## pull new commits
$ docker-compose pull ## pull new containers
$ docker-compose up ## turn on
```
升級看來還滿簡單，不過因為 yml 檔與 config.json 有做了點客製化，所以沒有打算直接從 repository git pull 下來，我是直接換掉目前 yml 檔中 CodiMD 的 image 來源，取代 git pull 的步驟。


<br class="big">

不過重新 `docker-compose up` 時遇到了點小問題
```shell
2019-06-04T06:16:56.142Z error: 	uncaughtException: HSTS must be passed a numeric maxAge parameter.
  TypeError: HSTS must be passed a numeric maxAge parameter.
    at Function.hsts (/codimd/node_modules/hsts/index.js:27:11)
    at Object.<anonymous> (/codimd/app.js:85:18)
    at Module._compile (module.js:653:30)
    at Object.Module._extensions..js (module.js:664:10)
    at Module.load (module.js:566:32)
    at tryModuleLoad (module.js:506:12)
    at Function.Module._load (module.js:498:3)
    at Function.Module.runMain (module.js:694:10)
    at startup (bootstrap_node.js:204:16)
    at bootstrap_node.js:625:3
```

問題不大，直接照 [issue](https://github.com/hackmdio/codimd/issues/1159) 所說的，修改 config.json 即可。


### 使用者管理
超蠢的一件事，我剛註冊完帳號回頭就忘了我自己的密碼，偏偏 UI 上又沒有忘記密碼的選項 Orz，只好去[問問](https://github.com/codimd/container/issues/34)，還好一條指令就搞定了。
```bash
$ docker-compose exec codimd ./bin/manage_users --reset <mail address>
```


### 環境配置
CodiMD 環境配置有兩種方法一是配置 [Environment variables](https://github.com/codimd/server/blob/master/docs/configuration-env-vars.md)，或是使用 [Config file](https://github.com/codimd/server/blob/master/docs/configuration-config-file.md)



## 小記
HackMD / CodiMD 是真的很好用，但說實話建置說明文件偏少（還是我太弱了！？），建置上相當的不方便。但管理者相當的熱心，回覆都還滿熱心且迅速的，如果有問題可以直接到 [論壇](https://community.codimd.org/) 提問。



## 參考資料 
1.  [CodiMD Documentation｜HackMD](https://hackmd.io/c/codimd-documentation/%2Fs%2Fcodimd-documentation) 
2. [建立個人用的 Markdown 筆記只要幾分鐘，使用 VirtualBox + Docker + HackMd (CodiMd)｜TerryL](https://terryl.in/zh/private-markdown-note-by-vagrant-docker-hackmd/)
3. [How To Install Docker Compose on Ubuntu 16.04｜DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-16-04)
4. [Docker “ERROR: could not find an available, non-overlapping IPv4 address pool among the defaults to assign to the network”｜Stack Overflow](https://stackoverflow.com/questions/43720339/docker-error-could-not-find-an-available-non-overlapping-ipv4-address-pool-am)
5. [Docker系列教程24-Docker Compose网络设置｜周立的博客 关注Spring Cloud、Docker](http://www.itmuch.com/docker/24-docker-compose-network/)



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-01-26</summary>
  <ul>
    <li>2021-01-26 更新：repository 資訊</li>
    <li>2019-06-04 發布</li>
  </ul>
</details>
