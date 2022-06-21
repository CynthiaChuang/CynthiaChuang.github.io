---
title: ID驗證系列｜電子發票捐贈碼驗證
date: 2020-09-24 23:25
is_modified: false
categories:
- 先備知識
tags:
- 編碼規則
- C/C++
- regexp
- 臺灣ID驗證系列
--- 

ID 驗證系列第三彈！     
處理完[電子發票條碼](/Check-E-Government-Uniform-Invoice)後，順便來處理捐贈碼。果然壓力大的時候來做雜事最抒壓了 XDDD 

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/Fl4uKtL.png?1" alt="捐贈碼">
    捐贈碼（圖片來源: <a href="https://www.einvoice.nat.gov.tw/ein_upload/html/ESQ/ESQ601W.htmll">課電子發票整合服務平台</a>）
</p>



## 編號規則
捐贈碼的規則頗簡單：
- 總長度介於 3 至 7 碼
- 由純數字所組成



## 程式碼
用 Regular Expression 
```
^\d{3,7}$
```

<br> 

上次說要用 C 寫，可能得先去複習一下語法，好久沒寫 C 了，而且好像沒有用 C 寫過 regex？都可以再[寫一篇網誌](/Regular-Expressions-in-C)了...

```cpp
#include <stdio.h>
#include <stdbool.h>
#include <regex.h>  
#include <assert.h>


bool check_love_code(char* code){
    regex_t preg; 
    const char* pattern = "^[0-9]{3,7}$";
    int success = regcomp(&preg, pattern, REG_EXTENDED|REG_ICASE); 
    assert(success==0);

    regmatch_t matchptr[1];
    const size_t nmatch = 1;   
    int status = regexec(&preg, code, nmatch, matchptr, 0); 
    
    bool result = false;
    if (status == 0){
        result = true;
    }

    regfree(&preg);    
    return result;
}

int main(){
    char* code = "1234"; 
    bool is_verify = check_love_code(code);
    printf(is_verify ? "true" : "false");
    return 0;
}
```

<br>

加點錯誤訊息：
```cpp
#include <stdio.h>
#include <stdbool.h>
#include <string.h>

bool check_love_code(char* code){ 
    size_t length = strlen(code);
    if(length<3 || length>7){
       printf("Fail, 長度錯誤\n");
       return false;
    }

 
    for (size_t i = 0; i < length; i++) {
      int ascii = (int) code[i];
      bool is_number = (ascii>=48 && ascii<=57);
      if(!is_number){
        printf("Fail, 非數字\n");
        return false;
      }
    }
  
    return true;
}

int main(){
    char* code = "987A"; 
    bool is_verify = check_love_code(code);
    printf(is_verify ? "true" : "false");
    printf("\n");
    return 0;
}
```

<br>
 
難得兩個長度差不多 XDDD，通常 regex 會簡潔很多。



## 參考資料 
1. (2017-12-05)。[手機條碼、自然人憑證條碼與捐贈碼檢核規則](https://www.cetustek.com.tw/news.php?id=186) 。檢自 鯨躍科技有限公司官方網站 (2020-08-28)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-09-24</summary>
  <ul>
    <li>2020-09-24 發布</li>
    <li>2020-09-11 完稿</li>
    <li>2020-08-28 起稿</li>
  </ul>
</details>