---
title: 【LeetCode】0014. Longest Common Prefix
date: 2019-06-10
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string  `""`.

<!--more-->
<br>

**Example 1:**
```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**
```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```
<br>

> **Note:**
All given inputs are in lowercase letters  `a-z`.

<br>

**Related Topics:** `String`



## 解題邏輯與實作
這題是要找出最長共同字首，最直覺的方法是兩兩相比，所以這邊使用了遞迴，把它拆成兩組後兩相比較後，可得到共同字首。

```python
class Solution:
   def longestCommonPrefix(self, strs: List[str]) -> str:
      if not strs:
         return ""
      strs.sort()	
      return self.LCP(strs)

   def LCP(self, strs): 
      total_str = len(strs)      

      prefix = ""
      if total_str == 1 :
         prefix = strs[0]

      elif total_str == 2 :
         if len(strs[0]) <   len(strs[1]) :
            prefix = strs[0]
            compared = strs[1]
         else:
            prefix = strs[1]			
            compared = strs[0]

         while prefix not in compared[:len(prefix)] and len(prefix)>0 :
            prefix = prefix[:len(prefix)-1]


      else:
         half= total_str // 2 
         prefix = self.LCP([self.LCP(strs[:half]), self.LCP(strs[half:])]) 

      return prefix
```

<br><br>

除了直接兩兩比較外，另一個想法是以第一個字串當做基準，依序取出其字元與其他字串做比較，若該字元與任一字串相同位址的字元不同時，則停止輪巡，回傳該字元之前的字串作為共同字首。

```python
class Solution:
   def longestCommonPrefix(self, strs: List[str]) -> str:
      if not strs:
         return ""

      strs.sort(key = lambda i:len(i),reverse=False) 

      prfix = strs[0]
      for i, c in enumerate(prfix):
         for s in strs:
            if c != s[i] :
               return prfix[:i]
      return prfix
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)