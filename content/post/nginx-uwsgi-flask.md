+++
description = ""
slug = "nginx-uwsgi-flask"
draft = false
title = "Nginx + uwsgi + Flask"
date = 2017-01-22T18:04:18Z
author = "Alibaba"
categories = ["Python","Flask"]
tags = ["Python","Flask"]

+++

## 目標
  本範例主要演示利用Python 撰寫一個取得目前自己的IP及此IP是屬於哪個國家。類似這樣一個網站[What is my IP address](https://ifconfig.co/"target="_blank),另外要使用Python 做一個動態網頁可以使用支援Python 的框架(FrameWork),如 [**Flask**](http://docs.jinkan.org/docs/flask/"target="_blank) [**Bottle**](http://bottlepy.org/docs/dev/"target="_blank) [**Django**](https://www.djangoproject.com/)等。本範例是使用[Flask](https://zh.wikipedia.org/wiki/Flask"target="_blank)這個微框架,並搭配**Nginx** (Web Server) 及 透過**uWSGI** (server ,或是一種可與支援的程式溝通的協議)與 Python 溝通。 

## 架構說明
 - 程式目錄內容
```
/var/www/html/WebPython/
├── getip.py
└── uwsgi2.ini
```
 - 圖示說明
![nginx_uWSGi_Python](/content/images/2017/01/nginx_uWSGI_Python.png)
## install  nginx
 - um install nginx 

## install  Flask  ,uwsgi
 - pip install flask
 - pip install uwsgi

## 編輯 Python 程式
```
from flask import Flask , request , jsonify
from geoip import open_database

GeoipDataBase="/usr/local/share/GeoIP/GeoLite2-Country.mmdb"
dbip=open_database(GeoipDataBase)

def getcountry(ip):
    ipstr=str(dbip.lookup(ip).country)
    return ipstr
    dbip.close()


app = Flask(__name__)

#@app.route('/')
#def index():
#    return "<h1>Hello Andy</h1>"

@app.route("/",methods=["GET"])
def getip():
    ip=request.remote_addr
    ip_country="<h1>"  + getcountry(ip) + "\t" + ip + "</h1>"
    return ip_country  , 200



if __name__ == "__main__":
    app.run(host='188.166.241.76',port=80, debug=True)
```
## uWSGi
 - edit uWSGi Config  (uwsgi2.ini)
```
[uwsgi]
socket = 127.0.0.1:9091
uid = 997
chdir = /var/www/html/WebPython/
module = getip
callable = app
master = true
workers = 3
reload-mercy = 10   
max-requests = 100
pidfile = /var/run/uwsgi9090.pid
daemonize = /var/log/uwsgi9090.log
```
 - 開啟uWSGi服務
```
uwsgi --ini  uwsgi2.ini
```
 - 觀察是否有uWSgi程序或是開啟 9091阜口

```
ps aux|grep uwsgi
nginx    15803  0.0  3.5 235132 17592 ?        S    10:51   0:01 uwsgi --ini uwsgi2.ini
nginx    15808  0.0  2.8 235264 14196 ?        S    10:51   0:00 uwsgi --ini uwsgi2.ini
nginx    15809  0.0  2.9 235696 14776 ?        S    10:51   0:00 uwsgi --ini uwsgi2.ini
nginx    15810  0.0  2.9 235728 14812 ?        S    10:51   0:00 uwsgi --ini uwsgi2.ini
```
```
netstat  -tlnp

tcp        0      0 127.0.0.1:9091          0.0.0.0:*               LISTEN      15803/uwsgi
```
## stop or reload uwsgi
在修改python 程序後,並不會立即生效,需要reload uwsgi

  - stop uwsgi
```
   uwsgi  --stop /var/run/uwsgi9090.pid
   rm -rf /var/run/uwsgi9090.pid
```
  - reload uwsgi
```
uwsgi --reload /var/run/uwsgi9090.pid
```
  


## edit Nginx Config and  Start Nginx

  - /etc/nginx/conf.d/uwsgi.conf
 
```
     server {
        listen       80; 
        server_name 192.168.200.1 ip.aligogo.pw;
        location / { 
          include  uwsgi_params;
          uwsgi_pass  127.0.0.1:9091;
          #uwsgi_pass  unix:/var/www/html/uwsgi.sock;                                                                                 
          #uwsgi_param UWSGI_SCRIPT hello.py;
          #uwsgi_param UWSGI_CHDIR /root/Python/introducing-python/myTest;
          #index  index.html index.htm;
    
        } 
```
 - nginx -t  && systemctl start  nginx  
  
## 測試
```
curl http://ip.aligogo.pw
```
## 參考資料:
 - [uWSGI – 碼人日誌](https://coder.tw/?p=6375"target="_blank)
 - [[Flask] 怎麼取得使用者的 IP 位址?](http://www.victorgau.com/?p=4140"target="_blank)