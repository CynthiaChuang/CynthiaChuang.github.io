---
title: 【LeetCode】0066. Plus One
date: 2019-01-03
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Given a  **non-empty**  array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

<!--more-->
<br>

**Example 1:**
```python
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

**Example 2:**
```python
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```

<br>

**Related Topics:**`Array`



## 解題邏輯與實作
這題也是逐位相加的的題目，不同的是這題加數固定是 1，所以要考慮的情況會簡單很多。關於這題我想到兩種做法，在 LeetCode 上跑起來效能差不多。


### 想法1
第一種想法是將加數當做進位數 carry ，使用迴圈依序取值與進位數相加，若相加結果大於 10 則繼續進位。最後回傳時，若進位數仍大於 1 ，則將其插入陣列前方。

迴圈執行中，我多加了一個終止條件，一旦進位值為 0 就停止迴圈，因為進位值若為 0 ，表示下個位數的值會與 0 相加，值不變也不會產生進位數，因此可以直接終止迴圈，減少迴圈次數。


```python
class Solution:
   def plusOne(self, digits):
      carry = 1
      digit_len = len(digits) 

      for i in range(digit_len-1,0-1,-1) :
         if carry == 0:
            break

         carry += digits[i]
         digits[i] = carry % 10
         carry //= 10    

      return digits if carry == 0 else [carry] + digits 
```


### 想法2
另一個想法則是，判斷尾數是否為 9 ，因為在加數為 1 情況下，唯一會啟動進位情況只有 (9,1) 這種組合。因此遇到 9 就回填 0 ，直到遇到第一個非 9 的數字，將其加 1 後中斷迴圈。回傳時，需檢查最高位是否為 0，若為 0 則在前方補上一個 1。

```python
class Solution:
   def plusOne(self, digits):
      digit_len = len(digits) 
      for i in range(digit_len-1,0-1,-1) :
         if digits[i] == 9:
            digits[i] = 0
         else:
            digits[i] += 1
            break

      return [1] + digits if digits[0] == 0 else digits
```
<br>

相同想法的另一個實作，但好像也沒比較快，三個程式碼都落在 56ms~60ms 左右。
```python
class Solution:
   def plusOne(self, digits):
      digit_len = len(digits) 
      index = -1
      for i in range(digit_len-1,0-1,-1) :
         if digits[i] != 9:
            index = i
            break 

      return [1] + [0] * digit_len if index == -1 else digits[:index] + [digits[index]+1] + [0] * (digit_len-index-1)
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)