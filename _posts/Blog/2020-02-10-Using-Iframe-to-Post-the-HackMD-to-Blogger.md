---
title: 【Blogger】透過 iframe 把 HackMD 貼到 Blogger
date: 2020-02-10
categories:
- Blog
tags:
- Blogger
- HackMD/CodiMD
- HTML
--- 

那天在閒逛的時候發現很酷的一招，可以直接把 HankMD 的內容直接貼到 Blogger。
 
紀錄一下，改天搞不好派的上用場？

<!--more-->
<br><br> 

## 實做步驟
1. 先準備一份 HackMD 文件，注意權限問題。
2. 複製 HackMD 的固定網址，應該會長這樣：  
    ```html
    https://hackmd.io/@<使用者名稱>/<固定網址>
    ```
3. 開啟 Blogger 的文章編輯，將 HackMD 透過 iframe 貼上。

    ```htmlmixed
    <iframe src='HackMD 的固定網址' 
            width="600px" 
            height="4096px"
            frameborder="0" 
            scrolling="no"
    </iframe>
    ```

<br><br> 
 
## 小結 
貼上是貼上了啦，不過 HackMD 的白底跟我 Blogger 的黑底反差超大，有點突兀，看來要慎選主題。

<br><br> 

## 參考資料 
1. sam-liaw (2020-01-21)。[覺得 Google 的 Blogger 不太順手?透過 HTML 的 iframe 移花接木 HackMD](hhttps://sam1221.blogspot.com/2020/01/google-blogger-html-iframe-hackmd.html) 。檢自 sam (2020-02-10)。
