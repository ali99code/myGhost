+++
tags = ["Docker"]
description = ""
slug = "docker-0"
draft = false
title = "Docker 基本操作 ..未完成"
date = 2016-11-15T18:01:51Z
author = "Alibaba"
categories = ["Docker"]

+++

#映像檔 images
- - -
1. 查看目前有哪些images
 - docker imagesID
2. 刪除 image
 - dockr rmi imagesID
3. 更新映像檔標籤
```
docker.io/centos         6.6        523995461077        
```
 - docker tag 523995461077  alicode/nginx-sshd:v5
```
alicode/nginx-sshd       v5       523995461077          
```

#容器  Container
***
1. 簡單建立容器
 - docker create  -it  imageID or Name (建立後,會產生一組CONTAINER ID)
 - docker start    CONTAINER ID
2. 另一種簡單建立 container ,執行bash 並使用互動模式(就像是 進入終端機一樣)
 - docker run -t -i  ubuntu  /bin/bash (若使用 exit ,及ctrl +d 跳出後, 這個container 立即停止)
 - docker run -t -i  -d ubuntu  /bin/bash (若使用 exit ,及ctrl +d 跳出後, 這個container 不會停止,因為 -d 表示在背景執行著)
3. 進入執行中的容器 (兩種方法)
 - docker attach CONTAINER ID  (方法一)
 - docker exec -it  CONTAINER ID /usr/bin (方法二)
4. 刪除停止狀態的容器
 - docker rm CONTAINER ID
5. 停止容器
 - docker stop CONTAINER ID
#查看images ID 及 container ID 資訊
***
1. docker inspect    images id 或 container id ; 此指令會 回應 一個 JSON  格式的資訊。
2.  看看以下指令
  - docker start   f44726376
  - docker  inspect  f44726376
  - docker inspect  -f '{{ .NetworkSettings }}' f44726376
  - docker inspect  -f  '{{.NetworkSettings.IPAddress}}' f44726376
