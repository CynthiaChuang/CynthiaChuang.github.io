---
title: 【LeetCode】0031. Next Permutation
date: 2019-01-10
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Implement  **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

<!--more-->
The replacement must be  **[in-place](http://en.wikipedia.org/wiki/In-place_algorithm)**  and use only constant extra memory.

<br>

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
```python
1,2,3  →  1,3,2 
3,2,1  →  1,2,3
1,1,5  →  1,5,1
```

<br>

**Related Topics:** `Array`

<br><br>

## 解題邏輯與實作
大概是寫到想睡覺，這題花了稍久的時間在理解題目，昨晚我家附近不曉得是不是有什麼事故，消防車、救護車來來回回的奔波；一早鄰居的狗又在狂吠，一整個沒睡好，現在超想睡的…Orz。

廢話結束，回來看題目吧。這題的基本理解，在以給定的數字所能構成的升序字典中，找出輸入的這組數值的下一個排序。

簡單來說，在使用相同的數字情況下，找目前比這個稍微大一點的數值。另外題目有限制這題沒有回傳值，必須使用 in-place 實做。另外如果輸入的數字已經是最大的了，那輸出應該輸最小的。

舉例來說，如果給定的數字為 [1, 9, 2, 5, 4, 1] ，而它的下一個數字則會是 [1, 9, 4, 1, 2, 5]。我們先找出第一個需要變動的的數字，例如，我們在 [1, 9, 2, 5, 4, 1] 中，先找出第一個需要變動的是 2 ，再從後面找出第一個比 2 還大的數字，也是 4 ，把 2 與 4 交換，剩下的 [5, 2, 1] ，做升序排列，最後得到的數值就為  [1, 9, 4, 1, 2, 5]，詳細過程如下：

[1, 9, **2**, 5, 4, 1]  
[1, 9, **2**, 5, **4**, 1]   
[1, 9, **4**, 5, **2**, 1]   
[1, 9, 4, **5,  2,  1**]   
[1,  9,  4, **1,  2, 5**]  

<br> 那麼還有一個問題：如何找到第一個需要變動的的數字？

觀察一下會發現，以 2 為分界右邊的是呈現一個降序，因為降序的排列本身就是這些組合中最大的，舉例：[5, 4, 1] 本身就是這個中最大的，如果從這邊找某個點來進行交換反而會變小，因次需要找某個分界點，其分界點的右邊是降序，因此當分界點與右邊第一個比分界點還大的數字交換時，才會使整個數值上升。

實作時，建議從後面向前找，從後面找來如果是升序的，就比較下一個數字，直到找到第一個降序的點為止。

<br><br>
整理一下實做步驟：
1. 由後向前搜尋找到第一個降序的點，其 index 為 i，若無則，將整個數列反轉後回傳
2. 找到 index 為 i 降序點右側第一個大於降序點的值，並與降序點交換
3. 將 index 為 i 的右側進行降序排列。

```python
class Solution:
  def nextPermutation(self, nums):
    """
    :type nums: List[int]
    :rtype: void Do not return anything, modify nums in-place instead.
    """

    descending_idx = self.find_descending_point(nums)
    if descending_idx == -1 :
        nums.reverse()
        return 
    
    swap_idx = self.find_more_than(nums,descending_idx)

    nums[swap_idx], nums[descending_idx]= nums[descending_idx], nums[swap_idx]

    nums[descending_idx+1:] = sorted(nums[descending_idx+1:]) 


  def find_more_than(self, nums, target_indx):
    result_indx = -1
    for i in range(target_indx+1,len(nums),1):
        if nums[i] > nums[target_indx] and (result_indx ==-1 or nums[i] < nums[result_indx]):
            result_indx = i

    return result_indx
      

  def find_descending_point(self, nums):
    point = -1   
    for i in range(len(nums)-1,0,-1):
        if nums[i] > nums[i-1] :
            point =  i-1
            break
    return point
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)