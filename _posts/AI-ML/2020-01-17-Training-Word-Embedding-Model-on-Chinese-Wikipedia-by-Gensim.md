---
title: 基於 gensim 訓練中文維基詞向量
date: 2020-01-17
modified: 2020-01-17
categories:
- AI/ML
tags:
- NLP
- gensim
- Word Embedding 
- Word2Vec
--- 
過年大掃除的時候發現，我的草稿夾內塞滿之前寫一半的草稿，想說欠都欠過年了，還是讓他繼續欠下去吧(誤)...
  
這篇是紀錄前一陣子（其實也快一年前了...）為了弄出一份符合我們應用的詞向量，開始嘗試著自給自足的故事 XD

<!--more-->

為了方便操作，這邊採用的是使用 python 的函式庫 —  [gensim](https://radimrehurek.com/gensim/) 來實作，文章裡所有的程式碼都會傳上 github...如果找不到那肯定是我拖延症又發作了XD

<br><br>

## Word Embedding 
先來看看 Word Embedding，又名詞向量，是指將文字的語意用一組向量來表示，其概念是出自於 Bengio 的 《A Neural Probabilistic Language Model》（論文：[原文](http://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf)、[筆記](https://medium.com/%E7%A8%8B%E5%BC%8F%E5%B7%A5%E4%BD%9C%E7%B4%A1/a-neural-probabilistic-language-model-%E8%AB%96%E6%96%87%E7%AD%86%E8%A8%98-61f4c5cecee7)）。


在詞向量的訓練過程中，它會儘可能提取特徵，使著具有相同上下文語意的詞，在高緯度空間中盡可能相近；反之，使含義並不相似的詞距盡可能遠離。此外，也是我們能夠用類別的方式推導詞，最著名的例子：

$$
\overrightarrow{King} - \overrightarrow{Man} +  \overrightarrow{Woman} \approx \overrightarrow{Queen}
$$
 
<br>

<center> <img src="https://i.imgur.com/9weAgyi.jpg" alt="word vector"></center>
<center class="imgtext">word vector（圖片來源: <a href="https://flashgene.com/archives/48063.html" class="imgtext">闪念基因</a>）</center>
<br>


而 word2vec 則是 Google 所提出用來實現 word embedding 的一種方法，主要採用了 Skip-Gram 與 CBOW 兩種模型。

在 Skip-Gram 中，一次只輸入一個字，輸出為其前後一定距離內的文字，所以<span class='highlighting'>同一個輸入會有多個輸出</span>；與 Skip-Gram 不同的是，CBOW 是將一段句子的中間字當作輸出為，其左右文字為輸入，所以是<span class='highlighting'>多個輸入一個輸出輸入</span>。

簡單來說，Skip-Gram 是給定 input word 來預測上下文。而 CBOW 則是給定上下文，反過來預測 input word。
<br>


不過，我們這邊不討論 word2vec 的數學公式，也不討論 word2vec 的訓練模型，這邊就只想辦法訓練一包詞向量而已 XD


- 若是對數學公式有興趣的，往這走 ↓  
-- [word2vec 中的数学原理详解（一）目录和前言_word2vec,CBOW,Skip-gram｜peghoty-CSDN博客](http://blog.csdn.net/itplus/article/details/37969519)

- 若是對神經網路架構有興趣的，看這邊 ↓  
-- [類神經網路 -- word2vec (part 1 : Overview)｜MARK CHANG'S BLOG](http://cpmarkchang.logdown.com/posts/773062-neural-network-word2vec-part-1-overview)

- 只想訓練一份詞向量的，就往下看吧

<br><br>

## 函式庫安裝

在開始之前，先把 gensim 安裝起來，只要一條指令：

```shell
$ pip install --upgrade gensim
```

<br> 另外，需要準備一套斷詞工具，顧名思義就是將整篇的語料拆成一個一個詞，才能交給 word2vec 進行訓練。這邊可以依照自己的需求挑選斷詞工具，例如： [jieba](https://github.com/ckiplab/ckiptagger)，而我所挑選的是 [pyhanlp](https://github.com/hankcs/pyhanlp)，在自定義的字典檔添加上它還滿方便的。

```shell
$ pip install pyhanlp
```

<br> HanLP 最近似乎有釋出 [HanLP 2.0](https://github.com/hankcs/HanLP) ，但還在 Alpha 階段就是了。不過，不管是 jieba 或是 pyhanlp/HanLP 都是以簡體中文為核心，若想使用繁體中文為核心的，可以考慮在去年(2019)中研院釋出 [CKIP](https://github.com/ckiplab/ckiptagger)的 python API 。
<br>

最後紀錄一下我所使用的 pyhanlp 版號，我有一陣子沒更新了說，[1.x branch](https://github.com/hankcs/HanLP/tree/1.x) 最後的版號應該是 v1.7.6 吧


<div class="alert info"> 
<div class="head">hanlp --version</div>
jar  1.6.8: /py3.6/lib/python3.6/site-packages/pyhanlp/static/hanlp-1.6.8.jar <br>
data 1.6.8: /py3.6/lib/python3.6/site-packages/pyhanlp/static/data <br>
config    : /py3.6/lib/python3.6/site-packages/pyhanlp/static/hanlp.properties <br>
</div>

<br><br>

## 取得語料

### 下載語料
在機器學習中，萬事起頭的第一步就是備齊資料，當然訓練詞向量也是。為了訓練出針對特定 domain 但又不失一般性的詞向量，因此必須分別收集 general 與 specific domain 的資料。

這邊 general 的資料是此用<span class='highlighting'>中文的維基百科</span>，因為它資料夠大，也較為全面。維基百科有[最新中文資料](https://dumps.wikimedia.org/zhwiki/latest/zhwiki-latest-pages-articles.xml.bz2)可供下載，也可以前往[維基百科的資料庫](https://zh.m.wikipedia.org/wiki/Wikipedia:%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%8B%E8%BD%BD)下載所需要的版本。需要特別注意的是，請選擇 `*-pages-articles.xml.bz2` 形式的語料。

下載完成後，先別急著將下載檔案解壓縮，因為這是一份 1.9G 的 xml 文件，如果不想自己寫 parser 來解析這份文件，請先別衝動！...說的就是我自己 XD 
<br>

### 解析語料
在 gensim 中有提供 API 可以直接調用，以取出文章的標題和內容。

使用的方法很簡單，先初始化 `wikiCorpus`，指定 wikipedia 的路徑與 dictionary...等選項。再使用 `get_texts()` 迭代每一篇文章，它會回傳一個 tokens list，將這些 tokens 用空白符號串接成字串寫出到另一份文件中，就完成解析了。
  
 
```python
from gensim.corpora import WikiCorpus
from pyhanlp import *

def hasUnicodeEncodeError(context):
    try:
        for char in context:
            char.encode('utf8')
    except UnicodeEncodeError as e:
        return True

    return False
    
    
sources="zhwiki-latest-pages-articles.xml.bz2"
output="wiki_Sentence.txt"
 
wiki_corpus = WikiCorpus(sources, dictionary={})
with open(output, "w") as f:
    for texts in tqdm(wiki_corpus.get_texts(), desc='wiki2txt'):
        corpus = []
        for text in texts:
            w = HanLP.s2tw(text)
            w = text if hasUnicodeEncodeError(w) else w
            corpus.append(w)
        f.write(" ".join(corpus) + ' \n')
```

<br> 在解析的過程中，我發現這份檔案有繁簡交雜的現象，導致我最終的結果不如預期，因此我在寫出檔案前多做了些處理。
1. 每個 token，依序做一次繁簡轉換。
2. 檢查轉換的結果是否符合編碼，若結果符合則留下，反之則保留轉換前的輸入。
 
<br><br>

## 訓練前處理

### 斷詞
因為 pyhanlp 的預設斷詞是使用簡體中文斷詞，如果想用簡體的斷詞器對繁體的語句進行斷詞，效果不彰，因此必須將語句換成簡體，或改用繁體的斷詞器，這邊是選擇使用<span class='highlighting'>繁體的斷詞器</span>。
 
但，想使用繁體的斷詞器必須去你的 pyevn 中的 site-packages 中更改 pyhanlp 的 init 檔案，讓它可以引入繁體的斷詞器：

```bash
$ cd ~/py3.6/lib/python3.6/site-packages/pyhanlp
```

<br> 接著修改 `__init__.py`，在最下方的 API 列表中加入
```python
TraditionalChineseTokenizer= SafeJClass('com.hankcs.hanlp.tokenizer.TraditionalChineseTokenizer')
```

<br> 如此一來就可以使用繁體的斷詞器了：
```python
from pyhanlp import *

# 用簡體的斷詞器進行斷詞
# [你好/vl, ，/w, 欢迎/n, 在/p, Python/nx, 中/f, 调用/n, HanLP/nx, 的/ude1, API/nx]
HanLP.segment('你好，欢迎在Python中调用HanLP的API')


# 用簡體的斷詞器對繁體的語句進行斷詞
# [你好/vl, ，/w, 歡迎/v, 在/p, Python/nx, 中/f, 調/n, 用/p, HanLP/nx, 的/ude1, API/nx]
HanLP.segment('你好，歡迎在Python中調用HanLP的API')


# 用繁體的斷詞器進行斷詞
# [你好/vl, ，/w, 歡迎/n, 在/p, Python/nx, 中/f, 調用/n, HanLP/nx, 的/ude1, API/nx]
TraditionalChineseTokenizer.segment('你好，歡迎在Python中調用HanLP的API')
```

<br> 若是不想修改 init 檔案，也可以在使用繁體斷詞器前才引入
```python
from pyhanlp import *

TraditionalChineseTokenizer= SafeJClass('com.hankcs.hanlp.tokenizer.TraditionalChineseTokenizer')
TraditionalChineseTokenizer.segment('你好，歡迎在Python中調用HanLP的API')
```
<br> 

完成繁體斷詞器配置之後，就可以針對上一個步驟所解析出來的語料進行斷詞：

```python
from pyhanlp import *
import re

def getWordsAndNatures(term):
    words = [w.word for w in term]
    natures = [w.nature.toString() for w in term]
    return [" ".join(words), " ".join(natures)]

def restructuring(line):
    endswith = line.endswith("\n")
    line = line.strip()
    line = re.sub(r'\s+', " ", line)
    if endswith:
        line += " \n"
    return line
        
sources=["Sentences_1.txt", "Sentences_2.txt"]      
output="segment_result.txt"

with open(self.output, "w") as fw:
    for file in self.sources:
      with open(file, 'r') as fr:
        for line in tqdm(fr, desc='segment {0} lines'.format(file)):
          line = line.strip()

          # （看情境）其他處理-1： 取代 e-mail、取代 url
          term = HanLP.segment(line)
          corpus = self.getWordsAndNatures(term)
          # （看情境）其他處理-2: 依照詞性取代掉姓名、機關名稱
          # 其他處理-3: 移除 stop word
          fw.write(self.restructuring(corpus[0]) + "\n")
```
<br>

### 其他前處理
如果有注意到，在上段程式碼中我留了三個其他處理的註解，這邊可以取決你的應用是否希望將它換成特定的標籤，例如將對話中的 e-mail 或 url 換成 `#EMAIL#`、`#URL#` 等標籤，使斷詞結果更符合我們預期，或是可以依照詞性取代掉姓名、機關名稱，使最後訓練出來詞向量可以集中這些詞性的詞，但這樣的修改有好有壞就是。

最後一個註解是移除 [stop word](https://zh.wikipedia.org/wiki/%E5%81%9C%E7%94%A8%E8%AF%8D)，中文翻作停用詞，這些詞極其普遍，但與其他詞相比它並沒有什麼實際含義，如：阿、呀。這些詞的移除可以加強單詞的上下文關係，理論上有助於詞向量的訓練。

除了上述的前處理外，在程式碼看不到的部份，我還對 Hanlp 的字典進行調整，引入了[內政資料開放平臺](https://data.moi.gov.tw/)的資料，並使用 Hanlp 內建的[新詞挖掘](https://github.com/hankcs/HanLP/wiki/%E6%96%B0%E8%AF%8D%E8%AF%86%E5%88%AB)的功能，使它的斷詞結果更符合臺灣的對話情境。
<br>

### Out-of-Vocabulary Words，即 OOV

斷詞處理的最後一段，我們設置一個詞頻的門檻值，針對低詞頻的詞將使用 `#UNK#` 這個標籤來取代。這是因為這些低頻詞由於缺乏足夠數量的語料，訓練出來的詞向量往往效果不佳，也可同時訓練出一組詞向量用以表示詞彙表中不存在的詞。

這邊就不分享替換 OOV 的程式碼了，我這邊暫時使用暴力法來實做，先將斷詞結果讀入，並計算每個詞出現次數，最後將低於門檻值的詞換成 UNK 標籤，再將結果寫出。只是這樣實做流程耗時又耗空間，還有待改進 orz...

<br><br>

## 訓練詞向量

最後就是訓練詞向量啦！程式碼很簡單就幾行而已，但麻煩的是超參數的調整只能按你的應用，慢慢實驗來調整。


```python
from gensim.models import word2vec

source="segment_result.txt"
model_name = "my_model"
vector_size = 200
min_count = 10
window_size = 3
workers = 3

sentences = word2vec.LineSentence(source)
model = word2vec.Word2Vec(sentences, size=vector_size, min_count=min_count, window=window_size, workers=workers)
model.save(model_name)
```
<br>

回頭看看程式碼的部分：

1. **sentences**  
    指的就是我們用來訓練詞向量的語料，因為我這邊無論是維基百科或是 specific domain 的資料，都是以一行為一篇文章或是對話。因此在讀入語料時，使用 `LineSentence` 的模式。
    
2. **vector_size**  
    詞向量的維度。
    
3. **window**  
    指訓練窗格大小，白話來說，一個詞在看上下文關係時，上下應該各看幾個字的意思。
    
4. **workers**  
    執行緒數目。
    
5. **min_count**  
    指一個詞出現的次數若小於 min_count，則拋棄不參與訓練。

<br><br>

## 訓練詞結果

訓練完成了，那就來試試下面的結果！

```python
from gensim.models import word2vec
from gensim import models

model = models.Word2Vec.load("my_model")

# 前 100 相似詞
model.most_similar("飲料",topn = 100)

# 兩個詞的相關性
model.similarity("棒球","全壘打")

# 相似關係的前 100 個詞
model.most_similar(["日本","東京"], ["美國"], topn= 100)                  
```

<br><br>

## 參考資料 

1. [word2vec和word embedding有什么区别?｜知乎](https://www.zhihu.com/question/53354714)
2. [DeepNLP的表示学习·词嵌入来龙去脉·深度学习（Deep Learning）·自然语言处理（NLP）·表示（Representation）｜Mr.Scofield-CSDN博客](https://blog.csdn.net/scotfield_msn/article/details/69075227)
3. [A Neural Probabilistic Language Model](http://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf)
4. [A Neural Probabilistic Language Model 論文筆記｜程式工作紡 - Medium](https://medium.com/%E7%A8%8B%E5%BC%8F%E5%B7%A5%E4%BD%9C%E7%B4%A1/a-neural-probabilistic-language-model-%E8%AB%96%E6%96%87%E7%AD%86%E8%A8%98-61f4c5cecee7)
5. [zonghan程式筆記: Word2Vec model Introduction (skip-gram & CBOW)](http://zongsoftwarenote.blogspot.com/2017/04/word2vec-model-introduction-skip-gram.html)
6. [word2vec 中的数学原理详解（一）目录和前言_word2vec,CBOW,Skip-gram｜peghoty-CSDN博客](http://blog.csdn.net/itplus/article/details/37969519)
7. [類神經網路 -- word2vec (part 1 : Overview)｜MARK CHANG'S BLOG](http://cpmarkchang.logdown.com/posts/773062-neural-network-word2vec-part-1-overview)
8. [word2vec实战：获取和预处理中文维基百科(Wikipedia)语料库，并训练成word2vec模型｜欢迎来到Jimmy的博客-CSDN博客](http://blog.csdn.net/qq_32166627/article/details/68942216
)
9. [中文维基百科语料库词向量的训练｜吴良超的学习笔记](https://wulc.me/2016/10/12/%E4%B8%AD%E6%96%87%E7%BB%B4%E5%9F%BA%E7%99%BE%E7%A7%91%E7%9A%84%E8%AF%8D%E5%90%91%E9%87%8F%E7%9A%84%E8%AE%AD%E7%BB%83/)
10. [以 gensim 訓練中文詞向量｜雷德麥的藏書閣](http://zake7749.github.io/2016/08/28/word2vec-with-gensim/) 
