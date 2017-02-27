+++
date = 2017-02-20T15:11:26Z
author = "Alibaba"
categories = ["RaspberryPi","RPi","樹莓派","IoT"]
tags = ["RaspberryPi","RPi","樹莓派","IoT"]
description = ""
slug = "rpi1"
draft = false
title = "Raspberry Pi :使用ttl線登入Console 端"

+++

## 說明:
在管理電腦或伺服器若網路或相關遠端登入不通時,就會跑到電腦前面插上螢幕,鍵盤,滑鼠等來登入電腦console端做管理。但不可避免的要被許多條線給牽絆。早期會使用RS-232或是 RS-232轉USB 等線頭,一端是RS-232插到伺服器上,另一端usb插到筆電,再安裝驅動及設定序列阜 **com port** ,**Speed**等設定,即可開啟console 端。這邊所要介紹是**[Raspberry Pi](https://zh.wikipedia.org/zh-hk/%E6%A0%91%E8%8E%93%E6%B4%BE"target="_blank)**(單板電腦),這個**Raspberry Pi**本身沒有RS-232介面,只能使用USB轉ttl來登入console 端。

## 準備材料
 - Raspberry Pi ,(第1代 - 第3代都可以)
 - USB轉ttl (價格約70~100元左右), (購買可以到一般電子材料行 ,[台灣樹莓派](https://www.raspberrypi.com.tw/2037/505/"target="_blank),或是[露天](http://search.ruten.com.tw/search/s000.php?enc=u&searchfrom=classf&enc=u&c=0011&k=usb%E8%BD%89ttl%E7%B7%9A"target="_blank))
 - USB 電源 5V 2A

## 整體展示
![AllShow](/content/images/2017/02/ALLShow.jpg)
## 實作步驟
- 將TTL線 接到 Raspberry Pi 上的 GPIO, usb 接到筆電
![GPIO_RPI3](/content/images/2017/02/RasberryPi3_GPIO--.PNG)
![TTLtoUSB](/content/images/2017/02/TTL-USB.PNG)
```
TTL線 黑 (接地) 接到  GPIO pin 06 (接地)
TTL線 白  (RX)  接到  GPIO pin 08 (UART TXD)
TTL線 綠  (TX)  接到  GPIO pin 10 (UART RXD)
```
> 注意: Raspberry Pi 3 有藍芽裝置,預設開啟藍芽會占用掉 UART,也就是 GPIO **pin 8**及 **pin 10**的位置。解決方法是 將 RPI3 SD卡裡 有一個檔案 config.txt,在此檔案最後面加上 **dtoverlay=pi3-disable-bt**,指示關掉藍芽。進系統後,徹底將藍芽服務關閉 (systemctl disable hciuart)

- 為了要使用 usb轉TTL 線,筆電需要安裝好驅動程式
大部分 TTL轉USB線都是使用 旺玖科技的晶片,因此到此下載驅動程式
[PL2303](http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=225&pcid=41"target="_blank)

> 注意:目前PL2303 支援 Win8 及Win 10 不是很好,若要使用看[這篇解決](http://yhhuang1966.blogspot.tw/2015/07/pl2303hx-win8.html"target="_blank) 

- 設定Serial終端機 (這邊使用[putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html"target="_blank))
以上步驟都好之後,將TTL轉USB的USB 插到筆電,要到裝置管理員查看是否有抓到 ,並且是第幾個com port。下圖看到是 com port 8
![com_port](/content/images/2017/02/DeviceManager_COM.PNG)
之後 開啟putty 終端機 設定 com port 及 速率為115200
![putty_config](/content/images/2017/02/putty.PNG)
- 開啟終端機是否有開機的訊息出來
![putty_login](/content/images/2017/02/putty_login.PNG)

## 參考資料:
 - [台灣樹莓派-從序列埠登入到 Raspberry Pi](https://www.raspberrypi.com.tw/1999/connect-to-raspberry-pi-via-serial/){: target="_blank"}
 - [Raspberry Pi：透過序列埠登入系統](http://yehnan.blogspot.tw/2013/09/raspberry-pi.html"target="_blank)
 - [樹莓派基礎-從電腦PC USB轉TTL 序列埠(Serial TTY)登入到 Raspberry Pi3](https://docs.com/randy1977/2542/pc-usbttl-serial-tty-raspberry-pi-3#"target="_blank)
 - [使用 PL2303 HXD USB 轉 TTL 傳輸線，從序列埠登入到 Raspberry Pi B+](https://blog.gtwang.org/iot/pl2303-hxd-usb-ttl-raspberry-pi-b-plus/"target="_blank) 
