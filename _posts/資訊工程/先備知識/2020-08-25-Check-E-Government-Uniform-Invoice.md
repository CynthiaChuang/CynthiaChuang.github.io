---
title: 電子發票手機條碼驗證
date: 2020-09-24 23:13
is_modified: false
categories:
- 先備知識
tags:
- 編碼規則
- Jave
- regexp
- 臺灣ID驗證系列
--- 

前一陣子寫了篇[身分證字號驗證](/CheckUID)後，結果同學分享了個有趣[專案](https://github.com/enylin/taiwan-id-validator)，包含了許多驗證項目。看起來還頗為有趣，決定試著了解一下。    
先挑常用的**電子發票條碼**練身手。

<!--more-->
<center> <img src="https://i.imgur.com/dN9tdi6.jpg" alt="電子發票手機條碼"></center>
<center class="imgtext">電子發票手機條碼（圖片來源: <a href="https://einvoice.nat.gov.tw/APCONSUMER/BTC500W/" class="imgtext">電子發票整合服務平台</a>）</center>
<br><br> 

## 編號規則

目前現行的電子發票共通性載具一共有 **8 碼**，包括第一個字元的 `/` 與 7 個英數字符號，其中合法的英數字符號包含：
- 0-9
- A-Z
- +/-/.

<div class="alert info">
<div class="head">一維條碼編碼規則</div>
如果想把這組數字換成一維條碼，則是走 <a href="http://www.appsbarcode.com/Code%2039.php">Code39</a> 的編碼規則。
</div>


<br><br> 

## 程式碼
規則出乎意料的簡單，一條 Regular Expression 就可以解決了。
```
^/[\dA-Z0-9+-\.]{7}$
```

<br> 

這次來寫 Java 好了，好久沒寫了，自從不寫 App 後都沒動過了。

```java
class Main {
    public static void main(String[] args) {
        int len_args = args.length ; 
        if (len_args <= 0){
            System.out.println("Give me an EGUI(Electronic Government Uniform Invoice)!!!!");
            return;
        }

        String egui = args[0]; 
        boolean verification = checkEgui(egui);	
        System.out.println(verification); 
    }
    
    public static boolean checkEgui(String egui){
        return egui.matches("^/[\\dA-Z0-9+-\\.]{7}$");
    }
}

java Main /ABC+123
```

<br>

加點錯誤訊息：
```java
public static boolean verifyEgui(String egui){
    if(egui.length() != 8){
        System.out.println("Fail, 長度錯誤"); 
        return false;

    } else if(egui.charAt(0) != '/'){
        System.out.println("Fail, 必須 / 開頭"); 
        return false;
    } 

    for(int i=1; i< egui.length(); i++){
        int charactercode = Character.codePointAt(egui, i); 
        boolean isSymbol = (charactercode == 43 || charactercode == 45 || charactercode == 46);
        boolean isNumber = (charactercode>=48 && charactercode<=57);
        boolean isLetter = (charactercode>=65 && charactercode<=90);
        
        if (!(isSymbol || isNumber || isLetter)) {
            System.out.println("Fail, 包含不合法字元"); 
            return false;
        } 
    }
    return true;
} 
```

<br>

阿阿阿，太久沒有寫 Jave，不是忘了寫分號，不然就是忘了宣告，動態程式語言寫太多的後遺症 XDDD

我看以後這系列就拿來複習其他語言的語法好了，不然除了 Python 其他語言我都快忘光了，下次來寫 C 跟 C++ 好了。


<br><br> 

## 參考資料 
1. [什麼是手機條碼？如何申請電子發票手機條碼？](https://www.powerweb.tw/modules/qna/V121.html) 。檢自 POWERWEB 虛擬主機網頁空間 (2020-08-24)。

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-09-24</summary>
  <ul>
    <li>2020-09-24 發布</li>
    <li>2020-08-25 完稿</li>
    <li>2020-08-24 起稿</li>
  </ul>
</details>