---
title: 【LeetCode】0001. Two Sum
date: 2018-12-19
categories:
- Interview/Problemset
tags:
- LeetCode
---

Given an array of integers, return  **indices**  of the two numbers such that they add up to a specific target.

<!--more-->

You may assume that each input would have  **_exactly_**  one solution, and you may not use the  _same_  element twice.

**Example:**
```python
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
<br>

**Related Topics:** `Hash Table`、`Array`

<br><br>

## 解題邏輯與實作
這一題算是 LeetCode 中的 Hello World 吧，網路上還流傳著**平生不識 TwoSum，刷盡 LeetCode 也枉然**這句話呢，可見這題是多麼的廣為人知(?)

簡單來說這題是要從給定陣列中找出兩個數字，其總和為給定的 target，最後還這兩個數字的索引值。每題必定只有一個解，且每個數字並不會被重述使用。

<br>

### 暴力法
最直覺想到的當然是暴力法，但果不其然 **Time Limit Exceeded**，畢竟用暴力法的時間複雜度是 $O(n^2)$

```python
class Solution:
  def cal_sum(self, num_1 , num_2):
    return num_1 + num_2

  def twoSum(self, nums, target):
    size = len(nums)
    for idx1 in range (size) :
      num_1 = nums[idx1]

    for idx2 in range(idx1+1 ,size):
      num_2 = nums[idx2]

      s = self.cal_sum(num_1,num_2)
      if s == target:
        return [idx1,idx2]

      raise RuntimeError("No two sum solution") 
```
<br>

### 雜湊表（Hash table)
既然暴力法時間複雜度暴表，那就拿空間換取時間吧！另外設立一個 HashMap 儲存走訪過得結果，當一個數字進來後去檢查，差值是否存在 HashMap 中，若存在則回傳結果，實做步驟如下：
1. 讀入陣列中一值 _n_，並計算其差值 _diff_ = _target_ - _i_
2. 檢查差值 diff ，是否存在於 HashMap 中，
	1. 不存在，將 _n_ 與所對應的索引值作為 key–value pair，存入 HashMap 中。
		- 因為，題目要求最後回傳的結果是兩個數字的索引值，所以必須一併紀錄索引值。
	2. 存在，則取出差值 diff 所對應的索引值，與當前索引值合併成一個陣列回傳。

```python
class Solution:
  def twoSum(self, nums, target):
    keys = {}
    for idx, value in enumerate(nums):
      diff = target - value
      if diff in keys:
        return [keys[diff], idx]

      keys[value] = idx
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)