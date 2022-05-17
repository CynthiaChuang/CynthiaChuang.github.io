---
title: 【LeetCode】0006. ZigZag Conversion
date: 2018-12-19
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

The string  `"PAYPALISHIRING"`  is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
<!--more-->
```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line:  `"PAHNAPLSIIGYIR"`
<br><br>

Write the code that will take a string and make this conversion given a number of rows:
```
string convert(string s, int numRows);
```

**Example 1:**
```python
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**Example 2:**
```python
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```
<br>

**Related Topics:** `String`

<br><br>

## 解題邏輯與實作
基本上這題就是在找規律，有點麻煩的說...

用一個比較好說明的例子，在找規律的時候，我先觀察了直行的部分：
```
numRows(n) = 5

row0:  0     8         16       24
row1:  1   7 9      15 17     23
row2:  2  6  10   14   18   22
row3:  3 5   11 13     19 21
row4:  4     12        20 
```
<br>

取出各個 row ，直行的部分，會發現它們都是等差數列，其差值可表示為 $2\ast(n-1)$
```
row0:  0, 8,  16
row1:  1, 9,  17
row2:  2, 10, 18
row3:  3, 11, 19
row4:  4, 12, 20 
```
<br>

另外可以發現，除了第一列與最後一列（row = 0 與 n-1）外，其他列在兩個直行之間會在多輸出一個字元，將這個字元與前一個字元合併用 pair 表示，每一個 pair 都是一個等差，其差值可表示為 $2\ast(n-1-i)$，其中 $i = 1, ... n-2$。
```
row1:  (1,7), (9,15),  (17,23)
row2:  (2,6), (10,14), (18,22)
row3:  (3,5), (11,13), (19,21)
```
<br>

此外，需要稍微注意一下corner case，為了簡單起見，我將 numRows 小於等於 1，以及 numRows 大於等於 s 的長度的 case，做了特別判斷，直接將輸入的 s 回傳回去。
<br>

```python
class Solution:
  def convert(self, s, numRows):
    if numRows<=1 or numRows >= len(s):
      return s

    result = ""
		
    len_s = len(s)
    max_row_number = numRows-1
    skip_mid_check = False
    for i in range(numRows):
      skip_mid_check = i == 0 or i == numRows-1
      for n in range(i, len_s, 2 * max_row_number):
        result += s[n]
        if not skip_mid_check:
          next_idx = n + 2 * (max_row_number-i)
          if next_idx < len_s:
            result += s[next_idx]
    return result
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)