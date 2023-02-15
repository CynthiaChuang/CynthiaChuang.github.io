---
title: 【LeetCode】0022.Generate Parentheses
date: 2023-02-15
is_modified: false
categories:
- "面試刷題"
tags:
- LeetCode
--- 

Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

<!--more-->
<br>

**Example 1:**
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

<br>

**Example 2:**
```
Input: n = 1
Output: ["()"]
```

<br>

**Constraints**:
-  1 <= n <= 8

<br>

**Related Topics:** `String` `Dynamic Programming` `Backtracking`



## 解題邏輯與實作
好久沒刷 LeetCode，所以先拿之前寫過，但還沒打成網誌的題目來練練手。

這題會給定一個數字 n，以生成包含 n 個括號的所有正確形式。若要列出所有結果，我第一個想到的會是用遞迴，幾個中止條件也在第一時間就浮出來了：
1. **長度為 2n，且左括號（open parenthesis）個數等於右括號（close parenthesis）個數**  
    這個條件應該是可以輸出正確形式的中止條件，所以這個終止時需要回傳正確的形式。但實作時，我不想多傳入常數 n（因為這個數在遞迴過程中不會變），所以我把條件給反過來使用倒數的方式：「當左括號剩餘個數為零，且右括號剩餘個數亦為零」時終止。
    
2. **右括號剩餘個數少於左括號剩餘個數**  
    假設 n = 3，若目前字串為 `())` ，則左括號剩餘個數則為 2、右括號剩餘個數為 1，遇到這樣的非法形式就直接返回不處理了。

如果兩種終止條件都不滿足就繼續往下走：
1. 若左括號個數仍有剩餘，即不為 0，則輸出一左括號後繼續向下遞迴。
2. 若右括號剩餘個數大於左括號剩餘個數，則輸出一右括號後繼續向下遞迴。

有這些基本條件後就能開始寫 code 了，完整程式碼如下：

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        results = []
        cur_str = ""
        self.generate(cur_str, n, n, results)
        return results


    def generate(self, cur_str: str, open_remaining: int, close_remaining: int, results:list):
        if open_remaining == 0 and close_remaining == 0:
            results.append(cur_str)
            return         
        if close_remaining < open_remaining:
            return

        if open_remaining > 0:
            self.generate(cur_str + "(", open_remaining-1, close_remaining, results)
        if close_remaining > open_remaining:
            self.generate(cur_str + ")", open_remaining, close_remaining-1, results)            
```

Runtime 大約 38 ms，約在 78.88% 左右，我再多加個終止條件試試應該可以加快速度：
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        results = []
        cur_str = ""
        self.generate(cur_str, n, n, results)
        return results

    def generate(self, cur_str: str, open_remaining: int, close_remaining: int, results:list):
        if open_remaining == 0 and close_remaining == 0:
            results.append(cur_str)
            return         
        if open_remaining == 0 and close_remaining > 0:
            cur_str += ')'*close_remaining
            results.append(cur_str)
            return   
        if close_remaining < open_remaining:
            return  
        if close_remaining < 0 or open_remaining < 0:
            return        
        
        if open_remaining > 0:
            self.generate(cur_str + "(", open_remaining-1, close_remaining, results)
        if close_remaining > open_remaining:
            self.generate(cur_str + ")", open_remaining, close_remaining-1, results)            
```
Runtime 有稍微提升來到了 31 ms，約在 95.25% 左右。



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2023-02-15</summary>
  <ul>
    <li>2023-02-15 發布</li>
    <li>2023-01-09 完稿</li>
    <li>2023-01-09 起稿</li>
  </ul>
</details>

