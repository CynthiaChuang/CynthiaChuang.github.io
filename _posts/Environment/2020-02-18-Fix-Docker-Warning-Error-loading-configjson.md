---
title: '解决 docker Warning: Error loading config.json'
date: 2020-02-18
is_modified: false
categories:
- Environment
tags:
- Docker
- Linux/Unix
--- 

有夠莫名其妙的，重開個的機。docker 指令莫名其妙就爛掉的 orz ....
   
雖然這條 Warning 不影響使用，但一直出現還是很討厭阿 QAQ

<!--more-->
<br><br> 

## 問題描述與釐清
某天~~睡醒~~忽然出現的的 WARNING，不影響使用但超煩！
```
WARNING: Error loading config file: /home/Cynthia_Chuang/.docker/config.json: stat /home/Cynthia_Chuang/.docker/config.json: permission denied
```

<br><br> 

## 解決辦法
1. 添加使用者  
    ```bash
    $ sudo gpasswd -a ${USER} docker 
    ```
2. 重起 docker-daemon  
    ```bash
    $ sudo systemctl restart docker
    ```
3. 設置權限  
    ```bash
    $ sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
    $ sudo chmod g+rwx "/home/$USER/.docker" -R
    ```
 

<br><br> 

## 參考資料 
1. kan2016 (2019-01-10)。[解决WARNING: Error loading config file: /home/kang/.docker/config.json: stat /home/kang/.docker/conf](https://blog.csdn.net/kan2016/article/details/86242571) 。檢自 运维 kan2016的博客 CSDN博客 (2020-02-18)。