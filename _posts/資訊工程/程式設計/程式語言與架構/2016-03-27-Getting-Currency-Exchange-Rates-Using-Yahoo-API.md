---
title: 【Android】使用 Yahoo API 抓取即時匯率
date: 2016-03-27
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- Android
--- 

<div class="alert danger"> 
<div class="head">網誌已過時！</div>
Yahoo! 財務 API 服務，該服務已停止並且不再起作用。<br>
我已經換新的 API 實作了，改天有空再來寫一篇新的。
</div>
 
最近S老大派下拿來練手的是一個匯率換算的 APP，今天總算告一個段落了～稍微記錄一下免得之後又忘記了。

<!--more-->
 
實作要求有二：
 1. 使用 Yahoo API。
 2. 透過 HttpURLConnection class 將資料以 Json 型態取下並顯示在手機上。


<br><br>

## 資料獲取

### 1. Yahoo API 提供的 URL 分析

點擊這個的 URL 會得一份 CSV 檔案，文件內容會是新臺幣兌美金的匯率。  

[http://download.finance.yahoo.com/d/quotes.csv?e=.csv&f=c4l1&s=TWDUSD=x](http://download.finance.yahoo.com/d/quotes.csv?e=.csv&f=c4l1&s=TWDUSD=x)  

其中 URL 中
- **f=c4l1** <br>
這是只取下**兌換幣別（美金）的代碼與匯率**。若想取得更詳細的資料，可以將 **f=c4l1** 換成 **f=sl1d1t1**，這個分別指定了代碼（含原始幣別跟兌換幣別）、匯率、日期、時間這四個欄位。
<br>

- **s=TWDUSD=x** <br>
其中 **TWD（新臺幣）為原始幣別**、 **USD （美金）為兌換幣別**，可以依需求自行更換，如果要查詢多筆資料則在後在後接續即可，例如：
    ```
    http://download.finance.yahoo.com/d/quotes.csv?e=.csv&f=c4l1&s=TWDUSD=x,TWDJPY=x
    ```

<br>

### 2. 轉成JSON檔
接下來要將 CSV 檔轉成 JSON，只要透過 Yahoo 所提供的 [csv parser](https://developer.yahoo.com/yql/console/?q=select%20%252a%20from%20csv%20where%20url%253D%2522http%253A%252F%252Ffinance.yahoo.com%252Fd%252Fquotes.csv%253Fe%253D.csv%2526f%253Dc4l1%2526s%253DEURUSD%253DX%252CGBPUSD%253DX%2522%23h=select%2520%252a%2520from%2520csv%2520where%2520url%253D%2522http%253A//finance.yahoo.com/d/quotes.csv%253Fe%253D.csv%2526f%253Dc4l1%2526s%253DUSDEUR%253DX%2522%253B)，將第一步的 URL 轉換成 YQL（Yahoo Query Language）格式的 URL 即可。  
      
在 YOUR YQL STATEMENT 的框中將 SQL 修改為 
```sql
select * from csv where url='上面獲得 csv檔的url'
```
按下 test 按鈕，即可得到對應 JSON檔的新 URL。
      
    
<br>

### 3. 取得全部幣別 

因為我希望使用下拉式的方式讓使用者選擇原始幣別與兌換幣別，避免輸入錯誤的可能，所以我必須先取得一份全部幣別。       
      
透過這個 [URL](http://finance.yahoo.com/webservice/v1/symbols/allcurrencies/quote?format=json) 就可以取得所有幣別 JSON 檔，只是上面只有代碼，沒有貨幣全稱，不過還好規範命名規則是走 [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)，還是有辦法知道，麻煩了點就是。  
    
<br><br>

## 實作
大概了解資料的處理流程後，就可以回到程式中來實做了。

### 1. 在程式中獲取 JSON 資料
在要讀取匯率資料的地方貼上，下列程式碼，如此就可以取得 JSON 檔。

```JAVA
String urlConnect = "對應JSON檔的新URL";
URL url = new URL(urlConnect);
HttpURLConnection conn = (HttpURLConnection)url.openConnection();
InputStreamReader isr = new InputStreamReader(conn.getInputStream());
BufferedReader in = new BufferedReader(isr);
String line = in.readLine();
```

<br>
      
### 2. 解析 JSON 檔內容  
讀到資料就像下面這麼一大串，不過這樣其實很不好看，所以我會用 [JSON 的排版器](http://www.bodurov.com/JsonFormatter/)排一下，好看一下該怎麼拆。

```json
{"query":
  {"count":2,
   "created":"2016-03-15T03:00:22Z",
   "lang":"zh-TW",
   "diagnostics":{
       "publiclyCallable":"true",
       "url":{
           "execution-start-time":"1",
           "execution-stop-time":"629",
           "execution-time":"628",
           "content":"http://download.finance.yahoo.com/d/quotes.csv?e=.csv&f=c4l1&s=TWDUSD=x,TWDJPY=x"
       },
       "user-time":"629",
       "service-time":"628",
       "build-version":"0.2.420"
    },
    "results":{
        "row":[
            {"col0":"USD","col1":"0.0305"},
            {"col0":"JPY","col1":"3.4708"}
        ]
   }
  }
}
   ```

<br>

基本上在 JSON 中
- 大括號 { } 指物件（object）
- 中括號 [ ] 指陣列（array）
- 引號 "" 則是指元素


所以像上面這一串所要取出的順序是這一串物件中 → query 物件 → results 物件 → row 陣列，最後在取出每個 index 中所包含的物件，而物件中的元素 col0、col1 分別對應到兌換幣別（美金）的代碼與匯率。  

<br><br>

## 踩雷

基本上做到上面就完成大概了，只是實際執行的時候，會跳出下面兩個 error。

### 1. android.os.NetworkOnMainThreadException
查了一下，這個是因為我把網路的活動跑在 main Thread 上，貼心過頭 Google 大神告訴你，你的 APP 可能會因為等待網路活動的回應太久，而被系統強制關閉。 

解決的方式只要開新的執行緒就好，不管是用 Thread、 Handle、 AsynTask 都行，別讓它跑在 main Thread 上就行。
 
<br>
 
### 2. android.system.ErrnoException: android_getaddrinfo failed: EACCES (Permission denied)

另一個會收到的是這個，主要是告訴你 uses-permission 忘了開，只要去 AndroidManifest.xml 中添加即可  
  
<br><br>

## 參考資料
1. [Android行動裝置網路程式應用｜ITs通訊](http://newsletter.ascc.sinica.edu.tw/news/read_news.php?nid=2665)
2. [通过Yahoo API 获取实时货币汇率｜ouyb_zou的专栏](http://blog.csdn.net/ouyb_zou/article/details/43734979)
3. [调用YAHOO API监控外汇汇率｜raynix 筆記](http://raynix.info/archives/2216)
4. [获取汇率webservice｜DamonFish’s Blog](http://damonfish.github.io/blog/2015/03/02/-%E8%8E%B7%E5%8F%96%E6%B1%87%E7%8E%87webservice.markdown/)
5. [《Android》android.os.NetworkOnMainThreadException 錯誤｜攝即是空](http://bibby1101.pixnet.net/blog/post/47753182-%E3%80%8Aandroid%E3%80%8Bandroid.os.networkonmainthreadexception-%E9%8C%AF%E8%AA%A4)
6. [Error message 'java.net.SocketException: socket failed: EACCES (Permission denied)'｜Stack Overflow](http://stackoverflow.com/questions/11273197/error-message-java-net-socketexception-socket-failed-eacces-permission-denie)
 