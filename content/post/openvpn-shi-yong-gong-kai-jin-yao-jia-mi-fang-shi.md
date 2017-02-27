+++
description = ""
slug = "openvpn-shi-yong-gong-kai-jin-yao-jia-mi-fang-shi"
draft = false
title = "OpenVpn-使用公開金鑰加密方式"
date = 2016-07-03T17:15:40Z
author = "Alibaba"
categories = ["OpenVPN"]
tags = ["OpenVPN"]

+++

##建立OpenVPN
1.安裝 OpenVPN
```
   yum install openvpn
```

2.安裝  easy-rsa.noarch

```
yum install easy-rsa.noarch
```

3.copy  easy-rsa
```
cp -a /usr/share/easy-rsa/2.0/  /etc/openvpn/
cd /etc/openvpn/2.0
```
4.編輯 vars   
```
      #export KEY_NAME="EasyRSA"
      KEY_NAME="AliRSA"
```
```
chmod a+x vars
```
    
    
    
##建立OpenVPN 憑證

1.建立 CA 憑證
```
  cd  /etc/openvpn/2.0/
    source vars
    ./clean-all
    ./build-ca
```

2.建立 server 憑證

```
./build-key-server MyWeb
```

3.建立client 憑證

```
./build-key myghost
```

4.建立build-dh

```
./build-dh
```

##設定 OpenVPN Server 設定檔
1.vim /etc/openvpn/server.conf
```
user openvpn
group openvpn
port  5628
proto tcp
dev tun
ca /etc/openvpn/2.0/keys/ca.crt
cert /etc/openvpn/2.0/keys/MyWeb.crt
key /etc/openvpn/2.0/keys/MyWeb.key
dh /etc/openvpn/2.0/keys/dh2048.pem
server 192.168.130.0 255.255.255.0
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"
#push  "route 10.130.0.0 255.255.0.0"
#push  "route 172.17.0.0 255.255.0.0"
plugin /usr/lib64/openvpn/plugins/openvpn-plugin-auth-pam.so openvpn
duplicate-cn
keepalive 20 60
comp-lzo
persist-key
persist-tun
daemon
log-append /var/log/openvpn.log
verb 3
#crl-verify /etc/openvpn/crl.pem
```
2.啟動 OpenVPN

```
/etc/init.d/openvpn start
```

3.建立 Client OpenVPN 設定檔

```
client
dev tun
proto tcp
remote ghost.utshop.tw  5628
remote-random
comp-lzo
verb 3
auth-user-pass
<ca>
  ca.crt
</ca>
<cert>
  Client .crt
</cert>
<key>
  Client  Key
</key>
```

##讓OpenVPN Client 用帳密登入
1.vim /etc/pam.d/openvpn

```
auth    required        pam_unix.so    shadow    nodelay
account required        pam_unix.so
```  

2./etc/openvpn/server.conf

```
...
plugin /usr/lib64/openvpn/plugins/openvpn-plugin-auth-pam.so openvpn
...
```
##參考資料

- [OpenVPN 安裝與設定-蔡宗融](http://www.ichiayi.com/wiki/tech/openvpn)

- [Configuring client-specific rules and access policies-Openvpn How to](http://openvpn.net/index.php/open-source/documentation/howto.html#policy)

- [OpenVPN 建置筆記(第7集)-紐菲斯部落格](http://blog.nuface.tw/?p=1347)