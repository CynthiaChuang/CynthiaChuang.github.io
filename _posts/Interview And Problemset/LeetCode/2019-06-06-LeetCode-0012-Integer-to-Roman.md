---
title: 【LeetCode】0012. Integer to Roman
date: 2019-06-06
categories:
- Interview/Problemset
tags:
- LeetCode
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

<br>
Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.


**Example 1:**
```
Input: 3
Output: "III"
```

**Example 2:**
```
Input: 4
Output: "IV"
```

**Example 3:**
```
Input: 9
Output: "IX"
```

**Example 4:**
```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

**Example 5:**
```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

<br>

**Related Topics:**`Math`、`String`

<br><br>

## 解題邏輯與實作
厄.. 解法有點偷懶，我直接 hard code 一張對照表，把所有可能的組合列出，在依序找出數字中包含的最大的可轉化為羅馬數字的數字。例如1994，最大可轉換數字是 1000 ，減去 1000 後接下來依序可轉換數字為900 、90、4，因此最後輸出 MCMXCIV。


```python
class Solution:
  def intToRoman(self, num: int) -> str:
    int_to_roman =  ((1000, 'M'), (900, 'CM'), (500, 'D'), (400, 'CD'), (100, 'C'),
                     (90, 'XC'), (50, 'L'), (40, 'XL'), (10, 'X'), (9, 'IX'),
                     (5, 'V'), (4, 'IV'), (1, 'I'))

    result = ""
    for k_int, v_roman in int_to_roman:
      digital = num // k_int
      if digital > 0 :
        result += (v_roman * digital)
        num %= k_int 
				
    return result 
```
<br> P.S. 這應該算是貪婪演算法！？

<br><br>
不過這個版本直接把 4 這個特殊情況都直接 hard code 進去。所以後來又試著做只使用標準符號的。
<br>
array

|int|1000|500|100|50|10|5|1|
|---|---|---|---|---|---|---|---| 
|roman|M|D|C|L|X|V|I|
|index|0|1|2|3|4|5|6|

<br> 觀察一下，以百位數區間為例（index == 2），除了本身有符號表示的 500 與 100 ，可在細分成
1.  100 ~ 300： _C*n_ ，這區間直接以 100 的羅馬符號 _C_ 表示，最高位則是羅馬符號的出現次數。
2.  400： _CD_ ， 對照回 array 可表示成 array[index] + array[index-1] 。
3.  500 ：_D_ ，  array[index-1] 。
4.  600 ~ 800 ： _DC*n_ ，表示成 500 + 100 ~300。
5.  900 ：_CM_ ，對照回 array 可表示成 array[index] + array[index-2] 。
基本上個、十、百位數的情況都類似，而千位數因題限制輸入範圍只到 3999，所以只會出現 case 1。

```python
class Solution:
  def intToRoman(self, num: int) -> str:
    int_to_roman =  ((1000, 'M'), (500, 'D'), (100, 'C'), (50, 'L'), (10, 'X'),
						(5, 'V'), (1, 'I'))

		
    result = ""
    for i in range(0, len(int_to_roman), 2): 
      k_int, v_roman = int_to_roman[i] 
      digital = num // k_int

      if(1 <= digital <= 3):
        result += (v_roman * digital)
      elif(digital == 4):
        result += v_roman + int_to_roman[i-1][1]
      elif(5 <= digital <= 8):
        result += int_to_roman[i-1][1] + v_roman * (digital - 5)
      elif(digital == 9):
        result += v_roman + int_to_roman[i-2][1]

      num %= k_int
		
    return result
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)