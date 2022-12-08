---
title: 使用 OpenCV 不存檔直接讀入 Flask 所傳進的圖片
date: 2020-08-19
is_modified: true
categories:
- "程式設計 › 程式語言與架構"
tags:
- Python
- flask
- OpenCV
--- 

在實做 flask 時，我透過 curl 指令取得了張圖片，但它的資料格式是 `'werkzeug.datastructures.FileStorage'`，不能直接用 `cv2.imread` ，但我又不想跟範例程式碼一樣，先存出再讀回。所以找了其他的方法來實做。

<!--more-->


## 先存出再讀回
一般常見的範例都會先存出再讀回，將像下面程式碼一樣。

```python
data =request.files['file']
filename = secure_filename(file.filename) 
filepath = os.path.join("/tmp", filename);
file.save(filepath)
img = cv2.imread(filepath)
```



## 直接從記憶體中讀取
不過要先存出再讀回，我實在不是很喜歡，所以找到的 `cv2.imdecode`，根據解釋：

<div class="alert info"> 
<div class="head">cv2.imdecode()</div>
函數從指定的記憶體快取中讀取數據，並把數據解碼成圖像格式；主要用於從網路傳輸數據中恢復出圖像。
</div>

<br class="big"> 

看到<mark>網路傳輸數據</mark>眼睛就亮起來了有沒有！當下就決定把 `cv.imread` 換成 `cv2.imdecod`，並搭配 `numpy` 的`fromstring` 使用字串來建立一個矩陣，完整程式碼如下：

```python
filestr = request.files['file'].read()
npimg = numpy.fromstring(filestr, numpy.uint8)
img = cv2.imdecode(npimg, cv2.IMREAD_COLOR)
```


### 資料型態
用這方式讀進來的資料型態是 `uint8`，在我的應用中，我會接著喂進 TensorFlow 做 predict，結果會跳出：

<div class="alert danger"> 
<div class="head">TensorFlow TypeError</div>
Value passed to parameter input has DataType uint8 not in list of allowed values: float16, float32
</div>

<br class="big"> 

所以我又在最後加了行型態轉換：

```python
filestr = request.files['file'].read()
npimg = numpy.fromstring(filestr, numpy.uint8)
img = cv2.imdecode(npimg, cv2.IMREAD_COLOR)
img = img.astype("float32")
```

<br class="big"><br class="big">

<div class="alert danger"> 
<div class="head">關於 IO 錯誤描述</div>
之前文句中出現 <b>省下那兩次的 IO</b> 的敘述有誤，感謝 Chi Gas 的指正。<br>
在使用 <code class="language-plaintext highlighter-rouge">imdecode()</code> 和 <code class="language-plaintext highlighter-rouge">imencode()</code> 也是<a href="https://github.com/opencv/opencv/blob/8d78400052c9e6b60374364163f234790251b8fb/modules/imgcodecs/src/loadsave.cpp#L758">進行了 IO</a> ，所以用<b>省下 IO</b> 的描述並不正確。
</div>



## 參考資料 
1.  -牧野- (2018-01-25)。[OpenCV-Python cv2.imdecode()和cv2.imencode() 图片解码和编码](https://blog.csdn.net/dcrmg/article/details/79155233) 。檢自 牧野的博客-CSDN博客(2020-02-19)。
2. flamelite (2017-12-28)。[Reading image file (file storage object) using CV2](https://stackoverflow.com/questions/47515243/reading-image-file-file-storage-object-using-cv2) 。檢自 Stack Overflow (2020-02-19)。
3. tbicr (2013-11-16)。[Read file data without saving it in Flask](https://stackoverflow.com/questions/20015550/read-file-data-without-saving-it-in-flask) 。檢自 Stack Overflow (2020-02-19)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期： 2020-08-19</summary>
  <ul>
    <li>2020-08-19 更新 修正錯誤描述</li>
    <li>2020-02-19 發布</li>
  </ul>
</details>