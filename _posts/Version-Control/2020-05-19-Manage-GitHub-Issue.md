---
title: GitHub Issue 維護與管理
date: 2020-05-19
modified: 2020-05-19
categories:
- Version-Control
tags:
- GitHub
--- 

其實這不只適用於 GitHub，所有的 Issue 維護也可以遵循這樣的標記系統，只是各 Label Groups 與 Label 的內容可能需要隨著專案進行變化，這邊依照過往的使用習慣列了一些常用的 Label Groups。

<!--more-->
<br><br> 

## Styleguide 
<table style="width:100%"> 
	<tr style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Gating Requirement</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label gating-requirement">🏷️ Gating</span> <span class="issue-label gating-requirement">🏷️ Gating Candidate</span> <span class="issue-label gating-requirement">🏷️ Non Issue</span>   </td>
    </tr> 
	<tr style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Platform</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label platform">🏷️ Phone</span>  <span class="issue-label platform">🏷️ Tablet</span>  <span class="issue-label platform">🏷️ Computer</span>   </td>
	</tr> 
	<tr  style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Environment</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label environment">🏷️ Development</span>  <span class="issue-label environment">🏷️ Testing</span>  <span class="issue-label environment">🏷️ Staging</span> <span class="issue-label environment">🏷️ Production</span></td>
	</tr> 
    <tr style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Problem</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label problem">🏷️ Bug</span> <span class="issue-label problem">🏷️ Erratum</span> <span class="issue-label problem">🏷️ Safety</span>  <span class="issue-label problem">🏷️ Suggest</span></td>
	</tr> 
    <tr style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Error type</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label error-type">🏷️ UI</span>  <span class="issue-label error-type">🏷️ UX</span> <span class="issue-label error-type">🏷️ Function</span> <span class="issue-label error-type">🏷️ Typo</span> <br> <span class="issue-label error-type">🏷️ Incorrect</span> <span class="issue-label error-type">🏷️ dependencies</span></td>
	</tr> 
    <tr  style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Action</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label action">🏷️ To-do</span> <span class="issue-label action">🏷️ Pending</span> <span class="issue-label action">🏷️Processing</span>   <span class="issue-label action">🏷️ Verifying</span>  <span class="issue-label action">🏷️ Done</span></td>
	</tr> 
    <tr  style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Inactive</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label inactive">🏷️ Duplicate</span> <span class="issue-label inactive">🏷️ No-Action</span> <span class="issue-label inactive">🏷️ Clarify</span>            </td>
	</tr> 
    <tr  style="border-width: 0px;"> 
	  <td style="border-width: 0px;" width="20%" class="group-name">Additions</td>
      <td style="border-width: 0px;" width="80%"><span class="issue-label additions">🏷️ Feature</span></td>
	</tr> 
</table>    

<br><br> 

## Label Groups

### <span class="issue-label groups gating-requirement"> Gating Requirement </span>
用來標記 issue 的緊急程度，依照嚴重性降冪排序，依序為 `Gating`、`Gating Candidate`、`Non Issue`

<br>

### <span class="issue-label groups platform">  Platform </span>
如果程式可在多個平台上執行，就必須告知是在哪個平台上發生問題，例如：`手機`、`平板`或是`電腦`，向下可以在細分出 device、 OS 或是 browser...等 Label Groups。

<br>

### <span class="issue-label groups environment">  Environment </span>
在理想的情況下，發行與管理程式所使用的環境可大致分成：`Development`、 `Testing`、 `Staging` 與 `Production` 四個獨立環境。不過實務上受限於體、時間或其他資源條件，可能會結合一或多個環境，但最少應該將 Production 獨立出來。

<br>

### <span class="issue-label groups problem"> Problem </span>
在歸類 issue 的時候第一步會區分是 `Bug`、 `Erratum`、 `Safety` 或是 `Suggest` ，如果運做結果不正確或不合預期就歸到 `Bug`；若是文件或註解甚至是 UI 上的文字有誤或者意思錯誤則歸到 `Erratum`；若是安全性相關的問題則標上 `Safety`；其他建議則使用 `Suggest`。

我常用的就這四個標籤，其他標籤會依照專案在進行擴充。
 

<br>

### <span class="issue-label groups error-type"> Error type </span>
這個標籤會在細分 Problem 的部份，我又分成 `UI`、 `UX`、 `Function`、 `Typo`、 `Incorrect` 與 `dependencies`。其中 `dependencies` 是指第三方函式庫或相依函式庫的問題。

<br>

### <span class="issue-label groups action"> Action </span>
我參考 trello 的 [template](https://trello.com/b/LGHXvZNL/kanban-template) 將執行進度分成：`To-do`、 `Pending`、 `Processing`、 `Verifying` 與 `Done`。 

`To-do` 顧名思義就是放入代辦清單。`Pending` 則是先暫時擱置，可能在等待 Design 或 Spec，抑或者有可能是第三方問題等待修復中，不過最常見的原因是出現較緊急的 issue 需要 hot fix ，所以先暫時擱置目前的工作。`Processing` 就是處理中， `Verifying` 則是請 Q 驗證中。
  
<br>

### <span class="issue-label groups inactive"> Inactive </span>
如果評估後不需要動作，則視情況澄清 issue `Clarify` 或是 `No-Action`。若是 isseu 重複，則為較後面出出現的 issue 貼上 `Duplicate` 標籤並關閉。

<br>

### <span class="issue-label groups additions"> Additions </span>
這個標籤 `Feature` 是標注這條 issue 已經在規劃中了。

<br><br> 

## 參考資料 
1. [kanban-template](https://trello.com/b/LGHXvZNL/kanban-template) 。檢自 trello (2020-02-06)。
2. Zach Dunn 。[How we organize GitHub issues: A simple styleguide for tagging](https://robinpowered.com/blog/best-practice-system-for-organizing-and-tagging-github-issues/) 。檢自 robinpowered (2020-02-06)。

<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-05-19</summary>
  <ul class="timestamp">
    　<li>2020-05-19 發表</li>
    　<li>2020-02-06 完稿</li>
  </ul>
</details>


<style>
table {    
  background-color:#ffffff;
  border:0;
  border-collpase:collpase;
  width:200px;
}

tbody{
    background-color:#ffffff;
}

tr {
    background-color:#ffffff;
}

td {
    border-style:solid;
    background-color:#ffffff;
    line-height: 36px;
}

.group-name {
  color:gray;
  text-align:right;
}

.issue-label {
    padding:5px 5px;
    font-weight:bold;
    border-radius:5px;
}

.issue-label.groups { 
    padding: 5px 15px;
}

.issue-label.additions {
    background-color:#91ca55;
}

.issue-label.inactive {
    background-color:#d2dae1;
}

.issue-label.action {
    background-color:#fbca04;
}

.issue-label.error-type {
    background-color:#fef2c0;
}

.issue-label.problem {
    background-color:#FFB5B5;
}


.issue-label.environment {
    background-color:#6abcfd;
}

.issue-label.platform {
    background-color:#c1d3f1;
}

.issue-label.gating-requirement{
    background-color:#f45d43;
} 


</style>
