---
title: 【Python】 bool()
date: 2020-02-27
categories:
- Computer-Language/Framework
tags:
- Python
--- 

今天開發的時候踩的一個坑，發現我的一個 flag 每次都是 true，後來才發現我掉的 `bool()` 的陷阱了 :crying_cat_face: 
<!--more-->
<br> 

情境是這樣的我們用了一個全域變數給使用者傳值，當它使用 multi gpu 時傳入 `1` 否則傳入 `0` ，用來作為判斷是否進行分散式訓練的前處理，按這個邏輯交出了這樣的程式碼：

```python
import os

IS_MULTI_GPU = bool(os.environ.get("IS_MULTI_GPU",0))
```

<br> 但卻發現我這個 flag 每次都是 true，導致 flag 有設跟沒設是一樣 :expressionless:

<br><br> 

##  bool() 

這個函數最廣為人知的用法大概是：

```python
bool(0)
>>> False

bool(1)
>>> True
```

<br>但回頭去查文件發現 <span class="highlighting">bool 其實是 int 的子類別</span>。看到這句話，我大概知道我的判斷式哪邊出錯了，`os.environ.get` 取回來的是<span class="highlighting">字串</span>，而不是整數...


<br>果然細看可以發現 `bool（）`，只有在下列情況回傳 false 而已，其他狀況都是回傳 true：

1. 傳入 False 值。
2. 傳入 None。
3. 傳入空值，如：空陣列、空列表、空字典、空字串...等。
4. 傳入數字 0 ，資料型態為整數或浮點數都算。
5. 傳入物件類別具有 `__bool __` 或 `__len（）__`，且其回傳值為 `False` 或 `0`。

<br>

```python
## case 1 : 傳入 False 值
bool(False)
>>> False

## case 2 : 傳入 None
bool(None)
>>> False

## case 3 : 傳入空值，如：空陣列、空列表、空字典、空字串...等
bool([])
>>> False

bool({})
>>> False

bool(())
>>> False

bool("")
>>> False

## case 4 : 傳入數字 0 ，資料型態為整數或浮點數都算。
bool(0.0)
>>> False

bool(0)
>>> False

## case 5 : 傳入物件類別具有 `__bool __` 或 `__len（）__`，且其回傳值為 `False` 或 `0`
class Test():
  def __init__(self):
    pass

  def __bool__(self):
    return False

bool(Test())
>>> False


class Test2():
  def __init__(self):
    pass
         
  def __len__(self):
    return 0
         
bool(Test())
>>> False
```

<br> 

而按照這規則 `os.environ.get` 取回來的是非空字串，所以這個 flag 每次都是 true...。既然知道哪邊出問題了，就好修正了...

```python
import os

IS_MULTI_GPU = os.environ.get("MULTI_GPU", 0) == "1"
```
<br> 最後嚴格要求當輸入為字串 `1` 時，才會啟動這個 flag。

<br><br> 

## 參考資料 
1. Chinmoy Lenka 。[bool() in Python](https://www.geeksforgeeks.org/bool-in-python/) 。檢自 GeeksforGeeks (2020-02-27)。
2. [Python bool() 函数](https://www.runoob.com/python/python-func-bool.html) 。檢自 runoob (2020-02-27)。