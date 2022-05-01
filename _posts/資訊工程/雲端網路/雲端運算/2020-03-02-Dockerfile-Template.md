---
title: Dockerfile 樣板
date: 2020-03-02
is_modified: false
categories:
- "雲端網路 › 雲端運算"
tags:
- Docker
- Dockerfile
--- 

為了提供 inference 給其他人使用，所以包了包 image 放上 server 給其他人呼叫。這裡記錄一下成果，下次可以直接複製貼上就好 XD

<!--more-->
<br><br> 


```dockerfile
FROM tensorflow/tensorflow:2.0.1-gpu-py3

# basic tool
RUN apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
		zip \
		curl \
		apt-utils \
		locales \
		tzdata \
	&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
	&& apt-get autoclean \
	&& apt-get clean

  
# set locale
RUN locale-gen zh_TW.UTF-8
ENV LANG=zh_TW.UTF-8 LANGUAGE=zh_TW:zh LC_ALL=zh_TW.UTF-8


# set timezone
RUN TZ=Asia/Taipei \
	&& ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
	&& echo $TZ > /etc/timezone \
	&& dpkg-reconfigure -f noninteractive tzdata 


# install inference depend on python packages
RUN apt-get update\
	&& apt-get install -y libsm6 libxext6 libxrender-dev \
	&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
	&& apt-get autoclean \
	&& apt-get clean \
	&& pip3 install --upgrade pip \
	&& pip3 install Werkzeug>=0.15.5 \
	   		flask flask-cors requests\
	   		h5py \
	   		numpy  \
	   		opencv-python \
	&& rm -rf ~/.cache/pip

# set working directory && copy data
ARG APP_HOME=/workspace
WORKDIR $APP_HOME
ADD ./workspace $APP_HOME

# port
EXPOSE 9999

# Start the entrypoint command 
CMD python /workspace/inference_server.py
```


<br><br> 

## 參考資料 
1. yeasy 。[使用 Dockerfile 定制镜像](https://yeasy.gitbooks.io/docker_practice/image/build.html) 。檢自 Docker —— 从入门到实践 (2020-03-02)。
2. zxcvbnius (2017-12-22) 。[[Day 3] 打造你的Docker containers](https://ithelp.ithome.com.tw/articles/10192519) 。檢自 iT 邦幫忙 (2020-03-02)。