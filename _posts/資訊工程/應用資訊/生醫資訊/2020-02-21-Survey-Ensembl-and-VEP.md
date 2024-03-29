---
title: 【Survey】Ensembl 與 VEP
date: 2020-02-21
is_modified: false
categories:
- "應用資訊 › 生醫資訊"
- "資訊科技 › 開發與輔助工具"
tags:
- 基因體學 Genomics
- 網站與雲端平台
- 工具介紹與操作
--- 

最近（2019/08）因為公司專案需求，因此稍微 Survey 一下 [Ensembl](https://asia.ensembl.org/index.html) 這個網站，不過 Survey 方向只會專注在專案需求上，並不會全面的 Survey 。
  
在 Survey 過程中最痛苦的大概是看懂並搞懂那些那些專有名詞，我的生物程度只停留在高三，而且還有退化的趨勢阿(( 崩潰

<br class="big">
  
 
<div class="alert warning">
<div class="head">翻譯交雜</div>
對 Bioinformatics 實在很不熟，許多專有名詞中國翻譯與臺灣翻譯交雜，我會儘量附上對應的英文方便日後找尋。
</div>


<!--more-->
## Ensembl Intro.
<p class="illustration">
    <img src="https://i.imgur.com/gX2Re5D.png" alt="ensembl_logo">
    Ensembl Logo（圖片來源: <a href="https://asia.ensembl.org/info/about/legal/logo_policy.html">ensembl</a>）
</p>

Ensembl 其實是一項開始於 1999 年的生物資訊學的研究計劃，由是一個由歐洲生物資訊研究所（European Bioinformatics Institute, EMBL）和維康基金桑格研究院（Wellcome Trust Sanger Institute）所推動。其目標是其致力於<mark>統整基因注釋（Annotation）和定序資料的整合</mark>，並讓研究人員可以透過網路來取得所需資料。
  
而網站則開始於 2000 年，原是一個真核生物注釋項目，主要側重在脊椎動物；但隨著時間推移，Ensembl 資料庫也包含了越來越多的基因體資料，同時，它的可用資料的範圍也擴展到了比較基因體學、變異位點...等方面。 
  
Ensembl 可支援將基因體變異、基因體大範圍重組（e.g. chromothripsis, chromoplexy）以<mark>視覺化方式呈現</mark>、檢視基因在染色體上的注釋、探索某個基因同源性（Homology）和進化樹、檢視比對到基因上的 mRNA 或蛋白的序列位置及 Variant Effect Predictor...等功能。

截至目前為止，Ensembl 發布的最新資料庫版號為 97。但它有持續更新，需要可以自行上[官網](https://asia.ensembl.org/index.html)查詢。
- 2023/01/05：我今天看好像已經到 **Release 108 (Oct 2022)**。



### 其他資料庫
除外 [Ensembl 資料庫](http://asia.ensembl.org/index.html)外，還有 NCBI（National Center for Biotechnology information）所提供的 [GenBank](https://www.ncbi.nlm.nih.gov/genbank/) 以及 UCSC 與其他資料庫，e.g. UniProt，前三者（在某些文章中）並列目前三大資料庫。
  
如果對三者的介紹有興趣可以看看這篇：[〈NCBI, UCSC, Ensembl, Uniprot, 一次学完统统不要钱〉](http://www.sohu.com/a/161725404_170798)，要不要錢我是不知道啦，~~反正我的 WeChat 救不回來了~~，反正它前面公開的部分有稍微說到三個的資料庫差異，我只需要先對它們有個概念就好。


 
## Ensembl 資料庫網站操作
在 Ensembl  資料庫網站想進行資料查詢，可藉由物種、基因名稱、基因位置與疾病名稱...等。這邊挑了兩種不同類型的搜尋結果做紀錄。


### 用物種搜尋
1. 從[主頁](http://asia.ensembl.org/index.html)輸入搜尋物種 "Human"，搜尋需要一點時間，出來[結果](http://asia.ensembl.org/Multi/Search/Results?q=Human;site=ensembl)第一個條目就是智人了。

    <p class="illustration">
    <img src="https://i.imgur.com/1ALsKQ3.png" alt="Human Search Result">
    </p>
    
2. 點擊搜尋結果，可以看到人類基因體的[主頁](http://asia.ensembl.org/Homo_sapiens/Info/Index)。

    <p class="illustration">
    <img src="https://i.imgur.com/G5bMvLn.png" alt="Human">
    </p>
    
3. 點選 **View karyotype** 可以看到 **Whole genome**。

    <p class="illustration">
    <img src="https://i.imgur.com/YCcV3oM.png" alt="Human Whole genome">
    </p>
    
4. 點擊任意一條基因 → Jump to region view → Chromosome summary，可以看到每一條基因的資訊。

    <p class="illustration">
    <img src="https://i.imgur.com/Ztp7QWZ.png" alt="Human Chromosome summary">
    </p>



### 用基因名稱搜尋
1.  直接搜尋 "BRCA2" [結果](http://asia.ensembl.org/Multi/Search/Results?q=BRCA2;site=ensembl_all)，一樣第一個就是我們要的搜尋結果。

    <p class="illustration">
    <img src="https://i.imgur.com/HPBxf0Z.png" alt="BRCA2 Search Result">
    </p>
    
2. 點擊後可以看到針對基因的描述等[資訊](http://asia.ensembl.org/Homo_sapiens/Gene/Summary?db=core;g=ENSG00000139618;r=13:32315086-32400266)。

    <p class="illustration">
    <img src="https://i.imgur.com/tUEJv3C.png" alt="BRCA2">
    </p>



### Ensembl ID
不過搜尋的時候看到那堆 ID、敘述...等，不禁讓人眼花撩亂，而且聽說不同的資料庫還都有不同的規則...這已經不是眼花撩亂而是頭痛欲裂了吧！不過也只能咬牙看下去了 QAQ
 
<br class="big">

1. **先看看頁面中最顯眼的 <mark>Gene: BRCA2</mark>**  
    BRCA2 是由 HGNC（HUGO Gene Nomenclature Committee，人類基因命名委員會）對基因進行命名描述的一個 HGNC Symbol，又稱為**縮寫標識符**，具有**唯一性**。 

    人類基因命名委員會，顧名思義就是為人類基因進行命名的。由於 HUGO 是國際權威的權威機構，因此多數資料庫都會引入它的命名與 ID ，方便跨資料庫進行搜尋。
 
    <br class="big">

2. **再看看 HGNC Symbol 右方的 <mark>ENSG00000139618</mark>**  
    這跟下方的 <mark>Ensembl version</mark> 是同一組編碼。是給 HGNC Symbol 在 Ensembl 中的一個編號，由五個部分所組成：  
    1. **ENS**  
        用來闡明這是一個 Ensembl ID。
    2. **物種前綴**  
        用來表示這是啥物種，如果有需要可以看對照表 → [Species prefixes](https://asia.ensembl.org/info/genome/stable_ids/prefixes.html)。值得一提的是，如果物種是人類，這欄位會是空的。
    3. **功能前綴**  
        用來註明這是一個基因、外顯子或是蛋白質家族...等。對照表 [Feature prefixes](https://asia.ensembl.org/info/genome/stable_ids/prefixes.html) 在這。
    4. **唯一的 11 碼數字**
    5. **版本號**  
        在小數點之後的數字是版號，為了維持 stable，Ensembl ID 儘量不會變動，因此在基因資料發生一些小的改動時會去變動最後的版號。不過，如果整個基因整體模式都變動的話，還是會重新分配一個 ID。

    <br class="big">

3. **往下看到 <mark>Description</mark>**  
    這行其實有兩項資訊。前面的 **BRCA2 DNA repair associated** ，其實是 HGNC [批准的全基因名稱](https://www.genenames.org/data/gene-symbol-report/#!/hgnc_id/HGNC:1101)，對應於上面的 HGNC Symbol。  

    而後面的 **HGNC:1101**，則是 HGNC 分配的基因編號。雖然 HGNC Symbol 的可讀性較高，但在資料處理時一般會說建議使用 HGNC ID 作為唯一標識符。因為有時候 HGNC 會對一些已經命名過的基因進行重新審查和命名，以確保新的基因命名在描述基因功能方面更加的準確，但 HGNC ID 卻是固定不變的。
    
    <br class="big">

4. **最後看 <mark>Gene Synonyms</mark> 的部分**  
    前面 HGNC 會對一些已經命名過的基因重新命名，而此時舊有的名稱就會被當作同義詞來使用。

<br class="big">

剩下的資訊就比較好懂了，如基因位置，就不贅述的。好吧，其實沒比較好懂...不過至少知道它是只基因上的座標 XDDD
  


##  Variant Effect Predictor (VEP)
不過這次 Survey 的主要目標是關於 <mark>annotation</mark> 的步驟。在 Ensembl 中有提供一套注釋與分析工具 - Variant Effect Predictor（簡稱 VEP），它可以對測試結果產生的變異進行注釋，包括 SNPs、Indel 等，每個注釋用來預測可能受到影響的轉錄。此外所輸出的結果也可以根據資料庫內容與需求，對變異進行過濾與排序，並列出最具致病性的或是全部的效應。

<p class="illustration">
    <img src="https://i.imgur.com/P8DYMOP.png" alt="VEP 注釋流程">
    VEP 注釋流程（圖片來源: <a href="https://www.genedock.com/article/2017/10/16/vep-%E5%BC%BA%E5%A4%A7%E7%9A%84%E5%8F%98%E5%BC%82%E6%B3%A8%E9%87%8A%E5%B7%A5%E5%85%B7/">GeneDock 文档</a>）
</p>

此外，在找尋資料的過程中，發現關於 annotation tool 中有三套被反覆提及，分別是 <mark>Annovar</mark>、<mark>SnpEff</mark> 與 Ensembl 的 <mark>VEP</mark>，這三套都是做變異注釋的工具：

- [〈突变注释工具SnpEff,Annovar,VEP,oncotator比较分析〉](https://www.jianshu.com/p/6284f57664b9)
    - 文章好像掛，可以直接拿標題去搜尋可以看到有人轉載。
- [〈變異註釋軟件SnpEff, VEP, Annovar的比較(上)〉](https://www.gushiciku.cn/dc_hk/200114289)  
    - 不是我不貼下，而是我真的找不到下。 

<br class="big"> 

這邊接下來記錄一下 VEP 安裝方式。
### Installation VEP
在 Ensembl 中有分 [Web interface](https://asia.ensembl.org/Tools/VEP) 與 [Command line tool](https://asia.ensembl.org/info/docs/tools/vep/script/index.html)，兩種使用方式，小資料量用前者，大資料量用後者。

VEP 的資料輸入與輸出可以直接支援 VCF 格式，但輸入的 VCF 應提供 identifier（不提供也可以執行，就只是會跑掉而已）
> In order to parse this correctly, VEP needs to convert such variants into Ensembl-type coordinates, and it does this by removing the additional base and adjusting the coordinates accordingly. This means that if an identifier is not supplied for a variant (in the 3rd column of the VCF), then the identifier constructed and the position reported in VEP's output file will differ from the input.


#### init 
```bash
$ apt-get update
$ apt-get upgrade
$ apt-get install git
$ apt-get install curl
```


#### Clone the Git repository
```bash
$ git clone https://github.com/Ensembl/ensembl-vep.git
$ cd ensembl-vep
```


####  Requirements
```bash
$ apt-get install build-essential ##(gcc,g++ and make)
$ apt-get install perl ## (>5.10 or above recommended (tested on 5.10, 5.14, 5.18, 5.22, 5.26))
$ cpan Archive::Zip
$ apt-get install libmysqlclient-dev
$ cpan DBD::mysql
$ cpan DBI
```


#### Installation
```bash
$ curl -0  https://api.github.com/repos/Ensembl/ensembl-vep 
$ cpan Archive::Extract
$ cpan Try::Tiny
$ perl INSTALL.pl
```


#### Run
```bash
$ ./vep -i examples/homo_sapiens_GRCh38.vcf --cache
```

<br class="big"> 

執行完的後 VEP 會為它的 VCF 輸出在每個 row 後面加上 [CSQ field](https://www.ensembl.org/info/docs/tools/vep/vep_formats.html)。



## 關於事前 Survey 的目的
D大提出關於 VCF annotation 可以歸納出幾項是需要進一步了解的。

1. **通常 VCF annotation 步驟的用意是什麼？**  
    annotation 主要是注釋 VCF，每個注釋用來預測可能受到影響的轉錄（transcription）。根據配置會列出最具致病性的或是全部的效應。

    VEP 會用於：
    1. 變異的位置、轉錄物的上游
    2. 受變異影響的基因與轉錄本
    3. 變異對於蛋白質序列與調控區域的影響
    4. 變異與 frequareucy data(?)

    <br class="big">

2. **會使用到哪些工具與資料庫？**  
    在 VEP 中用到的資料庫有：GENCODE、dbSNP、Loop Genomics、ESP、ExAC、COSMIC、HGMD public、clinvar、PolyPhen、SIFT。

    其他用於注釋的工具有 Annovar、VEP、SnpEff 與 oncotator，其中 oncotator 主要是用於癌症特異性變異位點的注釋，而 SnpEff 主要是面向臨床和精準醫療的。


3. **經過 annotation 步驟後, 檔案格式內容是什麼？**  
    input 可[支援](https://www.ensembl.org/info/docs/tools/vep/vep_formats.html) VCF、 VCF Structural variants...等，不過要注意 unbalanced variant。

    output 則是可以選擇 VCF 或 json，VCF 輸出文件內容會保持不變，但會增加 CSQ field。

4. **通常生技領域在做完 vcf annotation, 後續會如何應用? 是交給專業醫生去看? 還是有相關分析工具可以協助產生報告？**  
    報告部分跑完就會產生一份 HTML 檔 其中對於 vcf annotation 有完整的資料統計。



## 小結
這是過了一陣子才整理的資料，總比不上 Survey 立刻整理來的詳細，而且我記得我有跑數據，偏偏找不到數據了。不過沒魚蝦也好，好歹也是留個紀錄。

 

## 參考資料 
1. 協同撰寫。[Ensembl](https://zh.wikipedia.org/wiki/Ensembl)。檢自 維基百科 (2019-08-28)。
2. Lin Ting-Wei (2016-02-24)。[Ensembl API（一）：簡介](https://weitinglin.com/2016/02/24/ensembl-api%EF%BC%88%E4%B8%80%EF%BC%89%EF%BC%9A%E7%B0%A1%E4%BB%8B/)。檢自 我們的基因體時代 Our "Gene"ration (2019-08-28)。
3. Sam (2019-01-16)。[【3.2】 一级核酸数据库-Ensemble](https://qinqianshan.com/bioinformatics/biodatabase/ensembl/)。檢自 sam's note (2019-08-28)。
4. 子非鱼 (2017-08-02)。[NCBI, UCSC, Ensembl, Uniprot, 一次学完统统不要钱](http://www.sohu.com/a/161725404_170798)。檢自 解螺旋·临床医生科研成长平台 (2019-08-28)。
5. 生信技能樹 (2018-10-25)。[超精華生信ID總結，想踏入生信大門的你-值得擁有](https://www.twblogs.net/a/5bd13cec2b71776a052c9c7b)。檢自 台部落 (2019-08-28)。
6. 我爱小徐子 (2018-09-08)。[使用Ensembl数据库获取人类染色体图谱，并输出fasta](https://zhuanlan.zhihu.com/p/44059580)。檢自 知乎 (2019-08-28)。
7. Kai (2017-05-30)。[NCBI/Ensembl ID的转换](http://www.bioinfo-scrounger.com/archives/166)。檢自 生信笔记 (2019-08-28)。
8. [Ensembl ID /  Ensembl Stable ID](https://asia.ensembl.org/info/genome/stable_ids/index.html)。檢自 Ensembl (2019-08-28)。
9. Liu Xuanzhu (2017-10-16)。[VEP--强大的变异注释工具](https://www.genedock.com/article/2017/10/16/vep-%E5%BC%BA%E5%A4%A7%E7%9A%84%E5%8F%98%E5%BC%82%E6%B3%A8%E9%87%8A%E5%B7%A5%E5%85%B7/)。檢自 GeneDock 文档 (2019-08-28)。
10. 生信杂谈 (2017-09-26)。[突变注释工具SnpEff,Annovar,VEP,oncotator比较分析](https://www.jianshu.com/p/6284f57664b9)。檢自 簡書 (2019-08-28)。
11. GCBI知識庫 (2019-04-14)。[變異註釋軟件SnpEff, VEP, Annovar的比較 (上)](https://www.gushiciku.cn/dc_hk/200114289)。檢自 微文庫 (2019-08-28)。
12. [Annotation & Prediction](https://asia.ensembl.org/info/genome/index.html)。檢自 Ensembl (2019-08-28)。
13. [Ensembl Variant Effect Predictor (VEP)](https://asia.ensembl.org/info/docs/tools/vep/index.html)。檢自 Ensembl (2019-08-28)。
14. [Variant Effect Predictor - Data formats](http://asia.ensembl.org/info/docs/tools/vep/vep_formats.html#input)。檢自 Ensembl (2019-08-28)。
15. [Variant Effect Predictor - Tutorial](http://asia.ensembl.org/info/docs/tools/vep/script/vep_tutorial.html)。檢自 Ensembl (2019-08-28)。
16. [Variant Effect Predictor - Download and install](https://asia.ensembl.org/info/docs/tools/vep/script/vep_download.html)。檢自 Ensembl (2019-08-28)。
17. [Variant Effect Predictor - Annotation sources](https://www.ensembl.org/info/docs/tools/vep/script/vep_cache.html?redirect=no)。檢自 Ensembl (2019-08-28)。
18. [Variation File Format - Definition and supported options](https://www.ensembl.org/info/website/upload/var.html)。檢自 EnsemblMetazoa (2019-08-28)。
19. [Annotating VCF Files](https://doc-openbio.readthedocs.io/projects/jannovar/en/master/annotate_vcf.html)。檢自 Jannovar documentation (2019-08-28)。 