---
title: 【LeetCode】0010. Regular Expression Matching
date: 2018-12-19
is_modified: false
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for  `'.'`  and  `'*'`.
```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.
```
The matching should cover the  **entire**  input string (not partial).
<!--more-->
> **Note:**
> -   `s` could be empty and contains only lowercase letters  `a-z`.
> -   `p`  could be empty and contains only lowercase letters  `a-z`, and characters like `.` or `*`.

<br><br>
**Example 1:**
```python
Input:**
s = "aa"
p = "a"
Output:False
Explanation: "a" does not match the entire string "aa".
```

**Example 2:**
```python
Input:
s = "aa"
p = "a*"
Output: True
Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

**Example 3:**
```python
Input:
s = "ab"
p = ".*"
Output: True
Explanation: ".*" means "zero or more (*) of any character (.)".
```

**Example 4:**
```python
Input:
s = "aab"
p = "c*a*b"
Output: True
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
```

**Example 5:**
```python
Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
```
<br>

**Related Topics:**`Backtracking`、`Dynamic Programming`、`String`

<br><br>

## 解題邏輯與實作
這題是要實作一個簡易的 Regular Expression 的 Parser，僅支援 a-z、`.` 和 `*`，題目也限定了輸入會是這幾個字元，所以可以先不用考慮其他字元的情況。

<br>

### 遞迴
這題我最先想到的是使用遞迴來解，只是判斷的情況有些瑣碎。實作步驟如下：
1. 若 pattern 為空字串，則檢查 string 是否為空字串
	- 是，則回傳 True
	- 否，則回傳 False 

2.  檢查 pattern 的長度是否大於 2 且第二字元為 `*`
	- 是，判斷 string 是否為空字串
		- Case1：是，則呼叫遞迴傳入 string 與 pattern[2:] 
		- 否，則 pattern 的字首與 string 的字首是否相等〈相等情況包含 pattern 字首為`.`的情況〉
			- Case2：是，則呼叫遞分別傳入 string[1:] 與 pattern、string[1:] 與 pattern[2:] 和 string 與 pattern[2:] 
			- Case3：否，則呼叫遞迴傳入 string 與 pattern[2:] 
	
	- 否，則判斷 string 的長度是否大於 0 且 pattern 的字首與 string 的字首是否相等
		- 是，則呼叫遞迴傳入 string[1:] 與 pattern[1:]
		- 否，則回傳 False

<br>

另外針對流程中幾個地回傳入的Case進行說明：
- **Case1**： pattern 第二字元為 `*` 且 string 是為空字串，則呼叫遞迴傳入 string 與 pattern[2:] 
-`*` 可代表 pattern[0] 出現0次，因此略過 pattern 字首向後檢查。

- **Case2**：是，則呼叫遞分別傳入 string[1:] 與 pattern、string[1:] 與 pattern[2:] 和 string 與 pattern[2:] 
    -	string[1:] 與 pattern：為 `*` 出現多次的情況，下個字元比對時仍與 pattern 字首比對。
    -	string[1:] 與 pattern[2:] ：為 `*` 出現1次的情況，因此下一個字元比對時略過 pattern 字首。
    -	string 與 pattern[2:] ：為 `*` 出現0次的情況，因此忽略 pattern 字首。

- **Case3**： pattern 第二字元為 `*` 且 pattern 的字首與 string 的字首**不相等**，則呼叫遞迴傳入 string 與pattern[2:]  
-在不相等的情況下，因為 `*` 的緣故可以假定 pattern[0] 為不存在〈出現0次〉的元素，因此略過pattern 字首向後檢查。
 
```python
class Solution:
  def __init__(self):
    self.memo = {}

  def isMatch(self, s, p):
    p_len = len(p)
    if p_len == 0:
      return len(s) == 0
	
    if (s,p) in self.memo:
      return self.memo.get((s,p))

    result = False
    if p_len >= 2 and p[1] == '*' :
      if len(s) > 0 and (p[0] == '.' or  p[0] == s[0]):
        result = self.isMatch(s[1:] , p) or self.isMatch(s[1:] , p[2:]) or self.isMatch(s , p[2:])
      else:
        result = self.isMatch(s,p[2:])

    elif len(s) > 0  and (p[0] == '.' or  p[0] == s[0]):
      result =  self.isMatch(s[1:],p[1:])

    self.memo[(s,p)] = result
    return result
```
不過單用遞迴會 Time Limit Exceeded，因此多加了參數暫存比較結果，節省呼叫次數。

果然效能好多了，Runtime: 52 ms, faster than  99.53% 

<br>

### Dynamic Programming
是說寫完地回後很高興要切下一題的時候，才後知後覺的想到，我剛剛應該是挑了 DP  的標籤要練習阿XDDD

但是，我真的對DP不太擅長的說，想破了頭還是毫無頭緒，只好去翻了下[討論區的答案](https://leetcode.com/problems/regular-expression-matching/discuss/5684/9-lines-16ms-c-dp-solutions-with-explanations)，解釋有點難懂還沒有啃透，晚點再回來補充好了...腦袋燒壞Orz

1.  `P[i][j] = P[i - 1][j - 1]`, if  `p[j - 1] != '*' && (s[i - 1] == p[j - 1] || p[j - 1] == '.')`;
2.  `P[i][j] = P[i][j - 2]`, if  `p[j - 1] == '*'`  and the pattern repeats for  `0`  times;
3.  `P[i][j] = P[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.')`, if  `p[j - 1] == '*'`  and the pattern repeats for at least  `1`  times.

```python
class Solution:
  def isMatch(self, s, p):
    m = len(s)
    n = len(p)
    dp = [[False for i in range(n + 1)] for i in range(m + 1)]
    dp[0][0] = True
    for i in range(m+1):
      for j in range(1, n+1):
        if p[j-1] == '*':
          dp[i][j] = dp[i][j - 2] or (i > 0 and (s[i - 1] == p[j - 2] or p[j - 2] == '.') and dp[i - 1][j])
        else:
          dp[i][j] = i > 0 and dp[i - 1][j - 1] and (s[i - 1] == p[j - 1] or p[j - 1] == '.')
    return dp[m][n]
```
是說，我硬是將討論區的 Code 改成 Python 版本的，執行時間 60 ms，效能好像沒有比較好？




<br><br>

## 參考資料 
1. [9-lines 16ms C++ DP Solutions with Explanations](https://leetcode.com/problems/regular-expression-matching/discuss/5684/9-lines-16ms-c-dp-solutions-with-explanations)


<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)