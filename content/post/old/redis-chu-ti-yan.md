+++
title = "Redis - 初體驗"
date = 2016-08-16T17:18:36Z
author = "Alibaba"
categories = ["Redis"]
tags = ["Redis"]
description = ""
slug = "redis-chu-ti-yan"
draft = false

+++

## 安裝 redis server
1. yum 安裝 ,先使用epel repo 方式。
```
yum install epel-release
```
1-1.更改 /etc/yum.repos.d/epel.repo
```
[epel]
.....
enabled=0
.....
```
1-2.安裝 redi
```
yum --enablerepo=epel install redis
```
2.注意事項:
2-1. **安裝 redis-server,與jemalloc有相依性** 
2-2. **centos5需要有TCL套件 8.5 以上版本才能支援,因此 redis 3.x預設是無法安裝,需update tcl 到 8.5以上**

3./etc/redis.conf 預設  port 6379; bind 127.0.0.1 ,若只要讓本機存取 redis server,不用設定直接啟動即可。
```
/etc/init.d/redis start
netstat -tlnp

Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name   
tcp        0      0 127.0.0.1:6379              0.0.0.0:*                   LISTEN      3454/redis-server
```

3-1. 若需要更改設定 ,以下是基本
```
 port  6379      #指定 port 6379 ,(Default)
 bind  0.0.0.0   #若要讓其他Host 連進來,就要聽 0.0.0.0 (記的防火牆要開 port) 
 maxmemory 1G    #指定份配 記憶體 1G容量
```

## redis 基本指令
* 指令help
```
   redis 127.0.0.1:6379> help keys
	redis 127.0.0.1:6379> help type
		TYPE key
		summary: Determine the type stored at key
		since: 1.0.0
		group: generic
```
* 查看key 是哪一種數據結構
```
	127.0.0.1:6379> type andy
	string
    127.0.0.1:6379> type it
	list
```
* 看目前所有的keys
```
redis 127.0.0.1:6379> keys * 
```
* 設定 key valuse
```
redis 127.0.0.1:6379> set andy Google
	OK
```
* 取key的值
```
 redis 127.0.0.1:6379> get andy
  "Google"
```
* 清除所有 redis DB
```
 redis 127.0.0.1:6379> flushdb
  OK
```
* show redis 相關資訊
```
 redis 127.0.0.1:6379> info
```
* 設定slave
```
  redis 127.0.0.1:6379> slaveof  master  port
  redis 127.0.0.1:6379> slaveof  172.17.0.15 6378
```
* 取消設定slave
```
 redis 127.0.0.1:6379> slaveof on one
```
## 參考資料
* [redis介紹](http://wiki.jikexueyuan.com/project/redis-guide/instruction.html)
* [redis.conf 配置詳解 ](http://blog.csdn.net/ithomer/article/details/9232891)
