---
title: 【Survey】 BLINK ＆ PLINK
date: 2020-03-03
is_modified: false
categories:
- 生醫資訊
- 開發與輔助工具
tags:
- Genomics
- BLINK
- PLINK
- GWAS
--- 

這是前一陣子（2019-08-28）的筆記了...，那一陣子從「全基因體定序」、「全基因體圖譜繪製」到「全基因體關聯分析 （GWAS）」都稍微沾了點，但計畫最終不了了之，只好先擱置一旁...
  
不過沒留下點記錄真的很可惜，所以趁著兩個專案間的空檔，把工作時留下的隨手筆記從草稿夾抓出來整理整理。
  
不過不是立即記錄下的筆記，難免缺三漏四的...:sweat_smile:
   
<div class="alert warning">
<div class="head">大量複製貼上</div>
由於這篇這我的隨手筆記重新排版而成，大部分的內容可能是直接複製貼上...。當然我最後有附上參考資料...不過，不確定這樣可不可行...。
</div>
   
<div class="alert warning">
<div class="head">凌亂注意</div>
此篇就只是隨手筆記，內容非常的不連續。
</div>


<!--more-->
<br><br>

## 全基因體關聯分析 （GWAS） 

全基因體關體分析（Genome Wide Association Study）是指在人類全基因體範圍內找出存在的序列變異，即單核苷酸多態性（SNP），從中篩選出與疾病相關的 SNPs。

白話一點的說法就是，先決定一個想研究的性狀/疾病，然後去找到兩群人，一群有這性狀/疾病，另外一群做對照。考慮可能的混淆因素，並比對這兩群人所有/可能的 SNPs，最後依照基因型頻率分布來找到與該性狀/疾病相關的遺傳位點。之後的應用可以在搭配醫療資料的大數據分析，以進行疾病預防及治療...等。

而全基因體關聯研究（GWAS）效能取決於三個因素：computing speed, memory requirements, and	statistical	power，這三個因素取決於所使用工具的統計方法與工具本身設計。

在 GWAS 常用演算法有三種常用：BLINK、FarmCPU 和 PLINK，不過這三種演算法偏向統計方法的改進，這篇的目的就是為了 Survey 這些工具。


<br><br>

## 相關閱讀

不過考慮到我對這些流程一無所知，在開始玩這些工具之前先去閱讀了相關的論文，以及工具的介紹：
1. [Iterative Usage of Fixed and Random Effect Models for Powerful and Efficient Genome-Wide Association Studies](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1005767)
2. [BLINK: A Package for Next Level of Genome Wide Association Studies with Both Individuals and Markers in Millions](https://academic.oup.com/gigascience/article/8/2/giy154/5238723) 
    - [Lab](http://zzlab.net/blink/index.html)
    - [GitHub](https://github.com/Menggg/BLINK) / [BLINK_User_Manual.pdf](https://github.com/Menggg/BLINK/blob/master/BLINK_User_Manual.pdf "BLINK_User_Manual.pdf")
3. [一种交替运用固定效应和随机效应模型优化全基因组关联分析的算法开发](http://tow.cnki.net/kcms/detail/detail.aspx?filename=1016156134.nh&dbcode=CRJT_CDFD&dbname=CDFDTOTAL&v=)  
    P.S. 挑這一篇單純就只是看它的一些名詞解釋

<br>
  
### Fixed and random model Circulating Probability Unification（FarmCPU）

GWAS 分析一直飽受兩個問題的困擾：大量的 false positives 和 false negatives，False positives 由群體結構和個體之間的親緣關係矩陣造成的。

將群體結構作為固定效應加入到一般線性模型中或者同時將群體結構作為固定效應，親緣關係矩陣作為隨機效應加在混合線性模型中可以很好的控制 false positives ，但混合線性模型會導致兩種效應變量與待檢測位點之間的混雜問題，從而降低了模型對關聯位點的檢測效力，造成了一定程度的 false negatives。

Fixed and random model Circulating Probability Unification （FarmCPU） 是為了解決混合線性模型中存在的混雜問題，通過交替使用一個固定效應模型（FEM）和一個隨機效應模型（REM）來解決模型，更好地控制模型混雜問題，顯著提高了統計功效和運算速度。 

<br>

### BLINK

BLINK 有能與混合線性模型相比的控制 false positives 能力，但需要更少的複雜計算時間，幾乎與一般線性模型的線性計算時間複雜度相匹配。

在 FarmCPU 中，它將混合線性模型分離成固定效應模型（FEM）與隨機效應模型（REM），然後迭代地使用它們。但隨機效應模型（REM）計算成本計算成本較高，所以 BLINK，使用貝氏訊息準則  (Bayesian Information Criteria, BIC)將隨機效應模型替換為固定效應模型。

<br>

<div class="alert info">
<div class="head">BIC</div>
是在不完全情報下，對部分未知的狀態用主觀概率估計，然後用貝葉斯公式對發生概率進行修正，最後再利用期望值和修正概率做出最優決策。
</div>

 
<br>

根據實際和模擬數據分析顯示，與 FarmCPU 相比，BLINK 提高了統計功效，同時顯著縮短了計算時間。兩者分分析具有一百萬個人和五十萬個標記的數據集，FarmCPU 所需時間為一週，而 BLINK 可以在三小時內完成。 


執行：GUI 版本：[iPat](http://zzlab.net/iPat/)、command 版本：[BLINKC](http://zzlab.net/blink/index.html)
P.S.、都是張志武實驗做的

<br><br>

## PLINK
### 常見格式型別
PLINK軟體輸入檔案的常見格式型別：
1. 一般格式（PLINK text）：PED/MAP  
	```bash
	$ plink --file toy
	```
	會找名為 toy.ped and toy.map 的檔案，如要指定不同的 ped 與 map，則使用
	```bash
	--ped <filename>
	--map <filename>
	```
<br>


2. 二進位制格式（PLINK 1 binary）：BED/BIM/FAM  
	```bash
	--bfile [prefix]
	--bed <filename>  
	--bim <filename>  
	--fam <filename>
	```

	> **PLINK 1 binary is PLINK 1.9's preferred input format.**. In fact, PLINK 1.9 automatically converts most other formats to PLINK 1 binary before the main loading sequence.

	<br> 所以可以直接用來把其他檔案轉換成二進位制格式
	```bash
	$ plink --file text_fileset --out binary_fileset
	```
	  或是加上  --make-bed 做過濾
	  
	  如要保留執行過程中的轉換結果，則是用`--keep-autoconv` 
	  ```bash
	  $ plink --file text_fileset --freq --keep-autoconv --out results
	  ```
<br>
  
3. Variant Call Format 
	```bash
	--vcf <filename>  
	--bcf <filename>
	```
	VCF / BCF 的 filename 要打全稱，也就是要加上副檔名。此外提供不同的[參數](https://www.cog-genomics.org/plink/1.9/input#vcf)需依照資料格式來進行調整。
		
	> VCF reference alleles are set to A2 by the autoconverter even when they appear to be minor. However, to maintain backwards compatibility with PLINK 1.07, PLINK 1.9 normally forces major alleles to A2 during its loading sequence. One workaround is permanently keeping the .bim file generated during initial conversion, for use as --a2-allele input whenever the reference sequence needs to be recovered. (If you use this method, note that, when your initial conversion step invokes **--make-bed** instead of just --out, you also need **--keep-allele-order** to avoid losing track of reference alleles before the very first write, because --make-bed triggers the regular loading sequence.)

<br>

4. 其他： 
	1.  transposed text fileset：TPED/TFAM
		```bash
		--tfile [prefix]  
		--tped <filename>  
		--tfam <filename>
		```

	2. long-format fileset：LGEN
		```bash
		--lfile [prefix]  
		--lgen <filename>  
		--reference <filename>  
		--allele-count
		```
		
	3. Irregularly-formatted PLINK text files
 
 
<br><br>


### 輸入過濾

<center> <img src="https://pic2.zhimg.com/80/v2-b494eaba88bea07b1b8fb40b35f44e6d_hd.jpg" alt="GWAS 粗略流程圖"></center>
<center class="imgtext">GWAS 粗略流程圖（圖片來源: <a href="https://zhuanlan.zhihu.com/p/72490817" class="imgtext">用plink做一套GWAS分析 - 知乎</a>）</center>
<br>


<center> <img src="https://i.imgur.com/0xx5gzW.png" alt="GWAS基本分析内容"></center>
<center class="imgtext">GWAS基本分析内容（圖片來源: <a href="https://www.jianshu.com/p/70164cce947c" class="imgtext">GWAS基本分析内容 - 簡書</a>）</center>
<br>


<br>

### 準備
```
# SNP完整度在95%
$ plink --file wgas1 --make-bed --out wgas1 --mind 0.05
```
 
<br>

結果：
```
Logging to wgas1.log.
Options in effect:
  --file wgas1
  --make-bed
  --mind 0.05
  --out wgas1

15920 MB RAM detected; reserving 7960 MB for main workspace.
.ped scan complete (for binary autoconversion).
Performing single-pass .bed write (228694 variants, 90 people).
--file: wgas1-temporary.bed + wgas1-temporary.bim + wgas1-temporary.fam
written.
228694 variants loaded from .bim file.
90 people (45 males, 45 females) loaded from .fam.
90 phenotype values loaded from .fam.
1 person removed due to missing genotype data (--mind).
ID written to wgas1.irem .
Using 1 thread (no multithreaded calculations invoked).
Before main variant filters, 89 founders and 0 nonfounders present.
Calculating allele frequencies... done.
Total genotyping rate in remaining samples is 0.995473.
228694 variants and 89 people pass filters and QC.
Among remaining phenotypes, 48 are cases and 41 are controls.
--make-bed to wgas1.bed + wgas1.bim + wgas1.fam ... done.
```

<br>

### 過濾
Missingness per individual --mind 0.1 >10% 排除  ###樣本缺失率   
Missingness per marker --geno 0.1 > 10% 排除  ###點位缺失率  
Allele frequency --maf 0.05 MAF <= 0.05 排除  ###最小等位基因頻率  

<br>

1. 樣本缺失率 +點位缺失率  
	```bash
	$ plink --bfile wgas1 --missing --out miss_stat
	```
 
	結果
	```bash
	Logging to miss_stat.log.
	Options in effect:
	--bfile wgas1
	--missing
	--out miss_stat

	15920 MB RAM detected; reserving 7960 MB for main workspace.
	228694 variants loaded from .bim file.
	89 people (44 males, 45 females) loaded from .fam.
	89 phenotype values loaded from .fam.
	Using 1 thread (no multithreaded calculations invoked).
	Before main variant filters, 89 founders and 0 nonfounders present.
	Calculating allele frequencies... done.
	Total genotyping rate is 0.995473.
	--missing: Sample missing data report written to miss_stat.imiss, and
	variant-based missing data report written to miss_stat.lmiss.
	```

	.imiss : 樣本缺失率  
	.lmiss : 點位缺失率  
    <br>

 2. 最小等位基因頻率  
	``` bash
	$ plink --bfile wgas1 --freq --out freq_stat
	``` 
	結果
	```bash
	Logging to freq_stat.log.
	Options in effect:
	  --bfile wgas1
	  --freq
	  --out freq_stat

	15920 MB RAM detected; reserving 7960 MB for main workspace.
	228694 variants loaded from .bim file.
	89 people (44 males, 45 females) loaded from .fam.
	89 phenotype values loaded from .fam.
	Using 1 thread (no multithreaded calculations invoked).
	Before main variant filters, 89 founders and 0 nonfounders present.
	Calculating allele frequencies... done.
	Total genotyping rate is 0.995473.
	--freq: Allele frequencies (founders only) written to freq_stat.frq .
	```

	<br>區分群組的計算MAF
	``` bash
	$ plink --bfile wgas1 --within pop.cov --freq --out freq_stat
	```
	```bash
	Logging to freq_stat.log.
	Options in effect:
	  --bfile wgas1
	  --freq
	  --out freq_stat
	  --within pop.cov

	15920 MB RAM detected; reserving 7960 MB for main workspace.
	228694 variants loaded from .bim file.
	89 people (44 males, 45 females) loaded from .fam.
	89 phenotype values loaded from .fam.
	--within: 2 clusters loaded, covering a total of 89 people.
	Using 1 thread (no multithreaded calculations invoked).
	Before main variant filters, 89 founders and 0 nonfounders present.
	Calculating allele frequencies... done.
	Total genotyping rate is 0.995473.
	--freq: Cluster-stratified allele frequencies (founders only) written to
	freq_stat.frq.strat 
	```
    <br> 

 3. 主成分分析   
    > 在全基因體關聯性研究(Genome-Wide Association Study, GWAS)的分析中，干擾因子(confounder)的控制是重要的課題，其中最重要的干擾因子為族群分層(population stratification)。
    > 性狀(trait)或疾病的分布在不同族群中會不一樣，SNP的頻率在不同族群的分布也會不一樣，因此在探討兩者的關係時，若沒有考慮到族群分層，可能會出現偽陽性或偽陰性的結果，即使分析的研究對象都是來自單一族群，仍可能存在次分群的狀況。所以在完成 QC 後，進行相關性檢定前，需對研究族群進行主成分分析(Principle Component Analysis, PCA)，隨後將PCA結果作為共變數(covariate)，加入迴歸模型中以控制族群分層的影響。
    
    ```bash
    ./plink --bfile wgas1 --pca 10 --out wgas1_pca
    ```

    結果
    ```bash
    Logging to wgas1_pca.log.
    Options in effect:
    --bfile wgas1
    --out wgas1_pca
    --pca 10

    15920 MB RAM detected; reserving 7960 MB for main workspace.
    228694 variants loaded from .bim file.
    89 people (44 males, 45 females) loaded from .fam.
    89 phenotype values loaded from .fam.
    Using up to 4 threads (change this with --threads).
    Before main variant filters, 89 founders and 0 nonfounders present.
    Calculating allele frequencies... done.
    Total genotyping rate is 0.995473.
    228694 variants and 89 people pass filters and QC.
    Among remaining phenotypes, 48 are cases and 41 are controls.
    Relationship matrix calculation complete.
    --pca: Results saved to wgas1_pca.eigenval and wgas1_pca.eigenvec .
    ```
    <br>

4. 關聯分析  
    [https://zhuanlan.zhihu.com/p/72490817](https://zhuanlan.zhihu.com/p/72490817)  
    [http://zzz.bwh.harvard.edu/plink/anal.shtml](http://zzz.bwh.harvard.edu/plink/anal.shtml)  
    [https://www.cog-genomics.org/plink/1.9/assoc](https://www.cog-genomics.org/plink/1.9/assoc)  

    ```bash
    $ plink --bfile wgas1 --assoc --out as1
    ```
    ```bash
    Logging to as1.log.
    Options in effect:
    --assoc
    --bfile wgas1
    --out as1

    15920 MB RAM detected; reserving 7960 MB for main workspace.
    228694 variants loaded from .bim file.
    89 people (44 males, 45 females) loaded from .fam.
    89 phenotype values loaded from .fam.
    Using 1 thread (no multithreaded calculations invoked).
    Before main variant filters, 89 founders and 0 nonfounders present.
    Calculating allele frequencies... done.
    Total genotyping rate is 0.995473.
    228694 variants and 89 people pass filters and QC.
    Among remaining phenotypes, 48 are cases and 41 are controls.
    Writing C/C --assoc report to as1.assoc ... done.
    ```

    小樣本考慮FDR校正
    ``` bash
    $ plink --bfile wgas1 --assoc --adjust --out as2
    ```
    ```bash
    Logging to as2.log.
    Options in effect:
    --adjust
    --assoc
    --bfile wgas1
    --out as2

    15920 MB RAM detected; reserving 7960 MB for main workspace.
    228694 variants loaded from .bim file.
    89 people (44 males, 45 females) loaded from .fam.
    89 phenotype values loaded from .fam.
    Using 1 thread (no multithreaded calculations invoked).
    Before main variant filters, 89 founders and 0 nonfounders present.
    Calculating allele frequencies... done.
    Total genotyping rate is 0.995473.
    228694 variants and 89 people pass filters and QC.
    Among remaining phenotypes, 48 are cases and 41 are controls.
    Writing C/C --assoc report to as2.assoc ... done.
    --adjust: Genomic inflation est. lambda (based on median chisq) = 1.3727.
    --adjust values (188187 variants) written to as2.assoc.adjusted .
    ```
    <br> 

5. 關聯分析 - 經過一定篩選的特定位點的細化分析  
    ```bash
    ./plink --bfile wgas1 --model --out mod1
    ```
    ```bash
    CHR         SNP   A1   A2     TEST            AFF          UNAFF        CHISQ   DF            P
    1   rs3094315    G    A     GENO        0/14/33         0/7/34           NA   NA           NA
    1   rs3094315    G    A    TREND          14/80           7/75        1.948    1       0.1628
    1   rs3094315    G    A  ALLELIC          14/80           7/75        1.684    1       0.1944
    1   rs3094315    G    A      DOM          14/33           7/34           NA   NA           NA
    1   rs3094315    G    A      REC           0/47           0/41           NA   NA           NA
    1   rs6672353    A    G     GENO         0/1/47         0/0/40           NA   NA           NA
    1   rs6672353    A    G    TREND           1/95           0/80       0.8429    1       0.3586
    1   rs6672353    A    G  ALLELIC           1/95           0/80       0.8381    1       0.3599
    1   rs6672353    A    G      DOM           1/47           0/40           NA   NA           NA
    ```


    分層分析
    ```bash
    ./plink --bfile wgas1 --cluster --mc 2 --ppc 0.05 --out str1
    ```
    ```bash
    $ cat str1.cluster1 
    SOL-0	 CH18526_NA18526 CH18637_NA18637
    SOL-1	 CH18524_NA18524 JA18976_NA18976
    SOL-2	 CH18529_NA18529 CH18603_NA18603
    SOL-3	 CH18558_NA18558 CH18623_NA18623
    SOL-4	 CH18532_NA18532 CH18622_NA18622
    SOL-5	 CH18561_NA18561 CH18566_NA18566
    SOL-6	 CH18562_NA18562 CH18550_NA18550
    SOL-7	 CH18537_NA18537 CH18635_NA18635
    SOL-8	 CH18540_NA18540 CH18563_NA18563
    SOL-9	 CH18605_NA18605 CH18594_NA18594

    $ cat str1.cluster2
    CH18526 NA18526	0
    CH18524 NA18524	1
    CH18529 NA18529	2
    CH18558 NA18558	3
    CH18532 NA18532	4
    CH18561 NA18561	5
    CH18562 NA18562	6
    CH18537 NA18537	7
    CH18603 NA18603	2
    CH18540 NA18540	8

    $ cat str1.cluster3
    CH18526 NA18526	0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 00 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    CH18524 NA18524	1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 00 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    CH18529 NA18529	2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 0 0 0 0 0 0 0 00 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    CH18558 NA18558	3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 0 0 0 0 0 0 0 00 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    CH18603 NA18603	8 8 8 8 8 8 8 8 8 8 8 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 0 0 0 0 0 0 0 00 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0

    ```

    關聯分析 - 分組
    ```bash
    ./plink --bfile wgas1 --mh --within str1.cluster2 --adjust --out aac1
    ```
    ```bash
    $ cat aac1.cmh.adjusted
    CHR         SNP      UNADJ         GC       BONF       HOLM   SIDAK_SS   SIDAK_SD     FDR_BH     FDR_BY
    1   rs1323127  5.312e-05  0.0001136          1          1     0.9999     0.9999     0.8349          1 
    1   rs6692436  0.0001412  0.0002783          1          1          1          1     0.8349          1 
    3   rs2856028  0.0001478  0.0002902          1          1          1          1     0.8349          1 
    3   rs2856030  0.0001478  0.0002902          1          1          1          1     0.8349          1 
    19   rs2869480  0.0002386  0.0004501          1          1          1          1     0.8349          1 
    19   rs1860020  0.0002386  0.0004501          1          1          1          1     0.8349          1 
    19  rs13344436  0.0002386  0.0004501          1          1          1          1     0.8349          1 
    22   rs1296754  0.0002457  0.0004624          1          1          1          1     0.8349          1 
    10   rs7101088  0.0002566  0.0004811          1          1          1          1     0.8349          1 
    ```

    刪除/選定特定snp或樣本
    ```bash
    ./$ plink --file data --extract mysnps.txt
    ```



<br><br> 

## 參考資料 
1. 研之有物│中央研究院 (2018/05/18)。[全基因組分析能讓我們知道多少事？](https://kknews.cc/zh-tw/science/yarmxxa.html) 。檢自 PanSci 泛科學 (2020-03-03)。
2. 知乎日報 (2016-12-20)。[聽起來很厲害的「全基因組關聯分析」，能算命嗎？](https://kknews.cc/zh-tw/science/yarmxxa.html) 。檢自 每日頭條 (2020-03-03)。
3. 協同撰寫。[全基因組關聯分析](https://zh.wikipedia.org/wiki/%E5%85%A8%E5%9F%BA%E5%9B%A0%E7%BB%84%E5%85%B3%E8%81%94%E5%88%86%E6%9E%90) 。檢自 維基百科 (2020-03-03)。
4. 刘思远 (2019-07-29)。[华盛顿州立大学张志武教授来基因组所做作学术报告](http://agis.caas.cn/xwzx/xwzx1/195503.htm)。檢自 中国农业科学院农业基因组研究所 (2020-03-03)。
5. 心心向農隊 (2019-06-14)。[關聯分析的特點和優勢](https://kknews.cc/zh-tw/news/9gry48b.html)。檢自 每日頭條 (2020-03-03)。
6. rapunzel0103 (2018-01-08)。[临床生物信息学中的GWAS分析](https://vip.biotrainee.com/d/206-%E4%B8%B4%E5%BA%8A%E7%94%9F%E7%89%A9%E4%BF%A1%E6%81%AF%E5%AD%A6%E4%B8%AD%E7%9A%84gwas%E5%88%86%E6%9E%90)。檢自 生信技能树 (2020-03-03)。
7. 刘小磊 (2016)。[一种交替运用固定效应和随机效应模型优化全基因组关联分析的算法开发](http://tow.cnki.net/kcms/detail/detail.aspx?filename=1016156134.nh&dbcode=CRJT_CDFD&dbname=CDFDTOTAL&v=)。檢自 中國知網 (2020-03-03)。
8. [PLINK 1.9](https://www.cog-genomics.org/plink/1.9/)。檢自 PLINK (2020-03-03)。
9. [File format reference - PLINK 1.9](https://cog-genomics.org/plink/1.9/formats)。檢自 PLINK (2020-03-03)。
10. (2018-12-09)。[利用PLINK進行GWAS分析](https://www.itread01.com/content/1544350881.html)。檢自 IT閱讀 (2020-03-03)。
11. Beccaliii (2019-09-11)。[用plink做一套GWAS分析](https://zhuanlan.zhihu.com/p/72490817)。檢自 知乎 (2020-03-03)。
12. river1023 (2019-03-16)。[【GWAS】使用PLINK評估族群分層的現象 ](https://rover1023.pixnet.net/blog/post/403824620-%E3%80%90gwas%E3%80%91%E4%BD%BF%E7%94%A8plink%E8%A9%95%E4%BC%B0%E6%97%8F%E7%BE%A4%E5%88%86%E5%B1%A4%E7%9A%84%E7%8F%BE%E8%B1%A1)。檢自 走過必留下痕跡  痞客邦 (2020-03-03)。
13. Tommy Huang (2018-04-20)。[機器/統計學習:主成分分析(Principal Component Analysis, PCA)](https://medium.com/@chih.sheng.huang821/%E6%A9%9F%E5%99%A8-%E7%B5%B1%E8%A8%88%E5%AD%B8%E7%BF%92-%E4%B8%BB%E6%88%90%E5%88%86%E5%88%86%E6%9E%90-principle-component-analysis-pca-58229cd26e71)。檢自 Tommy Huang - Medium (2020-03-03)。
15. 橙子牛奶糖 (2018-12-17)。[GWAS分析基本流程及分析思路](https://www.cnblogs.com/chenwenyan/p/10129785.html)。檢自 橙子牛奶糖 - 博客园 (2020-03-03)。
16. rapunzel0103 (2018-02-04)。[跑通GWAS，用这些脚本就够了](https://www.jianshu.com/p/53362fe881cd)。檢自 簡書 (2020-03-03)。
17. rapunzel0103 (2018-01-20)。[GWAS基本分析内容](https://www.jianshu.com/p/70164cce947c)。檢自 簡書 (2020-03-03)。
18. Ochi (2017-01-04)。[plink进行GWAS分析](http://blog.sina.com.cn/s/blog_80572f5d0102x8fj.html))。檢自 Ochi - 新浪博客 (2020-03-03)。
19. [bcftools](https://samtools.github.io/bcftools/bcftools.html)。檢自 bcftools (2020-03-03)。
20. [PLINK assoc input: can it be two vcf files case/control?](https://www.biostars.org/p/355804/)。檢自 biostars (2020-03-03)。
 
 
 
 