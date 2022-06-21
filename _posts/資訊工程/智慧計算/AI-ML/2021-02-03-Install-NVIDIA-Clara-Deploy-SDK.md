---
title: 【Survey】 NVIDIA Clara Deploy SDK - 安裝篇
date: 2021-03-07 23:53
is_modified: false
categories:
- "智慧計算 › 人工智慧"
- "資訊科技 › 開發與輔助工具"
tags:
- AI/ML
- Clara
- NVIDIA
- 醫學影像
- 工具安裝與部署
--- 

Deploy SDK Part2！
  
原本想在[上一篇](/NVIDIA-Clara-Deploy-SDK)一起解決的，不過最後範例程式碼架上選染效果後真的超級長的，最後決定來開一篇新的寫安裝步驟好了。
  
但是說這篇真的拖有夠久的，產出速度比不上待寫文章的新增速度，結果草稿越積越多 Orz

<!--more-->
<p class="illustration">
    <img src="https://i.imgur.com/HMTDUR7.png" alt="安裝篇">
    安裝篇 （圖片來源: <a href="https://meet.bnext.com.tw/blog/view/10124">Meet創業小聚</a>）
</p>

按照[文件](https://docs.nvidia.com/clara/deploy/ClaraInstallation.html)看來安裝步驟似乎不難，不過按照以往經驗看來成不成功要看人品與緣份 XDDD



## 系統需求
在文件一開始列了落落長的系統需求，除了一些基本硬體需求外，還有指定了 K8S、 Helm 與 Docker 的版號：
- **Kubernetes 1.15.4**
- **Docker 19.03.1**
- **NVIDIA Docker 2.2.0**
- **Helm 2.15.2**

如果系統中未安裝這些需求，可以不用先行安裝，等等安裝時會幫忙一併安裝；但如果安裝的版號不合，可能需要先行移除，否則它們會跳過安裝。



## 安裝步驟
上張圖說明一下 Deploy SDK 的安裝流程：

<p class="illustration">
    <img src="https://i.imgur.com/9Wb8F1x.png" alt="安裝流程">
    安裝流程 （圖片來源: <a href="https://docs.nvidia.com/clara/deploy/ClaraInstallation.html">SDK 0.7.1 documentation</a>）
</p>

1. **下載並安裝 bootstrap**  
    首先先登入 [NGC](https://ngc.nvidia.com/catalog/collections?orderBy=modifiedDESC&pageNumber=0&query=&quickFilter=collections&filters=)，找到 [Clara Deploy Bootstrap](https://ngc.nvidia.com/catalog/resources/nvidia:clara:clara_bootstrap/performance) 並進行下載與解壓縮：
    ```bash
    $ wget --content-disposition https://api.ngc.nvidia.com/v2/resources/nvidia/clara/clara_bootstrap/versions/0.7.1-2008.1/zip -O bootstrap.zip
    $ unzip bootstrap.zip -d bootstrap
    ```

    完成下載後，進入資料夾執行腳本。這份腳本它將會安裝 Docker、 K8S ...等所需求的軟體：
    ```bash
    $ cd bootstrap
    $ sudo ./bootstrap.sh
    ```
       
    是說，如果不想登入 NGC 也行，登入與否其實不影響下載。不過還是建議登入下，否則很有機會在接下來的步驟中被它打斷，它實在很吵...
 
    <br>   

2.  **下載並安裝 CLI**  
     接下來再去 [NGC](https://ngc.nvidia.com/catalog/collections?orderBy=modifiedDESC&pageNumber=0&query=&quickFilter=collections&filters=)，找 [Clara CLI](https://ngc.nvidia.com/catalog/resources/nvidia:clara:clara_cli) 來下載與解壓縮：
    ```bash
    $ wget --content-disposition https://api.ngc.nvidia.com/v2/resources/nvidia/clara/clara_cli/versions/0.7.1-2008.1/zip -O clara_cli.zip
    
    $ sudo unzip clara_cli.zip -d /usr/bin/ && sudo chmod 755 /usr/bin/clara*
    Archive:  cli.zip
    inflating: /usr/bin/clara
    inflating: /usr/bin/clara-dicom
    inflating: /usr/bin/clara-monitor
    inflating: /usr/bin/clara-pull
    inflating: /usr/bin/clara-render
    ```
        
    將檔案放到 `/usr/bin/` 下後，可以試著呼叫 clara 指令，驗證是否安裝成功：
    ```bash
    $ clara version
    Clara CLI version: 0.7.1-12788.ae65aea0
    ```

    <br>
    
3. **配置 NGC 憑證**  
    安裝 Clara CLI 須配置 NGC 憑證，稍等 Clara CLI 才能從 NGC Pull 相關 Helm Chart 以進行部屬。
    
    這邊你須要拿到一把 `NGC_API_KEY`。這次就必須一定要登入 NGC 了，登入後點選右上角頭像選單中的 `Setup`，並選擇 `Generate API Key` 
    
    <p class="illustration">
        <img src="https://i.imgur.com/AqM4tPS.png" alt="Generate API Key">
    </p>

    
    進入頁面後，會右上方有個 Generate API Key 的按鈕，點擊後就會產生 `NGC_API_KEY` 了。
    
    <p class="illustration">
        <img src="https://i.imgur.com/PFMeczB.png" alt="Generate API Key-2">
    </p>


    完成後回到終端輸入下列指令，可以考慮 `orgteam` 使用預設值就好，：
    ```bash
    $ clara config --key NGC_API_KEY [--orgteam nvidia/clara] -y    
    ✔ Yes
    Configuration "ngc-clara"successfully created    
    ``` 
    <br>
    
    是說 **successfully** 的意思，是指你成功配置了憑證，但憑證是否能使用必須使用後才知道，可以試著使用 `pull` 指令來試試：
    ```bash
    $ clara pull platform     
    ✔ Yes
    Clara Platform 0.7.1-2008.1
    Chart saved at: /home/.clara/charts/clara
    Hint: use "clara platform start" or "clara platform restart" to deploy pulled Clara Platform.
    ```
    
    如果失敗可能會看到下列這樣的訊息：
    ```bash
    $ clara pull platform
    ✔ Yes
    Error: unable to fetch latest version information
    401 Unauthorized
    ```
    
    或是 
    
    ```bash
    $ clara pull platform
    ✔ Yes
    Error: unable to fetch latest version information
    403 Forbidden
    ```
    
    <br>

4. **啟動 Helm Chart**   
   在上一篇提到 [Helm Charts](/NVIDIA-Clara-Deploy-SDK#Helm-Charts) 時有提過，除了 Triton Inference Server 之外的 charts，都可以藉由這步驟啟動。 
    
    <p class="illustration">
    <img src="https://i.imgur.com/LgA32Ff.png" alt="NVIDIA Clara Deploy Architecture">
    DNVIDIA Clara Deploy Architecture（圖片來源: <a href="https://docs.nvidia.com/clara/deploy/index.html">SDK 0.7.1 documentation</a>）
    </p>

    因為 platform 的下載在剛剛測試 clara 的指令時已經順道完成了，所以這邊就直接啟動。
    ```bash
    $clara platform start
    Starting clara...
    NAME:   clara
    Note: If there is a running instance of Clara Console, Clara Dicom Adapter or Clara Renderer, they should be restarted.
    ```

    <br>
    
    接下來下載 Clara Deploy Services 的 Helm Charts：
    ```bash
    $ clara pull dicom
    ✔ Yes
    Clara Dicom Adapter 0.7.1-2008.1
    Chart saved at: /home/.clara/charts/dicom-adapter
    Hint: use "clara dicom start" or "clara dicom restart" to deploy pulled Clara Dicom Adapter.

    $ clara pull render
    ✔ Yes
    Clara Renderer 0.7.1-2008.1
    Chart saved at: /home/.clara/charts/clara-renderer
    Hint: use "clara render start" or "clara render restart" to deploy pulled Clara Renderer.

    $ clara pull monitor
    ✔ Yes
    Clara Monitor Server 0.7.1-2008.1
    Chart saved at: /home/.clara/charts/clara-monitor-server
    Hint: use "clara monitor start" or "clara monitor restart" to deploy pulled Clara Monitor Server.


    $ clara pull console
    ✔ Yes
    Clara Management Console 0.7.1-2008.1
    Chart saved at: /home/.clara/charts/clara-console
    Hint: use "clara console start" or "clara console restart" to deploy pulled Clara Management Console.   
    ```
    
    之後就可以試著啟動了：     
    ```bash
    $ clara dicom start
    Starting DICOM Adapter...
    NAME: clara-dicom-adapter

    $ clara render start
    NAME: clara-render-server


    $ clara monitor start
    NAME: clara-monitor-server

    $ clara console start
    NAME: clara-console
    ```


    
## 驗證安裝
如果一切順利的話，跑完上面算是安裝完成了，你可以試著下 `hlem ls` 指令來觀察目前所啟動的 charts：

```bash
$ helm ls
NAME                    CHART 
clara                     clara-0.7.1-2008.1   
clara-console             clara-console-0.7.1-2008.1  
clara-dicom-adapter        dicom-adapter-0.7.1-2008.1 
clara-monitor-server        clara-monitor-server-0.7.1-2008.1   
clara-render-server        clara-renderer-0.7.1-2008.1
```

<br>

或是下 `kubectl get pods` 應該會看到下面這些 Pods：
* clara-clara-platformapiserver-
* clara-dicom-adapter-
* clara-monitor-server-fluentd-elasticsearch-
* clara-monitor-server-grafana-
* clara-monitor-server-monitor-server-
* clara-render-server-clara-renderer-
* clara-resultsservice-
* clara-ui-
* clara-console-
* clara-console-mongodb-
* clara-workflow-controller-
* elasticsearch-master-0
* elasticsearch-master-1



## 觀察 Pod 的變化
提到啟動的 Pod，有點好奇在每個 Chart 啟動時，會啟動的 Pod 有哪些。所以把整個 Clara 卸掉，重新安裝一次並觀察 Pod 的變化。
 

1. **安裝前**        
    ```bash
    $ kubectl get all
    NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
    service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   4d23h

    $ kubectl get pods
    No resources found.
    ```
    <br>
    
2. **clara platform start**  
    ```bash
    $ kubectl get all    
    NAME                                                 READY   STATUS    RESTARTS   AGE
    pod/clara-clara-platformapiserver-54c5c44bbd-9b97b   1/1     Running   0          95s
    pod/clara-resultsservice-664477898f-zl8cr            1/1     Running   0          95s
    pod/clara-ui-6f89b97df8-fn2zm                        1/1     Running   0          95s
    pod/clara-workflow-controller-69cbb55fc8-t67ns       1/1     Running   0          95s
    pod/fluentd-7n2b8                                    1/1     Running   0          95s
    pod/fluentd-ccnzw                                    1/1     Running   0          95s


    NAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)           AGE
    service/clara                  NodePort    10.103.37.52    <none>        50051:31536/TCP   95s
    service/clara-resultsservice   ClusterIP   10.108.91.220   <none>        8088/TCP          95s
    service/clara-ui               ClusterIP   10.97.148.11    <none>        80/TCP            95s
    service/kubernetes             ClusterIP   10.96.0.1       <none>        443/TCP           9m52s

    NAME                     DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
    daemonset.apps/fluentd   2         2         2       2            2           <none>          95s

    NAME                                            READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/clara-clara-platformapiserver   1/1     1            1           95s
    deployment.apps/clara-resultsservice            1/1     1            1           95s
    deployment.apps/clara-ui                        1/1     1            1           95s
    deployment.apps/clara-workflow-controller       1/1     1            1           95s

    NAME                                                       DESIRED   CURRENT   READY   AGE
    replicaset.apps/clara-clara-platformapiserver-54c5c44bbd   1         1         1       95s
    replicaset.apps/clara-resultsservice-664477898f            1         1         1       95s
    replicaset.apps/clara-ui-6f89b97df8                        1         1         1       95s
    replicaset.apps/clara-workflow-controller-69cbb55fc8       1         1         1       95s
    ```
    <br>
    
    
3. **clara dicom start**  
    ```bash
    $ kubectl get all       
    NAME                                                 READY   STATUS    RESTARTS   AGE
    pod/clara-clara-platformapiserver-54c5c44bbd-9b97b   1/1     Running   0          2m44s
    pod/clara-dicom-adapter-7948fcd445-rtbqr             1/1     Running   0          33s
    pod/clara-resultsservice-664477898f-zl8cr            1/1     Running   0          2m44s
    pod/clara-ui-6f89b97df8-fn2zm                        1/1     Running   0          2m44s
    pod/clara-workflow-controller-69cbb55fc8-t67ns       1/1     Running   0          2m44s
    pod/fluentd-7n2b8                                    1/1     Running   0          2m44s
    pod/fluentd-ccnzw                                    1/1     Running   0          2m44s


    NAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                        $
    GE
    service/clara                  NodePort    10.103.37.52    <none>        50051:31536/TCP                $
    m44s
    service/clara-dicom-adapter    NodePort    10.105.101.54   <none>        104:31289/TCP,5000:31880/TCP   $
    3s
    service/clara-resultsservice   ClusterIP   10.108.91.220   <none>        8088/TCP                       $
    m44s
    service/clara-ui               ClusterIP   10.97.148.11    <none>        80/TCP                         $
    m44s
    service/kubernetes             ClusterIP   10.96.0.1       <none>        443/TCP                        $
    1m

    NAME                     DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
    daemonset.apps/fluentd   2         2         2       2            2           <none>          2m44s

    NAME                                            READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/clara-clara-platformapiserver   1/1     1            1           2m44s
    deployment.apps/clara-dicom-adapter             1/1     1            1           33s
    deployment.apps/clara-resultsservice            1/1     1            1           2m44s
    deployment.apps/clara-ui                        1/1     1            1           2m44s
    deployment.apps/clara-workflow-controller       1/1     1            1           2m44s

    NAME                                                       DESIRED   CURRENT   READY   AGE
    replicaset.apps/clara-clara-platformapiserver-54c5c44bbd   1         1         1       2m44s
    replicaset.apps/clara-dicom-adapter-7948fcd445             1         1         1       33s
    replicaset.apps/clara-resultsservice-664477898f            1         1         1       2m44s
    replicaset.apps/clara-ui-6f89b97df8                        1         1         1       2m44s
    replicaset.apps/clara-workflow-controller-69cbb55fc8       1         1         1       2m44s
    ```
    <br>
    

4. **clara render start**    
    ```bash
    $ kubectl get all    
     kubectl get all    
    NAME                                                     READY   STATUS             RESTARTS   AGE
    pod/clara-clara-platformapiserver-54c5c44bbd-gfwng       1/1     Running            0          24m
    pod/clara-dicom-adapter-7948fcd445-mv248                 1/1     Running            0          20m
    pod/clara-render-server-clara-renderer-d79dd4779-f5hgd   2/3     CrashLoopBackOff   7          11m
    pod/clara-resultsservice-664477898f-2vsw9                1/1     Running            0          24m
    pod/clara-ui-6f89b97df8-c5p2f                            1/1     Running            0          24m
    pod/clara-workflow-controller-69cbb55fc8-mc682           1/1     Running            0          24m
    pod/fluentd-ntl6q                                        1/1     Running            0          24m
    pod/fluentd-tvnrl                                        1/1     Running            0          24m


    NAME                                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          
                   AGE
    service/clara                                NodePort    10.101.135.71    <none>        50051:32455/TCP  
                   24m
    service/clara-dicom-adapter                  NodePort    10.100.25.126    <none>        104:31985/TCP,500
    0:30647/TCP    20m
    service/clara-renderer-clara-render-server   NodePort    10.108.60.232    <none>        8070:30105/TCP,80
    60:32006/TCP   11m
    service/clara-resultsservice                 ClusterIP   10.109.206.204   <none>        8088/TCP         
                   24m
    service/clara-ui                             ClusterIP   10.101.195.91    <none>        80/TCP           
                   24m
    service/kubernetes                           ClusterIP   10.96.0.1        <none>        443/TCP          
                   27m

    NAME                     DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
    daemonset.apps/fluentd   2         2         2       2            2           <none>          24m

    NAME                                                 READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/clara-clara-platformapiserver        1/1     1            1           24m
    deployment.apps/clara-dicom-adapter                  1/1     1            1           20m
    deployment.apps/clara-render-server-clara-renderer   0/1     1            0           11m
    deployment.apps/clara-resultsservice                 1/1     1            1           24m
    deployment.apps/clara-ui                             1/1     1            1           24m
    deployment.apps/clara-workflow-controller            1/1     1            1           24m

    NAME                                                           DESIRED   CURRENT   READY   AGE
    replicaset.apps/clara-clara-platformapiserver-54c5c44bbd       1         1         1       24m
    replicaset.apps/clara-dicom-adapter-7948fcd445                 1         1         1       20m
    replicaset.apps/clara-render-server-clara-renderer-d79dd4779   1         1         0       11m
    replicaset.apps/clara-resultsservice-664477898f                1         1         1       24m
    replicaset.apps/clara-ui-6f89b97df8                            1         1         1       24m
    replicaset.apps/clara-workflow-controller-69cbb55fc8           1         1         1       24m
    ```
    <br>

5. **clara monitor start**  
    ```bash
    $ kubectl get all          
    NAME                                                       READY   STATUS             RESTARTS   AGE
    pod/clara-clara-platformapiserver-54c5c44bbd-gfwng         1/1     Running            0          40m
    pod/clara-dicom-adapter-7948fcd445-mv248                   1/1     Running            0          36m
    pod/clara-monitor-server-fluentd-elasticsearch-dl7bj       1/1     Running            0          14m
    pod/clara-monitor-server-fluentd-elasticsearch-jxdk6       1/1     Running            0          14m
    pod/clara-monitor-server-grafana-5f874b974d-qvxgn          1/1     Running            0          14m
    pod/clara-monitor-server-monitor-server-59c8bf68f7-5rcg7   0/1     CrashLoopBackOff   7          14m
    pod/clara-render-server-clara-renderer-d79dd4779-f5hgd     2/3     CrashLoopBackOff   10         27m
    pod/clara-resultsservice-664477898f-2vsw9                  1/1     Running            0          40m
    pod/clara-ui-6f89b97df8-c5p2f                              1/1     Running            0          40m
    pod/clara-workflow-controller-69cbb55fc8-mc682             1/1     Running            0          40m
    pod/elasticsearch-master-0                                 1/1     Running            0          14m
    pod/elasticsearch-master-1                                 1/1     Running            0          14m
    pod/fluentd-ntl6q                                          1/1     Running            0          40m
    pod/fluentd-tvnrl                                          1/1     Running            0          40m


    NAME                                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          
                   AGE
    service/clara                                NodePort    10.101.135.71    <none>        50051:32455/TCP  
                   40m
    service/clara-dicom-adapter                  NodePort    10.100.25.126    <none>        104:31985/TCP,500
    0:30647/TCP    36m
    service/clara-monitor-server                 ClusterIP   10.111.167.160   <none>        50051/TCP        
                   14m
    service/clara-monitor-server-grafana         NodePort    10.100.148.116   <none>        80:32000[16/1632]
                   14m
    service/clara-renderer-clara-render-server   NodePort    10.108.60.232    <none>        8070:30105/TCP,80
    60:32006/TCP   27m
    service/clara-resultsservice                 ClusterIP   10.109.206.204   <none>        8088/TCP         
                   40m
    service/clara-ui                             ClusterIP   10.101.195.91    <none>        80/TCP           
                   40m
    service/elasticsearch-master                 ClusterIP   10.108.240.18    <none>        9200/TCP,9300/TCP
                   14m
    service/elasticsearch-master-headless        ClusterIP   None             <none>        9200/TCP,9300/TCP
                   14m
    service/kubernetes                           ClusterIP   10.96.0.1        <none>        443/TCP          
                   43m

    NAME                                                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAI
    LABLE   NODE SELECTOR   AGE
    daemonset.apps/clara-monitor-server-fluentd-elasticsearch   2         2         2       2            2   
            <none>          14m
    daemonset.apps/fluentd                                      2         2         2       2            2   
            <none>          40m

    NAME                                                  READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/clara-clara-platformapiserver         1/1     1            1           40m
    deployment.apps/clara-dicom-adapter                   1/1     1            1           36m
    deployment.apps/clara-monitor-server-grafana          1/1     1            1           14m
    deployment.apps/clara-monitor-server-monitor-server   0/1     1            0           14m
    deployment.apps/clara-render-server-clara-renderer    0/1     1            0           27m
    deployment.apps/clara-resultsservice                  1/1     1            1           40m
    deployment.apps/clara-ui                              1/1     1            1           40m
    deployment.apps/clara-workflow-controller             1/1     1            1           40m

    NAME                                                             DESIRED   CURRENT   READY   AGE
    replicaset.apps/clara-clara-platformapiserver-54c5c44bbd         1         1         1       40m
    replicaset.apps/clara-dicom-adapter-7948fcd445                   1         1         1       36m
    replicaset.apps/clara-monitor-server-grafana-5f874b974d          1         1         1       14m
    replicaset.apps/clara-monitor-server-monitor-server-59c8bf68f7   1         1         0       14m
    replicaset.apps/clara-render-server-clara-renderer-d79dd4779     1         1         0       27m
    replicaset.apps/clara-resultsservice-664477898f                  1         1         1       40m
    replicaset.apps/clara-ui-6f89b97df8                              1         1         1       40m
    replicaset.apps/clara-workflow-controller-69cbb55fc8             1         1         1       40m

    NAME                                    READY   AGE
    statefulset.apps/elasticsearch-master   2/2     14m
    ```
    <br>

6. **clara console start**  
    ```bash
    $ kubectl get all                                                             

    NAME                                                       READY   STATUS             RESTARTS   AGE
    pod/clara-clara-platformapiserver-54c5c44bbd-gfwng         1/1     Running            0          61m
    pod/clara-console-8565b4d565-77jhc                         2/2     Running            0          19m
    pod/clara-console-mongodb-85f8bd5f95-8nwqx                 1/1     Running            0          19m
    pod/clara-dicom-adapter-7948fcd445-mv248                   1/1     Running            0          58m
    pod/clara-monitor-server-fluentd-elasticsearch-dl7bj       1/1     Running            0          36m
    pod/clara-monitor-server-fluentd-elasticsearch-jxdk6       1/1     Running            0          36m
    pod/clara-monitor-server-grafana-5f874b974d-qvxgn          1/1     Running            0          36m
    pod/clara-monitor-server-monitor-server-59c8bf68f7-5rcg7   0/1     CrashLoopBackOff   11         36m
    pod/clara-render-server-clara-renderer-d79dd4779-f5hgd     2/3     CrashLoopBackOff   14         48m
    pod/clara-resultsservice-664477898f-2vsw9                  1/1     Running            0          61m
    pod/clara-ui-6f89b97df8-c5p2f                              1/1     Running            0          61m
    pod/clara-workflow-controller-69cbb55fc8-mc682             1/1     Running            0          61m
    pod/elasticsearch-master-0                                 1/1     Running            0          36m
    pod/elasticsearch-master-1                                 1/1     Running            0          36m
    pod/fluentd-ntl6q                                          1/1     Running            0          61m
    pod/fluentd-tvnrl                                          1/1     Running            0          61m

    NAME                                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          
                   AGE
    service/clara                                NodePort    10.101.135.71    <none>        50051:32455/TCP  
                   61m
    service/clara-console                        NodePort    10.99.119.217    <none>        8080:32002/TCP,50
    00:32003/TCP   19m
    service/clara-console-mongodb                ClusterIP   10.102.177.195   <none>        27017/TCP        
                   19m
    service/clara-dicom-adapter                  NodePort    10.100.25.126    <none>        104:31985/TCP,500
    0:30647/TCP    58m
    service/clara-monitor-server                 ClusterIP   10.111.167.160   <none>        50051/TCP        
                   36m
    service/clara-monitor-server-grafana         NodePort    10.100.148.116   <none>        80:32000/TCP     
                   36m
    service/clara-renderer-clara-render-server   NodePort    10.108.60.232    <none>        8070:30105/TCP,80
    60:32006/TCP   48m
    service/clara-resultsservice                 ClusterIP   10.109.206.204   <none>        8088/TCP         
                   61m
    service/clara-ui                             ClusterIP   10.101.195.91    <none>        80/TCP           
                   61m
    service/elasticsearch-master                 ClusterIP   10.108.240.18    <none>        9200/TCP,9300/TCP
                   36m
    service/elasticsearch-master-headless        ClusterIP   None             <none>        9200/TCP,9300/TCP
                   36m
    service/kubernetes                           ClusterIP   10.96.0.1        <none>        443/TCP          
                   65m

    NAME                                                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAI
    LABLE   NODE SELECTOR   AGE
    daemonset.apps/clara-monitor-server-fluentd-elasticsearch   2         2         2       2            2   
            <none>          36m
    daemonset.apps/fluentd                                      2         2         2       2            2   
            <none>          61m

    NAME                                                  READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/clara-clara-platformapiserver         1/1     1            1           61m
    deployment.apps/clara-console                         1/1     1            1           19m
    deployment.apps/clara-console-mongodb                 1/1     1            1           19m
    deployment.apps/clara-dicom-adapter                   1/1     1            1           58m
    deployment.apps/clara-monitor-server-grafana          1/1     1            1           36m
    deployment.apps/clara-monitor-server-monitor-server   0/1     1            0           36m
    deployment.apps/clara-render-server-clara-renderer    0/1     1            0           48m
    deployment.apps/clara-resultsservice                  1/1     1            1           61m
    deployment.apps/clara-ui                              1/1     1            1           61m
    deployment.apps/clara-workflow-controller             1/1     1            1           61m

    NAME                                                             DESIRED   CURRENT   READY   AGE
    replicaset.apps/clara-clara-platformapiserver-54c5c44bbd         1         1         1       61m
    replicaset.apps/clara-console-8565b4d565                         1         1         1       19m
    replicaset.apps/clara-console-mongodb-85f8bd5f95                 1         1         1       19m
    replicaset.apps/clara-dicom-adapter-7948fcd445                   1         1         1       58m
    replicaset.apps/clara-monitor-server-grafana-5f874b974d          1         1         1       36m
    replicaset.apps/clara-monitor-server-monitor-server-59c8bf68f7   1         1         0       36m
    replicaset.apps/clara-render-server-clara-renderer-d79dd4779     1         1         0       48m
    replicaset.apps/clara-resultsservice-664477898f                  1         1         1       61m
    replicaset.apps/clara-ui-6f89b97df8                              1         1         1       61m
    replicaset.apps/clara-workflow-controller-69cbb55fc8             1         1         1       61m

    NAME                                    READY   AGE
    statefulset.apps/elasticsearch-master   2/2     36m

    ```

<br>

當然，如果像我這個白目的安裝就沒有那順利了...
    
## 錯誤嘗試：部屬 Clara Platform 與 啟動 Helm Chart
在環境要求的部分，對我來說比較麻煩的是 Kubernetes 與 Helm 的版號，因為我的伺服器環境是與組員共用，所以一開始我決定保留同事需要的環境來硬幹，試試能不能安裝成功，如果真的不行再來嘗試退版安裝。

所以這段如果只是要完成 Deploy SDK 安裝的可以跳過，這邊只是因為我的一時興起所產生的錯誤紀錄而已，當然如果想看我怎麼焦頭爛額的可以繼續往下拉。

恩...我先跟大家說最後的嘗試結果好了，我最後還是退版了。不過我有將過程的一些錯誤紀錄保留下來，看看之後還有沒機會回來再看，絕對不是因為單純湊數字 XDDD
 
<br>
 
  
這邊接續安裝步驟-**配置 NGC 憑證**，在完成 platform chart 的下載後，試著啟動 platform，得到了第一條錯誤訊息：

```shell
$ clara platform start                  
Error: could not find tiller
Usage:
  platform start [flags]

Flags:
  -h, --help   help for start

Global Flags:
      --config string   config file (default is $HOME/.clara/config.yaml)
      --verbose         verbose output

Error: could not find tiller
```

<br>

在 [Stack Overflow](https://stackoverflow.com/questions/51646957/helm-could-not-find-tiller) 上看到了一條類似錯誤訊息的提問，似乎重新初始化 Helm 即可：

```shell
$ helm init
Error: unknown command "init" for "helm"

Did you mean this?
        lint

Run 'helm --help' for usage.
```

<br> 

結果沒有 `helm init`！

查詢了一下 Helm 所找不到的 Tiller 到底是啥，根據 [smalltown](https://medium.com/starbugs/helm-3-%E8%B8%B9%E8%B8%B9%E7%9C%8B-9e7c443fbd7a) 所說，在 Helm2 中，Tiller 是用來安裝與管理其他應用服務的 K8S 元件，簡單來說 Tiller 是一個用來與 K8S API Server 溝通的 **Service**，不過由於權限設置與管理的問題，在 Helm3 的推出後就走向歷史了。
 
很不幸的，我的 Helm 是 v3 的版本：
```shell
$ helm version
version.BuildInfo{Version:"v3.1.2", GitCommit:"d878d4d45863e42fd5cff6743294a11d28a9abce", GitTreeState:"$
lean", GoVersion:"go1.13.8"}
```

<br>

Helm2 與 Helm3 的變動已經屬於系統架構的變動，這個實在不好改。經過調查與[論壇](https://forums.developer.nvidia.com/t/error-could-not-find-tiller/157960)上發問，最後只好將 Helm 降版。我是透過 [ Binary Releases 安裝](https://helm.sh/docs/intro/install/#helm)的方式，將版本降回到 [v2.15.2](https://github.com/helm/helm/releases/tag/v2.15.2)。

降版後再次檢查 Helm 的版號，版號是正確了，但錯誤訊息依舊沒有消失：

```shell
$ helm version
Client: &version.Version{SemVer:"v2.15.2", GitCommit:"8dce272473e5f2a7bf58ce79bb5c3691db54c96b", GitTreeS
tate:"clean"}
Error: could not find tiller
```

<br> 

不過版本都降了，`helm init` 指令應該可以使用了：
```shell
$ helm init
$HELM_HOME has been configured at /home/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-h$
lm-installation
```

<br> 

再次檢查 Helm 的版號，可以發現多出了 Server，用 kubectl 查看正在運行的 Pod，可以看到 Tiller 正在努力工作：  
```shell
$ helm version
Client: &version.Version{SemVer:"v2.15.2", GitCommit:"8dce272473e5f2a7bf58ce79bb5c3691db54c96b", GitTree$
tate:"clean"}
Server: &version.Version{SemVer:"v2.15.2", GitCommit:"8dce272473e5f2a7bf58ce79bb5c3691db54c96b", GitTree$
tate:"clean"}

$ kubectl get pods --namespace kube-system
NAME                                               READY   STATUS    RESTARTS   AGE
coredns-6955765f44-bl6jd                           1/1     Running   0          18h
coredns-6955765f44-mtlxv                           1/1     Running   0          18h
etcd-esccluster-control-plane                      1/1     Running   0          18h
kindnet-dxlgj                                      1/1     Running   0          18h
kindnet-hpvkw                                      1/1     Running   0          18h
kindnet-qb5lm                                      1/1     Running   0          18h
kube-apiserver-esccluster-control-plane            1/1     Running   0          18h
kube-controller-manager-esccluster-control-plane   1/1     Running   0          18h
kube-proxy-cvpmt                                   1/1     Running   0          18h
kube-proxy-nspv9                                   1/1     Running   0          18h
kube-proxy-trkh5                                   1/1     Running   0          18h
kube-scheduler-esccluster-control-plane            1/1     Running   0          18h
tiller-deploy-58f57c5787-bsfkh                     1/1     Running   0          14m
```

<br> 

好了，排除 Tiller 的錯誤訊息後，重新 platform 的 Chart 後再重新 start 一次，看看會不會成功。


```shell
$ clara pull platform
✔ Yes
Clara Platform 0.7.1-2008.1
Chart saved at: /home/.clara/charts/clara
Hint: use "clara platform start" or "clara platform restart" to deploy pulled Clara Platform.


$ clara platform start
Starting clara...
RPC error: code = Unknown desc = namespaces "default" is forbidden: User "system:serviceaccount:kube-system:default" cannot get resource "namespaces" in API group "" in the namespace "default"
```

<br> 

呃，又出現 rpc 的 error。看到錯誤訊息，幾個可能的猜測：
1. **Server 安裝配置的問題**：會考慮這個是因為我的 Tiller 後來降版後我自己重起的。
2. **權限問題**：這個的機會比較大，在查資料的時候，幾乎碰到的是這個狀況。


<br>

算了先試試看 [Helm2 文件](https://v2.helm.sh/docs/install/#running-tiller-locally)中說的本地運行分 Tiller 試試看：
 
```shell
$ ~/bin/tiller 
[main] 2020/10/27 10:53:03 Starting Tiller v2.15.2 (tls=false)
[main] 2020/10/27 10:53:03 GRPC listening on :44134
[main] 2020/10/27 10:53:03 Probes listening on :44135
[main] 2020/10/27 10:53:03 Storage driver is ConfigMap
[main] 2020/10/27 10:53:03 Max history per release is 0
```

但文件中第二步驟連接到新的本地 Tiller 主機，看起來怪怪的，所還是沒做了，
 
<br>



直接放棄第一條路，先是試著處理權限問題好了，根據 Helm2 的 [Role-based Access Control](https://v2.helm.sh/docs/install/#running-tiller-locally) 說明與 [GitHub](https://github.com/fnproject/fn-helm/issues/21) 上的大神討論重新設定了連接，並 start platform：
```bash
$clara platform start
Starting clara...
NAME:   clara
Note: If there is a running instance of Clara Console, Clara Dicom Adapter or Clara Renderer, they should be restarted.
```

 
<br><br>

喔耶！ platform 起動後，就跟前面一樣來下載 Clara Deploy Services 的 Helm Charts：

```bash
$ clara pull dicom
✔ Yes
Clara Dicom Adapter 0.7.1-2008.1
Chart saved at: /home/.clara/charts/dicom-adapter
Hint: use "clara dicom start" or "clara dicom restart" to deploy pulled Clara Dicom Adapter.

$ clara pull render
✔ Yes
Clara Renderer 0.7.1-2008.1
Chart saved at: /home/.clara/charts/clara-renderer
Hint: use "clara render start" or "clara render restart" to deploy pulled Clara Renderer.

$ clara pull monitor
✔ Yes
Clara Monitor Server 0.7.1-2008.1
Chart saved at: /home/.clara/charts/clara-monitor-server
Hint: use "clara monitor start" or "clara monitor restart" to deploy pulled Clara Monitor Server.


$ clara pull console
✔ Yes
Clara Management Console 0.7.1-2008.1
Chart saved at: /home/.clara/charts/clara-console
Hint: use "clara console start" or "clara console restart" to deploy pulled Clara Management Console.
```


最後是心驚膽戰的最後一步：
```bash
$ clara dicom start
Starting DICOM Adapter...
NAME: clara-dicom-adapter

$ clara render start
NAME: clara-render-server

$ clara monitor start
Error: rpc error: code = Unknown desc = validation failed: [unable to recognize "": no matches for kind "PodSecurityPolicy" in version "extensions/v1beta1", unable to recognize "": no matches for kind "Deployment" in version "apps/v1beta2", unable to recognize "": no matches for kind "StatefulSet" in version "apps/v1beta1"]

$ clara console start
NAME: clara-console
```

<br>

就知道沒這麼好過年的，是有看到有個 [Issue](https://github.com/helm/helm/issues/6374) 在討論這問題的，不過必須承認的是，這個討論超過我這個初學者對於 K8S 的掌握了，我才剛入門沒幾天阿（崩潰 
 
問了論壇的人得到的[回覆](https://forums.developer.nvidia.com/t/not-responding-when-running-clara-render-start/158049)還是要我降版 K8S，所以最終只能鼻子摸摸開始降版了：

```bash
$ sudo apt remove kubectl kubeadm kubelet kubernetes-cni
$ rm -rf $HOME/.kube/config
$ sudo apt-get install -y kubelet=1.15.4-00 kubectl=1.15.4-00 kubeadm=1.15.4-00
$ kubectl version --short
Client Version: v1.15.4
Server Version: v1.15.6
```

重新啟動剛剛失敗 monitor Chart：
```bash
$ clara monitor start
NAME: clara-monitor-server
```



## 參考資料 
1. [Clara Deploy Platform](https://ngc.nvidia.com/catalog/collections/nvidia:claradeployplatform)。檢自 NVIDIA NGC (2021-02-02)。
2. NVIDIA Taiwan (2020-11-02)。[NVIDIA Clara Deploy](https://www.youtube.com/watch?v=vYuOvyJOHXk)。檢自 Youtube (2021-02-02)。
3. [2. Installation](https://docs.nvidia.com/clara/deploy/ClaraInstallation.html)。檢自 Clara Deploy SDK https://hackmd.io/0.7.3 documentation (2021-02-02)。
4. Community (2019-05-18)。[openshift - Helm: could not find tiller](https://stackoverflow.com/questions/51646957/helm-could-not-find-tiller)。檢自 Stack Overflow (2021-02-02)。
6. smalltown (2020-05-17)。[Helm 3 踹踹看](https://medium.com/starbugs/helm-3-%E8%B8%B9%E8%B8%B9%E7%9C%8B-9e7c443fbd7a)。檢自 Starbugs Weekly 星巴哥技術專欄｜Medium (2021-02-02)。
7. godleon (2021-01-24)。[[Kubernetes] Package Manager - Helm 簡介](https://godleon.github.io/blog/Kubernetes/k8s-Helm-Introduction/)。檢自 小信豬的原始部落 (2021-02-02)。
8. postak (2018-04-10)。[forbidden: User "system:serviceaccount:kube-system:default" cannot get namespaces in the namespace "default](https://github.com/fnproject/fn-helm/issues/21)。檢自 fnproject/fn-helm｜GitHub (2021-02-02)。 
9. noprom (2017-11-12)。[User "system:serviceaccount:kube-system:default" cannot get namespaces in the namespace "default"](https://github.com/helm/helm/issues/3130)。檢自 helm/helm｜GitHub (2021-02-02)。 
10. [Helm v2](https://v2.helm.sh/docs/install/#running-tiller-locally)。檢自 Helm 官網 (2021-02-02)。 
11. Nick (2019-10-12)。[[Day27] k8s應用篇（一）：Helm部署apps、HPA和CA](https://ithelp.ithome.com.tw/articles/10227329)。檢自 iT 邦幫忙 (2021-02-02)。
12. Terrones-Oscar (2020-08-13)。[helm fails Error: validation failed: [unable to recognize "": no matches for kind "PodSecurityPolicy"]](https://github.com/helm/charts/issues/23521)。檢自 helm/charts｜GitHub (2021-02-02)。
13. jckasper (2019-09-06)。[Helm init fails on Kubernetes 1.16.0](https://github.com/helm/helm/issues/6374)。檢自 helm/helm｜GitHub (2021-02-02)。
14. Zz Chen (2018-07-03)。[Helm 部署在 GKE 上的權限問題](https://medium.com/smalltowntechblog/helm-tiller-%E9%83%A8%E7%BD%B2%E5%9C%A8-gke-%E4%B8%8A%E7%9A%84%E6%AC%8A%E9%99%90%E5%95%8F%E9%A1%8C-a016f703372e)。檢自 smalltowntechblog｜Medium (2021-02-02)。
15. MengYun (2019-10-27)。[Not responding when running “clara render start”](https://forums.developer.nvidia.com/t/not-responding-when-running-clara-render-start/158049)。檢自 NVIDIA Developer Forums (2021-02-02)。



## 更新紀錄
<details class="update_stamp">
  <summary>最後更新日期：2021-03-07</summary>
  <ul>
    <li>2021-03-07 發布</li>
    <li>2021-02-03 完稿</li>
    <li>2020-11-09 起稿</li>
  </ul>
</details>

 