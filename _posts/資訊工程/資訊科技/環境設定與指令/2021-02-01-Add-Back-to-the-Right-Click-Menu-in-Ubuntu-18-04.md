---
title: 在 Ubuntu 18.04 中添加右鍵選單「新增文件」選項
date: 2021-03-07 23:44
is_modified: false
categories:
- "資訊科技 › 環境設定與指令"
tags:
- Linux/Unix
--- 

唉唉唉，為啥我升完級後，我右鍵的新增文件不見了！！！！雖然可以透過終端機下 `touch` 也可以達成目的，但要這樣來回切換實在有點麻煩，看看有沒有辦法把它加回來好了。

<!--more-->
<br>

## 通過 UI 新增新文件
拜了下大神，發現要把它用回來的方法來挺簡單了，在 Nautilus（Ubuntu 預設的檔案瀏覽器）中，它會去讀取家目錄下的模板資料夾，並將資料夾中檔案做為右鍵選單的選項。說起來很神奇，但其實只要 2 個步驟就可以完成了：

1. **建立模板資料夾**  
    首先，請先回到家目錄，檢查**模板**資料夾是否存：
    
    ```bash
    $ cd 
    $ ls | grep "模板"    
    ```

    如果有結果輸出，代表該資料夾存在；否，代表資料夾則不存在，須建立一個新的資料夾：
    ```bash
    $ mkdir "模板"    
    ```
    對了，因為我的系統是正體中文的，所以資料夾名稱是**模板**；若是英文的，則是使用 **Templates**。
    
2. **新增文字檔案選項**    
    接下來，進入模板資料夾後，建立一個名為**文字檔案**的空文字檔案。
    ```bash
    $ cd "模板"  
    $ touch "文字檔案"
    ```
    這時打開右鍵選項，就會發現我的新增檔案回來了！！！（喜極而泣
    
    <center> <img src="https://i.imgur.com/BTQlTPB.png" alt="新增檔案"></center>
    <br>
    
    如果你要新增任何或移除任何文件類型，只要在資料夾新增或移除對應的文件即可。

<br><br> 

## 參考資料 
1. OpenView (2019-08-18)。[小技巧：Ubuntu 18.04中為滑鼠右鍵菜單添加「新建文件」選項](https://kknews.cc/zh-tw/code/99nx3e8.html) 。檢自 每日頭條 (2020-02-01)。
2. 服務端 (2018-12-12)。[將新建文件添加回Ubuntu 18.04中的右鍵選單](https://www.itread01.com/content/1544555947.html) 。檢自 IT閱讀 (2020-02-01)。
3. Karim Buzdar (2018-12-04)。[Add “New Document” back to the right-click menu in Ubuntu 18.04](https://vitux.com/add-new-document-back-to-the-right-click-menu-in-ubuntu-18-04/) 。檢自 VITUX (2020-02-01)。


<br><br> 

## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-03-07</summary>
  <ul>
    <li>2021-03-07 發布</li>
    <li>2021-02-01 完稿</li>
    <li>2021-02-01 起稿</li>
  </ul>
</details>