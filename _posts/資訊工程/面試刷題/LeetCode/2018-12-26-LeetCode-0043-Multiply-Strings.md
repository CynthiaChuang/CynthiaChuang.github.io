---
title: 【LeetCode】0043. Multiply Strings
date: 2018-12-26
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

<!--more-->
<p class="paragraph-spacing"></p>

**Example 1:**
```python
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**
```python
Input: num1 = "123", num2 = "456"
Output: "56088"
```
<p class="paragraph-spacing"></p>

> **Note:**
> 1 .  The length of both  `num1`  and  `num2`  is < 110.    
> 2 .  Both  `num1`  and  `num2`  contain only digits  `0-9`.    
> 3 .  Both  `num1`  and  `num2` do not contain any leading zero, except the number 0 itself.     
> 4 .  You  **must not use any built-in BigInteger library**  or  **convert the inputs to integer**  directly.   

<p class="paragraph-spacing"></p>

**Related Topics:**`String`、`Math`



## 解題邏輯與實作
這題最直接的解法是用直式乘法的方法做，嘿，就是下面的那個東西，小學算的要死要活的那個！

![enter image description here](https://slidesplayer.com/slide/11174206/60/images/26/%E7%9B%B4%E5%BC%8F%E4%B9%98%E6%B3%95%E7%9A%84%E6%A6%82%E5%BF%B5.jpg)


直式乘法最基本的概念就是，按位相乘、位移相加，有這個概念就可以實作了，不過有幾點要先知道的：
1. m 位數與 n 位數相乘，結果最大為 m+n 位
2. n1[i] 與 n2[j] 相乘結果會出現在 answer[i+j]上 ← 忘了說，這條是因為我把乘數與被乘數反轉才成立的
3. **corner case** ：任何與 0 相乘都會是 0 ，所以遇到是 0 可以直接回傳了

<p class="paragraph-spacing"></p>

實做過程如下：
 1.  判斷乘數與被乘數是否 0 ，若任一數為 0 ，則回傳答案為 0 ，否則繼續向下執行。 
 2.  反轉乘數與被乘數，方便從最低位數開始計算。 
 3.  使用雙層迴圈，依序取值，兩相乘後與進位值和上一層相乘結果相加。
 4.  最後將計算結果反轉後回傳。  

 
```python
class Solution:
   def __init__(self):
      self.acrii_0 = ord('0')

   def chr_to_int(self,num):
      return ord(num) - self.acrii_0

   def int_to_chr(self,num):
      return chr(num + self.acrii_0)

   def mult(self, num1, num2, carry):
      num1 = self.chr_to_int(num1)
      num2 = self.chr_to_int(num2)
      carry = self.chr_to_int(carry)
	
      result = num1 * num2 + carry
      carry = 0
      if result >= 10 :
         carry = result // 10
         result = result % 10

      return self.int_to_chr(result), self.int_to_chr(carry)
	
   def add(self, num1, num2):
      num1 = self.chr_to_int(num1)
      num2 = self.chr_to_int(num2)
      result = num1+num2
      return self.int_to_chr(result)

   def multiply(self, num1, num2):
      if num1 == "0" or num2 == "0":
         return "0"

      len_num1 = len(num1)
      len_num2 = len(num2)
      answer = [''] * (len_num1 + len_num2)

      num1 = num1[::-1]
      num2 = num2[::-1]
 
      for j in range(len_num2):
         for i in range(len_num1):
            n1 = num1[i]
            n2 = num2[j]
            carry = '0' if answer[i+j]   == "" else answer[i+j] 

            result, carry = self.mult(n1,n2,carry)
            answer[i+j] = result
		 
            if carry != "0" :
               if answer[i+j+1] != "" :
                  answer[i+j+1] = self.add(answer[i+j+1] ,carry)
               else:
                  answer[i+j+1] = carry

      answer = answer[::-1]	
      return "".join(answer)
```
因為題目要求不能轉 int，但我又不想自己做相乘，所以我折中使用 arcii 的方法，算是有點偷吃步吧 XD



### 作弊...
上面那邊用自己做的乘法器，執行完的效果感覺不是很好，跑出了大約 536 ms、6.77% 的成績。想說要來就一下 40 ms 的寫法，看完之後我輸的不冤阿...

```python  
class Solution:  
   def multiply(self, num1, num2):      
      return str(int(num1)*int(num2))
```
這位仁兄直接轉整數後相乘再轉回來，當然快...



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)