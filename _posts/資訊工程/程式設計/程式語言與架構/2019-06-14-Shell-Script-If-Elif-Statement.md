---
title: 【Shell Script】if ... else 條件判斷式
date: 2019-06-14
is_modified: false
categories:
- "程式設計 › 程式語言與架構"
tags:
- Shell Script
--- 

Shell Script 的條件宣告是用 <mark>[]</mark> ，if 條件後面需接 <mark>then</mark> ，block 結束後接 <mark>fi</mark>

<!--more-->
<p class="paragraph-spacing"></p>

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

<p class="paragraph-spacing"></p>

是說在寫條件式的時候稍微卡了一下，習慣性比較時直接用了 ``==`
```bash
name="Cynthia" 
if [ "$name" == "Cynthia" ]; then
  # do something
fi
```

不過執行時一直跳出 **[: Cynthia: unexpected operator**，稍微 de 了一下 bug，後來才想到，我測試時是使用 sh run ，而 `==` 是 bash 的語法。所以要嘛改用 <mark>bash run</mark>，不然就把  <mark> **==** 改成  **=** </mark>。 



## 參考資料
1.  [第十二章、學習 Shell Scripts｜鳥哥的 Linux 私房菜](http://linux.vbird.org/linux_basic/0340bashshell-scripts.php)

