---
title: 【LeetCode】0140. Word Break II
date: 2018-12-21
categories:
- Interview/Problemset
tags:
- LeetCode
- Python
- Dynamic Programming
- Backtracking
--- 

Given a  **non-empty**  string  _s_  and a dictionary  _wordDict_  containing a list of  **non-empty** words, add spaces in  _s_  to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.
<!--more-->
<br>

> **Note:**
> -   The same word in the dictionary may be reused multiple times in the segmentation.
> -   You may assume the dictionary does not contain duplicate words.

<br>

**Example 1:**
```python
Input: s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output: [
  "cats and dog",
  "cat sand dog"
]
```

**Example 2:**
```python
Input: s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output: [
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.
```

**Example 3:**
```python
Input: s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: []
```

<br><br>
## 解題邏輯與實作
第二個例子，腦中瞬間出現這個旋律（想被洗腦的[這邊走](https://www.youtube.com/watch?v=Ct6BUPvE2sM)），上一題還沒注意到的說XDDD

![enter image description here](https://jvtea44x1mh3gctjp1q2nymo-wpengine.netdna-ssl.com/wp-content/uploads/2016/09/Think-Marketing-Pen-Pineapple-Apple-Pen-1200x630.jpg)

忽然覺得他的眼神有點鄙視......是我錯覺嗎( ˘•ω•˘ ) 



<br>

### 遞迴
好了，先別管古坂大魔王了，LeetCode 大魔王處理先。像這種找所有可能的題目，首先想到的是遞迴，更別說我在寫程式的時候，第一 round 偏好用暴力法，暴力法的雖然效能都不是很好，但它的寫法較為簡潔，適合拿來釐清思路，等寫出一個可以 round 的版本，我才會開始做效率的改進。這種作法有好有壞啦，端看大家的習慣怎樣。

基本上在做的時候還是在拆、拆、拆， 如果字串前半有在字典當中，就把後半部傳入遞迴再做拆解，如果後半也拆的開，就回傳回去作文文字的拼接。

```python
from collections import defaultdict
class Solution(object):
    def __init__(self):
        self.memo  = defaultdict(list)
        self.mydict = None

    def set_dict(self,mydict):
        self.mydict = set(mydict)

    def wordBreak(self, s, wordDict):
        self.set_dict(wordDict)
        return self.recursive(s)		

    def recursive(self,s):
        if not s:
            return [None]

        if s in self.memo:
            return self.memo[s]

        res = []
        for word in self.mydict:
            n = len(word)
            if s[:n] != word:
                continue
                
            for r in self.recursive(s[n:]):
                result = word + " " + r if r is not None else word
                res.append(result)
                
        self.memo[s] = res
        return res
```

<br>
### DAG
話說在寫這題的時候，越寫越覺得超級像中文斷詞的，只是不需要做到最後一步，在中間就可以回傳中間產物了。以 [Jieba](https://github.com/fxsjy/jieba) 斷詞演算法（雖然我慣用的斷詞API不是 Jieba XDDD）來說，他在第一個步驟會建立 Trie 與 DAG 資料結構，快速算出所有合法的切分組合，就只需要做到這裡就好，第二部份的統計模型計算可以先忽略它。

我有按照個想法時做了一版，但...它目前還只是個屍體而已...Orz

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)