---
title: 【LeetCode】0065. Valid Number
date: 2019-01-04
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Validate if a given string can be interpreted as a decimal number.

<!--more-->
Some examples:  
```python
"0"  =>  True  
" 0.1 "  =>  True  
"abc"  =>  False  
"1 a"  =>  False  
"2e10"  =>  True  
" -90e3 "  =>  True  
" 1e"  =>  False  
"e3"  =>  False  
" 6e-1"  =>  True  
" 99e2.5 "  =>  False  
"53.5e93"  =>  True  
" --6 "  =>  False  
"-+3"  =>  False  
"95a54e53"  =>  False
```

<br class="big"><br class="big">

> **Note:**  It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:
>
> -   Numbers 0-9
> -   Exponent - "e"
> -   Positive/negative sign - "+"/"-"
> -   Decimal point - "."
> 
> Of course, the context of these characters also matters in the input.

<br class="big">

**Related Topics:** `Math`、`String`



## 解題邏輯與實作
這題是要判斷給定的字串是否為一個合法的十進位數字，合法的數字中包含正負號、小數、科學記號 e，另外科學表示的部分 e 與後面的數字間可夾正號，最後是字串前後的的空白不影響判定。

基本上判定規則就這些，但需要特別注意兩個 case：`".1"` 與 `"3."` ，這兩種表示法也是合法的。 


### 正規表示式 Regular Expression
這種抽 pattern 的問題，我第一個想到的是 Regular Expression，思考會比簡單，程式碼也簡潔許多。

```python
import re

class Solution:
   def isNumber(self, s):
      reg = re.compile(r'^\s*[-+]?(\d+\.?|\.\d+)\d*(e[-+]?\d+)?\s*$')
      return re.match(reg, s) != None
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)