---
title: "【演講筆記】 NVIDIA GTC 2020"
date: 2020-12-31 23:45
is_modified: false
categories:
- 研讀筆記
- AI/ML
tags:
- 演講記錄
--- 

最近聽 [NVIDIA GTC 2020](https://www.nvidia.com/zh-tw/gtc/session-catalog/?search.language=1594320459782004L0mo&tab.catalogtabfields=1600209910618002Tlxt)，順便紀錄下部份場次的筆記。不過有點懶沒有紀錄全部的場次，只紀錄了被指定要分享的場次。

<!--more-->
<center> <img src="https://i.imgur.com/zwd11TS.png" alt="Nvidia GTC 2020"></center>
<center class="imgtext">Nvidia GTC 2020（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/" class="imgtext">NVIDIA</a>）</center>

<br>

##  [A21810RT] 人工智慧助力醫療影像分析
- **講者**： 葉肇元 , 執行長, 雲象科技
- **主要領域**: 醫學影像
- **簡介**：  
    自從 2015 年深層神經網路在圖像識別任務上勝過人類以來，人們一直熱衷於尋找這種強大技術可以產生影響的應用領域。     
    由於易於獲得的數據及其在診斷和治療計劃過程中的關鍵作用，醫學影像吸引了很多關注。值得注意的是，深層卷積神經網路 (CNN) 在診斷糖尿病性視網膜病變，分類皮膚疾病和識別淋巴結轉移性癌細胞方面優於人類專家。  
    令人驚訝的是，CNN 也顯示出能夠使用彩色眼底圖像預測心臟危險因素，並能夠使用組織病理學圖像區分基因型。CNN 的這些超人能力為醫生提供了許多無法預料的機會，以使他們能夠制定更好的診斷和治療計劃。  
    在本次演講中，我將使用 aetherAI 的真實案例來說明人工智慧如何將更高的標準引入醫學影像分析，將傳統的定性實踐轉變為定量實踐並最終對臨床決策過程產生影響的潛力。
- **相關連結**：
    - 演講： [A21810RT](https://www.nvidia.com/zh-tw/gtc/session-catalog/?search=A21810RT&search.language=1594320459782004L0mo&tab.catalogtabfields=1600209910618002Tlxt#/session/1598497404859001a22b)
    - [雲象科技將玻片影像數位化，打造世界級 AI 醫療影像公司 | TechNews 科技新報](https://technews.tw/2019/07/09/aetherai-digitizes-microscope-images-and-possesses-identification-ability-equal-to-that-of-a-pathologist/)       
    <br>
    <center> <img src="https://i.imgur.com/S8Li945.png" alt=""></center>
<br>

醫療影像可分兩個方向進行：  

1. **目前醫師已經在做的事情**：  
    這類事情 AI 通常提供輔助的診斷，常見案例是：
    1. **病灶的偵測**
    2. **疾病狀態的分類**，如：眼底視網膜。
    3. **量化工作**，如：染色細胞計算。
    4. **輪廓描繪**，用以計算最佳路徑，讓腫瘤可以接收到最高放射劑量的治療，確保其他器官不會收到放射治療的影響。
    
    這像的工作會比較好量化，並與醫生的成果做比較。不過這樣的流程會直接介入與干擾醫生工作流程，在推行的阻力比較大。

2. **醫師沒有再做的事情**：  
    1. 醫師只能做**質性**的分析，無法同時做**量化**分析，如：看 FFR-CT 冠狀動脈的血流是否堵塞。
    2. **預測組織的基因型**，可以跳過基因定序的時間
    3. **預測病患的癒後**，如：存活的時間、癒合狀況
    4. **預測免疫治療**，如：是否對治療有反應，避免醫療浪費

<br>

另外講者也分享了他們的實際案例：

1. **台大骨髓影像**：  
    懷疑白血病時，計算骨髓中細胞是否落在正常範圍。玻片須放大 1000x，使用 PA-Net 實例分割模型。

    <center> <img src="https://i.imgur.com/GkPzE6g.png" alt=""></center>
    <br>
    
    與台大標注血液疾病資料集，目前正在準備多國多中心的臨床試驗，準確率 0.948。
    <center> <img src="https://i.imgur.com/J1N4QKm.png" alt=""></center>
    <br>

2. **骨科影像分析**：  
    林口長庚，使用 **Res-UNet** 進行脊椎型態分析，用以表現骨脊治療前後的狀況進行量化的分析，方便進行說明與比較。

    <center> <img src="https://i.imgur.com/oxyTlFU.png" alt=""></center>
    <br>
    
    <center> <img src="https://i.imgur.com/207uayy.png" alt=""></center>
    <br>
    
    不過在一開始時，訓練集僅標注 25 的點，但這樣在訓練時，容易導致胸腔位置的脊椎出現懸空的狀況，因此又請醫生重新標注將節點新增至 49 個點。 
    <center> <img src="https://i.imgur.com/zTx1WOi.png" alt=""></center>
    <br>
    
    訓練結果看起來誤差極小：
    <center> <img src="https://i.imgur.com/hEQaxYW.png" alt=""></center>
    <br>

3. **數位病理玻片**：  
    目前數位病理玻片解析度太高，無法直接進行訓練，必須藉由 Patching base images 的方法，將大圖片拆成 `256*256` 或是 `512*512` 等小圖片，而這些小圖片必須再經由醫生進行標注，以確認每張小圖是否癌細胞，這樣的標注其實非常耗費時間，因此醫生標注的意願不高。
    
    因此他們提出了新了方法，使用了用系統記憶體，以容納整張玻片進行預測。
    
    <center> <img src="https://i.imgur.com/6IjaiDQ.png" alt=""></center>    
    <br>

    兩種方法出來的精確度差異不大：
    <center> <img src="https://i.imgur.com/4QWzX2c.png" alt=""></center>    
    <br>
    
    不過這樣的方法因為跨 GPU 記憶體與系統記憶體的使用，當資料在這兩種記憶體上移動時，速度會受限於 PCI Bus，因此在網路設計時需要特別注意。 
    <center> <img src="https://i.imgur.com/IuQNC0g.png" alt=""></center>
    <br>
    
在將人工智慧引入醫療影像分析的過程中，會遇到的挑戰有：
1. 資料蒐集與標注
2. 資源問題（人力、算力）
3. 醫療器材如何驗證
4. 實際合用標準
5. 如何整合工作流程
6. 商業模式

而若成功將人工智慧導入醫療影像後，會得到下列好處：
1. 加速醫療數位化
2. 加速工作流程
3. 對只能質性分析的資料進行量化分析
4. 更多資訊參考
5. 優化偏鄉醫療  

<br><br>

##  [A21160RT] 如何優化醫療影像 AI 訓練成果 
- **講者**： 吳億澤, 專案經理, 奧啓迪科技股份有限公司
- **主要領域**： 深度學習推論 - 最佳化與部署
- **簡介**：   
    深度學習近年在醫學影像的領域裡有著亮眼的發展，在許多應用上達到接近於人類專家的表現。但是醫學影像深度學習始終面臨一個挑戰—有效資料量的不足。資料增幅（data augmentation）及遷移式學習（Transfer Learning） 是能有效克服少量資料問題的兩種方式。  
    在這場演講中，我們將分享 DeepQ 使用在醫學影像 AI 所累積的經驗，以及我們如何有效運用上述工具達到較佳的結果，讓醫療人員以最簡單的方式驗證題目的發想，開發有價值的 AI 模型。
- **相關連結**：
    - 演講： [A21160RT](https://www.nvidia.com/zh-tw/gtc/session-catalog/?search=A21160RT&search.language=1594320459782004L0mo&tab.catalogtabfields=1600209910618002Tlxt#/session/1596525924759001aC7E)  
    <br>
    <center> <img src="https://i.imgur.com/Ty2aRm6.png" alt=""></center>
<br>
    
在演講的一開始，講者提到將人工智慧引入醫療影像會遭遇的困難：
<center> <img src="https://i.imgur.com/VOICuiE.png" alt=""></center>
<br>

為了克服上述的原因，講者提出了 DeepQ AI Platform，在這個平台上統一了標注工具，並提供標注人員協作的環境以加速開發的進行：

<center> <img src="https://i.imgur.com/NMNUeK9.png" alt=""></center>
<br>

在這頁講者稍微介紹了他們的平台的工作流程，大抵上是平台可以協助資料標記後，在將標注完的資料用於訓練，而訓練所得的模型可用於 inference 或是導回平台以優化標注過程：
<center> <img src="https://i.imgur.com/kcbfnOF.png" alt=""></center>
<br>

目前平台可以支援的工作如下：

<center> <img src="https://i.imgur.com/l4v6VJm.png" alt=""></center>
<br><br>
 
在後面講者提到了在醫療影像中會面對到的資料量問題，主要是源於醫療影像的特殊性，這些影像會來自不同醫療設備、儲存方式、不同參數設定，且又涉及病患的隱私...等因素，導致在醫療影像上的資料少於其他 CV 領域，而資料量的多寡直接影響的是模型的精準度：

<center> <img src="https://i.imgur.com/5tUtYS0.png" alt=""></center>
<br>

而為了克服資料量所帶來的影響，在 DeepQ AI Platform 中引入了 Auto ML engine，開發團隊在 TWCC 上進行自動調參引擎優化：

<center> <img src="https://i.imgur.com/G5mNuwg.png" alt=""></center>
<br>

而另一個方式克服資料量影響，則是藉由 flip、 crop、 translation、 rotate、 scale...等，資料增幅以增加資料量，開發團隊從中挑選了有效果的增幅加入他們的資料處理中：
<center> <img src="https://i.imgur.com/pE2kVzG.png" alt=""></center>
<br>

另一種方式則是則是遷移式學習（Transfer Learning），即把其他領域中的知識遷移到醫療影像領域中來，提高醫療影像分類效果，而不需要花大量時間去標注醫療影像的數據。

<br><br>

##  [A21194RT] 智慧軟體醫材與聯邦式學習
- **講者**： 許銀雄, 總處長, 宏碁股份有限公司
- **主要領域**: 醫學影像
- **簡介**：   
    AI 方興未艾，智慧醫療匯集台灣醫療及資通訊兩產業的跨業合作是台灣產業升級的契機。VeriSee DR 是針對糖尿病視網膜病變的 AI 診斷軟體，由宏碁與台大醫院合作研發，其臨床試驗計畫已取得 TFDA 核可，預計今年成為 TFDA 核可的 Class II 軟體醫材。  
    AI 是 VeriSee DR 的核心及關鍵技術，醫療資料及生態系有其特殊性，在不同醫院或聯邦醫院間如何有效讓 AI 模型精進並兼顧隱私保護是重要的議題。聯邦式學習是一個可能的方案。我們將介紹如何應用 GPU 、 Clara SDK 以及聯邦式學習來加速 AI 模型的開發以及在醫療場域實現。
- **相關連結**：
    - 演講： [A21194RT](https://www.nvidia.com/zh-tw/gtc/session-catalog/?search=A21194RT&search.language=1594320459782004L0mo&tab.catalogtabfields=1600209910618002Tlxt#/session/159663172429800107l3)    
    <br> 
    <center> <img src="https://i.imgur.com/ptVbC3t.png" alt=""></center>
<br>

這邊特別說明，Medical AI 也是醫療器材，它的販售必須經過主管機關的核准，以確保其**安全性**及**有效性**。

這邊講者提到由於 AI 的侷限性，無法包山包海，但若把問題限定 AI 的效果會不錯。在這開發過程中，需要跨領域合作：醫生標注、工程師訓練、醫療器材執照取得及商品化後的行銷。

<center> <img src="https://i.imgur.com/HWxZ8Fj.png" alt=""></center>
<br>

順便自賣自誇的介紹了下他們商品：
1. 台灣 TFDA 認證的第一個診斷軟體 GMP 工廠  
2. ISO 13485 認證的台灣第一個診斷軟體品質系統

<br>

接下來講者介紹了他們的彩色眼底圖輔助診斷糖尿病視網膜病變 Versee DR。在糖尿病患的照護流程中，在定期的門診中醫生可以使用眼底攝影進行檢查，若被診斷出有病變的風險，則進行轉診。

<center> <img src="https://i.imgur.com/97XvsT9.png" alt=""></center>
<center> <img src="https://i.imgur.com/Zl8iF06.png" alt=""></center>
<br>


接下來，介紹了 AI 模型的開發流程，看起來與一般開發流程差不多，比較需要特別注意的是，資料的取得與使用需要經由**人體試驗倫理委員會（Institutional. Review Board，IRB）** 的審核：

<center> <img src="https://i.imgur.com/N1mKG08.png" alt=""></center>
<br>
<center> <img src="https://i.imgur.com/BTNvZCR.png" alt=""></center>

<br><br>

在這篇中也提及了資料量不足的問題，在這篇也提到資料的不足主要源自於：設備費用高、數量少、嚴重不平均、不同廠牌設定...等。

<center> <img src="https://i.imgur.com/L4dYXVK.png" alt=""></center>
<br>

此外，這些醫療資料屬於特別個資，在使用上需要審慎使用：

<center> <img src="https://i.imgur.com/A58lBDb.png" alt=""></center>
<br>


在這篇針對醫療影像資料不足的狀況介紹了幾種方法：
1. 更改模型或演算法，讓只需要少量的樣本就能快速學習，例如：**小樣本學習（Few-shot Learning）**  
   更改可以從三個方向來進行：Mode Based、Metric Based 和 Optimization Based

   <center> <img src="https://i.imgur.com/1Nj2uGI.png?1" alt=""></center>
   <br>    
 
2. 與其他機構，取得更多資料，即：**聯邦式學習**  
   聯邦式學習是由 google 提出的，這個是 Clara Deploy SDK 的新功能。

   <center> <img src="https://i.imgur.com/1a9Mkm0.png" alt=""></center>
   <br>

<div class="alert info"> 
<div class="head"參考資料</div>
1. <a href="http://www.ipshop.xyz/12632.html">小樣本學習（Few-shot Learning）綜述</a><br>
2. <a href="https://medium.com/sherry-ai/%E8%81%AF%E7%9B%9F%E5%BC%8F%E5%AD%B8%E7%BF%92-federated-learning-b4cc5af7a9c0">聯盟式學習(Federated Learning). 突破隱私權的重圍！</a><br>
</div>
    
<br>

但也說明跨醫院的資料在使用上會遇到的問題：

1. 來自不同醫院以及不同醫生，要如何成為訓練資料？
2. 跨廠牌的設備、不同的設定、操作醫生或技師的也會有不同的喜好

<br>

因此若想進行跨醫院的訓練，必須：
1. 統一的 Terminology 
2. 統一的診斷標準
3. 統一得標注方式
4. 統一得標注儲存方式 
  
<br>

聯邦是學習在醫院實際應用的挑戰：
1. 不同體系的醫院有可能進行這樣的合作
2. 權利與義務？結果屬於誰？要如何分配？
3. 誰來主導？

<br>

另外，講者也做一些效能的比較與測試，供大家參考：
<center> <img src="https://i.imgur.com/hml91ep.png" alt=""></center>
<br>

<center> <img src="https://imgur.com/XjIxaDn.png" alt=""></center>
<br>

<center> <img src="https://imgur.com/5k4a8Li.png" alt=""></center>
 
<br><br>

##  [A21695RT] 智慧醫院 AI 協助診斷、佈署、開發經驗分享
- **講者**： 黃崇堯 , 主治醫師, 馬偕紀念醫院
- **主要領域**: 強化學習
- **簡介**：   
    馬偕紀念醫院放射線科 Clara AI 使用經驗分享： 
    1. 放射線科醫師在研華電子的 Medical T4 box 利用 Clara SDK 與 jupyterlab，匯入其他平台開發的 AI model 作為 pre-train model，使用本地端的醫療影像完成影像不離院的 Transfer learning 
    2. 使用 NGC container 進行 AI model 的開發；使用 DIGITS 對於醫療影像進行 Classification，輸出 AI model 並置入台灣商之器 EBM 的 SoliPACS server 進行演算，改善放射科醫師的工作流程
- **相關連結**：
    - 演講：[A21695RT](https://www.nvidia.com/zh-tw/gtc/session-catalog/?search=A21695RT&search.language=1594320459782004L0mo&tab.catalogtabfields=1600209910618002Tlxt#/session/1598349593658001Eox4)    
    <br> 
    <center> <img src="https://i.imgur.com/PnSUJYG.png" alt=""></center>

<br>

目前馬偕醫院導入**輔助流程**用以：

1. 肺部節結
2. 乳房篩檢
3. 胸部 X 光片

<center> <img src="https://imgur.com/QsQSqFl.png" alt=""></center>
<br>

講者本身是放射科醫生，他表示對於訓練良好的放射線科醫生而言，這些對於輔助是沒有用的， AI 診斷結果又慢又不正確，與其去使用它們，自己打標記還比較快。

且對其他科別的醫生來說，錯誤的診斷會產生更糟糕的**錨定效應 Anchoring Effect**，容易導致醫生決策受到干擾。

<div class="alert info"> 
<div class="head">錨定效應 Anchoring Effect</div>
人類在進行決策時，會過度偏重先前取得的資訊（這稱為錨點），即便這個資訊與決策無關。在進行決策時，人類傾向於利用此片斷資訊（錨點），快速做出決定。在接下來的決定中，再以第一個決定為基準，逐步修正。 <br> <br>

但是人類容易過度利用錨點，來對其他資訊與決定做出詮釋，當錨點與實際上的事實之間的有很大出入，就會出現當局者迷的情況。
</div>
<br>

另外涉及不同職級（醫學院學生、住院醫師、住院總醫師、主治醫師）、不同領域，導致除了放射線科醫師外，其他醫生不一定能判斷 AI 資訊的對錯，例如：腸胃科醫生不一定會看胸腔 X 光，所以不建議將 AI 診斷結果直接給其他科醫生看。

因此在講師不建議開放結果給臨床醫師，而結果又對放射科又沒有幫助的情況下，因此 AI 在醫院的角色會趨向分類與預警。
<br>

在目前的的流程的中，健檢車會到社區進行健檢，待健檢完畢後，資料會被帶回醫院，在行政人員完成整理後，資料會被送往放射科醫生進行標注，由於每次出診後待回來的資料眾多，等待資料標注完畢寄回給受檢者後，往往需要耗時 2 周與 1 月。但若真有任何異常情況，這樣的時間間隔可能錯過治療得黃金期。


<center> <img src="https://i.imgur.com/64UfVsP.png" alt=""></center>
<br>

在導入 AI 流程後，收回來的資料，會先經由 AI 進行預測挑選出分數較高的病患，這病患會有比較高優先權，放射科醫生會優先標注，並先寄送出。

<center> <img src="https://i.imgur.com/ICkfrJ2.png" alt=""></center>
<br><br>

接下來講者介紹了他們所引入的平台— Quibim，在 Quibim Precision 中利用十六個類神經網絡訓練了超過 1,000,000 張圖像的資料庫，以估評估胸部 X 光存在以下病變的可能性：肺塌陷、心臟擴大、積液、浸潤、腫塊、結節、肺炎、氣胸、肺實變、水腫、肺氣腫、纖維化、胸膜增厚和疝氣。

不過雖說平台提供這麼多的疾病檢測，但講者說其實他們只會用到**腫塊**與**結節**兩種，其他的項目並不會影響醫療決策。順便吐槽了下，他們不需要 AI 告知病患是否肺炎，這症狀僅需藉由臨床症狀與聽診器的輔助，就可以判斷。

<center> <img src="https://i.imgur.com/TYIJrLQ.png" alt=""></center>
<br>

另外，他說明選擇 Quibim 的原因：
1. 具備 Task engine 與 Data Mining
2. 可以加到其他 AI 模組
3. 可以深入整合到現有系統不是取代


舉例來說，當用 Quibim 完成預測之後，可以利用 Data Mining 找出患病機率，如：0.75，較高病例，開發團隊將這些病例放置到現有系統的警告區，警告放射科醫生應先看這份報告。

<center> <img src="https://imgur.com/dv4yKAq.png" alt=""></center>
<br>

對放射科醫生來說，他們每天至少撰寫上千份的報告，如果每次都要引入新的系統，他們就得必須重新學習新的系統操作，對他們而言是個負擔，因此他們會非常希望的是整入現有系統，而非導入新的系統。

<center> <img src="https://i.imgur.com/ekXTtct.png" alt=""></center>
<br>

或許會有疑問，為何不在 AI 診斷出來後直接通報醫生，而是預警放射科醫生？針對這點講者提到一點：**只有人類可以預警人類（Only Human can warn Huamn）**。

會這樣說是因為醫療體系是由人所構成的系統，在一般的檢驗過程中若出現的異常數值，他們一開始會先回頭檢查採樣、保存的過程是否符合規定，避免出現系統性的錯誤。因此這樣的檢查也被使用到了 AI 的應用。

<center> <img src="https://i.imgur.com/LZeOSx5.png" alt=""></center>
<br>

演講的後期做了一些比較，首先先比較了資料移出院外或留在院內實作的優缺點：

- 優點：
    1. 專業分工，各司其職

- 缺點：
    1. 資料移出院外的資料要去識別化
    2. 醫院其實不會想要把資料移出院外
    3. IRB 期限資料出不去

<br>

另外，講者還在一些公開平台上做了測試，並比較優缺點：

<center> <img src="https://i.imgur.com/6UauNJd.png" alt=""></center>
<br>
<center> <img src="https://i.imgur.com/7DizSgM.png" alt=""></center>
<br>
<center> <img src="https://i.imgur.com/E2jTawx.png" alt=""></center>

<br><br>

##  [A21188] 以深度學習為基礎的影像分析平台 
- **講者**： 吳鵬榮, 工程副總, 鐵雲科技 
- **主要領域**: 物聯網/ 5G/ 邊緣運算 
- **簡介**：  
    我們將介紹一個以深度學習為基礎的人工智慧影像分析與搜尋系統，該系統內建多個人工智慧分析引擎，可以從攝影機串流中即時分析人、車、物等資訊。使用者可以從大量的資料中針對車輛的種類、廠牌、型號、車牌、顏色進行即時搜尋，也可以針對人的年紀、性別、人臉辨識結果、衣著顏色、抑或是隨身物品如背包和公事包進行即時搜尋。其他如槍枝偵測、個人防護裝備偵測 (PPE) 、入侵偵測等應用也包含其中。我們會介紹該系統如何運作，也將透過實際佈建的系統進行展示。
- **相關連結**：
    - 演講： [A21188](https://www.nvidia.com/zh-tw/gtc/session-catalog/?search=A21188&search.language=1594320459782004L0mo&tab.catalogtabfields=1600209910618002Tlxt#/session/1596599680443001MpjE)
    - 鐵雲科技/VAIDIO：[官網](https://www.ironyun.com/)
    <br>
    <center> <img src="https://i.imgur.com/KX8zys4.png" alt="以深度學習為基礎的影像分析平台"></center>
    <center class="imgtext">以深度學習為基礎的影像分析平台（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/session-catalog/?search=A21188&search.language=1594320459782004L0mo&tab.catalogtabfields=1600209910618002Tlxt#/session/1596599680443001MpjE" class="imgtext">演講 A21188 </a>）</center>
<br>

主要介紹了一個名為 VAIDIO 的影像分析與搜尋系統，用來管理大量攝影機，並提供搜尋與即時告知的功能。

有些企業雖目前已經佈建了大量的監控設備，但因缺乏好的觀看方法，因此雖然架構了攝影機卻極少觀看與利用。雖有建制傳統的影像辨識引擎，但這類引擎的誤報率偏高，使得安全人員必須費時一一檢查這些警報，導致營運成本增加；而有些搜尋平台的問題，則是使用上較沒效率與彈性，它們是當要搜尋目標時，才會將影像上傳到平台中，並等它分析完畢後才能進行搜尋。
 
VAIDIO 是一套深度學習的影像辨識引擎，在該平台上可以同時進行行人屬性辨識、車輛屬性辨識、人臉辨識、車牌辨識...等辨識，並將上述辨識結果相互參照，進行即時分析與告警，且降低誤報率。這些辨識引擎也可依照所需的場域與應用來決定開啟的項目。

且該平台支援標準的 IP 攝影機與 NVR，可以相容於使用者目前現有設備，也將這些影像拉進來 VAIDIO，即可對這些影像分析；另外也有跟影像管理系統（Video Management System，VMS）進行整合，方便使用者進行影像調閱。

VAIDIO 是運行在使用者自己的資料中心，並不用將影像上傳到雲端。該平台支主要貢獻準確度、易用性。


<center> <img src="https://i.imgur.com/54Isft9.png?1" alt="以深度學習為基礎的影像分析平台"></center>
<br>
<center> <img src="https://i.imgur.com/lzTxzIy.png" alt=""></center>
<br>
<center> <img src="https://i.imgur.com/NwayqzP.png?1" alt=""></center>
<br>
[影片傳送門](https://f.hubspotusercontent30.net/hubfs/7620582/Marketing%20Materials/Website/Videos/IronYun-Intro.mp4)  
<center> <a href="https://f.hubspotusercontent30.net/hubfs/7620582/Marketing%20Materials/Website/Videos/IronYun-Intro.mp4"> <img src="https://i.imgur.com/l7SiTxt.png?1" alt=""></a> </center>



<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-12-31</summary>
  <ul>
    <li>2020-12-31 發布</li>
    <li>2020-11-18 完稿</li>
    <li>2020-10-16 起稿</li>
  </ul>
</details>