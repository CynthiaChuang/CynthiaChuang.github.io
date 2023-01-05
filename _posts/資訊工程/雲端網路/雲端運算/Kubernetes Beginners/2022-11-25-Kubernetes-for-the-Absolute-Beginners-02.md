---
title: "【K8S Beginners 筆記】CH 4~6 PODs, YAML, ReplicaSet and Deployments"
date: 2022-12-30
is_modified: false
image: https://i.imgur.com/2F0W0aE.png
categories:
- "雲端網路 › 雲端運算"
tags:
- "Udemy"
- "Docker"
- "K8S"
- "Kubernetes Beginners"
- "讀書筆記"
--- 

課程都上完了，但筆記還沒寫完 Orz
	
但往好處想，這篇完成後，我就完成了 1/2 了，至少從章節上看來是如此 XDDD

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/2F0W0aE.png" alt="課程縮圖">
    課程縮圖（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>



## CH 4｜Kubernetes Concepts
這章的大標雖然是 K8S 的概念，不過其實只講了 pod。對，就是在[上一篇](https://cynthiachuang.github.io/Kubernetes-for-the-Absolute-Beginners-01/#專有名詞)中，我自己去查相關定義的那個，這邊就看講師更有系統的說明吧。

<br class="big">

在開始前，先假設些先決條件：
1. 應用程式已經開發完成，並包成了 docker image。
2. 這個 docke image 已經上傳至 registry，無論是公開或是私人倉庫，反正 K8S 拉的回來就好。
3. K8S cluster 已經設置完成並辛勤工作中了！ 


### 4-1｜PODs
對於 K8S 而言，終極目標是將應用程式以 container 的形式部署到 work nodes 中。但若從微觀的視角來說，container 並<mark>不是直接部署</mark>到 node 上，這些 container 會被封裝到 pod 中，最後會以 pod 為單位部署到 node 上執行。

<p class="illustration">
    <img src="https://i.imgur.com/PKxEkJd.png" alt="Pod Instance">
    Pod Instance（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


Pod 是 K8S 中<mark>最小的運作單位</mark>，可以將它想像成是一個應用程式運行的 instance。注意，**是一個應用程式，而不是一個 container**。雖然多數情況下，pod 跟 container 是一對一的關係，但有些應用會需要 helper containers 以支援特定的任務，如：作為服務的前端、資料庫...等，會將兩者封裝到同一個 pod 中一併啟動。

將同一應用的 containers 放置同一個 pod 中有幾個好處：
1. 同一應用的 containers 生命週期會一致，因為它們是同屬同一個 pod，方便 K8S 管理與部署。
2. 它們共享相同的 network namespace，所以容器間可用 `localhost` 直接溝通。
3. 共享存儲空間。


<p class="illustration">
	<img src="https://i.imgur.com/CsYeazp.png" alt="Multi-Container PODs">
	Multi-Container PODs
</p>


#### Container 的擴展
之前提過當負載出現異動時，container 會向上或向下擴展。這邊介紹 container 的擴展與 pod 之間的關係。

若要新增應用的 instances 來分擔負載，也是以 **pod 為單位來新增**，它們並非在同一個 pod 中起 container，而是會在相同 <mark>node 上添加一個相同的 pod</mark>，所以才會說 pod 是最小的運作單位。是說，這也是可以想樣的，你想想如果同時起兩個負責 web 的 container，那麼那個 port 該給誰 XDDD。但若是 node 沒有足夠資源，則是會在將 pod 起在 cluster 中的其他 node 上。

反之，在縮小規模也是如此。當需求下降時，也是刪除 pod 以縮減服務，並非直接刪除 container。

<p class="illustration">
    <img src="https://i.imgur.com/S7HT0QO.png" alt="Create a Nwe Container and Pod">
    Create a Nwe Container and Pod（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


#### 補充資料  
- [Pod Overview](https://kubernetes.io/docs/concepts/workloads/pods/)


### 4-2｜Demo：PODs
我把這節的筆記跟上一節沒寫 kubectl 給合併在一起了，因為它們都在介紹跟部署 pod 的相關指令：

1. **kubectl run**  
	```bash
	$ kubectl run {pod-name} --image {image-name} 
	```

	這是起 pod 用的指令，其中 image 的部分預設會從 docker hub 下載所輸入的 image。若是要從私人倉庫下載[看起來是要另外設定 K8S](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)，我們就先別管它了。

	對了，這種方法看起來所啟動的 pod 跟 container 是一對一的關係。
	<p class="illustration">
	<img src="https://i.imgur.com/LbPXTI9.png" alt="kubectl run">
	kubectl run
	</p>

2. **kubectl get pods**    
	```bash
	$ kubectl get pods 
	```
	
	可用來看 cluster 中的 pod 列表與一些簡單的資訊，如：container 狀態、運行時間...等。 
	
	<p class="illustration">
	<img src="https://i.imgur.com/lwJehbW.png" alt="kubectl get pods">
	kubectl get pods
	</p>

	<br class="big">

	```bash
	$ kubectl get pods 
	```

	如果要看稍微多點的資訊，便帶上 `-o wide`，可以看到如：、IP、所屬 node...等資訊。
	
	<p class="illustration">
	<img src="https://i.imgur.com/vu82YB6.png" alt="kubectl get pods -o wide">
	kubectl get pods -o wide
	</p>
	
3. **kubectl describe**  
	與 get 指令相比，describe 提供了更多的資訊，如：label、IP、所屬 node、及 pod 中所有 container 的資訊，如果有多個 container 則會在此陳列。
	
	我認為最重要的是最後的附加訊息，它這邊呈現 pod 中所發生的所有事件，用來 debug 很好用 XDDD

	<p class="illustration">
	<img src="https://i.imgur.com/Wh3NXLW.png" alt="kubectl describe">
	kubectl describe
	</p>


對了，上述看到的 IP 的都只能從 node 內部去尋訪。但這些 container 不經配置，是從外部是無法取得的，這部分後面應該會教。
 
 
### 4-3｜測驗 3：PODs
1. **The smallest unit you can create in Kubernetes object model is:**
	- [ ] Service
	- [ ] Application
	- [x] Pod
	- [ ] Container
	- [ ] Process
2. **A Pod can only have one container in it**
	- [ ] True
	- [x] False
3. **What is the right approach to scale an application**
	- [ ] Deploy additional containers in the Pod
	- [x] Deploy additional Pods
	- [ ] You cannot scale an application in Kuberenetes. This is not a use-case of Kubernetes.



## CH 5｜YAML Introduction

看這節的投影片跟其他幾個章節的布景主題並不一致，應該是後來補充的，可能有人反應不會寫 YAML？

<p class="illustration">
    <img src="https://i.imgur.com/DSYkb8q.png" alt="YAML">
    YAML（圖片來源: <a href="https://zh.m.wikipedia.org/zh-hant/YAML">維基百科</a>）
</p>


### 5-1｜Introduction to YAML

YAML 全名是（YAML Ain’t a Markup Language）是一個可讀性高，通常用來表達資料序列的格式，特別適合用來表達或編輯資料結構、各種設定檔。在 K8S 這邊就是使用這種語言來撰寫設定檔。


#### XML、 JSON 與 YAML 比較
這節開始前快速比較了三種常見的結構數據 XML、 JSON 與 YAML。

<p class="illustration">
    <img src="https://i.imgur.com/7QNzYdg.png" alt="同資料以 XML、JSON 和 YAML 表示">
</p>

在 XML 中，每個<mark>屬性都是以添加標籤的方式來增加</mark>，雖然不影響數據表示，但冗餘資料是三者之中最多。相較之下，JSON 比 XML 簡潔得多，因為它改<mark>以 key-value pair 的方式來記錄資料</mark>，且改採 `:` 分隔 key-value、 `,` 區隔每筆資料，並以 `{...}` 表示物件、 `[...]` 表示陣，因此 JSON 還是比 XML 要小，而且耗時更少。

至於 YAML 也是以 key-value pair 來表示，它甚至移除所有的括號與逗號，因此可以進一步壓縮大小。至於撰寫方式等等一起記錄在下一節好了。


#### 撰寫規則

<p class="illustration">
	<img src="https://i.imgur.com/P0sD5ps.png" alt="YAML 撰寫規則">
    YAML 撰寫規則（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


1.  **key-value pair**  
	它與 JSON 一樣<mark>冒號</mark>來分隔 key-value。而且跟 JSON 一樣，**冒號後面記得加個空白**，不然會跟我一樣抓蟲抓個半天 XDDD
	
2. **Array/List**  
	至於如果是 array，則需要在<mark>每個 array 元素之前加上破折號</mark>。
	
3. **Dictionary/Map**  
	若要製作 dictionary 則在其對應鍵值之下列，填上相對應元素，且在**每項之前則是會加上相同數量的空格**。在 YAML 中<mark>縮排的空格數目不重要，重要的是相同層級要左對齊。</mark>

4. **List of dictionary**  
	其實沒這東西啦，就是 array 的元素是 dictionary，這種時候需特別注意縮排跟破折號，別歸錯類了。

5. **註釋**  
	YAML 的註解是以 `#` 開頭的。
	
<br class="big">
	
至於何時用　dictionary、何時用　array？
1. 無序集合用 dictionary，有序集合用 array。
2. 表示某個物件的構成元素用 dictionary，例如：汽車由顏色、型號、價格...之類的屬性所構成。


### 5-2｜編碼練習：YAML

1. **Update the food.yml file to add a Vegetable - Carrot.
	Not sure how to solve this? Check the answer in the answer.yaml file.**
	
	```yaml
	Fruit: Apple
	Drink: Water
	Dessert: Cake
	```

	答：

	```yaml
	Fruit: Apple
	Drink: Water
	Dessert: Cake
	Vegetable: Carrot
	```

2. **Update the food.yml file to add a list of Vegetables - Carrot, Tomato, Cucumber**
	
	```yaml
	Fruits:
	  - Apple
	  - Banana
	  - Orange
	```

	答：

	```yaml
	Fruits:
	  - Apple
	  - Banana
	  - Orange
	Vegetables:
	  - Carrot
	  - Tomato
	  - Cucumber
	```


3. **We have updated the food.yml file with nutrition information for Fruits. Similarly update the nutrition information for Vegetables. Use the below table for information**

	| Vegetables | Calories | Fat |Carbs |
	| -------- | -------- | -------- |-------- |
	| Carrot     | 25     | 0.1     | 6 |
	| Tomato     | 22     | 0.2     | 4.8 |
	| Cucumber     | 8     | 0.1    | 1.9|

	```yaml
	Fruits:
	- Apple:
		Calories: 95
		Fat: 0.3
		Carbs: 25
	- Banana:
		Calories: 105
		Fat: 0.4
		Carbs: 27
	- Orange:
		Calories: 45
		Fat: 0.1
		Carbs: 11
	```
	
	答：
	
	```yaml	
	Fruits:
	- Apple:
		Calories: 95
		Fat: 0.3
		Carbs: 25
	- Banana:
		Calories: 105
		Fat: 0.4
		Carbs: 27
	- Orange:
		Calories: 45
		Fat: 0.1
		Carbs: 11
	Vegetables:
	- Carrot:
		Calories: 25
		Fat: 0.1
		Carbs: 6
	- Tomato:
		Calories: 22
		Fat: 0.2
		Carbs: 4.8
	- Cucumber:
		Calories: 8
		Fat: 0.1
		Carbs: 1.9
	```

4. **Jacob is 30 year old Male working as a Systems Engineer at a firm. Represent Jacob's information (Name, Sex, Age, Title) in YAML format. Create a dictionary named Employee and define properties under it.**
	
	答：
	
	```yaml
	Employee:
      Name: Jacob
      Sex: Male
      Age: 30
      Title: Systems Engineer
	```
	
5. **Update the YAML file to represent the Projects assigned to Jacob. Remember Jacob works on Multiple projects - Automation and Support. So remember to use a list.**
	```yaml
	Employee:
	  Name: Jacob
	  Sex: Male
	  Age: 30
	  Title: Systems Engineer
	```

	答：

	```yaml
	Employee:
	  Name: Jacob
	  Sex: Male
	  Age: 30
	  Title: Systems Engineer
	  Projects:
		- Automation
		- Support
	```

6. **Update the YAML file to include Jacob's pay slips. Add a new property "Payslips" and create a list of pay slip details (Use list of dictionaries). Each payslip detail contains Month and Wage.**

	| Month  | Wage |
	| ------ | ---- |
	| June   | 4000 |
	| July   | 4500 |
	| August | 4000 |


	```yaml
	Employee:
	  Name: Jacob
	  Sex: Male
	  Age: 30
	  Title: Systems Engineer
	  Projects:
		- Automation
		- Support
	```
	
	答：
	
	```yaml
	Employee:
	  Name: Jacob
	  Sex: Male
	  Age: 30
	  Title: Systems Engineer
	  Projects:
		- Automation
		- Support
	  Payslips:
		- Month: June
		  Wage: 4000
		- Month: July
		  Wage: 4500
		- Month: August
		  Wage: 4000
	```
	
	
	
## CH 6｜Kubernetes Concepts - Pods, ReplicaSets, Deployments
這章來學怎麼為 K8S 寫 YAML 文件，以用來部署 Pod、Service、ReplicaSets 與 Deployment。


### 6-1｜PODs with YAML
在標準的 K8S 定義文件中包含 <mark>4 個必需根屬性：apiVersion、kind、metadata 和 spec</mark>。

<p class="illustration">
    <img src="https://i.imgur.com/vtkrTEP.png" alt="標準的 K8S 定義文件所包含的根屬性">
    標準的 K8S 定義文件所包含的根屬性（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>


#### apiVersion
> Which version of the Kubernetes API you're using to create this object

簡單來說，就是我們所使用的 K8S API 版本，最常用的大概是 `v1`、`apps/v1`，其他還有諸如：`apps/v1beta1`、`extensions/v1beta1`...等。這些 api 版本的差別後面課程會說，如果講師忘了這件事的話回頭再來[啃文件](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)吧。

是說，總感覺它跳版跳得很隨意，不僅名字很隨意，連功能也是。有些功能在前一版還是 beta，下一版就跳到 stable ...等等，你的 rc 呢？ 


#### kind
> What kind of object you want to create

就是你所要建立的物件類型，在這節就是填寫 Pod。其他可以填的值還有 ReplicaSet、Deployment 或 Service。另外，不同類型的物件類型搭配的 apiVersion 不盡相同，如上表所示，使用時需注意。


#### metadata 
> Data that helps uniquely identify the object, including a name string, UID, and optional namespace

`metadata` 這部分是用來擺放描述性資料地方，常見包含 `name`、 `label` 跟 `annotation`...等，其中 `name` 算是必備的，其他 `label` 跟 `annotation` 則是便於對物件進行識別、分群、管理與部署。需注意的是 `metadata` 的子屬性必須符合 K8S 的規範，但在 `label` 或 `annotation` 之下，你可以填寫任何你想記錄的 key-value pair。

與前 2 個都是字串的根屬性不同，metadata 是採 **dictionary** 的資料格式，所以撰寫時注意空格與對齊，別認錯父親了 XDDD


#### spec
> What state you desire for the object

定義完相關資訊後， `spec` 這邊則是對內容物的定義，像是最重要的 container 與其 image 就是在此段配置。像我們之前討論過的一個 pod 中存在多個 containers 就是在此處定義，列出個別 container 與其對應的 image。

對了，spec 的資料格式 dictionary，但其子屬性 `containers` 的資料格式是 list，別搞混了。注意別忘記了 `containers` 有 `s` 阿！忘了它的話，蟲會抓超久 XDDD


### 6-2｜Demo：PODs with YAML
這節的目的就是嘗試寫一份 yaml 並啟動。

<br class="big">

首先先建立一個名為 `pad.yml`，並使用任一編輯器，如：記事本、vim...等，來撰寫定義文件。根據上節所介紹的 4 個根屬性依序撰寫下來，不過撰寫時需要注意的是：  
1. **apiVersion**：pod 搭配的版號是 `v1`。  
2. **kind**：pod 使用的物件類型是 `Pod`，因為 YAML 是大小寫敏感的，所以**開頭 P 大寫**是有意義的。
3. **輸寫注意**：同階層的縮排要對齊、還有該有 `s` 的屬性別忘了吧！

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cynthia-pod-2
  labels:
    app : nginx
    tier: frontend
spec:
  containers:
    - name: nginx
      image: nginx
```

<br class="big"><br class="big">

一旦準備好定義文件後，就可以透過指令來建立 pod：
```bash
$ kubectl create -f pod.yml
or
$ kubectl apply -f pod.yml
```

<p class="illustration">
    <img src="https://i.imgur.com/2V0KQKO.png" alt="透過指令來建立 pod">
    透過指令來建立 pod
</p>


這邊有 2 個指令 `create` 與 `apply` 可用，至於兩者有何不同？雖然講師說 It doesn't matter，不過我還是很好奇兩者的差異。

由資料看來，kubectl create 是以**命令（Imperative）** 的方式建立資源。此方法是直接告訴 K8S API 要創建新資源，一旦資源已經存在，就會丟出錯誤訊息：

```bash
$ kubectl get pods
No resources found.

$ kubectl create -f pod.yml
pod/cynthia-pod-2 created

$ kubectl create -f pod.yml
Error from server (AlreadyExists): error when creating “pod.yml”: pods “cynthia-pod-2” already exists
```

<br class="big">

kubectl apply 則是以**聲明（Declarative）** 的方式建立資源。它會告知 K8S 所要部署資源的定義，將如何部署資源交由 cluster 來決定，如果資源不存在會配置新資源；若資源已經存在，則是根據目前的定義去調整資源。

兩者最直觀的差異，大概就是 `apply` 可以透過修改 YAML 來更改配置，但 `create` 無法，若要更改只能借助其他指令或是刪除資源重新建立。

<p class="illustration">
    <img src="https://i.imgur.com/zBhPPFc.png" alt="Kubectl apply vs Kubectl create">
    Kubectl apply vs Kubectl create（圖片來源: <a href="https://blog.csdn.net/textdemo123/article/details/104400985">大鹏blog｜CSDN</a>）
</p> 

<br class="big">

但無論是用哪個方法，在此階段都完成 pod 的建立，可以查看其狀態：

```bash
$ kubectl get pods
```

<p class="illustration">
    <img src="https://i.imgur.com/AUmNLdt.png" alt="檢查 pods 狀態">
    檢查 pods 狀態
</p>

或是用更進一步查詢：

```bash
$ kubectl describe pod {name}
```

<p class="illustration">
    <img src="https://i.imgur.com/1P7SjDt.png" alt="查看 pod 詳細狀況">
    查看 pod 詳細狀況
</p>



### 6-3｜K8S YAML Validation in IDEs
這邊介紹下一些可以支援 K8S YAML 的 IDE，雖然用 vim 跟筆記本也撰寫，但遇到大型 YAML 這會使難度直線上升阿，再加上大家都被 autocomplete 給慣壞了 XDDD 

1. **JetBrains 系列**  
	JetBrains 系列它有支援 YAML，能夠幫忙檢查排版與結構的錯誤。但若要支援 K8S YAML，需額外安裝 [plugin](https://plugins.jetbrains.com/plugin/9354-kubernetes-and-openshift-resource-support)。
	
2. **VS code**  
	它也是需要安裝 plugin 以支援 K8S YAML。影片中是用 [RedHat 所發布的 YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)。安裝完後基本上支援 YAML，但要支援 K8S YAML 需額外設定：

	`設定` → 選擇作用域 `User` / `Workspace` → `Yaml:Schems` → 在 `settings.json` 內編輯。在 `settings.json` 中，找到 `yaml.schemas` 並增加 `kubernetes`：

	```json
	"yaml.schemas": {
		"kubernetes": "*.yaml"
	}
	```
	
<br class="big">

不過這些都只能檢查 K8S YAML 的排版、結構以及一些特定屬性，其他的標籤與設定必須等到執行階段才能確定。


### 6-4｜編碼練習：PODs
1. **Let us start simple! Given a pod-definition.yml file. We are only getting   started with it. I have added two root level properties - apiVersion and kind.**   
	Add the missing two properties - metadata and spec  
	```yaml
	apiVersion: 
	kind:
	```
	
	答：	
	```yaml
	apiVersion: 
	kind:
	metadata: 
	spec:
	```
	
2. 	**Let us now populate values for each property. Start with apiVersion.**   
	Update value of apiVersion to v1. Remember to add a space between colon (:) and the value (v1)  
	```yaml
	apiVersion:
	kind:
	metadata:
	spec:
	```
	
	答：	
	```yaml
	apiVersion: v1
	kind:
	metadata: 
	spec:
	```
	
3. **Update value of kind to Pod.**    
	```yaml
	apiVersion: v1
	kind:
	metadata: 
	spec:
	```
	
	答：	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata: 
	spec:
	```

4. **Let us now get to the metadata section.**    
	Add a property "name" under metadata with value "myapp-pod". Remember to add a space before 'name' to make it a child of metadata    
	```yaml
	apiVersion: v1
	kind: Pod
	metadata: 
	spec:
	```
	
	答：	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	spec:
	```
	
5. **Let us add some labels to our Pod**  
	Add a property "labels" under metadata with a child property "app" with a value "myapp". Remember to have equal number of spaces before "name" and "labels" so they are siblings  	

	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	spec:
	```
	
	答：	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	  labels: 
	    app: myapp
	spec:
	```
	
6. **We now need to provide information regarding the docker image we plan to use.**  
	Add a property containers under spec section. Do not add anything under it yet.	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	  labels: 
	    app: myapp
	spec:
	```
	
	答：	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	  labels: 
	    app: myapp
	spec:
	  containers:
	```
	
7. **Containers is an array/list. So create the first element/item in the array/list and add the following properties to it: name - nginx and image - nginx**	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	  labels: 
	    app: myapp
	spec:
	  containers:
	```
	
	答：	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	  labels: 
	    app: myapp
	spec:
	  containers:
	    - name: nginx
	      image: nginx
	```

8. **Perfect! You have successfully created a Kubernetes-Definition file. Let us try it one more time, this time all on your own!**  
	Create a Kubernetes Pod definition file using values below: 
	- Name: postgres 
	- Labels: tier => db-tier
	- Container name: postgres
	- Image: postgres
	
	答：	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: postgres
	  labels:
	    tier: db-tier
	spec:
	  containers:
	    - name: postgres
	      image: postgres
	```

9. **Postgres Docker image requires an environment variable to be set for password.**  
	Set an environment variable for the docker container. POSTGRES_PASSWORD with a value mysecretpassword. I know we haven't discussed this in the lecture, but it is easy. To pass in an environment variable add a new property 'env' to the container object. It is a sibling of image and name. env is an array/list. So add a new item under it. The item will have properties name and value. name should be the name of the environment variable - POSTGRES_PASSWORD. And value should be the password - mysecretpassword	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: postgres
	  labels:
	    tier: db-tier
	spec:
	  containers:
	    - name: postgres
	      image: postgres
	```

	答：	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: postgres
	  labels:
	    tier: db-tier
	spec:
	  containers:
	    - name: postgres
	      image: postgres
	      env: 
	        - name: POSTGRES_PASSWORD
	          value: mysecretpassword
	```



### 6-5｜Hands-On Labs：PODs
- [🚪 Familiarise with the lab environment](https://kodekloud.com/topic/labs-familiarize-with-lab-environment-2/)

<br class="big">

因為這是第一次使用 Lab 所以在操作 Pod 前，先熟悉下環境。

1. **How many nodes are part of the cluster?**

	**答**：1
	
	<p class="illustration">
    <img src="https://imgur.com/GbXESmt.png" alt="How many nodes are part of the cluster?">
	</p>

2. **What is the version of Kubernetes running on the nodes ?**

	**答**：v1.24.1+k3s1
	
	<p class="illustration">
    <img src="https://i.imgur.com/k6ggkyu.png" alt="What is the version of Kubernetes running on the nodes ?">
	</p>

3. **What is the flavor and version of Operating System on which the Kubernetes nodes are running?**

	**答**：Alpine Linux
	
	<p class="illustration">
    <img src="https://i.imgur.com/48bh9RF.png" alt="What is the flavor and version of Operating System on which the Kubernetes nodes are running?">
	</p>

<br class="big">

熟悉完環境後，就開始本章重點吧 ～！
-  [🚪PODs with YAML](https://kodekloud.com/topic/labs-pods-with-yaml-2/)

<br class="big">

1. **How many pods exist on the system? In the current(default) namespace.**

	**答**：0
	
	<p class="illustration">
    <img src="https://i.imgur.com/Hi71hKq.png" alt="How many pods exist on the system? In the current(default) namespace.">
	</p>

2. **Create a new pod with the nginx image.**

	**答**：
	
	<p class="illustration">
    <img src="https://i.imgur.com/sBEngaD.png" alt="Create a new pod with the nginx image.">
	</p>

3. **How many pods are created now?**  
	Note: We have created a few more pods. So please check again.
	
	**答**：4
	
	<p class="illustration">
    <img src="https://i.imgur.com/iJhiBTB.png" alt="How many pods are created now?">
	</p>

4. **What is the image used to create the new pods?**  
	You must look at one of the new pods in detail to figure this out.	
	
	**答**：busybox。喔對，別選自己建的那個，那個 image 絕對是 nginx 呀 XDDD 

	<p class="illustration">
    <img src="https://i.imgur.com/MNaQWsS.png" alt="What is the image used to create the new pods?">
	</p>

5. **Which nodes are these pods placed on?**  
	You must look at all the pods in detail to figure this out.

	**答**：controlplane
	
	<p class="illustration">
    <img src="https://i.imgur.com/3NkzFZp.png" alt="Which nodes are these pods placed on?">
	</p>
	
6. **How many containers are part of the pod webapp?**  
	Note: We just created a new POD. Ignore the state of the POD for now.
	
	**答**：2
	
	<p class="illustration">
    <img src="https://i.imgur.com/vmZd2hv.png" alt="How many containers are part of the pod webapp?">
	</p>


7. **What images are used in the new webapp pod?**  
	You must look at all the pods in detail to figure this out.
	
	**答**：agentx & nginx
	
	<p class="illustration">
    <img src="https://i.imgur.com/Oxzt7Ux.png" alt="What images are used in the new webapp pod?">
	</p>

8. **What is the state of the container agentx in the pod webapp?**  
	Wait for it to finish the ContainerCreating state
	
	**答**：error & waiting  

	<p class="illustration">
    <img src="https://i.imgur.com/c087JPu.png" alt="What is the state of the container agentx in the pod webapp?">
	</p>

9. **Why do you think the container agentx in pod webapp is in error?**  
	Try to figure it out from the events section of the pod.
	
	**答**：A docker image with this name doesn't exist on Docker Hub 

	<p class="illustration">
    <img src="https://i.imgur.com/0puQHFA.png" alt="Why do you think the container agentx in pod webapp is in error?">
	</p>
	
10. **What does the READY column in the output of the kubectl get pods command indicate?**  

	**答**： Rununing / Totoal contaier on pod。回顧看來，在 webapp pod 中要求了 2 個 container，其中一個順利建立，但另一個因為 ErrImagePull 而啟動失敗。
	
	<p class="illustration">
    <img src="https://i.imgur.com/jpbWKVE.png" alt="What does the READY column in the output of the kubectl get pods command indicate?">
	</p>
 
11. **Delete the webapp Pod.**  
	Once deleted, wait for the pod to fully terminate.
	
	**答**：
	
	<p class="illustration">
    <img src="https://i.imgur.com/8w7WwEd.png" alt="Delete the webapp Pod.">
	</p>


12. **Create a new pod with the name redis and with the image redis123.**  
	Use a pod-definition YAML file. And yes the image name is wrong!
	
	**答1**：
	
	```yaml
	apiVersion: v1
	kind: Pod
	metadata: 
	  name: redis
	spec:
	  containers:
	    - name: redis
	      image: redis123
	```      

	<p class="illustration">
    <img src="https://i.imgur.com/wVsZ5jQ.png" alt="Create a new pod with the name redis and with the image redis123.-1">
	</p>
	
	<br class="big">
	
	**答2**：是不用準備 YAML 檔，直接建立物件。

	<p class="illustration">
    <img src="https://i.imgur.com/kNR7u1F.png" alt="Create a new pod with the name redis and with the image redis123.-2">
	</p>
	
	在執行時，可添加 `-o yaml` 將物件輸出成 YAML 檔 submit 出去。此時通常會搭配 `--dry-run=client` 使用，在 submit 時會先拿掉一些 default 參數。恩...如果沒加輸出的 YAML 檔會這麼長：
	
	<p class="illustration">
    <img src="https://i.imgur.com/O7cuqwG.png" alt="Create a new pod with the name redis and with the image redis123.-3">
	</p>

13. **Now change the image on this pod to redis.**  
	Once done, the pod should be in a running state.
	
	**答1**：直接修改 YAML 檔，再使用 apply 套用修改。
	```yaml
	apiVersion: v1
	kind: Pod
	metadata: 
	  name: redis
	spec:
	  containers:
	    - name: redis
	      image: redis
	```
	<p class="illustration">
    <img src="https://i.imgur.com/8dksFo8.png" alt="Now change the image on this pod to redis.-1">
	</p>	
	
	<br class="big">
	
	**答2**：也可利用 `kubectl edit` 來修改 pod 
	<p class="illustration">
    <img src="https://i.imgur.com/e37ElOd.png" alt="Now change the image on this pod to redis.-2">
	</p>	



### 6-6｜Replication Controllers and ReplicaSets
之前說過 controllers 是 orchestration 背後的大腦，且依照不同的用途出現了不同的 controller。

這節介紹負責產生副本的 controller - **replication controller** 顧名思義就是負責**控制複製**，會依照所宣告的副本（replica）數目，<mark>生成並維持相對應 pod 數量</mark>。

<p class="illustration">
<img src="https://i.imgur.com/kmnrnBt.png" alt="Replica">
</p>	

<br class="big">

而 **replica sets** 是在新版本的 K8S 中被建議用以取代 replication controller。不過在本質上兩者沒有的不同，只是 <mark>replica sets 多支援集合式的 selector</mark>，且此**屬性（selector）是必備的**。

Replica sets 會透過 selector 來篩選匹配 `label` 的 pod 進行管理，這樣的機制會使 replica sets 的管轄範圍不限於 replica sets 中所宣告的 pod，而是擴及所有的 pod 無論是已建立或是未來建立的。

 
#### Advantages
基於 **備援性**、 **擴展性**、 **共享性**：


1. **High Availability**  
	如前面所說，replication controller 可以**生成並維持**指定數量的 pod。換句話說，如果有 pod 異常，會生成新的 pod 來替代；而若有多出來的 pod 也會自動回收。
	
	因此一旦 pod 失效或故障，replication controller 會立即生成一個新的 pod 填補空缺，防止使用者無法尋訪應用，從而提供高可用性。這新生成的 pod 會依照 node 資源使用狀況，被分配到原 node 或新的 node 上。
 
	<p class="illustration">
    <img src="https://i.imgur.com/3q2qPC0.png" alt="High Availability">
    High Availability（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>
 
2. **Load Balancing & Scaling**  
	另一個使用 replication controller 的原因是為了建立多個 pod 來共享它們之間的負載。當使用者增加時，可以部署額外的 pod 來平衡兩個 pod 之間的負載，以擴展所提供的應用；反之，當使用者減少時，也可以適量減少 pod。

	<p class="illustration">
    <img src="https://i.imgur.com/0kPbrvX.png" alt="Load Balancing & Scaling">
    Load Balancing & Scaling（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>


#### RC & RS with YAML
這邊來看看如何撰寫 YAML。

1. **RC with YAML**  

	<p class="illustration">
	<img src="https://i.imgur.com/keUY2Dm.png" alt="RC with YAML">
	RC with YAML（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>

	如同前面所說，標準的 K8S 定義文件需包含 4 個必需根屬性：apiVersion、kind、metadata 和 spec。在撰寫 replication controller 的 YAML 時，需將 `kind` 設為 `ReplicationController` ，而對應的 `apiVersion` 則設為 `v1`。
	
	與 pod 的 YAML 最大的差異在於 `spec`。在撰寫 replication controller 時，會在 `spec` 之下新增 `template` 的子屬性，用以記錄 replication controller 所要控制的 pod。基本上，在 `template` 下撰寫 pod 與直接撰寫 pod YAML 無異，但唯一的不同之處在於無需宣告 `kind`，因為這邊都是 pod，而無需宣告 `kind`，也就無須宣告 `apiVersion`。
	
	在 `spec` 中除了新增 `template` 的子屬性外，還會添加 `replicas` 屬性用以表明需要多少份副本。最後需要注意的是， `template` 和 `replicas` 是 `spec` 的直接子屬性，所以他們倆個的縮排必需一致。  
 

2. **RS with YAML**  

	<p class="illustration">
	<img src="https://i.imgur.com/YPXY28P.png" alt="RS with YAML">
	RS with YAML（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>
	
	ReplicaSet 的撰寫與 ReplicationController 非常相似，但有個不同之處：
	1.  **`kind` 與 `apiVersion`**  
		這邊 `kind` 是要改成 `ReplicaSet`，重要的是 `ReplicaSet` 的 `apiVersion` 要改成 `apps/v1`。若持續使用 `v1`，則會得到下列錯誤訊息：

		<p class="illustration">
		<img src="https://i.imgur.com/nzPQTWj.png" alt="引用錯誤 apiVersion">
		引用錯誤 apiVersion（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
		</p>
	
	2. **selector**  
		前面提過 replication controller 與 replica sets 兩者最關鍵的差別在於 **集合式的 selector** 的支援。因此必須 `spec` 加上 `selector`，常見情況下會搭配 `matchLabels` 使用。



### 6-7｜Demo：ReplicaSets
這節的目的就是嘗試寫一份 YAML 並啟動。雖然在前面介紹了 rc 與 rs 兩種 YAML 寫法，但在實做上會建議採用較新的 ReplicaSets。

<br class="big">

基本重點在上一節已經介紹過了，所以這邊就直接上工了。不過撰寫時需要注意的是：
1. **apiVersion**：ReplicaSet 搭配的版號是 `apps/v1`。
2. **selector**：selector 的 label 需與 pod 的 label 可以相互匹配，不然 ReplicaSet 管不到這些 pod 呀 XDDD。不過 ReplicaSet 的 label 則無須配合，那是說明它本身的特性，而不是它的守備範圍。


```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: cynthia-app-replicaset
  labels:
    app: cynthia-app
spec:
  selector:
    matchLabels:
      tier: frontend
  replicas: 3
  template:   
    metadata:
      name: cynthia-pod-2
      labels:
        app : nginx
        tier: frontend
    spec:
      containers:
        - name: nginx
          image: nginx
```

<br class="big">

一旦準備好定義文件後，就可以透過指令 `create` 或 `apply` 來建立 pod：


```bash
$ kubectl create -f replicasets.yaml
or
$ kubectl apply -f replicasets.yaml

pod/cynthia-pod-2 created
```

完成 replicasets 建立後，可以查看其狀態：
```bash
$ kubectl get replicaset
```

<p class="illustration">
    <img src="https://i.imgur.com/PbTmFND.png" alt="檢查 replica set 狀態">
    檢查 replica set 狀態 
</p>

<br class="big">

觀察 pod 數目，可以注意到一旦 replicasets 建立後，就會生成並維持相對應 pod 數量：
```bash
$ kubectl get pods
```

<p class="illustration">
    <img src="https://i.imgur.com/FNWW34X.png" alt="觀察 pod 數目">
    觀察 pod 數目 
</p>

即便刪除一個 pod，會立即生成新的 pod 來替代：

<p class="illustration">
    <img src="https://i.imgur.com/rpjVcQF.png" alt="刪除 pod 後生成新的 pod">
    刪除 pod 後生成新的 pod
</p>

更進一步查詢 replica set 的狀況，可以看到持續維持 3 個 replicas，在 Event 的部分也可以看到有 4 個 create 紀錄，因為我們之前刪了一個 pod：

<p class="illustration">
    <img src="https://i.imgur.com/QW9zTn0.png" alt="查看 replica set 詳細狀況">
    查看 replica set 詳細狀況
</p>

若是試圖新增一個 pod，會注意到 pod 在生成過程一旦超過指定數量的 pod 就會中止：
<p class="illustration">
    <img src="https://i.imgur.com/LyCCOox.png" alt="新增超過指定數量的 pod 會被中止">
    新增超過指定數量的 pod 會被中止
</p>

<br class="big">

另外的情境是如何縮放 replicas？

1. **`edit`**  
	不過用這個方法是直接需改正在執行的暫存檔，因此需要非常小心，可能會出現出乎意外的副作用。

	```bash
	$ kubectl edit replicaset cynthia-app-replicaset
	```

	<p class="illustration">
		<img src="https://i.imgur.com/KTsNh3S.png" alt="暫存 YAML 檔">
		暫存 YAML 檔
	</p>
	
	一旦將 replicas 增加到 4，會自動生成新的 pod，以彌補數量的不足之處：
	
	<p class="illustration">
		<img src="https://i.imgur.com/IsX032m.png" alt="增加 replicas 會新增pod以彌補數量的不足之處">
		增加 replicas 會新增pod以彌補數量的不足之處
	</p>


2.  **`scale`**  
	另一種也是一次性的修改，但無須修改 YAML 檔：
	
	```bash
	$ kubectl scale replicaset cynthia-app-replicaset --replicas=2
	```
	
	<p class="illustration">
	<img src="https://i.imgur.com/l9v7mcV.png" alt="scale 縮放 replica set">
	scale 縮放 replica set
	</p>
	
3. 	**`apply`**  
	也可以透過修改 YAML 來更改配置，並使用 `apply` 去調整資源。
	
	<p class="illustration">
	<img src="https://i.imgur.com/KT5DA6F.png" alt="使用 apply 去調整資源">
	使用 apply 去調整資源
	</p>


### 6-8｜編碼練習：ReplicaSet

1. **Let us start with ReplicaSets! Given a blank replicaset-definition.yml file. We are only getting started with it, so let's get it populated. Add all the root level properties to it.**  
	**Note**: Only add the properties, not any values yet.  
	**答：**    
	```yaml
	apiVersion:
	kind: 
	metadata:
	spec:
	```

2. **Let us now add values for ReplicaSet. ReplicaSet is under apiVersion - apps/v1. Update values for apiVersion and kind** 

	```yaml
	apiVersion:
	kind: 
	metadata:
	spec:
	```

	**答：**  
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	spec:
	```

3. **Let us now add values for metadata. Name the ReplicaSet - frontend. And add labels app=>mywebsite and tier=> frontend.**

	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	spec:
	```

	**答：**  
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	```

4. **Let us now get to the specification. The spec section for ReplicaSet has 3 fields: replicas, template and selector. Simply add these properties. Do not add any values yet.**

	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	```

	**答：**  
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	  replicas:
	  selector:
	  template:
	```

5. **Let us update the number of replicas to 4.**

	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	  replicas:
	  selector:
	  template:
	```

	**答：**  
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	  replicas: 4
	  selector:
	  template:
	```

6. **The template section expects a Pod definition. Luckily, we have the one we created in the previous set of exercises. Next to the replicaset-definition.yml you will now find the same pod-definition.yml file that you created before.   Let us now copy the contents of the pod-definition.yml file, except for the apiVersion and kind and place it under the template section. Take extra care on moving the contents to the right so it falls under template.**

	pod-definition.yml:
	```yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  name: myapp-pod
	  labels:
		app: myapp
	spec:
	  containers:
		- name: nginx
		  image: nginx
	```  

	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	  replicas: 4
	  selector:
	  template:
	```

	**答：**  
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	  replicas: 4
	  selector:
	  template:
	  	metadata:
	  	  name: myapp-pod
	  	  labels:
	  	    app: myapp
	  	spec:
	  	  containers:
	  	    - name: nginx
	  	      image: nginx
	```

7. **Let us now link the pods to the ReplicaSet by updating selectors.  Add a property "matchLabels" under selector and copy the labels defined in the pod-definition under it.**  
	**Note**: This may not work in play-with-k8s as it runs on 1.8 version of kubernetes. ReplicaSets moved to apps/v1 in 1.9 version of Kubernetes.

	```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas: 4
      selector:
      template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
        spec:
          containers:
            - name: nginx
              image: nginx
	```
	
	**答：**  
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: frontend
	  labels:
	    app: mywebsite
		tier: frontend
	spec:
	  replicas: 4
	  selector:
	  	matchLabels:   
	  	  app: myapp
	  template:
	  	metadata:
	  	  name: myapp-pod
	  	  labels:
	  	    app: myapp
	  	spec:
	  	  containers:
	  	    - name: nginx
	  	      image: nginx
	```


### 6-9｜Hands-On Labs：ReplicaSet
- [🚪 Replica Sets](https://kodekloud.com/topic/labs-replica-sets-2/)

<br class="big">

1. **How many PODs exist on the system?**  
	**答：** 0

	<p class="illustration">
		<img src="https://i.imgur.com/ZqIs642.png" alt="How many PODs exist on the system?">
	</p>


2. **How many ReplicaSets exist on the system?**  
	**答：** 0

	<p class="illustration">
		<img src="https://i.imgur.com/bCfZomQ.png" alt="How many ReplicaSets exist on the system?">
	</p>

3. **How about now? How many ReplicaSets do you see?**  
	**答：** 1
	
	<p class="illustration">
		<img src="https://i.imgur.com/lP4bbHw.png" alt="How many ReplicaSets do you see?">
	</p>
	
4. **How many PODs are DESIRED in the new-replica-set?**  
	**答：** 4
	
	<p class="illustration">
		<img src="https://i.imgur.com/RGEcEPR.png" alt="How many PODs are DESIRED in the new-replica-set?">
	</p>
	
	
5. **What is the image used to create the pods in the new-replica-set?**  
	**答：** busybox777
	
	<p class="illustration">
	<img src="https://i.imgur.com/UWBbpbw.png" alt="What is the image used to create the pods in the new-replica-set?">
	</p>
	
6. **How many PODs are READY in the new-replica-set?**    
	**答：** 0

	<p class="illustration">
	<img src="https://i.imgur.com/c7f9MUp.png" alt="How many PODs are READY in the new-replica-set?">
	</p>
	
7. **Why do you think the PODs are not ready?**  
	**答：** The image busybox777 doesn't exist
	
	<p class="illustration">
	<img src="https://i.imgur.com/BkrIZwk.png" alt="Why do you think the PODs are not ready?">
	</p>
	
8. **Delete any one of the 4 PODs.**
	
	<p class="illustration">
	<img src="https://i.imgur.com/FnYwkEt.png" alt="Delete any one of the 4 PODs.">
	</p>

9. **How many PODs exist now?**  
	**答：** 4
	
	<p class="illustration">
	<img src="https://i.imgur.com/FnYwkEt.png" alt="How many PODs exist now?">
	</p>	

10. **Why are there still 4 PODs, even after you deleted one?**  
	**答：** replica set ensures that desired number of pods always run
	
11. **Create a ReplicaSet using the replicaset-definition-1.yaml file located at /root/.**  
	There is an issue with the file, so try to fix it.

	```yaml
	apiVersion: v1
	kind: ReplicaSet
	metadata:
	  name: replicaset-1
	spec:
	  replicas: 2
	  selector:
		matchLabels:
		  tier: frontend
	  template:
		metadata:
		  labels:
			tier: frontend
		spec:
		  containers:
		  - name: nginx
			image: nginx
	```
	
	**答：**   
	可以立刻發現是 `apiVersion` 填錯了，把它由 `v1` 改成 `apps/v1` 即可。
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: replicaset-1
	spec:
	  replicas: 2
	  selector:
		matchLabels:
		  tier: frontend
	  template:
		metadata:
		  labels:
			tier: frontend
		spec:
		  containers:
		  - name: nginx
			image: nginx
	```
	
	<p class="illustration">
	<img src="https://i.imgur.com/ra9vNby.png" alt="Create a ReplicaSet using the replicaset-definition-1.yaml file located at /root/.">
	</p>		
	
	是說如果沒有注意到的話，直接 `create` 也會跑錯誤訊息給你：
	<p class="illustration">
	<img src="https://i.imgur.com/toR1JHT.png" alt="Create replicaset-definition-1.yaml error msg">
	</p>	
	
	如果有注意到，但不確定應該換成哪個版號，則可以透過 `explain` 來取得相關資訊：
	```bash
	$ kubectl explain replicaset
	```
	
	<p class="illustration">
	<img src="https://i.imgur.com/8eTSMya.png" alt="kubectl explain">
	</p>	


12. **Fix the issue in the replicaset-definition-2.yaml file and create a ReplicaSet using it.
    This file is located at /root/.**

	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: replicaset-2
	spec:
	  replicas: 2
	  selector:
		matchLabels:
		  tier: frontend
	  template:
		metadata:
		  labels:
			tier: nginx
		spec:
		  containers:
		  - name: nginx
			image: nginx
	```
	
	**答：**   
	selector 的 label 跟 pod 的 label 無法相匹配。有兩個改法，一是改 selector 的 label，讓它與 pod 的 label 一致；另一種改法就是反過來了。
	```yaml
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: replicaset-2
	spec:
	  replicas: 2
	  selector:
		matchLabels:
		  tier: nginx
	  template:
		metadata:
		  labels:
			tier: nginx
		spec:
		  containers:
		  - name: nginx
			image: nginx
	```
	<p class="illustration">
	<img src="https://i.imgur.com/opF7X9q.png" alt="Fix the issue in the replicaset-definition-2.yaml file and create a ReplicaSet using it.">
	</p>	
	
	一樣直接跑 `create` 也會丟錯誤訊息給你：
	
	<p class="illustration">
	<img src="https://i.imgur.com/MIRyoA1.png" alt="Create replicaset-definition-2.yaml error msg">
	</p>	

13. **Delete the two newly created ReplicaSets - replicaset-1 and replicaset-2**

	<p class="illustration">
	<img src="https://i.imgur.com/5uPpcwX.png" alt="Delete the two newly created ReplicaSets - replicaset-1 and replicaset-2">
	</p>	

	對了，如果嫌 replicaSet 太長，可以直接使用縮寫 rs
	```yaml
	$ kubectl get rs
	```

14. **Fix the original replica set new-replica-set to use the correct busybox image.**  
	Either delete and recreate the ReplicaSet or Update the existing ReplicaSet and then delete all PODs, so new ones with the correct image will be created.
		
	**答：**   
	```yaml
	$ kubectl edit replicaset new-replica-set 
	```
	
	將 container 的 image 修改由 busybox777 成 busybox
	
	<p class="illustration">
	<img src="https://i.imgur.com/07HAS2p.png" alt="kubectl edit replicaset new-replica-set">
	</p>	
	
	然後刪掉就有的 pod，讓 replica set 重起 pod 以套用新的修改。
	
	<p class="illustration">
	<img src="https://i.imgur.com/OmCCfHH.png" alt="kubectl edit replicaset new-replica-set">
	</p>	
 
15. **Scale the ReplicaSet to 5 PODs.**  
 	Use kubectl scale command or edit the replicaset using kubectl edit replicaset.
	
	<p class="illustration">
	<img src="https://i.imgur.com/MrxGRZl.png" alt="Scale the ReplicaSet to 5 PODs.">
	</p>	


16. **Now scale the ReplicaSet down to 2 PODs.**  
	Use the kubectl scale command or edit the replicaset using kubectl edit replicaset.
	
		
	<p class="illustration">
	<img src="https://i.imgur.com/3NjCUdF.png" alt="Now scale the ReplicaSet down to 2 PODs.">
	</p>	



### 6-10｜Deployments
> a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features.

由[官網](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/#when-to-use-a-replicaset)的敘述可得知，K8S 建議使用 deployments 來部署 pod 與 replicaset，它可使用聲明式的方式為 pod 與 replicaset 提供設定與更新。

<p class="illustration">
	<img src="https://i.imgur.com/Dxw6RxA.png" alt="Deployment">
	Deployment（圖片來源: <a href="https://medium.com/aspnetrun/deploying-microservices-on-kubernetes-35296d369fdb">aspnetrun｜Medium</a>）
</p>


#### Pain points
在未使用 K8S 前，若要部署應用，可能會遭遇以下痛點：

1. **需手動多個 instance**  
	服務可能會需要多個元件 instance，或是基於負載會啟動多個 instance，這些都需一一手動啟動。
2. **升級麻煩**  
	當有新的 image 時，這些 instance 一一升級，費時且費力。此外，升級時為達到無停機服務遷移或為不影響使用者的使用，必須逐個升級 instance，這會需要花費更多的心力去管理。是說，這樣的更新策略有個專有名詞稱之為**滾動更新（RollingUpdate）**，這在之後的章節有詳細說明。
3. **撤消更新困難**  
	若某個升級出現意外錯誤，要撤消更新並 Rollback 到先前版本，得把升級動作反向執行，這過程非常麻煩且容易錯誤。
 

上述這些痛點正是 Deployment 特性。


#### Deployments with YAML

<p class="illustration">
	<img src="https://i.imgur.com/cb5hlUD.png" alt="Deployments with YAML">
	Deployments with YAML（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

基本上 `Deployment` 與 `ReplicaSet` 的 YAML 基本一致，只需要把 `kind` 那攔改成 `Deployment` 即可。


### 6-11｜Demo：Deployments
這邊一樣嘗試寫 YAML 並啟動。我直接複製 ReplicaSet 的 YAML 檔，並將 `kind` 改成 `Deployment` 。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cynthia-app-deployment
  labels:
    app: cynthia-app
spec:
  selector:
    matchLabels:
      tier: frontend
  replicas: 3
  template:   
    metadata:
      name: cynthia-pod-2
      labels:
        app : nginx
        tier: frontend
    spec:
      containers:
        - name: nginx
          image: nginx
```

<br class="big">

一旦準備好定義文件後，就可以透過指令 `create` 或 `apply` 來建立 deployment：

<p class="illustration">
<img src="https://i.imgur.com/aDOIFDx.png" alt="create deployment">
</p>	

<br class="big">

完成 deployment 建立後，可以查看其狀態：
<p class="illustration">
<img src="https://i.imgur.com/Ml5PHiy.png" alt="get deployment">
</p>	

<p class="illustration">
<img src="https://i.imgur.com/0sevHud.png" alt="get replicaset">
</p>	

<p class="illustration">
<img src="https://i.imgur.com/oELfxkA.png" alt="get pods">
</p>	

<br class="big">

如果一一查看各狀態有點麻煩可以直接用：
```bash
$ kubectl get all
```

<p class="illustration">
<img src="https://i.imgur.com/Gz6yrVo.png" alt="get all">
</p>	



### 6-12｜編碼練習：Deployments

1. **Let us start with Deployments! Given a deployment-definition.yml file. We are only getting started with it, so let's get it populated. Add all the root level properties to it. Note: Only add the properties, not any values yet.**  
	**答：**  
	```yaml
	apiVersion:
	kind:
	metadata:
	spec:
	```
    
2. **Let us now add values for Deployment. Deployment is under apiVersion apps/v1. Update values for apiVersion and kind.**
	```yaml
	apiVersion:
	kind:
	metadata:
	spec:
	```
	**答：**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    spec:
	```

3. **Let us now add values for metadata. Name the Deployment frontend. And add labels app=>mywebsite and tier=> frontend.**
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    spec:
	```
	**答：**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
	```

4. **Let us now get to the specification. The spec section for Deployment has 3 fields: replicas, template and selector. Simply add these properties. Do not add any values.**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
	```
	**答：**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas:
      selector:
      template:
	```

5. **Let us update the number of replicas to 4.**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas:
      selector:
      template:
	```
	**答：**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas: 4
      selector:
      template:
	```
    
6. **The template section expects a Pod definition. Luckily, we have the one we created in the previous set of exercises. Next to the deployment-definition.yml you will now find the same pod-definition.yml file that you created before. Let us now copy the contents of the pod-definition.yml file, except for the apiVersion and kind and place it under the template section. Take extra care on moving the contents to the right so it falls under template**

	pod-definition.yml:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: myapp-pod
      labels:
        app: myapp
    spec:
      containers:
        - name: nginx
          image: nginx
    ```	
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas: 4
      selector:
      template:
	```	
	**答：**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas: 4
      selector:
      template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
        spec:
          containers:
            - name: nginx
              image: nginx
	```
	 
7. **Let us now link the pods to the Deployment by updating selectors. Add a property "matchLabels" under selector and copy the labels defined in the pod-definition under it.**   
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas: 4
      selector:
      matchLabels:
        app: myapp
      template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
        spec:
          containers:
            - name: nginx
              image: nginx
	```
	**答：**  
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      labels:
        app: mywebsite
        tier: frontend
    spec:
      replicas: 4
      selector:
        matchLabels:
          app: myapp
      template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
        spec:
          containers:
            - name: nginx
              image: nginx
	```     



### 6-13｜Hands-On Labs：Deployments
- [🚪 Deployments](https://kodekloud.com/topic/labs-deployments-2/)

<br class="big">

1. **How many PODs exist on the system?**  
	**答：** 0   
	<p class="illustration">
	<img src="https://i.imgur.com/zYtD0X8.png" alt="How many PODs exist on the system?">
	</p>	

   
2. **How many ReplicaSets exist on the system?**  
	**答：** 0   
	<p class="illustration">
	<img src="https://i.imgur.com/X52bNJt.png" alt="How many ReplicaSets exist on the system?">
	</p>	
	
3. **How many Deployments exist on the system?**  
	**答：** 0   
	<p class="illustration">
	<img src="https://i.imgur.com/5S4UbXN.png" alt="How many Deployments exist on the system?">
	</p>	

4. **How many Deployments exist on the system now?**  
	**答：** 1  	  
	<p class="illustration">
	<img src="https://i.imgur.com/azDWvvM.png" alt="How many Deployments exist on the system now?">
	</p>	
	
5. **How many ReplicaSets exist on the system now?**  
	**答：** 1  	  
	<p class="illustration">
	<img src="https://i.imgur.com/ArkNJn6.png" alt="How many Deployments exist on the system now?">
	</p>	
		
6. **How many PODs exist on the system now?**  
	**答：** 4  
	<p class="illustration">
	<img src="https://i.imgur.com/YJ7W6Sq.png" alt="How many PODs exist on the system now?">
	</p>	

7. **Out of all the existing PODs, how many are ready?**  
	**答：** 0  
	<p class="illustration">
	<img src="https://i.imgur.com/D0Pxcaf.png" alt="Out of all the existing PODs, how many are ready?">
	</p>	
    
8. **What is the image used to create the pods in the new deployment?**  
	**答：** busybox888  
	<p class="illustration">
	<img src="https://i.imgur.com/Ik4w03z.png" alt="What is the image used to create the pods in the new deployment?">
	</p>	

9. **Why do you think the deployment is not ready?**  
	**答：** the image busybox888 doesn't exist  
	<p class="illustration">
	<img src="https://i.imgur.com/r1DQHxm.png" alt="Why do you think the deployment is not ready?">
	</p>	

10. **Create a new Deployment using the deployment-definition-1.yaml file located at /root/.
    There is an issue with the file, so try to fix it.**  

	```yaml
    apiVersion: apps/v1
    kind: deployment
    metadata:
      name: deployment-1
    spec:
      replicas: 2
      selector:
        matchLabels:
          name: busybox-pod
      template:
        metadata:
          labels:
            name: busybox-pod
        spec:
          containers:
          - name: busybox-container
            image: busybox888
            command:
            - sh
            - "-c"
            - echo Hello Kubernetes! && sleep 3600
    ```
    
	
	**答：**   
	直接跑 create 可以看到錯誤訊息，看起來如果不是 `apiVersion` 有誤，就是 `kind` 出差錯了。

	<p class="illustration">
	<img src="https://i.imgur.com/dvkwjmd.png" alt="Create a new Deployment using the deployment-definition-1.yaml">
	</p>  
		
    打開 YAML 可以看到是 `kind` 寫錯了，把它改成 `deployment`：
	
	```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: deployment-1
    spec:
      replicas: 2
      selector:
        matchLabels:
          name: busybox-pod
      template:
        metadata:
          labels:
            name: busybox-pod
        spec:
          containers:
          - name: busybox-container
            image: busybox888
            command:
            - sh
            - "-c"
            - echo Hello Kubernetes! && sleep 3600
	```
	
	再重新執行一次 `create` ，就能看到 deplyment 被執行了：  	  
	<p class="illustration">
	<img src="https://i.imgur.com/lo2KudY.png" alt="create">
	</p>  
	
11. **Create a new Deployment with the below attributes using your own deployment definition file.**
    - Name: httpd-frontend;
    - Replicas: 3;
    - Image: httpd:2.4-alpine

	**答**：  
	按照上面的條件寫一份 YAML，因為沒給 label，所以我直接把 name 當 label 用了 XDDD  
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: httpd-frontend
    spec:
      replicas: 3
      selector:
        matchLabels:
          name: httpd-frontend
      template:
        metadata:
          labels:
            name: httpd-frontend
        spec:
          containers:
            - name: httpd-frontend
              image: httpd:2.4-alpine
    ```  
    
	然後 `create` 就行了，你要 `apply` 也行：
    <p class="illustration">
	<img src="https://i.imgur.com/2Fil98M.png" alt="Create a new Deployment with the below attributes using your own deployment definition file.">
	</p>  

	解答則是沒寫 YAML 全用指令代入：
    <p class="illustration">
	<img src="https://i.imgur.com/XvyWTMM.png" alt="Create a new Deployment with the below attributes using your own deployment definition file.">
	</p>  


### 6-14｜Update and Rollback

<p class="illustration">
	<img src="https://i.imgur.com/pPyRp2C.png" alt=" Rolling and Rollback Deployments work">
	 Rolling and Rollback Deployments work（圖片來源: <a href="https://yankeexe.medium.com/how-rolling-and-rollback-deployments-work-in-kubernetes-8db4c4dce599">Yankee Maharjan｜Medium</a>）
</p>

這節還是跟 deployment 有關，不過這節會更詳細說明滾動更新（RollingUpdate）與回復舊版（RollBack）的功能。阿對，我對筆記內容做了調整，跟指令相關的部份我全部放到下一節跟 Demo 一併整理，這一節我只留下理論的部份。


#### Deployment Update Strategy
前一節在提及 pod 的更新，其實有隱約提過更新策略，這邊做更進一步的說明。在 K8S 中有 2 種部署策略：

1. **Recreate**   
	這策略是會先銷毀所有的 instance，再建立新版的 instance。但這部署策略的缺點是顯而易見的，<mark>在新舊版本交替間使用者是無法尋訪的</mark>。

	<p class="illustration">
		<img src="https://i.imgur.com/5UEsNkY.png" alt="Recreate">
		Recreate（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>

2. **Rolling Update**  
	這種策略與 recreate 不同的是，它並不會一次性銷毀全部的 instance，而是用新版本一個接一個地替換運行中舊版。這樣的更新可以達到<mark>無停機服務遷移</mark>的目的，但這樣的個更新方式有可能會同時出現新舊內容。
	
	在 K8S 中建置 deployment 時，若沒有指定部署策略**預設會採用 RollingUpdate**。在此基礎上，還衍生出 Ramped slow rollout、Best-effort controlled rollout 幾種變形，恩...也不算變形，嚴格來說還它們也是 Rolling Update 策略，就是針對 maxSurge 與 maxUnavailable 兩個參數的調整，用以控制更新與系統的穩定度，兩個參數我們後面再提。

	<p class="illustration">
		<img src="https://i.imgur.com/au8q8dY.png" alt="Rolling Update">
		Rolling Update（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>
	
	<br class="big">
	
	假設目前存在一個 deployment，它會建立一 replica set，創建滿足 replica 數量所需的 pod 數量。當試圖升級應用時，K8S 會建立一個新的 replica set，並按照 RollingUpdate 的策略刪除舊 replica set 中的 pod。
	
	<p class="illustration">
		<img src="https://i.imgur.com/8aneM3g.png" alt="Rolling Update 實況">
		Rolling Update 實況（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>

	如果用 `kubectl get replicasets` 觀看 replica sets，可以看到僅剩 0 個 pod 的舊 replica set 和具有 5 個 pod 的新 replica set。
 
	<p class="illustration">
		<img src="https://i.imgur.com/bjr7kKV.png" alt="Rolling Update 實況">
		Rolling Update 實況（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
	</p>

	
說到部署策略，除了 Recreate 跟 RollingUpdate 外，我在查資料時還有看到 Blue-Green Deployment（Red-Black Deployment）、 Canary deployment、 Shadow Deployment 跟 A/B Testing...等部署策略。但那些策略看起來更像 DevOps 的部署，且也不是撰寫 YAML 時會直接涉及的策略，所以這邊就先跳過吧。

如果對這幾種部署策略有興趣，可以看看這兩篇文章：
- [〈5 Kubernetes Deployment Strategies: Roll Out Like the Pros｜pot〉](https://spot.io/resources/kubernetes-autoscaling/5-kubernetes-deployment-strategies-roll-out-like-the-pros/)
- [〈Top 6 Kubernetes Deployment Strategies and How to Choose｜codefresh〉](https://codefresh.io/learn/software-deployment/top-6-kubernetes-deployment-strategies-and-how-to-choose/)


#### Revision and Rollback
在討論 rollback 前，必須先到知道 K8S 也是有版本管控的，不過這邊不是稱為 commit 而是 revision。每次 create 新的 deployment 或是升級現有 deployment 時都會觸發記錄，當然也可以主動去記錄。

就是有這些紀錄的存在，才有可能去做 rollback。至於如何進行 rollback，這就是取決於你的部署策略是 Recreate 還是 Rolling Update 了。


<p class="illustration">
	<img src="https://i.imgur.com/iR9zje7.png" alt="Revision">
	Revision（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

<br class="big">

若要回復舊版，則會刪除新的 replica set，並回覆舊的 replica set 中。 

<p class="illustration">
	<img src="https://i.imgur.com/pkn29pd.png" alt="Rollback 實況 1">
	Rollback 實況 1（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

如果用 `kubectl get replicasets` 指令比較 rollback 前後的 replica sets 狀況，可以看到新舊 replica set 的 pod 的變化。

<p class="illustration">
	<img src="https://i.imgur.com/DNB8mVJ.png" alt="Rollback 實況 2">
	Rollback 實況 2（圖片來源: <a href="https://www.udemy.com/course/learn-kubernetes/">課程截圖</a>）
</p>

#### Update Strategy with YAML

<p class="illustration">
	<img src="https://i.imgur.com/aXZBq67.png" alt="Revision and Rollback">
	Revision and Rollback
</p>


YAML 的撰寫主要延續自前面的 `Deployment`，但為因應部署策略的調整，需要在 `spec` 下添加 `strategy` 的子屬性。在 `strategy` 下最主要的是 **`type`** 屬性，這個屬性就是決定要採用 `Recreate` 或是 `RollingUpdate`。

若是最終決定採用 `RollingUpdate` 策略，在 `strategy` 下就會多一個 `rollingUpdate` 的子屬性。在這個子屬性中所要設定就是前面所提及 `maxSurge` 與 `maxUnavailable`：
- **`maxSurge`**  
   預設值為 25%。這參數是指<mark>允許超過 replicas 的最大比例</mark>。可將此數設定為整數（例如：5），或是指定為所需 pod 總數的百分比（例如：10% 但結果會四捨五入向上取整）。
   
   舉例來說，如果該值設定為 2，這表示會建立 2 個新 pod 後刪除舊的 pod。若假設 replicas 為 3，整個升級過程中最多會有 3+2 個 pod。
   
- **`maxUnavailable`**  
  預設值為 25%。該值表示在升級過程中，<mark>最多可以有幾個 pod 無法服務</mark>。跟 maxSurge 一樣，此數可設定為整數或百分比。
  
  需要注意的是，根據[文件](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#max-unavailable)說明，<mark>此值與 maxSurge 不可同時為 0；換句話說，當 maxSurge 不為零時，此值才可為 0</mark>。會特別這句話是因為我在查資料的時候看到一句話：「maxSurge 不為 0、maxUnavailable 亦不得為 0」，我就在想有什麼情況這兩個值會同時為 0，同時為 0 的話應該更新不動？看來應該是筆者筆誤了？
  


### 6-15｜Demo：Update and Rollback

開始時先建立一個 deployment：

<p class="illustration">
	<img src="https://i.imgur.com/W3XKxzH.png" alt="create deployment">
</p>

此時可以用 rollout 指令查詢部署或升級狀況：

```bash
$ kubectl rollout status deployment <deployment>
```

<p class="illustration">
	<img src="https://i.imgur.com/MoN9e0F.png" alt="kubectl rollout status">
</p>

每次 create 新的 deployment 或是升級現有 deployment 時都會觸發記錄，若想查看紀錄可以透過：
```bash
$ kubectl rollout history
```
<p class="illustration">
	<img src="https://i.imgur.com/bnRWOwX.png" alt="kubectl rollout history">
</p>

不過這樣的紀錄就像是沒有寫 commit message 的 commit，想知道究竟是記錄了什麼非常考驗記憶力，所以通常在下指令時會搭配 `--record` 使用。如此一來 history 的 change-cause 就會記錄下產生變動的指令：

<p class="illustration">
	<img src="https://i.imgur.com/nd8YqSq.png" alt="--record">
</p>


<br class="big">

完成部署後，可以來嘗試升級。升級的方式有幾種：

1. **如果想直接使用指令更改單一狀態**  
	可以用 `kubectl scale`／`kubectl set image` ...等。
2. **如果修改參數較多可以直接修改 YAML**  
	YAML 的修改有可以分成直接修改執行中暫存的 `kubectl edit`，或者直接修改最初配置的 `kubectl apply`／`kubectl replace`。不過直接修改最初配置的文件，可能會導致下次使用同一文件所產生的 deployment 不合最初預期，需要注意。
	

這邊使用 `kubectl set image` 來更新 images，並使用 `kubectl rollout status` 來觀察資源配置的狀況：

<p class="illustration">
	<img src="https://i.imgur.com/Asbwykl.png" alt="set image">
</p>

	
如果出現任何問題想暫停更新可以使用 `kubectl rollout pause`：
```bash
# 暫停滾動升級 
$ kubectl rollout pause deployment <deployment>

# 繼續滾動升級 
$ kubectl rollout resume deployment <deployment>
```

<br class="big">

關於 `Recreate` 或是 `RollingUpdate` 的差別在 describe 的 event 中也可見端倪。在使用 `Recreate` 策略時，可以看到舊的 replica set 首先縮小到 0，新的 replica set 再擴大到 5；但當使用 `RollingUpdate` 策略時，則是舊的 replica set 逐漸縮小同時新的 replica set 逐漸擴大。

<p class="illustration">
	<img src="https://i.imgur.com/XlYjbIQ.png" alt="`Recreate` 或是 `RollingUpdate` 的差別">
</p>


<br class="big">

嘗試幾次升級後，可以看到歷史紀錄中多了幾筆 revision，若想倒回去的話，則是使用：
```bash
$ kubectl rollout undo deployment <deployment>
```

<p class="illustration">
	<img src="https://i.imgur.com/twhRHB9.png" alt="rollout undo">
</p>


不過跟我想像的有點不太一樣的是，它 undo 後不太像是 git，會將最後的 commit 給移除，而是把原本的 revision 2 改成 revision 4 並移到最上層：

<p class="illustration">
	<img src="https://i.imgur.com/eGdxnha.png" alt="`Recreate` 或是 `RollingUpdate` 的差別">
</p>


<br class="big">

最後貼張指令表：
<p class="illustration">
	<img src="https://i.imgur.com/5gw4SCD.png" alt="指令表">
</p>



### 6-16｜Hands-On Labs：Update and Rollback
- [🚪 Practice Test Rolling Updates and Rollbacks](https://kodekloud.com/topic/labs-practice-test-rolling-updates-and-rollbacks/?utm_source=udemy&utm_medium=labs&utm_campaign=kubernetes)

<br class="big">

1. **We have deployed a simple web application. Inspect the PODs and the Services. Wait for the application to fully deploy and view the application using the link called Webapp Portal above your terminal.**

	它只要我們檢查，也沒要我們做啥，所以我就用了 `get all`：	
	<p class="illustration">
	<img src="https://i.imgur.com/g1bTOC8.png" alt="We have deployed a simple web application.">
	</p>	 
	
	順便依照要求看看 Portal 長怎樣：
	<p class="illustration">
	<img src="https://i.imgur.com/rFpoqll.png" alt="Webapp Portal">
	</p>	 
		
	
2. **What is the current color of the web application? Access the Webapp Portal.**

	**答：** blue  
 
 
3. **Run the script named curl-test.sh to send multiple requests to test the web application. Take a note of the output. Execute the script at /root/curl-test.sh.**
	
	就照要求跑個 script，看看結果：
	<p class="illustration">
	<img src="https://i.imgur.com/LEuHNX1.png" alt="Run the script named curl-test.sh">
	</p>	 


4. **Inspect the deployment and identify the number of PODs deployed by it.**  

	**答：** 4
	<p class="illustration">
	<img src="https://i.imgur.com/mHW3cni.png" alt="Inspect the deployment and identify the number of PODs deployed by it">
	</p>	


5. **What container image is used to deploy the applications?**  

	**答：** kodekloud/webapp-color:v1
	<p class="illustration">
	<img src="https://i.imgur.com/gmBXiMg.png" alt="What container image is used to deploy the applications">
	</p>	


6. **Inspect the deployment and identify the current strategy**

	**答：** 	RollingUpdate
	<p class="illustration">
	<img src="https://i.imgur.com/04z3cwf.png" alt="Inspect the deployment and identify the current strategy">
	</p>	
	
	
7. **If you were to upgrade the application now what would happen?**

	**答：** 	Pods are upgraded few at a time


8. **Let us try that. Upgrade the application by setting the image on the deployment to kodekloud/webapp-color:v2. Do not delete and re-create the deployment. Only set the new image name for the existing deployment.**

	透過 `edit` 直接更改 image：
	<p class="illustration">
	<img src="https://i.imgur.com/uKkjYdG.png" alt="Upgrade the application by setting the image">
	</p>

	改完後可以觀察下升級結果，不過我指令下晚了已經更新完了 XDDD
	<p class="illustration">
	<img src="https://i.imgur.com/jV02cgR.png" alt="Upgrade the application by setting the image">
	</p>

	不過可以直接從 Webapp Portal 觀看更新結果，他的底色已經從藍色變成綠色了：
	<p class="illustration">
	<img src="https://i.imgur.com/yweYnrN.png" alt="Webapp Portal">
	</p>


9. **Run the script curl-test.sh again. Notice the requests now hit both the old and newer versions. However none of them fail. Execute the script at /root/curl-test.sh.**
	<p class="illustration">
	<img src="https://i.imgur.com/LV1u61D.png" alt="Run the script curl-test.sh again">
	</p>


10. **Up to how many PODs can be down for upgrade at a time. Consider the current strategy settings and number of PODs - 4.**

	**答：** 	1  
	
	這邊 max unavailable 為 25%，$4*25\%=1$

	<p class="illustration">
	<img src="https://i.imgur.com/604lbAO.png" alt="Run the script curl-test.sh again">
	</p>


11. **Change the deployment strategy to Recreate. Delete and re-create the deployment if necessary. Only update the strategy type for the existing deployment.**

	因為我沒找到現成的 YAML 檔，所以先用 get 匯出 YAML 檔，再刪除 deployment 後重建。
	<p class="illustration">
	<img src="https://i.imgur.com/vmb9HRJ.png" alt="Run the script curl-test.sh again">
	</p>
	
	在改 YAML 檔時，除了把 type 改成 Recreate 外，別忘了把 rollingUpdate 給移除：
	<p class="illustration">
	<img src="https://i.imgur.com/4nRGXVK.png" alt="Run the script curl-test.sh again">
	</p>
	

12. **Upgrade the application by setting the image on the deployment to kodekloud/webapp-color:v3. Do not delete and re-create the deployment. Only set the new image name for the existing deployment.**  

	這邊其實跟前面的操作相仿:
	<p class="illustration">
	<img src="https://i.imgur.com/tn7bMcE.png" alt="Upgrade the application">
	</p>

	
13. **Run the script curl-test.sh again. Notice the failures. Wait for the new application to be ready. Notice that the requests now do not hit both the versions. Execute the script at /root/curl-test.sh.**
	
	截錯圖了 XDD 不過我有記得截圖 Portal，script 表示他是紅色的，但我怎麼看都是橘色的？
	<p class="illustration">
	<img src="https://i.imgur.com/JrIGWk5.png" alt="Run the script curl-test.sh again.">
	</p>



## 其他連結
1. 課程內容：[Kubernetes for the Absolute Beginners - Hands-on](https://www.udemy.com/course/learn-kubernetes/)
2. 目錄： [【K8S Beginners 筆記】目錄](https://cynthiachuang.github.io/Kubernetes-for-the-Absolute-Beginners-Contents)



## 參考資料 
1. CharyGao (2021-07-30)。[XML 與 JSON 優劣對比，TOML、CSON、YAML](https://www.twblogs.net/a/610398bf9eb5e3210c708c18)。檢自 台部落 (2022-11-11)。
2. 天府云创 (2021-04-13)。[Kubernetes之YAML语法](https://blog.csdn.net/enweitech/article/details/115674083)。檢自 天府云创的博客｜CSDN (2022-11-11)。
3. FoxuTech (2022-04-13)。[Kubectl apply vs Kubectl create.](https://foxutech.medium.com/kubectl-apply-vs-kubectl-create-8d946ed603a1) 。檢自 Medium (2022-11-16)。
4. Will 保哥 (2022-10-24)。[Kubernetes 101：釐清 kubectl create 與 kubectl apply 的差異](https://blog.miniasp.com/post/2022/10/24/Kubernetes-101-diff-between-kubectl-create-and-kubectl-apply) 。檢自 The Will Will Web (2022-11-17)。
5. Bob Reselman (2021-07-27)。[Kubectl apply vs. create: What's the difference?](https://www.theserverside.com/answer/Kubectl-apply-vs-create-Whats-the-difference)。檢自 TheServerSide (2022-11-16)。
6. 大鹏blog (2020-02-19)。[kubernetes: kubectl create与kubectl apply的区别](https://blog.csdn.net/textdemo123/article/details/104400985)。檢自 大鹏blog的博客｜CSDN (2022-11-16)。
7. (2022-11-21)。[ReplicationController 和 ReplicaSet](https://doc.cncf.vip/kubernetes-handbook/gai-nian-yu-yuan-li/controllers/replicaset)。檢自 kubernetes中文手册 (2022-11-21)。
8. godleon (2022-08-31)。[[Kubernetes] Deployment Overview](https://godleon.github.io/blog/Kubernetes/k8s-Deployment-Overview/)。檢自 小信豬的原始部落 (2022-11-23)。
9. Andy Chen (2020-02-12)。[Kubernetes 那些事 — Deployment 與 ReplicaSet（一）](https://medium.com/andy-blog/kubernetes-那些事-deployment-與-replicaset-一-406234a63d43) 。檢自  Andy的技術分享blog｜Medium (2022-11-23)。
10. Akhil Chawla (2021-09-02)。[Deployment Strategies In Kubernetes](https://auth0.com/blog/deployment-strategies-in-kubernetes/)。檢自 auth0 (2022-11-24)。
11. [Top 6 Kubernetes Deployment Strategies and How to Choose](https://codefresh.io/learn/software-deployment/top-6-kubernetes-deployment-strategies-and-how-to-choose/)。檢自 codefresh (2022-11-24)。
12. [5 Kubernetes Deployment Strategies: Roll Out Like the Pros](https://spot.io/resources/kubernetes-autoscaling/5-kubernetes-deployment-strategies-roll-out-like-the-pros/)。檢自 Spot by NetApp (2022-11-24)。
13. Kevin Yang (2021-07-16)。[[K8s] 開始學習 Kubernetes - Deployment Strategies](https://blog.kevinyang.net/2021/07/16/k8s-note-002/)。檢自 CK's Notepad (2022-11-24)。
14. sixinshuier (2020-08-26)。[文kubectl create / replace 与kubectl apply 的区别章文稱](https://www.cnblogs.com/shix0909/p/13566148.html)。檢自 sixinshuier｜博客园 (2022-11-25)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2022-12-30</summary>
  <ul>
    <li>2022-12-30 發布</li>
    <li>2022-11-25 完稿</li>
    <li>2022-11-04 起稿</li>
  </ul>
</details>