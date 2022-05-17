---
title: 【Python】讀取與寫入 xlsx 檔案
date: 2020-12-21 22:08
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- Python
--- 

那天，我媽的同事傳了份檢驗結果文件給她，但格式超級奇怪，既不是人工閱讀的文件格式、也不是資料系統的處理格式。我媽想把它轉成資料系統格式，可偏偏 Excel 的轉置、公式什麼的都沒辦法處理，只能人工處理。粗估下來，一份小資料大概 2 小時跑不掉。
 
我看了一下資料內容，是可以抓到一些規律的，因此打算寫程式幫她轉了，如果一切順利 15 分鐘就可以解決了。漏氣的是，第一關讀檔就卡住，我之前好像還沒有讀過 Excel 檔的文件。

<!--more-->
<br>

## 一般讀檔案

原本想說它跟 CSV 檔案類似，可以用一般讀檔的方式讀進來，結果一開始就吃鱉了。


```python
with open("原料年檢資料.xlsx", "r", encoding="utf-8") as fr:
   for line in fr :
      print(line)
```

立刻給了我個編碼問題，看來不能偷吃步了，只好來研究怎麼讀 Excle 檔了。

```bash
UnicodeDecodeError: 'utf-8' codec can't decode byte 0x90 in position 16: invalid start byte
```


<br><br> 

## Pandas 讀檔

拜了一下大神，發現有 openpyxl、 xlrd、 Pandas 幾種函式庫有支援 xlsx 的讀寫。最決定用 Pandas，因為我平常資料操作都是用它，就不用另外裝函式庫了。
- [Python 讀寫 Excel 文件--使用 xlrd 模塊讀取，xlwt 模塊寫入](https://www.itread01.com/content/1507641632.html)
- [Python 使用 openpyxl 讀寫 Excel 檔案的方法](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/365164/)

```python
import pandas as pd
df=pd.read_excel("原料年檢資料.xlsx")
# 印出前四筆資料
df.head(4)

# 轉成 numpy.ndarray 格式
nmp=df.values
```

讀進來之後就變 Pandas 格式了，可以接續一般的操作了。不過我在執行程式碼時遇到下面的錯誤訊息：

```bash
Pandas pd.read_excel giving ImportError: Install xlrd >= 0.9.0 for Excel support
```

看起來是安裝 xlrd 0.9.0 以上的版本就可以了，結果還是得裝套新的函式庫：

```bash
$pip install xlrd
```


<br><br> 

## Pandas 寫檔

至於寫檔的部分我其實偷懶，寫了 CSV 出去，反正給 CSV 我媽他們系統也可以接受。

不過為了寫網誌，還是[稍微看了一下如何寫檔](https://shihs.github.io/blog/python/2019/01/02/Python-%E8%AE%80%E5%8F%96%E8%88%87%E5%AF%AB%E5%85%A5xlsx%E6%AA%94%E6%A1%88/)，算是留個紀錄方便日後找查，絕對不是我不想再多裝個函式庫 XDDD    


```python
# 因為我要寫中文檔案 engine 要換成 openpyxl
writer = pd.ExcelWriter('pandas_simple.xlsx', engine='openpyxl')

# df 指要寫出的內容
df.to_excel(writer, sheet_name='Sheet1')
writer.save()
```

看這段程式碼沒意外還是得安裝 openpyxl：
```bash
$pip install openpyxl
```

<br><br> 

## 參考資料 
1. M.C. Shih (2019-01-02)。[[Python]讀取與寫入xlsx檔案](https://hackmd.io/qmlrnWi6SjqNiS7LendEaQ?edit) 。檢自 M.C. Shih (2020-07-31)。

<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期： 2020-12-21</summary>
  <ul>
    <li>2020-12-21 發布</li>
    <li>2020-09-12 完稿</li>
    <li>2020-09-11 起稿</li>
  </ul>
</details>
