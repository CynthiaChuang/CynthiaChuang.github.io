---
title: 【LeetCode】0011. Container With Most Water
date: 2019-06-06
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Given  _n_  non-negative integers  $a_1, a_2, ...,  a_n$ , where each represents a point at coordinate $(i,  a_i)$.  _n_  vertical lines are drawn such that the two endpoints of line  _i_  is at $(i,  a_i)$ and $(i,  0)$. Find two lines, which together with x-axis forms a container, such that the container contains the most water.

<!--more-->
<p class="paragraph-spacing"></p>

> **Note:** You may not slant the container and  _n_  is at least 2.

<p class="paragraph-spacing"></p>

**Example 1:**
```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

<p class="paragraph-spacing"></p>

**Related Topics:** `Array`、`Two Pointers`



## 解題邏輯與實作
給定一組長短不一的隔板，並指定其放置位置，從中挑其中的兩塊板，使得板子之間能裝最多的水。
<p class="paragraph-spacing"></p>

這種題目第一個想法是，使用兩個指針由外向內依序挑選隔板，由於在計算面積時，高  _h_ 的值會取決於兩塊隔板中較矮的一塊。因此當由外相內移動時，因為寬 _w_ 縮短，會移動較短的隔板期望能獲取較高的 _h_ 值，如此才有可能得到較大的容積。

```python
class Solution:
   def maxArea(self, height: List[int]) -> int:
      start = 0 
      end = len(height) - 1
      area = 0

      while(start < end):
         w = end - start
         if height[start] < height[end]:
            h = height[start]
            start += 1 
         else:
            h = height[end]
            end -= 1 
                        
         area = max(area, h*w)

      return area
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)