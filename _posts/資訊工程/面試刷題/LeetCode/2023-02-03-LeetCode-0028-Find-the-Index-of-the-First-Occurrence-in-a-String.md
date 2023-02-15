---
title: 【LeetCode】0028. Find the Index of the First Occurrence in a String
date: 2023-02-15
is_modified: false
categories:
- "面試刷題"
tags:
- LeetCode
--- 

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of haystack.

<!--more-->

**Example 1:**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```

<br>

**Example 2:**
```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
```
 
<br>

**Constraints:**  
- 1 <= haystack.length, needle.length <= 104
- haystack and needle consist of only lowercase English characters.

**Related Topics:** `Two Pointers` `String` `String Matching`


## 解題邏輯與實作
這題看來是要找子字串第一次出現的位置，若無，則回傳 `-1`。最直覺的方法就是從左到右一一比過去，若剩餘長度小於子字串長度就中止。對了，開始前可以加上兩判斷式，若 `haystack` 或 `needle` 長度為 0，直接回傳 -1；若 `needle` 長度大於 `haystack` 也直接回傳 -1。

先照這樣的想法去寫一版：

```python
def strStr(haystack: str, needle: str) -> int:
    h_len = len(haystack)
    n_len = len(needle)
    if h_len < n_len:
        return -1
    if h_len == 0 or n_len ==0:
        return -1

    end_idx = h_len-n_len+1  
    for i in range(0, end_idx):
        for j in range(0, n_len):
            if haystack[i+j] != needle[j]:
                break;
            if j == n_len-1:
                return i
    return -1
```            

是說，如果不是為了寫過程，我應該會直接呼叫 [`find`](https://www.w3schools.com/python/ref_string_find.asp) XDDD

```python
def strStr(haystack: str, needle: str) -> int:
    return haystack.rfind(needle)
``` 

<br>

依稀記得除了暴力搜尋外，應該還有一個從後面比的演算法，到[維基百科](https://zh.wikipedia.org/zh-tw/字串搜尋演算法)查了下，我印想中的應該是 [Boyer-Moore字串搜尋演算法](https://zh.wikipedia.org/wiki/博耶-穆尔字符串搜索算法)。所以我根據 [〈Boyer Moore Algorithm for Pattern Searching〉](https://www.geeksforgeeks.org/boyer-moore-algorithm-for-pattern-searching/)的教學實做了一次，但這份教學應該是 Boyer-Moore 演算法的精簡版本，因為它沒實做好字元位移規則的部份：

```python
def badCharHeuristic(needle: str): 
    NO_OF_CHARS = 256
    n_len = len(needle)
    badChar = [-1]*NO_OF_CHARS  
    for i in range(n_len):
        badChar[ord(needle[i])] = i;
    return badChar

def strStr(haystack, needle):
    h_len = len(haystack)
    n_len = len(needle)
    if h_len < n_len:
        return -1
    if h_len == 0 or n_len ==0:
        return -1

    badChar = badCharHeuristic(needle)
    i = 0
    end_idx = h_len-n_len
    while(i <= end_idx):
        j = n_len-1
        while j>=0 and needle[j] == haystack[i+j]:
            j -= 1
      
        if j<0:
            return i
        else:
            i += max(1, j-badChar[ord(haystack[i+j])])    
    return -1
```


## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-02-15</summary>
  <ul>
    <li>2023-02-15 發布</li>
    <li>2023-02-03 完稿</li>
    <li>2023-02-03 起稿</li>
  </ul>
</details>
