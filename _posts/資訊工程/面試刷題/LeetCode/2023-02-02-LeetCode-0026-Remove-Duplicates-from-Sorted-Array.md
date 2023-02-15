---
title: 【LeetCode】0026. Remove Duplicates from Sorted Array
date: 2023-02-15
is_modified: false
categories:
- "面試刷題"
tags:
- LeetCode
--- 

Given an integer array nums sorted in **non-decreasing order**, remove the duplicates [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do **not** allocate extra space for another array. You must do this by **modifying the input array** [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with O(1) extra memory.


<!--more-->
<br>

**Custom Judge:**

The judge will test your solution with the following code:
```python
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```
If all assertions pass, then your solution will be **accepted**.

<br>

**Example 1:**
```python
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

<br>

**Example 2:**
```python
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

<br>

**Constraints:**

- 1 <= `nums.length` <= 3 * 104
- -100 <= `nums[i]` <= 100
- nums is sorted in **non-decreasing** order.

<br>

**Related Topics:** `Array` `Two Pointers`


## 解題邏輯與實作
這題是要我們從 Array 中去除重複項。這邊我第一個想法是使用 for 迴圈，我另外用了兩個指標 `compar_idx` 與 `replace_idx` 以記錄目前的位置，並用 for 迴圈尋訪 Array 內容。一旦目前被尋訪到的值與比較值（`compar_idx` 所指的值）相同，就將該值指定給取代值（`replace_idx` 所指的值），完成後就將 `compar_idx` 移到目前位置且 `replace_idx` 也向前移動一步：

```python
def removeDuplicates(nums):
    if not nums:
        return 0
    
    length = len(nums)
    if length <= 1:
        return length

    compar_idx = 0
    replace_idx = 1
  
    for i in range(1,length): 
        if nums[compar_idx] != nums[i]:
            nums[replace_idx] = nums[i]
            compar_idx = i
            replace_idx += 1
    return replace_idx
```
出來效果還行 **83 ms，93.30%**。


## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-02-15</summary>
  <ul>
    <li>2023-02-15 發布</li>
    <li>2023-02-02 完稿</li>
    <li>2023-02-02 起稿</li>
  </ul>
</details>
