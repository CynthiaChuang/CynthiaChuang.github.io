---
title: 【LeetCode】0009. Palindrome Number
date: 2019-06-06
modified: 2019-06-06
categories:
- Interview/Problemset
tags:
- LeetCode
--- 

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

<!--more-->
<br>

**Example 1:**
```
Input: 121
Output: true
```

**Example 2:**
```
Input: -121
Output: false
Explanation: From left to right, it reads -121. 
From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**
```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```
<br><br>

>**Follow up:**
Coud you solve it without converting the integer to a string?

<br>

**Related Topics:**`Math`

<br><br>

## 解題邏輯與實作
既然是判斷回文，那麼直覺作法就是比頭尾是否相同，如果轉字串實做會變得很簡單，所以我就直接做 Follow up 要求了。

<br> 取尾數比較常見使用 mod (%) 即可。但取最高數的頭則比較麻煩，以 20001 為例，想取出最高位數，則必須計算 20001 // 10000 ，而除數 10000，其實可以換成這樣表示 $10^{len(被除數)-1}$ 。但另一個問題是，被除數是 int 沒有 len 這個函數，因此實際計算時改用迴圈計算被除數除以 10 直到小於 10 的次數，即為 len(被除數)-1 的值。

最後比較結束後，會將比較數字（e.g. 20001）去頭尾，並將除數（10000）除以 100 ，因為這個除數是對應比較數字長度，我將比較數字去頭尾，因此除數也必須少兩位。

```python
class Solution:
  def isPalindrome(self, x: int) -> bool:
    if (x < 0) | ((x % 10 == 0 ) & (x != 0)):
      return False

    div = 1
    while x // div >= 10:
      div *= 10

    while x > 0:
      highest = x // div
      lowest = x % 10

      if highest != lowest:
        return False

      x = (x % div) // 10
      div /= 100

    return True		
```
<br> 一開始的判斷是判斷兩種 case：
1. 是否為負數，若是負數則不可能是回文
2. 判斷尾數使否為 0，因為最高位數不可為 0 ，因此若尾數為 0 則不可能是回文，當然必須排除掉 0 本身。


<br><br>
除比頭尾外，另一個想法是翻轉後兩數直接相比，個人推測這種方法的效能應該會比較好

```python
class Solution:
  def isPalindrome(self, x: int) -> bool:
    if (x < 0) | ((x % 10 == 0 ) & (x != 0)):
      return False

    reverse_x = self.reverse(x)
    return x == reverse_x

  def reverse(self, x: int) -> int:
    res = 0;
    while x != 0:
      res = res * 10 + x % 10;
      x //= 10;

    return res
```
<br> 果然效能好很很多，從 **88 ms/60.57 %** 提升到 **56 ms/ 99.10%** 。

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)