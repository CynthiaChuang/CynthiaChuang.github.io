---
title: 【Android】取得所在（預設）地點的貨幣
date: 2016-03-21
is_modified: false
categories:
- Computer-Language/Framework
tags:
- Android
- App
--- 

這篇還是還是匯率相關XDDD
    
在實作匯率 App 時，我想讓 App 預先載入使用者最常使用及查詢的貨幣，讓使用者有比較好的 UX，只是到底怎麼定義使用者最常使用及查詢的貨幣，讓我有點傷透腦筋。

<!--more-->
<br> 

## 使用者最常使用及查詢的貨幣?
一開始，是有想說透過 GPS 取得使用者所在位置，然後取得所在位置的幣別，作為最常使用的貨幣，想說你都在當地應該會用那邊的貨幣吧？

只是如此一來在 OnCreate 要作的是就會很繁重，要取得位置及幣別，還要取得即時匯率，應該會被 ANR（Application Not Responding），而且我覺得匯率換算 App 跟使用者要位置的權限也頗奇怪。

最後是採用了所在（預設）位置，就是一開始在設定手機時，所選擇的國家（位置），然後取出它的幣別，當作我的預設幣別。

另外一方面，最常查詢的貨幣也是頗令人頭痛的東西。

原本是想說，有沒有辦法在得知預設幣別後，去找出這個國家最常兌換的貨幣，例如台灣最常查詢日幣、美金、人民幣…，只是 servey 不到可用的 API，所以目前只好先寫死固定的貨幣，之後再來修正吧QAQ

<br><br> 

## 取得所在（預設）地點的貨幣
在這邊使用了兩個類別，分別是 [Locale](http://developer.android.com/intl/zh-cn/reference/java/util/Locale.html) 及 [Currency](http://developer.android.com/intl/zh-tw/reference/java/util/Currency.html)

> Locale represents a language/country/variant combination. Locales are used to alter the presentation of information such as numbers or dates to suit the conventions in the region they describe.

<br> 語言環境（Locale） 代表一個語言/國家/ variant 組合。這三個參數足夠的資訊來描述該地區的語言文化及行為。不過說是這說，我有點搞不清楚最後的 variant 是在描述哪一部分，在Android官網中是給了這樣個範例：
```java
new  Locale("en", "US", "POSIX")
```

<br>

先不管 variant 的部分，目前我想取出的是 **國家** 這項資訊：
 
 1. 取出預設位置
	```java
	// 取得預設的語言環境
	Locale mDefault = Locale.getDefault()
	// 取得目前使用語言
	String mLanguage = Locale.getDefault().getLanguage();
	// 取得目前國家區域
	String mCountry = Locale.getDefault().getCountry();
	```
	<br>
 
 2. 取得該位置所對應貨幣代碼  
	在 [Currency](https://www.tutorialspoint.com/java/util/currency_getinstance.htm) 中提供了一個 function，可藉由傳入 Locale 參數取得到該 Locale 國家所對應的貨幣，一旦得知該貨幣，就可取出得該貨幣的代碼。
	
	```java
	// 藉由所取得的預設local，得到所對應的 Currency	
    Currency curr = Currency.getInstance(mDefault);
	// 取出該 Currency 所對應的ISO 4217 currency code
	String code = curr.getCurrencyCode()
	```
