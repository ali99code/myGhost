+++
categories = ["Python"]
tags = ["Python"]
description = ""
slug = "virtualenv"
draft = false
title = "Virtualenv for Python"
date = 2016-11-07T06:17:05Z
author = "Alibaba"

+++

#Virtualenv
```
virtualenv 是Python開發者的利器。解決Python 套件相依衝突,可
使用 virtualenv 隔離出 不同的Python環境。
```
```
以下展示是在 CEntos 7 環境下進行。Python 版本是 2.7.x 
另外,也透過epel repo 安裝 Python3.4
yum --enablerepo=epel install python34 
```
## Virtualenv Instlall
 - pip install virtualenv

## Virtualenv 基本操作

```
在/tmp 下,隔離出 名稱為 env1 env2 兩個 python環境。
envT1 是 python2.7.x
envT2 是 python 3.4.x
之後利用 active,進入 envT1 或 envT2
退出使用 deactive
```
 - cd /tmp
 - virtualenv envT1
 - virtualenv --python=python3  envT2
 - cd /tmp/envT1/; source bin/activate

```
(envT1) [root@centos7 envT1]# python -c "import sys;print sys.version"
2.7.5 (default, Sep 15 2016, 22:37:39)
(envT1) [root@centos7 envT1]# deactivate 
[root@centos7 envT1]#
```
  -  cd /tmp/envT2/;source bin/activate

```
[root@centos7 envT2]#source bin/activate
(envT2)[root@centos7 envT2]#
(envT2)[root@centos7 envT2]# python -c "import sys;print(sys.version)"

3.4.5 (default, Oct 20 2016, 22:54:51) 
(envT2)[root@centos7 envT2]# deactivate
[root@centos7 envT2]#
```

 