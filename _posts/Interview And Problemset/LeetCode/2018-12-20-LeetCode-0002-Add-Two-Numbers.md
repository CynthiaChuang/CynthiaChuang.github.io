---
title: 【LeetCode】0002. Add Two Numbers
date: 2018-12-20
categories:
- Interview/Problemset
tags:
- LeetCode
- Python
- Linked List
--- 

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
<!--more-->

**Example:**
```python
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

<br><br>

## 解題邏輯與實作
這題不算難，就是兩個數字做相加，進位的話向後放，需要注意的就是兩個數字長度不相同以及最高位進位的情況。

比較討厭的大概就是很久沒有寫鏈結陣列了，操作起來有點討厭。奇怪，為啥以前我會很喜歡寫鍊結陣列呢 (*´･д･)?
<br>

實做步驟如下：
1. 新增一個 fake 的 head 節點，並將 end 也指向這個節點。

2. 依序取出 input 的鍊結陣列的節點與前一個位數的進位進行相加。
	1. 若相加結果有進位，則紀錄進位數
	2. 否則，將進位數計為0

3. 將計算結果個位數使用一個新的節點，添加到新的鍊結陣列後，並將 end 指到這個節點。

4. 返回第二步，繼續計算，若鍊結陣列取出的節點不存在，則以 0 進行計算，直到兩陣列都計算完畢。


5. 計算結束後回傳 head 的下一個節點。
<br>

```python
class Solution:
  def addTwoNumbers(self, l1, l2):
    carry = 0
    result = end = ListNode(0)        
    while l1 or l2 or carry:
      v1 = l1.val if l1 else 0
      v2 = l2.val if l2 else 0
      carry, val = divmod(v1 + v2 + carry, 10)
                        
      l1 = l1.next if l1 else l1
      l2 = l2.next if l2 else l2
      end.next = ListNode(val)
      end = end.next
            
      return result.next
```


<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)