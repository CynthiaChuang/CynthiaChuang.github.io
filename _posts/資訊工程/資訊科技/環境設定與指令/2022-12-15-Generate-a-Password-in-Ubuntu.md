---
title: 在 Ubuntu 上生成隨機密碼
date: 2022-12-30
is_modified: false
image: https://i.imgur.com/W5gU1lT.png
categories:
- "資訊科技 › 環境設定與指令"
- "雲端網路 › 資訊安全"
tags:
- Linux/Unix
- 密碼
--- 

從草稿夾挖出來的一篇，我都忘記有這篇了 XDDD
	
之在[〈Ubuntu 上設定密碼規則並強迫使用者更換密碼〉](https://cynthiachuang.github.io/Enforce-Password-Policies-and-Force-User-to-Change-Password-in-Linux/)一文中提過，我們會生成亂數密碼給使用者，然後請使用者第一次登入時修改密碼。而這篇就是生成亂數密碼的部份。

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/W5gU1lT.png" alt="世界密碼日">
    世界密碼日（圖片來源: <a href="https://twgreatdaily.com/4z85DXIBd4Bm1__YOkhy.html">今日頭條</a>）
</p>

在 Linux 中有還滿多工具可以達成這目的，而且生成的密碼複雜度也足夠，還挺方便的。這邊蒐集我們辦公室常用與網路上的方法，這些方法大致可以依是否需要額外安裝分成兩類。



## 原生指令
這區主要列出一些用內建函式庫產生密碼的方法：


### OpenSSL 
這個是我常用的方法，直接調用 OpenSSL 產生隨機密碼。OpenSSL 是一個開放原始碼的軟體函式庫套件，應用程式可以使用這個套件來進行安全通訊，因此幾乎所有 Linux 發行版都包含了它。

<br class="big">

這邊我們使用 base64 對隨機數進行編碼：
```bash
$ openssl rand -base64 10
1JDptRPAC6HGGA==
```
不過我通常用 14，因為它會包含特殊字元：	
```bash
$ openssl rand -base64 14
3/ex+kOSwUNk/8LoIxo=
```

<br class="big">

是說關於最後一個數字參數，[理論](https://www.openssl.org/docs/man1.0.2/man1/openssl-rand.html)上是表示隨機數的長度。但...我產生的隨機數長度從來就不是我指定的長度...這情況根據 [david-wh-2020](https://blog.csdn.net/yaoshixian/article/details/85690958) 所說：
> 因為你生成的位數不足以達到高強度密碼要求，所以預設情況下 OpenSSL 會給你新增字元，使其保證密碼高強度！

<br class="big">

另外一件事，我在用的時候發現最後都會出 `=`，而且還不是隨機出現，是固定最後一定會出等號。所以根據 [hackercat](https://hackercat.org/diy-tools/generate-random-password-from-command-line)  的提示我去查查 [base64 原理](https://stackoverflow.com/questions/6916805/why-does-a-base64-encoded-string-have-an-sign-at-the-end)，原來 byte 數不能被 3 整除時，會在編碼後的 base64 文字上後綴一個或兩個 `=` 號，以補足 byte 數。



### random/urandom
在類 UNIX 系統中有兩隻特殊檔案 `/dev/random` 和 `/dev/urandom`，他們可以分別作為亂數產生器或**偽**亂數產生器。

他們兩者最大的不同是隨機字元取得的不同，`/dev/random` 是會直接從 entropy pool 中不重複挑選以組成隨機數並回傳，這樣產生的亂數是足夠亂的，<mark>會建議用於產生高強度的密碼</mark>。

<br class="big">

但是，這個方式最常遇到的問題是如果 entropy pool 空了，它就會直接卡住，直到 entropy pool 有足夠的 entropy 時，才會產生出密碼。但...直到我寫完這篇我的密碼還沒生出來 ╮（╯＿╰）╭

所以一般會用 `/dev/urandom` 來產生密碼，這邊的 u 指的是 unblocked，它會重複使用 entropy pool 的結果以產生偽亂數，因此它不像 `/dev/random` 會直接卡住。不過相對來說，這樣產生的密碼強度弱於前者，因此<mark>有建議別用於高強度的長期密碼</mark>，但對於我們一開始的目標是足夠用的。

<br class="big">
 
它的指令是用 `/dev/urandom` 產生隨機值，再利用 `tr` 去過濾出指定字元，最後用 `head` 得到指定長度：
```bash
$ strings /dev/urandom |tr -dc _#@A-Z-a-z-0-9-! | head -c 14; echo
AR3Hs86Fqz@Rth
```

<br class="big">

另外，我看過有人會將 `/dev/urandom` 配合 dd 指令來獲取隨機值。
```bash
$ dd if=/dev/urandom bs=1 count=14 | base64 ; echo
14+0 records in
14+0 records out
Lc+GsxecLeV67NVjGy0=
14 bytes copied, 9.2765e-05 s, 151 kB/s
```

不過這組合技我有點搞不清原理， dd 不是用來複製與備份的指令嗎？ 


	
### gpg 
gpg 這套軟體在我的系統中也是被預安裝的工具，它也可以用來產生密碼：

```bash
$ gpg --gen-random --armor 2 14
cZWNei7fvsDaEkmyMMs=
```

其中 `--gen-random` 參數是用來產生隨機變數，`--armor` 則是用來生成 ASCII，14 則是隨機值的長度。

<br class="big">

值得一提的是，`--armor` 後面有接一個數字，那個是 `--gen-random` 的參數，這個值指的是 quality level，它決定了從 `/dev/random` 和  `/dev/urandom` 中讀取的 bytes。

關於這個數值的選擇，我從 [〈gpg --gen-random quality level: is higher "better"?〉](https://crypto.stackexchange.com/questions/30376/gpg-gen-random-quality-level-is-higher-better)這邊看到了討論；簡單來說，<mark>選擇 2 會比 0 或 1</mark>。但如果你忘了填寫會出現錯誤訊息：

```bash
$ gpg --gen-random --armor  12
usage: gpg [options] --gen-random 0|1|2 [count]
```
 

<br class="big">


但關於此命令的使用，我在討論串中看到一個警語：
> 請不要使用此命令，除非您知道自己在做什麼；它可能會從系統中刪除寶貴的 entropy！這可能會給其它程式的隨機生成器帶來問題。

只是我在他說的 [GPG Man Pages](https://www.gnupg.org/documentation/manuals/gnupg/Operational-GPG-Commands.html) 文件找不到他警語的出處。不過從他們另起爐灶的討論串看來，[〈How does generating random numbers "remove entropy from your system"?〉](https://crypto.stackexchange.com/questions/30380/how-does-generating-random-numbers-remove-entropy-from-your-system)，結論是...有疑慮的話就用 `dd` 這個組合技就好。




	
	
### 雜湊演算法
系統中都有內建些雜湊演算法，常用來校驗碼檢查檔案是否正確，我們也可用它們來產生密碼。

1. **md5sum**   
	用於計算與校驗 RFC 1321 所描述的 128 位元 MD5 雜湊值。此方法若想用來產生密碼，會通常直接對時間做 md5 的 hash，為了加強密碼複雜度，會再使用 base64 對隨機數進行編碼，這會產生大小寫英文跟數字，最後一樣用 `head` 取出指定長度：

	```bash
	$ date | md5sum | base64 | head -c 14 ; echo
	ZTA0OWYwZmUyMG
	```

2. **SHA256**  
	SHA-2 下所包含的演算法，用法跟 md5sum 一樣，對 data 做後 hash，後接 base64 編碼：
	```bash
	$ date | sha256sum | base64 | head -c 14 ; echo
	ODU1OWQ0MjJlM2
	```

	同理，其他 SHA 家族的演算法都可以這麼用：
	
	```bash
	$ date | sha1sum | base64 | head -c 14 ; echo
	ZjhlN2Q4ZmRmOG

	$ date | sha256sum | base64 | head -c 14 ; echo
	YmJjMDQ3ZDNkMj

	$ date | sha512sum | base64 | head -c 14 ; echo
	Yzk2ZGZkOTBmZW
	```



## 第三方軟體
如果有需要對產出的密碼做更多的控制的話，可以考慮使用第三方軟體來達成！

### pwgen  
其中使用起來最簡單的大概是 pwgen，安裝也不難。
```bash
$ sudo apt update
$ sudo apt install -y pwgen
```

如果不使用任何選項，單用 `pwgen` 會產生 160 個長度為 8 的隨機值：
```bash
$ pwgen
Geo6Ange Keesh3th ooS6ohki aba7ahSh xus9Ooji vei3ohB4 feedoh8E ica5Ahxe
ohdeel7A vei5iYu4 uhiu9Lee ozieJei8 Eve1dah0 Su8achah IYae5she foh8Asae
ad0AhNg9 Iuladei5 nai7Tewe Aif7eifu TioMohn2 Vea1Xi1A oR1Ieth6 eshaiV8p
OPie2ot2 ahHa0ooc cooHahY8 aPah5ija iCheen7a Feig0phe eegaeN6K Za9aimoh
PohFoH3i ieyooR9g aph3lieS Voh0er1b ieree7Ee Noiph3Je chah1ouS egei7tiY
cicahDi4 Mae4aech bae7Arai thee2Ora uph9Wo2i Uere6iVi Eich5soh mee0ie6E
ooChoo2d Fae7pami wihuJie9 Oghoh2he tahDoH7M Aejee5uy uoCied6i yooShe4c
chee1Uxe aiM1kieZ Fo4AG1ie ae1Ieque Gohph5Ai eCa0aibo Hou4eela eej3kuNg
sohx1EeG Ieghax3f Chi6teeb iy2ooJoo uLu7sohd Jo2Aighu YuzaoL5u Beimang5
Iej1Ahng Ahbei6Ie wu1ooWae AFeme6es cu8Ich4l euj7sheP evoHai9w engeiy9T
keJee7aY Ii5ooriu auJidez3 Wa6ahbah Oche2aeM UlohB1ao eiJ3ohph ooF4eix8
eiN2eiqu ahm4Aqu8 ahSoow4A ui4Quooc aX3meiX8 oov5Equa rahX6ieh kuumeW0f
noh3aGha chai7Oos wi3cahGh Pei2gaeh Red9eeko uu2Ohj3l eiB6As4z aeZelig4
eiS6aroK te9daeSi yei1WieL iS9aej8A AibeiB9a mie5waiV xae3Vee1 Ahngi4ai
WoC5eeke Eiz7Laet IeYee9oo haeG1tha aeS2gieN kaiCh2ph aip2iChi aijooD8f
uDeehah6 joh2Oona ieS5leet sahShao6 aibie6Ei oodah8No uvaNgie4 Miu6ohla
Ziju6Eiz piexaB0s AhX3aeXe Eetha0Ei Osie9aix iMi6gie2 RahL4va5 Zoh8zo5a
eixuaPh7 Uuz4eish ohy1Zegh aepioR8o Oonaif9x daVoh7th Aech4ahr shahgh2T
jahNg0ko Ahpha3ma ahah6Rif Tou9inee eem8Yeit Zienoo3E uozo6Kec Wulae0ho
YahZ7yo3 guloo6Ei tiem4eeM ohchoo7I AhFee3pa eiHai7th aen4Fa3C Yied2aej
```

<br class="big">

預設情況下，pwgen 會嘗試產生較容易記憶的密碼...雖然我是覺得沒有好記多少啦 XDDD，不過若要產生完全隨機的字串可以加上 `-s` 的參數。下面試圖產生 5 個長度為 14 的完全隨機值，至於那個 `-1` 只是為了調整輸出結果，使一排印出一個密碼，可以忽略它： 

```bash
$ pwgen -1 -s 14 5 
66d9n1RUjz40TU
LXSxK9cdXkb7YL
bFjhc9Za0TtPPb
YmYkKey9OfdLZq
sn6yC8TIPPsyT4
```

<br class="big">

如果要加上特殊符號，則加上 `-y` 的參數：

```bash
$ pwgen -1 -s 14 5 -y
GmN%O5hTnc+cU!
x!PHG_:9OGC2Xg
~}Z+"1;DB`.WgS
f^4uGRs*N7I5'B
vuu,2"\jT9~nQy
```


### apg  
是 Automatic Password Generator 的縮寫，一樣可以透過 apt 安裝：

```bash
$ sudo apt update
$ sudo apt install apg
```

先用 `apg` 來看，預設一樣是可發音的密碼生成模式，它還很貼心的附上發音 XDDD 雖然我還是發不出來。

```bash	
$ apg
Aid$Flomen4Ej (Aid-DOLLAR_SIGN-Flom-en-FOUR-Ej)
Rydif?Kav7ovti (Ryd-if-QUESTION_MARK-Kav-SEVEN-ov-ti)
6OticEdCeeb^od (SIX-Ot-ic-Ed-Ceeb-CIRCUMFLEX-od)
hi%FloabHust6 (hi-PERCENT_SIGN-Floab-Hust-SIX)
Rug1twun~ (Rug-ONE-twun-TILDE)
```

<br class="big">

apg 則是透過 `-a` 參數來切換生成密碼的演算法，這邊試圖產生 5 個長度為 14 的完全隨機值： 

```bash		
$ apg -a 1 -n 5 -m 14 
:2A]{0!5~5l*nU
F%vbJ_rC+ik3hN
aY!=/)e!DrozZ[
C:NmvwH*Zlf/};
IcP%w]ZR0)paG<
```


### mkpasswd
這個命令有點~~討厭~~有趣，它在不同系統上工作方式各異，所以表現出的功能也不盡相同。像是在 CentOS/RHEL 的系統上，它是在 `expect` 這個函式庫中；但在 Ubuntu/Debian，則是在 `whois` 中：

```bash
### CentOS 安裝指令
$ yum install expect -y

### Ubuntu 安裝指令
$ apt install whois -y	
```

<br class="big">

從網路上的文章看來，兩者指令所能帶的參數也有所差別，像是 CentOS 可以用 `-l` 來決定生成的長度。不過我沒跑過，所以這僅供參考 XDDD
```bash
$ mkpasswd -l 14
```

反之在 Ubuntu 的版本中，我找不到控制長度的參數，所以它只能輸出固定長度的結果。
```bash
$ mkpasswd 'Thu Dec 15 16:26:42 CST 2022'
k1qs4aXB9/2h2
```
其中，所帶入的參數是要交給它加密的字串，所以我把時間給傳進去了，可惜沒法把 date 指令存給變數再傳給 `mkpasswd`。若不想提供字串也行，連續按兩此 enter 即可。


<br class="big">

因為 Ubuntu 的版本中只能輸出固定長度，但若想得到不同長度的結果，或許雜湊方式可行？
```bash
$ mkpasswd 'Thu Dec 15 16:26:42 CST 2022' -m des
SXI72X7xF.iTM

$ mkpasswd 'Thu Dec 15 16:26:42 CST 2022' -m md5
$1$bnm8frwH$oAqP8WjGS27Gghre1YjCJ0

$ mkpasswd 'Thu Dec 15 16:26:42 CST 2022' -m sha-256
$5$1LWM7JRAWQPBd$jG7FBFm8kquOPRk.yR.BSsu.M44Lc8fWZ0Qi7rrvBA/

$ mkpasswd 'Thu Dec 15 16:26:42 CST 2022' -m sha-512
$6$ObV..84a$60WoH6E1V53sfNf2cff6uCm068QgyXulODVy7RX5XNUPEgxFw38e4.D5LLemzicrifV7P1wet003AslKA0WCM.
```


### makepasswd
我在找 `mkpasswd` 的資料時，意外找到一個名字超級相似的 `makepasswd`，它也是用來產生隨機密碼的。不過跟 `mkpasswd` 不同的是，`mkpasswd` 是用 `crypt` 來產生結果，但是 `makepasswd` 則是使用 `/dev/urandom` 來生成真正的隨機密碼。

```bash
$ sudo apt install makepasswd
$ makepasswd --chars 14 --count 5
X72ChMfHdo4WfU
Wrj7HG7GgBg6Fn
UITvgx7JUef3rY
eSMtqDuEI0nurY
4WIEeazT2EY2eQ
```



## 參考資料 
1. Linux中國 (2019-03-27)。[在 Linux 终端下生成随机/强密码的五种方法](https://zhuanlan.zhihu.com/p/60551109)。檢自 知乎 (2022-05-24)。
2. Kerneltalks (2018-02-06)。[技术｜八种在 Linux 上生成随机密码的方法](https://linux.cn/article-9318-1.html)。檢自 Linux 中国 (2022-05-24)。
3. hackercat (2020-08-23)。[快速產生亂數隨機密碼](https://hackercat.org/diy-tools/generate-random-password-from-command-line)。檢自 駭客貓咪 HackerCat (2022-11-03)。
4. 亮子介 (2022-05-25)。[Ubuntu 下使用 APG 生成密码](https://blog.csdn.net/henryhu712/article/details/81381950)。檢自 亮子介的博客｜CSDN博客 (2022-11-03)。
5. david-wh-2020 (2019-01-03)。[openssl rand 密码字符长度 -base64](https://blog.csdn.net/yaoshixian/article/details/85690958)。檢自 david-wh-2020的博客｜CSDN博客 (2022-11-03)。
6. 協同撰寫。[/dev/random](https://zh.wikipedia.org/zh-hk//dev/random)。檢自 維基百科 (2022-11-03)。
7. GTWang (2015-01-27)。[dd 指令教學與實用範例，備份與回復資料的小工具](https://blog.gtwang.org/linux/dd-command-examples/)。檢自 G. T. Wang (2022-12-15)。
8. hkNaruto (2020-06-17)。[linux dd urandom 生成指定大小随机内容文件](https://blog.csdn.net/hknaruto/article/details/106809808)。檢自 hkNaruto的博客｜CSDN博客 (2022-12-15)。
9. HYL (2018-12-03)。[dd命令的高級應用](http://simple-is-beauty.blogspot.com/2018/12/dd.html)。檢自 簡單.減嘆 (2022-12-15)。
10. 協同撰寫。[Base64](https://zh.wikipedia.org/zh-tw/Base64)。檢自 維基百科 (2022-12-15)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-12-30</summary>
  <ul>
    <li>2022-12-30 發布</li>
    <li>2022-12-15 完稿</li>
    <li>2022-05-24 起稿</li>
  </ul>
</details>