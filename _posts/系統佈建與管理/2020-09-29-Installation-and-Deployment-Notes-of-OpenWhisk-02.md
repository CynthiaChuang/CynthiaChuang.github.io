---
title: 【Survey】OpenWhisk 折騰安裝部署筆記 02
date: 2020-12-31 23:21
is_modified: false
categories:
- 系統佈建與管理
- 雲端運算
tags:
- Fass/Serverless
- OpenWhisk
- Linux/Unix
- Fass/Serverless
--- 

繼續被 OpenWhisk 折騰... orz 
  
[前一篇](/Installation-and-Deployment-Notes-of-OpenWhisk-01)大概看了下基本的系統架構與限制後，這篇開始試著安裝 OpenWhisk。

<!--more-->
<br>

## Deploys anywhere
從 [Openwhisk 官網](http://openwhisk.incubator.apache.org/)上看到，OpenWhisk 可以利用一些常見的 Container 框架，例如： Kubernetes and OpenShift, Mesos and Compose 快速部屬。

<center> <img src="http://openwhisk.apache.org/images/illustrations/OW-Deployments.png" alt="The Learning Model"></center>
<center class="imgtext">Deploys anywhere（圖片來源: <a href="http://openwhisk.apache.org/" class="imgtext">Openwhisk</a>）</center>
<br>

從 Apache 的 [Githib](https://github.com/apache) 上，找到了幾份部屬的教學：
- [Kubernetes](https://github.com/apache/openwhisk-deploy-kube)
- [Openshift](https://github.com/apache/openwhisk-deploy-openshift)
- [Mesos](https://github.com/apache/openwhisk-deploy-mesos)
- [Compose](https://github.com/apache/openwhisk-devtools/tree/master/docker-compose)

研究了一下安裝難度與實際需要部屬的環境考量，最終決定挑 Kubernetes 來快速部屬。
 
<br><br>

## 前置安裝
既然決定要利用 [Kubernetes](https://kubernetes.io/)，那當然需要先將它部署起來。另外為了簡化 Kubernetes 叢集的部署管理，也安裝了 [Helm](https://helm.sh/)。

###  Kubernetes ＆ kind
要弄出測試用的 Kubernetes 小型叢集，最簡單的方法是使用 <span class='highlighting'>Docker-in-Docker</span>，直接在 Docker 上運行 Kubernetes。這邊為了迅速上手使用 <span class='highlighting'>kind</span> 這套工具，可以快速架設多節點的 Kubernetes 叢集環境。

對了，在 OpenWhisk 中配置的 Docker 預設須具備至少 4 GB 記憶體和 2 個 virtual CPUs。 

<center> <img src="https://i.imgur.com/Ncg5fgi.png" alt="kind"></center>
<center class="imgtext">kind（圖片來源: <a href="https://github.com/kubernetes-sigs/kind" class="imgtext">kind｜GitHub</a>）</center>
<br>

#### Step 1、安裝 kind
按照 [kind 安裝文件](https://github.com/kubernetes-sigs/kind)的說明，安裝 kind 的方法有二：
1. `GO111MODULE`
2. 直接下載原始碼來配置路徑

考慮到我的電腦有裝過 [Golang](https://www.runoob.com/go/go-environment.html) 所以決定用 `GO111MODULE` 來安裝，不過還是先用指令檢查下 Golang 版本：

``` bash
$ go version
go version go1.14 linux/amd64
```

可以看到版本為 `1.14` 符合文件中 `1.13` 以上的要求。確定版本後，就可以可以用 go module 來安裝：
```bash
$ GO111MODULE="on" go get sigs.k8s.io/kind@v0.7.0  
```

<br>

#### Step 2、建立 Kubernetes 叢集
安裝完 kind 後，就可以進行叢集的配置。這個的配置難度不高，基本上按照[設置文件](https://github.com/apache/openwhisk-deploy-kube/blob/master/docs/k8s-kind.md)就可以完成。

首先先建立一個 yaml 檔，並取名為 `kind-cluster.yaml`：

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
  extraPortMappings:
    - hostPort: 31001
      containerPort: 31001
- role: worker
```
<br>

完成 ymal 檔撰寫後，就可以使用 kind 指令開始配置 Cluster，其中指令中的 `--name` 如果沒有指定，預設會是 `kind`：
```bash
$ kind create cluster --config kind-cluster.yaml --name kind-cluster
Creating cluster "kind-cluster" ...
 ✓ Ensuring node image (kindest/node:v1.17.0) 🖼 
 ✓ Preparing nodes 📦 📦 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-kind-cluster"
You can now use your cluster with:

kubectl cluster-info --context kind-kind-cluster

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
```
<br>


#### Step 3、 Kubernetes 叢集確認
安裝完成後，可以用 `get clusters` 指令來確定剛剛所建立的叢集：
```bash
$ kind get clusters
kind-cluster
```
<br>

如果用 `docker ps` 來觀察目前啟動的 container，也會發現多了 3 個。另外，試試看 `create` 指令中最後一個步驟的提示，可以看到一些詳細資料：

```bash
$ kubectl cluster-info --context kind-kind-cluster
Kubernetes master is running at https://127.0.0.1:32778
KubeDNS is running at https://127.0.0.1:32778/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```
<br>

在網路上找尋安裝資訊的時候，看到有些指令提示中會多出 `export KUBECONFIG` 的指令，
```bash
$ kind create cluster --name moelove
Creating cluster "moelove" ...
 ✓ Ensuring node image (kindest/node:v1.13.4) 🖼
 ✓ Preparing nodes 📦
 ✓ Creating kubeadm config 📜
 ✓ Starting control-plane 🕹️
Cluster creation complete. You can now use the cluster with:

export KUBECONFIG="$(kind get kubeconfig-path --name="moelove")"
kubectl cluster-info
```
  
看[文字說明](https://kubernetes.io/docs/tasks/tools/install-kubectl/#verifying-kubectl-configuration)，這條指令像是配置 kubectl 使用的。我這邊指令提示中沒出現這行，我有點不確定是因為 kind 版本的關係，或者是因為之前同仁在這台機台有配置過 kubectl？
  
<br>
 
除了上述資訊外，還有一個 `kubectl config view`，也可以用來查看配置結果：
```bash
$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://127.0.0.1:32778
  name: kind-kind-cluster
contexts:
- context:
    cluster: kind-kind-cluster
    user: kind-kind-cluster
  name: kind-kind-cluster
current-context: kind-kind-cluster
kind: Config
preferences: {}
users:
- name: kind-kind-cluster
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
```
<br>

小插曲，第一次建立的時候，我發現我 cluster 拼錯成 cluste，只好把錯的叢集刪掉重新建立：

```bash
$ kind delete cluster --name kind-cluste
```

<br>

### Helm
<center> <img src="https://i.imgur.com/hR6ZGds.png?1" alt="Helm"></center>
<center class="imgtext">Helm（圖片來源: <a href="https://medium.com/@C.W.Hu/kubernetes-helm-chart-tutorial-fbdad62a8b61f" class="imgtext">Cheng-Wei Hu｜Medium</a>）</center>
<br>

另外一個需要先安裝的工具是 [Helm](https://github.com/helm/helm)，它是一個管理設定檔的工具，可以簡化 Kubernetes 叢集上應用程式的部署和管理。此外 OpenWhisk 中的 Helm chart 也需要 Helm 3。

Helm 所提供的安裝方式還不少，有直接下載 [binary file](https://github.com/helm/helm/releases) 、使用 Snap（Linux） 以及 shell script 的方式，就是安裝時要稍微注意一下版本，按文件要求必須安裝 Helm v3.0.1 以上的版本，安裝可以參考這份[教學](https://whmzsu.github.io/helm-doc-zh-cn/quickstart/install-zh_cn.html#%E5%AE%89%E8%A3%85-helm-%E5%AE%A2%E6%88%B7%E7%AB%AF)。

安裝後，試著使用 `helm help` 來確認是否安裝成功：

```bash
$ helm help
The Kubernetes package manager

Common actions for Helm:

- helm search:    search for charts
- helm pull:      download a chart to your local directory to view
- helm install:   upload the chart to Kubernetes
- helm list:      list releases of charts

Environment variables:
...（略）
```

<br><br>
 
## 部署
有了 K8S 叢集與 Helm 後，就可以使用 Helm 將 OpenWhisk 部署到 K8S 叢集上了，這邊依照文件的四個部屬動作來進行：

<br>

### Step 1. 初始化叢集設定
在[前置配置 Kubernetes 叢集](#Step-2、建立-Kubernetes-叢集)時，我們在 `kind-cluster.yaml` 中宣告了三個節點，其中一個節點作為 control-plane，另外兩個節點作為 worker：

```yaml
- role: control-plane
- role: worker
  extraPortMappings:
    - hostPort: 31001
      containerPort: 31001
- role: worker
```

這步驟中，我們要標記 Kubernetes 中每個 worker 的預計用途，按文件說明這邊將一個節點保留給 invoker，另一個用運行 OpenWhisk 系統： 

```bash
$ kubectl label node {name}-worker openwhisk-role=core 
node/{name}-worker labeled

$ kubectl label node {name}-worker2 openwhisk-role=invoker
node/{name}-worker2 labeled
```
<br>

:warning: 注意，請把指令中 `{name}` 換成建立叢集所取的名字，以上面的例子來說就是 `kind-cluster`，如果在建立叢集時沒特別命名，則會是預設 `kind`。名字打錯的話，它會跟你說 <span class='highlighting'>NotFound</span> 喔！

```bash
$  kubectl label node kind-worker openwhisk-role=core
Error from server (NotFound): nodes "kind-worker" not found
```
 
<br>

### Step 2. 定義 yaml
要配置 OpenWhisk，首先需要定義一個 `mycluster.yaml` 來指定一些入口訊息和其他的系統配置。不過，在此之前需要先用下列指令確定輔助節點的 internalIP：

```bash
$ kubectl describe node kind-cluster-worker | grep InternalIP: | awk '{print $2}'
172.17.0.4
```

得到一個實際的 \<INTERNAL_IP\> ，這個 IP 稍後會用於 `mycluster.yaml`。

```bash
whisk:
  ingress:
    type: NodePort
    apiHostName: <INTERNAL_IP>
    apiHostPort: 31001

invoker:
  containerFactory:
    impl: "kubernetes"

nginx:
  httpsNodePort: 31001
```
<br>

### Step 3. 使用 Helm Chart 配置 
準備好 `mycluster.yaml` 後，就可以使用 Helm 來將 OpenWhisk 部署到 Kubernetes 集群。在開始前，先建立個 namespace 名為 `openwhisk`：

```bash
$ kubectl create namespace openwhisk
namespace/openwhisk created
```
<br>

然後[下載 openwhisk-deploy-kube](https://github.com/apache/openwhisk-deploy-kube.git)，並確認專案中資料夾 `helm/openwhisk` 是否存在。若存在，就可以直接透過指令安裝 OpenWhisk 的 Chart。其中指令中的 `owdev` 是 release name，`openwhisk` 則是我們上一個步驟中所建立的 namespace：

```bash
$ helm install owdev ./helm/openwhisk -n openwhisk -f mycluster.yaml
NAME: owdev
LAST DEPLOYED: Fri Apr 10 15:32:13 2020
NAMESPACE: openwhisk
STATUS: deployed
REVISION: 1
NOTES:
Apache OpenWhisk
Copyright 2016-2018 The Apache Software Foundation

This product includes software developed at
The Apache Software Foundation (http://www.apache.org/).

To configure your wsk cli to connect to it, set the apihost property
using the command below:

  $ wsk property set --apihost <whisk.ingress.apiHostName>：<whisk.ingress.apiHostPort> 

Your release is named owdev.

To learn more about the release, try:

  $ helm status owdev [--tls]
  $ helm get owdev [--tls]

Once the 'owdev-install-packages' Pod is in the Completed state, your OpenWhisk deployment is ready to be
 used.

Once the deployment is ready, you can verify it using: 
 $ helm test owdev [--tls] --cleanup
```
<br>

過程中，需要稍微留意下安裝過程中所出現的 `<whisk.ingress.apiHostName>：<whisk.ingress.apiHostPort>` ，這組 HostName 與 HostPort 稍後會到用。但如果沒注意到也沒關係，可藉由下列指令再看一次：
```bash
$ helm status owdev -n openwhisk
```

不過，其實這兩個值是在 `mycluster.yaml` 中所定義的 `apiHostName` 與 `apiHostPort`。
 
<br>

### Step 4. 配置 wsk CLI. 
配置完 Helm Chart 後，就可以通過設置 auth 和 apihost 配置 wsk，以告訴 wsk CLI 如何連接到 OpenWhisk。不過開始配置前要先安裝完 [wsk CLI](https://github.com/apache/openwhisk-cli)。

 
```bash
$ wsk property set --apihost <whisk.ingress.apiHostName>:<whisk.ingress.apiHostPort>
ok: whisk API host set to 172.17.0.4:31001

$ wsk property set --auth 23bc46b1-71f6-4ed5-8c54-816aa4f8c502:123zO3xZCLrMN6v2BKK1dXYFpXlPkccOFqm12CdAsMgRU4VrNZ9lyGVCGuMDGIwP
ok: whisk auth set. Run 'wsk property get --auth' to see the new value.
```
<br>

### Step 5. 驗證 OpenWhisk 部署  
安裝完成後，就可以進行驗證了：

```bash
$ helm test owdev -n openwhisk
Pod owdev-tests-package-checker pending
Pod owdev-tests-package-checker pending
Pod owdev-tests-package-checker pending
Pod owdev-tests-package-checker running
Pod owdev-tests-package-checker succeeded
Pod owdev-tests-smoketest pending
Pod owdev-tests-smoketest pending
Pod owdev-tests-smoketest pending
Pod owdev-tests-smoketest running
Pod owdev-tests-smoketest succeeded
NAME: owdev
LAST DEPLOYED: Fri Apr 10 15:32:13 2020
NAMESPACE: openwhisk
STATUS: deployed
REVISION: 1
TEST SUITE:     owdev-tests-package-checker
Last Started:   Fri Apr 10 15:42:58 2020
Last Completed: Fri Apr 10 15:43:02 2020
Phase:          Succeeded
TEST SUITE:     owdev-tests-smoketest
Last Started:   Fri Apr 10 15:43:02 2020
Last Completed: Fri Apr 10 15:43:07 2020
Phase:          Succeeded
NOTES:
Apache OpenWhisk
Copyright 2016-2018 The Apache Software Foundation

This product includes software developed at
The Apache Software Foundation (http://www.apache.org/).

To configure your wsk cli to connect to it, set the apihost property
using the command below:

  $ wsk property set --apihost <whisk.ingress.apiHostName>：<whisk.ingress.apiHostPort> 

Your release is named owdev.

To learn more about the release, try:

  $ helm status owdev [--tls]
  $ helm get owdev [--tls]

Once the 'owdev-install-packages' Pod is in the Completed state, your OpenWhisk deployment is ready to be used.

Once the deployment is ready, you can verify it using: 

  $ helm test owdev [--tls] --cleanup
```
<br>

另外一個相對應的刪除指令，可以用來刪除所有已部署的 OpenWhisk 組件：
```bash
$  helm uninstall owdev -n openwhisk
```
<br>

把整個 [Openwhisk](https://github.com/apache/openwhisk) 全弄來好了後，就可測試是否安裝成功，下列指令中  `WHISK_SERVER` 和 `WHISK_AUTH` ，可以使用 `wsk property get --apihost` and `wsk property get --auth`  來查訊：

```bash
cd openwhisk
$ ./gradlew :tests:testSystemBasic -Dwhisk.auth=$WHISK_AUTH -Dwhisk.server=https://$WHISK_SERVER -Dopenwhisk.home=`pwd`
```
<br>

它會給出一個測試結果：
<center> <img src="https://i.imgur.com/47SoEgK.png" alt="測試結果"></center>
<center class="imgtext">測試結果</center>

<br><br>

## 測試
這邊試著建立觸發器（Trigger）、動作（Action）與規則（Rule）。

### action
1. **action with python**  
    這邊先準備一個用 python 所撰寫的 action，取名為 `catapi.py` XDDD，不過我懶的實做 function，所以我直接把傳進來的參數，直接丟回去：     
    ```python
    def main(args):
        name = args.get("name", "stranger")
        color = args.get("color", "white")
        command = "INSERT INTO catdb VALUES( {} , {} )".format(name,color)
        print(command)
        return {"command":command}  
    ```
    <br>

    準備好程式碼後，就使用指令建立 action，其中下列指令中的：
    1. `catapi` ；是指 ACTION_NAME。
    2. `catapi.py` ：則是對應的 action code。
    3. `kind` ：的 flag 則是指定 action code 所撰寫的語言。
    4. `web` ：則是告知你的 action 是否允許通過 url 溝通。
    5. `-i` ：則是 skip 掉憑證檢查，因為我是在 local 端測試所以就跳掉憑證檢查。如果是正式上線產品，才會需要加憑證功能，我看大家都用 openssl  去生成。

    ```shell
    $ wsk action create catapi catapi.py --kind python:3 --web true -i $ wsk action create catapi catapi.py --kind python:3 --web true -i
    ok: created action catapi
    ```
    <br>

    建立了後，就可以透過 invoke 來呼叫這些 action，會顯示一組回傳結果：

    ```shell
    $ wsk action invoke catapi --blocking -r --param name Cynthia --param color black -i
    {
        "command": "INSERT INTO catdb VALUES( Cynthia , black )"
    }
    ```

    另外，也可以透過 activation list 來看到 action 的活動情況：    

    ```bash
    $ wsk -i activation list 
    catapiDatetime            Activation ID                    Kind     Start Duration   Status  Entity
    2020-04-13 11:05:55 3eb4e20951ed4194b4e20951ed919498 python:3 cold  11ms       success guest/catapi:0.0.1
    ```
    <br> 

    因為我剛剛 `web` 設置為 true，允許使用 url 調用這個 action。這邊先將其建立為 api 並取得其 api path：    
    ```bash
    $ wsk -i api create /catapi /insert post catapi --response-type json
    ok: created API /catapi/insert POST for action /_/catapi
    https://172.17.0.4:31001/api/23bc46b1-71f6-4ed5-8c54-816aa4f8c502/catapi/insert

    $ curl --request POST \
     --url https://172.17.0.4:31001/api/23bc46b1-71f6-4ed5-8c54-816aa4f8c502/catapi/insert \
    --header "Content-Type: application/json" \
    --data '{"name":"CC", "color":"black"}' \ 
    -k

    {
      "command": "INSERT INTO catdb VALUES( CC , black )"
    }
    ```
    <br>

2. **action with bash**  
    這邊試著建立一個 bash 的 action，雖然[明面上所支援的語言](/Installation-and-Deployment-Notes-of-OpenWhisk-01#動作-Action)並不包括 bash，但在 [GitHub 的討論](https://github.com/apache/openwhisk/issues/2927)中發現，有人提到默認的 docker 框架實際上是可以執行 bash 腳本的，因此這邊按照[這篇網誌](https://medium.com/openwhisk/serverless-functions-in-your-favorite-language-with-openwhisk-f7c447558f42)試著來建立 bash 的 action。

    在開始之前，先準備一個名為 `flip.sh` 的程式碼，它是用來擲硬幣 N 次並返回拋擲硬幣的結果。

    ```bash
    #!/bin/bash
    # install jq if it does not exist
    # openwhisk 提供的docker 有裝
    if [ ! -f  /usr/bin/jq ]; then
      apk update && apk add jq
      # 取決於環境, 但 openwhisk 提供的環境應該用上面那條指令
     # apt-get update && apt-get install jq 
    fi

    # determine number of flips
    N=`echo "$@" | jq '."n"'`

    # total count of heads and tails
    HEADS=0
    TAILS=0

    for i in `seq 1 $N`; do
      echo -n "flipping coin..."
      if [ $(( RANDOM % 2 )) == 0 ]; then
        echo "HEADS"; HEADS=$(( HEADS + 1 ))
      else
        echo "TAILS"; TAILS=$(( TAILS + 1 ))
      fi
    done

    echo "{\"trials\": $N, \"heads\": $HEADS, \"tails\": $TAILS}"
    ```

    按照網誌中的指示，使用 native actions 來建立。

    ```bash
    $ wsk -i action create flip --native flip.sh --web true
    ok: created action flip

    $ wsk -i action invoke flip --result --param n 100
    {
        "heads": 47,
        "tails": 53,
        "trials": 100
    }
    ```

    除了使用 action create 來執行 bash 外，在下方也有看到[大神的回覆](https://github.com/apache/openwhisk/issues/2927#issuecomment-347018584)，表示可以透過修改 [runtime manifest](https://github.com/apache/openwhisk/blob/master/ansible/group_vars/all#L38) 的 [方式](https://github.com/apache/openwhisk/blob/master/docs/actions-new.md) 來將 bash 加入 kind 的候選列中。  
    
    P.S. 我在後來找不到 runtime manifest，它好像變成 [runtimes.json](https://github.com/apache/openwhisk/blob/556cd3d7e2fb12a3419555411b181bc1c080af8a/ansible/files/runtimes.json)？
    

    <br>

3. **action with C**   
    再來根據[文件](https://github.com/apache/openwhisk/blob/master/docs/actions-docker.md#example-static-c-binary)的敘述試著建立 C 語言的 action。

    先準備一個 C 的程式碼取名叫做 `main.c`：
    ```cpp
    #include <stdio.h>
    int main(int argc, char *argv[]) {
        printf("This is an example log message from an arbitrary C program!\n");
        printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
               (argc == 1) ? "undefined" : argv[1]);
    }
    ```

    並使用 gcc 對原始碼進行編譯後，將編譯結果打包壓縮檔案：
    ```shell
    $ gcc main.c -o exec
    $ zip -r action.zip exec
    ```

    最後一樣選擇使用 native actions，並傳入打包好的壓縮。
    ```bash
    $ wsk -i  action create c-binary action.zip --native
    ok: created action c-binary
    $　wsk -i action invoke c-binary --result --param name James
    {
        "args": {
            "name": "James"
        },
        "msg": "Hello from arbitrary C program!"
    }
    ```

    <br>

4. **action with C++**  
    最後試著建立一個 C++ 的 action，這邊預期可以像 C 一樣，先進行編譯後使用 native 建立 action。所以還是先準備一個原始碼 `helloworld.cpp`
 
 
    ```cpp
    #include <iostream>
    using namespace std;

    int main(){
        cout << "{ \"msg\": \"Hello from arbitrary C++ program!\"}" ;
    }         
    ```

    接下來將原始碼進行編譯後打包：
    ```shell
    $ g++ helloworld.cpp -o exec
    $ zip -r action.zip exec 
    ```
 
    最後建立並試著調用 action，看來沒辦法用相同的辦法
    ```shell
    $ wsk -i action create cplus action.zip --native
    ok: created action cplus

    $ wsk -i action invoke cplus3 --result
    {
        "error": "The action did not return a dictionary."
    }
    ```

    試著印出 log 看能不能有點頭緒：     
    ```shell
    wsk activation logs -i 1518db43520746e098db43520756e036
    2020-04-14T09:55:03.774174764Z stdout: Error loading shared library libstdc++.so.6: No such file or directory (needed by /action/exec)
    2020-04-14T09:55:03.774207363Z stdout: Error relocating /action/exec: _ZStlsISt11char_traitsIcEERSt13basic_ostreamIcT_ES5_PKc: symbol not found
    2020-04-14T09:55:03.774216897Z stdout: Error relocating /action/exec: _ZNSt8ios_base4InitC1Ev: symbol not found
    2020-04-14T09:55:03.774225361Z stdout: Error relocating /action/exec: _ZNSt8ios_base4InitD1Ev: symbol not found
    2020-04-14T09:55:03.774232412Z stdout: Error relocating /action/exec: _ZSt4cout: symbol not found
    2020-04-14T09:55:03.774244895Z stdout: 
    2020-04-14T09:55:03.776577246Z stderr: The action did not initialize or run as expected. Log data might be missing.
    ```
    
    可以發現錯誤訊息顯示沒有 libstdc++ 的函式庫。拜了下大神，[發現](https://github.com/kohlschutter/junixsocket/issues/33#issuecomment-273255073) openwhisk/dockerskeleton 是 alpine 體系的 docker ，本身**不含 libstdc++.so.6**，所以我如果想執行 C++，必須 build 新的 image 了。
    
    至於如何如何用 docker 建立 action 可以參考[這份教學](https://github.com/apache/openwhisk/blob/master/docs/actions-docker.md)，這邊紀錄一下 dockerfile：
    
    ```dockerfile
    # Dockerfile for example whisk docker action
    FROM openwhisk/dockerskeleton

    ENV FLASK_PROXY_PORT 8080

    ### Add source file(s)
    ADD cpp_code/helloworld.cpp /action/helloworld.cpp

    RUN export http_proxy=http://87.xx.xx.120:8080 && \
        export https_proxy=http://87.xx.xx.120:8080 && \
        apk add --no-cache --virtual .build-deps \
            bzip2-dev \
            g++ \
            libc-dev \
    ### Compile source file(s)
     && cd /action; g++ -o exec helloworld.cpp \

    CMD ["/bin/bash", "-c", "cd actionProxy && python -u actionproxy.py"]
    ```

    <br>
    
5. **action with other**  
    至於其他與研究不一一嘗試嘗試了，可以直接從 [Openwhisk 官方文件](https://openwhisk.apache.org/documentation.html#actions-creating-and-invoking)中了解其他內建語言的基礎用法。

<br>

### 第三方函式庫
忽然想到如果如果有相依函式庫的怎麼辦？如果是編譯語言應該連同相依函式庫一起傳入應該可行，但是直譯語言就必須來找找了。還好最後有找到[文件](https://github.com/apache/openwhisk/blob/master/docs/actions-python.md#packaging-python-actions-with-a-virtual-environment-in-zip-files)。

一樣準備一支原始碼 `__main__.py` ，根據我看到資料好像一定要叫這個？
```python
import numpy;

def main(args):
    return {"version":numpy.version.version}
```

另外根據你所使用的函式庫，提供一份 `requirements.txt`，並將兩者把包成壓縮檔。

```typescript
numpy
```

最後接著試著建立與調用。
```shell
$ wsk action create numpytext --kind python:3 helloPython.zip -i
ok: created action numpytext

$ wsk -i action invoke numpytext --result
{
    "version": "1.18.2"
}
```
 
<br><br>

## 其他

### CouchDB 
在上一章 Survey [整體系統架構](/Installation-and-Deployment-Notes-of-OpenWhisk-01#系統架構)時，有提到 CouchDB，不過啟動過程中完全沒有看到它的蹤影。

就[文件](https://github.com/apache/openwhisk-deploy-kube/blob/master/docs/configurationChoices.md#using-an-external-database)上來看，CouchDB 需要[額外配置並進行設定](https://github.com/apache/openwhisk/blob/master/tools/db/README.md#using-couchdb)，不然 CouchDB 的生命週期會與 OpenWhisk 的生命週期綁在一起，不利於錯誤修復與備援。

是說在文件中有提到，如果不想用 CouchDB，另一個選擇則是 Cloudant。但沒記錯的話 Cloudant 是 IBM 家的產品？所以其實等於沒有選擇...

<br>

### Redis
另外因為目前配置沒有設定 [Redis](https://github.com/apache/openwhisk-deploy-kube/blob/master/docs/configurationChoices.md#using-an-external-redis)，是用在 API Gateway 也就是 NGINX 會使用到它，不過現階段沒有特別去處理它，如果真要上限，這部分也要額外配置。

<br><br> 

## 評估
1. **需支援不同的程式語言，包含：Go、Java、Python、C++、C、C# 與 Bash**  
    前面[測試](/Installation-and-Deployment-Notes-of-OpenWhisk-02#action)的時候有嘗試過，上述語言都可以完成支援。其中Go、Java、Python、C# 為[內建支援](/Installation-and-Deployment-Notes-of-OpenWhisk-01#動作-Action)的語言，而 C 與 Bash 可透過建立 native actions 來實現，但 C++ 就必須提供 docker image。

2. **確認如何支援第三方Library，以及是否有檔案大小的限制**  
    1. 以 [python 為例](https://github.com/apache/openwhisk/blob/master/docs/actions-python.md#packaging-python-actions-with-a-virtual-environment-in-zip-files)，若是有第三方套件，則將所需的 requirements.txt 與程式碼包成 zip 後上傳。
    2. 否，有檔案大小的限制。  
       在之前的 Survey [系統限制](/Installation-and-Deployment-Notes-of-OpenWhisk-01#Actions)時，說明有列出檔案大小僅支援 48MB，因此若壓縮檔超出檔案大小限制，則須改用 docker。
    
    <br>
    
3. **支援的編譯語言要能在平台上進行編譯，並能選擇編譯的環境，例如 Java 選擇 JDK 版本，Golang 選擇 Golang 版本**  
    1. 目前各個平台都無法在直接在平台上進行編譯，create action 時必須提供編譯好的結果或是直譯式的原始碼。這段真要做需要 UI 配合。
    2. 如要不同的語言版本必須提供不同版本的 runtime docker，並修改 runtimes.json。  
    
    P.S. 是說文件中有看到， 可以用[直譯的方式](https://openwhisk.apache.org/documentation.html#actions-creating-and-invoking)，直接使用原始碼來建立 action 來執行程式 Golang，但也可以[先行編譯](https://github.com/apache/openwhisk/blob/master/docs/actions-go.md#precompiling-go-sources-offline)。

 
4. **直譯式語言要能夠選擇 runtime 版本**    
    同上，如要不同的語言版本必須提供不同版本的 runtime docker，並修改 runtimes.json。

5. **針對功能呼叫次數與使用資源進行計費**    
    可以參考 IBM 的計費機制，畢竟 IBM 底層也是 OpenWhisk。
    
    IBM 是根據每秒每個 GB 的記憶體收費的，每秒的執行對每個 GB 的配置記憶體收費為 0.000017，所以在設定 action 時，可以配置執行工作所需的最大記憶體，就可以進一步降低成本。  
    
    在這邊可以利用相關指令，分別取得每次的執行時間與 limit 資訊：   
    ```shell
    $ wsk activation list -i 
    $ wsk action get [action_name] -i
    ```
    <br>

6. **每個函數設定不同的資源用量限制，例如：RAM 或是 CPU**     
    在 `wsk action create` 的參數中，有些相關的 flag 可以用：    
    ```
    $ wsk action create --help
    create a new action
    Usage:
      wsk action create ACTION_NAME ACTION [flags]

    Flags:
        -m, --memory LIMIT the maximum memory LIMIT in MB for the action (default 256)
        -t, --timeout LIMIT the timeout LIMIT in milliseconds after which the action is terminated (default 60000)
        -l, --logsize LIMIT the maximum log size LIMIT in MB for the action (default 10)
    ```
    <br>

7. **每個相關功能進行監控**    
    <center> <img src="https://i.imgur.com/LGWTKrb.png" alt="監控"></center>
    <br>

    這是 IBM 的 UI 界面，界面中的活動摘要、活動時間都可以從 `wsk activation list` 取得
   ```shell
    $ wsk activation list hello -i
    Datetime            Activation ID                    Kind      Start Duration   Status  Entity
    2020-04-10 16:48:19 327771e4092449fcb771e4092439fcac nodejs:10 cold  48ms       success guest/hello:0.0.1
    2020-04-10 15:44:16 2ee94a612f684db2a94a612f68edb26b nodejs:10 warm  3ms        success guest/hello:0.0.1
    2020-04-10 15:44:16 0bd20f54c7e54718920f54c7e50718c5 nodejs:10 warm  3ms        success guest/hello:0.0.1
    2020-04-10 15:44:16 ad69c9ee77aa4be9a9c9ee77aacbe929 nodejs:10 warm  4ms        success guest/hello:0.0.1
    2020-04-10 15:43:06 7a1fb66a583e404e9fb66a583e704e9c nodejs:10 warm  5ms        success guest/hello:0.0.1
    2020-04-10 15:43:04 9f38e9c4706143c2b8e9c47061c3c23c nodejs:10 warm  4ms        success guest/hello:0.0.1
    2020-04-10 15:43:04 e94847e0d77b4f058847e0d77b3f05fb nodejs:10 cold  49ms       success guest/hello:0.0.1
    ```
    
    而 log 與返回的 action 結果，也可以藉由下列指令取得：
    ```shell
    $ wsk activation logs [Activation ID] -i
    $  wsk activation result [Activation ID ] -i
    ```

    
    ```shell
    $ wsk activation poll
    ```
    <br>

8. **提供管理網頁介面讓每個用戶對每個功能進行管理，包括線上編寫函數程式碼、建立、刪除、監控函數。**    
    管理網頁介面需要由 UI 實做，不過各功能可以藉由 cli 完成： 
    - action
        - **create**：create a new action
        - **update**：update an existing action, or create an action if it does not exist
        - **invoke**：invoke action
        - **get**：get action
        - **delete**：delete action
        - **list**：list all actions in a namespace or actions contained in a package
    
    除了更新的功能，但是編譯語言的更新還是需要重新編譯。
    

<br><br> 


## 小結
媽呀！終於告一段落！  

雖然沒有寫得很詳細，很多東西沒有寫到，不過算是告一個段落了。


<br><br> 

## 參考資料 
1. [Openwhisk 官網](http://openwhisk.incubator.apache.org/) 。檢自 Openwhisk (2020-09-29)。
2. 協同撰寫。[OpenWhisk Deployment on Kubernetes](https://github.com/apache/openwhisk-deploy-kube/blob/master/README.md)。檢自 openwhisk-deploy-kube｜GitHub (2020-09-30)。
3. 協同撰寫。[Deploying OpenWhisk on kind](https://github.com/apache/openwhisk-deploy-kube/blob/master/docs/k8s-kind.md)。檢自 openwhisk-deploy-kube｜GitHub (2020-09-30)。
4. 张晋涛。[Kubernetes 从上手到实践](https://juejin.im/book/6844733753063915533/section/6844733753126813709)。檢自 掘金小册 (2020-10-22)。
5. (2020-09-22)。[Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。檢自 Kubernetesb (2020-10-22)。
6. 胡程維｜Cheng-Wei Hu (2019-07-20)。[Kubernetes 基礎教學（三）Helm 介紹與建立 Chart](https://medium.com/@C.W.Hu/kubernetes-helm-chart-tutorial-fbdad62a8b61)。檢自 Medium (2020-10-22)。
7. Mingo (2019-02-27)。[安装 · Helm用户与开发者指南](https://whmzsu.github.io/helm-doc-zh-cn/quickstart/install-zh_cn.html)。檢自 github (2020-10-22)。
8. rodric rabbah (2018-01-23)。[Serverless functions in your favorite language with OpenWhisk | by  | Apache OpenWhisk](https://medium.com/openwhisk/serverless-functions-in-your-favorite-language-with-openwhisk-f7c447558f42)。檢自 Medium (2020-11-16)。
9. style95 (2017-11-06)。[bash kind action · Issue #2927 · apache/openwhisk](https://github.com/apache/openwhisk/issues/2927)。檢自 apache/openwhisk ｜ GitHub (2020-11-16)。
10. 協同撰寫。[openwhisk/actions-docker.md](https://github.com/apache/openwhisk/blob/master/docs/actions-docker.md)。檢自 apache/openwhisk ｜ GitHub (2020-11-16)。
11. pierredavidbelanger (2017-01-08)。[Error loading shared library libstdc++.so.6 · Issue #33](https://github.com/kohlschutter/junixsocket/issues/33)。kohlschutter/junixsocket ｜ GitHub (2020-11-16)。
12. [Documentation](https://openwhisk.apache.org/documentation.html#actions-creating-and-invoking)。Openwhisk (2020-11-16)。
13. 協同撰寫。[openwhisk/actions-python.md](https://github.com/apache/openwhisk/blob/master/docs/actions-python.md#packaging-python-actions-with-a-virtual-environment-in-zip-files)。檢自 apache/openwhisk ｜ GitHub (2020-11-16)。
14. 協同撰寫。[openwhisk-deploy-kube/configurationChoices.md](https://github.com/apache/openwhisk-deploy-kube/blob/master/docs/configurationChoices.md#using-an-external-database)。檢自 apache/openwhisk-deploy-kube ｜ GitHub (2020-11-16)。
15. 協同撰寫。[openwhisk/README.md](https://github.com/apache/openwhisk/blob/master/tools/db/README.md#using-couchdb)。檢自 apache/openwhisk ｜ GitHub (2020-11-16)。
16. Chanwit Kaewkasi [Docker for Serverless Applications: Containerize and orchestrate functions using OpenFaas, OpenWhisk, and Fn](https://books.google.com.tw/books?id=_d5YDwAAQBAJ&pg=PA120&lpg=PA120&dq=openwhisk+apigateway+redis&source=bl&ots=nT2aTGxuu-&sig=ACfU3U0NiWTKbgNqf4hqsvrP4iN3e94Gig&hl=zh-TW&sa=X&ved=2ahUKEwjuxYLdtPboAhWIbN4KHTfHC8YQ6AEwBHoECAsQPw#v=onepage&q=openwhisk%20apigateway%20redis&f=false)。檢自  Google 圖書 (2020-11-16)。


<br><br> 

## 更新紀錄
<details>
  <summary>最後更新日期：2020-09-29</summary>
  <ul class="timestamp">
    　<li>2020-12-31 發布</li>
    　<li>2020-11-17 完稿</li>
    　<li>2020-04-08 起稿</li>
  </ul>
</details>
