---
title: "【演講筆記】NVIDIA GTC 2022"
date: 2022-05-06 02:10
is_modified: false
categories:
- "智慧計算 › 人工智慧"
- "應用資訊 › 生醫資訊"
tags:
- AI/ML
- 藥物設計
- 醫學影像
- 讀書筆記
- 演講
--- 

又到了 GTC 的時候，去年 2021 沒有做，今年的份可得補上了。但一樣只記錄了被指定要分享的場次。

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/GafrKch.png" alt="">
    Nvidia GTC 2022（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/">NVIDIA</a>）
</p>



## [S42631] Dawn of a New Era of Discovery - The Convergence of AI and Life Sciences
- **中文標題**： 探索新時代的曙光:人工智慧與生命科學的結合
- **講者**： 
    - Rory Kelleher, Global Director, Business Development, Healthcare & Life Sciences, NVIDIA
    - Renee Yao, Global Healthcare AI Startups, NVIDIA
- **主要領域**：醫療保健與生命科學  
- **主題分類**：藥物發現、基因組學  
- **簡介**：  
    Learn why now is an unprecedented time in computational biology and drug discovery, and the areas in which NVIDIA and the ecosystem are innovating to bring forth these new capabilities. We'll highlight examples of recent breakthroughs across life sciences, explore the methods and techniques used to enable such advancements, and bring awareness to relevant libraries, frameworks, models, and applications for developers to be aware of. You'll also learn about top startups operating in AI-driven drug discovery and life sciences space and hear about relevant sessions available as part of the HCLS Dev Summit for continued learning opportunities.
- **相關連結**：  
    - 演講： [S42631](https://events.rainfocus.com/widget/nvidia/gtcspring2022/sessioncatalog/session/16435910953920018fVl)   
    <p class="illustration">
    <img src="https://i.imgur.com/wq07B95.png" alt="">
    </p>
<p class="paragraph-spacing"></p>

這場是 Nvidia 自己的演講，可分成兩個部分：**生物醫學的發展**與**創業投資現況**。

講者認為未來幾十年將是生物學的黃金時代，而這一切都將由 AI 所驅動，這可以由三個面向來分析：
1. 當今生物醫學數據集的**爆炸式增長**：
    <p class="illustration">
    <img src="https://i.imgur.com/sAk4Uah.png" alt="">
    </p>

    - 目前世界上 30% 的資料是由醫療照護產業所產生    
    - 基因資料預期到 2025 年將產生 40 EB 的數據（人類口語詞彙約 5EB的數據）
    - AlphaFold release, 蛋白質結構資料增加
    - 表型篩選因資料集、細胞成像和電腦視覺再度崛起  
        P.S. 表型篩選的藥物發現方法就是製藥裡面的神農嘗百草。  

<p class="paragraph-spacing"></p>

2. 深度學習和機器學習技術正在廣泛適用於更廣泛的問題
    > **Q**: 資料量太龐大如何從中篩選出有意義的東西？       
    > **A**: 將這些龐大的資料集與強大的模型、分析工具相結合。      

    <p class="illustration">
    <img src="https://i.imgur.com/4tj1pIM.png" alt="">
    </p> 

    - 深度學習技術不在侷限於 predicting 與 ranking，而是視為強大的通用分析、計算工具。  
    - （略網路架構演化歷），Transformer model 是其中重要的突破，它是在藥物設計與開發的流程及傳統語言處理皆被採用，包含文獻搜索、蛋白質結構預測、圖像分析。     
        - Transformer挑戰：        
            <p class="illustration">
            <img src="https://i.imgur.com/SoEvERZ.png" alt="">
            </p>   
            
            1. 它們是一個需要訓練的計算密集型模型，它們需要大量的計算。
            2. 是編碼和擴展這些模型是一項艱鉅的任務。 
       
       
<p class="paragraph-spacing"></p>

3. 多樣工具的衍生  
    - 因模型的成功，進而衍生出大量工具，如 AlphaFold、RoseTTAFold  
    - 除預測工具的進展外，新生成技術可創建在資料庫之外具有所需特性的新分子，導致化合物資料庫正在迅速增長到數萬億甚至更多。
      <p class="illustration">
        <img src="https://i.imgur.com/BfjEDNS.png" alt="">
      </p>               
    - 導致模擬工具的瓶頸，需要縱向擴展與橫向擴展其計算能力。將傳統的基於物理的模擬與現代深度相結合，提供了百萬倍的模擬性能能力，導致該領域的研究幾乎呈指數增長。  
       <p class="illustration">
        <img src="https://i.imgur.com/T1qT0HH.png" alt="">
      </p>


<p class="paragraph-spacing"></p>

總結原因：
1. 不斷增長的生物資料集。  
2. 顯著的演算法進步提供了新的分析能力。  
3. 模擬技術的進步，將幫助我們更深入了解疾病並以為這些疾病設計新療法。  

<p class="paragraph-spacing"></p>

恩...最後誇了下 Nvidia 自己
1. 它們提供了硬體資源，滿足數據中心、規模計算的需求。  
2. 提供 Nvidia 開發人員工具（如 package、api 和 sdks）有助於加速跨行業使用的演算法。
    <p class="illustration">
        <img src="https://i.imgur.com/6yybCH3.png" alt="">
    </p>

創業投資現況
- 醫療保健和生命科學行業的資金和增長創歷史新高
    - 570 億美元用於投資數位健康，430 億美元用於投資生物技術新創公司，在過去十年中增長了 21.3%，154 家新創公司獲得了巨額融資（mega-round funding, 籌集到1億美元或更多資金）。  
      <p class="illustration">
        <img src="https://i.imgur.com/FmoVRqQ.png" alt="">
      </p>
     - 許多新創公司正在使用AI進行藥物開發、設計和 discovery。 
     - 另一家新創公司使用AI來簡化蛋白質設計並支持製藥和生物技術公司能夠在幾分鐘內使用經過數十億氨基酸訓練的 AI 語言模型來探索蛋白質序列
     - 一家臨床前藥物發現公司使用 Nvidia gpus 來查看訓練和推理。
     - 一些他們的方案、SDK、Tool 的協助案例，就略過了吧



## [S42424] Deep Learning Demystified for the BioPharma Ecosystem
- **中文標題**：解密生物製藥生態系的深度學習
- **講者**： 
    - Siddharth Kotwal, Practice Head - AI and Embedded Computing, Quantiphi
- **主要領域**：醫療保健與生命科學  
- **主題分類**：藥物發現、基因組學  
- **簡介**：  
    The combination of massive datasets, accelerated computing, and novel machine learning algorithms has led to the development of data-driven decision-making systems in health care and life sciences. The volume of unstructured datasets generated across the life cycle of computational biology and drug discovery has grown exponentially and presents an opportunity for knowledge extraction. Deep learning allows building computational models from composable processing layers for learning representations of data with multiple levels of abstraction. Several recent advancements have proven to be helpful in tumor analysis, variant calling, clinical trials recruitment, and building knowledge graphs from scientific journals and electronic health records. Join this session to understand the nuts and bolts of deep learning, its breakthrough applications in life sciences, and how the NVIDIA computing platform enables the development of high-performance deep learning systems.      
- **相關連結**：  
    - 演講： [S42424](https://events.rainfocus.com/widget/nvidia/gtcspring2022/sessioncatalog/session/1642417943266001uxhK)
    <p class="illustration">
       <img src="https://imgur.com/Lc5teJ4.png" alt="">
    </p>
<p class="paragraph-spacing"></p>

總體來說跟 [S42631](#S42631-Dawn-of-a-New-Era-of-Discovery---The-Convergence-of-AI-and-Life-Sciences) 的內容相似，有很大的重複性。


引發深度學習在現代生命科學革命？**<mark>資料</mark>**  
生命科學的基本本質已經改變了機器人技術和小型化實驗的可用性，並帶來了可以生成的實驗數據量的急劇增加。以如今的的儀器與技術，能同時運行多組實驗，並在一天內生成數百萬個資料點。

<p class="illustration">
   <img src="https://imgur.com/GYBIPhv.png" alt="">
</p>

- 資料可用性
    - 實驗數據急劇增加。  
    - 存儲變得便宜並且質量也有所提高。  
        - 一名患者僅在成像方面每年產生大約 80 MB的數據。  
- 計算能力提昇
    - GPU 觸手可及，且可用於平行計算、矩陣運算，將工作流程加速 4-5 倍。  
- 新演算法興起
    - 每隔幾個月就推陳出新，改善計算準確度、效果...等。  

<p class="paragraph-spacing"></p>

<p class="illustration">
   <img src="https://imgur.com/ntaHav6.png" alt="">
</p>

- 在生物製藥價值鏈中應用人工智慧
    - **從最初的藥物設計階段**：從發現研究蛋白質配體與之相互作用、發現新分子和候選藥物
    - **臨床**：預測某種療法的毒性程度和其他副作用
    - **製造供應**：生產庫存管理
    - **上市**：監測患者的不良反應...等。

<p class="paragraph-spacing"></p>

- 名詞解說與歷史（略）
    - [什麼是人工智慧、機器學習和深度學習?](https://chih-sheng-huang821.medium.com/%E4%BB%80%E9%BA%BC%E6%98%AF%E4%BA%BA%E5%B7%A5%E6%99%BA%E6%85%A7-%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92%E5%92%8C%E6%B7%B1%E5%BA%A6%E5%AD%B8%E7%BF%92-587e6a0dc72a)  
    - [人工智慧、機器學習與深度學習間有什麼區別?](https://blogs.nvidia.com.tw/2016/07/29/whats-difference-artificial-intelligence-machine-learning-deep-learning-ai/)
        <p class="illustration">
        <img src="https://imgur.com/ML56W2V.png" alt="">
        </p>
    - 常見網路架構
        <p class="illustration">
        <img src="https://imgur.com/U3Mz9Ev.png" alt="">
        </p>
        <p class="illustration">
        <img src="https://imgur.com/MBeQWd2.png" alt="">
        </p>


- 網路模型的應用
    - 在生命科學中的應用  
        <p class="illustration">
        <img src="https://imgur.com/E1QBVkQ.png" alt="">
        </p>
    - 在藥物開發的應用  
        <p class="illustration">
        <img src="https://imgur.com/Ag6Sv5C.png" alt="">
        </p>

        - 包括藥物設計、定量結構活性預測、虛擬篩選以及從計算角度映射到兩個特定問題：分子性質預測與分子生成。
          <p class="illustration">
          <img src="https://imgur.com/jzZrB8Q.png" alt="">
          </p>

        - 方法的選擇也跟 Functional Space 與 Chemical Space 兩者間的推導（？） 有關
          <p class="illustration">
          <img src="https://imgur.com/gN1RU2B.png" alt="">
          </p>

 
 
## [S41682] Improving Screening Mammography using Integrated AI Service   
- **中文標題**：慈濟使用整合人工智慧服務改善乳房X光篩檢
- **講者**： 
    - Ping-An Wu , Vice Superintendent, Hualien Tzu Chi Hospital
    - Amy Lai, Deputy Director, Institute for Information Industry
    - Jason Chou, Product Manager, EBM Technologies
- **主要領域**：醫療保健與生命科學
- **簡介**：
    Screening mammography is an early process to detect breast cancer. By finding tiny suspicious lesions on the X-ray images of superimposed breast tissues, patients take further exams if needed. Defects in any stage of the process may delay treatment for breast cancer. With AI assisted image quality assurance, a working list prioritization, and lesion detection, we expect to enhance screening mammography performance, especially for women in rural areas who take an exam from a mobile mammography vehicle.      
- **相關連結**：  
    - 演講： [S41682](https://events.rainfocus.com/widget/nvidia/gtcspring2022/sessioncatalog/session/1638511222842001gWqR)
    <p class="illustration">
        <img src="https://imgur.com/VFlYjAX.png" alt="">
    </p>

<p class="paragraph-spacing"></p>

全篇涉及了 3 個腳色：
1. 慈濟：收集資料與使用者
2. 資策會：實作與訓練
3. emb ：edge dvices與 醫療系統的整合

聽起來，整體流程有點像是 [2020 馬偕](https://cynthiachuang.github.io/Lecture-Notes-Nvidia-GTC-2020/#a21695rt-智慧醫院-ai-協助診斷佈署開發經驗分享)那篇，兩者間的主要差異有：
1. 新增了影像質量的模型，在前線進行第一階段的把關
2. 資策會有針對台灣女性進行重新訓練
3. 設備端介入整合性較高
   - 標注操作層面
   - 臨床通知

<p class="paragraph-spacing"></p>

報告我直接跳過慈濟歷史跟證嚴法師的部分，直接快進資策會防守範圍，就一開始的說明看來他們擅長醫療影像的部分：

<p class="illustration">
    <img src="https://imgur.com/nPKZfPJ.png" alt="">
</p>

<p class="paragraph-spacing"></p>

**乳房攝影流程**  
乳房攝影工作流程是一項耗時的任務，在目前的的流程中，健檢車會到社區進行健檢，待健檢完畢後，資料會被帶回醫院，在行政人員完成整理後，資料會被送往放射科醫生進行標注，由於每次出診後帶回來的資料眾多，等待資料標注完畢寄回給受檢者後，往往需要耗時 27 周。但若真有任何異常情況，這樣的時間間隔可能錯過治療得黃金期。

<p class="illustration">
    <img src="https://imgur.com/Gfzawu7.png" alt="">
</p>

<p class="paragraph-spacing"></p>


在這流程中 AI 可介入的範圍有 3 ：
<p class="illustration">
    <img src="https://imgur.com/TkZL79G.png" alt="">
</p>

1. 拍攝時**確認影像質量**  
   目前拍攝結果，必須等到回院後由人工檢查，其中有 30％ 的不良品質圖片，而這些不良品質圖片會影響後續標注。
   <p class="illustration">
   <img src="https://imgur.com/5QaP2hI.png" alt="">
   </p>
2. **高風險的先看**  
   資料會被帶回醫院，在行政人員完成整理後，會先經由 AI 進行預測挑選出分數較高的病患，這病患會有比較高優先權，放射科醫生會優先標注，並先寄送出。
   <p class="illustration">
   <img src="https://imgur.com/99lNm9e.png" alt="">
   </p>
3. 自動檢測潛在病變，**協助醫生標注**  
   資策會針對台灣女性資料庫重 train，並根據重訓練完的檢測潛在病變。
   <p class="illustration">
   <img src="https://imgur.com/SgFSvP1.png" alt="">
   </p>

<p class="paragraph-spacing"></p>   

按計算，若 3 步驟皆導入可省下 30% 的時間：

<p class="illustration">
    <img src="https://imgur.com/XHTUJX3.png" alt="">
</p>

<p class="paragraph-spacing"></p>     

**EMB**  
設備公司負責終端設備與深度學習模型的整合：

<p class="illustration">
    <img src="https://imgur.com/My0uGef.png" alt="">
</p>

<p class="illustration">
    <img src="https://imgur.com/5wqxovk.png" alt="">
</p>

<p class="illustration">
    <img src="https://imgur.com/Ms2lsA2.png" alt="">
</p>

<p class="paragraph-spacing"></p>


除了查看結果。AI 平台還提供了報表系統的集成界面。該系統採用了平台商所提供的 SDK 和 API，為放射科醫師創建了一個交互式報告環境。他們只需單擊左側的匯總表，即可從右側瀏覽所有 AI 發現。如果醫生擔心，發現他們可以點擊複選框以生成報告內容。

<p class="illustration">
    <img src="https://imgur.com/8xI3XCf.png" alt="">
</p>

<p class="paragraph-spacing"></p>

不僅放射科醫生可以從 AI 模型中受益。平台商還開發了一個會議室系統，該系統也為臨床醫生提供。在系統中，加入了AI警告通知。如果對結果有重大發現，系統會觸發通知以提醒其臨床。

<p class="illustration">
    <img src="https://imgur.com/AdqnOSD.png" alt="">
</p>

<p class="paragraph-spacing"></p>

通知臨床醫生衝突的藥物劑量。
<p class="illustration">
    <img src="https://imgur.com/CjMpSBB.png" alt="">
</p>



## [S41686] First-principle-based and Data-driven Drug Design Platform Using a Multi-site Targeting Strategy 
- **中文標題**：清華大學使用多點打靶策略的資料導向藥物設計平台 
- **講者**： 
    - Lee-Wei Yang, Professor and Director of Institute of Bioinformatics and Structural Biology, National Tsing Hua University, Taiwan
- **主要領域**：醫療保健與生命科學
- **主題分類**：藥物發現、基因組學
- **簡介**：
    First I'll demonstrate, in a protein-dynamics-accentuated pipeline, how well-parameterized small molecules in a library comprising only 2,000-plus FDA-approved drugs can be correctly ranked against a targeted binding site by statistical models guided by known FDA drugs, whereby two new drugs can be discovered by reassembling fragments from a relatively safe chemical space. Leveraging the off-target effects to redefine specificity, we demonstrated that allosteric drugs and interface blockers can bring therapeutic synergy to inhibit an oncogenic autophagic enzyme, ATG4B, together with the orthostatic (active-site) drug tioconazole. This automated pipeline has been provided with a web portal, DRDOCK, integrating an NVIDIA DGX A100 node for GPU-accelerated molecular dynamics simulations, AI-based structure prediction (Alphafold2), and small molecule docking. The site is available at https://dyn.life.nthu.edu.tw/drdock/ .      
- **相關連結**：  
    - 演講： [S41686](https://reg.rainfocus.com/flow/nvidia/gtcspring2022/aplive/page/ap/session/16385155433000015Zik)
    - [其他投影片](https://phys.ncts.ntu.edu.tw/uploads/asset/data/621320271d41c8381f25ce5b/2._Lee-Wei_Yang.pdf)
    <p class="illustration">
    <img src="https://imgur.com/FIlR266.png" alt="">
    </p>

<p class="paragraph-spacing"></p>

這邊大概是這幾篇製藥相關最硬的一篇，害我還先去讀書…是說這是不是**基於片段的藥物設計（Fragment-based drug design, FBDD）**？

<p class="paragraph-spacing"></p>

小藥物藥物設計
- 市場的要求設計或修改一個具有高親和力的大化合物以找到感興趣的疾病目標。
- 但實際上藥物可能會直接或將最低限度地結合於這種疾病以外的蛋白質。

<p class="illustration">
    <img src="https://imgur.com/3e6ok8G.png" alt="">
</p>

<p class="paragraph-spacing"></p>

本文建立了一個名為 [DRDOCK](https://dyn.life.nthu.edu.tw/drdock/) 的藥物設計平台
- 集成了一個 NVIDIA DGX A100 節點，用於 GPU 加速的分子動力學模擬（MD）、基於 AI 的結構預測（Alphafold2）、和小分子對接（Docking）。
- Automated Pipeline 網站，工作流程可以概括為四個模塊：結構預處理、對接/排名 2016 FDA 批准的藥物、水中 10 ns NPT MD 模擬和結果渲染。
- 對應到 Clara Discovery Flow 中大約就是紅圈這一塊。
  <p class="illustration">
    <img src="https://imgur.com/RdbFrKG.png" alt="">
  </p>
- 工作流程圖
  - 不過它跟 Clara Discovery Flow 有點差異，它的流程是 `MD → Docking → MD`。  
    <p class="illustration">
    <img src="https://imgur.com/KVJTOD0.png" alt="">
    </p>
  - **MD**：第一次做 MD，是為了取得初始結構。實務上可能無法從 x-ray 或公開資料集取得或是為了畫出不同構態，就會先做 MD。
  - **Docking**：由 AI 提取特徵，根據這些特徵在 FDA 藥物中的分佈以及 FDA 藥物與非靶蛋白的蛋白質對接來，並找到好的角度、位置、能量。
  - **MD**：最後再執行 MD，以檢查兩件事
     - 生存條件是否符合人體條件（37度、120-150摩爾濃度（？）、PH=7）
     - 評估結合自由能 
       <p class="illustration">
       <img src="https://imgur.com/qIMbN73.png" alt="">
       </p>

     
<p class="paragraph-spacing"></p>

**多點打靶策略**  
研究人員在篩選藥物親和性的時候，往往希望得到親和性較高的化合物。但親和性高可能也表示藥物對靶點的選擇性高，因此親和性強未必會是良藥，甚至不能成藥，為了得到較好的療效和較低的不良反應，多靶點藥物則成關注焦點。

所謂多靶點藥物，是指同時作用於疾病中多個靶點的藥物，對各靶點的作用產生協同效應，使其效果大於各單點效應之和，達到最佳的治療效果。

- **脫靶效應**   
  與設計之外的靶點產生作用，即為**脫靶效應**。脫靶效應又稱藥物副作用，這些副作用或許患者有害的，但有時也可能使患者獲益。例如：威爾鋼，原本研發出來治療心絞痛，結果因脫靶效應成了治療性功能障礙。
  <p class="illustration">
    <img src="https://imgur.com/ThJYMwj.png" alt="">
  </p>
  
- **藥物結合**  
  為克服脫靶效益，一種方向是採用多點打靶策略結合兩種不同的藥物。以找到藥物與 EgIN2 結合但不與結構相似的 EglN1 結合。
  <p class="illustration">
    <img src="https://imgur.com/io6AxI6.png" alt="">
  </p>

  這邊嘗試 4 種 FDA 藥物，`A-X`、`A'''-C`、`A*-D`、`A''-B`
  <p class="illustration">
    <img src="https://imgur.com/zzzywMi.png" alt="">
  </p>
  
  - 4 種 FDA 藥物 `A-X`、`A′′′-C`、`A*-D`、`A''-B`，經由 Docking 篩選可以發現 `A''-B` 可以對 EglN2 產生影響，且較少脫靶效應。
  - EglN2 是乳腺癌細胞中的一種誘導型雌激素，所以另一組他們選擇在細胞水平上抑製乳腺導管癌（MCF7，T47D）的藥物。
  - 根據實驗結果， `A-X` 顯示出強大的腫瘤抑製作用。     
  - `A''-B` 與 `A-X` 藥物兩相結合 `A''-X`，能有效抑制 EglN2 
  - IC50 與 EC50
      - Concentration of 50 % inhibition(IC50)：為半抑制濃度。對能夠抑制某酵素活性的藥物，當酵素的活性下降一半時，此時所加的藥物濃度即為 IC50。
      - Concentration of 50 % Effect(EC50)：為半效應濃度。假設藥物發揮最大效力時，能夠使患病的細胞恢復正常，那使疾病症狀減輕一半時，即藥物發揮一半效力時所需的濃度即為 EC50。
      - 若以藥物來說，IC50 和 EC50 不同的地方，IC50 主要顯示的是**藥物抑制能力**，EC50主要顯示的是**藥物效果**。
  - 與用藥效團開發的功效相似藥物，效果要好
      - 藥效團（pharmacophore）是配體被生物大分子進行分子識別所必需的物理化學特徵及其空間排布。
        <p class="illustration">
        <img src="https://imgur.com/ibMxLep.png" alt="">
        </p>

- 製作多靶點藥物時，可以從同一疾病或靶點的**正構藥**、**異構藥**與**介面藥**去開發，從中找交集，確保藥物可以準確與靶點結合。
  <p class="illustration">
    <img src="https://imgur.com/31Emzzx.png" alt="">
  </p>
   

Drug Repurposing Screening Identifies Tioconazole as an ATG4 Inhibitor that Suppresses Autophagy and Sensitizes Cancer Cells to Chemotherapy  
- ATG4B 在控制自噬的機制中扮演關鍵的腳色
- 自噬（英語：Autophagy，或稱自體吞噬）是一個涉及到細胞自身結構通過溶酶體機制，負責將受損的細胞器、錯誤折疊的蛋白及其他大分子物質等運送至溶酶體降解並再利用的進化保守過程。
  <p class="illustration">
    <img src="https://imgur.com/vABcz7z.png" alt="">
  </p>
  <p class="illustration">
    <img src="https://imgur.com/ZE7IBeZ.png" alt="">
  </p>

<p class="paragraph-spacing"></p>

藥物設計平台中使用 MIG 技術
- **Multi-Instance GPU (MIG)**
    - 安培（Ampere）架構
    - 多執行個體 GPU 讓每個 GPU 最多能分隔成 7 個執行個體，各自完全獨立且具備個別的高頻寬記憶體、快取和運算核心。
    <p class="illustration">
        <img src="https://imgur.com/0nBKtBf.png" alt="">
    </p>
    <p class="illustration">
        <img src="https://imgur.com/GAbf6Hw.png" alt="">
    </p>
    
<p class="paragraph-spacing"></p>

藥物設計平台架構      
<p class="illustration">
    <img src="https://imgur.com/vHYNnhX.png" alt="">
</p>



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-05-06</summary>
  <ul>
    <li>2022-05-06 發布</li>
    <li>2022-04-21 完稿</li>
    <li>2022-03-22 起稿</li>
  </ul>
</details>
