---
title: 【LeetCode】0189. Rotate Array
date: 2018-12-21
is_modified: false
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given an array, rotate the array to the right by  _k_  steps, where _k_ is non-negative.
<!--more-->
<br>

**Example 1:**
```
Input: [1,2,3,4,5,6,7] and k = 3
Output:[5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5] 
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

**Example 2:**
```
Input:[-1,-100,3,99] and k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```
<br>

> Note:
> 1. Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
> 2. Could you do it in-place with O(1) extra space?

<br>

**Related Topics:**`Dynamic Programming`、`Backtracking`

<br><br>

## 解題邏輯與實作
這提示要求將 array 中的數字右旋 _k_ 位，簡單來說，就是將所以的數字向後移動 _k_ 位，而末尾 out of index 的部份則移到開頭。

這題說麻煩的部份是它最後希望我們提出三種不同實做的方法，例外提供一種只需要 _O(1)_ 空間的解法，會花比較多時間。

<br>

### 解法1 
最先想的解法，就是使用 for 迴圈執行 _k_，每次迴圈執行時，記住最後一個數字後，將其他數字向後位移一位，再將所記住的數字放回開頭。

這種解法相當的直覺，算是暴力法了吧(笑，這種解法的空間複雜度會符合 _O(1)_ 的要求，但時間複雜度應該會相當的高 _O(kn)_。為了減少執行次數，我將執行次數 _k_ 對 _n_ 取餘數，因為當 _k_ = _n_ 時，相當於沒有做旋轉。 


```python
class Solution:
  def rotate(self, nums: List[int], k: int) -> None:
    n = len(nums)
    k %= n
    for i in range(k) :
      final = nums[n-1]
      for idx in range((n-1)-1,-1, -1):
        nums[idx+1] = nums[idx]

      nums[0] = final

```

另外，題目有要求 in-place instead ，不然寫成這樣易讀性會更好

```python
nums = [nums[n-1]] + nums[:-1]
```

最後果不其然 **Time Limit Exceeded**。

<br>

### 解法2
另外想到基於解法1的解是直接將前 _n-k_ 個向後移動 _k_ 位，最後在將對後 _k_ 移動到開頭。這樣時間複雜度會降成 _O(n)_ ，但空間複雜度也會變成 _O(n)_ 。

```python
class Solution:
  def rotate(self, nums: List[int], k: int) -> None:
    n = len(nums)
    k %= n

    final = nums[n-k:]
    for idx in range((n-k)-1,-1, -1):
      nums[idx+k] = nums[idx]

    for idx in range(len(final)):
      nums[idx] = final[idx]
```

這次就被 accept 了，不過我一個月沒上來 LeetCode ，還多一個 Memory Usage 的比較耶～！
**Runtime: 68 ms, faster than  46.18%  of  Python3  online submissions forRotate Array.
Memory Usage: 13.4 MB, less than  5.04%  of  Python3  online submissions for  Rotate Array.**

<br>

### 解法3
第三個解法是從網路上找來的，有點類似翻轉 String 的方法，它先將前 _n-k_ 個數字翻轉，再把後 _k_ 個數字翻轉，最後將整個陣列都翻轉過來，就可以得到答案了。

這種解法的時間複雜度為 _O(n)_ ，但空間複雜度可以降到 _O(1)_。個人覺得還蠻神奇的一個解法，滿佩服想到這種解法的人！

**Example _k = 3_**  
1, 2, 3, 4, 5, 6,  7  
**4, 3, 2, 1,** 5, 6, 7  
4, 3, 2, 1, **7, 6, 5**  
**5, 6, 7, 1, 2, 3, 4**  


```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        k %= n

        self.reverse(nums, 0, (n-k)-1)
        self.reverse(nums, n-k, n-1)
        self.reverse(nums, 0, n - 1)

    def reverse(self, nums, start, end):
        while start < end :
            nums[start], nums[end] = nums[end],  nums[start]
            start += 1
            end -= 1
```
<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)