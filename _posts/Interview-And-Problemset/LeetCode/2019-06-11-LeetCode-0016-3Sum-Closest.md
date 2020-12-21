---
title: 【LeetCode】0016. 3Sum Closest
date: 2019-06-11
is_modified: false
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given an array `nums` of _n_ integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

<!--more-->
<br>

**Example 1:**
```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

<br>

**Related Topics:** `Array`、`Two Pointers`

<br><br>

## 解題邏輯與實作
這基本上是 [3Sum](/LeetCode-0015-3Sum/) 的變形，唯一不同的不再是回傳陣列，而是回傳最接近的總和。另外數字相同應該可以不用管，所以我先把它拔掉了。

```python
class Solution:
   def threeSumClosest(self, nums: List[int], target: int) -> int:
      assert len(nums) >= 3 , " n >= 3"

      diff = 2**32
      ans = 0

      length = len(nums)
      nums.sort()
      for first_idx, first in enumerate(nums[:-2]):
         if first_idx > 0 and first == nums[first_idx-1]:
            continue

         second_idx = first_idx + 1
         third_idx = length -1	
         
         while(second_idx < third_idx): 
            second = nums[second_idx] 
            third = nums[third_idx]

            summation = first + second + third   
            
            if abs(target-summation) < diff:
               diff = abs(target-summation)
               ans = summation

            if summation < target:
               second_idx += 1
            elif summation > target:
               third_idx -= 1
            else:
               break

      return ans
```

不過這份程式碼的效能有點差，只跑出了 **176 / 27.85%**  的成績，所以我這邊又加上了一些邊界的判斷進行剪枝，加速迴圈的進行。

<br><br> 
這邊加上了兩個條件
1. **在固定首位數字後與剩餘數列中頭兩個數字，計算三數之和。若和大於 target ，則與之前記錄下來答案相比，回傳最接近的 target 的值。**  
這是因為在陣列已排序且首位數字固定的情況下，剩餘數列中頭兩個數字和理應會是最小；且在尋訪的過程中，首位數字會逐漸加大，所以一旦出現首位數字與剩餘數列中頭兩個數字和大於 target 的狀況，其後的和皆會大於 target，且差值會愈來越大。因此一旦出現這狀況，即可停止尋訪。
<br>

2.  **在固定首位數字後與剩餘數列中末兩個數字，計算三數之和。若和小於 target ，直接向下尋訪下一個首位數字。**  
這是因為在陣列已排序且首位數字固定的情況下，剩餘數列中末兩個數字之和理應是最大，因此若此值小於 target，其他和也會小於 target，且差值大於此值。因此僅需記錄目前的狀況後，直接尋訪下一個首位數字即可，無須在考慮目前首位數字的其他組合。


```python
class Solution:
   def threeSumClosest(self, nums: List[int], target: int) -> int:
      assert len(nums) >= 3 , " n >= 3"

      diff = 2**32
      ans = 0

      length = len(nums)
      nums.sort()
      for first_idx, first in enumerate(nums[:-2]):
         if first_idx > 0 and first == nums[first_idx-1]:
                        continue

         summation = first + nums[first_idx+1] + nums[first_idx+2] 
         if summation > target:
            return summation if abs(target- summation) < diff else ans


         summation = first + nums[length-2] + nums[length-1] 
         if summation < target:
            if abs(target- summation) < diff :
               diff = abs(target-summation)
               ans = summation
            continue


         second_idx = first_idx + 1
         third_idx = length -1	

         while(second_idx < third_idx): 
            second =   nums[second_idx] 
            third = nums[third_idx]

            summation = first + second + third   

            if summation == target:
               return summation
            elif summation < target:
               second_idx += 1
            else:
               third_idx -= 1

            if abs(target-summation) < diff:
               diff = abs(target-summation)
               ans = summation

      return ans
```
改良的效果有點出乎意料，比我想像好很多從  **176 / 27.85%**   提升到  **28 / 100%**  。


<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)