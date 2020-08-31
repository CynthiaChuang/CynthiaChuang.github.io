---
title: "Horovod error: Read -1, expected ....."
date: 2020-08-11
is_modified: false
categories:
- AI/ML
tags:
- Horovod
--- 

最近[專案](/Distributed-Training-with-Horovod-for-TensorFlow-Keras)指定要用 [Horovod](https://horovod.readthedocs.io/en/stable/summary_include.html) 分散式深度學習框架，難度不大，就是執行的時候遇到一些惱人的小問題，不影響執行，但超煩 orz ....

<!--more-->
<br><br> 

## 問題描述

當執行分散式訓練時，終端機上不斷輸出下列訊息，嚴重影響 log 的閱讀，但此訊息並不影響最終訓練結果。
<center> <img src="https://i.imgur.com/zNPChb4.png" alt="log"></center>

<br><br> 

## 解決辦法

感謝大神同事-墨非解惑，他給了我兩種解決辦法：

1. **過濾掉這些訊息**
    ```bash
    $ mpirun ... python tensorflow_word2vec.py |& grep -v "Read -1"
    ```
2. **在執行 Docker 時候加上 --privileged**  
    加上 --privileged 來執行 Docker 就會啟動所謂的<span class='highlighting'>特權容器 (privileged container)</span>，使容器擁有主機完整的 root 權限。
    
    但因為擁有主機完整的 root 權限，因此在容器中可以看到所有的 gpu resurce，卻也會導致我限定 gpu 使用的指令無效。
    
    ```bash    
    export CUDA_VISIBLE_DEVICES=1,3
    ```
    
    因此若是要指定 Docker 僅看得到某些 gpu 並能採用此方法，只能透過方法一來解決。


<br><br> 

## 參考資料 
1. tgaddair (2018-09-21)。[Horovod error: Read -1, expected ..... · Issue #503](https://github.com/horovod/horovod/issues/503#issuecomment-423238630) 。檢自 horovod/horovod ｜ github (2020-07-02)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-08-11</summary>
  <ul class="timestamp">
    　<li>2020-08-11 發布</li>
    　<li>2020-07-09 完稿</li>
    　<li>2020-07-02 起稿</li>
  </ul>
</details>