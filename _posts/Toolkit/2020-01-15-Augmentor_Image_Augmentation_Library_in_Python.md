---
title: Augmentor：影像數據增強工具庫
date: 2020-01-15
categories:
- AI/ML
- Toolkit
tags:
- 資料處理與增強
- CV
- Python
--- 

紀錄一下最近使用的影像資料增強工具 - [Augmentor](https://augmentor.readthedocs.io/en/master/)，可用來做訓練圖像生成與雜訊添加...等

<!--more-->

<br> 

## 簡介
<center> <img src="https://i.imgur.com/thTW1NE.png" alt="AugmentorLogo"></center>
<center class="imgtext">   Augmentor Logo （圖片來源: <a href="https://github.com/taki0112/Augmentor" class="imgtext">Augmentor</a>）</center>
<br>

根據[文件簡介](https://augmentor.readthedocs.io/en/master/#augmentor)，Augmentor 是一個 Python Package，用於幫助機器學習任務的影像增強與生成。它主要是一種資料增強工具，但也提供了些基本的影像預處理功能。

影像處理[主要功能](https://augmentor.readthedocs.io/en/master/userguide/mainfeatures.html)包含：透視偏斜（Perspective Skewing）、彈性變形（Elastic Distortions）、
旋轉（Rotating）、推移（Shearing）、裁剪（Cropping）與鏡像（Mirroring）。當然最基礎的亮度、對比也有[支援](https://augmentor.readthedocs.io/en/master/code.html)。
 
由於影像增強通常是多階段的處理過程，因此 Augmentor 使用採用基於 pipeline 的處理方式，按使用者所選擇處理動作，依序添加到 pipeline 中。執行過程中，影像會被送進 pipeline 中，依序套用各個增強操作，最終形成新的影像。

另外，每個動作都可自定義套用機率，在影像經由 pipeline 時，會按機率決定是否套用該操作，以增加資料的多樣性。


↑ 以上全出自於 [Augmentor文件](https://augmentor.readthedocs.io/en/master/index.html)， Google 翻譯友情贊助 XDDDD

<br> 

## 安裝
我是透過 pip 安裝，所以還挺簡單，一條指令就搞定：

```shell
$ pip install Augmentor
```

<br><br> 
 
## 使用方法
 
### 純圖片增強
幾本上，就只有三個步驟：
<br>

首先，初始化 pipeline 物件，並指向所需處理影像的目錄
```python
import Augmentor
p = Augmentor.Pipeline("./images")
```
<br>

接下添加，所需的影像增強操作
```python
p.random_contrast(probability=0.5, min_factor=0.3, max_factor=0.8)
p.random_brightness(probability=0.5, min_factor=0.5, max_factor=1.0)
p.zoom(probability=0.5, min_factor=1.01, max_factor=1.03)
p.random_distortion(probability=0.5, grid_height=3, grid_width=3, magnitude=6)
p.skew(probability=0.5, magnitude=0.12)
p.random_erasing(probability=0.5, rectangle_area=0.11)
p.rotate(probability=0.5, max_left_rotation=4, max_right_rotation=4)
```
<br>

最後，指定增強後圖片數目總量
```python
p.sample(2000)
```

<br>

### 含圖片與標籤的增強
但通常在在進行模型訓練時，所取得資料不只圖片本身，還會有圖片相對應的標籤，此時 [`Augmentor.Pipeline` ]( https://augmentor.readthedocs.io/en/master/code.html#Augmentor.Pipeline.Pipeline)就不適用，應該改用 [`DataPipeline`](https://augmentor.readthedocs.io/en/master/code.html#Augmentor.Pipeline.DataPipeline)。

但需注意的是， `Pipeline` 所傳入的參數是<span class='highlighting'>圖片位置</span>，但 `DataPipeline` 傳入是<span class='highlighting'>圖片</span>。

```python
import cv2
import Augmentor

batch_imgs = []  
batch_texts = []   

for i, file in enumerate(img_files):
batch_imgs.append(cv2.imread(file))
texts.append(file)

# batch_imgs shape = (bs, 3)             
p = Augmentor.DataPipeline(batch_imgs, batch_texts)
```
<br>

原本以為這樣就可行了，但卻發現影像過了 pipeline 後，套用的影像增強不如預期，詳讀文件也沒找到可用的訊息，只好去細看他們所提供的[範例](https://github.com/mdbloice/Augmentor/blob/master/notebooks/Multiple-Mask-Augmentation.ipynb)。


果然在第五行的的程式碼中找到蛛絲馬跡，範例中是這樣的：
```python
images = [[np.asarray(Image.open(y)) for y in x] for x in collated_images_and_masks]
```
<br>

發現它讀入圖片後，外面有多一層 array，所以當這個步驟結束時，預期的維度會是 3 維，而非我的 2 維。依照這個邏輯，再將圖片放入 `DataPipeline` 前，先<span class='highlighting'>擴展維度</span>：
```python
import cv2
import Augmentor

batch_imgs = []  
batch_texts = []   

for i, file in enumerate(img_files):
batch_imgs.append(cv2.imread(file))
texts.append(file)

batch_imgs = np.expand_dims(batch_imgs, axis=1)
# batch_imgs shape = (bs, 1, 3)   
p = Augmentor.DataPipeline(batch_imgs, batch_texts)

```

<br>

最後再取出增強後圖片，與相對應的標籤。別忘了，記得把剛剛擴展維度的<span class='highlighting'>維度降回去</span>：
```python
batch_imgs, batch_texts = p.sample(batch_size)
batch_imgs = np.squeeze(batch_imgs, axis=1)
```

如此出來的增強影像，看起來正常多了。

<br>

### 自定義增強器

說到增強，我想讓我的模型多看看不同色調下的圖片，所以翻了翻文件，好不容易找到 [`HSVShifting`](https://augmentor.readthedocs.io/en/master/code.html#Augmentor.Operations.HSVShifting)，結果竟然...

<br><font color="#f00">CURRENTLY NOT IMPLEMENTED.</font>
<br><font color="#f00">CURRENTLY NOT IMPLEMENTED.</font>
<br><font color="#f00">CURRENTLY NOT IMPLEMENTED.</font>     

登愣！！

<br>

只好來研究如何[自定義增強器](https://augmentor.readthedocs.io/en/master/userguide/extend.html)

#### 建立一個新的操作子類別

在建立一個新的操作子類別時，有四個地方需要注意的

1. 子類別要繼承 [`Operation`](https://augmentor.readthedocs.io/en/master/code.html#Augmentor.Operations.Operation)。
2. 初始化時，同時記得呼叫父類別 `__init__()`。
3. 重載（Overload）`perform_operation()`，以自定義新的增強。
4. `perform_operation()` 的回傳資料型態必須是 `PIL.Image`。

<br>

快速實做版簡單的 `HSVShifting` 進行驗證：
 
```python
class CustomizeHSVShifting(Operation):
    def __init__(self, probability, max_h_shift=359, min_h_shift=1, max_s_shift=100, min_s_shift=0):
        Operation.__init__(self, probability)
        self.max_h_shift = max_h_shift
        self.min_h_shift = min_h_shift
        self.max_s_shift = max_s_shift
        self.min_s_shift = min_s_shift

    def perform_operation(self, images):
        def do(img):
            img = np.array(img)
            hsv_image = cv2.cvtColor(img, cv2.COLOR_RGB2HSV)

            h_shift = random.randint(self.min_h_shift, self.max_h_shift)
            hsv_image[:, :, 0] = hsv_image[:, :, 0] + h_shift

            s_shift = random.randint(self.min_s_shift, self.max_s_shift)
            hsv_image[:, :, 1] = hsv_image[:, :, 1] + s_shift

            return Image.fromarray(cv2.cvtColor(hsv_image, cv2.COLOR_HSV2RGB))

        augmented_images = []

        for image in images:
            augmented_images.append(do(image))

        return augmented_images
```

注意，perform_operation 傳入的會是圖片陣列，而非單張圖片，別被[文件](https://augmentor.readthedocs.io/en/master/userguide/extend.html)中的 <span class='highlighting'>image</span> 這個字給騙了！實做時可以與其他功能的[實做](https://augmentor.readthedocs.io/en/master/_modules/Augmentor/Operations.html#RandomBrightness)進行對照。

<br>

#### 將操作子類別加到 pipeline 中

呼叫 [`add_operation`](https://augmentor.readthedocs.io/en/master/code.html#Augmentor.Pipeline.Pipeline.add_operation) 來進行添加：

```python
p.add_operation(CustomizeHSVShifting(probability=0.5, max_h_shift=330, min_h_shift=30, max_s_shift=30, min_s_shift=0))
```

如此一來就將自定義的增加入 pipeline。
 



 
<br><br> 

## 參考資料 
1. [Augmentor 0.2.6 documentation｜Augmentor](https://augmentor.readthedocs.io/en/master/)
2. [taki0112/Augmentor: Image augmentation library in Python for machine learning.｜GitHub](https://github.com/taki0112/Augmentor)
3. [Extending Augmentor｜Augmentor 0.2.6 documentation](https://augmentor.readthedocs.io/en/master/userguide/extend.html)
4. [資料增強利器–Augmentor｜程式前沿](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/463180/) 
5. [非常好用的Python图像增强工具，适用多个框架｜知乎](https://zhuanlan.zhihu.com/p/81952280) 
