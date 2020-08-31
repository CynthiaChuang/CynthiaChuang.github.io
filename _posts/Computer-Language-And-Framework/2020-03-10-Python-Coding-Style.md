---
title: Python Coding Style
date: 2020-03-10
is_modified: false
categories:
- Computer-Language/Framework
tags:
- Coding Style
- Python
--- 
 
紀錄一下我自己的 Python Coding Style，最近都照專案原本的 Coding Style 在寫，還是紀錄一下，免得忘了自己 Style。

<!--more-->
<center> <img src="https://i.imgur.com/8znl2lD.jpg" alt="Coding Style"></center>
<center class="imgtext">Coding Style（圖片來源: <a href="https://www.orientation-education.com/metier/developpeur-informatique" class="imgtext">orientation</a>）</center>
<br>

## Code Lay-out

### 縮排
Python 是用縮排來表示區塊的語言，所以最好保持縮排的一致，不然很容易出問題...。尤其是跟 Windows 的開發者 co-work 的時候，這個狀況更明顯，為了避免無謂的 `diff` ，依照 [code style](https://www.python.org/dev/peps/pep-0008/#code-lay-out) 建議改用<span class='highlighting'>用 4 個空格取代 Tab</span>，這個可以直接在 IDE 設定，把 Tab 的輸出換成 4 個空格即可。

<br>

### 單行字數
guideline 建議 79，只是我通常取整數設 <span class='highlighting'>80</span> 。另外，如果有遇到縮排縮太深，導致字串超過螢幕，必須捲動下方捲才能閱讀的，我會直接換行，不過通常 IDE 會幫你把前面的空格一併記入字數計算  

...是說如果縮排縮太深，應該要考慮重構了。


<br>

### 換行與對齊
當變數名稱太長或是參數太多，導致需要換行的時候，如果有第一個參數，對齊第一個參數；如果沒有，就縮排一次。

```python
## 如果有第一個參數，對齊第一個參數
self.train_batches = tdatagen.flow_from_directory(self.dataset_train_path,
                                                       target_size=self.image_size,
                                                       interpolation='bicubic',
                                                       class_mode=self.class_mode,
                                                       shuffle=True,
                                                       batch_size=self.batch_size)

## 如果沒有，就縮排一次
self.train_batches = datagen.flow_from_directory(
    self.dataset_train_path,
    target_size=self.image_size,
    interpolation='bicubic',
    class_mode=self.class_mode,
    shuffle=True,
    batch_size=self.batch_size)

```

<br> 如果是遇到計算式需要換行，則在運算子之前換行
```python
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```


<br>

### 空行
最外層的函數或類別，兩兩之間用<span class='highlighting'>兩行</span>隔開，類別內部的方法則用<span class='highlighting'>單行</span>進行區隔。不過這我老是忘記，都會用自動排版幫忙校正。

 
<br>

### Imports
<span class='highlighting'>一行一個 Import 模組</span>，除非是用 Wildcard imports 才會把從同一個模組 imports 出來的東西寫在同一行


```python
import os
import sys
from subprocess import Popen, PIPE
```
 
<br> 另外 guideline 建議 <span class='highlighting'>import 要分群</span>，每群之間使一行空白分隔，分群規則則按照：
- 標準函式庫
- 第三方函式庫
- 本地端檔案及函式庫引用

不過我通常第一跟第二項都混成一群了....

 
<br> 另外 guideline 推薦 import 本地端檔案及函式庫用相對路徑...，不過我個人習慣是用<span class='highlighting'>相對路徑</span>。

<br><br>  

## String Quotes
在 python 中，字串單引號與雙引號都可以，但我偏愛先<span class='highlighting'>雙引號</span>再單引號。


<br><br>  

## Whitespace in Expressions and Statements
與分隔符號及表達式間不用空白。

```python
Yes: spam(ham[1], {eggs: 2})
No:  spam( ham[ 1 ], { eggs: 2 } )

Yes: foo = (0,)
No:  bar = (0, )
 
Yes: if x == 4: print x, y; x, y = y, x
No:  if x == 4 : print x , y ; x , y = y , x

Yes: spam(1)
No:  spam (1)
```

<br> 參數名稱與值之間也不用留白
```python
Yes:
def complex(real, imag=0.0):
    return magic(r=real, i=imag)

No:
def complex(real, imag = 0.0):
    return magic(r = real, i = imag)
```

不過我發現，如果有指定參數型態的話，IDE 又會建議加上空白 @@

```
Yes:
def complex(real, imag: float = 0.0):
        pass

No:
def complex(real, imag:float=0.0):
        pass
```

    
    

<br> 但我會在低優先權的運算子兩邊加上空白。
```python
i = i + 1
submitted += 1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

<br><br>  

## Naming Conventions

### 命名規則
1. Modules and packages：一律小寫
2. Globals and constants：一律大寫
3. Class ：大駝峰式命名法， i.e. `ClassName`
4. Methods and functions：小蛇式命名法（所有字都小寫，並用 _ 區隔）， i.e. `method_name`
5. Local variables：小蛇式命名法

<br>

### 底線開頭的變數

1. `_foo`:  
    類似 C/C++ 系列的 <span class='highlighting'>protected</span>。無法被直接 import ，也不應該直接呼叫函數存取變數或存取變數，這邊 Python 沒禁止，但還是建議將它視為 protected 變數，不要在外部呼叫。最後與 protected 變數相同可以被子類別繼承。
    
    雖說是無法被 import ，但若是硬要 import 也不是做不到
    
    ```python
    No: import *
    Yes: import _protected_func
    
    ```

2. `__foo__`:  
    不要用！這是留給 <span class='highlighting'>Python builtin 專用</span>的，用了小心哪天升級了後會爆炸 :bomb:
    
3. `__foo`:  
    這對應到的是 <span class='highlighting'>private</span> 變數。只允許類別本身呼叫，無法被外部成員或是子類別直接操作與呼叫。


 
 
<br><br> 

## Linter

我慣用的 IDE 有兩個，分別是 Intellij IDEA 與 vscode。

如果在 Intellij IDEA 我通常會依賴內建自動排版來挑整一些縮排，並提供一些提示；但若在 vscode 我則是會額外安裝 [Pylint](https://www.pylint.org/) 來輔助。

 
<br><br> 

## 參考資料 
1. [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/) 。檢自 Python (2020-03-06)。
2.  subineru (2018-02-20)。[Python 變數命名規則](https://subineru.wordpress.com/2018/02/20/python-%E8%AE%8A%E6%95%B8%E5%91%BD%E5%90%8D%E8%A6%8F%E5%89%87/) 。檢自 CJ Hung (2020-03-06)。
3. 宅吉便 (2017-06-05)。[Python，你到底是在__底線__什麼啦！](https://aji.tw/python%E4%BD%A0%E5%88%B0%E5%BA%95%E6%98%AF%E5%9C%A8__%E5%BA%95%E7%B7%9A__%E4%BB%80%E9%BA%BC%E5%95%A6/) 。檢自 宅吉便 (2020-03-06)。
4. sooner高 (2017-05-07)。[Python 类中Name mangling和下划线命名](https://blog.csdn.net/g11d111/article/details/71367649) 。檢自 Python_g11d111的博客 - CSDN博客 (2020-03-06)。