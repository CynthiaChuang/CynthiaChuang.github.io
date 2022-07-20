---
title: 【LeetCode】0008. String to Integer (atoi)
date: 2019-06-05
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Implement  `atoi`  which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

<!--more-->
<p class="paragraph-spacing"></p>

> **Note:**
> -   Only the space character  `' '`  is considered as whitespace character.
> -   Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31}, 2^{31} − 1]$. If the numerical value is out of the range of representable values, INT_MAX ($2^{31} − 1)$ or INT_MIN ($−2^{31}$) is returned.

<p class="paragraph-spacing"></p>

**Example 1:**
```
Input: "42"
Output: 42
```

**Example 2:**
```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

**Example 3:**
```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

**Example 4:**
```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

**Example 5:**
```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned
```

<p class="paragraph-spacing"></p>

**Related Topics:** `Math`、`String`



## 解題邏輯與實作
這題是要將給定的字串轉成數字，字串轉數字不難，但討厭的是傳進來的字串有太多的整可能，例如：空格、正負號、非數字字元...等。
<p class="paragraph-spacing"></p>

依照題目給的範例，稍微列舉了一下可能狀況：
1. **字串前後若有空字元，不影響轉換：**  
這好處理，字串進來先執行 str.strip() 即可。

2. **有非數字字元的狀況：**  
這狀況有兩種，一種非數字字元（非正負號）是出現字首，則回傳 0 ；若非數字字元出現在中間或字尾，則將非數字字元之後全部省略。

3. **最後是 overflow 的狀況：**  
但這對 python 來說可以最後在判斷就好。

<p class="paragraph-spacing"></p>

也就是說，我需要取出字首含正負號的數字出來轉換，若不存在則回傳 0 ；反之，若存在則進行轉換。思考了下條件，決定用 **Regular Expression** 進行抽取。

```python
import re
class Solution:
   def myAtoi(self, str: str) -> int:
      INT_MAX = 2147483647
      INT_MIN = -2147483648

      val_str = re.match('(^[\+\-]?\d+)', str.strip())
      
      if not val_str :
         return 0
               
      val = int(val_str.group(0)) 
      val = min(INT_MAX, max(INT_MIN, val))
      return val
```

<p class="paragraph-spacing"></p><p class="paragraph-spacing"></p>

後來又試著不用  Regular Expression 做了一版，並進一步在轉換字串時，不使用 int 來實做
```python
class Solution:
   def myAtoi(self, str: str) -> int:
      INT_MAX = 2147483647
      INT_MIN = -2147483648

      str = str.strip()
      if len(str) == 0 :
         return 0 
			
      sign = 1
      val = 0
      if str[0] in ["-", "+"]:
         sign = 1 if str[0] == '+' else -1	
         str = str[1:]

      for char in str:
         if not char.isdigit(): 
            break
         val = val * 10 + (ord(char) - ord("0"))

      return min(INT_MAX, max(INT_MIN, val * sign))
```
<p class="paragraph-spacing"></p>

執行速度由 **44 ms / 83.62 %** 提升到 **40 ms / 92.02 %** 。



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)