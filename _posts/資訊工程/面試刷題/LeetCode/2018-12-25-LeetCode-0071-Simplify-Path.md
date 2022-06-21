---
title: 【LeetCode】0071. Simplify Path
date: 2018-12-25
is_modified: false
categories:
- 面試刷題
tags:
- LeetCode
--- 

Given an absolute path for a file (Unix-style), simplify it.
<!--more-->
For example,  
**path**  =  `"/home/"`, =>  `"/home"`  
**path**  =  `"/a/./b/../../c/"`, =>  `"/c"`  
**path**  =  `"/a/../../b/../c//.//"`, =>  `"/c"`  
**path**  =  `"/a//b////c/d//././/.."`, =>  `"/a/b/c"`
 
In a UNIX-style file system, a period ('.') refers to the current directory, so it can be ignored in a simplified path. 

Additionally, a double period ("..") moves up a directory, so it cancels out whatever the last directory was. For more information, look here: [https://en.wikipedia.org/wiki/Path_(computing)#Unix_style](https://en.wikipedia.org/wiki/Path_(computing)#Unix_style)
<br>

**Corner Cases:**
-   Did you consider the case where  **path**=  `"/../"`?  
    In this case, you should return  `"/"`.
-   Another corner case is the path might contain multiple slashes  `'/'`  together, such as  `"/home//foo/"`.  
    In this case, you should ignore redundant slashes and return  `"/home/foo"`.
  
<br>

**Related Topics:**`String`、`Stack`



## 解題邏輯與實作
這題是要將給定的路徑進行簡化，路徑中可能會出現的元素有三種分別是 `.` 、 `..` 及 folder 或 file（先假裝他們是吧，不然我也不知道怎麼稱呼他們XD）

如果熟悉 Unix 的路徑規則的話，應該會認的這幾個符號，我用自己的想法稍微做個歸納，不過路徑規則不熟的話好像也沒關係，花點時間找一下題目規律而已：
1. `.`   
    表示，同一層目錄，可以直接跳過不參考
    
2. `..`   
    表示返回上一層，所以應該取消（刪除）上一個的路徑指令，另外根據 Corner Cases 給的提示，若上一層超過所給定的路徑，直接回傳 '/' 

另外需要處理的就是正規化左斜線，去除冗餘的斜線。


## Stack
這題可以使用 stack 來實做，依照上面的解題邏輯：遇到 `.` 就跳過、 `..` 就pop，若非以上兩者就 push 進 stack，整題實做流程如下：

1. 使用 Regular Expression，先將路徑正規化
2. 將路徑依左斜線切斷後，依序取出
	1. 若為 `.` ，就跳過不執行
	2. 若為 `..` 就，且 stack 不為空，就 pop 最頂層元素
	3. 若非以上兩者，就當作是一個 folder 或 file，將它 push 進入 stack
3. 最後將 stack 中的結果使用左斜線串接起來 

```python
import re
class Solution:
   def preprocess(self,path):
      reg = r'/+'
      path = re.sub(reg, "/", path)
      path = path.strip('/')
      return path.split("/")

   def simplifyPath(self, path):
      stack = []
      tokens = self.preprocess(path)

      for t in tokens:
         if t == '.':
            continue
         elif t == '..':
            if len(stack) > 0:
               stack.pop()
         else :
            stack.append(t)

      return "/"+"/".join(stack)
```
不過跑出來的效能有點不盡理想，只有 96 ms,  2.95% 。
<br>

所以稍微 Tune 了一下，放棄使用 Regular Expression 做正規化了，改成直接使用左斜線切斷後，若是遇到需要正規化的部分，切斷後的結果會是空字串，此時跳過不處理，就可以達到對左斜線做正規化的效果了。

```python
class Solution:
   def simplifyPath(self, path):
      stack = []
      tokens = path.split("/")
      for t in tokens:
         if len(t) == 0 or t == '.':
            continue
         elif t == '..':
            stack = stack[:-1]
         else :
            stack.append(t)

      return "/"+"/".join(stack)
```
跑出來的效能果然進步許多，52 ms, 49.56% ，雖然還是沒有過半...



## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)