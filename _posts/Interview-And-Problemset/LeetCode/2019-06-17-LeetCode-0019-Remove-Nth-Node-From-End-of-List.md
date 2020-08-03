---
title: 【LeetCode】0019. Remove Nth Node From End of List
date: 2019-06-17
modified: 2019-06-17
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Given a linked list, remove the _n_-th node from the end of list and return its head.

<!--more-->

**Example 1:**
```
Given linked list: 1->2->3->4->5, and _n_ = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```
<br>

> **Note:**
> Given  _n_  will always be valid.

<br>
>**Follow up:**
>Could you do this in one pass?

<br>

**Related Topics:**`Linked List`、`Two Pointers`

<br><br>

## 解題邏輯與實作
這題讓我們移除鏈結陣列中倒數第 N 個節點，且題目中限定了 N 是一定是有效的，也就是說 N 一定是鏈結陣列中的節點，因此可以省略些檢查步驟。

<br>

### remove
一開始的想法很簡單，當尋訪到指定的 node 時就將它跳過，但因為題目的 n 指的是從後方數來，而單向的鏈結陣列只能從前向後走，因此必須先計算要移除的 node 從前方數來的 index，也就是 $len(鏈結陣列) - n$。

想法很簡單...唯一的問題就是...鏈結陣列沒有長度相關的 method ... Orz

所以只好先尋訪一次，得出鏈結陣列總長度，並計算出所需移除的 index。第二次尋訪時，再進行移除。


```python 
class Solution:
  def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
    if not head: 
      return head	

    dummy = ListNode(-1)
    dummy.next=head
    result = dummy

    tmp = head
    head_len = 0
    while tmp:
      head_len += 1
      tmp = tmp.next

    remove_idx = head_len-n 
    idx = -1
    while result:
      idx += 1
      if idx == remove_idx: 
        result.next = result.next.next
        break
      result = result.next

    return dummy.next
```

<br><br>

只是上面的作法，並不符合 **Follow up** 的要求，而且效率也並不是很突出，*~~重點是程式碼好醜~~*。所以又進一步使用 dict 優化，這邊先將 node 本身與對應的 index 放入 dict，之後即得出鏈結陣列長度，且可用 dict 操作元素的方式來進行 node 移除。<br>


```python 
class Solution:
  def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
    if not head: 
      return head	

    dummy = ListNode(-1)
    dummy.next = head

    idx = 0
    idx_to_node = {-1: dummy}

    while head :
      idx_to_node[idx] = head
      head = head.next
      idx +=1


    head_len = len(idx_to_node)-1
    if head_len == 1:
      return None

    remove_idx = head_len - n 
    if remove_idx == head_len-1:
      idx_to_node[remove_idx-1].next = None
    else:
      idx_to_node[remove_idx-1].next = idx_to_node[remove_idx+1]

    return dummy.next
```

<br>執行結果顯示第二版的效能（36 ms /  91.17%），也比第一版有所提昇（40 ms / 77.72 %）。

<br>

### Two Pointers

不過這題如果去網路查查，看到最多的會是 **Two Pointers** 的作法。

分別定義兩個指標 prev 與 cur，一開始由 prev 先尋訪 n 個 node，如果走完 n 個 node 後尚把未整個鏈結陣列給尋訪完的話，就讓 prev 繼續向下尋訪，且讓 cur 也開始尋訪，直到 prev 尋訪整個鏈結陣列後 cur 也停止尋訪。

此時，cur 會指向要移除 node 的前一個位置，修改 cur 的 next 指標，讓它跳過要移除的 node。

```python 
class Solution:
  def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
    if not head:
      return head
      
    dummy = ListNode(-1)
    dummy.next=head
    prev = dummy
    cur = dummy
    while prev and n >= 0:
      prev = prev.next
      n -= 1 
            
    while prev:
      prev = prev.next
      cur = cur.next
            
    cur.next = cur.next.next
    return dummy.next
```


<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)