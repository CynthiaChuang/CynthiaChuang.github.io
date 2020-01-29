---
title: 【Linux】將應用程式加到 Ubuntu 的 Launcher 捷徑
date: 2016-02-20
categories:
- OS-Settings
tags:
- Linux/Unix 
--- 

在使用 Linux 環境的時候，若是使用 .deb 或是使用 apt-get install 進行安裝的程式，在安裝之後就會產生啟動捷徑。但是有些程式是下載後解壓縮就能夠執行的（例如：IntelliJ 系列、Sublime Text...等），這類的程式雖然安裝方便，啟動卻不是很友善，每次都需要執行去特定的路徑或檔案，實在有點麻煩，因此記錄下如何將程式加到啟動捷徑。
<!--more-->
<br> 

以添加 web-strom 的 Launcher 捷徑為例


## 1. 路徑切換

若是希望所新增的 Launcher 可供此電腦所有使用者使用，則將路徑切換到：
```shell
$ cd  /usr/share/applications
```

<br> 反之，若紙希望單一使用者使用，則路徑切換到
```shell
$ cd  ~/.local/share/applicationss
```
<br><br>

## 2. 添加 `.desktop` 檔案

在選定路徑下新增 `.desktop` 檔案
```shell
$ sudo vim web-strom.desktop
```
<br>

檔案內容如下：
```
[Desktop Entry]
Encoding=UTF-8
Name=WebStorm
Comment=WebStorm
Terminal=false
Type=Application
Exec=/opt/webStorm/WebStorm-191.6183.63/bin/webstorm.sh
Icon=/opt/webStorm/WebStorm-191.6183.63/bin/webstorm.png
Categories=Development;IDE
```
<br>

其中

1. **Encoding**  
    表示執行所用的編碼。
    
2. **Name**  
    在應用程式清單中所顯示的名稱。
    
3. **Terminal**  
    是否可使用終端機來開啟。
    
4. **Type**  
    用來定義此 Entry 的型態。常見的有 Application 和 Link 兩種。顧名思義， Application 表此 Launcher 指向一個應用程式； Link 則表示指向一個 URL。   

5. **Exec**  
    點擊此 Entry 時，所執行的指令。只有當 Type 是 Application 才有意義；如果 Type 是 Link，則應該改用 URL。
    
4. **Icon**  
    在應用程式清單中所顯示的 Icon。

5. **Categories**  
    表示此 Entry 的分類。詳細的分類可以參考[這裡](https://specifications.freedesktop.org/menu-spec/menu-spec-1.0.html#category-registry)。



<br><br> 

## 參考資料 
1. [將Android Studio加到Ubuntu的Launcher捷徑｜acow's notebook](http://acow.github.io/blog/2013/06/27/jiang-android-studiojia-dao-launcher-jie-jing/)
2. [[教學] 如何在ubuntu新增自定義程式到應用程式清單中(Desktop Entry)｜辛比誌](https://xenby.com/b/167-%E6%95%99%E5%AD%B8-%E5%A6%82%E4%BD%95%E5%9C%A8ubuntu%E6%96%B0%E5%A2%9E%E8%87%AA%E5%AE%9A%E7%BE%A9%E7%A8%8B%E5%BC%8F%E5%88%B0%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E6%B8%85%E5%96%AE%E4%B8%ADdesktop-en)
3. [Desktop Menu Specification](https://specifications.freedesktop.org/menu-spec/menu-spec-1.0.html#category-registry)
4. [Desktop files: putting your application in the desktop menus](https://developer.gnome.org/integration-guide/stable/desktop-files.html.en)
5. [Linux Desktop Entry 文件深入解析｜IBM](https://www.ibm.com/developerworks/cn/linux/l-cn-dtef/index.html)


