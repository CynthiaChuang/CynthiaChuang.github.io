---
title: 【LeetCode】0007. Reverse Integer
date: 2019-06-05
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Given a 32-bit signed integer, reverse digits of an integer.

<!--more-->
**Example 1:**
```
Input: 123
Output: 321
```

**Example 2:**
```
Input: -123
Output: -321
```

**Example 3:**
```
Input: 120
Output: 21
```
<br class="big"><br class="big">

> Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [$−2^{31}$, $2^{31} − 1$]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

<br class="big">

**Related Topics:** `Math`



## 解題邏輯與實作
這題是要翻轉整數，如果直接當字串用 slice 就可以輕鬆解決，在加上 python 幾乎沒有 overflow 的問題，所以過程中也不用特地處理，回傳做個判斷就好，就是要注意一下正負問題而已，喔...還有尾數為 0 的狀況...

```python
class Solution:
   def reverse(self, x: int) -> int:
      if x == 0:
         return 0

      is_negative = x < 0
      x = x * -1 if is_negative else x 

      str_x = str(x).strip('0')
      str_x = str_x[::-1]
      x = int(str_x)
      
      x = x * -1 if is_negative else x            
      x = 0 if   x < -2147483648 or x > 2147483647 else x
      return x
```
<br class="big"><br class="big">
不過這一題，應該不是希望這樣寫才對，所以又乖乖著墨了一版不轉成字串的版本。

```python
class Solution:
   def reverse(self, x: int) -> int:
      if not x:
         return 0

      is_negative = x < 0
      x = x * -1 if is_negative else x 
      result = 0
      
      while x :
         result = result * 10 + x % 10
         x //= 10

      result = result * -1 if is_negative else result            
      result = 0 if result < -2147483648 or result > 2147483647 else result
      return result
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)