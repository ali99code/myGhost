+++
slug = "webya-li-ce-shi-webbench"
draft = false
title = "Web壓力測試- webbench"
date = 2016-07-07T03:07:34Z
author = "Alibaba"
categories = ["webbench"]
tags = ["webbench"]
description = ""

+++

#Web Bench
1.安裝
```
cd /usr/local/src/
wget http://home.tiscali.cz/~cz210552/distfiles/webbench-1.5.tar.gz
tar zxf webbench-1.5.tar.gz
cd webbench-1.5/
make 
make install
```
1-2.若是在 Centos 平台下安裝的話,在 make install 會出現 錯誤訊息


**install: cannot create regular file ‘/usr/local/man/man1’: No such file or directory**


1-3.在執行編譯之前,需要更改 Makefile (cd /usr/local/src/webbench-1.5/)

```
....
#install -m 644 webbench.1 $(DESTDIR)$(PREFIX)/man/man1   改成 下面的方式
install -m 644 webbench.1 /usr/local/share/man/man
....
```


2.使用法:
```
./webbench -c 50 -t 10 http://test100man.wbao88.com/showSession.php
```
```
-c 平行數量
-t  持續時間  ; -t 120 ;表 持續120秒
```

##參考資料:
* [Web Bench 1.5](http://home.tiscali.cz/~cz210552/webbench.html)
* [WEB 壓測軟體-siege and Webbench](http://shinchuan1.blogspot.tw/2013/11/web.html)