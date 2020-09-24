---
title: 雲端計算 IaaS、PaaS、SaaS 與 FaaS
date: 2020-05-26
is_modified: false
categories:
- Cloud-Computing
tags:
- Cloud Platform
- Saas
- Paas
- Iaas
- Fass/Serverless
--- 

最近（或該說前一陣子？）在看 FasS/Serverless 相關文章，順帶提一提一些 SaaS、PaaS 與 IaaS。

<!--more-->

<center> <img src="https://i.imgur.com/Aj0HhN7.jpg" alt="IaaS、PaaS、SaaS 與自行架構比較。"></center>
<center class="imgtext">IaaS、PaaS、SaaS 與自行架構比較。（圖片來源: <a href="https://www.ionos.com/digitalguide/server/know-how/caas-container-as-a-service-service-comparison/" class="imgtext">IONOS</a>）</center>

<br><br> 

## Infrastructure as a Service，IaaS

<center> <img src="https://i.imgur.com/ljyHR1Y.png" alt="IaaS"></center>
<center class="imgtext">IaaS（圖片來源: <a href="https://azure.microsoft.com/zh-tw/overview/what-is-iaas/" class="imgtext">Microsoft Azure</a>）</center>
<br>

Infrastructure as a Service 翻譯為**基礎設施即服務**，一般簡稱 **IaaS**，顧名思義就是 <span class='highlighting'>服務商提供基礎設施作為他們的服務</span>，是一種雲端運算產品。

在此服務（或稱商品）中，服務商會提供底層/物理層基礎設施，諸如：伺服器、資料中心、伺服器機房、儲存及網路...等，各種使用者所需用來處理、儲存、計算與網路的運算資源。<br>

由於此項服務的提供，使用者無須在自行建構伺服器、軟體等網路設備，即可任意部署和運行相關資源。

雖然此模式無法控管或控制底層的基礎設施，或者只能有限度地控制特定的網路元件，但使用者也無須花費額外人力進行採購、配置與維運，也能依照實際使用需求快速擴展與收縮，使用者可能專注於此服務以外的項目，例如：部署和執行所需執行作業系統或應用程式...等，即可。

此外，IaaS 通常可提供高可用性、商務持續性與災害復原等服務，避免資料因天災或人禍而損毀或中斷。


<br><br> 

## PaaS

<center> <img src="https://i.imgur.com/XKLsrUQ.png" alt="PaaS"></center>
<center class="imgtext">PaaS（圖片來源: <a href="https://azure.microsoft.com/zh-tw/overview/what-is-paas/" class="imgtext">Microsoft Azure</a>）</center>

<br>

Platform as a Service 翻譯為**平台即服務**，一般簡稱 **PaaS**，在這項服務中供應商會提供使用者開發與管理所需的平台環境，如此一來，使用者只需要專注在資料處理與應用程式的開發項目...等，免除開發環境設置、相依軟體安裝、安全性、伺服器軟體及備份...等困擾。
 
因為 PaaS 是基於 IaaS 所提供的服務，因此它也同時具備 IaaS 所帶來的優勢，並提供額外，如：<span class='highlighting'>開發時間減少</span>、<span class='highlighting'>便捷管理應用程式生命週期</span>...等優勢。
 
<br><br> 

## SaaS

<center> <img src="https://i.imgur.com/J0otbOf.png" alt="SaaS"></center>
<center class="imgtext">SaaS（圖片來源: <a href="https://azure.microsoft.com/zh-tw/overview/what-is-saas/" class="imgtext">Microsoft Azure</a>）</center>

<br>

Software as a Service 翻譯為**軟體即服務**，簡稱 **SaaS**。它可以讓使用者透過網際網路和瀏覽器等媒介，提供使用者所需軟體服務，最常見的如：電子郵件、日曆...等。

使用者無須購買、安裝、更新或維護任何硬體、中介軟體，即可以使用該軟體，且資料放在雲端，基本上只要裝置可以連網，幾乎可以從世界各地存取應用程式，是目前日常生活中最常接觸到的服務。

<br><br> 

## FaaS
Function as a Service，這部分去可以看看[之前的筆記](/Serverless-Use-Cases-Study-Notes-01#什是-serverless)，這是將整個程式運行環境給託管出去的，將開發出來的函式直接放動雲端去當作服務在調用，特點有：

- 片段程式碼，按需執行、按需擴展、無需管理任何基礎設施相關部份。
- 事件驅動行計算。函式被事件觸發或是被 HTTP 請求調用。

簡單來說：
<div class="blockquote-center">
無需管理、按需執行、按需擴展、按使用來計費
</div>

 

<br><br> 

## 參考資料 
1.  (2017-09-01)。[Iaas、Pass、Saas 傻傻分不清楚](https://dotblogs.com.tw/007_Lawrence/2017/08/21/155203) 。檢自 我想與您們分享我的點點滴滴 點部落 (2020-03-19)。
2. (2018-12-31)。[IaaS, PaaS, SaaS, BaaS, Faas](https://www.itread01.com/content/1546260315.html) 。檢自 IT閱讀 (2020-03-19)。
3. Javier Barabas。[IaaS、PaaS 及 SaaS - IBM Cloud 服務模式](https://www.ibm.com/tw-zh/cloud/learn/iaas-paas-saas) 。檢自 IBM (2020-03-19)。
4. Vincent Ke (2018-12-31)。[已經進入SaaS / PaaS / IaaS 時代已久，還在用舊時代想法規劃網站嗎？](https://progressbar.tw/posts/51) 。檢自 progressbar (2020-03-19)。
5. (2019-08-01)。[Iaas、Paas、Saas 哪種適合您的需求?](https://accord-tec.com.tw/iaas%E3%80%81paas%E3%80%81saas-%E5%93%AA%E7%A8%AE%E9%81%A9%E5%90%88%E6%82%A8%E7%9A%84%E9%9C%80%E6%B1%82/) 。檢自 台灣雅閣科技  (2020-03-19)。
8. [何謂 IaaS？](https://azure.microsoft.com/zh-tw/overview/what-is-iaas/) 。檢自 Microsoft Azure (2020-03-19)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期： 2020-05-26</summary>
  <ul class="timestamp">
    　<li>2020-05-26 發布</li>
    　<li>2020-05-25 完稿</li>
  </ul>
</details>

