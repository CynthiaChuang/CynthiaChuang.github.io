---
title: 【LaTeX】 常用 LaTeX 數學符號指令
date: 2019-01-05
categories:
- Toolkit
- Computer-Language/Framework
tags:
- LaTeX
--- 

整理一些我自己用過的 LaTex  數學符號指令，下次就不用在像無投蒼蠅般亂找了 XD

不過我的使用情境多數是在寫網誌時，插入公式用，偶爾寫寫課堂上的證明題，比較少拿它來寫文件，所以缺少關於排版部分的語法，只在文章後面稍微提到了點公式的排版。

是說，後面部分有點亂，是不是應該把這篇拆成兩篇
<!--more-->
<br>

## 目錄
1.  [二元運算號](#二元運算號)
2.  [二元關係符號](#二元關係符號)
3.  [邏輯符號](#邏輯符號)
4.  [上標、下標及Head](#上標、下標及Head)
5.  [大尺寸運算符號](#大尺寸運算符號)
6.  [標準函式符號](#標準函式符號)
7. [根號與分數](#根號與分數)
8. [二項式係數](#二項式係數)
9.  [箭號](#箭號)
10.  [分隔符號](#分隔符號)
11.  [希臘字母](#希臘字母)
12.  [陣列與方程式](#陣列與方程式)
13.  [空格](#空格)
14.  [書寫時, 英文字體顯示](#書寫時-英文字體顯示)
15.  [公式對齊](#公式對齊)

<br> 
> **Warning**<br>
> 下列指令前後應該皆用 **\$\$** 包圍，告訴編譯器這邊是應該用 mathjax 來渲染。<br>
>  `$$ 指令 $$`



<br><br>

## 二元運算號

|預覽|指令|.|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|---|---|---|
|$+$|+||$-$|-||$\pm$|\pm||$\mp$|\mp|
|$\times$|\times||$\ast$|\ast||$\star$|\star||$\cdot$|\cdot|
|$\div$|\div||$\setminus$|\setminus|
|$\oplus$|\oplus||$\ominus$|\ominus||$\otimes$|\otimes||$\odot$|\odot|
|$\oslash$|\oslash||$\bigcirc$|\bigcirc|
|$\vee$|\vee||$\wedge$|\wedge||$\uplus$|\uplus|
|$\circ$|\circ||$\bullet$|\bullet||$\diamond$|\diamond|
$\triangleright$|\triangleright||$\triangleleft$| \triangleleft|

<br><br>

## 二元關係符號

|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|
|$\not <$|<||$>$|>||$=$|=|
|$\le$|\le||$\ge$|\ge||$\equiv$|\equiv|
|$\ll$|\ll||$\gg$|\gg||$\doteq$|\doteq|
|$\prec$|\prec||$\succ$|\succ||$\sim$|\sim|
|$\preceq$|\preceq||$\succeq$|\succeq||$\simeq$|\simeq|
|$\subset$|\subset||$\supset$|\supset||$\approx$|\approx|
|$\subseteq$|\subseteq||$\supseteq$|\supseteq||$\cong$|\cong|
|$\sqsubset$|\sqsubset||$\sqsupset$|\sqsupset||$\Join$|\cong|
|$\sqsubseteq$|\sqsubseteq||$\sqsupseteq$|\sqsupseteq||$\bowtie$|\bowtie|
|$\in$|\in||$\ni$|\ni||$\propto$|\propto|
|$\vdash$|\vdash||$\dashv$|\dashv||$\models$|\models|
|$\perp$|\perp||$\mid$|\mid||$\parallel$|\parallel|
|$\smile$|\smile||$\frown$|\frown||$\asymp$|\asymp|

可在前面加上 \not 得到否定形式，ex：`\not <` 得到 $\not <$




<br><br>

## 邏輯符號

|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---| 
|$\forall$|\forall||$\exists$|\exists||$\nexists$|\nexists| 
|$\therefore$|\therefore||$\because$|\because||$\And$|\And| 
|$\lor$|\lor||$\vee$|\vee||$\curlyvee$|\curlyvee|
|$\bigvee$|\bigvee|
|$\land$|\land||$\wedge$|\wedge||$\curlywedge$|\curlywedge|
|$\bigwedge$|\bigwedge|

<br><br>

## 上標、下標及Head

|預覽|指令|.|預覽|指令|
|---|---|---|---|---|
|$a^b$|a^b||$a_t$|a_t|
|$a^b_t$|a^b_t||$a^{b+1}_{t+1}$|a^{b+1}_{t+1}|
|$\overline{m+n}$|\overline{m+n}||$\underline{m+n}$|\underline{m+n}|
|$\overbrace{m+\cdots+n}^{26}$|\overbrace{m+\cdots+n}^{26}||$\underbrace{m+\cdots+n}_{26}$|\underbrace{m+\cdots+n}_{26}|
|$\bar{a}$|\bar{a}||$\vec{a}$|\vec{a}|
|$\hat{a}$|\hat{a}||$\dot{a}$|\dot{a}|
|$\overrightarrow{ab}$|\overrightarrow{ab}||$\overleftarrow{ab}$|\overleftarrow{ab}|
|$\widehat{abc}$|\widehat{abc}||$\overset{\frown} {ab}$|\overset{\frown} {ab}|

<br><br>

## 大尺寸運算符號

|預覽|指令|.|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|---|---|---|
|$\sum$|\sum||$\bigcap$|\bigcap||$\bigcup$|\bigcup||$\biguplus$|\biguplus|
|$\bigsqcup$|\bigsqcup||$\prod$|\prod||$\coprod$|\coprod||$\bigodot$|\bigodot|
|$\bigotimes$|\bigotimes||$\bigoplus$|\bigoplus||$\bigwedge$|\bigwedge||$\bigvee$|\bigvee|
|$\int$|\int||$\oint$|\oint|

<br><br>

## 標準函式符號

|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|
|$\sin$|\sin||$\cos$|\cos||$\tan$|\tan| 
|$\sec$|\sin||$\csc$|\csc||$\cot$|\cot| 
|$\arcsin$|\arcsin||$\arccos$|\arccos||$\arctan$|\arctan| 
|$\sinh$|\sinh||$\cosh$|\cosh||$\tanh$|\tanh|
|$\exp$|\exp||$\log$|\log||$\ln$|\ln|
|$\lim$|\lim||$\liminf$|\liminf||$\limsup$|\limsup|
|$\inf$|\inf||$\max$|\max||$\min$|\min|
|$\deg$|\deg||$\arg$|\arg||$\gcd$|\gcd|

<br><br>

## 根號與分數

|預覽|指令|.|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|---|---|---|
|$\surd$|\surd||$\sqrt{2}$|\sqrt{2}||$\sqrt[n]{}$|\sqrt[n]{}||$\sqrt[3]{2}$|\sqrt[3]{2}|
|$\frac{2}{4}$|\frac{2}{4}||$\tfrac{2}{4}$|\tfrac{2}{4}||$\cfrac{2}{4}$|\cfrac{2}{4}|

<br><br>

## 二項式係數

|預覽|指令|.|預覽|指令|
|---|---|---|---|---|
|$\dbinom{n}{r}$|\dbinom{n}{r}||$\binom{n}{n-r}$|\binom{n}{n-r} |

<br><br>

## 箭號

|預覽|指令|.|預覽|指令|
|---|---|---|---|---|
|$\leftarrow$|\leftarrow||$\rightarrow$|\rightarrow|
|$\longleftarrow$|\longleftarrow||$\longrightarrow$|\longrightarrow|
|$\Leftarrow$|\Leftarrow||$\Rightarrow$|\Rightarrow|
|$\Longleftarrow$|\Longleftarrow||$\Longrightarrow$|\Longrightarrow|
|$\leftharpoonup$|\leftharpoonup||$\rightharpoonup$|\rightharpoonup|
|$\leftharpoondown$|\leftharpoondown||$\rightharpoondown$|\rightharpoondown|
|$\hookleftarrow$|\hookleftarrow||$\hookrightarrow$|\hookrightarrow|
|$\mapsto$|\mapsto||$\longmapsto$|\longmapsto|
|$\leftrightarrow$|\leftrightarrow||$\longleftrightarrow$|\longleftrightarrow|
|$\Leftrightarrow$|\Leftrightarrow||$\Longleftrightarrow$|\Longleftrightarrow|
|$\rightleftharpoons$|\rightleftharpoons| 
|$\uparrow$|\uparrow||$\downarrow$|\downarrow|
|$\Uparrow$|\Uparrow||$\Downarrow$|\Downarrow|
|$\updownarrow$|\updownarrow||$\Updownarrow$|\Updownarrow|
|$\nearrow$|\nearrow||$\searrow$|\searrow|
|$\swarrow$|\swarrow||$\nwarrow$|\nwarrow|


<br><br>

## 分隔符號

|預覽|指令|.|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|---|---|---|
|$($|(||$)$|)||$[$|[||$]$|]|
|$\{$|\\{||$\}$|\\}||$\langle$|\langle||$\rangle$|\rangle|
|$\lfloor$|\lfloor||$\rfloor$|\rfloor||$\lceil$|\lceil||$\rceil$|\rceil|
|$\vert$|\vert||$\Vert$|\Vert||$/$|/||$\backslash$|\backslash|

<br><br>
## 希臘字母

**大寫字母**

|預覽|指令|.|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|---|---|---|
|$\Gamma$|\Gamma||$\Delta$|\Delta||$\Lambda$|\Lambda||$\Xi$|\Xi|
|$\Pi$|\Pi||$\Sigma$|\Sigma||$\Upsilon$|\Upsilon||$\Phi$|\Phi|
|$\Psi$|\Psi||$\Omega$|\Omega|

<br>**小寫字母**

|預覽|指令|.|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|---|---|---|
|$\alpha$|\alpha||$\beta$|\beta||$\gamma$|\gamma||$\delta$|\delta|
|$\epsilon$|\epsilon||$\varepsilon$|\varepsilon||$\zeta$|\zeta||$\eta$|\eta|
|$\theta$|\theta||$\vartheta$|\vartheta||$\iota$|\iota||$\kappa$|\kappa|
|$\lambda$|\lambda||$\mu$|\mu||$\nu$|\nu||$\xi$|\xi|
|$o$|o||$\pi$|\pi||$\varpi$|\varpi||$\rho$|\rho|
|$\varrho$|\varrho||$\sigma$|\sigma||$\varsigma$|\varsigma||$\tau$|\tau|
|$\upsilon$|\upsilon||$\phi$|\phi||$\varphi$|\varphi||$\chi$|\chi|
|$\psi$|\psi||$\omega$|\omega|
 
<br><br>
## 陣列與方程式

**陣列**

$$\begin{matrix}
a+b & c     & 9   \\
5   & x^2+y & 3   \\
8   & 7     & z_1 \\
\end{matrix}$$

指令
```
\begin{matrix}
a+b & c     & 9   \\
5   & x^2+y & 3   \\
8   & 7     & z_1 \\
\end{matrix}
```
.

$$\begin{vmatrix}
x & y \\
z & v
\end{vmatrix}$$

指令

```
\begin{vmatrix}
x & y \\
z & v
\end{vmatrix}
```

.

**真值表**

$$\begin{array}{|c|c||c|} a & b & S \\
\hline
0&0&1\\
0&1&1\\
1&0&1\\
1&1&0\\
\end{array}$$

```
\begin{array}{|c|c||c|} a & b & S \\
\hline
0&0&1\\
0&1&1\\
1&0&1\\
1&1&0\\
\end{array}
```

.

**方程組**

$$\begin{cases}
3x + 5y +  z \\
7x - 2y + 4z \\
-6x + 3y + 2z
\end{cases}$$

指令
```
\begin{cases}
3x + 5y +  z \\
7x - 2y + 4z \\
-6x + 3y + 2z
\end{cases}
```

<br><br>

## 空格

|效果說明|指令|預覽|.|效果說明|指令|預覽|.|效果說明|指令|預覽|
|---|---|---|---|---|---|---|---|---|---|---|
| 大空格 | \ |  $a\  b$| | 兩個quad空格|\qquad | $a\qquad b$|| 没有空格||  $ab$|
|中空格 | \\; |$a\;b$|| 一個quad空格 |\quad|$a\quad b$ ||  緊貼 | \\! | $a\!b$|
|小空格 |\\, |$a\,b$ || 換行|\\\\|$a\\ b$

<br><br>

## 書寫時, 英文字體顯示

|預覽|指令|.|預覽|指令|.|預覽|指令|.|預覽|指令|
|---|---|---|---|---|---|---|---|---|---|---|
|$if$|if||$\text{ if }$|\text{ if } ||$\mathbb{R}$|\mathbb{R}||$\mathrm{C}$|\mathrm{C}|

<br><br>

## 公式對齊

$$
\begin{aligned}		
	a &= b + c \\
	   &= d  +f 
\end{aligned}
$$	 

指令
```
\begin{aligned}		
	a &= b + c \\
	   &= d  +f 
\end{aligned}
```

<br><br> 

## 參考資料
1.  [TeX / LaTeX 數學模式符號指令](http://bcc16.ncu.edu.tw/7/latex/math_tex/)
2. [常用数学符号的 LaTeX 表示方法](http://mohu.org/info/symbols/symbols.htm)
3.  [使用說明:數學公式｜維基百科](https://zh.wikipedia.org/wiki/Help:%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F#%E9%80%BB%E8%BE%91%E7%AC%A6%E5%8F%B7)
4. [Latex数学公式中的空格｜王志 新浪博客](
http://blog.sina.com.cn/s/blog_4ddef8f80100iwwv.html)
5. [【LaTeX入门】05、换行、换段、换页、首行缩进等命令｜xiazdong](
https://blog.csdn.net/xiazdong/article/details/8892105)