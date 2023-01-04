---
title: 建立 Python 的虛擬環境
date: 2022-12-30
is_modified: false
image: https://i.imgur.com/q7snBft.png
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Python
- venv
--- 

沒啥重點，純粹就是我熊熊忘了怎麼離開虛擬環境所生出來的一篇 XDDD

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/q7snBft.png" alt="Python 的虛擬環境">
    Python 的虛擬環境（圖片來源: <a href="https://blog.debugeverything.com/virtual-environments-with-python-virtualenv/">DebugEverything</a>）
</p>

## 安裝虛擬環境套件
一般來說，為了不弄亂系統本身的 Python 環境，在開發時會建立虛擬環境。這能幫助你避免相依性地獄，也能避免你因弄亂環境而被使用相同環境的同事圍毆 XDDD

之前最常使用 <mark>virtualenv</mark> 這個命令來建立虛擬環境。使用前必須先透過 pip 安裝：
```bash
$ pip install virtualenv
```

<br class="big">

另一套我最近常用的是 <mark>Python 的 venv</mark>。它出現在 **Python3.6** 之後的版本中，理論上它是內建不需要特別安裝，但如果不幸遇到了，它會提醒你安裝 python3-venv：
```bash
$ sudo apt-get install python3-venv
```



## 如何使用虛擬環境套件
雖然提了兩套，但這兩套的用法大同小異，除了建立時的指令略有不同外，後續的操作基本相同。那首先先來建立虛擬環境。


### 建立虛擬環境
因為我的系統主要是使用 Python 3.9.4，那為了看清楚區別我建立一個 Python 3.6 的虛擬環境：

```bash
$ virtualenv -p python3.6 myenv_py36

created virtual environment CPython3.6.12.final.0-64 in 261ms
  creator CPython3Posix(dest=/home/diatango_lin/Cynthia_Chuang/pyenv/myenv_py36_1, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/diatango_lin/.local/share/virtualenv)
    added seed packages: pip==21.3.1, setuptools==59.6.0, wheel==0.37.1
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
```

其中，參數 `--python` 或是 `-p` 可用來指定虛擬環境的 Python 版號。

<br class='big'>

或是用 Python 的 venv 來建立虛擬環境，這個指令則是透過 Python 這個指令選擇特定或是任意 Python 的版本：
```bash
$ python3.6 -m venv myenv_py36
```
另外，還記得前面說過 Python 的 venv 是在 Python3.6 之後才出現的嗎？因此若想建立 Python3.6 之前的虛擬環境，這個方法是行不通的。



### 使用虛擬環境
當成功建立一個虛擬環境後，你發現目前的下會產生一個虛擬環境資料夾。例如，當使用上述的指令建立虛擬環境後，可以發現名為 `myenv_py36` 的資料夾。而我們要啟動虛擬環境，就需要這些資料夾：

```bash
$ source myenv_py36/bin/activate
```

如此就可以進入虛擬環境模式之中。此時會注意到命令列的前方會有相對應的標示：
```bash
(myenv_py36) $ 
```



### 停用虛擬環境
接下就是本篇重點（誤）！當要離開虛擬環境時輸入：
```bash
(myenv_py36) $ deactivate
```

我老是習慣性直接輸入 `exit`，然後就不小心把 ssh 連線給切斷 🤣 ，雖說這也是從虛擬環境離開了沒錯啦 XDDD


 
## 參考資料 
1. kirai (2018-10-04)。[DAY03-搞懂Python的virtualenv](https://ithelp.ithome.com.tw/articles/10199980)。檢自 iT 邦幫忙 (2022-12-26)。
2. [12. 虛擬環境與套件](https://docs.python.org/zh-tw/3/tutorial/venv.html)。檢自 Python 3.11.1 說明文件 (2022-12-26)。
3. Coding Lab (2021-06-06)。[Python 的 Virtualenv 虛擬環境安裝與使用](https://medium.com/ai-for-k12/python-的-virtualenv-虛擬環境安裝與使用-8645f5884aac)。檢自 AI for K-12｜Medium (2022-12-26)。
4. Richard Ho (2021-04-28)。[建立Python的虛擬環境](https://104.es/2021/04/28/python-virtualenv-introduction/)。檢自 何敏煌老師的課程教材 (2022-12-26)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-12-30</summary>
  <ul>
    <li>2022-12-30 發布</li>
    <li>2022-12-26 完稿</li>
    <li>2022-12-26 起稿</li>
  </ul>
</details>