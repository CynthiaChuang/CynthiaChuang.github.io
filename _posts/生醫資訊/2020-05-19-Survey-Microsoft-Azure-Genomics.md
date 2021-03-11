---
title: 【Survey】 Microsoft Azure Genomics
date: 2020-05-19
is_modified: false
categories:
- 生醫資訊
tags:
- Genomics
- Azure
--- 
 
年初的工作要求，小組就四個基因雲端平台： [AWS 基因體雲端運算](https://aws.amazon.com/tw/health/genomics/) 、 [Microsoft Genomics](https://azure.microsoft.com/zh-tw/services/genomics/) 、 [Cloud Life Sciences BETA 版](https://cloud.google.com/life-sciences?hl=zh-tw) 與 [阿里雲 基因數據分析管理](https://cn.aliyun.com/solution/healthcare/Genomics) 分頭進行 survey。
  
我這邊負責 survey [Microsoft Genomics](https://azure.microsoft.com/zh-tw/services/genomics/)。

<!--more-->

<br><br> 

## 基因定序流程

<center> <img src="https://i.imgur.com/SCrZi7c.jpg" alt="DNA"></center>
<center class="imgtext">DNA（圖片來源: <a href="https://pixabay.com/" class="imgtext">Pixabay</a>）</center>

<br> 
在開始之前，我們先提提基因定序的流程。一般來說，從採檢到生物樣本到最終提出變異報告，可分成三個階段。

這裡採用 MS 在 [Genomics 白皮書](https://azure.microsoft.com/mediahandler/files/resourcefiles/accelerate-precision-medicine-with-microsoft-genomics/Accelerate_precision_medicine_with_Microsoft_Genomics.pdf)中的分類定義：

1) **初級分析（Primary analysis）**  
這個階段會先檢測採樣的生物樣本，如血液或唾液...等，經由專業儀器分析後產生原始資料。  

此階段多在生物實驗室中進行，研究員會使用定序設備對 DNA 序列進行鹼基識別，並同時記錄定序儀在鹼基識別過程中的信心程度。最終定序儀會產生 30 倍定序深度的定序片段資料，連同其它的詮釋資料，如剛剛所提到質量信心分數，一起產生約 60 GB 的壓縮原始資料。
 
2) **次級分析（Secondary analysis）**  
此階段會將定序儀產生的原始資料與參考基因體進行比對，並將樣本與參考基因體不同處，及變異位點，一一標出。此階段可以在細分成兩個步驟：
1. **找到比對位置**   
    每個定序片段必須從 30 億個可能位置中找的原本的位置，才能與參考基因體進行比對。
    
2. **變異位點偵測**  
    它會從定序片段與參考基因體的比對結果中找出相異處，並確定它們是定序錯誤還是樣本中的真的產生變異。變異從點突變到結構變異都有可能。
 
3) **三級分析（Tertiary analysis）**  
此階段會為變異位點添加註釋，以協助解釋其生物學或臨床上的意義。最終產生報告以闡明該樣本與參考基因體的差異及其差異含義。
    
這階段取決於分析的目的以及臨床醫生或研究人員的總體方法，此階段使用了各種各樣的工具和資料庫。
    
<center> <img src="https://i.imgur.com/Z4o4ubJ.png" alt="Analysis Pipeline"></center>
<center class="imgtext">Analysis Pipeline（圖片來源: <a href="https://academic.oup.com/clinchem/article/61/1/124/5611448" class="imgtext">Bioinformatics for Clinical Next Generation Sequencing</a>）</center>
<br><br>

不過，這分類不是絕對的，比較像業界的潛規則（de facto standard）。就有曾經看過把 Annotation 列在次級分析的步驟中，例如：MS Azure 就在自家提供的 Databricks [文件中](https://docs.microsoft.com/zh-tw/azure/databricks/applications/genomics/)描述 Genomics 的操作步驟時，就將它給列在次級分析。

    
<center> <img src="https://imgur.com/IC5TYvY.png" alt="Analysis Pipeline"></center>
<center class="imgtext">MS 其他服務就把它分在次級分析（圖片來源: <a href="https://docs.microsoft.com/zh-tw/azure/databricks/applications/genomics/" class="imgtext">MS Azure Databricks </a>）</center>

<br><br>


## Microsoft Genomics

回頭來看這次的主角 - Microsoft Genomics，按照官網的簡介稍微回顧下： 

### 簡介
 - **目標客群**  
     研究員、資料科學家與臨床醫生等工作者。
     
 - **提供服務**  
     1. 雲端 Burrows-Wheeler Aligner（BWA）以及基因體分析工具（Genome Analysis Toolkit，GATK）。
     2. EB 規模工作負載的資料中心與 HPC 級的 CPU，提供分析時所需的儲存與計算能力。


 - **使用者付費**  
     可以依需求擴充或縮小規模，只需支付使用到的部分以降低成本。
     
 - **資料管理**  
   1. 資料的操作與保管使用有通過 ISO 認證並遵循 HIPAA 法規 ，確保患者記錄和健康資訊的隱私。
   2. [A Practical Guide to Designing Secure Health Solutions using Microsoft Azure](https://gallery.technet.microsoft.com/Azure-A-Practical-Guide-to-5ebdc8bd)。(:cold_sweat:...誰看完了分享一下吧！記錄一下有這東西就好，需要實在回頭看。

<br>

<center> <img src="https://i.imgur.com/EsOa8j3.png" alt="Microsoft Genomics"></center>
<center class="imgtext">Microsoft Genomics（圖片來源: <a href="https://azure.microsoft.com/zh-tw/services/genomics/" class="imgtext">Microsoft Azure 截圖</a>）</center>

<br><br>


**簡單來說**  
MS Genomics 提供儲存與運算空間，讓使用者做無需處理軟硬體維護與更新，就可以執行<mark>次級分析</mark>： GATK 標準分析流程。其他的附加價值是突顯在儲存的可靠性、方便性、計算速度與計價方式上。

上一張他家合作夥伴 [Persistent](https://www.persistent.com/partner-ecosystem/) 的圖，這張圖很的詮釋了 MS Genomics 所提供的服務範圍。下圖中橘色的部份是 Persistent 所提供的服務，可以看出來是針對三級分析與二級分析的輸出結果進行衡量；而鐵灰色的部份則是所提供的服務次級分析的 GATK 最佳 pipeline。

<center> <img src="https://i.imgur.com/aVzl6bv.jpg" alt="Persistent Analysis Pipeline" ></center>
<center class="imgtext"> 灰色底的為 MS Genomics 所提供的服務，橘色底則為 Persistent 所提供的服務  （圖片來源: <a href="https://www.persistent.com/partner-ecosystem/microsoft/genomics-and-azure/" class="imgtext">Persistent Systems</a>）</center>
<br>

// 在看看要塞哪裡
<center> <img src="https://i.imgur.com/jjZ8wwi.png" alt="Persistent Analysis Pipeline" ></center>
<center class="imgtext">Microsoft Genomics Service（圖片來源: <a href="https://www.microsoft.com/en-us/genomics/" class="imgtext">Microsoft Genomics</a>）</center>
<br>


### 系統架構

系統架構分成兩部份來看，先看 Microsoft Genomics 內部的架構，接下來再來看整體的工作流程。


#### 內部架構

Microsoft Genomics 系統由上而下總共可分為三個部份，分別是：Service Controller、SNAP Engine、SNAP Engine 與次級分析的 pipline - BWA-MEM & GATK HaplotypeCaller。

<center> <img src="https://i.imgur.com/qgJIlqx.png" alt="Architecture"></center>
<center class="imgtext">Microsoft Genomics Architecture（圖片來源: <a href="https://azure.microsoft.com/mediahandler/files/resourcefiles/accelerate-precision-medicine-with-microsoft-genomics/Accelerate_precision_medicine_with_Microsoft_Genomics.pdf" class="imgtext">白皮書</a>）</center>
<br>


**Service Controller**  
此區塊是用來管理分佈在機器集區（pools of machines）裡的一批基因體的處理，主要用接收 API 請求、安排工作佇列以及管理 Azure Batch 中跨機器集區的執行。

前端會接受來自 Azure API 的請求，並將它放置在工作佇列中。而 Web Job Application 則會為佇列中每個項目進行排程，將它們分發到運行基因體引擎的伺服器，並監視其性能與進度，最後評估結果。

當 Azure Batch 執行任務時，service executable 下載參考資料和輸入文件，並執行 SNAP 引擎，再將結果寫入文件流回到 Azure Storage，最後報告完成情況。

最後的分析結果接著向下做三級分析和或是其他機器學習。


<br>

**SNAP Engine**  
SNAP Engine 這是 Microsoft Research 與 UC Berkeley AMPLab 一起合作開發的 [SNAP short read aligner](http://snap.cs.berkeley.edu/#publications)。不過此版本不使用 SNAP alignment 演算法，而是改採 BWA MEM。主要用來規劃單一樣本於單一機器上的 IO 與所有計算的流程。。

~~(...恩，所以把 SNAP 核心演算法弄掉了，為啥還叫 SNAP Engine？)~~

<center><img src="https://i.imgur.com/fq0zMBR.png" alt=" Standard BWA/GATK Pipeline"></center>
<center class="imgtext">Standard BWA/GATK Pipeline（圖片來源: <a href="https://azure.microsoft.com/mediahandler/files/resourcefiles/accelerate-precision-medicine-with-microsoft-genomics/Accelerate_precision_medicine_with_Microsoft_Genomics.pdf" class="imgtext">白皮書</a>）</center>
<br>

這是一張 GATK 的標準流程，每個綠色區塊之間都進行了一次讀寫動作。而 SNAP Engine 宣稱它具有一個非同步輸入/輸出子系統，可有效處理大量基因體資料，並排程跨多核心的計算密集型工作並收集結果。

為了盡量減少硬碟讀取次數，他們改良了 pipeline 的流程只進行兩次的讀寫動作，減少了標準 BWA/GATK pipline 中的讀寫次數。

<center> <img src="https://i.imgur.com/BFgcc8q.png" alt=" Microsoft Genomics Pipeline"></center>
<center class="imgtext">Microsoft Genomics Pipelin（圖片來源: <a href="https://azure.microsoft.com/mediahandler/files/resourcefiles/accelerate-precision-medicine-with-microsoft-genomics/Accelerate_precision_medicine_with_Microsoft_Genomics.pdf" class="imgtext">白皮書</a>）</center>
<br>

可以看到改良後的 pipeline 分成兩個階段，在第一階段是跨所有核心進行大量批次定序片段比對，並進行前處理時不依賴任何的排序，例如：收集用來做定序質量分數重新校正的統計資料。然後，將定序片段依序批次寫入中間文件。

第二階段將所有 batches 合併到一個定序片段的 single fully-sorted stream 中，並完成預處理和後處理步驟：應用 BQSR 統計信息、標記重複項、構建 BAM 索引以及壓縮 BAM 文件。

在編寫 BAM 文件的同時，此階段還協調了幾個 GATK 流程來進行變異點偵測，識別潛在變異點附近的激活區域，並僅將那些定序片段直接輸送到 HaplotypeCaller 中以提高效率。由於排序是 IO 密集型的，而變異點偵測是 CPU 密集型的，因此可以有效地平衡機器資源的整體使用。

標準的 GATK 分析流程管線中通常按順序執行每個步驟。每個步驟都必須讀入上一步生成的文件，因此必須等待它完成。如圖 2 所示，若將工作依基因體的不同區域分進行劃分，則在一個步驟中就會存在平行性可能（例如：分別在每個染色體上運行 MarkDuplicates）。

根據微軟的[合作夥伴表示](http://info.microsoft.com/rs/157-GQE-382/images/EN-CNTNT-ebook-PartneringToAdvanceClinicalGenomics.pdf)，整套次級分析的時間由先的 28 小時降至 4 小時，僅需原先時間的 1/7 。
 
<br>

**BWA-MEM & GATK HaplotypeCaller**   

先提一下 HaplotypeCaller 的核心操作就是四步：

1. 尋找激活區域，就是和參考基因體不同部分較多的區域
2. 通過對該區域進行局部重組裝，確定單倍型（haplotypes）。就是這一步可以省去indel realignment
3. 在給定的read數據下，計算單倍型的可能性。
4. 分配樣本的基因型

在這 BWAMEM 和 GATK HaplotypeCaller 程式碼的大部分保持不變，以保持與標準 pipelines 的兼容性。僅進行少量修改預先計算的激活區域通過 pipelines 輸入到標準 input 中，也就是上述提到的第一步驟，這步驟沒有相依性可以拆開來做。


<br>

#### 整體的工作流程
<center> <img src="https://i.imgur.com/Vtv8sER.png" alt="Microsoft Azure workflow"></center>
<center class="imgtext">Microsoft Azure workflow（圖片來源: <a href="https://github.com/microsoft/CromwellOnAzure" class="imgtext">
CromwellOnAzure</a>）</center>
<br>

由找到的流程圖看來，他們後面用的 Cromwell，不過是有經過客製化，稱作[Cromwell on Azure](https://github.com/microsoft/CromwellOnAzure)，搭配 TES 最回後端來調用 Azure Batch 。若是對回服務內部的架構來看，被我暱稱飛天豬+TES的這部份，會對照回 Service Controller。

<div class="alert warning">
越寫越不確定 SNAP Engine， 是對照的途中的那一塊。

原本推論，是 Trigger Service 的部份，但後來有覺得的是 compute environment 上的 VM ，因為文件中對它描述的規劃單一樣本於單一機器上流程。
</div>

至於 WDL 則是下一張會提到的 msgen。


<br> 

### 執行 Microsoft Genomics 服務

其實這邊有如果需要以直接看 MS Genomics 的[官方文件](https://docs.microsoft.com/zh-tw/azure/genomics/quickstart-run-genomics-workflow-portal)。若是想要看圖文手把手教學，可以看看這篇《[次世代定序二次分析 Microsoft Azure Genomics](https://medium.com/@102316115/%E6%AC%A1%E4%B8%96%E4%BB%A3%E5%AE%9A%E5%BA%8F%E4%BA%8C%E6%AC%A1%E5%88%86%E6%9E%90microsoft-azure-genomics-77f2cc4994b6)》。
<br>

<br> 不過這邊還是留一下記錄：

#### 事前準備
1. Azure 帳戶 ~~(廢話)~~，並準備 **Azure Blob 儲存體** 帳戶與 **Microsoft Genomics** 帳戶。
2. 準備基因資料 FASTQ 或是 BAM 格式皆可。
3. 安裝 pip 與 [Python 2.7.12+](https://www.python.org/downloads/release/python-2714/)。  

    <div class="alert danger">
    <div class="head">注意</div>
    是安裝 Python 2 不是 Python 3，兩者不相容。     
    </div>

<br>

#### 執行步驟
1. 設定：
    1. 若沒有 Microsoft Genomics 帳戶請[參考教學文件](https://docs.microsoft.com/zh-tw/azure/genomics/quickstart-run-genomics-workflow-portal#set-up-create-a-microsoft-genomics-account-in-the-azure-portal)。
    2. 安裝 [Microsoft Genomics 命令列介面](https://github.com/MicrosoftGenomics/msgen)。
    ```shell
    $ pip install --upgrade --no-deps msgen
    $ pip install msgen
    ```

2. 上傳輸入檔案：
    1. 若沒有  Microsoft Azure 儲存體帳戶請[參考教學文件](https://docs.microsoft.com/zh-tw/azure/genomics/quickstart-run-genomics-workflow-portal#create-a-microsoft-azure-storage-account)。
    2. 將準備好的基因配對 (fastq 或 bam 檔案)上傳到儲存體。這邊有公開的範例資料可用： 
        - https://msgensampledata.blob.core.windows.net/small/chr21_1.fq.gz
        - https://msgensampledata.blob.core.windows.net/small/chr21_2.fq.gz
     
3. 執行：
    1. 設定 `config.txt` 檔案，主要是必須包含：訂用帳戶金鑰、儲存體帳戶名稱、輸入和輸出的金鑰與容器名稱。
    2. 最後就可以透過 Microsoft Genomics 命令列介面提交工作流程。
        ```shell
        $ msgen submit -f [full path to your config file] -b1 [name of your first paired end read] -b2 [name of your second paired end read]
        ```
        工作狀態的檢視
        ```shell
        $ msgen list -f c:\temp\config.txt
        ```

<br>

#### 注意與限制
1. [Microsoft Genomics 命令列介面](https://github.com/MicrosoftGenomics/msgen)看來是 MS 包裝好 WDL，但根據說明能改動的似乎不多：
    1. 可將 process_name 參數設定為 gatk4 以改成，以執行 GATK4。
    2. 新增 emit_ref_confidence 參數設定為 gvcf，以輸出 gvcf。
2. 無法提交<mark>多個個體</mark>的 FASTQ 或 BAM 檔案，所以若要分析多個個體資料只能分別下指令。

<br>不過跟同事討論了一下，MS 這套 WDL 的設計應該是先前 Survey 的資訊一樣，是為了研究員或臨床醫生這些非資訊背景人員所設計的，所以設計的挺好上手的，但代價就是可以修改的地方不多。

看起來整套的核心就是，Controller 的 Batch 監控與 Engine 的用跨多核心的計算密集型工作排程並結果回收，其實這兩區塊要刻應該也是可以，但工會非常的大。這邊應該有辦法可以不借助 msgen 自己撰寫 WDL 才對(不負責任猜測。 

<br>
 
## 使用案例
這邊就不寫了，直接看這份《[Partnering to Advance Clinical Genomics](http://info.microsoft.com/rs/157-GQE-382/images/EN-CNTNT-ebook-PartneringToAdvanceClinicalGenomics.pdf)》 。

就目前看來最多人使用的 AWS > Azure > google。


<br><br> 

## 參考資料 
1. 李建興 (2018-03-01)。[基因定序也要上雲，微軟發表基於Azure的Microsoft Genomics](https://www.ithome.com.tw/news/121565) 。檢自 iThome (2020-02-10)。
2. [Genomics - Azure Databricks](https://docs.microsoft.com/zh-tw/azure/databricks/applications/genomics/)。檢自 Microsoft Docs (2020-02-10)。
3. [Microsoft Genomics 支援基因組定序及研究深入解析](https://azure.microsoft.com/zh-tw/services/genomics/)。檢自 Microsoft Azure (2020-02-10)。
4. [Accelerate precision medicine with Microsoft Genomics](https://azure.microsoft.com/mediahandler/files/resourcefiles/accelerate-precision-medicine-with-microsoft-genomics/Accelerate_precision_medicine_with_Microsoft_Genomics.pdf)。檢自 Microsoft Genomics (2020-02-10)。
5. [Microsoft Genomics And Azure - Partner EcoSystem](https://www.persistent.com/partner-ecosystem/microsoft/genomics-and-azure/)。檢自 Persistent Systems (2020-02-10)。
6. [microsoft/CromwellOnAzure](https://github.com/microsoft/CromwellOnAzure)。檢自 Github  (2020-02-10)。
7. [amplab/snap](https://github.com/amplab/snap)。檢自 Github  (2020-02-10)。
8. [SNAP Scalable Nucleotide Alignment Program](http://snap.cs.berkeley.edu/#publications)。檢自 UC Berkeley AMP Lab (2020-02-10)。
9. [Partnering to Advance Clinical Genomics](http://info.microsoft.com/rs/157-GQE-382/images/EN-CNTNT-ebook-PartneringToAdvanceClinicalGenomics.pdf)。檢自 Microsoft Genomics (2020-02-10)。
10. hoptop(2017-05-03)。 [GATK之HaplotypeCaller](http://www.biotrainee.com/thread-1417-1-1.html)。檢自 生信技能树 (2020-02-10)。
11. [Scalable and Secure Genomic Data Analysis on Azure](https://www.microsoft.com/en-us/genomics/)。檢自 Microsoft Genomics (2020-02-10)。
12. Chaitanya Bangur (2019-10-11)。[Powering genomics tools and analysis with cloud computing](https://cloudblogs.microsoft.com/industry-blog/health/2019/10/11/powering-genomics-tools-and-analysis-with-cloud-computing/) 。檢自 Microsoft Industry Blogs (2020-02-10)。
13. Todd Bergeson (2018-04-17)。[Leveraging the power of cloud computing to fuel genomic research](https://cloudblogs.microsoft.com/industry-blog/health/2018/04/17/leveraging-the-power-of-cloud-computing-to-fuel-genomic-research/) 。檢自 Microsoft Industry Blogs (2020-02-10)。 
14. [A Practical Guide to DESIGNING SECURE HEALTH SOLUTIONS Using Microsoft Azure](https://aka.ms/azureindustrysecurity)。檢自 Microsoft Genomics (2020-02-10)。
15. Amazon Web Services (2018-07-02)。[Accelerating Analytics for the Future of Genomics](https://www.slideshare.net/AmazonWebServices/accelerating-analytics-for-the-future-of-genomics)。檢自 SlideShare (2020-02-10)。
16. Brannann (2019-07-29)。[次世代定序二次分析Microsoft Azure Genomics](https://medium.com/@102316115/%E6%AC%A1%E4%B8%96%E4%BB%A3%E5%AE%9A%E5%BA%8F%E4%BA%8C%E6%AC%A1%E5%88%86%E6%9E%90microsoft-azure-genomics-77f2cc4994b6)。檢自 Brannann - Medium (2020-02-10)。
17. Jenn Roth (2018-04-26)。[Maximizing the value of genomic data in healthcare](https://cloudblogs.microsoft.com/industry-blog/health/2018/04/26/maximizing-the-value-of-genomic-data-in-healthcare/)。檢自 Microsoft Genomics (2020-02-10)。

## 更新紀錄
<details>
  <summary>最後更新日期：2020-05-19</summary>
  <ul class="timestamp">
    　<li>2020-05-19 發表</li>
    　<li>2020-02-10 完稿</li>
  </ul>
</details>
