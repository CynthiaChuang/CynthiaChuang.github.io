---
title: NVIDIA Clara Discovery - Drug Design 篇
date: "2022-06-29 22:33" 
is_modified: false
categories:
- "應用資訊 › 生醫資訊"
- "資訊科技 › 開發與輔助工具" 
tags:
- 藥物設計
- Clara
- AI/ML
- NVIDIA
--- 

Clara Discovery 是 NVIDIA Clara Train 4.0 所新增的項目，忘記誰跟我說它是藥物製造，~~或許是 google translate （!?）~~，但 Clara Discovery 其實並不涉及製造，而是用在**藥物設計**。
  
不過這一篇網誌有點太長了，所以內文被我拆成上下篇，分別討論[藥物設計](https://cynthiachuang.github.io/NVIDIA-Clara-Discovery-Drug-Design/)與 [Toolkits](https://cynthiachuang.github.io/NVIDIA-Clara-Discovery-Toolkits)。

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/O82AmD4.png" alt="將人工智慧和高效能運算技術應用於藥物設計">
    將人工智慧和高效能運算技術應用於藥物設計（圖片來源: <a href="https://www.nvidia.com/zh-tw/clara/drug-discovery/">Nvidia</a>）
</p>

<div class="alert warning"> 
<div class="head">小白讀書筆記，慎入</div>
此篇記錄了一個小白 Survey 藥物設計的過程與筆記，可能出現常識性錯誤，服用時請小心！  
若有看到錯誤也請指出，謝謝！🙇‍♀️
</div>


## 解決痛點
在 Clara Train 4.0 推出的新功能中，Clara Discovery 因受肺炎疫苗開發的影響尤為引人注目。它可透過預先訓練過的 AI 模型與 GPU 算力的提升與優化，用以<mark>加速新藥研究流程</mark>，並幫助製藥者<mark>改善分析、效率和擴充能力</mark>，使之能更有效地在龐大的分子資料庫中搜尋探索、縮短病毒分析所需時間，以及用物理學及 AI 來模擬分子與人體的生物化學反應，找出並最佳且最有可能用於治療的化合物，以達到<mark>降低成本與失敗率</mark>的目的。

<br>

根據過往資料可以發現，新藥研發痛點大多脫離不了**時間**、**成本**和**失敗率**三點，根據資料顯示：
1. 從發現合適靶點到最終正式上市，平均需要 **14 年**的時間，所需投入成本高達 **18 億 ~ 26 億**美元，且整體成功率小於 **4％**。
2. 在研發過程中，團隊通常需要對**數十萬種分子**進行分子動力學模擬計算，以從中篩出合適藥物，這需要耗費大量的**時間成本**。
3. 若有需要進一步研究不同蛋白質之間的相互作用，其計算量將達到**天文級別**，不僅耗時耗力，且有高達 **90%** 的失敗率。

<p class="illustration">
    <img src="https://i.imgur.com/tCaiJG3.png" alt="新藥研發所需時間與成本">
    新藥研發所需時間與成本（圖片來源: <a href="https://ctee.com.tw/news/fund/545734.html">工商時報</a>）
</p>

因此為降低失敗率，並加快藥物設計，Clara Discovery 集合了相關框架、應用程式和預訓練模型，並借助 GPU 加速，以縮減蛋白質分析、搜尋候選分子所需時間，並且透過模擬、比對...等方式確認藥物成效。

相較傳統的設計與篩選需要費時 **4~5 年**的時間，才能夠進入人體臨床試驗階段，而 AI 分析技術能將時程縮短至 **1 年**以內，讓救命的藥物能更快進入臨床使用。

<p class="illustration">
    <img src="https://imgur.com/KZKewt0.png" alt="AI 分析技術">
    AI 分析技術（圖片來源: <a href="https://www.uu.nl/en/events/lecture-matthew-botvinick-deep-meta-learning">Utrecht University</a>）
</p>


目前 Clara Discovery 應用中包含了些常用工具、加速處理的方法與新開發的工具，例如：Parabricks、BioMegatron、RAPIDS、MegaMolBART、AutoDock 與 GROMACS...等。而這些工具將應用於研發各階段，如：蛋白質體學、顯微鏡學、虛擬篩選、計算化學、可視化、臨床成像和自然語言處理...等多個領域。

下圖是 NVIDIA 所繪製的理想流程圖，他們運用了目前手邊的武器庫兜出了個漂亮的流程圖。

呃...我用了「兜」這個字是因為在 Survey 的過程中，我所了解到的步驟與他們畫出了流程圖有些出入，但造成這落差的原因有可能是因為實務操作、研究方向、各領域進行客製化，甚至是個人習慣所導致。所以我先認定 NVIDIA 繪製的是一個概括的流程圖，至於[詳細的流程](#藥物設計流程)與 [Toolkits](https://cynthiachuang.github.io/NVIDIA-Clara-Discovery-Toolkits) 的介紹我們後面章節再說。

流程圖中讓我覺得比較突兀的是 `IMAGING` 與最後的 `NLP`。`IMAGING` 或許還可以解釋為臨床實驗的效果確認，但...好像不是所有的結果都可以透過醫療影像可以判別的！？而 `NLP` 那塊我真的覺得是為了流程而放進去的（不負責任猜測 🤣 XD）

<p class="illustration">
    <img src="https://imgur.com/oeB3ggO.png" alt="Clara Discovery Flow">
    Clara Discovery Flow（圖片來源: <a href="https://www.nvidia.com/zh-tw/gtc/">GTC 2022</a>）
</p>


是說...這張流程圖畫重點，等等應該會很常看到它。



## 先備知識
在開始前，先來補充點先備知識，包含一些等等可能會反覆出現的專有名詞或觀念。先列一些出來，如果有沒提到的，再問我嘿~

1. **受體（receptor）**  
    又稱受器、接收器，是一個生物化學上的概念，習慣上所說的**靶點**。
    
    指一類能傳導細胞外訊號，並在細胞內產生特定效應的分子。產生的效應可能僅在短時間內持續，比如改變細胞的代謝或者細胞的運動；也可能是長效的效應，比如上調或下調某些基因的表達。
    
    受體通常是一種蛋白質分子，通常存在酵素分子的表面，可以和神經傳導物質、荷爾蒙、藥物或是毒物等配體結合。每種受體只能結合某些特定形狀的配體分子，受體在與配體結合之後，會帶來構型（conformation，受體蛋白質的三維結構）的改變，因而影響蛋白質活動，並進一步引起各種細胞反應。
    
    <div class="alert info"> 
    <div class="head">關於 conformation 一詞的翻譯</div>
    這個字的翻譯我總共看到了 3 種構型、構形與構象，最後我根據<a href="https://terms.naer.edu.tw/search/?q=conformation&field=ti&op=AND&group=&num=10">研究院的雙語辭書中藥學的學術名詞</a>選用了構型這個字。 
    </div>


2. **配體（ligand）**  
    在這篇中我們可以直接理解成**藥物分子**。

    在生物化學和藥理學中，配體是指一種能與受體結合以產生某種生理效果的物質。在蛋白質-配體複合物中，配體通常是與靶點蛋白特定結合位點相連的訊號觸發分子。 
   
3. **靶點**  
	這邊說的是藥物靶點，指藥物在體內的作用結合位點，也就是藥物分子的作用目標，包括基因位點、受體、酶、離子通道、核酸等生物大分子。
 
4. **藥物作用**  
	在人體內，受體與配體結合，並引起一系列生物功能變化的過程。
   
5. **活性化合物（hit compound）**  
	活性化合物對不同研究人員的含義可能不同。這邊簡單理解成，粗選出來準備進行篩選的化合物。
   
6. **先導化合物（lead compound）**   
	從活性化物中篩選或擴展出來的第二階段化合物。
   
	是一種具有藥理學或生物學活性的化合物，可被用於開發新藥，但存在某些缺陷，如活性不夠高、化學結構不穩定、毒性較大、選擇性不好、藥物動力學性質不合理或其他脫靶效應，這些化合物往往還無法作為理想藥物，需要進行化學優化，將藥性提高至足以進行生物試驗或臨床試驗的程度。

7. **候選藥物（candidate）**  
	先導化合物通常需再合成千百個衍生物，評估並比較其活性、毒性、安定性、藥物動力學後，選上數個具有潛力者的候選藥物（candidate）進入下一階段之臨床前試驗。

	化合物與藥物開發階段對應關係如下	
    <p class="illustration">
        <img src="https://imgur.com/XIOc937.png" alt="化合物與藥物開發階段">
        化合物與藥物開發階段（圖片來源: <a href="https://static.rainfocus.com/nvidia/gtcspring2022/sess/1643414209690001u10x/SessionFile/HCLS%20Dev%20Summit%3A%20AI%20for%20Biology%20-%20Protein%20Structure%20Prediction%20and%20Beyond_1647313234154001tQdH.pdf">GTC 2022</a>）
    </p>



## 藥物設計 
本節開始前，可以先去看看這支 Overview 的影片–[《Drug discovery and development process》](https://youtu.be/3Gl0gAcW8rw)，它簡單敘述了個完整的藥物設計與研發流程，雖然看到最後才發現它是某間公司的廣告 XDDD 

關於 Drug Design 這個字我找到的翻譯是**藥物設計**，根據[維基百科](https://zh.wikipedia.org/zh-tw/%E8%97%A5%E7%89%A9%E8%A8%AD%E8%A8%88)的定義這是**一個根據靶點尋找與發明藥物的過程**；按我自己的話來說，它是**從找靶點到找到候選藥物（candidate）的過程**。

如要詳細描述藥物設計的流程，它是根據靶點的三維結構，測定或計算受體與配體的作用模型，並依據化學結構、電價與形狀等，設計出可能達到效果的新型化學藥物。除了考慮藥物與受體的結構特點外，還需要考慮藥物的毒性、吸收、分布、排除、代謝過程等因素的影響。

<p class="illustration">
    <img src="https://imgur.com/PDYymDy.png" alt="藥物設計：方法、概念和作用模式">
    藥物設計：方法、概念和作用模式（圖片來源: <a href="https://www.books.com.tw/products/CN11665534">博客來</a>）
</p>

若想深入了解藥物設計，可以看看 Gerhard Klebe 著作的[《藥物設計：方法、概念和作用模式》](https://book.sciencereading.cn/shop/book/Booksimple/onlineRead.do?id=B8B1CF16CF3798C4BE053020B0A0AA597000&readMark=0#)。這我有找到簡中譯本，算是一本教科書等級的書籍，光看製藥公司親自來翻譯這本書，就可以看出它的專業性，我都懷疑這本書是他們公司新訓員工的教材，~~這麼想想那些新員工好像有點慘（！？）~~

不過這本書真的寫得太專業的，我大概從高中畢業後就再也沒有看過任何的化學式，所以我自己也沒把這 700 多頁看完，就把它當辭典跳著看。但也能讓初學者大致理解了藥物設計與 Clara Discovery 提供的工具使用情境與流程，~~還多了奇奇怪怪的化學知識~~。


### Drug Design 與 Discovery
會有這個章節，純粹是因為一開始我使用 Drug Discovery 來 Survey 相關資料時，發現資料寥寥無幾。但反之，當我用 Drug Design 來找搜尋時，資料反而不計其數。喔對，特指中文資料。

所以我就有點好奇啦，Drug Discovery 與 Drug Design 在定義上是否有所差異。


#### ⎚ Drug Development
在看 Drug Discovery 的定義前，先看看另外一個字 – Drug Development。這個字的中文翻譯是**藥物研發**，它的[定義](https://pharmaeducation.net/difference-between-drug-design-and-drug-development/#_Definition_of_Drug_Development)是比較明確的，就是從候選藥物到上市這段時間。

<p class="illustration">
    <img src="https://i.imgur.com/N7GwErw.jpg?1" alt="藥物設計與研發">
    藥物設計與研發（圖片來源: <a href="https://www.slideteam.net/business_powerpoint_diagrams/89124683-style-cluster-hexagonal-6-piece-powerpoint-presentation-diagram-infographic-slide.html">Slide Team</a>）
</p>


#### ⎚ Drug Discovery  
我發現有人直譯成**藥物發現**，在網路上針對這個字的定義較模稜兩可，我看到幾種關於 Drug Discovery 這個字的定義：

1. **Drug Design 的前導步驟**：   
    根據[《Difference between Drug Design and Drug Development》](https://pharmaeducation.net/difference-between-drug-design-and-drug-development/) 一文中所提到的：
    > Generally, a drug is available in the market by passing through 3 stages:  
    > Drug Discovery >>> Drug Design >>>Drug Development 

    這裡的解釋將 Drug Discovery 視為 Drug Design 的前導步驟，但在文中它並沒有確切定義所謂的前導步驟，所以我自己推測可能是藥物選擇與結構推定（尤藥物反推可以治癒的疾病的開發過程中會需要）之類的步驟。

    我只在[《Drug Design and Discovery: Principles and Applications》](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6155886/#B1-molecules-22-00279)找到了個可能類似的定義：
    > **Drug discovery** is the process through which potential new therapeutic entities are identified, using a combination of computational, experimental, translational, and clinical models Despite advances in biotechnology and understanding of biological systems, drug discovery is still a lengthy, costly, difficult, and inefficient process with a high attrition rate of new therapeutic discovery. **Drug design** is the inventive process of finding new medications based on the knowledge of a biological target. 

    他們說 Drug Discovery 是識別潛在新治療實體的過程（？）。這句話有點難懂，難道是先找目標疾病或基因嗎？不過在這篇文章中很明顯可以看出，Drug Discovery 與 Design 是兩個相異的動作。

2. **與 Drug Design 同義**  
    根據[維基百科](https://en.wikipedia.org/wiki/Drug_discovery)的定義，這也是我在找資料的時，最常看到的解釋：
    > Drug discovery is the process by which new candidate medications are discovered.

    在多數文章中會把 Discovery 與 Design 並用或連用，從上下文中看來它們兩多是等義的存在。

    其他，如[《The Stages of Drug Discovery and Development Process》](https://www.researchgate.net/publication/341097009_The_Stages_of_Drug_Discovery_and_Development_Process)一文、GTC 2022 [《AI FOR BIOLOGY:PROTEIN STRUCTURE PREDICTION AND BEYOND》](https://static.rainfocus.com/nvidia/gtcspring2022/sess/1643414209690001u10x/SessionFile/HCLS%20Dev%20Summit%3A%20AI%20for%20Biology%20-%20Protein%20Structure%20Prediction%20and%20Beyond_1647313234154001tQdH.pdf)演講中，都用了採用了類似的定義。
   
3. **藥物設計與研發完整流程**  
    單看 Clara Discovery Flow，我覺得上面兩種都不太符合 NVIDIA 的定義。

    從流程圖看來它比像是 Drug Design 與部分 Development 的整合，因為圖中一路從基因定序包到臨床試驗及之後的病例回饋，但又少了人體試驗到上市之後（或許有，`IMAGING` 擴大解釋能可以視為藥物上市後的投藥情況，但沒有明說）。

    所以嚴格說來<mark>藥物設計的相關應用才是這是新增的主要內容</mark>，但它跟舊有的基因定序、醫療影像、自然語言處理...等應用，行成了最後推出的 Clara Discovery Flow。



### 電腦輔助藥物設計（Computer-Aided Drug Design, CADD）
會特別想提到藥物設計，是因為在藥物設計中有一子領域，不同於傳統藥物開發從茫茫化合物海中撈出適合的潛力藥物，它是藉由**電腦模擬和計算受體與配體的相互作用**，進行先導化合物的最佳化與設計。

電腦輔助藥物設計這子領域認真要探討是一個非常龐大的論題，可以涉及到結構化學、藥物化學、分子藥理學、生物化學、結構生物學、分子生物學、化學生物學、細胞生物學、生理學、病理學、生物物理學、組合化學、量子化學、分子力學、分子動力學、分子圖形學、計算化學、化學資訊學、生物資訊學、X-ray 晶體學、核磁共振技術、電腦圖形技術、資料庫技術和人工智慧技術等基礎學科和應用學科與技術。  

...上面那串我就是列出來嚇人的 XDDD。上述算是電腦輔助藥物設計的基礎學科知識與技術，除非要深入研究或專門領域人員，不然應該不會碰到那麼多的技術。

<p class="illustration">
    <img src="https://imgur.com/hB8ngu8.png" alt="CADD 涵蓋範圍">
    電腦輔助藥物設計涵蓋範圍（圖片來源: <a href="https://www.semanticscholar.org/paper/An-Approach-of-Computer-Aided-Drug-Design-(CADD)-In-Hoque-Chatterjee/f6e5b3019f63c22842db2e9c461de550990437e5">An Approach of Computer-Aided Drug Design (CADD) Tools for In Silico Pharmaceutical Drug Design and Development</a>）
</p>

有沒有覺得這些解釋跟知識技術與 Clara Discovery 在做的事情超像有沒有！？因為從簡單定義來看，<mark>電腦輔助藥物設計就是使用電腦分子建構技術，來進行藥物設計，而 Clara Discovery 可以被歸入這個範疇</mark>。

從廣義定義來說，電腦輔助藥物設計泛指**資訊技術在藥物設計與研發過程中的所有應用**，包含分析技術、處理過程...等；但狹義上，也是通常意義上的電腦輔助藥物設計僅指基於**分子模擬的設計技術**，從電腦運算模擬篩選出活性化合物，進一步能用於結構修飾的先導化合物的開發，大幅降低後續試驗規模與成本，並加速藥物開發。

將上述狹義的電腦輔助藥物設計與 Clara Discovery Flow 相對應，也就是流程圖中的橘色虛線框區域。至於廣義的部分我就不畫了，整個 Flow 算進廣義電腦輔助藥物設計應該沒問題？

<p class="illustration">
    <img src="https://imgur.com/ylyVoKl.png" alt="對照 CADD 於 Clara Discovery Flow">
    對照電腦輔助藥物設計於 Clara Discovery Flow
</p>



## 藥物設計流程
既然都提到 Clara Discovery Flow，那就來看看整體新藥設計的流程，並將該流程與 Clara Discovery Flow 相互對照吧！


### **藥物分類**  
製藥、製藥，但藥物本身也有不同的分類，對於不同分類的藥物其設計法大相徑庭。在生技醫藥產業中將藥物依照**分子量**大小區分為：  
1. **小分子藥物（small molecule）**  
    一般為化學藥，此類藥物大部分是由化學合成製造且分子量小於 **1000 道爾頓（dalton，Da）** 的化合物（1 道爾頓等於一個氫原子的重量）。另外，有些資料會將分子量上限設在 **900 道爾頓**，因為 900 道爾頓以下的分子在人體內能較快速地擴散進入細胞。

    常見的阿斯匹靈就是一種化學藥，它有 21 個原子 180 道爾頓，因此屬於小分子藥。

2. **大分子藥物（large molecule）**  
    一般為生物藥，具有較大且複雜的分子，並由生物技術製造而成，在動植物或微生物體內形成。

    常見用於改善生長遲緩的生長激素與單株抗體，它們分別由 3000 與 25000 個原子組所組成，都屬大分子藥物。

    <p class="illustration">
        <img src="https://imgur.com/hFazzWS.png" alt="藥物分類">
        藥物物分類（圖片來源: <a href="https://topics.cnyes.com/biotech01/">鉅亨網</a>）
    </p>
     
<mark>Clara Discovery 主攻的就是小分子藥物製藥</mark>，它會著眼於分析受體與配體的三維結構模型，並利用統計學建模工具或者虛擬篩選的方法確定新的分子實體。


 
### 常見藥物設計法
對於小分子藥物設計，按照著眼點的不同，有 **3 種**常見開發法：
1. **基於結構的藥物設計（Structure-based drug design, SBDD）**  
    從**受體的結構和性質**出發尋找可以與其特異結合的配體分子，所以也稱為**直接藥物設計**，包括基於受體結構的分子嵌合（Docking）、活性位點分析、從頭藥物設計（在通過分子生成的方法，生成具有特定性質的全新分子以探索化學空間，補充化合物資料庫的方法）...等。

    這種設計方式顧名思義就是仰賴**結構資訊**，**需要有化合物與目標蛋白配受體兩方的結構**，將其逐一嵌合，考慮彼此的物化特性、計算兩者之間的各種分子作用力，綜合評比找出較能與受體穩定結合的活性化合物，爾後反覆進行結構修飾與嵌合，設計出先導化合物。
    
    <mark>NVIDIA 的 Clara Discovery 看來是採用此藥物設計法來開發的。</mark>
    
2. **基於配體的藥物設計（Ligand-based drug design, LBDD）**  
    根據**已知活性的先導化合物**，構建結構-活性關係或藥效基團模型，稱為**間接藥物設計**，包括定量構效關係（Quantative Atructure-activity Relationship, QSAR）、藥效基團模型、受體映射、基於分子形狀的疊合...等方法。

    此方法適用於**結構或結合區域未知的目標蛋白**，但這需要知道不同化合物對該蛋白的抑制能力與活性的關係，以建立構效關係，進而找出活性化合物，再優化其結構與代謝特性，成為先導化合物。

3. **基於片段的藥物設計（Fragment-based drug design, FBDD）**  
    這個算是比較特別的開發方式，不同於基於結構的藥物設計法主要通過從天然或已知化合物中隨機篩選來發現新藥。在一個藥物分子結構中，每個組成片段都發揮著不同的作用，這種開發法，主要利用現有的藥物分子片段，將不同的**結構片段進行組合或者延伸**，以期發現先導化合物。

    會提到這個，是因為在 GTC 2022 中，清大楊教授所介紹的[《First-principle-based and Data-driven Drug Design Platform Using a Multi-site Targeting Strategy》](https://cynthiachuang.github.io/Lecture-Notes-Nvidia-GTC-2022/#s41682-improving-screening-mammography-using-integrated-ai-service)的藥物設計應該就是這個方向。
   


### 現代藥物設計流程
[林榮信研究員](https://newsletter.sinica.edu.tw/14190/)將現代的藥物設計流程，簡敘述為以下步驟：
1. 先透過大規模基因體學與實驗，找到可成為藥物的分子及治療靶點 
2. 解出其分子結構
3. 依此分子結構來設計合成藥物化合物
4. 臨床試驗

<p class="illustration">
    <img src="https://i.imgur.com/6AKNfsv.png" alt="現代的藥物設計流程與 Clara Discovery Flow 相對應">
    現代的藥物設計流程與 Clara Discovery Flow 相對應
</p>


接下來，我試圖將此藥物設計流程與 Clara Discovery Flow 相對應。


#### Step1. 先透過大規模基因體學與實驗，找到可成為藥物的分子及治療靶點  
> 主要對應 Flow 中的 `GENOMICS` 區塊。

針對受體進行次世代基因定序，得出其基因序列，並期望從中發現關鍵的藥物作用靶點。選擇並確定新穎（？）的有效靶點是藥物設計的首要任務與核心，可以說靶點的選取會直接決定了藥物開發的成敗。曾有看過某阿茲海默症藥物設計開發過程，雖耗時 8 年開發，但最後以失敗告終，溯其原因就是錯誤靶點的選取。

以設計醫治病毒的藥物為例，靶點的目的是**讓藥物干擾病毒複製過程的某個階段**，以穩定且有效地壓制病毒。有點類似在生產線上卡關的概念，我們用藥物抑制細胞生產特定蛋白質，就像抽走流水線上的一個零件，最終讓產線無法合成病毒；又或者是我們使用要藥物誘導細胞生產另一相似結構的蛋白質，就像是生產了不合規格的零件，產線雖可以合成病毒，但還是因次級零件的使用導致最終成品無法發揮其作用。

關於靶點選擇與確認的方法，並不是我這次主要的 Survey 目標，所以我就先不細究了。


#### Step2. 解出其分子結構  
> 主要對應 Flow 中的 `STRUCTURE` 區塊。

在藥物化學中，有個還滿有名個概念，至少最近在啃相關資料時看過不只一次了：「**如果你要了解某個功能，那麼你首先要了解它的結構**」，算是知彼知己，百戰不殆嗎？

解出分子結構的學科或工具相當的多樣，如：
1. **X-ray 晶體學**  
    它可由電子密度圖可以推測出分子高解析度的三維結構。

    但既然是晶體學，待測物就必須是結晶狀才有辦法應用該技術，如病毒這種不會長成晶體的就不適合使用該方法。即便待測物能產生結晶，很多蛋白質只能在特定的酸鹼和溫度下才會產生結晶，若想要觀察蛋白質在不同環境下的結構變化，會不得其門而入。
   
2. **核磁共振光譜學**  
    這方法可以獲得高解析度的三維結構，它是利用磁場與待測物原子核進行共振，解讀釋放後電磁波訊號，反推原子核在待測物的數量與分布。但這項技術的限制是待測物的分子量必須小於 35000 mol。
   
3. **冷凍電子顯微術**  
    這方法對待測物的限制物較小，不需要形成結晶，因此可以觀測較多樣的環境。但在某篇[文章](https://modular-course.web2.ncku.edu.tw/p/404-1100-192842.php)中提到冷凍電子顯微術是三者中精確度較低的，關於這點我需要再查查資料。 

    <p class="illustration">
        <img src="https://imgur.com/ljGko6C.png" alt="解出分子結構儀器">
        解出分子結構儀器（左、中圖片來源: <a href="https://chemistry.cycu.edu.tw/%E9%97%9C%E6%96%BC%E6%9C%AC%E7%B3%BB/%E7%92%B0%E5%A2%83%E5%8F%8A%E8%B3%87%E6%BA%90/%E8%B2%B4%E9%87%8D%E5%84%80%E5%99%A8/">中原大學化學系</a>、右圖片來源:<a href="https://research.sinica.edu.tw/tsai-ming-daw-cryo-electron-microscope/">中央研究院研之有物</a>）
    </p>

另一個得到獲得蛋白質結構的方式是透過**蛋白質結構資料庫**，但這些資料庫雖有提供分子結構，實質上這些結構仍只是模型。在現實中蛋白質是動態的，每個原子的 X, Y, Z 位置都會隨著時間與環境而變動並非一成不變的，因此需具備結構生物學知識，來理解蛋白質結構資料庫中「分子結構」的真實意義。   

除傳統觀測方法外，還有 DeepMind 的 [**AlphaFold**](https://github.com/deepmind/alphafold) 與華盛頓大學 Baker 團隊的 [**RoseTTAFold**](https://github.com/RosettaCommons/RoseTTAFold)，兩種用方法可以透過一維胺基酸序列直接**預測出三維蛋白質結構**，此外也能加速分析由實驗上取得的結構資訊。

雖然 NVIDIA 的 Clara Discovery Toolkits 中有提供電子顯微鏡的影像處理軟體的工具，不過就 Flow 的角度，它可能會是主推用 RoseTTAFold？不然 Flow 中的 `GENOMICS` 與 `STRUCTURE` 不一定得相接，~~還能順便賣賣 GPU~~。


#### Step3. 依此分子結構來設計合成藥物化合物  
> 主要對應 Flow 中的 `SEARCH` + `DOCKING` + `SIMULATION` 區塊。

此步驟主要利用電腦模擬藥物開發所選定的靶點，來快速篩選出有機會的先導化合物，高精度計算兩者相遇後會如何作用與運動。

這麼一段話其實可以分成 3~4 個動作：

1. **選定的靶點**  
    其實這個動作應該算是前兩步驟的整合。舉例來說，涉及基因的疾病，可能在 `GENOMICS` 時分析出變異點，然後在 `STRUCTURE` 找出變異點在三維結構的位置，最終得到一個靶點，這算是我們 Step3 的 input 吧。
    
2. **篩出先導化合物**  
    在這邊不得不提到化合物篩選的幾個階段 **Hit-Lead-Candidate**：

    <p class="illustration">
        <img src="https://imgur.com/iryatts.png" alt="Hit-Lead-Candidate">
        Hit-Lead-Candidate（圖片來源: <a href="https://www.ddw-online.com/hit-to-lead-and-lead-to-candidate-optimisation-using-multi-parametric-principles-1524-200112/">Drug Discovery World (DDW)</a>）
    </p>

     
    1. **活性化合物的篩選**  
        在這階段又被稱為 **Target to Hit**，個人認為這個步驟應該對應到 Flow 中的 `SEARCH`。
        
        這邊會自動化的初步篩選出一批化合物，通常由機器完成，通常可能會篩選出 20 萬到超過 100 萬種化合物，這些對靶點有作用的化合物會被標為 hits。
        
        這批化合物，會對特定標點具有初步活性。而篩選途徑主要是隨機篩選（如：高通量篩選 High throughput screening，HTS）或是基於受體結構來找分子...等。取得一個活性化合物後可以再由其衍生出來的一系列化合物的集合。
        
        是說，在部分資料會看到有人用苗頭化合物這個詞，這個應該是中國用詞，兩個是同義詞。


    2. **先導化合物的發現**  
        這階段有個專有名詞稱之為 **Hit to Lead**。
        
        一旦獲得大量的活性化合物後，團隊會從中篩選出約 100 種活性好的化合物，作為先導化合物用於繼續深入研究。此時的化合物已經具備一定藥理學或生物學活性，但仍存在某些缺陷，而無法作為理想藥物，需要進行化學優化，將藥性與副作用優化至足以進行試驗的程度。
        
        這個步驟應該對應到 Flow 中的 `DOCKING`。
        
    3. **候選藥物的選定**  
        在該階段，也稱為**先導化合物最佳化 Lead Optimization**。這時通過詳細的生物測試來進一步指導先導化合物的優化，最後得到 1~2 種候選化合物，進行臨床試驗。
        
        不過這個小標超出範圍了，所以這個步驟與 Flow 對應等等我們下一個項目聊。

3. **高精度計算兩者相遇後會如何作用與運動**  
    這句話我們可以跟剛剛候選藥物的選定一起看，對應的是 Flow 中的 `SIMULATION`。
    
    `SIMULATION` 這區塊的主要行為是進行分子動力學模擬（Molecular Dynamics Simulation, MD），它會計算受配體在相遇後會如何作用與運動，主要模擬蛋白質動態資訊，並透過進階的生物物理實驗，如：37 度、120~150 摩爾濃度、PH=7，確認生存條件是否符合人體條件，並間接驗證所得到的動態訊息，例如：帶有時間解析度的 X-ray 晶體學、甚至是自由電子雷射。
    
    這些操作會有助於優化受體和配體的相互作用，達到活性最佳化，所以我看到有人將分子動力學模擬視為候選藥物選定操作的一部分，但我總感覺 Hit to Lead 和 Lead Optimization 的分界並不是那麼壁壘分明。
    
    <br>
    
    就目前我所知的習慣上分子嵌合與分子動力學模擬會連續操作。兩者主要都是對活性進行確認，但差別在蛋白質動態資訊，蛋白質中的每個原子位置都會隨著時間與環境而變動，這點是分子嵌合所無法模擬的。
    
    而為了彌補蛋白質動態資訊的缺失，或是當無法從 X-ray 或公開資料集取得不同構型時，部分研究中這兩個動作會形成一個迴圈，先做分子動力學模擬，取得不同的初始結構，再進行分子嵌合挑選活性，最後再做一次分子動力學模擬進行活性最佳化，前面提過的[清大楊教授](https://phys.ncts.ntu.edu.tw/uploads/asset/data/621320271d41c8381f25ce5b/2._Lee-Wei_Yang.pdf)就是這麼做的。
        
    <p class="illustration">
        <img src="https://imgur.com/KVJTOD0.png" alt="MD → Docking → MD">
        MD → Docking → MD（圖片來源: <a href="https://phys.ncts.ntu.edu.tw/uploads/asset/data/621320271d41c8381f25ce5b/2._Lee-Wei_Yang.pdf">First-principle-based and Data-driven Drug Design Platform Using a Multi-site Targeting Strategy</a>）
    </p>
  
    所以說 `SIMULATION` 算是 Lead Optimization 的一部分嗎？雖然有點曖昧不清，但以讓**先導化合物更安全更有效**的角度出發，我認為它可以算是最佳化的一環。但 `SIMULATION` 完後的結果就是候選藥物了嗎？應該還不是。
    
    從我的理解，此時的化合物只能算是經過最佳化的先導化合物，還達不到能被稱之候選藥物程度，中間可能要再經過毒性及代謝的測試...等實驗，才能放入下一步臨床階段，而不是像流程圖中直接接入臨床，中間可能省掉了些操作。這也是我先前認定 NVIDIA 繪製的是一個概括的流程圖的主要原因。
    
<br>

都提到先導化合物最佳化，就不得不提提 NVIDIA 在 Clara Discovery 主打的一個 Toolkit – **MegaMolBart**。

雖然這一工具並不在他們的流程圖上，但在[宣傳影片中提到](https://youtu.be/pySSYb0Jftk?t=75)可以用來做**先導化合物最佳化**，因此我猜測是放置在 `DOCKING` 跟 `SIMULATION` 中間，它會將 `DOCKING` 的結果做化學式的最佳化，再送入 `SIMULATION` 複測活性。從[臉書上的貼文](https://www.facebook.com/NVIDIA.TW/posts/4047371831968806/)看來，我的猜測應該也是合理。

<p class="illustration">
    <img src="https://imgur.com/Ehya9rY.png" alt="MegaMolBart 能運在虛擬篩選上進行快速分析及做先導化合物最佳化">
    （圖片來源: <a href="https://www.facebook.com/NVIDIA.TW/posts/4047371831968806/">NVIDIA Taiwan| Facebook</a>）
</p>

一般來說 Hit-Lead-Candidate 的過程曠日彌久，很難通過一次循環（`SEARCH` → `DOCKING` → `SIMULATION`）就可以找到候選藥物，通常需 **10~15 次**的嘗試才有可能找到最為合適的分子。

呃...這邊又蹦出一個專有名詞 - 虛擬篩選...嗚...我等等再開[一個小節](#分子模擬與篩選)來介紹好了，不然這一小節比重完全失衡了。


#### Step4. 臨床試驗    
> 部分對應 Flow 中的 `IMAGING` 區塊。

<p class="illustration">
    <img src="https://imgur.com/OINCJaU.png" alt="MD → Docking → MD">
    新藥開發流程（圖片來源: <a href="https://ic.tpex.org.tw/ic_report.php?ic=C100&i=3">產業價值鏈資訊平台</a>）
</p>

又可以分成臨床前試驗與臨床試驗兩個階段：
1. **臨床前試驗（Pre-clinical tests）**  
    顧名思義，在藥物應用於人體之前的試驗。會用動物對先導化合物進行**安全性評估**，其目的在於了解藥物的**療效**、**潛在副作用**和**臨床安全劑量**，這通常又要花費 1.5~3 年左右的時間。
    
    詳細來說，就是了解該藥物被活體吸收之後，在體內如何吸收、分布、代謝和排泄，以及是否能達到期望的效果與是否會產生毒性、致突變性及致癌性...等。具體涉及藥物效應動力學（Pharmacodynamics, PD）、藥物代謝動力學（Pharmacokinetics, PK）跟與疾病動物模型的有效性評估...等學科知識，這邊就略過不表。
    
    <br>
 
    有沒有發現這邊用的是**先導化合物**？就敘述來看，先導化合物最佳化和臨床前實驗在時間上是並行，他們可能得出化合物後，接著進行實驗找出缺陷，再重新進行最佳化或是找尋其他化合物。
    
    這一階段對應到流程圖中，應該位於 `SIMULATION` 與 `IMAGING` 中間，因為圖中 `IMAGING` 的輸入已經候選藥物。不過 `IMAGING` 也可用來觀察動物實驗中與醫療影像相關的變化，就是模型可能要重新訓練？
    
2. **臨床試驗（Clinical trial）**  
    這一階段主要是**評估藥物對人體療效**的方式，通常要花費 5~7 年左右的時間。
    
    臨床前試驗通過後，必須向主管機關通過試驗用新藥（investigational new drug, IND）的審核，且取得人體試驗倫理委員會（Institutional Review Board, IRB）的同意，在經核可的醫學中心或醫院進行。

    <p class="illustration">
        <img src="https://imgur.com/0GwOGU6.png" alt="各期臨床試驗目的">
        各期臨床試驗目的（圖片來源: <a href="https://www.commonhealth.com.tw/article/78924">康健雜誌</a>）
    </p>
    
    新藥的臨床試驗通常分成四期，但在進行第四期前需先申請新藥查驗登記（New drug application, NDA）。
    
    - **臨床一期**  
        此階段研究通常採用 20~50 位健康志願者為測試對象，並無治療性目的，主要目的為**測試新藥的安全性、毒性**，以了解試驗藥物的最大可能、安全與治療...等劑量及給藥時程。
      
    - **臨床二期**  
        此階段通常會有 50~300 位病患，是**探討不同劑量或治療方法**對病患的安全性與有效性，並評估其對人體的影響。
      
    - **臨床三期**  
        此階段會確認臨床二期中試驗藥物的安全性與有效性，並進行更進一步地評估。這會以 250~1000 位病患為試驗對象，將**試驗藥物與目前的標準療法作比較**。參與者會被分成對照/試驗組兩組，試驗組給予試驗藥物，而對照組可能服用已上市之藥物或是安慰劑作比較。試驗時會依雙盲（double blind）準則進行試驗，並確保試驗結果不會受到人為選擇或其他因素的影響。
      
        此階段主要目的為，判斷試驗藥物是否優於或不亞於標準療法，並為向藥品管理機構申請新藥查驗登記（NDA）做準備。

    - **臨床四期**               
        亦名上市後臨床試驗**監視期**，作長期使用是否會產生慢性副作用、發生率極低之不良反應或嚴重副作用的追蹤與評估。
      


## 分子模擬與篩選
這章算是不小心開出來的支線任務 XDDD 內容主要是針對前面所提到的一些專有名詞做出補充說明。


### 分子模擬（Molecular Simulations）
分子模擬是運用力學理論與電腦計算，來構築分子結構與行為，進而模擬各種物理、化學性質與分子間及分子內的交互作用，以探討系統微觀結構與能量的關係。還可模擬定溫、定壓...等實驗條件，從分子角度闡釋其運動行為，甚至做出預測。除了模擬分子的動態行為外，也可以用來模擬分子的靜態結構。

在藥物設計中，最常被~~我這篇網誌~~提及的兩個方法分別是 **分子嵌合（Molecular Docking）** 與 **分子動力學模擬（Molecular Dynamics Simulation, MD）**：

#### ⎚ 分子嵌合（Molecular Docking）
分子嵌合，又譯分子對接，是分子模擬的重要方法之一，其本質是分子間的識別過程，過程涉及分子之間的空間和能量匹配。不過說實話，分子對接這個翻譯搞不好比較熱門，但我這邊還是先以[教育研究院雙語詞彙](https://terms.naer.edu.tw/detail/769497/)的翻譯為主。

嵌合的主要目的是找到受體與配體**最佳結合位置**及**其結合強度**，在過程中會透過電腦模擬將配體放置於受體的結合區域，再通過計算物化參數預測兩者的結合自由能和方式，最終可以獲得受體與配體的結合構型。結果所得出的可能構型不只一種，一般認為**自由能最小的構型**存在的機率最高，但不一定是最佳結果。

<p class="illustration">
    <img src="https://i.imgur.com/ZppJgdS.jpg" alt="Molecular Docking">
    Molecular Docking（圖片來源: <a href="https://zhuanlan.zhihu.com/p/43686534">魔德科技｜知乎</a>）
</p>

**受體與配體的結合力**   
既然提到受體與配體的結合與自由能，這邊還是稍微說明下兩者之間的關係。

受體與配體的結合端賴**分子間作用力**，其中並未進行化學反應，也沒有生成共價鍵，因此這樣的結合方式是可以拆解，也是可以被取代的。所以有可能出現因作用力不夠強烈，而導致**脫靶現象**。
    
血紅素與氧氣及一氧化碳的三角關係就是一例：身為受體的血紅素在平時都與氧氣結合，但是當空氣中的一氧化碳濃度過高時，血紅素就會拋下氧分子，選擇和一氧化碳分子結合。這是因為一氧化碳對血紅素的親和力大過於氧氣的親和力，所以在與受體結合的競爭關係中，**親和力越大**的分子越容易取得勝利。
  
<br>

那麼親和力與前述所說的自由能又是？

親和力（Affinity）一詞最早是屬於化學領域的一個概念，特指一種原子與另外一種原子之間的關聯特性；而自由能則是物理化學的名詞，在熱力學第二定律中，所有反應過程中能自發進行達到的程度都是由初態反應物與終態產物之間的自由能變化決定的。在用於描述受體與配體之間的自由能，稱之為**結合自由能**（Binding Affinity/Binding Free Energies）或 **結合能**（Binding Energy）。
   
在實際的配體-受體複合物體系中，一般結合自由能只受**非鍵相互作用力** （non-bonded interactions）的影響。這作用力較弱且能量低，存在於大部分體系中，主要由三項構成：**凡得瓦力**、**靜電作用**和**氫鍵作用**。因為非鍵相互作用力的結合是可逆的，有利於自身的代謝和排泄，但也可能出現脫靶現象。
   

總而言之，藥物設計的結合自由能主要看非鍵相互作用力，**結合自由能越低表配體-受體結合所需能量越低，親和力越高，越容易結合**。


<br>

**分子嵌合的原理和類別**  
受體與配體結合必須滿足互相匹配原則，除了必須滿足靜電、氫鍵與疏水相互作用互補匹配，另外還須滿足**受體與配體幾何形狀互補匹配**。

- **鎖鑰理論（Lock and Key Theory）**：   
    19 世紀 80 年代初期，德國化學家 Emil Fischer 提出，分子和生物細胞之間的關係存在高度的專一性，就像鎖頭和鑰匙一樣，在空間形狀上要互相匹配，每道鎖都必須利用某把特定的鑰匙才能夠打開。  <br>   

    <p class="illustration">
     <img src="https://imgur.com/WguFWSh.png" alt="鎖鑰理論">
     鎖鑰理論（圖片來源: <a href="https://smallcollation.blogspot.com/2013/04/models-of-enzyme.html#gsc.tab=0">小小整理網站</a>）
    </p>

    在鎖鑰理論中將受體與配體視為**剛性**不易變動的結構，此理論能夠說明結合前後三維結構變化較小的情況，但對結合前後三維結構變化較大的情況無法解釋。

- **誘合理論/誘導契合（Induced-Fit Theory）**：   
    然而，也有一些理論認為受體是**柔性**分子而非固定不動的剛性結構，因此可以在與形狀不完全相合的配體結合時，改變自己的構型去配合配體的形狀，這種具有較大彈性的配對結合方式，亦即受體會**受到形狀不合的配體誘導而改變構型與之結合**。<br>  

    <p class="illustration">
        <img src="https://imgur.com/FWQooNK.png" alt="誘導契合">
        誘導契合（圖片來源: <a href="https://smallcollation.blogspot.com/2013/04/models-of-enzyme.html#gsc.tab=0">小小整理網站</a>）
    </p>

根據上述兩種理論與受體與配體的剛柔特性，嵌合的方法可分為 3 類：
 
- **剛性嵌合**   
    符合鎖鑰理論，在剛性嵌合方法在計算過程中，參與嵌合的分子構型不發生變化，僅改變分子的空間位置。 
    <br> 

- **半柔性嵌合**  
    在半柔性嵌合方法中，一般是將受體蛋白設爲剛性，配體設爲柔性。在嵌合過程中配體構型發生一定程度的變化，但構型的調整也可能受到一定程度的限制，如固定某些非關鍵部位的鍵長、鍵角等。 
    <br> 

- **柔性嵌合**  
    A柔性嵌合方法在嵌合過程中允許研究體系的構型發生自由變化，由於變量隨著體系的原子數呈幾何級數增長，計算量會非常龐大。
  
  
整體來說
- 剛性嵌合，簡化程度高、計算量較小，適合於處理大分子之間的嵌合。
- 半柔性嵌合，兼顧計算量與預測能力，是應用較廣泛的嵌合，多數嵌合軟體的預設值。
- 柔性嵌合，最廢算力，適合精確考察分子間識別情況。
 
 
#### ⎚ 分子動力學模擬（Molecular Dynamics Simulation, MD）
分子動力學模擬是一種分子模擬方法，該方法以經典力學、量子力學、統計力學為基礎，利用電腦數值求解分子體系運動方程的方法，類比研究**分子體系的結構與性質**，模擬分子體系在一段時間內，位置與速度的運動軌跡，從而以動態觀點洞察系統隨時間演化的行為。是繼實驗和理論兩種研究方法之後，研究分子體系結構與性質的第三種科學研究方法。

<p class="illustration">
    <img src="https://imgur.com/E0qiAgi.png" alt="分子模擬是運用牛頓和統計力學理論與電腦計算，探討系統微觀結構與能量的關係。">
    分子模擬是運用牛頓和統計力學理論與電腦計算，探討系統微觀結構與能量的關係。（圖片來源: <a href="https://scitechvista.nat.gov.tw/Article/C000003/detail?ID=7f99c34a-156e-485f-a9fe-d3ce23f2851d">科技大觀園</a>）
</p>

研究蛋白質和配體的結合模式中，嵌合與動力學模擬是兩個最重要的計算生物學方法，各有特點，相互補充：
1. **如果把嵌合比喻為一幅圖片，那麼動力學模擬就像是一幀楨畫面組成的動態電影**  
    雖說嵌合依照受體與配體的剛柔特性區分會有 3 種嵌合方式，但即便設置柔性嵌合，蛋白質也只是活性口袋的幾個氨基酸殘基在動，其餘幾乎是固定的，並不能展現蛋白質的整體運動；此外，嵌合並未考慮時間因素，只看是否結合，因此非常有可能結合後馬上就掉下來了。

    反之，在進行動力學模擬時，根據誘合理論蛋白質會作出相應的構型改變去適應配體，蛋白質整體都會產生運動。另外更重要的是它能夠模擬實際情況，通過控制諸如溫度、壓力、原子數、離子濃度和溶劑的類型等不同因素進行實驗，達到控制蛋白質的柔性程度、觀察運動軌跡的目的。

    因此做動力學模擬是有必要的！ 

2. **可以為互相對方提供不同初始條件**  
    出於優化受體與配體的目的，可以讓兩者相互配合，互為相對方提供不同初始條件。
    
    若蛋白質結構是從公開資料集或是預測所得，則可能需要先進行動力學模擬，產生更多或更優的起始蛋白質構型。而在執行動力學模擬之前，可能需要先進行嵌合提供不同的初始結合模式，增加模擬的多樣性，提高模擬效率。
 
<br>

因此，在實務上嵌合與動力學模擬可能不只執行一次，由哪個操作開始也未定，端看你的資料與應用。不過我在[論壇](http://bbs.keinsci.com/thread-25666-1-1.html#pid173778)中看到是推薦先做嵌合，再做動力學模擬，原因如下：
1. 動力學模擬是相對昂貴的計算，做一次模擬的成本比嵌合高的多，如果沒有嵌合做參考直接做動力學模擬感覺浪費計算資源。
2. 動力學結果重現性較差，每次執行結果迥然有異，如果初始結構無所依的話要想得到好的結果會比較困難。


### 藥物篩選（Drug Screening）  
藥物篩選是在藥物設計流程中找出**具有特定生理活性化合物**的一個步驟。它會通過實驗、模擬或預測等方式，從大量化合物找出對靶點具有高活性的化合物的過程。從維基百科給的流程圖看來，廣義的藥物篩選步驟涵蓋了 **Target to Hit** 與 **Hit to Lead** 兩個階段。

<p class="illustration">
    <img src="https://imgur.com/IGRymF7.png" alt="新藥研發流程">
    新藥研發流程（圖片來源: <a href="https://zh.wikipedia.org/wiki/File:Drugscreentw.jpg">維基百科</a>）
</p>

常用的篩選方式有高通量篩選（High Throughput Screening，HTS）和虛擬篩選（Virtual Screening，VS）。


#### ⎚ 高通量篩選（High Throughput Screening，HTS）
高通量篩選是伴隨組合化學而生的一種藥物篩選方式。它是以分子等級和細胞等級的**實驗方法**為基礎、自動化的操作實驗系統執行試驗、高靈敏度檢測系統及數據採集系統搜羅實驗結果，最後以電腦分析處理實驗數據的一門技術。

高通量篩選可以通過一次實驗獲得大量的訊息，並從中找到有價值的資訊，可應用於對現有化合物資料庫的篩選，是**當今藥物開發的主要方式**。

先撇除偽陽、偽陰的結果誤判，高通量篩選亦存在許多問題：
1. **化合物樣本來源短缺、試劑和儀器昂貴**  
    先前提到高通量篩選是以實驗方法為基礎，因此需要實體樣本與儀器。但有些化合物樣本數量稀少或是難以取得，甚至不是天然化合物；此外，此方法所需使用的試劑和儀器都非常昂貴，除大型製藥公司外，大部分科研單位都無法配備。
    
2. **通用性不高**  
    很多高通量篩選方法只能對一種或幾種酶具有適應性。

<br>   

我們先回到 Clara Discovery 上。因高通量篩選是藉由實驗篩選所得，應該不屬於電腦輔助藥物設計。不過如果硬要把它對應到流程圖中，應該會對應到圖中 `SEARCH`，也就是活性化合物的篩選 Target to Hit 階段。此階段所得的化合物，需要再進行活性測試與最佳化，以得到一系列先導化合物。


#### ⎚ 虛擬篩選（Virtual Screening，VS）
<p class="illustration">
    <img src="https://imgur.com/hwprwe6.png" alt="虛擬篩選">
    虛擬篩選（圖片來源: <a href="https://www.light-biotech.com/virtual-screeing">lightbiotech</a>）
</p>

由於實體藥物篩選需要取得大量的化合物樣本，以構建大規模的化合物資料庫，並且需要昂貴試劑和儀器支持。因此隨著電腦輔助藥物設計的快速發展，虛擬藥物篩選成了是藥物篩選技術發展的另一個方向。

虛擬篩選為發現特定生理活性化合物提供了一種快速而經濟的方法。因為它是將其篩選過程在電腦上模擬，因此不存在樣品限制與消耗，從而**降低篩選成本**、縮**短新藥研發週期**，並加入考慮化合物分子藥動學性質和毒性等因素。除此之外**命中率也獲得了提升**，相較實驗性高通量篩選的典型命中率 **0.01％ ~ 0.14％**，虛擬篩選可提升至 **2％ ~ 40％**。

<br>

虛擬篩選會因藥物設計開發法的不同而有不同的工作流程：**基於分子嵌合的虛擬篩選**、**基於藥效團的虛擬篩選**

<p class="illustration">
    <img src="https://imgur.com/103yXRQ.png" alt="因藥物設計開發法的不同而有不同的工作流程">
    因藥物設計開發法的不同而有不同的工作流程（圖片來源: <a href="https://link.springer.com/article/10.1007/s12272-015-0640-5">Role of computer-aided drug design in modern drug discovery</a>）
</p>

**基於分子嵌合的虛擬篩選**  
[分子嵌合](#⎚-分子嵌合（Molecular-Docking）)我們前面提過，它是基於標靶三維結構的分子模擬，通過多個分子間的識別，綜合分析得分，找到受體與配體最佳結合位置及其結合強度，為候選化合物的挑選提供依據，篩選潛在活性化合物。

[前面](#常見藥物設計法)提過 Clara Discovery 就是採用基於結構的藥物設計設計法來開發的，所以在 <mark>Clara Discovery 中應該是基於分子嵌合的虛擬篩選</mark>。若將虛擬篩選對應到流程圖中的 `SEARCH` + `DOCKING`，而嵌合結束後的虛擬篩選也被歸入 `DOCKING` 中，因為常見執行分子嵌合軟體都有提供虛擬篩選的設置。

另外，由於嵌合的過程對化合物結構並沒有限制，基於分子嵌合的虛擬篩選完全有可能獲得**結構新穎**的先導化合物。

<p class="illustration">
    <img src="https://imgur.com/puDCyoc.png" alt="基於分子嵌合的虛擬篩選">
    基於分子嵌合的虛擬篩選（圖片來源: <a href="https://www.light-biotech.com/virtual-screeing">lightbiotech</a>）
</p>

是說我怎麼感覺基於分子嵌合的虛擬篩選好像是直接從 Target to Lead？

<br>

**基於藥效團的虛擬篩選**  
> 藥效團（Pharmacophore）的概念是指一組化合物與同一標靶結合時，所需的特定原子和官能基的空間分布，這個概念的本質是從具有相同機理的不同化合物中，提出決定活性的共同結構特徵，而強調的是呈現活性的功能基團之性質和位置。

我放棄理解這句話的意思，直接上結論吧 XDDD

藥效團篩選的計算量較小，可以在分子嵌合前進行，篩選只需要很短時間。另外，此篩選法所獲得的化合物與建模所用的化合物結構性質類似，在發展新穎先導化合物方面具有一定的侷限性。



## 下一步
藥物設計流程大概到這邊告一個斷落，至於 Clara Discovery Toolkits 就請期待下集吧，應該不會太久，都被要筆記要到三次元來了。

<div class="alert primary no-icon"> 
🤟 喔耶～我完成了~~ → <a href="https://cynthiachuang.github.io/NVIDIA-Clara-Discovery-Toolkits">NVIDIA Clara Discovery - Toolkits 篇</a>
</div>



##  參考資料
1. 協同撰寫。[先導化合物](https://zh.wikipedia.org/wiki/%E5%85%88%E5%AF%BC%E5%8C%96%E5%90%88%E7%89%A9)。檢自 維基百科 (2022-04-19)。
2. 陳欣文、孫彬訓 (2021.11.10)。[科技神助攻 新藥研發加速](https://ctee.com.tw/news/fund/545734.html)。檢自 工商時報 (2022-04-19)。
3. 智東西 (2020-08-27)。[NVIDIA 安培架構 GPU 公開課下周開講，詳解 NVIDIA GPU 如何加速抗疫藥物研發及科學新發現](https://twgreatdaily.com/xcmCL3QBURTf-Dn5TWju.html)。檢自 每日頭條 (2022-04-19)。
4. ypei91510 (2020-11-10)。[人工智慧AI設計藥物首度進入人體試驗階段，是否能帶來藥物發展突破？](https://aigo.org.tw/zh-tw/forum/content/164)。檢自 AIGO-AI產業實戰應用人才淬煉計畫 (2022-04-19)。
5. [Clara Discovery](https://www.nvidia.com/zh-tw/clara/drug-discovery/)。檢自 NVIDIA (2022-04-19)。
6. [運用加速運算平台研發藥物](https://www.nvidia.com/zh-tw/industries/healthcare-life-sciences/drug-discovery/)。檢自 NVIDIA (2022-04-19)。
7. Roy Chou (2021-03-20)。[CLARA SDK 介紹](https://roychou121.github.io/2021/02/15/nvidia-clara/)。檢自 不務正業工程師的家 (2022-04-19)。
8. 雕刻時光 (2020-10-06)。[NVIDIA Clara Discovery Platform](https://blog.cineneural.com/zh-cn/blog/2020-10/nvidia-clara-discovery/)。檢自 CineNeural Mathematics (2022-04-19)。
9. (2021-04-08)。[蹲！加速AI藥物研發，NVIDIA 數據科學家帶你到行業風口](https://www.sohu.com/a/459640351_133140)。檢自 搜狐網 (2022-04-19)。
10. 楊又肇 (2020-10-05)。[NVIDIA攜手全球第三大藥廠GSK加快藥物研發 於英國建造AI超級電腦Cambridge-1](https://mashdigi.com/nvidia-on-ai-developing-for-health/)。檢自 Mashdigi (2022-04-19)。
11. 協同撰寫。[藥物設計](https://zh.wikipedia.org/wiki/%E8%97%A5%E7%89%A9%E8%A8%AD%E8%A8%88)。檢自 維基百科 (2022-04-19)。
12. 王任小 (2016-12)。[分子模擬方法的應用](http://bnpc.sioc.ac.cn/upload/resume/POCwangrx-7-v2016.pdf)。檢自 中科院研究生物理有機化學課程 (2022-04-19)。
13. 藥渡 (2017-06-01)。[經久不衰的「合理藥物設計」](https://kknews.cc/design/lpnnalg.html)。檢自 每日頭條計 (2022-04-19)。
14. Gerhard Klebe (2019-05-01)。[藥物設計：方法、概念和作用模式](https://book.sciencereading.cn/shop/book/Booksimple/onlineRead.do?id=B8B1CF16CF3798C4BE053020B0A0AA597000&readMark=0#)
15. PharmaEducation Team。[Difference between Drug Design and Drug Development](https://pharmaeducation.net/difference-between-drug-design-and-drug-development/)。檢自 PharmaEducation (2022-04-19)。
16. 協同撰寫。[Drug discovery](https://en.wikipedia.org/wiki/Drug_discovery)。檢自 Wikipedia (2022-04-19)。
17. Amol B Deore, Et al. (2019-12)。[The Stages of Drug Discovery and Development Process](https://www.researchgate.net/publication/341097009_The_Stages_of_Drug_Discovery_and_Development_Process)。檢自 ResearchGate (2022-04-19)。
18. Shu-Feng Zhou1, Wei-Zhu Zhong (2017-02)。[Drug Design and Discovery: Principles and Applications](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6155886/#B1-molecules-22-00279)。檢自 PMC (2022-04-19)。
19. Tamanna Anwar, Et al. (2021-02)。[Molecular Docking for Computer-Aided Drug Design](https://www.sciencedirect.com/science/article/pii/B9780128223123000114)。檢自 ScienceDirect (2022-04-19)。
20. 黃云宣 (2021-10)。[電腦輔助藥物設計打破缺藥僵局：以熱帶血吸蟲病為例](https://investigator.tw/12365/%E9%9B%BB%E8%85%A6%E8%BC%94%E5%8A%A9%E8%97%A5%E7%89%A9%E8%A8%AD%E8%A8%88%E6%89%93%E7%A0%B4%E7%BC%BA%E8%97%A5%E5%83%B5%E5%B1%80%EF%BC%9A%E4%BB%A5%E7%86%B1%E5%B8%B6%E8%A1%80%E5%90%B8%E8%9F%B2%E7%97%85/)。檢自 The Investigator Taiwan (2022-04-19)。
21. 黄明威 (2019-04-17)。[藥物篩選之計算機輔助藥物設計](https://zhuanlan.zhihu.com/p/62725907)。檢自 知乎 (2022-04-19)。
22. 協同撰寫。[計算機輔助藥物設計](https://baike.baidu.hk/item/%E8%A8%88%E7%AE%97%E6%A9%9F%E8%BC%94%E5%8A%A9%E8%97%A5%E7%89%A9%E8%A8%AD%E8%A8%88/9987932)。檢自 百度百科 (2022-04-19)。
23. 馮昇華、陳立立 (2020-02-06)。[新藥開發流程解析，如何用數位化工具、系統逐步抗擊病毒！](https://blogs.3ds.com/taiwan/%E6%96%B0%E8%97%A5%E9%96%8B%E7%99%BC%E6%B5%81%E7%A8%8B%E8%A7%A3%E6%9E%90%EF%BC%8C%E5%A6%82%E4%BD%95%E7%94%A8%E6%95%B8%E4%BD%8D%E5%8C%96%E5%B7%A5%E5%85%B7%E3%80%81%E7%B3%BB%E7%B5%B1%E9%80%90%E6%AD%A5/)。檢自 (2022-04-19)。
24. USER (2020-12-17)。[加速小分子新藥開發流程 – 分子智藥](https://ntpc-awsjic.com.tw/interview-virtualman-medicine-invent/)。檢自 亞馬遜 AWS 聯合創新中心 (2022-04-19)。
25. 科學月刊 (2013-04-05)。[新藥的研發流程概論](https://pansci.asia/archives/38529)。檢自 PanSci 泛科學 (2022-04-19)。
26. 陳培英 (2017-09-15)。[「生物藥」強勢崛起](https://www.gvm.com.tw/article/40097)。檢自 遠見雜誌 (2022-04-19)。
27. 柯旂、張語辰 (2018-07-10)。[藥物如何在體內發生反應？先用電腦模擬！專訪林榮信](https://research.sinica.edu.tw/lin-jung-hsin-pharmacoinformatics-computational-biophysics/)。檢自 中央研究院研之有物 (2022-04-19)。
28. 林榮信 (2013-04-01)。[挑戰神奇子彈—高效能計算與藥物設計](http://scimonth.blogspot.com/2013/04/blog-post_8417.html)。檢自 科學月刊 (2022-04-19)。
29. 求實醫藥 (2021-03-09)。[使用表型篩選發現全新的抗體藥物及靶點：能否摘下掛的最高的抗體藥物果實？](http://med.china.com.cn/content/pid/241994/tid/1026/iswap/1)。檢自 中國網醫療頻道 (2022-04-19)。
30. CASE PRESS (2010-12-23)。[【化學奇境】受體‧受體‧受體](https://case.ntu.edu.tw/blog/?p=6809)。檢自 CASE報科學 (2022-04-19)。
31. Egtok (2010-12-23)。[自由能專題1：原理及常見方法](https://zhuanlan.zhihu.com/p/365654509)。檢自 知乎 (2022-04-19)。
32. [酵素模型(Models of Enzyme)](https://smallcollation.blogspot.com/2013/04/models-of-enzyme.html#gsc.tab=0)。檢自 小小整理網站 Smallcollation (2022-04-19)。
33. (2009-03-06)。[淺談蛋白質資料庫（Protein Data Bank）](https://highscope.ch.ntu.edu.tw/wordpress/?p=967)。檢自 科學Online (2022-04-19)。
34. 藥渡 (2017-02-05)。[先導化合物的確定，絕不是簡單的「篩活性」！](https://kknews.cc/zh-tw/science/e9ll26q.html)。檢自 每日頭條 (2022-04-19)。
35. 中大唯信 (2018-11-12)。[藥物設計中結合自由能的計算](https://kknews.cc/zh-tw/health/6zqgq3v.html)。檢自 每日頭條 (2022-04-19)。
36. 王國慧、陳天文 (2014-03)。[蛋白質體學於運動科學之應用](http://tkuir.lib.tku.edu.tw:8080/dspace/retrieve/149656/%E8%9B%8B%E7%99%BD%E8%B3%AA%E9%AB%94%E5%AD%B8%E6%96%BC%E9%81%25)。檢自 中華體育季刊 (2022-04-25)。
37. 寶來證券 (2001-10-02)。[「結構基因體學/蛋白質體學」對新藥開發的影響](https://www.moneydj.com/kmdj/report/reportviewer.aspx?a=3a372f85-3450-4182-adee-1c4ce9b13fe9)。檢自 MoneyDJ理財網 (2022-04-25)。
38. 邵偉凱。[結構生物學與低溫電子顯微鏡影像分析 心得分享](https://modular-course.web2.ncku.edu.tw/p/404-1100-192842.php)。檢自 國立成功大學理學院模組化課程 (2022-04-25)。
39. 黃欣。[初學者投資生技 新藥篇](https://topics.cnyes.com/biotech01/)。檢自 亨鉅網 (2022-06-01)。
40. 克里克學院 (2017-05-09)。[淺談基於片段的藥物設計](https://kknews.cc/zh-tw/design/4mrylzv.html)。檢自 每日頭條 (2022-06-01)。
41. 藥渡 (2018-03-22)。[FBDD~基於片段的藥物發現](https://www.gushiciku.cn/dc_hk//106108629)。檢自 微文庫 (2022-06-01)。
42. 有機合成 (2021-08-13)。[【收藏】一圖瞭解新藥研發到上市流程](https://www.daytime.cool/tech/7482826.html)。檢自 日間新聞 (2022-06-01)。
43. 殷賦科技 (2017-10-24)。[基於藥效團的虛擬篩選發現新STAT3二聚體抑制劑](https://kknews.cc/science/krea2rp.html)。檢自 每日頭條 (2022-06-01)。
44. 黃云宣 (2019-05)。[AlphaFold 與 RoseTTAFold：蛋白質結構預測誰更勝一籌？](https://investigator.tw/11852/news-alphafold-%E8%88%87-rosettafold%EF%BC%9A%E8%9B%8B%E7%99%BD%E8%B3%AA%E7%B5%90%E6%A7%8B%E9%A0%90%E6%B8%AC%E8%AA%B0%E6%9B%B4%E5%8B%9D%E4%B8%80%E7%B1%8C%EF%BC%9F/)。檢自 The Investigator Taiwan (2022-06-01)。
45. 小刚刚 HitGen (2016-08-03)。[漫谈新药研发之路——先导化合物的发现](https://zhuanlan.zhihu.com/p/21854203)。檢自 知乎 (2022-06-01)。
46. 林承勳、林洵安 (2020-04-17)。[【中研抗疫2】小分子藥物研發策略：優先考慮學名藥，盼及時找到救命解方](https://research.sinica.edu.tw/covid-19-drug-academia-sinica/)。檢自 中央研究院研之有物 (2022-04-19)。
47. 科學月刊 (2013-04-05)。[新藥的研發流程概論](https://pansci.asia/archives/38529)。檢自 PanSci 泛科學 (2022-06-02)。
48. 廖宗志。[臨床試驗分期介紹](https://www.vghtc.gov.tw/UploadFiles/WebFiles/WebPagesFiles/Files/22b3f1f4-7acc-4206-97a4-d1c3773bae46/%E8%87%A8%E5%BA%8A%E8%A9%A6%E9%A9%97%E5%88%86%E6%9C%9F%E4%BB%8B%E7%B4%B9%EF%BC%9A%E7%AC%AC%E4%B8%80%E6%9C%9F%E8%87%B3%E7%AC%AC%E5%9B%9B%E6%9C%9F%E8%87%A8%E5%BA%8A%E8%A9%A6%E9%A9%97%E7%B0%A1%E4%BB%8B.pdf)。檢自 台中榮總 (2022-06-02)。
49. julia (2018-06-06)。[從實驗到上市，一款藥物的開發可以耗費多少青春與成本？](https://www.thenewslens.com/article/95507)。檢自 The News Lens 關鍵評論網 (2022-06-02)。
50. (2022/1/12)。[認識臨床試驗](https://www.cgh.org.tw/ec99/rwd1320/category.asp?category_id=384)。檢自 國泰綜合醫院 (2022-06-02)。
51. 爱菩新医药 (2018-12-25)。[当虚拟筛选遇上高通量筛选](https://blog.csdn.net/weixin_44246077/article/details/85242622)。檢自 爱菩新医药的博客｜CSDN博客 (2022-06-02)。
52. [虛擬篩選--小分子藥物設計](https://www.cloudscientific.com/micromolecule-23)。檢自 康昱盛 (2022-04-19)。
53. [虛擬篩選](https://www.light-biotech.com/virtual-screeing)。檢自 lightbiotech (2022-04-19)。 
54. 魔德科技 (2018-08-29)。[基于结构的药物设计：从分子对接到分子动力学](https://zhuanlan.zhihu.com/p/43216518)。檢自 知乎 (2022-04-19)。
55. NVIDIA (2021-08-31)。[利用 NeMo 1.0 的全新先進神經網路和功能加快對話式人工智慧研究](https://blogs.nvidia.com.tw/2021/08/31/accelerating-conversational-ai-research-with-new-cutting-edge-neural-networks-and-features-from-nemo-1-0/)。檢自 NVIDIA 台灣官方部落格 (2022-04-19)。
56. 海大风妖 (2021-10-20)。[虚拟筛选-分子对接](https://blog.csdn.net/qq_34186880/article/details/120861561)。檢自 海大风妖的博客｜CSDN博客 (2022-06-07)。
57. 李博, Et al. (2019-03-21)。[分子对接与分子动力学计算模拟概论](https://pdf.hanspub.org/CC20190100000_98590607.pdf)。檢自 Journal of Comparative Chemistry 比较化学 (2022-06-07)。
58. Yang (2017-10-06)。[蛋白-配体结合:联合使用分子对接和分子动力学模拟](http://yangli.name/2017/10/06/20171006docking/)。檢自 Yang's Garden (2022-06-07)。
59. fffff (2021-10-12)。[[综合交流] 请教，蛋白和配体做完分子对接后，为啥还要做分子动力学模拟 #2](http://bbs.keinsci.com/forum.php?mod=redirect&goto=findpost&ptid=25666&pid=173778)。檢自 计算化学公社 (2022-06-07)。
60. 邱繼正 (2019-04-30)。[見微知著─分子模擬的應用](https://scitechvista.nat.gov.tw/Article/C000003/detail?ID=7f99c34a-156e-485f-a9fe-d3ce23f2851d)。檢自 科技大觀園 (2022-06-07)。
61. 協同撰寫。[藥物篩選](https://zh.wikipedia.org/wiki/%E8%8D%AF%E7%89%A9%E7%AD%9B%E9%80%89)。檢自 維基百科 (2022-06-08)。
62. 林振文 (2013-02)。[新農藥開發之理論設計簡介](https://www.coa.gov.tw/ws.php?id=2446932)。檢自 農委會 (2022-06-08)。
63. (2021-07-28)。[MegaMolBART](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/clara/models/megamolbart)。檢自 維基百科 NVIDIA NGC (2022-06-09)。
64. 協同撰寫。[簡化分子線性輸入規範](https://zh.wikipedia.org/zh-tw/%E7%AE%80%E5%8C%96%E5%88%86%E5%AD%90%E7%BA%BF%E6%80%A7%E8%BE%93%E5%85%A5%E8%A7%84%E8%8C%83)。檢自 維基百科 (2022-06-09)。
65. xk6891 (2021-05-03)。[SMILES:一种简化的分子语言](https://blog.csdn.net/xk6891/article/details/116380262)。檢自 xk6891的博客｜CSDN (2022-06-10)。
67. 協同撰寫。[小分子](https://zh.wikipedia.org/zh-tw/%E5%B0%8F%E5%88%86%E5%AD%90)。檢自 維基百科 (2022-06-10)。
68. 協同撰寫。[autodock](https://baike.baidu.hk/item/autodock/5684905)。檢自 百度百科 (2022-06-16)。
69. 魔德科技 (2018-08-25)。[分子对接软件综述](https://zhuanlan.zhihu.com/p/42834554)。檢自 知乎 (2022-06-16)。
70. 協同撰寫。[分子對接](https://zh.wikipedia.org/zh-tw/%E5%88%86%E5%AD%90%E5%AF%B9%E6%8E%A5)。檢自 維基百科 (2022-06-16)。
71. 互动派 (2021-08-18)。[分子对接软件大比拼](https://zhuanlan.zhihu.com/p/400913640)。檢自 知乎 (2022-06-16)。
72. 殷赋科技 (2019-11-25)。[有关UCSF DOCK6、AUTODOCK 与AUTODOCK VINA对接准确率的探讨](https://zhuanlan.zhihu.com/p/93534382)。檢自 知乎 (2022-06-16)。
73. Garrett (2020-01-16)。[AutoDock 4 and AutoDock Vina](https://www.blopig.com/blog/2020/01/autodock-4-and-autodock-vina/)。檢自 Oxford Protein Informatics Group (2022-06-16)。
74. 纽普生物 (2019-05-07)。[教程：用AutoDock Vina进行分子对接](https://www.novopro.cn/articles/201905071181.html)。檢自 纽普生物  (2022-06-16)。
75. 協同撰寫。[GROMACS](https://baike.baidu.hk/item/GROMACS/2243638)。檢自 百度百科 (2022-06-17)。
76. (2022-05-19)。[使用GROMACS软件进行高性能计算](https://help.aliyun.com/document_detail/57897.html)。檢自 阿里云 (2022-06-17)。
77. (2016-08-12)。[分子模擬](http://md.hzsh.us/tag/%E5%88%86%E5%AD%90%E6%A8%A1%E6%93%AC)。檢自 分子動力學模擬 (2022-06-17)。
78. 清榎 (2022-04-01)。[Gromacs初探](https://blog.csdn.net/weixin_41997940/article/details/123838272)。檢自 清榎的博客｜CSDN博客 (2022-06-19)。
79. Alan Gray (2020-02-25)。[Creating Faster Molecular Dynamics Simulations with GROMACS 2020](https://developer.nvidia.com/blog/creating-faster-molecular-dynamics-simulations-with-gromacs-2020/)。檢自 NVIDIA Technical Blog (2022-06-17)。
80. Alan Gray, Szilárd Páll (2021-10-08)。[Maximizing GROMACS Throughput with Multiple Simulations per GPU Using MPS and MIG](https://developer.nvidia.com/blog/maximizing-gromacs-throughput-with-multiple-simulations-per-gpu-using-mps-and-mig/)。檢自 NVIDIA Technical Blog (2022-06-17)。
81. 李宗翰 (2020-05-28)。[運算效能提升20倍，Nvidia新款資料中心等級GPU上陣](https://www.ithome.com.tw/review/137818)。檢自 iThome  (2022-06-17)。
82. Edward (2019-04-14)。[NVIDIA MPS总结](https://asphelzhn.github.io/2019/04/14/tensor_09_MPS/)。檢自 Edward (2022-06-17)。
83. Jerkwin。[GROMACS中文教程](https://jerkwin.github.io/9999/10/31/GROMACS%E4%B8%AD%E6%96%87%E6%95%99%E7%A8%8B/)。檢自 哲●科●文Jerkwin (2022-06-19)。
84. 张群峰 (2021-07-30)。[初学Gromacs](http://www.bioengx.com/%E5%88%9D%E5%AD%A6gromacs/)。檢自 BioEngX (2022-06-19)。
85. lammps加油站 (2021-02-20)。[lammps教程:nve/nvt/npt系綜設置方法](https://ppfocus.com/0/sc39927c8.html)。檢自 人人焦點 (2022-06-19)。
86. Jerkwin。[GROMACS文件类型](https://jerkwin.github.io/9999/10/01/GROMACS%E6%96%87%E4%BB%B6%E7%B1%BB%E5%9E%8B/)。檢自 哲●科●文Jerkwin (2022-06-19)。
87. 于浩然 (2016-12-02)。[你知道Gromacs中各文件的用途吗？](http://www.bioengx.com/gromacs/)。檢自 BioEngX (2022-06-19)。
88. [分子动力学软件那个比较快？](https://www.zhihu.com/question/453732470)。檢自 知乎 (2022-06-19)。  
89. auybv (2010-10)。[【求助】Gromacs和NAMD比Amber快很多吗？如何选择？](http://muchong.com/html/201706/2523471.html)。檢自 小木虫 (2022-06-19)。
90. linxi1454。[【转贴】大分子蛋白质模拟：Amber, Gromacs和NAMD之我见](http://muchong.com/html/200806/861914.html)。檢自 小木虫 (2022-06-19)。
91. Zhang Xinhuai (2017-12-11)。[HOW FAST CAN AMBER AND GROMACS JOB RUN WITH P100 GPU ACCELERATOR](https://nusit.nus.edu.sg/services/hpc-newsletter/how-fast-can-amber-and-gromacs-job-run-with-p100-gpu-accelerator/)。檢自 NUS Information Technology (2022-06-19)。
92. [Amber Hardware & Software Configurations](https://www.nvidia.com/es-la/data-center/gpu-accelerated-applications/amber/)。檢自 NVIDIA Data Center (2022-06-19)。
93. Higher Ed & Research (2012-05-08)。[Molecular Dynamics Applications Overview AMBER NAMD GROMACS LAMMPS Sections Included](https://slideplayer.com/slide/4460537/)。檢自 SlidePlayer (2022-06-19)。
94. tiehan (2020-12-10)。[【3.2.1】gromacs教程--水中的溶菌酶](https://qinqianshan.com/bioinformatics/molecular_dynamics/gromacs-water-lysozyme/)。檢自Sam' Note (2022-06-20)。



## 更新紀錄
<details class="update_stamp">
    <summary>最後更新日期：2022-06-29</summary>
    <ul>
        <li>2022-06-29 發表</li>
        <li>2022-06-17 完稿</li>
        <li>2022-03-31 起稿</li>
    </ul>
</details>