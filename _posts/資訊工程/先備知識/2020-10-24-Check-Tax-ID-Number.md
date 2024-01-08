---
title: ID驗證系列｜公司統一編號驗
date: 2023-12-20 00:07
is_modified: true
categories:
- 先備知識
tags:
- 編碼規則
- C/C++
- regexp
- 臺灣ID驗證系列
--- 

又是不務正業的一篇 XDDD    
不過統編的資料有點少，所有的資料看來都出自同一個地方。 

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/GPyBBgm.jpg?1" alt="公司統一編號">
    公司統一編號
</p>

<div class="alert warning">
<div class="head">公司統一編號驗證檢查碼邏輯修正說明</div>
1. 統一編號預估空號將於 2024 年用罄，故擴增統一編號。<br>
2. 為相容新舊統一編號，檢查邏輯由可被『10』整除改為可被『5』整除。<br>
   `Sum%10=0` -> `Sum%5=0` <br>
3. 預計 <b>2023 年 4 月</b>以後，逐步釋出新統一編號。<br>
4. 不過是說 2023 年才釋出的編號，我改這麼快幹嘛啦 XD<br>
</div>



## 編號規則
目前現行的統編是 **8 個數字**。在驗證時，會將其乘上相對應的權重分別得到每個位元乘積的十位與個位數的：


|Index|$n_0$|$n_1$|$n_2$|$n_3$|$n_4$|$n_5$|$n_6$|$n_7$|
|---|---|---|---|---|---|---|---|---|
|權重|1|2|1|2|1|2|4|1|
|乘積十位數|$t_0$|$t_1$|$t_2$|$t_3$|$t_4$|$t_5$|$t_6$|$t_7$|
|乘積個位數|$d_0$|$d_1$|$d_2$|$d_3$|$d_4$|$d_5$|$d_6$|$d_7$|

 
再將每個位元的乘積的十位與個位數兩兩相加，得到一新的 8 碼數字 $s_0s_1s_2s_3s_4s_5s_6s_7$：

$$
s_i = t_i + d_i，\text{ where }  0 \le i \le 7 \text{ and } 0 \le s_i < 10
$$
 
<br class="big">

最後新的 8 碼數字總和若為 **10 的倍數**，即為有效的驗證碼。

$$
Sum\%10=0，\text{ where }  Sum = \sum_{i=0}^{7}s_i
$$

不過在 **2023 年 4 月**逐步釋出新的統一編號後，若為相容新舊統一編號，則應檢查數字總和是否為 **5 的倍數**：
 
$$
Sum\%5=0，\text{ where }  Sum = \sum_{i=0}^{7}s_i
$$


### 例外：$n_6 = 7$
不過上述的計算過程中，會有一個例外情況，就是當 $n_6 = 7$ 時，當其乘上對應權重 $4$ 後，會得到：

$$
t_6 = 2 , d_6 = 8
$$

但，兩者相加後，其值會大於 10：

$$
s_6 = t_6 + d_6 = 2 + 8 = 10
$$

<br class="big">

當遇到這情況時，會採用一個折衷的方法，將 $10$ 的兩個位數分別當成：

$$
s_6 = 1, s_6' = 0
$$

<br class="big">

把這兩個數分別計算總和，得到 $Sum$ 與 $Sum'$ ：

$$
Sum = \sum_{i=0}^{7}s_i \text{ , }
Sum' = \sum_{i=0, i \not = 6}^{7}s_i + s_6'
$$

<br class="big">

若其中一個和為 **10 的倍數**，即為有效的驗證碼。

$$
Sum\%10=0 \text{ or } Sum'\%10=0
$$

一樣若在新版統編釋出後，為相容兩者檢查邏輯則改為：其中一個和為 **5 的倍數**

$$
Sum\%5=0 \text{ or } Sum'\%5=0
$$



## 程式碼
這次驗證規則有點瑣碎，regexp 只能用來驗證是否為數字與長度。
```
^\d{8}$
```

<br class="big">

至於其他的驗證只能靠程式了，regexp 派不上用場，對了這次是 C++。偷個懶直接把 checkNumber 拉成變數並設置為 5，就自己視需求要用 10 或 5。
```cpp
#include <regex>
#include <string>
#include <iostream>
using namespace std;

bool checkTaxId(string taxId);

int main(int argc, char *argv[]){
    int count = 0; 
    while(argv[++count]);

    if(count!=2){
        cout << "Give me an Tax-ID-Number!!!!";
        return 0;
    }

    string taxId = argv[1]; 
 
    bool verification = checkTaxId(taxId);	
    cout << boolalpha << verification << endl;
}

bool checkTaxId(string idStr){
    regex reg("^\\d{8}$");
    smatch m;
    ssub_match sm;
    
    if(!regex_match(idStr, m, reg)){
        cout << "Fail, 長度錯誤" << endl;
        return false;
    }

    int len = 8  ;
    int idArray[len];
    int weight[] = {1,2,1,2,1,2,4,1};

    int sum = 0;
    for (int i = 0; i < len; i++){
         // conver char to int 
        idArray[i] = idStr[i] - '0' ;
        int p = idArray[i] * weight[i];
        int s = p/10 + p%10;
        s = s==10?0:s;
        sum += s ;  
    }

    // when s[6]==7, after adding the two, its value will be equal to 10, in this case, 1 or 0 should be used to calculate the sum. But here 10 is used directly to calculate the sum, because if 0 and 10 are used to take the remainder, both are 0; if 1 is taken to take the remainder as 0, it can be reversed that the remainder should be 9.  
    int checkNumber = 5 ;
    bool isLegal = sum%checkNumber==0 || ((sum+1)%checkNumber==0 && idArray[6]==7);

    if(!isLegal){
        cout << "Fail, 不合法的統編驗證" << endl;   
    }

    return isLegal;
}
```



## 參考資料 
1. 林壽山 (2013-03-17)。[營利事業統一編號邏輯檢查方法](https://superlevin.ifengyuan.tw/%E7%87%9F%E5%88%A9%E4%BA%8B%E6%A5%AD%E7%B5%B1%E4%B8%80%E7%B7%A8%E8%99%9F%E9%82%8F%E8%BC%AF%E6%AA%A2%E6%9F%A5%E6%96%B9%E6%B3%95/) 。檢自 Levin's Blog-林壽山 (2020-10-23)。
2. hero (2018-11-07)。[營利事業統一編號驗證完全手冊(Javascript,Java,C#,PHP)](http://herolin.webhop.me/entry/is-valid-TW-company-ID/) 。檢自 Hero Think~用手摀住我的嘴 (2020-10-23)。
3. beethobear (2006-10-30)。[[商用軟體]統一編號檢查碼規則](http://phorum.study-area.org/index.php/topic,11397.html) 。檢自 酷!學園 (2020-10-23)。
4. 電子發票組 (2021-12-22)。[營利事業統一編號檢查碼邏輯修正說明](https://www.fia.gov.tw/singlehtml/3?cntId=c4d9cff38c8642ef8872774ee9987283) 。檢自 財政部財政資訊中心 (2022-05-01)。 



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-12-20</summary>
  <ul>
    <li>2023-12-20 更新：使計算過程與文件相符，但不影響判別是否為合法統編</li>
    <li>2022-05-01 更新：檢查碼邏輯修正說明</li>
    <li>2022-05-01 更新：檢查碼判別式錯誤修正</li>
    <li>2020-12-31 發布</li>
    <li>2020-10-24 完稿</li>
    <li>2020-10-23 起稿</li>
  </ul>
</details>