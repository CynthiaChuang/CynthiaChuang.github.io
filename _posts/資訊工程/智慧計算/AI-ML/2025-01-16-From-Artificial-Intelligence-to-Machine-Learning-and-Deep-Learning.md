---
title: 從人工智慧到機器學習、深度學習
date: 2025-01-16
is_modified: true
categories:
- "智慧計算 › 人工智慧"
tags:
- "AI/ML"
- "煉丹常識"
---

之前在整理 [〈GTC 2022 某場演講〉](https://cynthiachuang.github.io/Lecture-Notes-Nvidia-GTC-2022/#s42424-deep-learning-demystified-for-the-biopharma-ecosystem)與[〈CodeFree｜喝一杯咖啡，輕鬆學電腦科學〉](https://cynthiachuang.github.io/Hiskio-Codefree-Computer-Science-2/#16-1人工智慧的巨量資料學習法)時，我就有打算要整理。不過拖延症又發作，導致這篇文章一度難產 ╮(╯▽╰)╭。   

直到最近文組的朋友問了我這個問題，我才想起這篇躺在草稿夾裡的文章，想到好久沒更新的網誌，乾脆把它拿出來寫一寫唄！...雖然距離重寫到現在發表，又過了好一段時間（掩面

<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/3NNjy42.png" alt="人工智慧是一個總稱，包括機器學習和深度學習" width="300px">
    人工智慧是一個總稱，包括機器學習和深度學習（圖片來源: <a href="https://www.prowesscorp.com/whats-the-difference-between-artificial-intelligence-ai-machine-learning-and-deep-learning/">Prowess Consulting</a>）
</p>

這張圖可以幫助我們理解三者之間的關係，基本上它們是彼此的子集。<mark>人工智慧是一個總稱，包含了機器學習，而深度學習又是機器學習下的一個分支</mark>。

<br class="big"> 

是說，我在閱讀時，找到一句很棒的話：
<div class="blockquote-center">
深度學習驅動機器學習，最後實現了人工智慧
</div>


## 人工智慧（Artificial Intelligence，AI）
人工智慧（Artificial Intelligence，AI）其實並不是一個特定的技術，而是一種<mark>通過電腦來展現人類智慧</mark>的概念。這個概念發展至今日，已經廣泛應用於醫療、製造、電子產品、交通、翻譯、金融等領域。

依照這個概念來說，所有模擬人類智慧的技術都可以被視為 AI 的範疇，因此像是 **規則式系統（Rule-based Systems）** 與 **專家系統（Expert Systems）** 也都屬於 AI 的範疇。所以，當許多公司宣稱擁有 AI 時，可能跟你想像中的 AI 有些不同 XDDD。

<br class="big"> 

現如今，大家對人工智慧的想像，可能更貼近《鋼鐵人》中賈維斯的形象，甚至比它更強大——不僅能像人類一樣進行真正的思考、推理，還能實現自我學習與持續進化。像這類的技術依其智慧能力可劃分在 **通用 AI（Artificial General Intelligence，AGI）** 的階段。

然而，目前通用 AI 仍停留在科幻小說和理論研究階段，尚未真正實現。不過，去年底，據傳 OpenAI 的內部計畫 Q（Q Star）* 在某些關鍵領域取得了突破性進展，這可能標誌著向通用 AI 邁進了一步。

<p class="illustration">
    <img src="https://i.imgur.com/lmHAjCD.png" alt="AI的演進" width="600px">
    AI的演進（圖片來源: <a href="https://www.eetimes.com/a-path-to-broad-ai-5-challenges/">EE Times</a>）
</p>

若說，通用 AI 是追求像人類般的全面智能，那 **狹義 AI（Narrow AI）** 和 **廣義 AI（Broad AI）**，則代表了處於進行式和未來式的兩個階段：
1. **狹義 AI（Narrow AI）**  
    這是目前發展較為成熟的 AI 系統，專注於<mark>單一工作任務</mark>，在具備充足資訊的情況下，它們的表現可以達到甚至超越人類。見的應用領域包括電腦視覺、語音識別、語言翻譯、聊天機器人...等。
2. **廣義 AI（Broad AI）**  
    廣義 AI 則著眼於<mark>跨領域、多任務的工作執行</mark>，模擬人類在做決策時所需綜合考量的各種情況。這是目前學術機構和大公司積極探索的領域。
    
即便在聊天機器人領域的領頭羊 ChatGPT，目前仍未達到真正的廣義 AI 的程度。所以，IBM 森本典繁先生在 2019 年的一篇文章中提到：
<div class="blockquote-center">
我們目前在狹義 AI 和廣義 AI 之間，只到達 AI 的入口而已！
</div>
 
在更新這篇文章時，我去問 ChatGPT 他是否覺得自己已經達到廣義 AI 的程度。它回答我，它並未達到真正的廣義 AI 的程度，但在某些方面展現了廣義 AI 的初步特徵，並給了我以下的原因：

1. **跨領域應用的能力**  
    ChatGPT 確實能夠在多個領域中回答問題，從科學、語言學到生活常識，甚至創意寫作。這表明它具有一定的跨領域知識處理能力，但這是基於大規模語料的訓練結果，而非內在理解。
2. **上下文處理與適應**  
    它能記住對話上下文，調整回應風格，對使用者需求進行一定程度的適應。然而，這種適應是基於統計模型，而非真正的推理或長期學習。
3. **學習與改進**  
    ChatGPT 無法在與使用者互動中實時學習，也不能主動更新知識。它的能力停留在模型訓練完成的知識點上，而廣義 AI 應具備的自主學習和持續改進特性並不存在。

<br class="big">

思考了一下，為什麼 ChatGPT 會這樣回覆我。若從**跨領域整合**與**持續學習**兩方向來思考，它確實展現了狹義 AI的高度成熟，但仍未達到 Broad AI 的核心要求：
1. **跨領域整合**  
   ChatGPT 雖然能模擬或回答跨領域的知識應用，但它這功能是基於語料的訓練結果，無法像人類一樣處理複雜、動態的多任務需求；但理想的廣義 AI 應該能做到多領域知識之間進行真正的整合。
2. **推理與理解能力**  
    ChatGPT 的推理能力主要基於訓練語料的模式匹配，難以解決未見問題或生成真正原創的解答；但理想的廣義 AI 應該具備人類般的推理能力，給出創新的解決方案。

<br class="big"> 

題外話，我不太確定 General AI 這個詞該怎麼翻，我看過有文件將 General 這個詞翻譯成普適、通用、一般、廣義...等，最最常見的翻譯是「廣義」一詞。但如果選擇這個翻譯，Broad AI 這個詞反而無法對應，所以我最終採用了[《天下雜誌》IBM 專欄](https://event.cw.com.tw/2018ibm/article/index2/article7.html)中所使用的翻譯，將其分別翻譯為通用 AI、狹義 AI、廣義 AI。


## 機器學習（Machine Learning，ML）
要讓人工智慧從從概念走向現實，機器學習扮演了至關重要的角色。它是<mark>達成人工智慧的一種方法</mark>，旨在<mark>模仿人類如何從經驗中汲取智慧</mark>。

可以說，機器學習是個從<mark>過往的資料和經驗中學習並找到其運行規則</mark>的過程。它通過**輸入資料**，並經由模型（即是演算法）進行分析和學習。通過大量數據和演算法來**訓練**機器，達成判斷或預測的目的。這個過程可以類比於人腦的學習過程：
<p class="illustration">
    <img src="https://i.imgur.com/moxMqme.jpg" alt="機器學習的過程就是仿照人腦的學習過程">
    機器學習的過程就是仿照人腦的學習過程（圖片來源: <a href="https://codefree.hiskio.com/courses/6">Hiskio Codefree 電腦科學</a>）
</p>

與過往的規則式系統（Rule-based Systems）不同，機器學習無需手動編寫包含特定規則和邏輯的程式來進行判斷或預測；它是從大量樣本中訓練機器辨識規則（即演算法、模型），並利用這些規則進行判斷或預測。

<br class="big"> 

就像上班族遇到不懂的，會糾結到底是要去問人，直接得到明確答案？還是自己慢慢摸索，得出自己的經驗？在機器學習中，也是有這這樣兩種學習方式：**監督式學習（Supervised Learning）** 與 **非監督式學習（Unsupervised Learning）**，它們是根據資料是否已經標註進行分類：
1. **監督式學習**：資料已經標註正確答案，機器根據這些輸入與預期輸出進行學習  
2. **非監督式學習**：沒有標註預期輸出，機器需要從未標註的資料中探索隱藏模式。

兩種方法各有優缺點，監督式學習通常效果較好，但需要大量標註資料，且標註品質會影響訓練結果。因此，後來衍生出了**半監督學習（Semi-supervised Learning）**。

除了常見的監督式和非監督式學習外，還有一種獨特的方法——**強化學習（Reinforcement Learning）**，它不像前兩者需要標註資料，而是通過 **正向回饋（Positive Reward）** 和 **負向回饋（Negative Reward）** 來引導機器調整行為，最終達成預期結果。

我在[〈【機器學習基石筆記】數學基礎 Week3〉中的 Learning with Different Data Label](https://cynthiachuang.github.io/Machine-Learning-Foundations-Study-Notes-Mathematical-Foundations-Week3/#learning-with-different-data-label) 一文中曾介紹過這些學習方式，細節就不再贅述，需要了解的可以過去看，麻煩大家幫忙衝下點閱率了（誤

<p class="illustration">
    <img src="https://i.imgur.com/3eJ3vqZ.png" alt="機器學習大致上可以分為監督式學習、非監督式學習與強化學習三類">
    機器學習大致上可以分為監督式學習、非監督式學習與強化學習三類（圖片來源: <a href="https://ithelp.ithome.com.tw/articles/10217849">iT 邦幫忙</a>）
</p>

<br class="big"> 

那麼，在實際應用中，該如何選擇最適合的機器學習模型呢？這就需要使用者根據具體問題、資料特性以及可能的挑戰，例如是否會出現過擬合（Overfitting）...等情況進行全面評估。

常見的模型有隨機森林、SVM、決策樹、集群、貝葉斯網路、神經網路（Neural Network，NNs）...等。其中**神經網路**是機器學習的一個重要分支，它模仿人腦中神經元的結構，而<mark>多層隱藏層的神經網路又稱為深度學習</mark>，是目前人工智慧發展中最為關鍵的技術。

<p class="illustration">
    <img src="https://i.imgur.com/zmcAUPA.png" alt="深度神經網路或深度學習網路具有數百萬個連結在一起的人工神經元隱藏層。">
    深度神經網路或深度學習網路具有數百萬個連結在一起的人工神經元隱藏層。（圖片來源: <a href="https://aws.amazon.com/tw/what-is/neural-network/">AWS</a>）
</p>


## 深度學習（Deep Learning,，DL）
雖說機器學習為人工智慧的發展奠定了基礎，但對於處理更複雜的問題，例如影像識別或自然語言處理，傳統方法難以達到實際需求。這時，深度學習提供了一條新路徑，嘗試將需要人類智慧的複雜任務自動化。

深度學習是機器學習的一個重要分支，如前述，其核心是構建多層的神經網路，模仿人類大腦的神經元結構，並從海量數據中學習和提取特徵，你可以將它視為一種<mark>實現機器學習的技術</mark>，甚至可以直接將其視為先進的機器學習技術。

<br class="big"> 

雖然兩者的工作流程與目標一致，但在演算法的處理上略有不同。以我在 [Quora](https://www.quora.com/What-is-the-difference-between-deep-learning-and-usual-machine-learning) 上看到的一張辨識車輛的學習流程為例：

<p class="illustration">
    <img src="https://i.imgur.com/GzdqmOc.png" alt="車輛辨識在機器學習與深度學習的差異">
     車輛辨識在機器學習與深度學習的差異 （圖片來源: <a href="https://www.quora.com/What-is-the-difference-between-deep-learning-and-usual-machine-learning">Quora</a>）
</p>

在傳統機器學習中，需要進行**特徵擷取（Feature Extraction）**，然後將資料輸入訓練模型，最後得到預期結果。值得注意的是，所擷取的特徵都是由人類所能理解並定義，如：顏色、品牌...等，畢竟這很大機會是工程師肝出來 XDDD

在圖中可以看到，機器學習需要手動進行特徵擷取，而在深度學習中則省略了這一步。只需將資料輸入模型，模型將自動提取特徵並輸出結果。不同於人工擷取的特徵，深度學習所提取的特徵通常更為抽象且難以理解，這也是深度學習之所以被稱為黑盒子的原因。記得之前嘗試過將模型每一層的輸出視覺化，結果出來了一堆奇奇怪怪的幾何或線條，我都不曉得它到底是看了什麼...當然，也不排除我自己印錯了 XDDD


## 小結
<p class="illustration">
<img src="https://imgur.com/ML56W2V.png" alt="人工智慧發展史">
人工智慧發展史（圖片來源: <a href="https://events.rainfocus.com/widget/nvidia/gtcspring2022/sessioncatalog/session/1642417943266001uxhK">NGC 演講</a>）
</p>

雖然人工智慧這幾年火到快燒掉，但其實，從人工智慧的誕生到神經網路的發展，這並非一個全新的領域。

人工智慧的概念早在 1950 年代就已被提出，當時艾倫．圖靈（Alan Mathison Turing）提出了著名的「圖靈測試」，開啟了人類對機器智慧的探索。至於神經網路與用於模型訓練的反向傳播演算法（Back Propagation），則是在 1980 年代問世。儘管早期由於運算能力的限制，相關研究一度陷入低潮，但 2006 年多層神經網路訓練的突破讓這項技術以「深度學習」的姿態強勢回歸，並隨著資**料蒐集效率的提升**與 **GPU 技術的成熟**，深度學習得以迅速崛起，並逐漸成為人工智慧領域的主流方法。

<div class="alert info"> 
<div class="head">補充一下</div>
如果對歷史科普有興趣，我推薦可以去看看腦洞烏托邦推出的<a href="https://youtu.be/ojnO6hrz1FE?si=elo4ND30bSnagLuB">科普影片</a>，還滿有趣的～👍
</div>

近年來，人工智慧領域最火熱的寵兒——**生成式人工智慧（Generative AI）**，特別是 ChatGPT、DALL·E、Stable Diffusion 等應用的誕生，展現了生成式模型在文本、圖像生成等領域的強大能力，而它的基礎也是**深度學習**。

但隨著 AI 能力增強、巨型模型的出現，也帶來了倫理與法律挑戰，包括隱私問題、偏見與歧視、假資訊泛濫等，需要我們多加注意。因此目前推動人工智慧法案（AI Act），以規範 AI 的設計和使用，確保技術發展的同時符合社會利益。可解釋 AI（Explainable AI，XAI）如何讓人類瞭解人工智慧下判斷的理由。
 
然而，隨著AI能力的增強和巨型模型的出現，也帶來了諸多倫理與法律挑戰，例如隱私問題、偏見與歧視、假資訊泛濫，甚至衍生出 AI 詐騙等議題，這些都需要我們在日常生活多加注意。因此，目前各界積極推動人工智慧法案（AI Act），旨在規範 AI 的設計和使用，確保技術發展與社會利益的和諧。而可解釋 AI（Explainable AI，XAI）的興起，也讓人類能夠更清楚了解人工智慧做出判斷的依據。


## 參考資料 
1. [Codefree - 電腦科學（下）](https://codefree.hiskio.com/courses/6)。檢自 Hiskio (2022-10-07)。
2. iKala Cloud (2021/11/25)。[人工智慧、機器學習、深度學習是什麼? – Machine Learning 教學系列 (一)](https://ikala.cloud/ml-1-ai-ml-deep-learning-intro/)。檢自 技術部落格 (2022-10-07)。
3. Michael Copeland (2016-07-29)。[人工智慧、機器學習與深度學習間有什麼區別?](https://blogs.nvidia.com.tw/2016/07/29/whats-difference-artificial-intelligence-machine-learning-deep-learning-ai/)。檢自 NVIDIA 台灣官方部落格 (2022-10-07)。
4. Samson Tai (2019-03-06)。[淺談 AI人工智能發展里程](https://www.ibm.com/blogs/think/hk-en/2019/03/淺談-ai人工智能發展里程/)。檢自 Medium (2022-10-07)。
5. 阿物科技 (2016-04-19)。[別怕天網成真！關於人工智慧的五個詳解](https://www.inside.com.tw/article/6164-detailed-annotations-of-artificial-intelligence-ai)。檢自 INSIDE (2022-10-07)。
6. Junko Yoshida (2018-06-29)。[邁向廣義AI之路的5大挑戰](https://www.eettaiwan.com/20180629nt01-path-to-broad-ai-challenges/)。檢自 EE Times (2022-10-07)。
7. [從 AI 到量子，未來 5 年科技突破力](https://event.cw.com.tw/2018ibm/article/index2/article7.html)。檢自 IBM藍色觀點｜天下雜誌 (2022-10-07)。
8. [什麼是神經網路？](https://aws.amazon.com/tw/what-is/neural-network/)。檢自 AWS (2024-01-09)。
9. YC Liu (2020-07-19)。[AI 學習筆記 #1: 機器學習 vs. 深度學習](https://medium.com/mr-lius-murmur/ai-學習筆記-i-機器學習-vs-深度學習-98b1ce9123a3)。檢自 YC Liu's Notes｜Medium (2024-01-09)。
10. Lynn (2019-03-22)。 [從人工智慧、機器學習到深度學習，你不容錯過的人工智慧簡史](https://www.inside.com.tw/feature/ai/9854-ai-history)。檢自 INSIDE (2024-01-09)。


## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2025-01-16</summary>
  <ul>
    <li>2025-01-16 更新：更新 ChatGPT 現況</li>
    <li>2024-01-09 完稿</li>
    <li>2021-09-14 起稿</li>
  </ul>
</details>
