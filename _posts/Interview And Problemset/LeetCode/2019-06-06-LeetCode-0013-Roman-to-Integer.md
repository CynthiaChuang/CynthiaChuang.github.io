---
title: 【LeetCode】0013. Roman to Integer
date: 2019-06-06
categories:
- Interview/Problemset
tags:
- LeetCode
- Python
--- 

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.
<!--more-->

|**Symbol**|**Value**|
|-------------|---| 
|I|1|
|V|5|
|X|10|
|L|50|
|C|100|
|D|500|
|M|1000|


<br>

For example, two is written as  `II` in Roman numeral, just two one's added together. Twelve is written as,  `XII`, which is simply  `X`  +  `II`. The number twenty seven is written as  `XXVII`, which is  `XX`  +  `V`  +  `II`.
<br>

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not  `IIII`. Instead, the number four is written as  `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as  `IX`. There are six instances where subtraction is used:
<br>

-   `I`  can be placed before  `V`  (5) and  `X`  (10) to make 4 and 9.
-   `X`  can be placed before  `L`  (50) and  `C`  (100) to make 40 and 90.
-   `C`  can be placed before  `D`  (500) and  `M`  (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**
```
Input: "III"
Output: 3
```

**Example 2:**
```
Input: "IV"
Output: 4
```

**Example 3:**
```
Input: "IX"
Output: 9
```

**Example 4:**
```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 5:**
```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

<br>

**Related Topics:**`Math`、`String`

<br><br>

## 解題邏輯與實作
在羅馬數字的規則中有一條，若是左邊的數字比右邊小，則這個位數的值為右減左。其他情況下至需要把羅馬數至相對應的值相加。

但在實做時，若發現當前的數字比前一個數字大，才回頭更新值，會使的程式流程變得相當複雜，因此這邊將傳入的字串反過來操作。

```python
class Solution:
  def romanToInt(self, s: str) -> int:
    roman_to_int = {"M": 1000, "D": 500, "C": 100, "L": 50, "X": 10, "V": 5, "I": 1}
    result = 0
    pre_int = 0
    for idx , char in enumerate(s[::-1]):
      now_int = roman_to_int [char]
      result += now_int * -1 if now_int < pre_int else now_int
      pre_int = now_int
			
    return result
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)