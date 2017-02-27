+++
tags = ["Docker","Alpine"]
description = ""
slug = "alpine-ssh"
draft = false
title = "Alpine Linux Docker container with sshd"
date = 2017-01-15T08:21:31Z
author = "Alibaba"
categories = ["Docker","Alpine"]

+++

## 簡介
Alpine Linux 是很輕量級且安全度很高的發行版本。基本系統（包含可執行核心）在5MB以下,非常適合運作在 嵌入式系統及單板電腦上。幾乎可以與BusyBox Linux 這個發行版本相比拼,但與BusyBox 不同的是它支援需多popular套件可以安裝。因此此篇章試著用Alpine image 運作在 Docker上。

## 建立Dockerfile
  - 先建立 目錄 名為 Alpine, 此目錄下建 Dockerfile

```
mkdir /tmp/Alpine && touch /tmp/Dockerfile
``` 

  - 編寫 Dockerfile

```
FROM alpine:latest
RUN  apk update
RUN  apk add openssh
RUN  /usr/bin/ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN  /usr/bin/ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN  /usr/bin/ssh-keygen -t ecdsa -f  /etc/ssh/ssh_host_ecdsa_key                                                                     
RUN  /usr/bin/ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key
RUN  echo 'root:qwedcxza'|chpasswd
RUN  echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
CMD ["/usr/sbin/sshd", "-D", "-f", "/etc/ssh/sshd_config"]
```  

 - 執行建立docker image

```
cd /tmp/Alpine
docker build  -t ali:Alpine .
``` 

 - 從剛建立的 ali:Alpine 這 image,建立container,並執行它

```
docker run  -d --name=AlpineTest --hostname=Alpine ali:Alpine
```
 - 看 此 Container 目前分配的 IP 是多少

```
docker inspect -f  '{{.NetworkSettings.IPAddress}}' AlpineTest

172.17.0.2
```
 - 測試登入看看

```
ssh 172.17.0.2

Welcome to Alpine!

The Alpine Wiki contains a large amount of how-to guides and general
information about administrating Alpine systems.
See <http://wiki.alpinelinux.org>.

You can setup the system with the command: setup-alpine

You may change this message by editing /etc/motd.

Alpine:~#
```

## 參考資料:
 - [官方 Alpine wiki](https://wiki.alpinelinux.org/wiki/Main_Page"target="_blank)
 - [官方 Alpine apk 套件管理](https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management"target="_blank)
 - [Alpine Linux 挑戰最小 docker image OS](https://blog.wu-boy.com/2015/12/a-super-small-docker-image-based-on-alpine-linux/"target="_blank)
 - [解決啟動SSHD服務時報錯](http://firesk.blog.51cto.com/180373/46625"target="_blank)