---
title: 【C 語言】使用 Regular Expressions 
date: 2021-03-05 20:15
is_modified: true
categories:
- "程式設計 › 程式語言與架構"
tags:
- C/C++
- regexp
--- 

[挖了個坑](/Check-Love-Code)給自己跳，為啥我會這想不開，要在 C 上寫 regexp？  
不過最大的坑應該是...我幹麻選 C 阿，都 4、5 年沒碰了。

<!--more-->
<br>

## regex.h 函式庫

用 C 來寫正規表示式第一個碰到的問題是，<mark>C 的標準函式庫中並沒有支援正規表示式</mark>。


不過還好我的開發環境是 Linux，在現行 Linux 中大多有安裝 POSIX.2，所以可以直接引入 `<regex.h>` 來使用，但如果是 Windows 就得[自食其力](https://stackoverflow.com/questions/8230905/regex-h-for-windows)了。
 
看了下它的[原始碼](https://code.woboq.org/linux/include/regex.h.html)還挺簡單的，抽掉細節跟註解，比較常用的也就 4 個函式與一些常數（根據 [felix021 的網誌](https://www.felix021.com/blog/read.php?2099)說明，整份文件有 2 個類別、4 個函式和 7 個常數）。

在使用 regex.h 進行開發時，一般會有三個步驟：**編譯**、 **匹配**、 **釋放**，三個步驟分別對應到四個常用函式中的其中三個：`regcomp()`、 `regexec()`、 `regfree()`，剩下一個是錯誤處理 `regerror()`。


<br><br>

## 編譯：regcomp()

這個是把指定的表示式 `pattern` 編譯成特定的資料格式 `regex_t` ，在下一個階段會使用編譯後的結果 `preg` 進行匹配。

```cpp
int regcomp(regex_t *preg, const char *pattern, int cflags)
```

這個函式有三的參數，分別是：
1. **preg**： 它的資料格式是 regex_t，用來存放編譯後的結果，所以傳進去的是指標。
2. **pattern**：這邊傳進我們的表示式，也是傳指標。除此之外，它是個常數。
3. **cflags**： 這部份是一些參數的設定，可傳入一個或多個（用｜串接）值，可使用的值有：
    1. **REG_EXTENDED**：使用 ERE（Extended Regular Expressions，擴展型正規表示式）模式進行匹配。
    2. **REG_ICASE**：忽略字母大小寫。        
    3. **REG_NOSUB**：僅回報匹配成功或失敗。        
    4. **REG_NEWLINE**： 識別換行符號。  
<br>
    思考了下，我應該會開 **ERE**，因為我會的**流派**應該算是 <mark>PCRE （Perl Compatible Regular Expressions）</mark>一派，跟 BRE（Basic Regular Expression，基本型正規表示式）[差異](http://www.greenend.org.uk/rjk/tech/regexp.html) 看起來頗多，相較之下 ERE 還稍微相近一點，不過還是有些差異。 
 
<br>

回傳值的部份，編譯成功會回傳 0，否則傳回其他值。來個片段看一下：
```cpp
// 比對電子信箱
regex_t preg; // 宣告編譯結果變數
const char* pattern = "^[a-z0-9_]+@([a-z0-9-]+\\.)+[a-z0-9]+$"; // 定義表示式
// 編譯，這邊使用 ERE，且不考慮大小寫
int success = regcomp(&preg, pattern, REG_EXTENDED|REG_ICASE);
assert(success==0);
```

是說 POSIX 體系，<mark>沒有</mark> `\d` 跟 `\w` 可用，只能寫成 `[a-z0-9]` 。
 
 
<br><br>

## 匹配：regexec()

編譯好後就可以把目標字串放進去匹配了。

```cpp
int regexec (regex_t *preg, char *target, size_t nmatch, 
             regmatch_t matchptr [], int eflags)
```
<br>

先提一下 `regmatch_t` 這個資料格式，在程式碼中它是長這樣：
```cpp
typedef struct{
    regoff_t rm_so;  /* Byte offset from string's start to substring's start.  */
    regoff_t rm_eo;  /* Byte offset from string's start to substring's end.  */
} regmatch_t;
```
其中的 `rm_so` 是用來記錄匹配結果在字串中的起始位置，`rm_eo` 是結束位置。一般會將它宣告為陣列，index 為 0 存放 full match，之後存放 group，陣列長度會決定記錄多少的 group。 

<br>


我們來看下要傳啥參數進去，
1. **preg**：這就是在上一個階段使用 `regcomp` 編譯完的 `preg`，直接丟進來就好。
2. **target**：我們的目標字串。
3. **nmatch** 與 **matchptr**：`matchptr` 就是上面所提過 `regmatch_t` 陣列。`nmatch` 則是 `matchptr` 的長度。
4. **eflags**：有兩個值 **REG_NOTBOL**、**REG_NOTEOL**。

<br>

繼續我們的 Code：
```cpp
char* target = "testmail_10@gmail.com";   //目標字串
regmatch_t matchptr[1];   // 記錄匹配結果陣列，長度為1僅記錄 full match
const size_t nmatch = 1;    //  matchptr陣列長度
int status = regexec(&preg, target, nmatch, matchptr, 0); //匹配
if (status == REG_NOMATCH){ // 沒匹配
    printf("No Match\n");
}
else if (status == 0){  // 匹配
    printf("Match\n");
    // 取出起始與結束位置印出字串
    for (int i = matchptr[0].rm_so; i < matchptr[0].rm_eo; i++){  
        printf("%c", target[i]);
    }
    printf("\n");
}
```

 
<br><br>

## 釋放：regfree()

把空間釋放放掉。

```cpp
void regfree (regex_t *preg)
```

在使用完畢或要重編譯新的表示式前，調用這個清空 `preg` 指向的 `regex_t` 的内容。

```cpp
regfree(&reg);
```

<br><br>

## 錯誤處理： regerror()
當執行 `regcomp` 或 `regexec` 產生錯誤的時候，就可以調用這個函數返回錯誤訊息的字串。

```cpp
size_t regerror(int errcode, const regex_t *preg,
    char *errbuf, size_t errbuf_size);
```

參數的部分：
1. **errcode**：就是剛剛 `regcomp` 跟 `regexec` 回傳的狀態碼，直接丟進去就好。
2. **preg**：就是剛剛編譯好的正規表示式。
3. **errbuf**：一個陣列，拿來存放錯誤訊息用的。
4. **errbuf_size**：指 `errbuf` 的長度。 


```cpp
char msgbuf[256];
regerror(status, &preg, msgbuf, sizeof(msgbuf)); 
printf("error: %s\n", msgbuf);
```

<br><br>

## 程式範例

把上面的程式碼，整合起來：

```cpp
#include <stdio.h>
#include <stdbool.h>
#include <regex.h>  
#include <assert.h>

int main(){
    regex_t preg; // 宣告編譯結果變數
    const char* pattern = "^[a-z0-9_]+@([a-z0-9-]+\\.)+[a-z0-9]+$"; // 定義表示式
    int success = regcomp(&preg, pattern, REG_EXTENDED|REG_ICASE); // 編譯，這邊使用 ERE，且不考慮大小寫
    assert(success==0);

    char* target = "testmail_10@gmail.com";   //目標字串
    regmatch_t matchptr[1];   // 記錄匹配結果陣列，長度為1僅記錄 full match
    const size_t nmatch = 1;    //  matchptr陣列長度
    int status = regexec(&preg, target, nmatch, matchptr, 0); //匹配
    if (status == REG_NOMATCH){ // 沒匹配
        printf("No Match\n");
    }
    else if (status == 0){  // 匹配
        printf("Match\n");
        // 取出起始與結束位置印出字串
        for (int i = matchptr[0].rm_so; i < matchptr[0].rm_eo; i++){  
            printf("%c", target[i]);
        }
        printf("\n");
    }
    else {  // 執行錯誤
        char msgbuf[256];
        regerror(status, &preg, msgbuf, sizeof(msgbuf)); 
        printf("error: %s\n", msgbuf);
    }

    regfree(&preg);  // 釋放
    return 0;
}
```


<br><br> 

## 參考資料 
1. 陈止风 (2017-02-06)。[c regex 用法](https://blog.csdn.net/yingzinanfei/article/details/54893394) 。檢自 陈止风的博客｜CSDN博客 (2020-08-28)。
2. felix021 (2012-11-25)。[使用Linux/Unix/BSD的regex库](https://www.felix021.com/blog/read.php?2099) 。檢自 Felix021 (2020-08-28)。
3. piedog (2016-06-22)。[When using regex in C, \d does not work but [0-9] does](https://stackoverflow.com/questions/37956719/when-using-regex-in-c-d-does-not-work-but-0-9-does) 。檢自 StackOverflow (2020-08-28)。
4. Richard Kettlewell 。[Regexp Syntax Summary](http://www.greenend.org.uk/rjk/tech/regexp.html) 。檢自 RJK (2020-08-28)。
5. elbort (2012-09-04)。[c语言 正则表达式可编译c文件](https://blog.csdn.net/elbort/article/details/7943317) 。檢自 elbort的专栏 ｜ CSDN博客 (2020-08-28)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-09-24</summary>
  <ul>  
    <li>2021-03-05 錯誤修正</li>
    <li>2020-09-24 發布</li>
    <li>2020-09-09 完稿</li>
    <li>2020-08-28 起稿</li>
  </ul>
</details>