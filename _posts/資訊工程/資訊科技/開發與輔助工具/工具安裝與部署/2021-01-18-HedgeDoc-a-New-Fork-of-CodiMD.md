---
title: CodiMd 與 HedgeDoc 的愛恨情仇（誤）
date: 2021-01-26 21:09
is_modified: false
categories:
- "資訊科技 › 開發與輔助工具"
tags:
- Linux/Unix
- HackMD/CodiMD/HedgeDoc
- 協同合作工具
- 工具安裝與部署
--- 

有天 PM 找了詢問關於 CodiMd 的新功能 **[Image Lightbox Support](https://hackmd.io/@codimd/release-notes/%2F%40codimd%2Fv2_2_0#Image-Lightbox-Support)**，並拜託我測試下這個功能。不過，當我用了上次的[安裝方法](/How-to-Setup-CodiMD)升級後，出現的卻是一個名為 **HedgeDoc** 頁面，著實嚇了我一跳。

<!--more-->
<center> <img src="https://i.imgur.com/sOg1sTf.png?1" alt="HedgeDoc"></center>
<center  class="imgtext">HedgeDoc（圖片來源: <a href="https://demo.hedgedoc.org/"  class="imgtext">HedgeDoc Demo</a>）</center>

<br>

## 變更歷史

認真回顧了下，它們兩個的[恩怨情仇](https://github.com/hackmdio/codimd/issues/1219)（！？），總結來說就是社群被原本開發團隊氣到（？），因此重新取名、換了 logo，並且使用 TypeScript 重新撰寫了 **HedgeDoc**。

還是一頭霧水？正常，它們歷經了好幾次的變更與命名，這真的會把人搞暈。

<center> <img src="https://i.imgur.com/uAhfGhq.jpg?1" alt="把人搞暈了"></center>
<center  class="imgtext">把人搞暈了（圖片來源: <a href="https://www.pexels.com/zh-tw/photo/4458420/"  class="imgtext">Yan｜Pexels</a>）</center>

<br>

這得先從他們共同的起源—**HackMD** 說起，HackMD 是個主打即時文件協作（Real-time collaboration）的共享筆記工具，聽說它是由一份資訊安全的<mark>學校作業</mark>誕生的。嘖，大神就是大神，作業寫一寫，就推出了一個這麼棒的產品（來自魯蛇的忌妒 :upside_down_face: 

後來 HackMD 逐漸成熟穩定後，遂推出了商業版本 **HackMD EE（Enterprise Edition，企業版）**；但同時，團隊也保持開源版本 **HackMD CE（Community Edition，社區版）**，不過後來為了避免兩個版本間的混淆，因此把 HackMD CE 改名成 CodiMD。

在這期間社群也基於此開源版本進行開發，但小尷尬的是社群版也是沿用了 CodiMD 這個名稱。所以一時間 Github 上存在兩個名為 CodiMD 的儲存庫—**[hackmdio/codimd](https://github.com/hackmdio/codimd)** 與 **[codimd/server](https://github.com/hedgedoc/hedgedoc)**，但說實話這對剛入門的人來說是非常難以理解的。

因此基於種種原因，社群版的 CodiMD 後來更名成為 HedgeDoc，並將 logo 換成一隻可愛小刺蝟。

<br> 

總結來說：

<div class="blockquote-center">
HedgeDoc 是 CodiMD 的社群驅動分支，而後者是 HackMD 的開源版本。
</div>
 
<center> <img src="https://i.imgur.com/FqoEzbr.jpg" alt="HedgeDoc History"></center>
<center  class="imgtext">HedgeDoc History（圖片來源: <a href="https://hedgedoc.org/history/"  class="imgtext">HedgeDoc 官網</a>）</center>

<br><br>

## HedgeDoc

因為我之前所使用的 [Container](https://github.com/codimd/container) 是由社群所維護的，因此升級完後才會跑出那隻可愛的小刺蝟，出來的網站就像 [Demo 網站](https://demo.hedgedoc.org/)這樣。

HedgeDoc 的安裝步驟在之前的[《安裝踩雷筆記…》](/How-to-Setup-CodiMD)有介紹過，這邊就不再複述了。不過還是附上程式碼與 Container：
- Source code：[codimd/server](https://github.com/codimd/server)
- Container： [codimd/container](https://github.com/codimd/container)

<br>

對了，那刺蝟真的好可愛 :heart_eyes: ，害我對 HedgeDoc 愛不釋手...

<center> <img src="https://i.imgur.com/WDNpZ7v.png" alt="可愛的小刺蝟"></center>
<center  class="imgtext">可愛的小刺蝟（圖片來源: <a href="https://pixabay.com/zh/photos/hedgehog-cute-animal-little-nature-1215140/"  class="imgtext">Pixabay</a>）</center>

<br><br>

## CodiMd

不過，從那張歷史分支圖看來，兩邊的開發紀錄已經分開了。而 PM 所要求的功能的圖片放大縮小功能， 是 Hackmd 團隊底下所維護的 CodiMd 才有，所以還是得來看看怎安裝。


按照 CodiMd 官方的 [Docker Deployment](https://hackmd.io/c/codimd-documentation/%2Fs%2Fcodimd-docker-deployment) 的文件，我們一樣可以藉由 `docker-compose.yml` 部屬，其中 `<my_user>` 與 `<my_password>` 換成自己的名字跟密碼：
```yaml
version: "3"
services:
  database:
    image: postgres:11.6-alpine
    environment:
      - POSTGRES_USER=<my_user>
      - POSTGRES_PASSWORD=<my_password>
      - POSTGRES_DB=<database>
    volumes:
      - "database-data:/var/lib/postgresql/data"
    restart: always
  codimd:
    image: hackmdio/hackmd:2.3.2
    environment:
      - CMD_DB_URL=postgres://<my_user>:<my_password>@database/<database>
      - CMD_USECDN=false
    depends_on:
      - database
    ports:
      - "3000:3000"
    volumes:
      - upload-data:/home/hackmd/app/public/uploads
    restart: always
volumes:
  database-data: {}
  upload-data: {}
```

稍微研究了一下 HedgeDoc 的 [`docker-compose.yml`](https://github.com/hedgedoc/container/blob/master/docker-compose.yml)。我想兩者最主要的差別在於在於使用的 image 不同：
- [CodiMd](https://hub.docker.com/r/hackmdio/hackmd/tags/?page=1&ordering=last_updated)
- [HedgeDoc](https://quay.io/repository/hedgedoc/hedgedoc?tag=1.7.2&tab=tags)

<br>

啟動用的指令一樣是用：
```bash
$ docker-compose up 
``` 

不過超衰的的是，我又撞到 **could not find an available, non-overlapping IPv4 address** 的問題，按照[之前的經驗](/How-to-Setup-CodiMD#%E5%95%8F%E9%A1%8C2%EF%BC%9A-could-not-find-an-available-non-overlapping-IPv4-address)，試著重起網路：
```bash
$ sudo service network-manager restart
``` 
 
打完收工！理論上 DB 跟 Volume 也要處理，但 PM 只是要先嘗試新功能，所以也就不著急配置，先丟給 PM 玩玩，有需要再來處理吧。


<br><br> 

## 參考資料 
1. [CodiMD - release-notes](https://hackmd.io/@codimd/release-notes/) 。檢自 HackMD (2021-01-18)。
2. Maxine Maz (2019-05-09)。[為工程師文件而生的協作平台：HackMD 開發故事](https://medium.com/starrocket/hackmd-product-story-1e332f83d343) 。檢自 Star Rocket｜Medium (2021-01-18)。
3. Daisy Tsai (2019-11-25)。[HackMD 技術長：從寫程式進化到做產品 你需要換一種思考方式！](https://tw.alphacamp.co/blog/hackmd-cto-ama-sharing) 。檢自 ALPHA Camp Blog (2021-01-18)。
4. HedgeDoc developers。[HedgeDoc History](https://demo.hedgedoc.org/)。檢自 HedgeDoc - Markdown 協作筆記 (2021-01-18)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-01-26</summary>
  <ul>
    <li>2021-01-26 發布</li>
    <li>2021-01-26 完稿</li>
    <li>2021-01-18 起稿</li>
  </ul>
</details>
