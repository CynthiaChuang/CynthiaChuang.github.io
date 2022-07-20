---
title: CodeFree｜喝一杯咖啡，輕鬆學 Python
date: 2022-07-05
is_modified: false
image: 
categories:
- "程式設計 › 程式語言與架構"
tags:
- Python
- 讀書筆記
- Hiskio
--- 

又是喝咖啡學程式系列，這次是學 Python。筆記內容包括[基礎](https://codefree.hiskio.com/courses/27)與[進階](https://codefree.hiskio.com/courses/28)兩堂課。

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/ibijtxL.png" alt="hiskio codefree">
    Codefree Time（圖片來源: <a href="https://codefree.hiskio.com/">Hiskio Codefree</a>）
</p>



## CH 1｜關於 Python 的那些大小事

### CH 1-1｜Python 之父為什麼創造 Python？
這個我知道！聽說是聖誕節的時候太無聊而設計出來的語言，超狂的有沒有！

<p class="illustration">
    <img src="https://i.imgur.com/SBmlvxB.png" alt="吉多·范羅蘇姆（Guido van Rossum）">
    吉多·范羅蘇姆（Guido van Rossum）（圖片來源: <a href="https://buzzorange.com/techorange/2020/11/17/python-creator-joins-microsoft/">TechOrange</a>）
</p>

Python 之父名為吉多·范羅蘇姆（Guido van Rossum），而 Python 是他在 1989 年聖誕假期時為了打發時間的產物。是說，這位仁兄閒起來有多狂？我上一次看到他的消息就是因為[他說退休太無聊了，準備去微軟打工！](https://buzzorange.com/techorange/2020/11/17/python-creator-joins-microsoft/) 


Python 設計哲學是 **「優雅」** 、 **「明確」** 、 **「簡單」**。它將許多的細節隱藏交由直譯器處理，讓程式設計師可以專注於思考程式邏輯而不是具體的實做細節。

有句 Python 的 Slogan：「人生苦短，我用 Python （Love is short, use python）」，這句話也側面說明了 Python 語法簡單與高效。

<p class="illustration">
    <img src="https://i.imgur.com/OYD5b2b.png" alt="Python 的 Slogan：人生苦短，我用Python">
    人生苦短，我用Python （Love is short, use python）（圖片來源: <a href="https://read01.com/zh-tw/Pzz63ND.html">壹讀</a>）
</p>

另外課程中提到，Python 擁有「可擴充」使用的彈性，我想這應該是在說明它的第三方套件？


#### 測驗與練習
1. **請問下列何者並非為Python的設計哲學**
    - [ ]  簡單
    - [ ]  明確
    - [x]  快速
    - [ ]  優雅


### 1-2｜程式的開端：向世界說你好！
兩個重點：
1. Python 的標準輸出是 `print()`
1. 輸出內容以引號將其包圍，**雙引號或單引號皆可**，但必須<mark>成對</mark>出現。

<p class="paragraph-spacing"></p>

使用 `print()` 螢幕輸出：
```python
print("Hello World!")
```

結果：
```bash
Hello World!
```

是說第一個程式寫 Hello World！的傳統是從哪裡來的？


#### 測驗與練習
1. **完成程式缺少的元素**  
    請觀察以下程式，完成程式缺少的部分並執行。
    ```bash
    Hello~
    ```
    
    **答：**
    ```python
    print("Hello~")
    ```
    <p class="paragraph-spacing"></p>
2. **使用 `print()` 新增文字**  
    承上題（保留上一題的程式），請新增新的 print() 在原有的程式碼下，並輸出「Today is a good day」。
    ```bash
    Hello~
    Today is a good day
    ```
    
    **答：**
    ```python
    print("Hello~")
    print("Today is a good day")
    ```


### 1-3｜多行輸出：幾個 print 就幾行輸出
Python 在識別程式指令時，是由上往下逐一識別。理論上，呼叫幾次 print 就輸出幾行。

我會說理論上是因為...如果我在輸出內容插了換行符號的話，這到底算幾行？

<p class="paragraph-spacing"></p>

**多行輸出指令**
```python
print("Hello")
print("My name is HiSKIO!")
```

結果：
```bash
Hello
My name is HiSKIO!
```


#### 測驗與練習
1. **若電腦執行以下程式，請問總共會輸出幾行結果？**
    ```python
    print("Hi")
    print("My name is HiSKIO")
    print("Nice to meet you!")
    ```

    - [x] 3行
    - [ ] 1行
    - [ ] 5行



### 1-4｜工程師的小幫手，論註解的重要性
在電腦語言中，註解（comment）是重要的一個組成部分。

在 Clean Code 一書有提到：
<div class="blockquote-center">
如果一段註解對於理解程式碼沒有幫助，則沒必要為程式碼添加註解。
</div>

註解雖可以告訴別人你程式碼的目的，也能增強程式可讀性、可維護性，但除了一些合適的註解，如法律、資訊補充、意圖解說與闡釋外，單純使用程式碼就能表達意圖是最好的。如果是因為程式的函式、命名...等原因，讓人無法輕易理解開發者意圖，需花費大把時間寫註解的，可能重寫程式碼比較快 XDDD

等等，這邊不是 clean code 的讀書心得，先回到課程上。總之註解，好的註解可以幫助理解程式，在 Python 中，用 **一個井號 `#`** 做<mark>單行註解</mark>、用 **三個雙引號 `"`** 或 **三個單引號 `'`** 做<mark>多行註解</mark>。

<p class="paragraph-spacing"></p>

**註解指令**

```python
# 這是單行註解

"""
這是多行註解
這是多行註解
這是多行註解
這是多行註解
"""
```
 
不管是哪種註解方式，註解內容不會執行，因此輸出皆為空白。


#### 測驗與練習
1. **使用「單行註解」符號完成註解**  
    有一程式需要請你幫忙註解一行程式，請註解程式 `print("Hello World"!)`，其餘照常輸出。
    
    **答：**
    ```python
    # print("Hello World!")
    print("Nice to meet you")
    print(1)
    ```
    <p class="paragraph-spacing"></p>
2. **運用「多行註解」符號註解程式**  
    承上題，請利用多行註解方法，註解程式碼，註解內容為「前兩行程式碼」，其餘照常輸出。
        
    **答：**
    ```python
    '''
    print("Hello World!")
    print("Nice to meet you")
    '''
    print(1)
    ```


### 1-5｜你不知道你錯，那就電腦告訴你錯！
當撰寫的程式不符合語法時，會得到程式語法錯誤的錯誤訊息。
```python
print("Hello"
```

結果：
```bash
SyntaxError: invalid syntax
```

<p class="paragraph-spacing"></p>

另外，錯誤訊息通常會提示錯誤在第幾行附近，甚至是哪個位置，<mark>仔細觀察訊息可以快速排除錯誤</mark>。
```bash
File "main.py", line 2
if(a=10):
    ^
SyntaxError: invalid syntax
```


#### 測驗與練習
1. **SyntaxError 代表？**  
    - [ ] 程式未執行
    - [ ] 程式輸出資料有誤
    - [x] 程式語法錯誤


## CH 2｜創造變數

### CH 2-1｜資料取名字：變數
變數提供具名稱的記憶體儲存空間，具有<mark>唯一性</mark>。

因 Python 的語言**動態型別**特性，在使用該變數前毋須宣告變數，其資料型別會根據被賦值的型別來自動定義。在建立變數時，只須給出變數名稱（`name`），並使用賦值運算子（`=`）進行賦值（`"HiSKIO"`）。範例如下：
 
```python
name="HiSKIO"
```

<p class="paragraph-spacing"></p>

在訂定變數名稱須遵守 Python 的變數命名規則：
1. 變數首字元須為英文字母或底線字元。
2. 除首字元外，其他字元僅能為英文字母、底線字元或數字。
3. 不能是保留字，類似 int 之類的。
 
其他的大、小蛇式命名法...之類的使用規則應該算是 Coding Style？可以看看我之前寫的 [Python Coding Style](https://cynthiaChuang.github.io/Python-Coding-Style#naming-conventions)。至於課程中提到的**以「使用目的」去命名**，這...應該不算命名規則，這是命名常識吧 XDDD


#### 測驗與練習
1. **良好的變數命名習慣有助於整體程式更有架構，在閱讀時也能更方便。請觀察程式，選擇變數「today」儲存資料值為何?**    
    ```python
    name="HiSKIO"
    color="blue"
    today="Saturday"
    ```
    - [ ] HiSKIO
    - [x] Saturday
    - [ ] blue


### CH 2-2｜變數的輸出
使用變數最大的好處是，當需要運用該資料時，可以直接呼叫變數名稱得到該值，毋須重複撰寫資料內容與計算。

<p class="paragraph-spacing"></p>

**輸出儲存於 name 這個變數的內容**
```python
name="HiSKIO"
print(name)
print(name)
print(name)
```

結果：
```bash
HiSKIO
HiSKIO
HiSKIO
```

<p class="paragraph-spacing"></p>

若改動變數內資料，則輸出結果也會跟著變動。舉例來說將 `name` 值由 「HiSKIO」 改成 「Hi」，結果如下：
```python
name="Hi"
print(name)
print(name)
print(name)
```

結果：
```bash
Hi
Hi
Hi
```

<p class="paragraph-spacing"></p>
 
引用變數時必須完全正確。若引用錯誤會被視為使用未宣告變數，因而找不到對應資料輸出，出現 **NameError** 的錯誤。
```python
name="HiSKIO"
print(nam)
```

結果：
```bash
NameError: name 'nam' is not defined
```

<p class="paragraph-spacing"></p>
 
若要做資料拼接，可在輸出時使用逗號來拼接變數。
```python
name="HiSKIO"
print("My name is" , name)
```

結果：
```bash
My name is Hiskio
```
 
<p class="paragraph-spacing"></p>
 
若使用多個逗號，在輸出時會依照順序，由左至右疊加。
```python
name="HiSKIO"
print("My name is" , name , "nice to meet you" , "!")
```

結果：
```bash
My name is HiSKIO nice to meet you !
```
 
<p class="paragraph-spacing"></p>

說到做字串拼接，如果複雜點的字串我應該會用**字串格式化**或是**字串插值**的方法，這兩個寫起來可讀性比較好，就是得注意一下 Pytnon 版號。
```python
host="HiSKIO"
visitor="Cynthia" 

## 字串格式化
print("Hello {visitor}. My name is {host}, nice to meet you !".format(visitor=visitor, host=host))

## 字串插值
print(f"Hello, {visitor}. My name is {host}, nice to meet you !")
```

結果：
```bash
Hello Cynthia. My name is HiSKIO, nice to meet you !
Hello Cynthia. My name is HiSKIO, nice to meet you !
```


#### 測驗與練習
1. **請觀察程式，請問輸出為何者?**  
    ```python
    name="HiSKIO"
    print("Hello",name,"!")
    ```
    - [x] Hello HiSKIO !
    - [ ] Hello name !



## CH 3｜資料型態

### CH 3-1｜Python 世界中的資料型態與種類
Python 資料型別有以下幾種：
1. 數值型態（Numeric）：int, float, bool, complex
2. 字串型態（String）：str, char 
3. 容器型態（Container）：list, set, dict, tuple

<p class="paragraph-spacing"></p>

其中比較常用的資料型態，如下：

| 中文 | 英文| 說明 |舉例|
| -------- | -------- | -------- | -------- |
| 文字 | string 	| 用引號包圍的資料內容 |  `"Hello~Hiskio!"`   `'Hi~'`    |
| 整數 | int | 常見的阿拉伯數字 |  `5` | 
| 浮點數 | float | 含小數的阿拉伯數字 | `0.23`|
| 布林值 | boolean | True or False，首字母大寫 |`True`|
| 串列 |	list | 具備順序性的集合 | `["apple","orange","pineapple","grape"]`|
| 字典 |	dictionary | 由鍵（key）和值（value）構成 | `{"name":"王小明","id":249,"age":22}`|
 

#### 測驗與練習
1. **請觀察程式，並選擇變數 number 儲存的資料型態為何?**  
    ```python
    number="5"
    ```
    - [x] 文字
    - [ ] 數字
    - [ ] 布林值



### CH 3-2｜資料型態：文字
在賦值文字變數時，可使用引號來表字串的資料型態，雙引號 `""` 或單引號`''` 都行，Python 沒有用這個來區分字串或字元。

像我自己習慣最外層是雙引號，我同事的話則是保留 C 習慣，字元用單引號、字串用雙引號，反正整個專案記得統一就好，~~不統一也不會怎樣，就是逼死處女座而已~~。

若引號內沒有任何資料，則稱為空字串。
```python
id="" #變數id的資料值等於空，因為雙引號內無任何資料
print(id) #輸出無任何資料
```

<p class="paragraph-spacing"></p>

字串可透過加號由左至右拼接。
```python
name="HiSKIO"+"!" 
print(name) 
```

結果：
```bash
HiSKIO!
```

<p class="paragraph-spacing"></p>

教材中說到，**對於數學四則運算符號（`+ - * /`），文字只支援「加號」符號**。我想它這句話的意思，是說只有加號能支援被加數跟加數都是文字，其他的運算符號無法，會直接迸出 **TypeError**。

但若是被乘數是文字、乘數是數字，這情況是成立的：
```python
say1 = "重要的事要說三次！"
say3 = say1 * 3
print(say3)
```

結果：
```bash
重要的事要說三次！重要的事要說三次！重要的事要說三次！
```


#### 測驗與練習
1. **以變數設立字串**  
    請用變數 color 設立一個字串，儲存「blue」並輸出變數 color。
    
    **答：**
    ```python
    color="blue"
    print(color)
    ```
    <p class="paragraph-spacing"></p>
2. **使用加號完成文字相加**     
    請設計一道程式，觀察以下執行後的結果格式，將文字相加，變數 color 與 data 分別儲存「blue」與「book」。
    
    **答：**
    ```python
    color="blue"
    data="book"
    print(color+data)
    ```

 
### CH 3-3｜資料型態：數字
Python 常見的數字型態有整數 int 和浮點數 float 兩種。跟其他語言不同，Python 浮點數並沒有單雙精度，也就是 float 與 double，的區別。不過 Python 中的 float 與其他程式語言中的 double 具有相同的精度。  

兩兩數字資料型態可以任意使用四則運算符號進行運算，不過當整數和浮點數互相運算時得出結果會是浮點數。
```python
a=8
b=5.5
print(type(a+b))
```

結果：
```bash
<class 'float'>
```
 
<p class="paragraph-spacing"></p>

對了課程中沒提及，這些運算符號稱為 **運算子（operator）**，而用來算術計算的被特稱為 **算術運算子（arithmetic operator）** ，而常見的算術運算子除四則運算符號外，還包括取商數、取餘數跟次方三種。

| 運算子 | 功能 | 範例 |
| -------- | -------- | -------- |
| + | 加 |`a + b #8+5=13`|
| - | 減 |`a - b #8-5=3`|
| * | 乘 |`a * b #8*5=40`|
| / | 除 |`a / b #8/5=1.6`|
| // | 取商數 |`a // b  #8//5=1`|
| % | 取餘數 |`a % b #8%5 取餘數 3`|
| ** | 指數	|`a ** b #8的5次方=32768` |

上述這種<mark>運算式（expression）是由兩個運算元（operand）與一個運算子所構成</mark>。最後的計算結果可依需求直接輸出、賦值到新的變數或寫回原變數。

```python
a=8
#直接輸出
print(a+3) #11

#賦值到新的變數
c=a+3 #c=11

#寫回原變數
a=a+3 #a=11
```

如果是寫回原變數，可省略被運算元，將運算子後面加上 `=`：
```python
a+=3 #a=11
```
 
<p class="paragraph-spacing"></p>

進行除法運算時，得注意除數不得為 0。在數學上除以 0 沒有意義，但在電腦中除以 0 它會直接掛給你看：
```python
print(10/0)
```

結果：
```bash
ZeroDivisionError: division by zero
```

恩...好像也不一定直接掛，看語言跟資料型別吧。像是 Java 除以 0 也是得 error、除以 0.0 會得 infinity、0 除以 0 則會得到 NaN。Javascript 好像也是，不過 Python 必掛掉就是了。


#### 測驗與練習
1. **有兩個變數 a 與 b，分別儲存 7 和 9，請觀察輸出結果，找出使用的四則運算符號**  
    ```python
    a=7
    b=9
    ```

    輸出結果
    ```python
    -2 40353607
    ```

    - [ ] `a-b , a*b`
    - [ ] `a+b , a%b`
    - [ ] `a+b , a//b`
    - [x] `a-b , a**b`


## CH 4｜流程控制

### CH 4-1｜識別程式碼的好夥伴 — 縮排
Python 是個採用**越位規則** :soccer: 的程式語言。該種語言是透過縮排來表示區塊，而非大括號或關鍵詞來確定結構；也就是說，在 Python 中 <mark>同樣的縮排代表其程式是同一個區塊</mark>。因此當你縮排錯誤，Python 會毫不留情地給你報錯：
```python
print("Hello~")
    print(1)
```

結果：
```bash
IndentationError: unexpected indent
```

<p class="paragraph-spacing"></p>

在課程中它是 **Tab** 來縮排，不過要注意的是 Tab 在不同的環境、不同的編輯器開啟，所呈現的效果會不同，可能會被展開成空格，有的不會，即便展開成空格也有 4 個或 8 個的長度區別。記得有一次進 code 就是因為這樣產生了 conflict。

<p class="paragraph-spacing"></p>

所以你說我是空白派的嗎？其實不算，因為我習慣按 Tab 縮排；但你說我是 Tab 派的嗎？也不算，因為我會習慣調整 IDE 讓它在按下 Tab 時插入 4 個空格。不管哪一派，反正你統一一種用就是了。

雖然我記得看過人家的[實驗](https://iter01.com/92400.html)兩種混用是也行，就是得算好 Tab 跟空格的數量，但如果你真的這樣搞絕對會被其他開發者爆打的 XDDD

<p class="illustration">
    <img src="https://i.imgur.com/Ayg8TPb.png" alt="縮排，Tab 還是空格？">
    縮排，Tab 還是空格？（圖片來源: <a href="https://mobile.twitter.com/_naidile">Naidile (@_Naidile) / Twitter</a>）
</p>


#### 測驗與練習
1. **請選出縮排的使用用意與對應的鍵盤按鍵**   
    - [ ] 用意：區分程式碼、對應按鍵：Backspace
    - [ ] 用意：成立流程控制、對應按鍵：Tab
    - [x] 用意：區分程式碼、對應按鍵：Tab


### CH 4-2、if 流程控制
if 的流程控制簡單來說就是<mark>如果符合什麼條件就去做什麼動作</mark>。而在程式語言書寫方面，會有**條件式**、**冒號**與**縮排**等元素，一旦括號內條件成立時，則執行縮排內的程式。
```python
if(設立條件):
   #達成條件下的狀況
```

以一個生活化的例子來說：「如果期中考數學考了100分，媽媽就會獎勵我一塊雞排」，將它撰寫成虛擬碼則會變成：
```python
if(期中考數學考100分):
   print("獎勵雞排")
```
 
 
<p class="paragraph-spacing"></p>

在進行條件判斷時會用到 **關係運算子（Relational operator）** 或稱 **比較運算子（Comparison operator）** 來比較兩數間的關係。

| 關係運算子 | 意義     | 使用範例 | 範例運算結果 |
| ---------- | -------- | -------- |:------------:|
| ==         | 等於     | 1+1 == 2 |   True, 1    |
| !=         | 不等於   | 3 != 4   |   True, 1    |
| >          | 大於     | 5 > 6   |   False, 0   |
| >=         | 大於等於 | 7 >= 8   |   False, 0   |
| <          | 小於     | 9 < 10    |   True, 1    |
| <=         | 小於等於 | 10 <= 10   |   True, 1    |

將前面的虛擬碼用關係運算子來撰寫的話會變成：
```python
score=100
if(score==100):
   print("獎勵雞排")
```

現在寫習慣了是還好，但剛開始學寫程式的時候常常把等於的運算子打成 `=`，每次都 debug 好久 XDDD

<p class="paragraph-spacing"></p>

說到錯誤，這部分在撰寫時常見的錯誤不外乎是**少冒號**或**縮排錯誤**，不過這些錯誤都能從錯誤訊息得知：
```python
score=100
if(score==100)
   print("獎勵雞排")
```

結果：
```bash
File "main.py", line 2
if(score==100)
             ^
SyntaxError: invalid syntax
```
 
<p class="paragraph-spacing"></p>

或是
```python
score=90
if(score==100):
print("獎勵雞排")
#90分未符合，但無縮排故都會輸出
```

結果：
```bash
File "main.py", line 3
print("獎勵雞排")
^
IndentationError: expected an indented bloc
```


#### 測驗與練習
1. **使用 if 流程控制判斷餘數**  
    請設計一支程式，設立兩個整數變數 a 與 b ， a 與 b 分別儲存 10 與 5 判斷兩個變數相除（a/b）餘數是否為 0 ，若餘數為 0，輸出餘數為 0。
 
    **答：**
    ```python
    a=10
    b=5

    #a除以b的餘數等於多少為整除~，注意if架構符號
    if(a % b == 0):      
      print("餘數為0")
    ```


### CH 4-3｜else 例外處理
有 if 怎麼會沒有 else 呢 XDDD 

在未達成條件時，也有觸發相對應的情況，因此可以用 else 來控制程式。
```python
score=100
if(score==100):
   print("獎勵雞排")
else:
   print("沒獎勵~")
```

結果
```bash
獎勵雞排
```


#### 測驗與練習
1. **else 例外情況**  
    承上單元實作練習，請設計一支程式，設立兩個整數變數 a 與 b ， a 與 b 分別儲存 11 與 5，判斷兩變數餘數是否為 0，若兩整數相除後餘數為 0，輸出餘數為 0 ；若兩整數相除後餘數不為 0，請輸出餘數不為 0。
 
    **答：**
    ```python
    a=11
    b=5
    if(a%b==0):
        print("餘數為0")
    else:
      print("餘數不為0")
    ```


### CH 4-4｜elif 多重流程控制判斷
不過現實生活並非非黑即白，是會出現許多的可能。例如：媽媽說考 100 分，獎勵雞排；若沒有 100 分但有 90 分以上，就獎勵杯珍珠奶茶：連 90 分都沒有就只能空手而回了。

這種情境下 `if...else...` 可能會不好使，改使用多重判斷的 `elif` 會是比較好的選擇：
```python
score=95
if(score==100):
   print("獎勵雞排")
elif(score>=90):
   print("獎勵珍珠奶茶")
else:
   print("沒有獎勵")
```

電腦在進行判讀時，是由上往下執行：
- step 1：先判斷 score 是否等於 100，達成條件就停止判斷，沒有達成條件則往下繼續執行。
- step 2：是否大於等於 90，達成則不會繼續往下執行。
- step 3：皆無達成上述條件，停止執行。


#### 測驗與練習
1. **符號多重條件判斷**  
    請幫忙設計一個計算機程式，判斷變數 notation 中儲存的四則運算符號，變數 notataion 儲存為「+」。

    若 notation 儲存為「+」，則輸出「符號:加」；notation儲存為「-」，則輸出「符號:減」；notation儲存為「*」，則輸出「符號:乘」；notation儲存為「/」，則輸出「符號:除」
    
    **答：** 
    ```python
    notation="+"
    if(notation=="+"):
      print("符號:加")
    elif(notation=="-"):
        print("符號:減")
    elif(notation=="*"):
        print("符號:乘")
    elif(notation=="/"):
        print("符號:除")
    ```

    嘖嘖，Python 沒有 Swich-case  可以用。


### CH 4-5｜巢狀 if 流程控制
多層的條件判斷可稱為**巢狀 if 流程控制**，簡單來說來說，就是在條件成立後再做另一層條件判斷。在寫這種結構的時候，更需要注意同樣縮排的程式碼為同一區塊，當然冒號也別忘了：
```python
if(條件1):
    #達成條件1的情況
    if(條件1-1):
        #達成條件1和條件1-1的情況
    else(條件1-2):
        #達成條件1和條件1-2的情況
```

是說，如果你覺得巢狀多到難以閱讀，或是已經會混淆縮排的狀況，真心建議重構你的程式碼 XDDD

<p class="paragraph-spacing"></p>

一樣來個傷媽媽荷包的範例：「若媽媽規定期中考數學考 100 分而且國文也考 100 分，就獎勵雞排和泡芙。」
```python
math_score=100
chinese_score=100
# Step1
if(math_score==100):
    # Step2
    if(chinese_score==100):
        print("獎勵雞排和泡芙")
    # Step3
    print("數學分數達成，但國文分數並未達成")
```

是說，它的範例程式碼有點問題。如果這樣寫最終輸出結果會是：
```bash
獎勵雞排和泡芙
數學分數達成，但國文分數並未達成
```

因為它 Step3 那行輸出並沒任何條件判斷，換句話說只要達成 Step1 的條件後，Step3 就一定會輸出。如果按照題意改的話，應該要多加一個 else：
```python
math_score=100
chinese_score=100
# Step1
if(math_score==100):
    # Step2
    if(chinese_score==100):
        print("獎勵雞排和泡芙")
    # Step3
    else:
        print("數學分數達成，但國文分數並未達成")
```


#### 測驗與練習
1. **巢狀 if 流程控制判斷贈品**  
    請設計一支程式判斷，消費者是否得到贈品，變數 totalcash 與 purchase 分別儲存「1000」與「鳳梨」，規則如下:
    - 若消費者購買金額達「1000元」，而且購買物為「鳳梨」，則輸出「環保購物袋」
    - 若消費金額未達 1000 元，則直接輸出「無贈品」
    
    **答：**
    ```python
    totalcash=1000
    purchase="鳳梨"

    if(totalcash==1000 and purchase=="鳳梨"):
      print("環保購物袋")
    else:
      print("無贈品")
    ```
    呃...寫完才發現不合題意，沒用巢狀結構。算了不理它了。
    
    
### CH 4-6｜符號 and 與 or
像剛剛那種簡單條件判斷，很少會把它拆成巢狀來寫，更多時候會用**邏輯運算子（Logical operators）**，來釐清所有條件之間的關係。常用的邏輯運算子有`and`、`or` 和 `not` 三種，運算的結果不是 `True` 就是 `False`。

<p class="paragraph-spacing"></p>

讓我們繼續獻祭媽媽的錢包
- **條件皆達成**： 數學考100分 而且（and） 國文考100分 → 獎勵雞排和泡芙
- **條件其一達成**： 數學考100分 或者（or） 國文考100分 → 獎勵雞排 
```python
if(math_score==100 and chinese_score==100):
   print("獎勵雞排和泡芙")
elif(math_score==100 or CH chinese_score==100):
   print("獎勵雞排")
```

<p class="paragraph-spacing"></p>

題外話，如果兩邊運算元都是布林值的情況下，這時 `and` 跟 `or` 會跟位元運算子的 `&` 和 `|` 等價。不過還是別這麼做會比較好，小心把自己給搞混了。
- [Python Tips: and, or, &, ｜ 的差別](https://killer0001.blogspot.com/2018/10/python-tips-and-or.html)
- [Python 中 （&，｜）和（and，or）之間的區別](https://kknews.cc/code/mn9ggj2.html)


#### 測驗與練習
1. **使用 and 或 or 符號完成流程控制判斷**  
    請設計一支程式判斷，根據下列規則，判斷消費者得到贈品的狀況，變數 totalcash 與 purchase 分別儲存「1000」與「葡萄」，並善用 and 與 or 的用法。 
    - 若購買金額達「1000元」，而且購買物為「鳳梨」，兩者皆達成則輸出「環保購物袋」
    - 若購買金額達「1000元」，或者購買物為「鳳梨」，達到其中一個規則，則輸出「鳳梨軟糖一顆」
    - 若上述規則皆未達到，則輸出「無贈品」

    **答：**
    ```python
    totalcash=1000
    purchase="葡萄"

    if(totalcash==1000 and purchase=="鳳梨"):
        print("環保購物袋")
    elif(totalcash==1000 or purchase=="鳳梨"):
        print("鳳梨軟糖一顆")
    else:
        print("無贈品")
    ```


## CH 5｜for 迴圈

### CH 5-1｜迴圈介紹
另一種常見的控制流程方法是**迴圈**，它在程式中只被撰寫一次，但可能會<mark>重複執行數次</mark>。在 Python 中實作迴圈的方式有 for 迴圈和 while 迴圈兩種。

<p class="paragraph-spacing"></p>

不過實作迴圈前，這邊先來介紹迴圈控制的指令 `break` 與 `continue`：
- **break**：打破迴圈，強制跳出**整個**迴圈。
- **continue**：停止**本次**迴圈動作，但迴圈整體沒有結束，繼續進入下一輪。

另外還有一個比較少用的指令 `pass`，它是 do nothing 的概念。我自己都拿它搭配 TODO 的註解來使用來。


#### 測驗與練習
1. **下列迴圈重要指令當中，何者是在迴圈指令當中，直接打破迴圈，使程式強制跳出迴圈？**  
    - [x] break
    - [ ] for loop
    - [ ] continue


### CH 5-2｜無窮迴圈
<div class="blockquote-center"> 
注意迴圈執行狀況，勿出現無窮迴圈的情況。
</div>

什麼是無窮迴圈？就是讓<mark>迴圈永遠執行</mark>就叫無窮迴圈。通常發生**終止條件未設定好**，導致迴圈不斷執行無法停止動作，最終會導致電腦記憶體不足而發生當機的情況。

所以撰寫迴圈時，請確定程式中至少有個地方是會讓迴圈終止。不過說是這麼說啦，通常寫迴圈都會有終止條件，但偏偏中間做運算的時候出了差錯，導致無窮迴圈的產生，這種情況就真的很難抓蟲呀...


#### 測驗與練習
1. **請選出導致電腦發生「無窮迴圈」的情況**  
    - [x] 沒有設定好迴圈的條件，電腦不知道什麼時候要停止重複的動作
    - [ ] 有設定好迴圈的條件，電腦明確知道什麼時候要停止重複的動作


### CH 5-3｜for 迴圈架構：(首項、末項、差值)
在 for 迴圈的架構中會存在一個迴圈變數-**初始值、條件、遞增值**，使在疊代過程中能知曉執行順序。不過在課程中，初始值、條件、遞增值三個詞它使用的**首項、末項、差值**：
 ```python
for i in range(首項，末項，差值):
   #要重複執行的指令
```

<p class="paragraph-spacing"></p>

常見的次數控制型迴圈多會搭配 `range` 來函數使用，在函數中會定義首項、末項與差值的參數：
- **首項**  
    初始值。迴圈的起始點，也就是變數 i 的起始數字，預設為 0。例如 `range(0,5)` 等價於 `range(5)`。
- **末項**  
    條件。迴圈結束的條件，在差值為正時，當變數 i 大於等於該值即會終止迴圈，因此終點值是不會執行的！舉例來說，`range(0,5)` 是 [0, 1, 2, 3, 4] 沒有 5。
- **差值**  
    遞增值。每一輪迴圈結束後，變數 i 會加上此差值，作為下一輪迴圈的變數值，預設為 1。因此， `range(0,5,1)` 等價於 `range(0,5)`。

<p class="paragraph-spacing"></p>
  
實際執行結果如下：
```python
for i in range(0,3,1):
    print(i)
```

人工直譯的話其過程如下：

| 起始值給變數 i | 確認變數 i 是否小於末項 | 執行輸出   | 賦予差值給變數 i |
| -------------- | ----------------------- | ---------- | ---------------- |
| i=0            | i<3(成立則繼續)         | print(0)   | i=0+1=1          |
| i=1            | i<3(成立則繼續)         | print(1)   | i=1+1=2          |
| i=2            | i<3(成立則繼續)         | print(2)   | i=2+1=3          |
| i=3            | i<3(不成立則結束回圈)   | 已跳出程式 | 已跳出程式       |


因此最終輸出：
```bash
0
1
2
```

<p class="paragraph-spacing"></p>

在課程中提到，當迴圈的**首項一定要小於末項**。因為當首項大於末項且差值為1時，是沒有辦法執行該迴圈的，因為首項大於末項使終止恆成立，則無法進入迴圈。
 
其實這句話不完全正確，當首項大於末項且差值為正數時，的確無法執行迴圈。但差值也可為負數，這樣就會是一個遞減型的計數器，只是要稍微注意一下<mark>末項的判斷條件會與正數剛好相反</mark>。舉例來說，`range(0,5,1)` 是當變數**大於等於** 5 時，即會終止迴圈；`range(5,0,-1)` 則是變數**小於等於** 0 時，終止迴圈。


#### 測驗與練習
1. **以「range」控制迴圈執行**  
    請設計一支程式，利用 for 迴圈，首項為 1、末項為 8、差值為 2，執行並輸出每一次迴圈中執行的結果。
    
    **答：**
    ```python
    for i in range(1,8,2):
        print(i)
    ```
    <p class="paragraph-spacing"></p>
2. **觀察 for 迴圈程式執行結果並完成程式改寫**  
    承上題，若輸出規律變為如下，請觀察以下輸出規律，並利用 for 迴圈完成程式撰寫。
    - 每一行兩數字間有一空格，使用逗點輸出變數
    - 觀察每一行兩個輸出內容之間的數學關係（加、減、乘、除、商、餘、指數）
    ```bash
    1 1
    3 9
    5 25
    7 49
    ```    

    **答：**
    ```python
    for i in range(1,8,2):
        print(i,(i**2)) 
    ```


### CH 5-4｜for 迴圈裡的流程控制
迴圈可以搭配 if 條件判斷使用，在迴圈裡面設立條件以控制迴圈。
```python
for i in range(0,5,1):
   if(i==2):
      break
   print(i)
```

結果：
```bash
0
1
```

<p class="paragraph-spacing"></p>

另一個迴圈控制指令 `continue` 的效果如下：
```python
for i in range(0,5,1):
   if(i==2):
      continue
   print(i)
```

結果：
```bash
0
1
3
4
```

不同於 `break` 的直接結束迴圈，`continue` 只是停止本次迴圈動作，繼續進入下一輪。因此會看到一個遇到 2 時，輸出就停止了；另一個則是跳過不輸出 2。


#### 測驗與練習
1. **以 break/continue 控制 for 迴圈**   
    請改寫下列程式，新增一個 if 流程控制當控制迴圈的變數 i 為 5 時，跳過當次輸出，其餘正常輸出。

    **答：**
    ```python
    for i in range(1,10,1):
        if(i==5):
          continue
        print(i)
    ```


### CH 5-5｜巢狀 for 迴圈
跟巢狀 if 流程一樣，多層的 for 迴圈稱為**for 巢狀迴圈**。撰寫的時候除了一樣要注意縮排外，另外要注意的是變數的作用域，外層迴圈的變數會作用於內層迴圈之中，因此<mark>不同層的迴圈必需要用不同的變數去控制迴圈</mark>，否則兩者控制變數會被互相覆寫。
```python
for i in range(3,5,1):
   for j in range(0,3,1):
      print(i,j)
```

再來當一次人工直譯器：

| 外層變數 i | i<5? | 內層變數 j | j<3？                  |
| ------------ | ---- | ------------ | ---------------------- |
| i=3          | 成立 | j=0          | 成立                   |
|              |      | j=1          | 成立                   |
|              |      | j=2          | 成立                   |
|              |      | j=3          | 不成立則跳出內層迴圈 |

 
完成內層後，回到外層重新賦值變數 i ，重新進入內層迴圈，再一次以 `range(0,3,1)` 執行： 

| 外層變數 i | i<5? | 內層變數 j | j<3？                |
| ---------- | ---- | ---------- | -------------------- |
| i=4        | 成立 | j=0        | 成立                 |
|            |      | j=1        | 成立                 |
|            |      | j=2        | 成立                 |
|            |      | j=3        | 不成立則跳出內層迴圈 |

最終輸出結果如下：
```bash
3 0
3 1
3 2
4 0
4 1
4 2
```


#### 測驗與練習
1. **觀察程式執行後的結果完成缺少的巢狀 for 元素**   
    請觀察輸出範例，觀察輸出數字，並根據提示程式碼填寫遺漏的程式碼。
    ```python
    for i in range(0,,1): #少了末項數字該寫什麼呢?
        for j in range(0,,1): #少了末項數字該寫什麼呢?
            print(i,j)
    ```

    輸出數字：
    ```bash
    0 0
    0 1
    1 0
    1 1
    2 0
    2 1
    ```

    **答：**
    ```python
    for i in range(0,3,1): #少了末項數字該寫什麼呢?
        for j in range(0,2,1): #少了末項數字該寫什麼呢?
            print(i,j)
    ```


## CH 6｜while 迴圈

### CH 6-1｜當 while 迴圈條件成立時
除 for 迴圈外，另一種實作迴圈的方式是 while 迴圈，兩者在架構上及用途上大相逕庭。從英文的角度來看，while 的用法可以理解成**當…的時候**，放到 Python 中就是<mark>當括號的條件成立的時候</mark>：
```python
while(條件判斷):
   #要重複執行的指令
```

<p class="paragraph-spacing"></p>

不同於 for 迴圈有一個很明顯的計數器的存在，在 while 迴圈中只要判斷條件成立，就會一直執行下去，並沒有執行次數的初始限制在；因此較適合用於<mark>不確定會執行幾次，但只要條件符合就一直執行</mark>的情況。所以在寫 while 迴圈要注意迴圈終止條件的檢查，不然真的很容易搞出無窮迴圈。
```python
number=3

while(number<8): 
   print(number)
   number+=1
```

結果：
```bash
3
4
5
6
7
```


#### 測驗與練習
1. **以 while 迴圈完成程式**  
    請設計一支程式碼，觀察輸出情況並使用 while 迴圈完成程式撰寫。
    ```bash
    2
    4
    6
    8
    ```

    **答：**
    ```python
    number=2

    #設立while迴圈
    while(number<10): 
        print(number)
        number+=2
    ```


### CH 6-2｜while 迴圈裡的流程控制
在 while 迴圈中有一種寫法是 `while(True)`，它會強迫條件判斷一直成立，此時必須搭配 `break` 來對迴圈進行控制，否則絕對是無窮迴圈 XDDD
```python
number=0
while(True):
   if(number==3): 
      break 
   print(number)
   number+=1
```

結果：
```bash
0
1
2
```

<p class="paragraph-spacing"></p>

沒有認真去看過 Python 的原始碼，不過就行為來說 `while` 後面的判斷式跟 `bool()` 的行為一樣，有興趣的可以去看看上次[踩的坑](https://cynthiachuang.github.io/Python-Bool-Function/)。`while` 只有在下列情況回傳 False 而已，其他狀況都是回傳 True：
1. 傳入 False 值。
2. 傳入 None。
3. 傳入空值，如：空陣列、空列表、空字典、空字串...等。
4. 傳入數字 0 ，資料型態為整數或浮點數都算。
5. 傳入物件類別具有 `__bool __` 或 `__len（）__`，且其回傳值為 False 或 0。

所以如果傳入其他的情況，會被視為 `while(True)` 繼續向下執行：
```python
number=0
while(1): #視為while(True)
   if(number==3): 
      break
   print(number)
   number+=1
```

結果：
```bash
0
1
2

```

#### 測驗與練習
1. **使用 if 流程控制 while 迴圈**  
    請完成下列程式，有一變數 a=5，使用 while(True)，在迴圈內每完成一次就讓 a+1，當 a 大於 10 時，則停止迴圈，輸出當下變數 a 的數值。
    ```bash
    5
    6
    7
    8
    9
    10
    11
    ```

    **答：**
    ```python
    a=5

    #設立while迴圈
    while(True):
        #判斷是否停止迴圈「continue/break」
        if a > 10:
            break
        print(a)
        a += 1
        #變動變數a控制迴圈執行狀況
    ```
    
    題目有點問題，在它要求的輸出結果中有 11，但按文字敘述 a 大於 10 時停止迴圈，不應該列出 11 才對，而它批改的答案中也果然沒有 11。


### CH 6-3｜while 運用
基本上，上述所提到的流程控制，都可以相互使用，只要邏輯成立，就可以使程式順利執行。

讓我們繼續殘害媽媽的錢包：
- 如果數學考試考了 100 分，就會得到 10 張「獎勵雞排兌換卷」
- 若數學分數介於 90~100 之間（不包含 100 分），就會得到 3 張「獎勵雞排兌換卷」
- 若低於 90 分，則「無獎勵」
```python
math_score=90
if(math_score==100):
    number=0
    while(number<10):
        print(number+1,"獎勵雞排兌換卷")
        number+=1
elif(math_score>=90):
    number=0
    for number in range(0,3,1):
        print(number+1,"獎勵雞排兌換卷")
else:
   print("無獎勵")
```


#### 測驗與練習
1. **while 迴圈應用練習**  
    請設計一支程式，使用 while 迴圈執行 20 以內（不含 20）的偶數相加，並以變數 sum 儲存總和，意即 1~19 的所有偶數相加（2、4、8、10、12、14、16、18）。
    - 偶數定義：可被2整除（被2除後餘數為0）
    - 和為 90

    **答：**
    ```python
    a=0
    sum=0

    #設立「while迴圈」
    while(a<20): 
        if(a%2==0): #若a為偶數
            #sum加總a
            sum += a

        #變數a做更動以控制while迴圈
        a += 1
    print(sum)
    ```



## CH 7｜資料型態：串列

### CH 7-1｜串列的架構
串列（list）是一種容器資料型別，可以用來儲存一連串有順序性的元素。例如我們要儲存資料 「hi」、「你好」、「Bonjour」、「こんにちは」、「15」，一共五項資料，換成串列儲存就可以變為：
```python
# 設定串列的方法：以中括號將儲存的資料前後包起來，並以逗點隔開。
# 變數名稱=[儲存資料內容（用逗點隔開）]
mylist=["hi","你好","Bonjour","こんにちは",15]
print(mylist)
```

結果：
```bash
['hi', '你好', 'Bonjour', 'こんにちは', 15]
```


#### 測驗與練習
1. **使用中括號完成串列**  
    請設計一支程式，新增一個串列 fruit 儲存下列水果名稱，並輸出一整個串列。
    ```bash
    "apple","orange","pineapple","grape"
    ```

    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]    #想想看少了什麼串列符號
    print(fruit) #輸出一整個串列
    ```


### CH 7-2｜串列的長度
串列長度有多長，取決於串列內部總共有**多少筆資料**。在 Python 中有提供一函數能快速回傳串列長度：
```python
len(串列變數名稱)
```
 
<p class="paragraph-spacing"></p>

還是剛剛的串列例子：
```python
mylist=["hi","你好","Bonjour","こんにちは",15]
print(len(mylist))
```

在串列中一共有 5 筆資料，因此串列長度輸出為 5：
```bash
5
``` 


#### 測驗與練習
1. **使用 len() 輸出串列長度**    
    承上單元題目，請輸出該串列 fruit 的長度，不可以用` print(4)` 直接輸出。

    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]
    print(len(fruit))#輸出該串列長度
    ```
    
    這題目可以！直接輸出 4，害我想到翻轉二元樹的笑話 XDDD 



### CH 7-3｜獲取取串列資料
因串列中儲存的資料具備**順序性**，因此若需要獲取串列中的資料時，可以使用**索引值**來提取，第一筆資料由 `0` 開始，以此類推，直到最後一筆。

<p class="illustration">
    <img src="https://i.imgur.com/BWUeGIa.png" alt="串列中的索引值">
    串列中的索引值（圖片來源: <a href="https://codefree.hiskio.com/courses/28">Codefree - Python 進階篇</a>）
</p>

針對最後一筆資料，除可使用 `總長度-1` 來表示外，也可以直接使用 `-1` 來讀取。
 
<p class="paragraph-spacing"></p>

```python
mylist=["hi","你好","Bonjour","こんにちは",15]
print(mylist[4])
print(mylist[-1])
```
 
結果：
```bash
15
15
```
 
<p class="paragraph-spacing"></p>

如果存取了超過串列長度的索引，會而引起的 **IndexError**：
```python
mylist=["hi","你好","Bonjour","こんにちは",15]
print(mylist[5])
```
 
結果：
```bash
IndexError: list index out of range
```


#### 測驗與練習
1. **透過串列索引值讀取串列資料**  
    承上單元題目，請輸出串列 fruit 第一個存入的資料。

    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]
    print(fruit[0]) 
    ```


### CH 7-4｜新增串列的資料
如要新增資料到串列中，存在 3 種方法：
- **`.append(新增資料)`**：新增資料至串列最後一筆
    ```python
    mylist=["hi","你好","Bonjour","こんにちは",15]
    mylist.append("안녕") 

    print(mylist)
    ```

    結果：
    ```bash
    ['hi', '你好', 'Bonjour', 'こんにちは', 15, '안녕']
    ```
- **`.insert(指定位置,新增資料)`**：新增資料至串列指定位置
    ```python
    mylist=["hi","你好","Bonjour","こんにちは",15]
    mylist.insert(0,"Olá") #指定把資料放到第0個位子

    print(mylist)
    ```

    結果：
    ```bash
    ['Olá', 'hi', '你好', 'Bonjour', 'こんにちは', 15]
    ```
- **`.extend([多筆資料])`**：新增多筆資料至串列後方 
    ```python
    mylist=["hi","你好","Bonjour","こんにちは",15]
    mylist.extend(["Olá","안녕"])

    print(mylist)
    ```

    結果：
    ```bash
    ['hi', '你好', 'Bonjour', 'こんにちは', 15, 'Olá', '안녕']
    ```


#### 測驗與練習
1. **新增資料值至串列最後**  
    請使用「新增資料值至串列最後」的方法，新增新的資料「peach」至串列 fruit 最後，並輸出一整個串列。
    ```bash
    ['apple', 'orange', 'pineapple', 'grape', 'peach']
    ```
    
    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]
    fruit.append("peach")
    print(fruit)
    ```
    <p class="paragraph-spacing"></p>
2. **新增資料值至串列指定位置**  
    承上題，請使用新增多筆資料值至串列後方的方法，新增新的資料「tomato」、「lemon」至串列 fruit 最後，並輸出一整個串列。
    ```bash
    ['guava', 'apple', 'orange', 'pineapple', 'grape', 'peach']
    ```

    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]
    fruit.append("peach")
    fruit.insert(0,"guava")
    print(fruit)
    ```
    <p class="paragraph-spacing"></p>
- **挑戰3. 新增多筆資料值至串列後方**  
    承上題，請使用「新增多筆資料值至串列後方」的方法，新增新的資料「tomato」、「lemon」至串列fruit最後，並輸出一整個串列。
    ```bash
    ['guava', 'apple', 'orange', 'pineapple', 'grape', 'peach', 'tomato', 'lemon']
    ```

    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]
    fruit.append("peach")
    fruit.insert(0,"guava")
    fruit.extend(["tomato", "lemon"])
    print(fruit)
    ```


### CH 7-5｜刪除串列的資料
有新增就有刪除！不過刪除串列資料的方法則只有 2 種：
- **`del 串列變數名稱[欲刪除資料的位置]`**：刪除串列特定索引值資料   
    ```python
    mylist=["hi","你好","Bonjour","こんにちは",15]
    del mylist[0]
    
    print(mylist)
    ```

    結果：
    ```bash
    ['你好', 'Bonjour', 'こんにちは', 15]
    #刪除完資料後，會自動遞補資料，因此索引值0為你好
    ```
- **`.remove(資料內容)`**：刪除串列內指定資料  
    若存在相同資料時，會砍**最左邊**的，也就是砍索引值小的。
    ```python
    mylist=["hi","你好","Bonjour","こんにちは",15,"こんにちは"]
    mylist.remove("こんにちは")
    print(mylist)
    ```

    結果：
    ```bash
    ['hi', '你好', 'Bonjour', 15, 'こんにちは']
    ```
 

<p class="paragraph-spacing"></p>

如果指定刪除的資料不在串列時，會得到 **ValueError**： 
```python
mylist=["hi","你好","Bonjour","こんにちは",15,"こんにちは"]
mylist.remove("안녕")
print(mylist)
```
 
結果：
```bash
ValueError: list.remove(x): x not in list
```


#### 測驗與練習
1. **指定刪除串列的資料**  
    請利用串列刪除方法，刪除串列 fruit 最後一個資料內容。請使用程式找出最後一個位子，勿輸出 `del fruit[3]` 
    ```bash
    ['apple', 'orange', 'pineapple']
    ```

    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]
    del fruit[len(fruit)-1]
    print(fruit)
    ```
    <p class="paragraph-spacing"></p>
2. **刪除串列內特定資料值**    
    承上題，請刪除串列 fruit 中資料名稱為「pineapple」的資料。
    ```bash
    ['apple', 'orange']
    ```

    **答：**
    ```python
    fruit=["apple","orange","pineapple","grape"]
    del fruit[len(fruit)-1]
    fruit.remove("pineapple")
    print(fruit)
    ```


### CH 7-6｜串列綜合運用
請設計一支程式，運用前面章節所學到的串列方法，完成下列實作練習規定：

1. **使用新增串列資料方法新增資料**   
    運用新增方法新增資料至串列 country 中，資料值為：「Japan」,「India」,「Algeria」,「Brazil」，最後輸出該串列。
    
    **答：**
    ```python
    country=[]
    #1
    country.extend(['Japan', 'India', 'Algeria', 'Brazil'])
    print(country)
    ```
    <p class="paragraph-spacing"></p>
2. **將「該串列長度」資料放至特定位子**  
    計算串列 country 長度，並將結果新增至串列最一開始的位子，輸出整個串列內容。
    
    **答：**
    ```python
    country=[]
    #1
    country.extend(['Japan', 'India', 'Algeria', 'Brazil'])
    #2
    country.insert(0,len(country))

    print(country)
    ```
    <p class="paragraph-spacing"></p>
3. **刪除串列規定資料值**  
    刪除串列最後一個資料值，並輸出該串列。
    **答：**
    ```python
    country=[]
    #1
    country.extend(['Japan', 'India', 'Algeria', 'Brazil'])
    #2
    country.insert(0,len(country))
    #3
    del country[len(country)-1]

    print(country)
    ```
    <p class="paragraph-spacing"></p>
4. **修改串列資料值**  
    修改串列索引值第 0 位的資料內容，讀取第 0 位資料值後減 1，並輸出該串列。

    **答：**
    ```python
    country=[]
    #1
    country.extend(['Japan', 'India', 'Algeria', 'Brazil'])
    #2
    country.insert(0,len(country))
    #3
    del country[len(country)-1]
    #4
    country[0] -= 1

    print(country)
    ```



## CH 8｜資料型態：字典

### CH 8-1｜字典架構
字典是 Python 所提供的另一種容器資料型別，在儲存資料時會以鍵（key）和值（value）組合。其中 key 值具備**唯一性**，如果存在相同 key 值，後面會蓋掉前面宣告；但反之 value 值並不具備該特性。因此<mark>有可能會出現一個 key 值對應一個 value 值，但 value 值卻有可能對應多個 key 值</mark>。
```python
# 儲存資料時會以 key 和 value 組合，以逗點隔開資料，並且使用「大括號」包圍資料。
apple={
    "name":"蘋果",
    "color":"red",
    "number":12,
    "variety":["青森蘋果","五爪蘋果","智利蘋果"]
}
print(apple)
```

結果：
```bash
{'name': '蘋果', 'number': 12, 'color': 'red', 'variety': ['青森蘋果', '五爪蘋果', '智利蘋果']}
```
 
與串列不同的是，字典並**沒有固定排序**，因此每次執行輸出時的資料順序各不相同；因此，<mark>字典通常會用在不在意排順序的資料上</mark>。

格式上類似 JSON ，兩者都是種結構化資料，皆是利用 key 作為分類關鍵字，將儲存內資料依照各定方式進行分類，這些資料可以是字串、數字、布林、容器資料型別...等資料格式。

<div class="alert info">
<div class="head">容器資料型別</div>
  <ul>  
    <li>List（串列）：一個值可變、可重複、存放有順序性的資料結構。</li>
    <li>Tuple（元組）：一個值不可變、可重複、存放有順序性的資料結構。</li>
    <li>Set（集合）：一個值可變、不可重複、存放沒有順序性的資料結構。</li>
    <li>Dictionary（字典）：一個值可變、可重複、存放沒有順序性且使用唯一 Key的資料結構。</li>
  </ul>
</div>

#### 測驗與練習
1. **以字典方式儲存資料**  
    請設計一支程式儲存關於「人類」的資料值，字典名稱為「people」分類與資料為下，請輸出整個字典內容。

    資料內容：
    - name :人類
    - variety :white,black,yellow
    - gender :male,female
    - age :0,12,18,45,60

    **答：**
    ```python
    people={
            "name":"人類",
            "variety":["white","black","yellow"],
            "gender":["male","female"],
            "age":[0,12,18,45,60]
            }
    print(people)
    ```


### CH 8-2｜字典取資料值
因為字典不具備順序性，因此不存在索引值。若要提取字典中的資料，則需透過**直接指定 key** 的方式，來讀取相對應的 value。
```python
apple={
    "name":"蘋果",
    "color":"red",
    "number":12,
    "variety":["青森蘋果","五爪蘋果","智利蘋果"]
}
print(apple["number"]) 
```

結果：
```bash
12
```


#### 測驗與練習
1. **讀取字典資料值**  
    承上單元題目，請輸出鍵（key）為「gender」的資料值。

    **答：**
    ```python
    people={
        "name":"人類",
        "variety":["white","black","yellow"],
        "gender":["male","female"],
        "age":[0,12,18,45,60]
    }

    print(people["gender"])
    ```


### CH 8-3｜字典新增資料值
也因為它不具備順序性，因此在新增的時候只需要透過新增 key 並賦其 value 值的方式即可。 
```python
apple={
    "name":"蘋果",
    "color":"red",
    "number":12,
    "variety":["青森蘋果","五爪蘋果","智利蘋果"]
}
apple["cook"]="apple pie"
print(apple)
``` 

結果：
```bash
{'color': 'red', 'number': 12, 'name': '蘋果', 'variety': ['青森蘋果', '五爪蘋果', '智利蘋果'], 'cook': 'apple pie'}
```


#### 測驗與練習
1. **新增資料值至字典中儲存**  
    承上單元題目，請運用字典新增方式新增下列 key 與 value，並輸出一整個字典內容。
    ```python
    single:["yes","no"]
    ```

    **答：**
    ```python
    people={
        "name":"人類",
        "variety":["white","black","yellow"],
        "gender":["male","female"],
        "age":[0,12,18,45,60]
    }

    people["single"]=["yes","no"]
    print(people)
    ```


### CH 8-4｜字典刪除資料值
刪除字典內的資料有 2 種方法，一種是直接呼叫 del，將它從字典中**刪除**：
```python
apple={"name":"蘋果",
"color":"red",
"number":12,
"variety":["青森蘋果","五爪蘋果","智利蘋果"]
}
del apple["name"]
print(apple)
```

結果：
```bash
{'color': 'red', 'variety': ['青森蘋果', '五爪蘋果', '智利蘋果'], 'number': 12}
```
 
另一種則是使用 `pop`，將該值從字典中**彈出**。既然是彈出，就會有回傳值：
```python
apple={"name":"蘋果",
"color":"red",
"number":12,
"variety":["青森蘋果","五爪蘋果","智利蘋果"]
}
rname=apple.pop("name") #回傳值儲存在變數rname
print(apple)
print(rname)
``` 

結果：
```bash
{'color': 'red', 'variety': ['青森蘋果', '五爪蘋果', '智利蘋果'], 'number': 12}
蘋果
```


#### 測驗與練習
1. **刪除字典資料值**  
    請刪除鍵 key 為「age」的值，並輸出一整個字典與「age」返回之值。

    答：
    ```python
    people={
        "name":"人類",
        "variety":["white","black","yellow"],
        "gender":["male","female"],
        "age":[0,12,18,45,60]
    }
    age=people.pop("age")

    print(people)
    print(age)
    ```



## CH 9｜函數 function

### CH 9-1｜function 設定格式架構
在寫程式碼的一個重要的觀念是 **Don’t Repeat Yourself**，也就是要盡量**避免相同的程式碼重複出現**，這會降低可讀性很低外，並讓你維護的直升地獄難度。所以此時會進行封裝，達到程式碼的重用。

在 Python 中，若我們要<mark>封裝一段程式邏輯便於其後的使用</mark>時，我們會 function 函式來進行。而函式又分成**內建函式**與**自定義函式** 2 種。


例如說上一章節所使用 `len()`、計算總和 `sum()`、排序 `sorted()` 、取最大值的 `max()`...等，這些都是 Python 中常用的內建函式，可以直接拿來使用：
```python
score=[88,90,76]
print(sum(score))
```
 
結果：
```bash
254
```

<p class="paragraph-spacing"></p>

另一種則是我們定義的函式，這可以實做我們想要的邏輯。這邊有個重要的關鍵字 `def`，用來定義即將實做的函式：

<p class="illustration">
    <img src="https://miro.medium.com/max/1184/1*dIRdHJL9PgCg8HsCGuJrqQ.png" alt="定義即將實做的函式">
    定義即將實做的函式（圖片來源: <a href="https://medium.com/@meowent/python%E9%82%8F%E8%BC%AF%E5%B0%81%E8%A3%9D-%E5%AE%A2%E8%A3%BD%E7%89%B9%E5%AE%9A%E7%A8%8B%E5%BC%8F%E9%82%8F%E8%BC%AF%E5%B0%81%E8%A3%9D-3ed9ad0d098c">Wen Chang｜Medium</a>）
</p>


#### 測驗與練習
1. **Python 擁有眾多函式提供給開發者使用，但若沒有自己想要的函式，可以根據自己要求客製化函式使用，稱作「自定義函式」，而自定義函式有一個很重要的關鍵字，請問為何？**  
    - [ ] `sorted`
    - [ ] `sum()`
    - [x] `def`



### CH 9-2｜在 function 內直接輸出
課程這邊依照是否有回傳值分開講解，這一個章節會先教**在函式內直接輸出**的方法：
```python
def checknumpr(x):
  if(x%2==0):
    print("是偶數")
  else:
    print("不是偶數")

checknumpr(5)
```
 
結果：
```bash
不是偶數
```

<p class="illustration">
    <img src="https://cdn-beta.hiskio.com/codefree/courses/mcf6hclyawk9gk2" alt="呼叫自定義函式">
    呼叫自定義函式（圖片來源: <a href="https://codefree.hiskio.com/courses/28">Codefree - Python 進階篇</a>）
</p>

#### 測驗與練習
1. **自定義函式-判斷閏年**  
    請完成閏年判斷的自定義函式，根據閏年條件將程式完成，於函式內直接輸出是否為函式的結果。
    - 條件1：能被 100 整除且能被 400 整除
    - 條件2：不能被 100 整除且能被 4 整除

    **答：**
    ```python
    def checkyear(year):
        if((year%100==0 and year%400==0) or (year%100!=0 and year%4==0)): 
            print(year,"是閏年")
        else:
            print(year,"不是閏年")

    #完成呼叫自定義函式，帶入參數2021
    checkyear(2021)
    ```


### CH 9-3｜function 結束後回傳資料值
承上單元題目，一樣是判斷是否為偶數的程式，若使用函式的回傳值功能，程式將改寫為以下：
```python
def checknum(x):
   if(x%2==0):
      return "是偶數"
   else:
      return "不是偶數"

answer=checknum(5)
print(answer)
```

結果：
```bash
不是偶數
```

<p class="illustration">
    <img src="https://cdn-beta.hiskio.com/codefree/courses/s24x8nlhj1p7z43" alt="呼叫自定義函式-回傳值">
    呼叫自定義函式-回傳值（圖片來源: <a href="https://codefree.hiskio.com/courses/28">Codefree - Python 進階篇</a>）
</p>


#### 測驗與練習
1. **自定義函式-透過「回傳值」方法判斷閏年**  
    承上單元題目，請將閏年自定義函式改為回傳值（return）方式。    

    **答：**
    ```python
    def rCheckyear(year):
        if((year%100==0 and year%400==0) or (year%100!=0 and year%4==0)):
            return("是閏年")
        else:
            return("不是閏年")
    year=2021
    print(year,rCheckyear(year))
    ```


### CH 9-4｜全域變數與區域變數
呼叫自定義函式時，需要注意變數的擺放位置，擺放位置會影響變數的作用域。若變數擺於函式的外面，則代表為**全域變數**；反之，則代表為**區域變數**。

全域變數運用的範圍需特別注意，它在函式內可被讀取，但若需要做其他的運算，則需將其在函式中命定 `global`；若沒有命定，則程式無法進行運算並且會拋出錯誤：
```python
a=5
def countsum(x):
    global a #將a定為全域變數
    a+=x
    return a

print("呼叫 countsum 函式之前的 a：",a)
print("呼叫 countsum 函式之後的 a：",countsum(3))
```

結果：
```bash
呼叫 countsum 函式之前的 a： 5
呼叫 countsum 函式之後的 a： 8
```


#### 測驗與練習
1. **在程式當中的設定在自定義函式以外就稱作全域變數，若需要在自定義函式內使用全域變數做運算，則需要多執行什麼動作？**  
    - [ ] 將變數放在函數外
    - [x] 在自定義函式中宣告「global」
    - [ ] 電腦自己會知道哪些是全域變數



## CH 10｜物件導向程式概念

### CH 10-1｜什麼是物件？
物件導向程式設計（Object-oriented programming，OOP）有三大特色：**封裝（IEncapsulation）**、**繼承（Inheritance）**、**多型（Polymorphism）**。其優點在於程式比較有架構且可重複使用。

舉例來說，現實生活當中的人、貓、狗、椅子、桌子...等都可以稱作物件。而將這些資料抽象化則成類別，這些類別是由**屬性（attributes）** 和 **方法（method）** 所構成組成，若當我們將類別實例化時，就成了物件。

<div class="blockquote-center"> 
類別就如同物件的設計藍圖，物件則依照設計藍圖設計出來的實體。
</div>

我們以貓為例子，在類別中我們會定義它的資料：
- **屬性（attributes）**：貓毛的顏色、尾巴的長短...等等
- **方法（method）**：會吃、會叫 


#### 測驗與練習
1. **類別（Class）中又包含哪些組成？**  
    - [ ] 類別（class）和方法（method）
    - [x] 屬性（attribute）和方法（method）
    - [ ] 類別（class）和物件（object）
    <p class="paragraph-spacing"></p>
2. **關於物件導向程式設計特色，以下何者並非為特色？**   
    - [ ] 繼承
    - [x] 屬性
    - [ ] 封裝
    - [ ] 多型


### CH 10-2｜物件（Object）與類別（Class）之間關係與使用
在創立類別時，需在程式當中寫關鍵字 `class` 後，並在其後接上類別名稱。習慣上，類別的命名會採用**大駝峰式**命名法：
```python
class Cat: #類別名字第一個字母為大寫
   print("這是一隻貓") #直接輸出

blackcat = Cat() 
```
 
結果：
```bash
這是一隻貓
```

在將類別實例化時，會直接呼叫類別名稱，並其賦值給指定變數，這就成了一個物件。


#### 測驗與練習
1. **使用類別（Class）**  
    有一支程式已有類別 Dog ，請試著使用該類別。
    有一支程式已有類別 Dog ，請試著使用該類別。
    ```python
    class Dog:
        print("這是狗")
    ```

    **答：**
    ```python
    class Dog:
      print("這是狗")

    #呼叫類別
    dog = Dog()
    ```
    <p class="paragraph-spacing"></p>
2. **在創造類別（Class）時需注意名字的第一個字要特別做什麼動作？**  
    - [x] 第一個字母需大寫
    - [ ] 最後一個字母需小寫
    - [ ] 第一個字母需小寫

    雖然這是公認的命名規則，但實際上它只是屬於命名潛規則，即便用小寫也不會跳 error。問我怎麼知道？我剛剛去嘗試過了！

    <p class="illustration">
        <img src="https://i.imgur.com/YgG3VBE.png" alt="類別命名字首不為大寫">
        類別命名字首不為大寫
    </p>


### CH 10-3｜class 與 method 的關係
在前面章節中有講解到，類別當中有包含屬性和方法，而 Python 中會使用 `__init__()` 來設定初始屬性資料。
```python
class People:
    #self是必須的參數、其他參數例如voice是自己設定的參數    
    def __init__(self, voice):  
        self.peoplevoice = voice

chinese=People("你好~")
print(chinese.peoplevoice) #讀取該實例的 peoplevoice 資料
```
 
結果：
```bash
你好~
```

<p class="paragraph-spacing"></p>

而方法的定義，則與前一節的 function 定義一樣：
```python
class People:
    def __init__(self, voice):
        self.peoplevoice = voice

    def nationality(self,local):
        self.local=local
        print(self.local)

chinese=People("你好!") #設定實例
print(chinese.peoplevoice)
chinese.nationality("Taiwan") #使用color方法
```
 
結果：
```bash
你好！
Taiwan
```


#### 測驗與練習
1. **改寫範例程式以創造新的實例**  
    請根據提供的範例程式，創造新的實例變數 english，並說「Hello~」。

    **答：**
    ```python
    class People:
        def __init__(self, voice):
            self.voice = voice
        def speak(self):
            return "Hello~"

    english = People("你好!").speak()
    print(english)
    ```


### CH 10-4｜繼承
物件導向程式觀念之中，最常運用到的就是**繼承**關係。在繼承關係中，會出現**父類別**與**子類別**兩個角色，子類別會承自父類別的程式架構，子類別再根據自身需求與特性進行修改。

以下圖形來觀察繼承觀念：

<p class="illustration">
    <img src="https://cdn-beta.hiskio.com/codefree/courses/btbrr4xtgwgf6as" alt="類別命名字首不為大寫">
    串列中的索引值（圖片來源: <a href="https://codefree.hiskio.com/courses/28">Codefree - Python 進階篇</a>）
</p>
 
結果：
```bash
001
艾咪
女性
```


#### 測驗與練習
1. **物件繼承**  
    請設計一支程式，設立一個子類別（Class）為 Cat 去繼承父類別 Animal 的內容，並在子類別中設立一個方法「tail」輸出「有尾巴」。

    **答：**
    ```python
    class Animal():
        def __init__(self,voice):
            self.voice=voice
            print(self.voice)


    class Cat(Animal):
        def tail(self):
            print("有尾巴")

    tomcat=Cat("喵~")
    tomcat.tail()
    ```



## 課程內容
1. [Codefree - Python 基礎篇](https://codefree.hiskio.com/courses/27)
2. [Codefree - Python 進階篇](https://codefree.hiskio.com/courses/28)



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-07-05</summary>
  <ul>  
    <li>2022-07-05 發布</li>
    <li>2022-07-05 完稿</li>
    <li>2022-06-29 起稿</li>
  </ul>
</details>
