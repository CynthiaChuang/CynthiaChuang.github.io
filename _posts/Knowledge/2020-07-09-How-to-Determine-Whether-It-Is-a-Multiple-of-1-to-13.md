---
title: "如何判別是否為 1～13 的倍數"
date: 2020-08-10
is_modified: false
categories:
- Knowledge
tags:
- Math
- Python
--- 

那天在社團看到有人在詢問倍數的判斷的寫法，在家監工閒著也是閒著，就按這篇[判別法](https://leestar2013.pixnet.net/blog/post/45638266)把剩下的判斷式也補齊了。
  
雖然我覺得我在實際專案中應該不會這麼判斷 XD
<!--more-->

<center> <img src="https://i.imgur.com/6urZjMj.jpg" alt="Math"></center>
<center class="imgtext">（圖片來源: <a href="https://www.pexels.com/zh-tw/photo/3729557/" class="imgtext">Ian Panelo ｜ Pexels</a>）</center>
<br><br> 

## 1的倍數 
1 的倍數：任何數皆為 1 的倍數。
```python
def is_multiple_of_1(number):
  return True
```

<br>

## 2的倍數
2 的倍數：個位數字為偶數（含0）。
```python
def is_multiple_of_2(number):
  sd = int(number[-1])
  return sd % 2 == 0
```

<br>

## 3的倍數
3 的倍數：各個數字和為 3 的倍數。
```python
def is_multiple_of_3(number):
  s = sum([int(n) for n in number[::]])
  return s % 3 == 0
```

<br>

## 4的倍數
4 的倍數：末二位數為 4 的倍數。
```python
def is_multiple_of_4(number):
  s = int(number[-2:])
  return s % 4 == 0
```

<br>

## 5的倍數
5 的倍數：個位數字為 5 或 0。
```python
def is_multiple_of_5(number):
  s = int(number[-1])
  return s==0 or s==5
```

<br>

## 6的倍數
6 的倍數：各個數字和為 6 的倍數（同時是2和3的倍數）。
```python
def is_multiple_of_6(number):
  return is_multiple_of_2(number) and is_multiple_of_3(number)
```

<br>

## 7的倍數
7 的倍數：由個數起每三位數字一節，各奇數節的和與偶數節的和相減，其差是 7 的倍數。
```python
def is_multiple_of_7(number):
  group = [number[max(i-3,0):i] for i in range(len(number), 0, -3)]
  odd_group = sum([int(n) for n in group[::2]])
  even_group = sum([int(n) for n in group[1::2]])
  df = odd_group - even_group
  return df % 7 == 0
```

<br>

## 8的倍數
8 的倍數：末三位數為 8 的倍數。
```python
def is_multiple_of_8(number):
  s = int(number[-3:])
  return s % 8 == 0
```
    
<br>

## 9的倍數
9 的倍數：各個數字和為 9 的倍數。
```python
def is_multiple_of_9(number):
  s = sum([int(n) for n in number[::]])
  return s % 9 == 0
```

<br>

## 10的倍數
10 的倍數：個位數字為 0。
```python
def is_multiple_of_10(number):
  sd = int(number[-1])
  return sd == 0
```

<br>

## 11的倍數
11 的倍數：奇數位數字和與偶數位數字和相差為 11 的倍數。

```python
def is_multiple_of_11(number):
  odd_idx = sum([int(n) for n in number[::2]])
  even_idx = sum([int(n) for n in number[1::2]])
  d = odd_idx-even_idx 
  return d % 11 == 0
```

<br>

## 12的倍數
12 的倍數：同時是 3 和 4 的倍數。
```python
def is_multiple_of_12(number):
  return is_multiple_of_3(number) and is_multiple_of_4(number)
```

<br>

## 13的倍數
13 的倍數：由個數起每三位數字一節，各奇數節的和與偶數節的和相減，其差是 13 的倍數。
```python
def is_multiple_of_13(number):
  group = [number[ max(i-3,0):i] for i in range(len(number), 0, -3)]
  odd_group = sum([int(n) for n in group[::2]])
  even_group = sum([int(n) for n in group[1::2]])
  df = odd_group - even_group
  return df % 13 == 0
```

<br><br> 

## 參考資料 
1. 李老師 (2012-09-17)。[[數學]1～13的倍數判別法](https://leestar2013.pixnet.net/blog/post/45638266) 。檢自 痞客邦 (2020-07-02)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-10</summary>
  <ul class="timestamp">
    　<li>2020-08-10 發布</li>
    　<li>2020-07-09 完稿</li>
    　<li>2020-07-02 起稿</li>
  </ul>
</details>