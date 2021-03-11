---
title: 【LeetCode】0005. Longest Palindromic Substring
date: 2019-03-06
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

<!--more-->

**Example 1:**
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**
```
Input: "cbbd"
Output: "bb"
```

<br>

**Related Topics:**`Dynamic Programming`、`String`

<br><br>

## 解題邏輯與實作
這題是計算最長回文子字串。

<br>

### Manacher  演算法
其實這種題目我最愛暴力法，因為最直覺 XDDDD。

以這題來說，每次把一個字元當作回文的中心，並找到已該字元為中心的最長回文子字串，直到所有字元為中心的回文都被找完後，從中找出最長的結果回傳。

但這樣做，不僅時間複雜度高，且在實做還必須分奇偶長度的回文進行討論，有夠麻煩的。
 
<br><br> 後來找到一篇關於最長回文子串的[討論](https://www.zhihu.com/question/40965749)，裡面提到不少針對這個問題的演算法與分析，其中有一個被暱稱為<mark>馬拉車演算法</mark>的 <mark>Manacher's algorithm</mark>，這個方法可以將時間複雜度降到線性，討論中不少人推崇這是計算最長回文子串的最理想方法。

關於~~馬拉車~~ Manacher 演算法的介紹可以看看[這篇](https://segmentfault.com/a/1190000003914228#articleHeader3)，實做這題時我也是基於這篇的。
<br>

實做步驟如下：
1.  先做前處理，在頭尾與兩兩字元中間加上 **#**，另外為了避免處理時的邊界判斷，我在頭尾又分別加上 **\$** 與 **＠**。

2.  判斷目前的 idx 是否在 max_right **左側**，若在左側表示之前已經被接觸過，因此可取 idx 以 center 為對稱軸所對應 idx' 的回文半徑作為 idx 初始的半徑，但其長度不得超過 max_right。因為此時我們只能確認以 center 為中心、半徑為 max_right 的部份是回文。

3.  嘗試以 idx 為中心，並以初始的回文半徑向外擴展回文，直到左右兩邊字元不同，或者到達邊界。

4.  最後，判斷此次擴展是否有觸及更右邊的字元，也就是 idx 加上其回文半徑大於是否有大於 max_right，若有則將 idx 設為 center，並更新 max_right。

5.  直到所有字元的回文半徑計算完畢後，回傳最長的最長回文子字串。


<br>

```python
class Solution:
   def longestPalindrome(self, s: str) -> str:
      if not s:
         return ""
         
      s = "$#" + "#".join(s) + "#@" 
      radii = [0] * len(s)
      max_right, center = 0 , 0 
      
      for idx, char in enumerate(s[1:-1],start=1):
         if idx < max_right:
            radii[idx] = min(radii[center - (idx - center)], max_right- idx)

         while s[idx+(radii[idx]+1)] == s[idx-(radii[idx]+1)] :
            radii[idx] += 1 

         if idx + radii[idx] > max_right:
            max_right =   idx + radii[idx]
            center = idx

      max_radius = max(radii)
      max_radius_idx = radii.index(max_radius) 
      palindrome = s[max_radius_idx-max_radius : max_radius_idx+max_radius+1].replace("#","")
      return palindrome
```

<br><br>

## 參考資料 
1. [最简便的找字符串中最长回文子串的方法是什么？｜知乎](https://www.zhihu.com/question/40965749)
2. [最长回文子串——Manacher 算法｜曾会玩 - SegmentFault 思否](https://segmentfault.com/a/1190000003914228#articleHeader3)
3. [Manacher's Algorithm 马拉车算法｜Grandyang - 博客园](https://www.cnblogs.com/grandyang/p/4475985.html)

<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)