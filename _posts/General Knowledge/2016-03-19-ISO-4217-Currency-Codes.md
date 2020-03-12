---
title: 【知識】貨幣代碼規則 - ISO 4217
date: 2016-03-19
categories:
- General-Knowledge
tags:
--- 

前一陣子在實作匯率換算的 APP 時候，有注意到從 Yahoo API 上拉下來的全是貨幣代碼，對於不熟悉的人很難知道，這三個字母到底對應的是哪種貨幣。
 
被S老大一問三不知之後，只好摸摸鼻子去想辦法懂這些代碼規則 - **ISO 4217**。別一臉鄙視...至少我還知道 TWD 是新台幣，好過 Designer 把 TWD 拼成 TMD ...，還是她其實在罵我...

<!--more-->
<br> 

## ISO 4217

所謂的 ISO 4217 是由國際標準化組織制定的國際標準，用以表示貨幣或資金名稱及基金的代碼。ISO 4217 代碼在全球的銀行和企業中均會使用。

在許多的國家和地區，報紙和銀行在發布匯率時都會直接使用 ISO代碼，而非將貨幣名稱進行翻譯或者使用貨幣符號。ISO 4217 代碼也會在機票和國際專列上用以表示價格。

嗯…以上是取自維基百科的解釋，我只要知道這組代碼是國際通用，且知道怎查詢幣別名就好 XDDDD


<br><br> 

## Yahoo API 所有幣別對應表

在實作 [Yahoo API 取回所有國家貨幣資料](/Getting-Currency-Exchange-Rates-Using-Yahoo-API/)時，會得到 173 筆資料，這裡稍微整理了一下這173個代碼和所對應的幣別名（如表1），不過只有英文版的，中文的實在有點想偷懶了XDDD

P.S. 說不想整理，結果最後還是把它弄出來了

<br>
<center>表1、代碼和所對應的幣別名</center>
<br>

|     代碼           |幣別名|幣別名(中)|
|----------------|-------------------------------|-----------------------------|
|AED|United Arab Emirates dirham|阿聯迪拉姆|
|AFN|Afghan afghani|阿富汗阿富汗尼|
|ALL|Albanian lek|阿爾巴尼亞列克|
|AMD|Armenian dram|亞美尼亞德拉姆|
|ANG|Netherlands Antillean guilder|荷屬安的列斯盾|
|AOA|Angolan kwanza|安哥拉寬扎|
|ARS|Argentine peso|阿根廷比索|
|AUD|Australian dollar|澳大利亞元|
|AWG|Aruban florin|阿魯巴弗羅林|
|AZN|Azerbaijani manat|亞塞拜然馬納特|
|BAM|Bosnia and Herzegovina convertible mark|波士尼亞赫塞哥維納可兌換馬克|
|BBD|Barbados dollar|巴貝多元|
|BDT|Bangladeshi taka|孟加拉塔卡|
|BGN|Bulgarian lev|保加利亞列弗|
|BHD|Bahraini dinar|巴林戴納|
|BIF|Burundian franc|蒲隆地法郎|
|BMD|Bermudian dollar|百慕達元
|BND|Brunei dollar|汶萊元|
|BOB|Boliviano|玻利維亞諾|
|BRL|Brazilian real|巴西雷亞爾|
|BSD|Bahamian dollar|巴哈馬元|
|BTN|Bhutanese ngultrum|不丹努爾特魯姆|
|BWP|Botswana pula|波札那普拉|
|BYR|Belarusian ruble|白俄羅斯盧布|
|BZD|Belize dollar|貝里斯元|
|CAD|Canadian dollar|加拿大元|
|CDF|Congolese franc|剛果法郎
|CHF|Swiss franc|瑞士法郎
|CLF|Unidad de Fomento (funds code)|UF值（資金代碼）|
|CLP|Chilean peso|智利比索|
|CNH|Chinese yuan (when traded offshore)|人民幣元（離岸）|
|CNY|Chinese yuan|人民幣元|
|COP|Colombian peso|哥倫比亞比索|
|CRC|Costa Rican colon|哥斯大黎加科郎|
|CUP|Cuban peso|古巴比索|
|CVE|Cape Verde escudo|維德角埃斯庫多|
|CYP|Cypriot pound|賽普勒斯鎊|
|CZK|Czech koruna|捷克克朗|
|DEM|German mark|德國馬克|
|DJF|Djiboutian franc|吉布地法郎|
|DKK|Danish krone|丹麥克朗|
|DOP|Dominican peso|多米尼加比索|
|DZD|Algerian dinar|阿爾及利亞戴納|
|ECS|Ecuadorian sucre|厄瓜多蘇克雷|
|EGP|Egyptian pound|埃及鎊|
|ERN|Eritrean nakfa|厄利垂亞奈克法|
|ETB|Ethiopian birr|衣索比亞比爾|
|EUR|Euro|歐元|
|FJD|Fiji dollar|斐濟元|
|FKP|Falkland Islands pound|福克蘭群島鎊|
|FRF|French franc|法國法郎|
|GBP|Pound sterling|英鎊|
|GEL|Georgian lari|喬治亞拉里|
|GHS|Ghanaian cedi|加納塞地|
|GIP|Gibraltar pound|直布羅陀鎊|
|GMD|Gambian dalasi|甘比亞達拉西|
|GNF|Guinean franc|幾內亞法郎|
|GTQ|Guatemalan quetzal|瓜地馬拉格查爾|
|GYD|Guyanese dollar|蓋亞那元|
|HKD|Hong Kong dollar|港元|
|HNL|Honduran lempira|宏都拉斯倫皮拉|
|HRK|Croatian kuna|克羅埃西亞庫納|
|HTG|Haitian gourde|海地古德|
|HUF|Hungarian forint|匈牙利福林|
|IDR|Indonesian rupiah|印尼盾|
|IEP|Irish pound (punt in Irish language)|愛爾蘭鎊|
|ILS|Israeli new shekel|以色列塞克|
|INR|Indian rupee|印度盧比|
|IQD|Iraqi dinar|伊拉克戴納|
|IRR|Iranian rial|伊朗里亞爾|
|ISK|Icelandic króna|冰島克朗|
|ITL|Italian lira|義大利里拉|
|JMD|Jamaican dollar|牙買加元|
|JOD|Jordanian dinar|約旦戴納|
|JPY|Japanese yen|日圓|
|KES|Kenyan shilling|肯亞先令|
|KGS|Kyrgyzstani som|吉爾吉斯斯坦索姆|
|KHR|Cambodian riel|柬埔寨瑞爾|
|KMF|Comoro franc|葛摩法郎|
|KPW|North Korean won|朝鮮圓|
|KRW|South Korean won|韓圓|
|KWD|Kuwaiti dinar|科威特戴納|
|KYD|Cayman Islands dollar|開曼群島元|
|KZT|Kazakhstani tenge|哈薩克斯坦堅戈|
|LAK|Lao kip|寮國基普|
|LBP|Lebanese pound|黎巴嫩鎊|
|LKR|Sri Lankan rupee|斯里蘭卡盧比|
|LRD|Liberian dollar|賴比瑞亞元|
|LSL|Lesotho loti|賴索托洛蒂|
|LTL|Lithuanian litas|立陶宛立特|
|LVL|Latvian lats|拉脫維亞拉特|
|LYD|Libyan dinar|利比亞戴納|
|MAD|Moroccan dirham|摩洛哥迪爾汗|
|MDL|Moldovan leu|摩爾多瓦列伊|
|MGA|Malagasy ariary|馬達加斯加阿里亞里|
|MKD|Macedonian denar|馬其頓代納爾|
|MMK|Myanmar kyat|緬元|
|MNT|Mongolian tögrög|蒙古圖格里克|
|MOP|Macanese pataca|澳門幣|
|MRO|Mauritanian ouguiya|茅利塔尼亞烏吉亞|
|MUR|Mauritian rupee|模里西斯盧比|
|MVR|Maldivian rufiyaa|馬爾地夫拉菲亞|
|MWK|Malawian kwacha|馬拉威克瓦查|
|MXN|Mexican peso|墨西哥比索|
|MXV|Mexican Unidad de Inversion (UDI) (funds code)|墨西哥發展單位（UDI，資金代碼）|
|MYR|Malaysian ringgit|馬來西亞令吉|
|MZN|Mozambican metical|莫三比克梅蒂卡爾|
|NAD|Namibian dollar|納米比亞元|
|NGN|Nigerian naira|奈及利亞奈拉|
|NIO|Nicaraguan córdoba|尼加拉瓜科多巴|
|NOK|Norwegian krone|挪威克朗|
|NPR|Nepalese rupee|尼泊爾盧比|
|NZD|New Zealand dollar|紐西蘭元|
|OMR|Omani rial|阿曼里亞爾|
|PAB|Panamanian balboa|巴拿馬巴波亞|
|PEN|Peruvian Sol|秘魯索爾|
|PGK|Papua New Guinean kina|巴布亞紐幾內亞基那|
|PHP|Philippine peso|菲律賓披索|
|PKR|Pakistani rupee|巴基斯坦盧比|
|PLN|Polish złoty|波蘭茲羅提|
|PYG|Paraguayan guaraní|巴拉圭瓜拉尼|
|QAR|Qatari riyal|卡達里亞爾|
|RON|Romanian leu|羅馬尼亞列伊|
|RSD|Serbian dinar|塞爾維亞戴納|
|RUB|Russian ruble|俄羅斯盧布|
|RWF|Rwandan franc|盧安達法郎|
|SAR|Saudi riyal|沙特里亞爾|
|SBD|Solomon Islands dollar|索羅門群島元|
|SCR|Seychelles rupee|塞席爾盧比|
|SDG|Sudanese pound|蘇丹鎊|
|SEK|Swedish krona/kronor|瑞典克朗|
|SGD|Singapore dollar|新加坡元|
|SHP|Saint Helena pound|聖赫倫那鎊|
|SIT|Slovenian tolar|斯洛維尼亞托拉爾|
|SLL|Sierra Leonean leone|獅子山利昂|
|SOS|Somali shilling|索馬利亞先令|
|SRD|Surinamese dollar|蘇利南元|
|STD|São Tomé and Príncipe dobra|聖多美和普林西比多布拉|
|SVC|Salvadoran colón|薩爾瓦多科郎|
|SYP|Syrian pound|敘利亞鎊|
|SZL|Swazi lilangeni|史瓦濟蘭里蘭吉尼|
|THB|Thai baht|泰銖|
|TJS|Tajikistani somoni|塔吉克斯坦索莫尼|
|TMT|Turkmenistani manat|土庫曼斯坦馬納特|
|TND|Tunisian dinar|突尼西亞戴納|
|TOP|Tongan paʻanga|東加潘加|
|TRY|Turkish lira|土耳其里拉|
|TTD|Trinidad and Tobago dollar|特立尼達和多巴哥元|
|TWD|New Taiwan dollar|新台幣元|
|TZS|Tanzanian shilling|坦尚尼亞先令|
|UAH|Ukrainian hryvnia|烏克蘭格里夫納|
|UGX|Ugandan shilling|烏干達先令|
|USD|United States dollar|美元|
|UYU|Uruguayan peso|烏拉圭比索|
|UZS|Uzbekistan |烏茲別克斯坦索姆|
|VEF|Venezuelan bolívar|委內瑞拉玻利瓦爾|
|VND|Vietnamese dong|越南盾|
|VUV|Vanuatu vatu|萬那杜瓦圖|
|WST|Samoan tala|薩摩亞塔拉|
|XAF|CFA franc BEAC|中非法郎|
|XAG|Silver (one troy ounce)|銀（1金衡盎司）|
|XAU|Gold (one troy ounce)|金（1金衡盎司）|
|XCD|East Caribbean dollar|東加勒比元|
|XDR|Special drawing rights|特別提款權|
|XOF|CFA franc BCEAO|西非法郎|
|XPD|Palladium (one troy ounce)|鈀（1金衡盎司）|
|XPF|CFP franc (franc Pacifique)|太平洋法郎（franc Pacifique）|
|XPT|Platinum (one troy ounce)|鉑（1金衡盎司）|
|YER|Yemeni rial|葉門里亞爾|
|ZAR|South African rnd|南非蘭特|
|ZMK|Zambian kwacha|尚比亞克瓦查|
|ZMW|Zambian kwacha|尚比亞克瓦查|
|ZWL|Zimbabwean dollar A/10|辛巴威元A/10


<br> 另一種比較常見的做法是用 [Currency](https://docs.oracle.com/javase/8/docs/api/java/util/Currency.html) 所提供的類別來實作，只是這種做法我認為存在兩個缺點：
1. 無法送翻譯，實現多國語系。
2. 取得貨幣數少，Yahoo 目前還有提供一些已經不流通貨幣（e.x.法國法郎）或貴金屬的匯率查詢，而 Currency 無法取得。


<br><br> 

## 參考資料 
1. [ISO 4217｜維基百科](https://zh.wikipedia.org/wiki/ISO_4217)