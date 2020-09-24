---
title: 【LeetCode】0032. Longest Valid Parentheses
date: 2018-12-21
is_modified: false
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given a string containing just the characters  `'('`  and  `')'`, find the length of the longest valid (well-formed) parentheses substring.
<!--more-->
<br>

**Example 1:**
```python
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is `"()"`
```
**Example 2:**
```python
Input: "`)()())`"
Output: 4
Explanation: The longest valid parentheses substring is `"()()"`
```
<br>

**Related Topics:**`String`、`Stack`、`Dynamic Programming`

<br><br>

## 解題邏輯與實作
這題算是  [Valid Parentheses](/LeetCode-0020-Valid-Parentheses/) 的進階版，不過還是可以用 Stack 來解決。

<br>

### Stack
由於我需要知道知道它們之間的距離，所以推入 stack 時我除了紀錄括號外，也紀錄他們的索引值。
1. 開始前先 `push ( -1, ')' )` 進入 stack
2. 依序讀入字串
	1. 若遇到左括號，`push (index ,  '(' )`
	2. 若遇到右括號，則檢查stack最上層
		1. 若為右括號，`push  (index ,  ')' )`
		2. 若為左括號，將其 pop 並檢查長度，檢查方式為：使用目前索引值減掉目前最上層的索引值後，與目前所紀錄的最長合法長度取 max。

```python
from pythonds.basic.stack import Stack 
class Solution:
   def longestValidParentheses(self, s):
      max_valid = 0
      stack = Stack()
      stack.push((-1,')'))
      for i, char in enumerate(s) :
         if char == '(':
            stack.push((i,char))
         else:
            if stack.peek()[1] == ")" :
               stack.push((i,char))
            else:
               stack.pop()
               max_valid = max(max_valid, i - stack.peek()[0]) 

      return max_valid
```

<br>

### Dynamic Programming
Po 文前打 Tag 時才發現，這題的標籤是 DP 反而沒有 Stack 耶～！  
所以...我逃避現實先，之後再來補好了....是說我欠了多少題的 DP 解了？

DP 參考作法 → [傳送門](https://leetcode.com/articles/longest-valid-parentheses/)



<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)