---
title: 【LeetCode】0044. Wildcard Matching
date: 2018-12-20
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for  `'?'`  and  `'*'`.
```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```
The matching should cover the  **entire**  input string (not partial).
<!--more-->
> **Note:**
> -   `s` could be empty and contains only lowercase letters  `a-z`.
> -   `p`  could be empty and contains only lowercase letters  `a-z`, and characters like  `?` or `*`.

<br>

**Example 1:**
```python
Input: s = "aa"  p = "a"
Output: False
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**
```python
Input: s = "aa"  p = "*"
Output: True
Explanation: '*' matches any sequence.
```

**Example 3:**
```python
Input: s = "cb"  p = "?a"
Output: False
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```

**Example 4:**
```python
Input: s = "adceb" p = "*a*b"
Output: True
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```

**Example 5:**
```python
Input: s = "acdcb" p = "a*c?b"
Output: False
```

<br>

**Related Topics:**`String`、`Backtracking`、`Dynamic Programming`、`Greedy`

<br><br>

## 解題邏輯與實作
有點像是第十題的 [Regular Expression Matching](/LeetCode-0010-Regular-Expression-Matching/)，不過符號的意義不太一樣，這邊使用的 `?` 表示一個萬用字元， `*` 則是表示多個萬用字元。


<br>
### 遞迴
跟 Regular Expression 相比最大的差異應該是 `*` 的用法。在Regular Expression 中 `*` 不可能單獨存在，必定會跟跟在某個字元後面，在產生 subpattern 時要特別注意；但在這題中 `*` 可以單獨存在，subpatter 應該只有 pattern 或 pattern[1:] 兩種。

1. 若 pattern 為空字串，則檢查 string 是否為空字串
	1.  是，則回傳 True
	2.  否，則回傳 False
2. 檢查 string 是否為空字串
    1.  是，判斷pattern是否為 `*`
		  1. 是，呼叫遞迴傳入 string 與 pattern[1:] 
		  2. 否，則回傳 False
    2.  否，向下執行 
3. pattern 的字首與 string 的字首是否匹配（匹配情況包含 pattern 字首為 `?` 與 `*` 的情況）
	1.  是，判斷 pattern 是否為 `*`
		 1. 是，則呼叫遞迴傳入分別傳入 string[1:] 與 pattern 和 string[1:] 與 pattern[1:] 
		 2. 否，則呼叫遞迴傳入 string[1:] 與 pattern[1:] 
	2.  否，則回傳 False

<br>

一樣多加了參數暫存比較結果，節省呼叫次數。不過按照上面那個邏輯去寫在遇到這個 [testcase](https://leetco%20de.com/submissions/detail/195948838/testcase/) 時會 Memory Limit Exceeded，所以修改了第二個判斷式的邏輯，判斷 pattern 扣除掉 `*` 長度：
1. 若 pattern 扣除掉 `*`長度為0，則回傳True
	- 代表 pattern 剩下的整句都是 `*`，因此不用管string內容是什麼一定會匹配。
2. 若 pattern 扣除掉 `*` 長度大於 string 的長度，則回傳 False
	-  也就是即便 `*` 全用來匹配空字串，仍會有部份 pattern 無法匹配上 string，因為除`*` 外的 pattern 符號都必須匹配一個字元，但目前 pattern 比 string 還長...
	- 另外 string 長度等於 0 的 case，也包含在這，因此原先判斷式中對於 string 長度為 0 的 case 可以全刪除了。


```python
class Solution:
  def __init__(self):
    self.memo = {}

  def isMatch(self, s, p):
    if (s,p) in self.memo:
      return self.memo.get((s,p))

    p_len = len(p)
    s_len = len(s)
    sub_p_len = len(p.replace("*",""))
     
    if p_len == 0:
      return s_len == 0

    if sub_p_len == 0 :
      return True
  
    if sub_p_len > s_len:
      self.memo[(s,p)] = False
      return False

    result = False
    if p[0]=='?' or p[0]=='*' or p[0] == s[0]:
      if p[0] == '*':    
        result = self.isMatch(s[1:] , p) or self.isMatch(s , p[1:])
      result = result or self.isMatch(s[1:] , p[1:]) 

    self.memo[(s,p)] = result
    return result
```
總算可以過了，但說實話效能不是很好跑出了1096 ms、40.91 %的成績，還有非常大的改進空間。

<br>

### 雙指標

原本是在找 Dynamic Programming 的解法，但連續找到兩篇說用 DP 會 TLE，說要改用雙指標回朔，所以我決定也來寫寫看：
1. 初始化兩個指標 s_index 與 p_index，分別指向 string 與 pattern 的字首
2. 判斷 s_index 是否小於 string 長度
	1. 是
		1. 若指標位置可以匹配（匹配情況包含 pattern 為 `?` ，但排除為 `*` 的情況），兩個指標皆向後移動
		2. 若 pattern 目前指向為 `*`，則 pattern 指標向後移動，並記下目前指標位置（last_s_index 與 last_p_index）
			- 先假設 `*` 匹配的是空字元的情況，向後進行匹配，
		3.  假設 `*` 匹配空字元狀況下，無法完成整句匹配，因此所紀錄的位置，且 last_s_index加一
			- 表示它匹配了 `*`，因此可以不再考慮
		4.  若上述的 Case 皆無法匹配，則為回傳 False
	2. 否，向下執行
3.  在掃完 string 後，檢查 patter n是否還有剩下除 `*` 外的字元
	1. 有，表示未完成匹配，回傳 False
	2. 無，則回傳 True

```python
class Solution(object):
  def isMatch(self, s, p):
    """
    :type s: str
    :type p: str
    :rtype: bool
    """
    s_len, p_len = len(s), len(p)
    s_idx, p_idx = 0, 0
    last_s_idx, last_p_idx = -1, -1

    while s_idx < s_len:
      if p_idx < p_len and (p[p_idx]=='?' or p[p_idx]==s[s_idx]):
        s_idx += 1
        p_idx += 1
      elif p_idx < p_len and p[p_idx]=='*' :
        p_idx += 1
        last_s_idx, last_p_idx = s_idx , p_idx 
      elif last_p_idx != -1 :
        last_s_idx += 1
        s_idx, p_idx = last_s_idx, last_p_idx
      else:
        return False
    
    return len(p[p_idx:].replace("*","")) == 0
```

效能好上不少，136 ms、 83.93% 

<br>

### Dynamic Programming & Greedy Algorithm
是說看標籤應該還可以用 Dynamic Programming 跟 Greedy Algorithm 來解題，先欠著回頭再來研究 XD


<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)