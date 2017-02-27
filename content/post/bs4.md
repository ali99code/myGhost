+++
categories = ["Python"]
tags = ["Python"]
description = ""
slug = "bs4"
draft = false
title = "簡單示範 Beautiful Soup用法"
date = 2017-01-14T07:24:46Z
author = "Alibaba"

+++

## 簡介
**Beautiful Soup** 是剖析HTML 及 xml 的Python Library。通常在寫網路機器人或是網路爬蟲相關程式時,會大量取回HTML資料,透過正規表示法及其他工具運用來處理取回的資料,來獲取我們所需要的資料,但資料處理的過程中是很繁瑣的,因此使用**Beautiful Soup** 可以 從 HTML 或 XML提取我們要的資料,使過程簡單化。
## 安裝
- 這邊演示是用**Python 2.7.x**

```
pip install beautifulsoup4
```
     

- 為抓取網路上的資料,再額外裝[requests](http://docs.python-requests.org/en/master/"target="_blank)
```
 pip  install requests
```  
## 示範抓取資料
 - 這邊使用 proxpn VPN 商提供VPN 服務資料來當作[此範例](http://proxpn.com/updater/locations-v3.xml"target="_blank)
 - 載入 requests 及 BeautifulSoup模組
```
Type "help", "copyright", "credits" or "license" for more information.
>>> import requests
>>> from bs4 import BeautifulSoup
```
 - 抓取資料,會response回來很多HTML相關tag資料。
```
>>> url="http://proxpn.com/updater/locations-v3.xml"
>>> rs=requests.get(url)
>>> rs.text
```
 - 開始使用Beautiful Soup
```
>>> soup=BeautifulSoup(rs.text,'html.parser')
```
 - 依照此資料格是的tag (location)抓取第一筆資料
```
>>> soup.location

<location>\n<name>Free Chicago, IL</name>\n<type>free</type>\n<openvpn-udp ip="196.52.21.65" key="free-chi1">\n</openvpn-udp>\n<openvpn-udp ip="196.52.21.134" key="free-chi2">\n</openvpn-udp>\n</location>
```
 - 呈上,取的"type" tag 的資料
```
>>> soup.location.type.string
u'free'
```
 - 利用 find_all 搜索 屬於 "name"的tag,資料回應以串列格式,並計算筆數,列出第二筆 "name" tag 資料
```
>>> soup.find_all('name')

[<name>Free Chicago, IL</name>, <name>New York, USA</name>, <name>Miami, USA</name>, <name>Los Angeles, USA</name>, <name>Seattle, USA</name>, <name>Chicago, USA</name>, <name>Toronto, CA</name>, <name>Amsterdam, NL</name>, <name>Stockholm, SE</name>, <name>London, UK</name>, <name>Bucharest, RO</name>, <name>Sydney, AU</name>, <name>Frankfurt, DE</name>, <name>Singapore, SG</name>, <name>Hong Kong, CN</name>, <name>San Jose, CR</name>, <name>Hafnarfjordur, IS</name>, <name>Zurich, CH</name>, <name>Trial</name>, <name>Miami</name>, <name>New York</name>, <name>Chicago</name>, <name>Seattle</name>, <name>Frankfurt</name>, <name>Stockholm</name>, <name>Amsterdam</name>, <name>London</name>]
>>> len(soup.find_all('name'))
27

>>> soup.find_all('name')[1].string
u'New York, USA'

>>> soup.find_all('name')[1].text
u'New York, USA'
``` 

- 使用像 CSS 選擇器的方式,篩選出 "location" tag 中 "openvpn" tag 的資料。資料回應以串列輸出

```
>>> soup.select('location > openvpn')
``` 

 - 呈上, 取第二筆資料 ,屬性值 "key" 及 "ip"。或是使用attrs 方法使屬性資料,回應成字典的格式
 
```
>>> soup.select('location > openvpn')[1]
<openvpn ip="192.240.98.3" key="openvpn-mfl">\n</openvpn>

>>> soup.select('location > openvpn')[1]['ip']
u'192.240.98.3'

>>> soup.select('location > openvpn')[1]['key']
u'openvpn-mfl'

>>> soup.select('location > openvpn')[1].attrs
{u'ip': u'192.240.98.3', u'key': u'openvpn-mfl'}
```

## 利用此範例 及 上面取資料方法,組合一個簡單的程式

```
#!/usr/bin/env python
import requests
from bs4 import BeautifulSoup
url="http://proxpn.com/updater/locations-v3.xml"
sess=requests.Session()
rs=sess.get(url)
soup=BeautifulSoup(rs.text,'html.parser')
for  iteam in soup.find_all('location'):
     vpnName=iteam.select('name')[0].text
     vpnType=len(iteam.select('type'))
     vpnUdp=len(iteam.select('openvpn-udp'))
     vpnTcp=len(iteam.select('openvpn'))
     if vpnType >0:
        vpnType=iteam.select('type')[0].text
     if vpnTcp >0:
        for vpnTcp in iteam.select('openvpn'):
            print "TCP",vpnName,";",vpnType,";",vpnTcp.get('key'),";",vpnTcp.get('ip')

     if vpnUdp >0:
        for vpnUdp in iteam.select('openvpn-udp'):
            print "UDP",vpnName,";",vpnType,";",vpnUdp.get('key'),";",vpnUdp.get('ip')
```
  

## 參考資料:
  - [官方 Beautiful Soup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/"target="_blank)
  - [中文版-官方 Beautiful Soup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html"target="_blank)
  - [Beautiful Soup 的用法](http://wiki.jikexueyuan.com/project/python-crawler-guide/beautiful-soup.html"target="_blank)