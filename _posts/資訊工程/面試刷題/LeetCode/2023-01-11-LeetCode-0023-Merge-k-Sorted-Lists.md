---
title: 【LeetCode】0023. Merge k Sorted Lists
date: 2023-02-15
is_modified: false
categories:
- "面試刷題"
tags:
- LeetCode
--- 

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

<!--more-->
<br>

**Example 1:**
```
nput: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

<br>

**Example 2:**
```
Input: lists = []
Output: []
```

<br>

**Constraints**:
- k == lists.length
- 0 <= k <= 104
- 0 <= lists[i].length <= 500
- -104 <= lists[i][j] <= 104
- lists[i] is sorted in ascending order.
- The sum of lists[i].length will not exceed 104.


<br>

**Related Topics:** `Linked List` `Divide and Conquer` `Heap (Priority Queue)`  `Merge Sort`


## 解題邏輯與實作
之前有解過寫過一篇 [0021. Merge Two Sorted Lists](/LeetCode-0021-Merge-Two-Sorted-Lists)，這題就是它的進階。

我打算按照提示中的分而治之（Divide and Conquer）法與遞迴不斷將鏈結串列剖半對分，直到剩下一個或是兩個鏈結串列時，開始合併與回傳。

以長度為 6 的鏈結串列 [A, B, C, D, E, F] 為例，用遞迴去剖半對分的話最終會是 A、B 排，AB 排後再跟 C 排，再 D、E 排，DE 排後再跟 F 排，最後 ABC 與 DEF 再排一次，總共執行了 5 次。

```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:    
        return self.merged(lists, 0, len(lists))
    
    def merged(self, lists: List[Optional[ListNode]], start_idx: int, end_idx: int) -> Optional[ListNode]:
        len_list = end_idx-start_idx;
        if len_list <= 0:
            return None
  
        merged_result = lists[start_idx]
        if len_list > 1:
            k = round(len_list/ 2);
            merged_result = self.mergeTwoLists(self.merged(lists, start_idx, start_idx+k), 
                                               self.merged(lists, start_idx+k, end_idx))
      
        return merged_result    

    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        answer = ListNode(-1)
        head = answer

        while list1 and list2:
            if list1.val < list2.val:
                answer.next = list1
                list1 = list1.next    
            else:
                answer.next = list2
                list2 = list2.next
            answer = answer.next

        answer.next = list1 if list1 else list2

        return head.next 
```
Runtime 為 112 ms，Beats 是 78.87%。不過它效率比我想像中的還差。

<br class="big">

因此我打算換個寫法試試，一樣是分而治之法，不過把它改成直接兩兩抓來做排序，就是 A、B 排、C、D 排、C、D 排：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        len_list = len(lists)

        if len_list == 0:
            return None
      
        resules = lists        
        while len_list > 1:
            if len_list % 2 != 0:
                resules.append(None)
                len_list += 1
                
            new_resule = []            
            for i in range(0, len_list, 2):
                new_resule.append(self.mergeTwoLists(resules[i], resules[i+1]))
        
            resules = new_resule
            len_list = len(resules)
            
        return resules[0]    
 
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        answer = ListNode(-1)
        head = answer

        while list1 and list2:
            if list1.val < list2.val:
                answer.next = list1
                list1 = list1.next    
            else:
                answer.next = list2
                list2 = list2.next
            answer = answer.next

        answer.next = list1 if list1 else list2

        return head.next 
```
效果出來出乎意料的不錯欸，Runtime 96 ms、Beats 95.64%。

<br class="big">

偷看了下 70ms 的答案，因為它直接用 sort 難怪效能不錯 XDDD

```python
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        head = temp = ListNode()

        arr = []
        for l in lists:
            while l != None:
                arr.append(l.val)
                l = l.next
        arr.sort()
        for a in arr:
                temp.next = ListNode()
                temp = temp.next
                temp.val = a       
        
        return head.next
```


## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-02-15</summary>
  <ul>
    <li>2023-02-15 發布</li>
    <li>2023-01-11 完稿</li>
    <li>2023-01-11 起稿</li>
  </ul>
</details>

