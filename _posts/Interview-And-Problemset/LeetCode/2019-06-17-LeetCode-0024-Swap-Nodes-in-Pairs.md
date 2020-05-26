---
title: 【LeetCode】0024. Swap Nodes in Pairs
date: 2019-06-17
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given a linked list, swap every two adjacent nodes and return its head.

<!--more-->
**Example:**
```python
Given `1->2->3->4`, you should return the list as `2->1->4->3`.
```
<br>

> **Note:**
> -   Your algorithm should use only constant extra space.
> -   You may  **not**  modify the values in the list's nodes, only nodes itself may be changed.

<br>

**Related Topics:**`Linked List`

<br><br>

## 解題邏輯與實作
這題是要以兩個節點為一組翻轉鏈結陣列，難度不高，但很容易把自己搞暈頭轉向，建議先畫圖輔助會比較清楚。

<br>

## 迭代
這個的想法比較簡單，就是兩兩指標交換，不過在交換時需要注意下交換的順序，不然很容易把指標搞丟了。

```python
class Solution:
  def swapPairs(self, head):
    dummy_head = ListNode(-1)
    dummy_head.next = head
    iteration = dummy_head

    while iteration.next and iteration.next.next :
      node1 = iteration.next
      node2 = iteration.next.next

      iteration.next = node2
      node1.next = node2.next
      node2.next = node1

      iteration = iteration.next.next

    return dummy_head.next
        
```

<br>

## 遞迴
想說迭代做的到的，應該也可以用遞迴來實做。實做時，先交換最後兩個，再依序向前交換。

```python
class Solution:
  def swapPairs(self, head):
    if not head or not head.next:
        return head

    node = head.next
    head.next = self.swapPairs(head.next.next)
    node.next = head

    return node        
```

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)