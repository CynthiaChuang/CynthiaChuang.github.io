---
title: 【Python】 如何計算行數與其效率分析
date: 2020-08-10
is_modified: false
categories:
- Computer-Language/Framework
tags:
- Python
--- 

最近在訓練的時候，需要事前知道訓練集的大小來對 learning rate 進行 decay。但是，我的檔案大又多，想盡可能壓縮計算的時間。

<!--more-->

<br><br>

## 行數計算

<center> <img src="https://images.pexels.com/photos/1329296/pexels-photo-1329296.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260" alt="計數"></center>
<center class="imgtext">計數（圖片來源: <a href="https://www.pexels.com/zh-tw/@magda-ehlers-pexels" class="imgtext">Magda Ehlers | pexels</a>）</center>
<br>


### readlines

計算行是最簡單、直覺的方法當然是用 `readlines()`：

```python
count = len(open(file_name,'rb').readlines())
```

但這方法並不適用於大檔案，我的測試檔案有 187 份每一份的大小是 5G，用這方法讀應該會卡到往生 :ghost: 
<br>

因此為了減輕讀取壓力，我設定了每次讀取位元組的限制：

```python
start = time.time()

count = 0
with open(file_name, 'rb') as f :
    while True:
        lines = f.readlines(1024*8192)
        if not lines:
            break
        count += len(lines)
        
endtime = time.time()
print (count, (endtime - start))
```
一份檔案的讀取時間約：**4.602344036102295** 秒。


<br>


### readline

除了用 `readlines()`，也可以用 `readline()`：
```python
start = time.time()

count = 0
with open(file_name, 'rb') as f :
    while True:
        line = f.readline()
        if not line:
            break
        count +=1
endtime = time.time()
print (count, (endtime - start))
```
一份檔案的讀取時間約：**4.239390134811401** 秒，看起來稍稍快一點？

<br>

### read
 
Python 讀檔三寶除了 `readline()` 和 `readlines()` 外，有個 `read()`，接下來試試用 `read()`。不過 `read()` 讀進來的也是整份文件，為了不讓記憶體爆掉，也是設了 chunks size。

```python
start = time.time()
count = 0
with open(file_name, "rb") as reader:
    while True:
        data = reader.read(1024*8192)
        if not data:
            break
        count += data.count(b'\n')
endtime = time.time()
print ((endtime - start))
```

不過他讀進來的是字串，沒有行的概念，所以統計換行符號個數來計算行數。算下來讀取一份檔案約 **4.244061231613159** 秒。
<br>

組長問我用 `rb` 跟 `r` 來取檔案速度到底差多少？反正也就點工，出去吃飯的時候，順便跑了下。出來結果一份檔案要  **16.770071983337402** 秒，是用 `rb` 模式讀取時間的 4 倍。
 
<br>

### 迭代器

是說，BufferedReader 本身就是一個迭代器物件(iterator)，拿它來算行數也行。

```python
start = time.time()

count = 0
with open(file_name,'rb') as f :
    for line in f :
        count += 1
endtime = time.time()
print ((endtime - start))
```
一份文件的讀取時間約 **4.3013999462127686** 秒。
<br>

有看到有人在把跟 `enumerate` 一起用，省掉 count 累加的部份，跑出來的時間 **4.196686029434204** 秒，倒是目前最低的。


```python
start = time.time()
count=-1
for count, line in enumerate(open(file_name,'rb')):
    pass
count+=1

endtime = time.time()
print (endtime - start)

```



<br><br>

## Multiprocess

看來 5G 檔案，4 秒左右是極限了。所以我把主意打到了 Multiprocess 上，雖然不一定又作用，畢竟讀寫頭順著不寫應該會比較快？

網路上看到現成的，我就不自己寫了：

```python
import multiprocessing as mp
from itertools import (takewhile,repeat)


def count_lines(file_name):
    count = 0    
    with open(file_name,'rb') as f:
        f = open(file_name, 'rb')
        bufgen = takewhile(lambda x: x,
                           (f.raw.read(1024 * 1024) for _ in repeat(None)))
        count += sum(buf.count(b'\n') for buf in bufgen if buf)

    return count


start = time.time()
pool = mp.Pool(processes=4)
asyncResult = pool.map_async(count_lines, file_names)
count = sum(asyncResult.get())    
endtime = time.time()
print (count, (endtime - start))
```

以一份檔案 4 秒讀取時間來算，187 份檔案預計會花上 12.5 分鐘。而，實測結果花了 **10.3** 分鐘。  
P.S. 寫到這邊才想到，我又不重 CPU 計算的部份，應該開 Multithread 就好，不用開到 Multiprocess。

<br><br> 

## 參考資料 
1. vmele (2017-12-06)。[optimization - Optimize file and number line count in Pytho](https://stackoverflow.com/questions/47637617/optimize-file-and-number-line-count-in-python) 。檢自 Stack Overflow (2020-06-18)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-10</summary>
  <ul class="timestamp">
    　<li>2020-08-10 發布</li>
    　<li>2020-06-18 完稿</li>
  </ul>
</details>

