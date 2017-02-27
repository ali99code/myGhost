+++
date = 2017-01-03T15:10:05Z
author = "Alibaba"
categories = ["Linux","RPM","SRPM"]
tags = ["Linux","RPM","SRPM"]
description = ""
slug = "srpm"
draft = false
title = "自編RPM ... 待續"

+++

## Macros
 - 在執行rpmbuild 指令編譯及打包時,定義一些環境變數。大陸用語稱為**宏**
 - 一般來說以RedHat系列版本,都定義在此檔**/usr/lib/rpm/macros**
 - 藉由rpm指令顯示出**rpmrc**及**macro**
```
rpm --showrc
```
 - 藉由rpm指令顯示出 macro 的值。example: %{_topdir}
```
rpm --eval %{_topdir}
```

## rpmbuild常用指令
- 只會打包成 **FileName.rpm**檔(二進制Binary)。
```
rpmbuild -bb
```
- 打包成 **rpm**也同時打包成**srpm**
```
rpmbuild -ba 
``` 
- 檢測SPEC文件中**BuildRequires**的相依套件此環境是否已完成
```
rpmbuild --nobuild
```
- 其餘參數如下
```
-bc  #僅執行到spec文件中的%build階段即停止進行
-bp  #僅執行至spec文件中的%prep階段即停止進行
-bi  #僅運行至spec文件中的%install階段即停止運行
-bl  #檢查spec文件中%file段所列出的文件是不是與BUILDROOT目錄中存在的文件完全匹配
-bs  #僅製作生成源碼格式的rpm包
```


## 參考資料:
- [【 馬哥教育內部文檔】-RPM製作指南 ](http://mageedu.blog.51cto.com/4265610/1205205"target="_blank)
- [Fedora_How to create an RPM package_繁體中文版](https://fedoraproject.org/wiki/How_to_create_an_RPM_package/zh-tw"target="_blank)
- [RPM打包](http://mattshma.github.io/2015/11/04/rpm%E6%89%93%E5%8C%85/"target="_blank)