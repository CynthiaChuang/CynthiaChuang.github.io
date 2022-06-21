---
title: 【LeetCode】0017. Letter Combinations of a Phone Number
date: 2019-06-11
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
---

Given a string containing digits from  `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

<p class="illustration">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png" alt="repl.it">
</p>

<!--more-->
<br>

**Example 1:**
```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
<br>

> **Note:**
> Although the above answer is in lexicographical order, your answer could be in any order you want.

<br>

**Related Topics:** `String`、`Backtracking`



## 解題邏輯與實作
這一題是要求字母的組合，在電話鍵盤上 2-9 分別對應到不同的字母，題目會給定一串數字，找出這串數字所有可能的字母組合。


這一題可以遞回來解，先建立一個 dict 記錄數字所對應的字母，然後將每個數字對應的字母都當做樹的節點，並使用深度優先搜尋 (dsf) 的方式來尋訪這棵樹。


```python
class Solution:
   digit2letters = { '2': "abc", '3': "def", '4': "ghi",   '5': "jkl",
                               '6': "mno", '7': "pqrs", '8': "tuv", '9': "wxyz", }
   def letterCombinations(self, digits: str) -> List[str]:
      if not digits:
         return []

      return self.dsf(digits,[],"")

   def dsf(self, digits, results = [], current = ""):
      if not digits:
         results.append(current)
         return results

      for c in self.digit2letters[digits[0]]:
         results = self.dsf(digits[1:],results,current+c)
      return results
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)