---
title: 【LeetCode】0139. Word Break
date: 2018-12-21
categories:
- Interview/Problemset
tags:
- LeetCode
- Python
--- 

Given a  **non-empty**  string  _s_  and a dictionary  _wordDict_  containing a list of  **non-empty**words, determine if  _s_  can be segmented into a space-separated sequence of one or more dictionary words.
<!--more-->
<br> 

> **Note:**
>  - The same word in the dictionary may be reused multiple times in the segmentation.
> - You may assume the dictionary does not contain duplicate words.

<br>

**Example 1:**
```python
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: True
Explanation:Return true because "leetcode" can be segmented as "leet code".
```

**Example 2:**
```python
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: True
Explanation:Return true because "leetcode" can be segmented as "leet code".
```

**Example 3:**
```python
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: False
```

<br>

**Related Topics:**`Dynamic Programming`

<br><br>

## 解題邏輯與實作
這題是要將題目給定的句子拆解成字典中的單字。第一個想到的就是我最愛的暴力法，拆、拆、拆就對了！


<br>

### 遞迴
也就是暴力解，想法很簡單，就是把句子拆成前後兩句下去查，一旦有一種拆法可以查的到就回傳 True，否則回傳 False。當然為了避免大量重複計算，例外用HashTable 紀錄查過得結果，解法如下：

1.  查詢此單字是否存在於字典中
	1. 有，則回傳 True
	2. 否，則向下執行
2.  使用一個指標由前向後遍歷此單字，並依指標位置，將單字拆成前後部份並呼叫遞迴傳入，唯有當前後部份皆可在字典中查到時才回傳 True，否則繼續向後遍歷。
3. 若遍歷完整個字串皆無法找到拆解法則回傳 False


```python
class Solution:
    def __init__(self, mydict=None):
        self.mydict = mydict
        self.memo = {}

    def set_dict(self,mydict):
        self.mydict = mydict
        
    def wordBreak(self, s, wordDict):
        self.set_dict(set(wordDict))
        return self.check(s)

    def check(self, s):
        length = len(s)
        
        if s in self.memo:
            return self.memo.get(s)

        if length <= 0 or s in self.mydict:
            self.memo[s] = True
            return True

        result = False
        for i in range(1, length):
            result = self.check(s[:i]) and self.check(s[i:])
            if result :
                break

        self.memo[s] = result
        return result
```
但果不期然，效率有點差跑出了個 740 ms, 1.20%  的成績，摸摸鼻子改寫 DP 去。

<br>

### Dynamic Programming
這題大概是難得我這幾天整理的題目中，DP 解終於不是欠著的題目了XDDD

這邊使用了一個 dp 陣列紀錄結果，其中 dp[i] 中紀錄著字串 s[:i] 是否能夠拆解，接下來若是 s[i:j]也可以進行拆解，因為已知 s[:i] 可拆解，又s[i:j] 可拆解，故 s[:j] 可拆解，因此紀錄 dp[j]為 True，按這關係推導，最後就可以判斷目標字串是否可以被拆解。
 
```python
class Solution:
    def wordBreak(self, s, wordDict):
        n = len(s)
        dp = [False] * (n + 1)
        dp[0] = True
    
        words = set(wordDict)
        for j in range(n):
            for i in range(j, -1, -1):
                if dp[i] and s[i:j + 1] in words:
                    dp[j + 1] = True
                    break
        return dp[n]
```
這效能果然好上不少，跑出了68 ms, 15.49% 的成績。

<br>

### Dynamic Programming 改進版
看到我的 code 雖然有比上次進步，但其實無條件進位也才 16% 而已，有些好奇前面 code是怎樣的，所以手賤點了前段班的 code 出來看。

發現他雖然也是用 DP 解，但他加上了點巧思，與我不同的是他是由後向前遍歷，並寫加上了檢查距離的限制，限制在字典中最長的單字長度上，檢查再多也沒有意義，因為字典就沒有這麼長的單字咩。最後跑出來成績為 36 ms, 99.65% 。

 
```python
class Solution:
    def wordBreak(self, s, wordDict):
        words = set(wordDict)
        maxLen = 0
        for word in wordDict:
            maxLen = max(maxLen, len(word))
        n = len(s) 
        dp =  [False] * (n + 1)
        dp[n] = True 

        for i in range(n - 1, -1, -1):
            for j in range(i + 1, min(i + maxLen, n) + 1): 
                if dp[j] and s[i:j] in words:
                    dp[i] = True
                    break 
        return dp[0]
```
<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)