---
title: 【LeetCode】0226. Invert Binary Tree
date: 2018-12-16
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Invert a binary tree.

<!--more-->

**Example:**
```python
Input:

     4
   /   \
  2     7
 / \   / \
1   3 6   9


Output:

     4
   /   \
  7     2
 / \   / \
9   6 3   1

```
<p class="paragraph-spacing"></p>

**Trivia:**  
This problem was inspired by  [this original tweet](https://twitter.com/mxcl/status/608682016205344768)  by  [Max Howell](https://twitter.com/mxcl):

> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.

<p class="paragraph-spacing"></p>

**Related Topics:** `Hash Table`



## 解題邏輯與實作
頗為知名的一題(笑)，但基本上沒什麼難度，只需要互換節點左右兩邊的子樹即可。


### 遞迴
實作上就是採用**後序**（post-order）的方式尋訪整棵樹，並在尋訪父節點時交換左右子樹。

```python
class Solution:
    def invertTree(self, root):
        self.invert(root)
        return root
        
    def invert(self, root):
        if root is None:
            return
        
        self.invertTree(root.left)
        self.invertTree(root.right)
            
        root.left, root.right =  root.right, root.left            
```


### 最速解
我寫這篇就是為了分享這個，在 [Ruby China](https://ruby-china.org/topics/25977) 上看到**如何用最少字元翻轉二元樹**XDDDDD

```python
class Solution:
  def invertTree(self, root):
    print("(╯°□°)╯︵ ǝǝɹʇ ʎɹɐuıq")
    return root
```



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)



## 參考資料 
1. [Invert Binary Tree｜LeetCode](https://leetcode.com/problems/invert-binary-tree/)
2. [Homebrew 作者被 Google 鄙视了… ｜Ruby China](https://ruby-china.org/topics/25977) 



 