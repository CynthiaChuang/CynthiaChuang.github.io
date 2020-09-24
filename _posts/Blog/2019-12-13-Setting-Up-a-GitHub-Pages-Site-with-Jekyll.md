---
title: 【技能樹栽種】使用 Jekyll 和搭建 Github Pages
date: 2019-12-13
is_modified: false
categories:
- Blog
tags:
- Github Page
- Jekyll
- Jekyll-NextT
- 技能樹栽種
--- 

之前有一陣子在找合適的 blog 想用來存放文章，最好能支援數學公式、程式碼，若能使用 Markdown 來撰寫文章就更好了，另外版面最好整潔些不要像傳統 blog 有側欄的干擾（要求真多 XD
 
前後試過痞客邦、Blogger、Medium、Github Pages...，各有各的優缺點，但其實都不怎麼滿意，最後挑了Github Pages 主要是想玩玩 Jekyll 順便練習一下前端 XD

<!--more-->
<br> 

## 背景知識
### Jekyll 
雖然說是『想玩玩 Jekyll 』而選了 Github Pages，不過其實不懂 Jekyll 也沒差，知道它是一個 Ruby 編寫出來的 framework，讓使用者可以快速的建構出靜態頁面的 blog，並讓使用者可以用 Markdown 來編寫 HTML。有這些知識即可。<br>

### Git / Github
要建 Github Pages ，首先當必須有個 [Github](https://github.com/) 帳號，如果不喜歡 Github 也可以考慮用 GitLab pages，兩個的步驟其實是一樣的，就看你喜歡章魚貓還是狐狸了。至於 git 的部份，只要會 commit 跟 push 就夠了（應該?），不會的話可以找[猴子老師](https://backlog.com/git-tutorial/tw/intro/intro1_1.html)學學。對了，git 要記得安裝。

p.s. 是說我一直覺得**『連猴子都能懂的Git入門指南』**這個網站名稱有點嗆！？還是只有我這認為？不過它真的還滿淺顯易懂的，很適合 git 的入門。<br>

<br>

## 建置步驟
### 1. 新增專案
新增名為「`username`.github.io」的專案，這個 `username` 指的是自己的 GitHub 帳號。這不僅是你的專案名稱，也是你的 blog 網址。<br>
專案成立後，按照 Quick setup 的提示準備好專案的資料夾。<br>


### 2. 安裝 Ruby
剛剛提過 Jekyll 是 Ruby 所編寫出來的 framework，因此在安裝 Jekyll 之前要先安裝，由於我家用的環境是 Windows  ~~（因為要打電動）~~ ，所以以下的步驟是在 Windows 上進行的。在 Windows 平台上安裝 Ruby 可以使用 [RubyInstaller](https://rubyinstaller.org/)，下載符合您自己系統的版本後，可以直接透過 UI 界面完成安裝步驟。<br>

> **Note**<br>
> 使用 UI 界面安裝過程中，請記得勾選 **Add Ruby executables to your PATH** 這個選項，讓安裝程式會自動設定環境變數。<br>
> 否則當你想使用 ruby 指令時，會出現**不是內部或外部命令、可執行的程式或批次檔**的錯訊息，如果出現該訊息就必須手動設定環境變數。

<br> 	
安裝完成後，接著開啟命令提示字元確認 Ruby 是否安裝成功，若成功應該可以看到目前的版號，請務必確認所安裝的 Ruby 是 Jekyll 所要求的 **2.0 以上**的版本 。
```sh
$ ruby -v
ruby 2.5.5p157 (2019-03-15 revision 67260) [x64-mingw32]
```
<br>

### 3. 安裝 Jekyll
Ruby 版本確認無誤後，就可以接著安裝 Jekyll 了
```sh
$ gem install jekyll
```
<br> 	

### 4. 建立 Jekyll 的網站
接下來將專案（e.g. myblog 資料夾）設為 Jekyll 的網站，由於我的專案中已經存在些檔案，所以我在指令的後面加上 **force** 避免覆蓋。網站設置完時，它會動配置一組預設主題 - [minima](https://github.com/jekyll/minima)。
```sh
$ jekyll new myblog  --force 
```
<br> 到此網站就算建立完成了，若想在上線前預覽建置的結果，可先進入專案進行編譯。
```sh
$ cd myblog
$ jekyll serve
```
編譯結果可在瀏覽器輸入 [http://localhost:4000](http://localhost:4000) 後查看。若可以順利執行，就將這次的 diff 進行 commit 後送上 github了。<br> 	

### 5. debug啦
不過通常我做事都沒這順利...果不其然，當我執行 jekyll serve 時，結果出現下列錯誤訊息：
```sh
$ jekyll server
Traceback (most recent call last):
	5: from C:/Ruby25-x64/bin/jekyll:23:in `<main>'
	4: from C:/Ruby25-x64/bin/jekyll:23:in `load'
	3: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-4.0.0/exe/jekyll:11:in `<top (required)>'
	2: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-4.0.0/lib/jekyll/plugin_manager.rb:50:in `require_from_bundler'
	1: from C:/Ruby25-x64/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in `require'
C:/Ruby25-x64/lib/ruby/2.5.0/rubygems/core_ext/kernel_require.rb:59:in `require': cannot load such file -- bundler (LoadError)
```

<br> 根據錯誤訊息 ``cannot load such file -- bundler`` 應該是我漏裝了bundler，因此用 gem 裝一下 bundler。
```sh
$ gem install bundler
```

<br> 不過裝完太心急，忘記 install 就直接執行 jekyll serve ，果不其然又收到錯誤 **Bundler::GemNotFound** 的錯誤。
```sh
$ jekyll server
Traceback (most recent call last):
	15: from C:/Ruby25-x64/bin/jekyll:23:in `<main>'
	14: from C:/Ruby25-x64/bin/jekyll:23:in `load'
	13: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-4.0.0/exe/jekyll:11:in `<top (required)>'
	12: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-4.0.0/lib/jekyll/plugin_manager.rb:52:in `require_from_bundler'
	11: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler.rb:107:in `setup'
	10: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/runtime.rb:20:in `setup'
	9: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/runtime.rb:108:in `block in definition_method'
	8: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/definition.rb:226:in `requested_specs'
	7: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/definition.rb:237:in `specs_for'
	6: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/definition.rb:170:in `specs'
	5: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/definition.rb:258:in `resolve'
	4: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/resolver.rb:22:in `resolve'
	3: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/resolver.rb:49:in `start'
	2: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/resolver.rb:255:in `verify_gemfile_dependencies_are_found!'
	1: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/resolver.rb:255:in `each'
C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-2.0.2/lib/bundler/resolver.rb:287:in `block in verify_gemfile_dependencies_are_found!': Could not find gem 'minima (~> 2.5) x64-mingw32' in any of the gem sources listed in your Gemfile. (Bundler::GemNotFound)
```
<br> 補一下，install 就 run 的起來了
```sh
$ bundle install
```
<br><br>

## 主題更換
如果不喜歡預設的 minima 的話，可以試著改寫 css 弄出自己獨一無二的 style。但如果你跟我一樣懶的話，那就考慮直接套用現成的主題來進行修改：

### 1. 挑選主題
網路上現成的主題還滿多的，可以直接在 google 上搜尋 **jekyll theme** 就會套出一堆網站了，像我是在 [Jekyll Themes](http://jekyllthemes.org/) 上來進行挑選，挑了三個樣式較為簡潔的樣板，分別是：
- [Type](https://github.com/aspirethemes/type) :  [Demo](https://type-jekyll.aspirethemes.com/style-guide)
- [Lanyon ](https://github.com/poole/lanyon) :   [Demo](http://lanyon.getpoole.com)
- [jekyll-theme-next](https://github.com/Simpleyyt/jekyll-theme-next):   [Demo](https://simpleyyt.com/jekyll-theme-next/)

<br>其實有點大同小異啦，最後挑了第三個 jekyll-theme-next。運氣不錯，它還附有[安裝教學](http://theme-next.simpleyyt.com/getting-started.html)，可以直接照著做就好，不過因為剛剛我已經把專案建好了我就不學它 git clone project 來執行 jekyll serve，而是直接複製貼上了 XD <br>

###  2. 重新安裝相依性
主題套用完成後，因為各各主題所使用的套件不同，所以必需重新安裝相依性。
```sh
$ bundle install
Traceback (most recent call last):
	2: from C:/Ruby25-x64/bin/bundle:23:in `<main>'
	1: from C:/Ruby25-x64/lib/ruby/2.5.0/rubygems.rb:303:in `activate_bin_path'
C:/Ruby25-x64/lib/ruby/2.5.0/rubygems.rb:284:in `find_spec_for_exe': Could not find 'bundler' (1.17.1) required by your D:/CynthiaChuang.github.io/Gemfile.lock. (Gem::GemNotFoundException)
To update to the latest version installed on your system, run `bundle update --bundler`.
To install the missing version, run `gem install bundler:1.17.1`
```

<br>又得到錯誤訊息了，按照它的提示先 update 後再重新安裝
```sh
$ bundle update --bundler
$ bundle install
```
<br>

### 3. 網站預覽
相依性檔案重新安裝完成後，就可以再次進行預覽，但我在這邊遇到點問題：
```sh
$ jekyll server
Traceback (most recent call last):
	10: from C:/Ruby25-x64/bin/jekyll:23:in `<main>'
	9: from C:/Ruby25-x64/bin/jekyll:23:in `load'
	8: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-4.0.0/exe/jekyll:11:in `<top (required)>'
	7: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/jekyll-4.0.0/lib/jekyll/plugin_manager.rb:52:in `require_from_bundler'
	6: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-1.17.1/lib/bundler.rb:107:in `setup'
	5: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-1.17.1/lib/bundler/runtime.rb:26:in `setup'
	4: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-1.17.1/lib/bundler/runtime.rb:26:in `map'
	3: from C:/Ruby25-x64/lib/ruby/2.5.0/forwardable.rb:229:in `each'
	2: from C:/Ruby25-x64/lib/ruby/2.5.0/forwardable.rb:229:in `each'
	1: from C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-1.17.1/lib/bundler/runtime.rb:31:in `block in setup'
C:/Ruby25-x64/lib/ruby/gems/2.5.0/gems/bundler-1.17.1/lib/bundler/runtime.rb:319:in `check_for_activated_spec!': You have already activated concurrent-ruby 1.1.5, but your Gemfile requires concurrent-ruby 1.1.3. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
```

<br> 似乎是我所安裝的 concurrent-ruby 版本與主題中 Gemfile  所要求的版號不相符，不過這問題不大，在指令前加上 **bundle exec** 即可
```sh
$ bundle exec jekyll server
```	
<br>

### 4. 繼續debug
反正，我每次安裝從來沒有一次照著說明書就可以安裝完成的（嘆
```sh
$ bundle exec jekyll server
Configuration file: D:/tmp/_config.yml
			Source: D:/tmp
	   Destination: D:/tmp/_site
Incremental build: disabled. Enable with --incremental
Generating...
Jekyll Feed: Generating feed for posts
GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
GitHub Metadata: Error processing value 'description':
Liquid Exception: No repo name found. Specify using PAGES_REPO_NWO environment variables, 'repository' in your configuration, or set up an 'origin' git remote pointing to your github.com repository. in /_layouts/post.html
ERROR: YOUR SITE COULD NOT BE BUILT:
------------------------------------
No repo name found. Specify using PAGES_REPO_NWO environment variables, 'repository' in your configuration, or set up an 'origin' git remote pointing to your github.com repository.
```
	
<br> 我拿到了一條 **GitHub Metadata: No GitHub API authentication could be found** 的錯誤訊息，google 了一下，網上很多[方法](https://0xl2oot.cn/2018/01/26/jekyll-error-github-api/) 說是到 Github 申請一個 token，再配置一個環境變數。只是我覺得有點莫名其妙，為啥一定要跟 Github 綁定？ 最後在 jekyll-theme-next 的 issue 中，找到[有人](https://github.com/Simpleyyt/jekyll-theme-next/issues/19#issuecomment-362142730)提出不用申請 token 的解法：
```直接在 _config.yml 中，為空的 description 加上描述後就不會出現```

<br>在 description 加上描述後，錯誤訊息終於變了：
```sh
Rendering Markup: assets/css/style.scss
	Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/style.scss':
					Invalid CP950 character "\xE2" on line 5
jekyll 3.7.4 | Error:  Invalid CP950 character "\xE2" on line 5
```
<br> 照著[說明](https://github.com/mmistakes/jekyll-theme-hpstr/issues/185#issuecomment-340777090)，我在 `assets/css` 這個路徑下新增一個 `style.scss`  的檔案，檔案內容如下：	

```css
---
---

@charset "UTF-8";
```
<br><br>

## 撰寫文章
歷經千辛萬苦終於把網站弄起來了！之後只要將新寫好的 Markdown  文章，放在 `_posts`  資料夾下，文章就會顯示在網站上。不過要注意以下幾點：

-   Markdown 的檔案命名必須為  `YYYY-MM-DD-{article_title}.md`。
-   Markdown 內容的開頭應該類似下方，不過不同的主題可能會略有不同，請按照各自主題的定義來撰寫：

	```
	---
	title: 文章標題
	date: 2013-12-24 23:29:53
	categories:
	- Foo
	tags:
	- Foo
	- Bar
	- Baz
	--- 

	// 以下開始撰寫你的文章
	```

	> 2019/12/13 紀錄<br>
	> 目前我使用 jekyll-theme-next 測試 categories 無法接受中文，一旦使用中文，會導致找不到該頁面，而導向 404 Page ([#63](https://github.com/Simpleyyt/jekyll-theme-next/issues/63))


<br>

## 後記
不過雖然把 Github Pages 建起來了，不過都沒怎麼更新，主要是它的預覽實在有點麻煩，我不能在行動裝置或非本人的電腦進行，有點小苦惱，再加上後來有找到用 Markdown 寫 Blogger 的方法，所以還是以 Blogger 為主~~（雖然那邊也是在長草）~~。

<br>

## 參考資料 
1. [ Jekyll 介紹 ｜卡斯伯 Blog - 前端，沒有極限](https://wcc723.github.io/jekyll/2014/01/04/what-is-jekyll/)
2. [在 Windows 安裝 Ruby on Rails ｜ShunNien's Blog](https://shunnien.github.io/2015/11/05/in-windwos-install-ruby-on-rails/)
3. [使用 Jekyll + Github 來建立一個部落格 ｜MMiooiMM](https://mmiooimm.github.io/2018/09/10/using-jekyll-with-github-pages.html)
4. [[Ting's筆記Day2] 在Github用Jekyll創建自己的blog ｜iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10198964)
5.  [如何在GitHub Pages使用Jekyll建置部落格 ｜Yung-An's World!](https://mathsigit.github.io/blog_page/2017/11/07/githubpage-with-jekyll/)
6. [使用 GitHub Pages 和 Jekyll 來建立 Blog ｜Xaree](http://xareelee.github.io/tech_note/2015/07/23/%E4%BD%BF%E7%94%A8-GitHub-Pages-%E5%92%8C-Jekyll-%E4%BE%86%E5%BB%BA%E7%AB%8B-Blog.html)
7. [Jekyll使用教程笔记 一 ｜掘金](https://juejin.im/post/5b235a1cf265da597568a97d)
8. [rubygems - cannot load such file -- bundler/setup (LoadError) ｜Stack Overflow](https://stackoverflow.com/questions/19061774/cannot-load-such-file-bundler-setup-loaderror)
9. [jekyll s出现...(Bundler::GemNotFound)问题解决方法 ｜简书](https://www.jianshu.com/p/c70dc6d3af14)
10. [【已解决】GitHub Metadata:No GitHub API authentication — 0xl2oot](https://0xl2oot.cn/2018/01/26/jekyll-error-github-api/) 
11. [本地运行报错 · Issue #19 · Simpleyyt/jekyll-theme-next ｜GitHub](https://github.com/Simpleyyt/jekyll-theme-next/issues/19)
12. [Scss encountered an error while converting 'assets/css/style.scss' · Issue #185 · mmistakes/jekyll-theme-hpstr ｜GitHub](https://github.com/mmistakes/jekyll-theme-hpstr/issues/185#issuecomment-340777090)
13. [撰写博客 ｜Jekyll • 简单静态博客网站生成器](http://jekyllcn.com/docs/posts/)
 





