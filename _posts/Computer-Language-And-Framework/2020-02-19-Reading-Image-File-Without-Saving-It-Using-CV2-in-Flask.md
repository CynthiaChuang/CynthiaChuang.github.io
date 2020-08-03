---
title: 使用 OpenCV 不存檔直接讀入 Flask 所傳進的圖片
date: 2020-02-19
modified: 2020-02-19
categories:
- Computer-Language/Framework
tags:
- Python
- flask
- OpenCV
--- 

在實做 flask 時，我透過 curl 指令取得了張圖片，但它的資料格式是 `werkzeug.datastructures.FileStorage`，不能直接用 `cv2.imread` ，但我又不想跟範例程式碼一樣，先存出再讀回，有點浪費 I/O 時間。
  
所以找了其他的方法來實做。


<!--more-->
<br><br> 

## 先存出再讀回
一般常見的範例都會先存出再讀回，將像下面程式碼一樣。

```python
data =request.files['file']
filename = secure_filename(file.filename) 
filepath = os.path.join("/tmp", filename);
file.save(filepath)
cv2.imread(filepath)
```

<br><br> 

## 直接從記憶體中讀取
不過, 我想省下那兩次的 IO。所以找到的 `cv2.imdecode`，根據解釋：

<div class="alert info"> 
<div class="head">cv2.imdecode()</div>
函數從指定的記憶體快取中讀取數據，並把數據解碼成圖像格式；主要用於從網路傳輸數據中恢復出圖像。
</div>

<br> 看到<span class="highlighting">網路傳輸數據</span>眼睛就亮起來了有沒有！當下就決定把 `cv.imread` 換成 `cv2.imdecod`，完整程式碼如下：

```python
filestr = request.files['file'].read()
npimg = numpy.fromstring(filestr, numpy.uint8)
img = cv2.imdecode(npimg, cv2.IMREAD_COLOR)
```
<br>

### 資料型態
用這方式讀進來的資料型態是 `uint8`，在我的應用中，我會接著喂進 TensorFlow 做 predict，結果會跳出

<div class="alert danger"> 
<div class="head">TensorFlow TypeError</div>
Value passed to parameter input has DataType uint8 not in list of allowed values: float16, float32
</div>


<br> 所以我又在最後加了行型態轉換：


```python
filestr = request.files['file'].read()
npimg = numpy.fromstring(filestr, numpy.uint8)
img = cv2.imdecode(npimg, cv2.IMREAD_COLOR)
img = img.astype("float32")
```


 

<br><br> 

## 參考資料 
1.  -牧野- (2018-01-25)。[OpenCV-Python cv2.imdecode()和cv2.imencode() 图片解码和编码](https://blog.csdn.net/dcrmg/article/details/791552332) 。檢自 牧野的博客-CSDN博客(2020-02-19)。
2. flamelite (2017-12-28)。[Reading image file (file storage object) using CV2](https://stackoverflow.com/questions/47515243/reading-image-file-file-storage-object-using-cv2) 。檢自 Stack Overflow (2020-02-19)。
3. tbicr (2013-11-16)。[Read file data without saving it in Flask](https://stackoverflow.com/questions/20015550/read-file-data-without-saving-it-in-flask) 。檢自 Stack Overflow (2020-02-19)。