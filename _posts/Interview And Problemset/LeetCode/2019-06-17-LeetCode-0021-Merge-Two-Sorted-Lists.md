---
title: 【LeetCode】0021. Merge Two Sorted Lists
date: 2019-06-17
categories:
- Interview/Problemset
tags:
- LeetCode
- Python
- Linked List
--- 

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

<!--more-->
<br>

**Example 1:**
```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

<br><br>
## 解題邏輯與實作
因為題目給的是兩個有序的鏈結陣列，所以這題只須兩個指針分別指向給定的陣列，將節點值較小的添加到新的鏈結陣列，添加完後將指標移往下一個節點，直到兩鏈結陣列的節點都被添加到新的鏈結陣列為止。


```python
class Solution:
  def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
    dummy = ListNode(-1)
    head = dummy

    while l1 and l2:
      if l1.val < l2.val:
        head.next = l1
        l1 = l1.next    
      else:
        head.next = l2
        l2 = l2.next
        
      head = head.next

    head.next = l1 if l1 else l2
    return dummy.next
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/interview/problemset/2018/12/19/LeetCode-0000-Contents/)