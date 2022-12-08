---
title: 【LeetCode】0003. Longest Substring Without Repeating Characters
date: 2019-03-06
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Given a string, find the length of the  **longest substring**  without repeating characters.
<!--more-->
> **Note**    
> that the answer must be a **substring**, `"pwke"` is a _subsequence_ and not a substring.

<br class="big">

**Example 1:**
```python
Input: "abcabcbb"
Output: 3 
Explanation: The answer is `"abc"`, with the length of 3. 
```

**Example 2:**
```python
Input: "bbbbb"
Output: 1 
Explanation: The answer is `"b"`, with the length of 1.
```

**Example 3:**
```python
Input: "pwwkew"
Output: 3 
Explanation: The answer is `"wke"`, with the length of 3. 
```
<br class="big">

**Related Topics:** `Hash Table`、`String`



## 解題邏輯與實作
這題是給定一個字串，從中找出一最長子字串，且子字串中不包含重複字元。


### Sliding Window
我這題使用 Sliding Window 來解，一開始將 ```start``` 與 ```end``` 指標指在起始位置（ index=0 ），每輪 ```end``` 指標移動一格納入一個新的字元，在納入新字元後，檢查新字元是否已經存於 Window 中。

若有，則更改  ```start```  位置，將指標移動到第一個重複字元的下一個位置。若無，則  ```start```  停留在原位，最後記錄總長度並結束此輪。直到字串輪巡結束後回傳最長長度。

<br class="big">

理論上是這樣啦，不過實做上為了效能考量這邊使用了 <mark>HashMap</mark> 來記錄，實做步驟如下：

1.  初始化一個 HashMap _indexes_ ，用以記錄目前出現過的字元，與其對應的下標。

2.  開始輪巡整個字串，每一輪讀入一個新的字元，若該字元存於 _indexes_，則判斷記錄的下標是大於 _start_ ，若大於 _start_ 則更新 _start_ 所指向的位置。

3.  每一輪結束時，記錄最長的長度並更新新字元於 _indexes_ 所記錄的下標。


```python
class Solution:
   def lengthOfLongestSubstring(self, s: str) -> int:
      start = 0
      max_len = 0
      indexes = {}

      for idx, letter in enumerate(list(s)):
         if letter in indexes and start <= indexes[letter]:
            start = indexes[letter] + 1

         max_len =   max(idx-start+1, max_len)
         indexes[letter] = idx

      return max_len 
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)