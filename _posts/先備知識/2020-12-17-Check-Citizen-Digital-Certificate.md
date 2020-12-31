---
title: 【臺灣ID驗證系列】自然人憑證編號驗證
date: 2020-12-31 23:16
is_modified: false
categories:
- 先備知識
tags:
- Golang
- 編碼規則
- regexp
- 臺灣ID驗證系列
--- 

操作手冊寫累了，決定換換口味寫點輕鬆的。  
這次來寫寫自然人憑證編號驗證。

<!--more-->
<center> <img src="https://i.imgur.com/Vbkap1k.png?1" alt="自然人憑證"></center>
<center class="imgtext">自然人憑證（圖片來源: <a href="https://www.hshalu.taichung.gov.tw/1898437/post" class="imgtext">臺中市沙鹿區戶政事務所</a>）</center>
<br>

## 編號規則
自然人憑證編號的規則頗簡單：
- 總長度為 16 碼字元
- 前 2 碼為大寫英文
- 後 14 碼為數字


<br><br>

## 程式碼
如果用 Regular Expression 來實做還挺簡單的： 
```bash
^[A-Z]{2}[0-9]{14}$
```

<br>

寫完了 C/C++，這次換成 Golang。

```go
package main

import (
    "fmt"
    "os"
    "regexp"  
)

func checkCitizenDigitalCertificate(digital string) bool {
    r, _ := regexp.Compile("^[A-Z]{2}[0-9]{14}$") 
    return r.MatchString(digital)
}

func main () {
    digital :=  os.Args[1]
    is_verify := checkCitizenDigitalCertificate(digital)
    fmt.Println(is_verify)
}
```

加點錯誤訊息：

```go
package main

import (
    "fmt"
    "os"
)

func checkCitizenDigitalCertificate(digital string) bool {

    if len(digital) != 16 {
        fmt.Println("Fail，長度不正確");
        return false
    }

    letter := digital[:2]
    number := digital[2:]
    
    for _, l := range letter {
        if l < 64 || l > 91{
            fmt.Println("Fail，字首非為大寫字母");
            return false
        } 
    }

    for _, l := range number {
        if l < 48 || l > 57{
            fmt.Println("Fail，尾14碼非數字");
            return false
        } 
    }

    return true
}


func main () {
    digital :=  os.Args[1]
    is_verify := checkCitizenDigitalCertificate(digital)
    fmt.Println(is_verify)
}

$> go build main.go
$> ./main AB12345678901234
```

<br><br> 

## 參考資料 
1. (2017-12-05)。[手機條碼、自然人憑證條碼與捐贈碼檢核規則](https://www.cetustek.com.tw/news.php?id=186) 。檢自 鯨躍科技有限公司官方網站 (2020-12-17)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-12-31</summary>
  <ul class="timestamp">
    　<li>2020-12-31 發布</li>
    　<li>2020-12-17 完稿</li>
    　<li>2020-12-17 起稿</li>
  </ul>
</details>