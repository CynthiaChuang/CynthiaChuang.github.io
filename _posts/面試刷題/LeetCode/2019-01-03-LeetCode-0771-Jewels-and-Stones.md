---
title: 【LeetCode】0771. Jewels and Stones
date: 2019-01-03
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

You're given strings  J  representing the types of stones that are jewels, and  S  representing the stones you have. Each character in  S  is a type of stone you have. You want to know how many of the stones you have are also jewels.

The letters in  J  are guaranteed distinct, and all characters in  J  and  S  are letters. Letters are case sensitive, so  "a"  is considered a different type of stone from  "A".

<!--more-->
<br>

**Example 1:**
```python
Input: J = "aA", S = "aAAbbbb"
Output: 3
```


**Example 2:**
```python
Input: J = "z", S = "ZZ"
Output: 0
```
<br>

> **Note:**
> -   `S`  and  `J`  will consist of letters and have length at most 50.
> -   The characters in  `J`  are distinct.

<br>

**Related Topics:**`Hash Table`

<br><br>

## 解題邏輯與實作
還滿簡單的一題，連想都不太需要想。題目翻譯成白話文是這樣的，給定兩個字串 J 與 S，檢查字串 S 中有多少字元屬於字串 J。不用管它寶石還石頭啦 XD

<br>

這題用暴力法逐一搜索就好了，為了改善時間複雜度，我將 J 放入 HashSet 中
```python
class Solution:
   def numJewelsInStones(self, J, S):
      J = set(J) 
      counter = [c for c in S if c in J]
      return len(counter)
```

<br>

程式碼可以進一使用 map 來化簡 
```python
class Solution:
   def numJewelsInStones(self, J, S):
      return(sum(map(J.count,S)))
```
<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)

 



 