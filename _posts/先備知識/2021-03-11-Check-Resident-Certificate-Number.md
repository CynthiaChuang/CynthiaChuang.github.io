---
title: 【臺灣ID驗證系列】居留證驗證號
date: 2021-09-02 22:12
is_modified: false
categories:
- 先備知識
tags:
- Javascript
- 編碼規則
- regexp
- 臺灣ID驗證系列
--- 

這是[這個系列](/tag/#/臺灣ID驗證系列)中的最後一篇，最後就來看看居留證的驗證。

<!--more-->

<center> <img src="https://i.imgur.com/rqlFFMq.png" alt="居留證樣本"></center>
<center class="imgtext">居留證樣本（圖片來源: <a href="https://zh.wikipedia.org/wiki/%E4%B8%AD%E8%8F%AF%E6%B0%91%E5%9C%8B%E5%B1%85%E7%95%99%E8%AD%89#%E5%A4%96%E4%BE%86%E4%BA%BA%E5%8F%A3%E7%B5%B1%E4%B8%80%E8%AD%89%E8%99%9F%E7%B7%A8%E7%A2%BC%E5%8E%9F%E5%89%87" class="imgtext">維基百科</a>）</center>

<br>

## 編碼規則

居留證編碼比較麻煩是目前有<mark>兩版</mark>的規則。在今年 2021 年的 1 月 2 日開始換發，新版「外來人口統一證號」會比照國民身分證字號編碼改版，不過為了降低能量針對藍領居留之移工是採屆期換發，因此未來可能有段時間會是兩套編碼規則並行的。

<center> <img src="https://i.imgur.com/1Moj5yd.jpg?1" alt="新版「外來人口統一證號」比照國民身分證字號編碼改版"></center>
<center class="imgtext">新版「外來人口統一證號」比照國民身分證字號編碼改版（圖片來源: <a href="https://www.cna.com.tw/news/ahel/202101020034.aspx" class="imgtext">中央社 CNA</a>）</center>

<br>

因此，我們也分成兩部份來討論：

### 舊版規則

編碼的方式與身分證字號類似，居留證編碼一樣有 **10 碼**，一樣可以將其分成四區：**區域碼**、**性別代碼**、**流水碼** 跟 **檢核碼**。

<table>
    <tbody>
    <tr>
      <th>1</th>
      <th>2</th>  
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
    </tr>
    <tr>
      <th>區域碼</th>
      <th>性別碼</th>  
      <th colspan="7">流水碼</th>
      <th>檢核碼</th>
    </tr>
    <tr>
        <td>A-Z</td>
        <td>無戶籍國民與中港澳地區人民： 男：A / 女：B<br>
            外國人： 男：C / 女：D </td>    
        <td colspan="7">阿拉伯數字 </td>
        <td>阿拉伯數字</td>
    </tr>
    </tbody>
</table>


身分證字號與居留證編碼，兩種編碼最大的差異在於第二碼的**性別代碼**。與身份證用 1、2 分別表示男女性不同，舊版居留證的性別碼依照人民來源與性別來賦予一個<mark>英文代號</mark>。

<br>

整體來說，一個完整的居留證編碼如下：

<table>
    <tbody>
    <tr>
      <th>區域碼</th>
      <th>性別碼</th>  
      <th colspan="7">流水碼</th>
      <th>檢核碼</th>
    </tr>
    <tr>
      <td>F </td>
      <td>A </td>
      <td>1 </td>
      <td>2 </td>
      <td>3 </td>
      <td>4 </td>
      <td>5 </td>
      <td>6 </td>
      <td>8 </td>
      <td>9 </td>
    </tr>
    </tbody>
</table>

<br>



在進行編碼檢查時，一樣需將首碼的區域碼換成相對應的數值，其轉換表格與身分證相同，如 F 就會被轉換成 `15`。


|Ａ|Ｂ|Ｃ|Ｄ|Ｅ|Ｆ|Ｇ|Ｈ|Ｉ|Ｊ|
|---|---|---|---|---|---|---|---|---|---|
|10|11|12|13|14|15|16|17|34|18|

|Ｋ|Ｌ|Ｍ|Ｎ|Ｏ|Ｐ|Ｑ|Ｒ|Ｓ|Ｔ|
|---|---|---|---|---|---|---|---|---|---|
|19|20|21|22|35|23|24|25|26|27|

|Ｕ|Ｖ|Ｗ|Ｘ|Ｙ|Ｚ|
|---|---|---|---|---|---|
|28|29|32|30|31|33|


而次碼的性別碼，也是需要依照上表將其轉成二位數字後，只取其**個位數**，如 A 就會被轉換成 `0`。

<br>

將轉換完成的數值，乘上相對應的權重後進行加總：  

|Index|$n_0$|$n_1$|$n_2$|$n_3$|$n_4$|$n_5$|$n_6$|$n_7$|$n_8$|$n_9$|$n_{10}$|
|---|---|---|---|---|---|---|---|---|---|---|---|
|權重|1|9|8|7|6|5|4|3|2|1|1|

若總和為 **10 的倍數**，即為有效的驗證碼。

<br>

詳細的計算步驟跟[身分證字號的驗證](/CheckUID)方式相同，這邊就不再算一次了。


<br>


### 新版規則

舊版的規則雖然與身分證字號類似，但格式還是有差異，導致在進行網路購物、訂票、醫療掛號時，會遇到證號格式不同帶來的困擾。為解決這困擾，相關機構決定統一格式，將新版居留證號比照身分證字號編碼。


<table>
    <tbody>
    <tr>
      <th>1</th>
      <th>2</th>  
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>10</th>
    </tr>
    <tr>
      <th>區域碼</th>
      <th>性別碼</th>    
      <th>身分碼</th>     
      <th colspan="6">流水碼 </th>
      <th>檢核碼</th>
    </tr>
    <tr>
        <td>A-Z</td>
        <td>男：8<br>
            女：9</td>    
        <td>外國人與無國籍人士：0-6<br>
            無戶籍國民：7<br>
            港澳居民：8<br>
            大陸地區人民：9
            </td>  
        <td colspan="6">阿拉伯數字 </td>
        <td>阿拉伯數字</td>
    </tr>
    </tbody>
</table>

<br>
 
除用 8、9 分別表示男女外，其他的轉換方式跟[身分證字號的驗證](/CheckUID)方式相同。

<br><br> 
 
## 程式碼

新版的驗證規則跟身分證字號的驗證一模一樣，所以我就不寫了，只寫舊版的規則。

```javascript
function verifyId(id) {
    id = id.trim();
    
    <!-- 在 js 中遇到反斜線要跳脫，所以這邊用兩個反斜線 -->
    <!-- 如果你看到四個反斜線，那是我為了讓 NexT.Mist 主題順利渲染所再做跳脫 -->
    verification = id.match("^[A-Z][ABCD]\\\\d{8}$")
	if(!verification){
		return false
	}

    let conver = "ABCDEFGHJKLMNPQRSTUVXYWZIO"
    let weights = [1, 9, 8, 7, 6, 5, 4, 3, 2, 1, 1]

    id = String(conver.indexOf(id[0]) + 10) +  String((conver.indexOf(id[1]) + 10)%10) + id.slice(2);

    checkSum = 0
    for (let i = 0; i < id.length; i++) {
        c = parseInt(id[i])
        w = weights[i]
        checkSum += c * w
    }
	
    return checkSum % 10 == 0
}

console.log(verifyId("FA12345689"));
```

 
<br><br> 

## 參考資料 

1. 梁珮綺 (2021-01-03)。[新版外來人口統一證號2日起換發 外國人生活更便利](https://www.cna.com.tw/news/ahel/202101020034.aspx)。檢自 中央社 CNA (2021-03-11)。
2. 協同撰寫。[外來人口統一證號編碼原則](https://zh.wikipedia.org/wiki/%E4%B8%AD%E8%8F%AF%E6%B0%91%E5%9C%8B%E5%B1%85%E7%95%99%E8%AD%89#%E5%A4%96%E4%BE%86%E4%BA%BA%E5%8F%A3%E7%B5%B1%E4%B8%80%E8%AD%89%E8%99%9F%E7%B7%A8%E7%A2%BC%E5%8E%9F%E5%89%87)。檢自 維基百科 (2021-03-11)。
3. gomumu (2006-11-29)。[[知識+]外籍人士用的統一證號編碼規則](https://gomumu.pixnet.net/blog/post/3128951-%5B%E7%9F%A5%E8%AD%98%2B%5D%E5%A4%96%E7%B1%8D%E4%BA%BA%E5%A3%AB%E7%94%A8%E7%9A%84%E7%B5%B1%E4%B8%80%E8%AD%89%E8%99%9F%E7%B7%A8%E7%A2%BC%E8%A6%8F%E5%89%87)。檢自 痞客邦(2021-03-10)。 
4. 內政部 (2019-10)。[「外來人口統一證號格式專案」修正計畫（核定本）](http://www.academic.fcu.edu.tw/wSite/public/Attachment/f1582594331972.pdf)。檢自 內政部 (2021-03-10)。

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-09-02</summary>
  <ul>
    <li>2021-09-02 發布</li>
    <li>2021-03-11 完稿</li>
    <li>2021-03-10 起稿</li>
  </ul>
</details>