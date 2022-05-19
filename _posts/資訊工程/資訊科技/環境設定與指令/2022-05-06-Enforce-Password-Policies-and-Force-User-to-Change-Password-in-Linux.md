---
title: "在 Ubuntu 上設定密碼規則並強迫使用者更換密碼"
date: 2022-05-06 02:18
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
- "雲端網路 › 資訊安全"
tags:
- Linux/Unix
- 密碼
--- 

剛剛看到 google 提示今天是**世界密碼日**，當下決定來發篇跟密碼相關的網誌。所以翻出之前在搞伺服器時留下來的筆記，筆記內容 IT 也貢獻了不少，不過內容雜亂無序，希望最後整理完的時候今天還沒有過 XD
  
這篇要做事情有二：
1. 設定密碼複雜度
2. 強迫使用者登入更換密碼


<!--more-->
<br>

<center> <img src="https://i.imgur.com/W5gU1lT.png" alt="世界密碼日"></center>
<center  class="imgtext">世界密碼日（圖片來源: <a href="https://twgreatdaily.com/4z85DXIBd4Bm1__YOkhy.html"  class="imgtext">今日頭條</a>）</center>
<br>


## 設定密碼複雜度

你知道每年五月的第一個星期四是**世界密碼日**嗎？很好，我也不知道，直到今天才知道 XD 反正這個節日就是提醒你關心下自己的密碼是否安全。

根據 Microsoft 的[密碼原則建議](https://docs.microsoft.com/zh-tw/microsoft-365/admin/misc/password-policy-recommendations?view=o365-worldwide)可以朝兩個方向強化密碼強度：
1. **密碼複雜度**：  
   設定密碼規則，組成應包含大寫、小寫、數字、特殊符號跟最小長度。像我們公司 AD 帳號就密碼長度至少 15 字元以上，且要求至少包含數字、大寫、小寫、特殊符號。WTF...密碼越來越長了啦...。
   
2. **更換密碼天數**：  
   設定幾天後強制使用者換密碼，如 90 天一換。

<br>

### Step1、安裝套件

在 Ubuntu 中密碼強度檢查的功能，是透過 `libpam-pwquality` 這個套件來強制設定密碼的最低複雜度，以避免弱密碼的出現。所以一開始需先安裝該套件：

```
$ sudo apt install libpam-pwquality
```

<br>

### Step2、設定密碼複雜度

安裝完後，編輯 `/etc/pam.d/common-password` 設定檔來設定密碼複雜度

```
$ vim /etc/pam.d/common-password
# here are the per-package modules (the "Primary" block)
password   requisite   pam_pwquality.so retry=3
password	[success=1 default=ignore]	pam_unix.so obscure use_authtok try_first_pass sha512
...
```

<br>

找到以下這一行 pam_pwquality.so 的設定值：
```
password   requisite   pam_pwquality.so ....
```

我這邊直接把它改成：
```
password   requisite   pam_pwquality.so retry=3 minlen=15 maxrepeat=3 difok=4 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1 reject_username enforce_for_root
```

<br>

其中：

1. **`retry=3`**  
    代表輸入密碼時最多嘗試次數，當密碼輸入超過 `3` 次就會產生錯誤。

2. **`minlen=15`**  
    設定最短的密碼長度為 `15`。

3. **`maxrepeat=3`**   
   密碼中相同的字元最多只能連續出現 `3` 個，若超過則密碼就會被拒用，例如：`maxrepeat=3`，表示不接受 `pwwwwd` 為密碼。如果設為 `0`，則不檢查。

4. **`difok=4`**  
   新密碼中必須有 `4` 個字元要與舊密碼不同。若設定為 `0` 則不檢查，只要新舊密碼不要完全一樣即可。

5. **`ucredit=-1`**、**`lcredit=-1`**、**`dcredit=-1`**、**`ocredit=-1`**  
   這邊就是密碼複雜度的主要控制部分。嚴格來說，前面的 `minlen` 並不完全是密碼最小長度，而是密碼最少應得貢獻值。貢獻值的計算是每個字元得 `1` 分，其中若字元為特定類別，會有額外貢獻，但這些貢獻會有上限的，舉例來說：
   - **`ucredit=3`**、**`lcredit=2`**、**`dcredit=1`**、**`ocredit=0`**  
     ucredit、lcredit、dcredit、ocredit 分別對應到**大寫**、**小寫**、**數字**、**特殊符號**四個類別，其值則為其類別貢獻值上限。
     
     舉例來說，一密碼為 `$$$$$Pwd012`，其長度為 `11`，其中包含 `1` 個大寫、`2` 個小寫、`3` 個數字、`4` 個特殊符號，但因數字貢獻值上限為 `1`，特殊符號不計算貢獻值，因此最終貢獻值為 $11+1+2+1+0=15$。因最終貢獻值等於最低貢獻值 `15`，故雖長度不及 `15`，但仍是符合所設定的複雜度。
     
   - **`ucredit=0`**、**`lcredit=0`**、**`dcredit=0`**、**`ocredit=0`**  
     這個例子就是所有類別都不算額外貢獻，直接看密碼長度。
     
   - **`ucredit=-1`**、**`lcredit=-1`**、**`dcredit=-1`**、**`ocredit=-1`**  
     如果覺得計算貢獻值太複雜，則可以用負數直接指定該類別所想要的數量。以這個例子來說，就是密碼中必須包含大寫、小寫、數字、特殊符號各一個字元。
 
6. **`reject_username`**  
   拒絕包含正、反向的 username。
   
7. **`enforce_for_root`**  
   設定 root 管理者亦需遵守密碼規定。說實話管理者往往是最不遵守密碼規定的 XD
   

<br>

### Step3、禁止使用過往密碼

另一個~~討厭~~常見的功能是禁止使用過往用過密碼，這功能一樣透過編輯 `/etc/pam.d/common-password` ，找到以下這一行 pam_unix.so 的設定值：
```
$ password	[success=1 default=ignore]	pam_unix.so obscure use_authtok try_first_pass sha512
```

在尾端加上 `remember=3`，表不與前三次密碼相同：
```
$ password	[success=1 default=ignore]	pam_unix.so obscure use_authtok try_first_pass sha512 remember=5
```

<br><br>

## 設定更換密碼天數

強化密碼強度的另一個方向則是設定密碼有效期限，迫使使用者定期更新密碼。

<br>

### Step1、設定密碼有效期限  

可透過編輯 `/etc/login.defs` 設定密碼有效期限：

```
# vim /etc/login.defs
PASS_MAX_DAYS   90
PASS_MIN_DAYS   10
PASS_MIN_LEN    15
PASS_WARN_AGE   7
```

其中

1. **`PASS_MAX_DAYS`**：密碼效期，以天計算。
2. **`PASS_MIN_DAYS`**：兩次密碼更換至少間隔天數。
3. **`PASS_MIN_LEN`**：密碼最小長度。
4. **`PASS_WARN_AGE`**：密碼到期前幾天通知使用者。
 
若時間的變動超過 `PASS_MAX_DAYS`，則登入時會要求使用者更新密碼並遵循新的密碼規則。但必須注意的是，這裡設定只會**套用在之後新建的帳號**。若要修改使現有使用者，需另外使用 `chage` 指令來進行變更。

<br>

### Step2、設定現有使用者密碼有效期限  

剛剛的設定只會套用在新使用者身上，但對於現有使用者於要額外指令變更：

```
$ sudo apt install chage
```

安裝完後，使用指令來更新天數設定：
 
```
$ sudo chage --maxdays 90 <user>
$ sudo chage --mindays 10 <user>
$ sudo chage --warndays 7 <user>
```

<br><br>

## 強迫使用者登入更換密碼  

常見情境是我們生成亂數密碼給使用者，然後請使用者第一次登入時修改密碼，或是當我更新密碼複雜度時，請舊有使用者改密碼遵循新的密碼規則。這功能可以用剛剛的 `chage` 來達成：

```
$ chage --lastday 0 <user>
$ chage -d 0 <user>
```

這樣會直接把它的到期日期設定成 1970/01/01，強迫它過期。

<br><br> 

## 參考資料     
1. (2022/03/08)。[密碼原則建議](https://docs.microsoft.com/zh-tw/microsoft-365/admin/misc/password-policy-recommendations?view=o365-worldwide)。檢自 Microsoft Docs (2022-05-05)。
2. 只說區塊鏈 (2020-05-13)。[世界密碼日，零極為網絡安全保駕護航！](https://twgreatdaily.com/4z85DXIBd4Bm1__YOkhy.html)。檢自 今日頭條 (2022-05-05)。
3. Chia-An Lee (2021-08-14)。[Linux 上密碼相關設定](https://calee.xyz/post/note_linux_passwd/)。檢自 Chia-An Lee (2022-05-05)。
4. James Kiarie(2020-08-12)。[How to Enforce Password Policies in Linux (Ubuntu / CentOS)](https://www.linuxtechi.com/enforce-password-policies-linux-ubuntu-centos/)。檢自 LinuxTechi (2019-01-04)。
5. [Linux 強制密碼最低複雜度 pam_pwquality 設定教學與範例](https://officeguide.cc/linux-pam-pwquality-password-complexity-policy-configuration-tutorial-examples/)。檢自 Office 指南 (2022-05-05)。
6. admin 。[UNIX / Linux : how to force user to change their password on next login after password has reset](https://www.thegeekdiary.com/unix-linux-how-to-force-user-to-change-their-password-on-next-login-after-password-has-reset/)。檢自 The Geek Diary (2022-05-05)。
7. LinuxSysAdmin (2016-02-25)。[Linux Password policy – /etc/login.defs No change for existing users](https://www.linuxsysadmin.biz/linux-password-policy-etc-login-defs-no-change-for-existing-users/)。檢自 Linux SysAdmin (2022-05-05)。
8. Sk (2016-03-01)。[How To Set Password Policies In Linux](https://ostechnix.com/how-to-set-password-policies-in-linux/)。檢自 OSTechNix (2022-05-05)。  
9. Sk (2019-05-09)。[如何設定 Linux 系統的密碼策略](https://iter01.com/205158.html)。檢自 IT人 (2022-05-05)。
10. osc_v1ao43h5 (2020-12-16)。[Linux 系統密碼策略設定](https://www.mdeditor.tw/pl/gpHY/zh-tw)。檢自 MdEditor (2022-05-05)。
11. [chage(1): change user password expiry info](https://linux.die.net/man/1/chage)。檢自 Linux man page (2022-05-05)。

<br><br>  

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-05-06</summary>
  <ul>
    <li>2022-05-06 發布</li>
    <li>2022-05-05 完稿</li>
    <li>2022-05-05 起稿</li>
  </ul>
</details>
