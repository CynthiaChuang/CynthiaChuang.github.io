---
title: 【面試】簡易實做 AutoComplete - Python
date: 2019-01-03
categories:
- Interview/Problemset
tags:
- LeetCode
- Python
- Trie
- AutoComplete
--- 

這大概是我有經驗來最神奇的面試經驗了(●▼●;)
面試時間一個小時，基本上就是**就一個人寫code，另外一個人一直看著寫code..XD**。想當然爾，面試結果想當然是被考官狠狠洗臉了QAQ
 
![就一個人寫code，另外一個人一直看著寫code](https://i.imgur.com/vM4ReDR.jpg)
<center class="imgtext">  （圖片來源:   <a href="https://office12.gr/events/practice-a-new-programming-language-hscc/"  class="imgtext">Heraklion Innovation Map</a>）</center>
  
<br> 
大概會好一陣忘不了這次面是的考題— ``AutoComplete`` 吧...

<!--more-->
<br>

## Ultimate Autocomplete
那天面試官一開始就直接跳過自我介紹，端主菜了上個道 Design and Implement Question 給我 - 實做一個 **Ultimate Autocomplete**。

<div class="alert info">
<div class="head">Ultimate Autocomplete</div>
在最短的時間和空間內以良好的順序輸出預測的單詞列表，在理想情況下，列表的第一個單詞是正確的選擇。
</div>

 
<br><br>

##  概念陳述

  ![trip](https://cdn-images-1.medium.com/max/1600/1*jL2Rc-EpEmNZII552xX7Ig.jpeg)
 
<br>在寫程式碼之前，面試官要求先用文字寫下演算法後再進行實作。
聽到這個題目，第一反應是使用 [ <span class="highlighting">字典樹(Trie)</span>](https://zh.wikipedia.org/wiki/Trie) 實做，先用目前使用者的輸入做為 prefix，進行搜尋找出所有以該詞為字首的單詞，再將取出的結果進行排序。

<br>實作分幾個階段
1. 將字典中的單字建成字典樹
	1.  從字典取出每個單字變成 char stream
	2.  將 char stream 由根節點往深度方向尋訪，依序建出子樹
		- 根節點為空字元，不應該寫入任何字元
		- 若該節點為單字結尾，該節點應加上標記，並寫入額外資訊，例如：詞頻、詞性...等
	3.  重複步驟 1.1 與 1.2，直到字典中所有單字都建立完畢。<br>
<br>

2. 輪巡字典樹，找出所有符合的清單
	1.   將使用者的輸入做為 prefix，進行搜尋找出目前所在節點
	2.   使用深度優先，尋訪該節點的所有子樹
	3.   回傳尋訪結果
          結果包含完整單字（即由根節點到被標記為單字結尾節點的路徑所組成的字串）、詞頻、詞性...等額外資訊。<br>
<br>          
          
3. 將結果進行排序後輸出
	- 排序時，可再加入如歷史資訊等訊息，使預測結果更符合使用者的需求
	- 回傳結果應去除額外資訊

<br><br>

## 實作

```python
from collections import defaultdict
class Node:
	def __init__(self):
		## 簡單起見，額外資訊僅記錄詞頻
		self.childs = defaultdict(Node)
		self.is_word_end = False
		self.frequency = 0

class Trie:
	def __init__(self):
		self.root = Node()

	def insert(self, word, freq):
		word = word.lower()
		
		current = self.root
		for char in word:
			current = current.childs[char]
		
		current.is_word_end = True
		current.frequency = freq

	def complete(self, prefix):
		prefix = prefix.lower()
		
		current = self.root
		for char in prefix:
			current = current.childs.get(char)
			if current == None:
				return  []

		results = self.find(current,list(prefix),[])
		results.sort(key=lambda x: x[1],reverse=True)
		return  [w for w,f in results]

	  
	def find(self, node, prefix, results):
		if node.is_word_end:
			results.append(("".join(prefix),node.frequency))
	 
		if len(node.childs) <= 0  :
			return results

		for char, next_node in node.childs.items():
			prefix.append(char)
			results = self.find(next_node,prefix,results)
			prefix.pop(-1)

		return results

trip = Trie()
words = [('this',40),  ('that',80),  ('there',70),  ('what',60),  ('where',50),  ('when',11)]

for w,f in words:
	trip.insert(w,f)

prefix = 'whe'
suggest_words = trip.complete(prefix)

print(suggest_words)
```

<br><br>

## 感想
果然靜下心來寫，陳述的會比較有條理些... 為啥面試的時候可以卡成那樣 Orz
然後 LeetCode 要乖乖刷阿，這次時間太趕根本來不及準備，[LeetCode No. 208](https://leetcode.com/problems/implement-trie-prefix-tree/) 是建 Trie 的阿阿阿阿