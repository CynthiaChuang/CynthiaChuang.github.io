---
title: 【LeetCode】0020. Valid Parentheses
date: 2018-12-20
categories:
- Interview/Problemset
tags:
- LeetCode
- Python
- String
- Stack
--- 

Given a string containing just the characters  `'('`,  `')'`,  `'{'`,  `'}'`,  `'['`  and  `']'`, determine if the input string is valid.

An input string is valid if:

1.  Open brackets must be closed by the same type of brackets.
2.  Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.
<!--more-->
<br>

**Example 1:**
```python
Input: "()"
Output: True
```

**Example 2:**
```python
Input: "()[]{}"
Output: True
```

**Example 3:**
```python
Input:** "(]"
**Output:** false
```

**Example 4:**
```python
Input:"([)]"
Output: False
```

**Example 5:**
```python
Input: "{[]}"
Output: True
```


<br><br>

## 解題邏輯與實作
這題是在驗證括號是否成對，看到這種題目第一反應就是用 **Stack**：
1. 依序遍歷輸入字串
	 1. 若為左括號，則 push 進入 stack
	 2. 若為右括號，則
		  1. 若 Stack 為空，則回傳 False
		  2. 若 Stack 不為空，則取出 Stack 頂端元素，判斷是否成對，若不成對則回傳 False，反之繼續向下執行。
2. 直到字串遍歷完畢，且 Stack 為空，則回傳 True，反之回傳 False。

```python
class Stack(object):
  def __init__(self):
    self.stack=[]
  def isEmpty(self):
    return self.stack==[]
  def push(self,item):
    self.stack.append(item)
  def pop(self):
    if self.isEmpty():
      raise IndexError
    return self.stack.pop()
  def peek(self):
    return self.stack[-1]
  def size(self):
    return len(self.stack)
    
class Solution:
  def isValid(self, s: str) -> bool:
    match = {")":"(", "}":"{","]":"["}

    stack = Stack()
    legal = True
        
    for c in s:
      if c in match.values():
        stack.push(c)
      elif c in match.keys():
        if stack.isEmpty():
          legal=False
          break
        else:                    
          if match[c] !=  stack.pop():
            legal = False
            break
      else :
          legal=False
          break
                    
    if not stack.isEmpty():
      legal = False
        
    return legal
```

<br><br>

依照題目的輸入限制，進一步優化與精簡程式碼：
```python
class Solution:
  def isValid(self, s: str) -> bool:
    stack = []
    match = {")":"(", "}":"{","]":"["}

    for c in s:
      if c in match:
        if stack == [] or match[c] !=  stack.pop():
          return False
      else:
        stack.append(c)

    return stack == []
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)