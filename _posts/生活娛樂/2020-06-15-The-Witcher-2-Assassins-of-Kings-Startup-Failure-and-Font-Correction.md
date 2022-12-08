---
title: "Witcher 2 加強版：啟動失敗/字體修正"
date: 2020-08-11
is_modified: false
categories:
- 生活娛樂
tags:
- 遊戲桌遊
--- 

不務正業的一篇 XD   
最近看完亨超演的巫師後，默默的把 Witcher 2 加強版又安裝回來玩了，說實話這款遊戲我最熟的是新手教學（笑
  
不過每次重來一次都得找一次補釘還真麻煩！受不了每次都要重找補釘，我還是自己記錄一下吧：)
  
<!--more-->

<p class="illustration">
    <img src="https://i.imgur.com/9uRiqjZ.jpg" alt="Witcher 2 加強版">
    Witcher 2 加強版（圖片來源: <a href="https://store.steampowered.com/app/20920/The_Witcher_2_Assassins_of_Kings_Enhanced_Edition/">steam</a>）
</p> 



## x64系統啟動失敗
是說 steam 上的巫師 2 真的不打算修一下這個 bug 嗎？這條 bug 是 2015 年就有的吧，都五年多了耶！

<p class="illustration">
    <img src="https://i.imgur.com/ztrCZ4K.png" alt="x64系統啟動失敗">
</p>

查到解法有二： 
1.  變更成**波蘭文**  
    到 `控制台` 將 `地區及語言:格式` 變更波蘭文為即可。不過我不太喜歡動系統語系，所以先 pass 這個方法。
2.  **base_scripts.dzip**  
    下載[修正錯誤檔](http://www.mediafire.com/download/5p7jc0axfx04omo/The_witcher_2_ErrorsFix.zip)，然後將檔案覆蓋掉原本的 **The Witcher 2\CookedPC\base_scripts.dzip** 即可正常啟動



## 字型修改
另一個必做的修正是字型修改，巫師2中文字型真是慘不忍睹，字型實在太細了，玩的眼睛很累。

好像[找的資源](https://forum.gamer.com.tw/Co.php?bsn=07364&sn=10709)多數雅黑體的，但我實在不愛雅黑體，由於它是簡中的字型包，所以在繁中表現上有點差強人意。它的字元的高度不一致，導致畫面凌亂、不整齊。這會逼瘋處女座的 XDDD

<p class="illustration">
    <img src="https://i.imgur.com/n1LCpWm.png" alt="微軟正黑體 (上) 和 微軟雅黑體 (下) 來顯示的結果">
    微軟正黑體 (上) 和 微軟雅黑體 (下) 來顯示的結果（圖片來源: <a href="https://louis925.wordpress.com/2018/03/29/%E8%AB%8B%E4%B8%8D%E8%A6%81%E7%94%A8%E5%BE%AE%E8%BB%9F%E9%9B%85%E9%BB%91%E9%AB%94%E4%BE%86%E9%A1%AF%E7%A4%BA%E5%8F%B0%E7%81%A3%E5%8D%80%E7%9A%84%E7%B9%81%E9%AB%94%E5%AD%97/" >稚空's Blog</a>）
</p>

還好後面有找到 [Adoltw 阿特魯所提供的儷黑體](https://www.ptt.cc/bbs/Steam/M.1392644855.A.476.html)與 [Clavius 提供思源黑體](https://steamcommunity.com/sharedfiles/filedetails/?id=402934070)的字型檔，不然我就要來研究怎麼自製字型了 XD，這邊記錄一下儷黑體的更換步驟：
1. 下載[字體](http://www.mediafire.com/download/8i313kxns6045ds/The_witcher_2_ZH_Fonts.zip) 得到 **cfonts.swf** 及 **fonts_zh.swf** 兩個檔案，並放到 <b>the witcher 2\CookedPC\globals\gui\ </b> 下，其中 cfonts.swf 主要影響介面中的小字體
2. 另外將 **fonts.csv** 放在 <b>the witcher 2\CookedPC\globals\gui\fonts  </b>  目錄下



## 其他修正
有興趣的可以看看 [xone 的這篇](https://m.gamer.com.tw/forum/Co.php?bsn=7364&snB=29588)，他提供了一些包含
1. 遊戲團隊提供的各項戰鬥修正/更新/再平衡檔案包
2. 額外的繁體中文修正檔
3. 視野調整

不過這些修正我都沒做，有興趣的可以嘗試看看。

<br class="big">

另外，關於第二點的**額外的繁體中文修正檔**，他所提供的修正檔連結死掉了，如果想下載  zh0.w2strings ，可以從下面的連結
- [The Witcher 2 Chinese Text Fixes](https://www.nexusmods.com/witcher2/mods/268/?tab=files&navtag=%2Fajax%2Fmodimages%2F%3Fuser%3D0%26id%3D268)

但不曉得是我下載的版本翻譯有修正過了是不是，目前玩起來感覺還好？所以就暫時不打算換了。



## 攻略
最後記錄一下一些攻略的連結
1. [[心得] 巫師2心得與注意要點｜批踢踢實業坊](https://www.ptt.cc/bbs/XBOX/M.1357041046.A.C34.html)
2. [《巫師 2：王國刺客》全章節圖文流程攻略｜遊戲百科 GameWikia](https://www.gamewikia.com/guide/full/5347)
3. [巫師2：王國刺客 隱藏彩蛋｜娛樂計程車](https://www.entertainment14.net/blog/post/63661521-%e5%b7%ab%e5%b8%ab2%ef%bc%9a%e7%8e%8b%e5%9c%8b%e5%88%ba%e5%ae%a2-%e9%9a%b1%e8%97%8f%e5%bd%a9%e8%9b%8b)
4. [《巫師2：王國刺客》圖文流程攻略｜娛樂計程車](https://www.entertainment14.net/blog/post/63661349-%e3%80%8a%e5%b7%ab%e5%b8%ab2%ef%bc%9a%e7%8e%8b%e5%9c%8b%e5%88%ba%e5%ae%a2%e3%80%8b%e5%9c%96%e6%96%87%e6%b5%81%e7%a8%8b%e6%94%bb%e7%95%a5)
5. [《巫師2：王國刺客》 攻略匯集 (4/26更新)｜娛樂計程車](https://www.entertainment14.net/blog/post/63661955-%E3%80%8A%E5%B7%AB%E5%B8%AB2%EF%BC%9A%E7%8E%8B%E5%9C%8B%E5%88%BA%E5%AE%A2%E3%80%8B-%E6%94%BB%E7%95%A5%E5%8C%AF%E9%9B%86-4-26%E6%9B%B4%E6%96%B0)
6. [《The Witcher 2 巫師2: 國王的刺客》加強版綜合攻略｜Wii遊戲最速攻略](http://www.isheart.com/viewthread.php?tid=141297)
7. [巫师2刺客之王攻略秘籍_巫师2刺客之王全攻略_巫师2刺客之王攻略专区｜游侠网](https://gl.ali213.net/z/5954/)
8. [《巫师2》超详细支线全攻略｜游侠网](https://gl.ali213.net/html/2011/22522_2.html)
9. [《巫师2：刺客之王》详细图文流程攻略_序章 国王在早晨召见我｜游民星空](https://www.gamersky.com/handbook/201105/174153.shtml)
10. [《巫师2：刺客之王》详细图文流程攻略_第一章 支线任务｜游民星空](https://www.gamersky.com/handbook/201105/174153_7.shtml)
11. [RE:【心得】巫師2(加強版)攻略 (爆雷劇透) (pc版本)｜巴哈姆特](https://m.gamer.com.tw/forum/Co.php?bsn=7364&snB=35952)
12. [《巫师2：刺客之王》详细图文攻略-第一章 - 巫师2：刺客之王攻略｜游久单机游戏频道](http://pcgame.uuu9.com/gonglue/201105/373507_5.shtml)
13. [《巫師2》超詳細支線全攻略](https://www.gamewikia.com/guide/100042)



## 參考資料 
1. Clavius (2015-03-06)。[更換字體為思源黑體](https://steamcommunity.com/sharedfiles/filedetails/?id=402934070) 。檢自 Steam 社群 (2020-06-15)。
2. Adoltw 阿特魯 (2014-02-17)。[[遊戲] 巫師2加強版 字型修改 心得](https://www.ptt.cc/bbs/Steam/M.1392644855.A.476.html) 。檢自 ptt - Steam (2020-06-15)。
3. AviLun 艾維倫 (2015-06-05)。[【二代】《巫師2：加強版》修改字體傻瓜包(新增預覽圖)](https://forum.gamer.com.tw/Co.php?bsn=07364&sn=10709) 。檢自 巫師 The Witcher 系列 哈啦板 - 巴哈姆特 (2020-06-15)。
4. xone (2015-06)。[【情報】巫師2加強版 啟動失敗/字體修正/戰鬥平衡MOD及各項修正檔[更新2]](https://m.gamer.com.tw/forum/Co.php?bsn=7364&snB=29588) 。檢自 巫師 The Witcher 系列 哈啦板 - 巴哈姆特 (2020-06-15)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2020-08-11</summary>
  <ul>
    <li>2020-08-11 發布</li>
    <li>2020-06-15 完稿</li>
    <li>2020-06-05 起稿</li>
  </ul>
</details>