---
title: 【LeetCode】0208. Implement Trie (Prefix Tree) 
date: 2018-12-21
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Implement a trie with  ``insert``,  ``search``, and  ``startsWith``  methods.
<!--more-->
<br>

**Example:**
```python
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```
<br>


> **Note:**
> -   You may assume that all inputs are consist of lowercase letters  `a-z`.
> -   All inputs are guaranteed to be non-empty strings.

<br>

**Related Topics:**`Trie`

<br><br>

## 解題邏輯與實作
相見恨晚的一題阿，我面試時就是考這題的概念，可惜太久沒寫 Tree ，也沒用 Python 寫過 Tree，面試實作時超級卡的Orz

需要注意的是，在實作與樹不一樣的是，節點本身不需要儲存字元，而是使用節點的連線來代表字母，換句話說在父節點儲存子節點時，使用 mapping (其 Key 值為字元，Value 為子節點)方式來儲存，會比較方便操作。另外節點本需要額外變數紀錄樹否為某一單字結尾。

基本上`search` 與 `startsWith` 是在做同樣的事情，只是 `search` 在回傳時需多判斷該結果是否成詞。

實做步驟：
1. insert 
	1. 將要輸入的單字變成 char stream
	2.  將 char stream 由根節點往深度方向尋訪，依序建出子樹
		- 根節點為空字元，不應該寫入任何字元
		- 若該節點為單字結尾，該節點應加上標記
2. search
	1. 將字串作為 char stream 由根節點往深度方向尋訪
		-  若該字元不存在下一字元的子節點，則回傳False
		-  反之，則繼續向下搜尋，直到所輸入字字串搜尋完畢
	2. 若該字串存在於字典樹中，且最後一個節點有被標記為單字結尾，則回傳 True，反之 False
3. startsWith
	1. 實作同 search ，唯回傳時僅需考慮該字串是否存在於字典樹中，無需考慮是否成詞。


```python
## 用defaultdict單純只是我懶的判斷在dict中key值存不存在
from collections import defaultdict
class Node:
    def __init__(self):
        self.childs = defaultdict(Node)
        self.is_word_end = False        

class Trie:
    def __init__(self): 
        self.root = Node()

    def insert(self, word):
        current = self.root
        for char in word:
            current = current.childs[char]
        current.is_word_end = True

    def find(self, word):
        current = self.root
        is_exist = True
        for char in word:
            current = current.childs.get(char)
            if current == None:
                is_exist = False
                break
        return is_exist, current
        
    def search(self, word):
        is_exist, current = self.find(word)	
        return is_exist and current.is_word_end 

    def startsWith(self, prefix):
        is_exist, _ = self.find(prefix)
        return is_exist
```
<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)

<br><br>

## 參考資料 
1. [大量 String 資料結構 : Trie｜演算法筆記](http://www.csie.ntnu.edu.tw/~u91029/String.html#6)
2. [Trie树（Prefix Tree）介绍｜神奕的专栏 -  CSDN博客](http://blog.csdn.net/lisonglisonglisong/article/details/45584721)
3. [Solution -Implement Trie (Prefix Tree)｜LeetCode](https://leetcode.com/problems/implement-trie-prefix-tree/solution/)



 