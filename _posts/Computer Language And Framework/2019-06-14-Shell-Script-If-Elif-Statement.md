---
title: 【Shell Script】if ... else 條件判斷式
date: 2019-06-14
categories:
- Computer-Language/Framework
tags:
- Shell Script
--- 

Shell Script 的條件宣告是用 <span class='label'>[]</span> ，if 條件後面需接 <span class='label'>then</span> ，block 結束後接 <span class='label'>fi</span>

<!--more-->
<br>

寫法大概長這樣
```bash
if [ 條件 ]; then
  # do something
elif [ 條件 ]; then
  # do something
else
  # do something
fi
```


<br><br> 是說在寫條件式的時候稍微卡了一下，習慣性比較時直接用了 `==`
```bash
name="Cynthia" 
if [ "$name" == "Cynthia" ]; then
  # do something
fi
```

不過執行時一直跳出 **[: Cynthia: unexpected operator**，稍微 de 了一下 bug，後來才想到，我測試時是使用 sh run ，而 `==` 是 bash 的語法。所以要嘛改用 <span class='label'>bash run</span>，不然就把  <span class='label'> **==** 改成  **=** </span>。 



<br><br>
## 參考資料
1.  [第十二章、學習 Shell Scripts｜鳥哥的 Linux 私房菜](http://linux.vbird.org/linux_basic/0340bashshell-scripts.php)

