---
title: 【翻譯練習】Accelerate precision medicine with Microsoft Genomics
date: 2020-02-14
is_modified: false
categories:
- "應用資訊 › 生醫資訊"
tags:
- 文獻
- 基因體學 Genomics
- Azure
- 自翻譯
--- 

最近的任務 Survey 到了 Azure 的 Microsoft Genomics，想說看了都看了，所以閒暇之餘練習翻譯他們[白皮書](https://azure.microsoft.com/mediahandler/files/resourcefiles/accelerate-precision-medicine-with-microsoft-genomics/Accelerate_precision_medicine_with_Microsoft_Genomics.pdf)。
  
<div class="alert danger"> 
<div class="head">糟糕翻譯慎入</div>
有非常明顯的機器翻譯痕跡，還有些部份詞不達意，甚至有些地方還點過度腦補。
<br>
服用時請小心！
</div>

<!--more-->
<br><br>

## 0. Abstract
人類基因體定序的成本已從十年前的數百萬美元大幅下降至一千美元。這使得研究計劃能夠對數十萬人進行定序[^Sequence]，而臨床研究也可以將對患者的基因體進行定序作為他們治療標準流程的一部分。基因體定序的巨大進步還需仰賴大量的資料儲存和計算能力，因為每個基因體[^genomes]都是大約 60 Gb 的壓縮資料，且通常每小時需要耗費約一千個 CPU 來處理。
  
Microsoft Azure 雲端運算服務憑藉著可靠性、安全性、全球資料儲存與計算服務，非常適合用來滿足此需求。此外， Microsoft Azure 上的 Microsoft Genomics service 提供了一種易於用來分析基因體的 Web 服務，其速度比標準分析基因體的管線[^pipeline]快上幾倍。這項服務遵循的是由麻省理工和哈佛大學共同創立的 Broad 研究所所建立關於一致性和準確性的最佳做法[^best-practices]，這也是基因體分析的業界標準[^de-facto]。Microsoft Genomics service 由於其速度、準確性和簡便性，使其在癌症、罕見疾病、人群健康和精密醫學領域有廣泛的應用。

<br><br> 

## 1. Introduction
正常人細胞中的 DNA 由 23 對染色體所組成，加起來總共有 64 億個鹼基對（以及其他一些片段，如線粒體DNA[^mtDNA]）。每個鹼基對使用字母 A、C、T 或 G 表示，用以識別每個位置的特定核苷酸。由於成對的染色體非常相似，因此在排序時通常會將它們排列在一起，從編號 1 的染色體（最大）到 22 號染色體，然後是 X＆Y 性別染色體，依序排列成 32 億個位置[^locations]。由於人類 DNA 的個體差異相對較小（通常為1000個定位中的有1個左右的差別），因此我們通常將待測的人類 DNA 與參考基因體進行比較，該參考基因體是基於第一個人類基因體計劃結果所得的複合基因體。

基因體定序的流程是由一份生物樣本例如血液或唾液...等開始，最後得出一份闡明該樣本與參考基因體的差異及其差異含義的報告結束。這流程通常會分成三個階段：  
1) **初級分析（Primary analysis）**  
    生化檢測樣本並產生原始資料。
    
2) **次級分析（Secondary analysis）**  
    將所產生的原始資料與參考基因體進行比對[^align]，並將與參考基因體不同，或稱作變異位點，標出。

5) **三級分析（Tertiary analysis）**  
    用於偵測變異位點，並添加註釋[^annotations]，以協助變異位點解釋其生物學或臨床上的意義。


初級分析階段是在實驗室中使用 Illumina 與 Thermo-Fisher 等公司的專業定序設備完成的。基因體定序儀會結合生化、光學、電子學和影像處理，在大規模平行處理過程中複製並片段化 DNA 最後讀取鹼基對。辨識 DNA 序列中的鹼基對，即鹼基識別(Base calling)，是困難的，並且也會發生識別錯誤。所以除了識別每個位置的 A、C、T、G 鹼基外，定序儀還會產生一份**質量分數**，用以記錄定序儀在鹼基識別過程中的信心程度。定序人類全部的基因體通常會產生約十億個長度為 100 個字元的 A、C、T、G 字串，即定序片段[^reads]，平均以 30 個副本涵蓋整個基因體[^30x]以實現樣本擴增[^redundancy]。最終，會連同其它的詮釋資料[^metadata]如質量分數，一起產生約 60 GB 的壓縮原始資料以供次級分析進行。

次級分析的第一步是將每個定序片段與參考基因體進行比對，考慮到定序過程中的錯誤、樣本與參考基因體之間的差異以及整個基因體存在許多相似區域等原因，會存在將近 30 億個可能位置，此步驟必須從中找到最可能的位置進行比對。第二步會進行變異位點偵測，它會從每個定序片段與參考基因體的比對結果中找出相異處，並判斷這是屬於定序錯誤還是樣品 DNA 中的真的產生變異。這些變異可以是簡單的單個核苷酸 A、T、C 或 G 的改變所影響產生的變異，稱為單點突變 (Single Nucleotide Variants，SNVs)，也可以是更複雜的，插入與刪除（insertion/deletion) 與基因重組(Rearrangement)
   

三級分析更是更為複雜且多樣。此階段使用了各種各樣的工具和資料庫。根據分析目的，可能會添加有關進化演化保守性(Evolutionary Conservation)、蛋白質結構、藥物反應、疾病風險與基因體相互作用...等資訊。資料庫與工具的選擇在很大程度上取決於分析的目的以及臨床醫生或研究人員的總體方法。
 
無論要做哪種三級分析，都必須先進行次級分析。次級分析中的比對和變異點偵測步驟都非常依賴計算資源，因為它們需要處理大量資料並執行複雜的計算。二次分析的標準工具有用於比對的 Burrows-Wheeler Aligner（BWA）及由麻省理工和哈佛大學共同創立的 Broad 研究所開發用於變異點偵測的 Genome Analysis Toolkit（GATK）。從開始讀取定序儀的原始資料到生成比對完定序片段和變異位點偵測文件，一般版本的工具需要一天才能在 16 核心伺服器上處理完 30 倍的全基因體樣本。
 
<br><br> 

## 2. Microsoft Genomics
由於不斷擴大規模、複雜性與基因體學的安全需求，使得 Microsoft Azure 雲端運算服務成為搬遷的最理想的人選。Microsoft Azure 擁有位於世界各地的資料中心，其儲存與計算能力能滿足存放與分析未來幾年內要定序的數十萬個基因體的需求。此外，Microsoft Azure 有通過全球主要的安全和隱私認證，如：ISO 27001，並制定符合 HIPAA 法規的安全操作標準用以處理個人健康資料。

為使 Microsoft Azure 成為基因體學最佳的雲端運算服務，Microsoft Genomics 開發了一種優化的輔助分析服務，可以在幾小時內處理 30x 基因體，而不需要花費一天或以上的時間來處理。 Microsoft Genomics Service 包含一個經過優化的高效能引擎，可以讀取較大的基因體資料文件，並使用多核心進行處理、排序並對結果進行過濾，最後再將過濾結果寫回。該引擎協調 BWA 與 GATK HaplotypeCaller 的變異位點偵測操作以實現最大吞吐量。此外，它還整合其他一些標準基因體管線中較為簡易的部份，如：重複標記（Duplicate Marking）、定序質量分數重新校正（Base Quality Score Recalibration，BQSR）與索引（indexing）。從單一樣本的原始資料讀取、定序片段比對到變異點偵測，該引擎在一台多核心伺服器上可以在幾個小時以內完成。
 
Microsoft Genomics service controller 管理雲端運算服務中分佈在機器集區（pools of machines）裡的一批基因體的處理。它維護傳入請求的佇列，將它們分發到運行基因體引擎的伺服器，並監視其性能與進度，最後評估結果。它確保服務在安全的 Web Service API 的後方可以可靠且安全地大規模運行。客戶端無需處理複雜的軟硬體維護與更新，並且可以快速且有效地執行精確的 GATK 標準分析流程。其分析結果可以輕鬆連接到三級分析和與機器學習服務，如：Azure 上的 Microsoft R Server。
<br>

### 2.1 Client Architecture
Microsoft Genomics 客戶端（msgen）是 Python 實做的前端用以連接 web service。它可以像標準的 Python 函式庫一樣，在 Windows 或 Linux 直接 python pip 函式庫管理器（`pip install msgen`）。 

對於要處理的每個基因體樣本，你需要建立個配置文件配置包含用於下載資料、執行 Microsoft Genomics 管線以及上傳結果所需的所有參數：

- Microsoft Genomics 訂閱金鑰
- 執行過程與其參數
- 提供一對 FASTQ 文件、一對壓縮 FASTQ 文件或一對 BAM 格式的輸入檔案在 Azure Storage 中的路徑與相對應 Storage 帳密。
- 輸出文件寫出到 Azure Storage 時的位置的路徑與相對應 Storage 帳密。

之後，你就可以調用 msgen 客戶端來啟動處理流程，並監視進度，直到處理完成。最終比對完定序片段的 BAM 檔案與變異點偵測後的 VCF.GZ 檔案，會被放置在指定輸出 Azure Storage 中。 使用者可以輕鬆整合到現有工作流程中。


<br>

### 2.2 Service Architecture
Microsoft Genomics service 在 Azure 中負責處理基因體資料。整個系統架構由幾層組成，如圖 1 所示：
 
<center> <img src="https://i.imgur.com/qgJIlqx.png" alt="Architecture"></center>
 
1. **服務控制層，service control layer**  
    用於接收 API 請求，安排工作佇列以及管理 Azure Batch 中跨機器集區的執行。
    
2. **SNAP執行引擎，SNAP execution engine**  
    規劃單一樣本於單一機器上的 IO 與計算的所有流程。

3. **GATK pipeline 前/後處理步驟的優化**  
    例如：重複標記、定序質量分數重新校正、索引與 BAM 壓縮。
    
4. **標準的 BWA-MEM 和 HaplotypeCaller算法**  
    用於比對和變異點偵測，只需進行最小的修改即可在保持兼容性的同時進行優化。
    
5. **優化的 AVX2 程式**  
    針對計算密集型演算法（例如：Smith-Waterman 序列比對）和用於單倍體評估的成對隱藏馬可夫模型（Pair Hidden Markov Model）。
<br>

#### 2.1.1  Service Controller
Service Controller 是個具有可執行後端服務的分佈式 C＃ 網路應用程式。前端接受來自 Azure API 管理的客戶端請求，並將它放置在工作佇列中。Azure Web Job application 會為佇列中每個項目進行排程與監控 Azure Batch 任務。當 Azure Batch 執行任務時，service executable 下載參考資料和輸入文件，並執行 SNAP 引擎以進行比對和變異點偵測，再將結果寫入文件流回到 Azure Storage，最後報告完成情況。整個應用遵循 Azure 在安全性、合規性、審核和監視的方面最佳實踐流程，例如：所有客戶端和應用程式機密均保存在 Azure Key Vault 中以提供保護。


#### 2.1.2  SNAP Engine
SNAP Engine 是基於 Microsoft Research 與 UC Berkeley AMPLab 合作開發的的高性能 SNAP short read aligner。但此版本不使用 SNAP alignment 演算法，而是使用更廣泛的 BWA MEM 比對器來取代。它擁有一個高性能的非同步輸入/輸出子系統，可以有效地處理大量的基因體數據。它還包括一個高效的系統，用於排程跨多核心的計算密集型工作並收集結果。為了盡量減少額外的硬碟讀取次數，通過該框架的總體資料流只在兩個步驟間進讀取和寫入數據，而不是像標準 BWA / GATK 管線中所要求的半打或以上的讀寫次數。

<center><img src="https://i.imgur.com/fq0zMBR.png" alt=" Standard BWA/GATK Pipeline"></center>
<br>
  
第一階段會同時跨所有核心批次大量比對定序片段，並在進行前處理時不依賴任何的排序，例如：收集用來做定序質量分數重新校正的統計資料。然後，將定序片段依序批次寫入中間文件。第二階段將所有批次合併到一個定序片段的 single fully-sorted stream 中，並完成前/後處理步驟：應用 BQSR 統計信息，標記重複項，構建 BAM 索引以及壓縮 BAM 文件。在編寫 BAM 文件的同時，此階段還協調了幾個 GATK 流程來進行變異位點偵測，識別潛在變異位點附近的激活區域，並僅將那些定序片段直接輸送到 HaplotypeCaller 中以提高效率。由於排序是 IO 密集型的，而變異位點偵測是 CPU 密集型的，因此可以有效地平衡機器資源的整體使用

<center> <img src="https://i.imgur.com/BFgcc8q.png" alt=" Microsoft Genomics Pipeline"></center>


#### 2.1.3  BWA-MEM & GATK HaplotypeCaller
Microsoft Genomics 使用 BWA 的 opensource 版本和 Broad 研究所發布的 GATK 許可版本並對其進行了優化和加速，以在 Microsoft Azure 雲端服務上運行。BWA MEM 和 GATK HaplotypeCaller 程式碼的大部分保持不變，以保持與標準 pipelines 的兼容性。 只需進行少量更改即可將它們融合到整個管線中：BWA MEM 被製成一個函式庫、HaplotypeCaller 被修改為接受預先計算好的激活區域作為標準input中。此外，重複標記、來自 Picard校正演算法和 GATK 已經改由 C ++ 實做，且被應用在當資料流通過引擎時去最大程度地減少不必要的 I/O。

這些程式中的大部分時間都花在了兩個低階層計算的 Kernel：SmithWaterman 與 PairHMM 上。這些已使用 Intel AVX2 向量指令進行了高度優化，比標準版本運行速度明顯提高。該初始版本計劃支持 GATK 3.5，並會根據客戶需求隨時間增加對其他版本的支持。管線可以選擇生成 gVCF 文件，該文件可以在多個樣本中合併以進行聯合基因分型。
<br>

###  2.3 Performance
通過彈性分配 Microsoft Azure 雲端運算服務中的資源， Microsoft Genomics service 的整體體系結構可以擴展至平行處理數百或數千個基因體。每個基因體分別在單個高容量虛擬機上執行，以最大化吞吐量並最小化每次執行時的通訊和存儲開銷。前/後處理步驟盡可能平行運行，以減少 end-to-end 的處理時間。

標準的 GATK 分析流程管線中通常按順序執行每個步驟。每個步驟都必須讀入上一步生成的文件，因此必須等待它完成。如圖 2 所示，若將工作依基因體的不同區域分進行劃分，則在一個步驟中就會存在平行性可能（例如：分別在每個染色體上運行 MarkDuplicates）。
  
Microsoft Genomics 管線則完全不同。它僅對資料進行兩次讀寫，將每次讀寫中的許多不同處理步驟組合在一起，並且平行處理許多區域，以最大效率，如 圖3所示。
  
<br>

###  2.3 Concordance
陣亡！ 這部份先欠著，反正內容大抵上就是說準確度不輸標準流程、但速度快很多之類的。

<center> <img src="https://imgur.com/fvnLx0K.png" alt="举白旗的兔子"></center>
<center class="imgtext">舉白旗，陣亡！（圖片來源: <a href="http://616pic.com/sucai/1yqiqp69z.html" class="imgtext">图精灵</a>）</center>

<br><br>

## 小結
真的好難翻，明明看得懂但翻成中文怎麼也不順 Orz  
我翻的最順的部份搞不好是他們自誇的部份，其他涉及生物資訊的部份怎麼翻怎麼彆扭。

翻到後面都忘了寫註解了，而且整篇排版超級亂的... orz

<br><br>

## 參考資料 
1. 活躍星系核 (2012-09-21)。[全球化視野下的基因體學](https://pansci.asia/archives/27710)。檢自 PanSci 泛科學 (2020-01-30)。 
2. 黃彥華 (2019-10-01)。[新世代定序技術](https://scitechvista.nat.gov.tw/c/sTfS.htm)。檢自 科技大觀園 (2020-01-30)。 
3. 基因叔叔(2019-06-08)。[什麼是全基因定序(Whole genome sequence ,WGS)](https://unclegene6666.pixnet.net/blog/post/352053530-%E4%BB%80%E9%BA%BC%E6%98%AF%E5%85%A8%E5%9F%BA%E5%9B%A0%E5%AE%9A%E5%BA%8F%28whole-genome-sequence-%2Cwgs%29)。檢自 基因叔叔：科普、期刊導讀(Uncle Gene) - 痞客邦 (2020-01-30)。
4. 協同撰寫。[De facto](https://zh.wikipedia.org/wiki/De_facto)。檢自 維基百科 (2020-01-30)。
5. 協同撰寫。[粒線體DNA](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%B2%92%E4%BD%93DNA)。檢自 維基百科 (2020-01-30)。
6. [人類基因組計劃](https://zh.wikipedia.org/wiki/%E4%BA%BA%E7%B1%BB%E5%9F%BA%E5%9B%A0%E7%BB%84%E8%AE%A1%E5%88%92)。檢自 維基百科 (2020-01-30)。
7. [NGS 次世代定序常用名詞](https://www.zgenebio.com.tw/ngs-274251999020195234502420724120299922151735422.html)。檢自 力鈞生物科技有限公司 (2020-02-10)。
8. [测序技术的个体化医学检测](http://www.hbccl.cn/HbcclUpload/201504/08/201504081113340408.pdf)。檢自 湖北临床检验中心 (2020-01-30)。
9. 微笑如酒 (2019-01-13)。[测序了，然后呢（二）｜基因功能注释](http://www.360doc.com/content/19/0131/11/19913717_812332751.shtml)。檢自 微笑如酒-個人圖書館 (2020-01-30)。 
10. 圖爾思生物科技 (2016-05-30)。[重定序(Re-sequencing)常見專有名詞，你知道幾個呢？](http://toolsbiotech.blog.fc2.com/blog-entry-19.html)。檢自 次世代定序知識櫥窗 (2020-01-30)。
11. TCoo_阿西 (2019-12-21)。 [Reads在生物基因里是何物？可否形象解释一下？或者给个专业的翻译......](https://zhidao.baidu.com/question/381471696.html)。檢自 百度知道 (2020-01-30)。
12. 勤奋的du芝痲 (2017-11-22)。[基因学中reads指的是什么意思](https://zhidao.baidu.com/question/1434647735364863619.html)。檢自 百度知道 (2020-01-30)。
13. [次世代定序（NEXT GENERATION SEQUENCING，NGS）](http://ai-ngs.com/NGS.asp)。檢自 AI-NGS.com (2020-01-30)。
14. [metadata - 詮釋資料](hhttp://terms.naer.edu.tw/detail/1679224/?index=6)。檢自 國家教育研究院 (2020-01-30)。
15. 張益祥/有勁生物科技 (2019-05-17)。[變異位點偵測─left alignment](https://yourgene.pixnet.net/blog/post/119576346-%E8%AE%8A%E7%95%B0%E4%BD%8D%E9%BB%9E%E5%81%B5%E6%B8%AC%E2%94%80left-alignment)。檢自 有勁的基因資訊-痞客邦 (2020-01-30)。
16. 基因叔叔 (2017-04-18)。[麼是單核苷酸多型性 (Single Nucleotide Polymorphism，簡稱SNP，讀作/snip/)?](https://unclegene6666.pixnet.net/blog/post/308333779)什。檢自 基因叔叔：科普、期刊導讀(Uncle Gene) - 痞客邦 (2020-01-30)。
17. 協同撰寫。[保守序列](https://zh.wikipedia.org/wiki/%E4%BF%9D%E5%AE%88%E5%BA%8F%E5%88%97)。檢自 維基百科  (2020-01-30)。
18. starsyi (2016-05-25)。[变异检测（BWA+ SAMtools+ picard+ GATK）](http://starsyi.github.io/2016/05/25/%E5%8F%98%E5%BC%82%E6%A3%80%E6%B5%8B%EF%BC%88BWA-SAMtools-picard-GATK%EF%BC%89/)。檢自 寂寞先生 (2020-01-30)。
19. [SNAP Scalable Nucleotide Alignment Program](http://snap.cs.berkeley.edu/#publications)。檢自 UC Berkeley AMP Lab (2020-02-10)。


<br><br>

## 註解

[^Sequence]: **DNA定序（DNA sequencing）**<br> 在網路上常見的翻譯有兩種：DNA 定序或 DNA 測序，不過因為我一開始接觸的時候周圍的人都是使用 DNA 定序，所以我也習慣用這個。<br> 根據[科普文章](https://scitechvista.nat.gov.tw/c/sTfS.htm)的解釋，可將用來「讀取」 DNA 片段序列的實驗，統稱為「定序」。 

[^genomes]: **基因體（genomes）**<br> 網路上最常見的翻譯有兩種分別是基因組與基因體，選擇使用基因體單純是我個人喜好。

[^pipeline]: **管線（pipeline）**<br> 其實我不喜歡將 pipeline 翻譯，口語上我都直接使用 pipeline。不過 MS 的[文件](https://docs.microsoft.com/zh-tw/azure/genomics/overview-what-is-genomics)中使用了`最佳做法管線`的字眼，所以我這邊也將它翻為管線。<br> ...雖然我覺得文件中有些句子像是機器硬翻出來的，不過我也沒資格說別人就是了 Orz

[^best-practices]: **最佳作法（best practices）**<br> best practices 這個字的翻譯我找到[有人](http://mypaper.pchome.com.tw/angelo_chen/post/1283275875)譯為「典範實務」，不過感覺這比較偏向管理的上的字眼。<br> 另外文中提到的翻譯，如：「最佳作業流程」、「最佳實務」、「典範實務」、「典範經驗」，感覺都不太適合。最後使用了[文件](https://docs.microsoft.com/zh-tw/azure/genomics/overview-what-is-genomics)中使用的**最佳做法**這個翻譯。

[^de-facto]: **業界標準（de facto standard）**<br> de facto 為拉丁語法學詞彙（發音：[deː ˈfaktoː]，/diː ˈfæktoʊ/）意思指「實務上」或者「執行上」，而法律上並未宣告。<br> 根據[維基百科](https://zh.wikipedia.org/wiki/De_facto)， de facto standard 可譯為業界標準或非官方標準，是指一套技術上規格或標準，而該套規格或標準屬於主流且每個人都習慣將它視為法定標準般遵守。

[^mtDNA]: **粒線體 DNA（mitochondrial DNA，mtDNA）**<br> 專有名詞。指一些位於粒線體內的 DNA，與一般位於細胞核內的 DNA 有不同的演化起源，我也不太懂，還是去看[維基百科](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%B2%92%E4%BD%93DNA)吧。

[^locations]: **位置？ locations**<br> 我不太確定這邊是不是用位置，還是它本身有對應的專有名詞？

[^align]: **比對（[v] align / [n] alignment）**<br> 這個字有點討厭，按照字面意思是校準、對齊的意思，但翻成中文後放進上下文，卻怎麼看怎麼彆扭，偏偏需多生物資訊的相關文章對這個字都沒翻譯，後來在某份[文件](http://www.hbccl.cn/HbcclUpload/201504/08/201504081113340408.pdf)看到它有列一些專有名詞的對照表中，有可以使用的翻譯，就直接拿來用了。

[^annotations]: **註釋 annotations**<br> 顧名思義，就是寫註解，有分成結構性註釋與功能性註釋。結構性註釋是標注外顯子、內含子等資訊。功能性註釋則是說明美條基因編碼什麼蛋白質、有什麼功能。
 
[^reads]: **定序片段 reads**<br> reads 並不是基因基因體中的組成，是一小段短的定序片段，為高通量定序儀產生的定序數據。不過這個自很多時候都沒有翻譯就直接使用了。

[^30x]: **平均以 30 個副本涵蓋整個基因體**<br> 猜這邊想表達的應該是 30x 的定序深度。

[^redundancy]: **樣本擴增** 與 **redundancy**<br> 其實這邊原文是 **covering the genome with an average of 30 copies for redundancy.**，但翻成**實現冗餘**，又好像哪裡怪怪，最後參考了[次世代定序的操作流程](http://ai-ngs.com/NGS.asp)決定翻成**樣本擴增**好了。

[^metadata]: **詮釋資料 metadata**<br> 在國家教育研究院的[雙語詞彙](http://terms.naer.edu.tw/detail/1679224/?index=6)將這個自翻譯成詮釋資料，不過我口語從來沒說過這個詞 XDDDD
