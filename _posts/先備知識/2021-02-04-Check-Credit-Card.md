---
title: 【臺灣ID驗證系列】信用卡卡號驗證
date: 2021-03-07 22:59
is_modified: false
categories:
- 先備知識
tags:
- Javascript
- 編碼規則
- 臺灣ID驗證系列
--- 

趁著演講者在講些五四三的時候，來補我的不務正業系列，這次來看看信用卡卡號驗證。
<!--more-->

<center> <img src="https://i.imgur.com/60gBFeq.jpg?1" alt="信用卡"></center>
<center class="imgtext">信用卡（圖片來源: <a href="https://www.shutterstock.com/zh-Hant/discover/image-footage-music?kw=shutterstock&c3apidt=p15867520243&gclid=Cj0KCQiA0-6ABhDMARIsAFVdQv-8MA9Y7Q9L9uCanv_OyNtEbjV-ydbXQfXYXYP0NkXR6z705PN5MAkaAgBNEALw_wcB&gclsrc=aw.ds" class="imgtext">shutterstock</a>）</center>
<br>

## 編號規則
信用卡的編碼規則，相較之下稍微稍微複雜點。根據 [ISO/IEC 7812 標準](https://zh.wikipedia.org/wiki/ISO/IEC_7812)，信用卡一般在 **13~19** 碼，但 Mastercard 旗下似乎有 **12** 碼簽帳金融卡（Debit Card）。而在臺灣比較常見的長度是 <mark>16 碼</mark>，因此下面在說明多以 16 碼來說明，但不管幾碼編碼規則其實都是相同的。

<br>

長達 16 碼的信用卡卡號，其實是由 3 部分所組成的，以一張卡號為 `4311-4656-0640-6131` 的 Visa 信用卡為例：

<table>
    <tbody>
    <tr>
      <th colspan="6">發卡機構代碼</th>
      <th colspan="9">個人帳戶號碼</th>   
      <th>檢核碼</th>
    </tr>
    <tr>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>6</td>
      <td>5</td>
      <td>6</td>
      <td>0</td>
      <td>6</td>
      <td>4</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
    </tr>
    </tbody>
</table>
<br>

- **發卡機構代碼（Issuer Identification Numbers，IIN）**  
    IIN 有時被稱為銀行識別號碼（Bank Identification Number，BIN），這是用來辨別卡片的主要資訊，由 [行業標識碼（Major Industry Identifier，MII）](https://zh.wikipedia.org/wiki/ISO/IEC_7812#%E8%A1%8C%E4%B8%9A%E6%A0%87%E8%AF%86%E7%AC%A6)為首所組成的 **6 碼**數字。 

    因此可以從左起第 1 碼數字能看出發卡組織的端倪。一般來說，5 開頭是萬事達卡、4 是 Visa為、3 是美國運通與 JCB、62 是中國銀聯。不過這些編碼似乎會因為商業聯盟與組織營運，如果要查確切的對應，可能得去查查[美國國家標準協會（American National Standards Institute，ANSI）](https://www.ansi.org/) 的 IIN 資料庫。
   
  
- **帳戶號碼（Primary Account Number, PAN）**  
    中間位數由發卡單位自定義，一般由 **6~12** 碼數字所組成。其意義並沒有硬性規定，可能會包含分行之類的訊息，但也有可能只是流水號。
    
- **檢核碼（Checking Number）**  
    顧名思義就是用檢查這組信用卡的卡號真偽的一個數字，由 [Luhn 演算法](https://zh.wikipedia.org/wiki/Luhn%E7%AE%97%E6%B3%95)計算所得出。


<br>

在找資料的時候發現，有些文章的檢查規則是照著 Luhn 演算法的生成規則一步步計算的，但我想檢查不是生成，有些動作可以適度的化簡：

1. **設定權重**  
    由右到左為每個數字賦予一個權重，其中奇數位的權重是 1、偶數位的權重是 2：

    | idx | $n_{15}$ | $n_{14}$ | $n_{13}$ | $n_{12}$ | $n_{11}$ | $n_{10}$ | $n_9$ | $n_8$ | $n_7$ | $n_6$ | $n_5$ | $n_4$ | $n_3$ | $n_2$ | $n_1$ | $n_0$ |
    | ----- | -------- | -------- | -------- | -------- | -------- | -------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
    | 卡號  | 4        | 3        | 1        | 1        | 4        | 6        | 5     | 6     | 0     | 6     | 4     | 0     | 6     | 1     | 3     | 1     |
    | 權重  | 2        | 1        | 2        | 1        | 2        | 1        | 2     | 1     | 2     | 1     | 2     | 1     | 2     | 1     | 2     | 1     |

2. **乘積計算**  
    將卡號每碼與其對應的權重一一相乘，若乘積大於 10 則取兩位數之和。
    
    
    | idx | $m_{15}$ | $m_{14}$ | $m_{13}$ | $m_{12}$ | $m_{11}$ | $m_{10}$ | $m_9$ | $m_8$ | $m_7$ | $m_6$ | $m_5$ | $m_4$ | $m_3$ | $m_2$ | $m_1$ | $m_0$ |
    | ----- | -------- | -------- | -------- | -------- | -------- | -------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
    | 乘積  | 8        | 3        | 2        | 1        | 8        | 6        | 10     | 6     | 0     | 6     | 8    | 0     | 12     | 1     | 6     | 1     |
    | 取和  | 8        | 3        | 2        | 1        | 8        | 6        | **1**     | 6     | 0     | 6     | 8     | 0     | **3**     | 1     | 6    | 1     |


3. **10的倍數**      
    將取和完成的數值進行加總，若總和為 **10 的倍數**，則為有效卡號：  
    
    $$
    (m_{0}+m_{1}+m_{2}+m_{3}+m_{4}+m_{5}+m_{6}+m_{7}+m_{8}+m_{9}+m_{10}+m_{11}+m_{12}+m_{13}+m_{14}+m_{15})\%10 = 0
    $$ 
    
    將上面的例子套回計算式後：
    
    $$
    \begin{aligned}	
    &(1+6+1+3+0+8+6+0+6+1+6+8+1+2+3+8)\%10 \\
    &= 60\%10\\
    &= 0
    \end{aligned}
    $$ 
    
    餘數為 0，表為有效卡號。
 
    
    

<br><br> 
 
## 程式碼

```javascript
function verifyId(id) {
    id =  id.replace(/-/g,"").trim();
    let len = id.length;

    if(len<12 || len >19){
        return false;
    }

    let revId = id.split("").reverse();
     let checkSum = revId.reduce(function(prev, curr, idx){
         curr  = parseInt(curr);
         let w = (idx+1) % 2 == 0 ? 2: 1;
         let res =  curr * w 
         if(res>=10){
             res = parseInt(res/10) + res%10 ;
         }
         return prev + res;
     }, 0);
    return checkSum % 10 == 0
}

console.log(verifyId("4311-4656-0640-6131"));
```

這組程式碼能加 log 的地方也只有長度檢查的部份，所以就不再寫一版有 log 的了。是說原本想寫 Clojure，不過 Clojure 還不是很熟練，邊寫還要邊查語法，有點太花時間了，改天再來補 Clojure 的程式碼好了。

<br><br> 

## 參考資料 
1. 協同撰寫。[發卡行識別碼](https://zh.wikipedia.org/wiki/%E5%8F%91%E5%8D%A1%E8%A1%8C%E8%AF%86%E5%88%AB%E7%A0%81)。檢自 維基百科 (2021-02-04)。
2. 協同撰寫。[ISO/IEC 7812](https://zh.wikipedia.org/wiki/ISO/IEC_7812)。檢自 維基百科 (2021-02-04)。
3. Claire (2019-01-10)。[卡號與安全機制篇](https://ipromise.com.tw/blog/post/creditcard-bin-number)。檢自 愛承諾金融網 (2021-02-04)。
4. 協同撰寫。[Luhn算法](https://zh.wikipedia.org/wiki/Luhn%E7%AE%97%E6%B3%95)。檢自 維基百科 (2021-02-04)。
5. Roger Jang 。[10-4 常用資料規則](http://mirlab.org/jang/books/javascript/dataRule.asp?title=10-4%20%B1%60%A5%CE%B8%EA%AE%C6%B3W%ABh)。檢自 JavaScript 程式設計與應用：用於網頁用戶端 (2021-02-04)。 
6. 支付圈 (2017-05-02)。[銀行卡號編碼規則](https://kknews.cc/finance/85jp284.html)。檢自 每日頭條 (2021-02-04)。
7. 溫子豪 (2016-08-22)。[解開信用卡卡號秘密　檢查碼輕鬆算防偽卡](https://www.cardu.com.tw/news/detail.php?nt_pk=4&ns_pk=30424)。檢自 卡優新聞網 (2021-02-04)。

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-03-07</summary>
  <ul>
    <li>2021-03-07 發布</li>
    <li>2021-02-04 完稿</li>
    <li>2021-02-04 起稿</li>
  </ul>
</details>