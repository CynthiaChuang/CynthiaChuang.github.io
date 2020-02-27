---
title: 【LeetCode】0025. Reverse Nodes in k-Group
date: 2019-01-07
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given a linked list, reverse the nodes of a linked list  _k_  at a time and return its modified list.

_k_  is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of  _k_  then left-out nodes in the end should remain as it is.

<!--more-->
**Example:**
```python
Given this linked list:  1->2->3->4->5

For k  = 2, you should return:  2->1->4->3->5
For k  = 3, you should return:  3->2->1->4->5
```

<br>

>**Note:**
>-   Only constant extra memory is allowed.
>-   You may not alter the values in the list's nodes, only nodes itself may be changed.

<br>

**Related Topics:**`Linked List`

<br><br>

## 解題邏輯與實作

這題是 [Swap Nodes in Pairs](/LeetCode-0024-Swap-Nodes-in-Pairs/) 的進階題，不同於上一題是兩兩交換，這一題是以每 k 個節點為一組翻轉鍊結陣列。
 
不過這題在實做時，卡了我老久，主要是我試圖在一個迴圈把所整個翻轉過程，連同頭尾的指標一併搞定，所以寫起來超級的不順手。後來在[這一篇](https://shenjie1993.gitbooks.io/leetcode-python/025%20Reverse%20Nodes%20in%20k-Group.html)中看到它的整理，這才總算讓我的思考清晰了許多：
```
現在有一陣列為 A->B->C->D->E ，現在我們要翻轉 BCD 三個節點，其步驟如下：
1. C->B
2. D->C
3. B->E
4. A->D

整的步驟其實分成兩個部份:
1. 先把 k 個節點翻轉，也就是步驟 1 與 2
2. 再將這 k 個節點與前後連起來，步驟 3 與 4。 
```


```python
class Solution(object):
  def __init__(self,k=None):
    self.k = None
		
  def reverseKGroup(self, head, k):
    self.k = k
    if not head or self.k <= 1:
      return head

    dummy_head = ListNode(-1)
    dummy_head.next = head
    iteration = dummy_head

    while self.hasNextK(iteration):
      iteration = self.reverseNextK(iteration)

    return dummy_head.next

  def hasNextK(self, head):
    has = True
    tmp = head
    for i in range(self.k):
      if tmp.next is None :
        has = False
        break
      tmp = tmp.next

    return has

  def reverseNextK(self, head): 
    pre = head
    current = head.next
    group_end = head.next

    for i in range(self.k):
      next_node = current.next
      current.next = pre
      pre, current  = current, next_node

    head.next = pre
    group_end.next = current

    return group_end
```


<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)