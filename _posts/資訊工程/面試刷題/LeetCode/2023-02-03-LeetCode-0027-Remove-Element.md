---
title: 【LeetCode】0027. Remove Element
date: 2023-02-15
is_modified: false
categories:
- "面試刷題"
tags:
- LeetCode
---  

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [in-place](https://en.wikipedia.org/wiki/In-place_algorithm). The relative order of the elements may be changed.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` after placing the final result in the first `k` slots of `nums`.

Do not allocate extra space for another array. You must do this by **modifying the input array** [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with O(1) extra memory.

<!--more-->

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
If all assertions pass, then your solution will be accepted.
```
 

**Example 1:**

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
Example 2:

Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Constraints:**
- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`


**Related Topics:** `Array` `Two Pointers`


## 解題邏輯與實作
這題給我們一個未排序 List 並給定一個數字，要我們移除從 List 中移除該數字。因為題目要求是 in-place，所以必須將合法值全部移到前方後，回傳合法值的總個數。這題目跟感覺跟上一題 [0026. Remove Duplicates from Sorted Array](/LeetCode-0026-Remove-Duplicates-from-Sorted-Array) 很相似，當然實做的目的並不相同，就只是操作很雷同。

因為涉及交換，所以我第一時間想到是左到右遍歷 List，並用一個指標指在最右邊，若遍歷到的值是要移除的值，則將該值與右邊指標的值交換，並將該指標向左移一位，直到兩指標指到同一位置為止。


```python
def removeElement(nums, val) -> int:
    left = 0
    right = len(nums) -1

    while(left <= right):
        if nums[left] == val:
            tmp = nums[right] 
            nums[right] = nums[left]
            nums[left] = tmp
            right -= 1
        else:
            left += 1

    return left
```


## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-02-15</summary>
  <ul>
    <li>2023-02-15 發布</li>
    <li>2023-02-03 完稿</li>
    <li>2023-02-02 起稿</li>
  </ul>
</details>
