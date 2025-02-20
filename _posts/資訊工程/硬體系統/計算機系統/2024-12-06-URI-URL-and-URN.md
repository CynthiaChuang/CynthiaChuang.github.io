---
title: URI、URL 與 URN 
date: 2023-12-06
is_modified: false
categories:
- "硬體系統 › 計算機系統"
tags:
- "計算機概論" 
---

在學習網路相關知識的過程中，URI、URL 和 URN 這三個術語會反覆出現。好吧，主要是 URI 和 URL 使用得比較多，反而是 URN 比較少被提及。這篇網誌就簡單整理這三個術語的定義與差異。
 
是說，寫完後一直沒發出來，導致我都有點忘了當初我為啥會寫這篇筆記了。好像是命名變數時，經常在 URI、URL 和 URN 之間拿不定主意吧？🤔但如果你也對這些術語感到困惑，這篇可以賞光一下~

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/LOUwjvI.png" alt="URI 可以是 URL 或 URN，甚至同時具備兩者的特性。" width="450px">
    URI 可以是 URL 或 URN，甚至同時具備兩者的特性。（圖片來源: <a href="https://zh.wikipedia.org/zh-tw/统一资源名称">維基百科</a>）
</p>


## URI （Uniform Resource Identifier，統一資源標識符）
URI，即統一資源標識符，呃...算了，先別管這個拗口詞，反正我平時也沒怎麼用這個詞。我們還是來看看它的英文定義好了，在 [RFC2396](https://datatracker.ietf.org/doc/html/rfc2396) 這份文件中，對 [URI 的定義是](https://datatracker.ietf.org/doc/html/rfc2396#section-1.1)：

- **Uniform**  
    允許在同一上下文中使用不同類型的資源標識符，並統一解釋常見的語法規則。這樣可以讓協議方案的增擴變得更加容易。
    
- **Resource**      
    資源指的是可以被識別的任何事物，像是文件、圖像、服務...等，不僅限於具體物理實體，也可以是虛擬資源，甚至是一組實體的集合。
    
- **Identifier**  
   標識符，用來表明可識別的資源。
   
寫的超複雜的有沒有！！其實只要記住，<mark>URI 就是用來標示資源的字串，無論是抽象的還是實體的都可以</mark> 就好，像是網路上的各種資源（HTML 文件、圖像、音檔、影片、程序等）都可以用 URI 來定位。

換句話說，**URI 是資源的抽象定義**，而要進行實際定位，通常有兩種方法：
1. URL （Uniform Resource Locator，統一資源定位符）
2. URN （Uniform Resource Name，統一資源名稱）

<p class="illustration">
    <img src="https://i.imgur.com/JdMhaCu.png" alt="URI 是資源的抽象定義，其中包含了兩種實際定位方式 URL 與 URN" width="400px">
   URI 是資源的抽象定義，其中包含了兩種實際定位方式 URL 與 URN（圖片來源: <a href="https://www.readfog.com/a/1660187374166052864">閱坊</a>）
</p>


## URL （Uniform Resource Locator，統一資源定位符）
URL 就是我們常說的**網址**，用來描述是網際網路上資源的位址，它是<mark>具體的定位方法，告訴我們如何到達資源</mark>。有點像是網路上的**門牌號碼**，可以直接把你導航到網站的特定頁面。

例如，定位 HTML 資源的 URL 可能是：
```html
https://www.example.com.tw
https://www.example.com/index.html
```

然而，並非所有的 URL 都是以 HTTP/HTTPS 開頭，還有許多其他協議可用，比如：ftp、mailto、telnet、file...等。

```html
ftp://user:password@ftp.gitee.com:21/code/chat/chat.py
file://localhost/code/chat/chat.py
```

<br class="big">

一般來說，URL 由三個部分組成：
1. 協議（或稱服務方式）
2. 資源所在伺服器的名稱或 IP 地址（有時也包括埠號）
3. 主機資源的具體地址，如目錄和檔名等。

<p class="illustration">
    <img src="https://i.imgur.com/BBzwTm1.png" alt="URL 是由協議、伺服器地址、檔名地址所組成">
    URL 是由協議、伺服器地址、檔名地址所組成
</p>

## URN （Uniform Resource Name，統一資源名稱）
接下來，我們來看看最後一個術語 URN。說個有趣的插曲，我在查資料時，Google 搜尋結果的第一項居然是骨灰罈...不過在這邊 URN 是指<mark>某個特定資源的名稱</mark>，它不關心資源在何處，只關心資源是誰。


跟 URL 一樣，URN 也是由三個部份組成：
```bash
<URN> ::= "urn:" <NID> ":" <NSS>
```    

1. URN：表示這是一個 URN 方案標識。
2. NID： 註冊於 [IANA](https://www.iana.org/)的命名空間標識符。
3. NSS：具體的命名空間字串。

<br class="big">

**ISBN（國際標準書號）** 是一個常見的 URN 例子，ISBN 可以用來表示特定書籍，即便我把書本從書局帶回我家，它的 ISBN 依然能唯一識別這本書，就像是牛被牽到北京，它還是牛。

如果將書籍的 ISBN `ISBN 10 :9780132350884`，寫成 URN 的格式，就是：
```bash
URN:ISSN:9780132350884
```    
    
<br class="big">

最後舉個例子，如果今天要在台灣找到某個具體的人（URI），比如用地址來描述：例如臺北市中正區龍福里重慶南路二段永和寓所的主人，那就是 URL。如果用身份證號碼加上姓名（例如：賴清德），那就是 URN 了。

簡而言之，<mark>URI 是對資源的定義，它既可以是抽象的描述，也可以是具體的表達</mark>。這些描述方式可以通過 URN 來標識某個事物的身份，或通過 URL 來提供尋找該事物的具體方法。簡單來說，URI、URL 和 URN 在網路資源定位中各自扮演著不同的角色，彼此協作，幫助我們準確地找到所需的資源。

## 參考資料 
1. GreedIsGood (2021-09-20)。[URL / URN / URI 的定義](https://ithelp.ithome.com.tw/articles/10266610?sc=pt)。檢自 iT 邦幫忙 (2023-07-05)。
2. 蜗牛蒲哥 (2022-07-30)。[URL与URI，有联系有区别？](https://zhuanlan.zhihu.com/p/38120321)。檢自 知乎 (2023-07-05)。
3. [一文帶你理解 URI 和 URL 有什麼區別？](https://www.readfog.com/a/1660187374166052864)。檢自 閱坊 (2023-07-05)。
4. 刘Java (2021-08-04)。[URI和URL的概念和区别](https://juejin.cn/post/6992383657340551204)。檢自 掘金 (2023-07-05)。
5. 協同撰寫。[統一資源標識符](https://zh.wikipedia.org/zh-tw/统一资源标志符)。檢自 維基百科 (2023-07-05)。
6. 協同撰寫。[統一資源定位符](https://zh.wikipedia.org/zh-tw/统一资源定位符 )。檢自 維基百科 (2023-07-05)。
7. 協同撰寫。[統一資源名稱](https://zh.wikipedia.org/zh-tw/统一资源名称)。檢自 維基百科 (2023-07-05)。
8. [URN](https://www.issn.org/zh-hans/services-et-prestations/services-en-ligne/urn/)。檢自 ISSN (2023-07-05)。

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-12-06</summary>
  <ul>
    <li>2023-12-06 發布</li>
    <li>2023-07-06 完稿</li>
    <li>2023-07-05 起稿</li>
  </ul>
</details>
