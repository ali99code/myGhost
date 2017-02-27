+++
tags = ["Docker"]
description = ""
slug = "docker-openvpn"
draft = false
title = "Docker for OpenVPN"
date = 2016-09-28T08:34:29Z
author = "Alibaba"
categories = ["Docker"]

+++


## Test Example for OpenVPN Client
***
 - 先在 Host端放置 openvpn 設定檔 (*ovpn 或是 *.conf)

```
# ls -l /tmp/vpngate/
st1.conf  st2.conf  st3.conf  vpn.conf
```
 - 假設 此 image id (8ef0cd679f9)已有 openvpn 主程式
```
# docker images
REPOSITORY                         TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
alicode/svpn                       t2                  8ef0cd679f96        2 hours ago         539.2 MB
......
```
- 簡單建立 container for OpenVPN Client

```
# docker run -it --cap-add=NET_ADMIN --device /dev/net/tun  --name svpnT2 -v /tmp/vpngate/:/etc/openvpn -d   8ef0cd679f9   bash -c  'openvpn --config /etc/openvpn/st3.conf'
```
- 呈上一個項目 的看盤重點是
  * **--cap-add=NET_ADMIN** ,因為 container 預設使用的網路模式無法自行在 container更改網路相關參數
  *  run 在 container 的 OpenVPN 的主程式 不要加 **--daemon** 參數
## 參考資料:
***
 - [dperson/openvpn-client
](https://github.com/dperson/openvpn-client)