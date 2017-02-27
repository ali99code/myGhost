+++
title = "Monitor For VMware ... 未完待續"
date = 2016-10-26T17:15:01Z
author = "Alibaba"
categories = ["VMware"]
tags = ["VMware"]
description = ""
slug = "monitor-vmware"
draft = false

+++

## esxtop or resxtop
 - 利用 esxtop 分析 CPU 情況,以下哪三個指標顯示 CPU 爭奪 [參考解讀CPU統計值_英文 ](https://communities.vmware.com/docs/DOC-9279)  
[esxtop 詳細命令_中文](http://www.360doc.com/content/16/0225/11/22078350_537242602.shtml)
    * %RDY (Ready Time: 說明vCPU 要等候多久才有可用的LCPU)
    * %RUN
    * %MLMTD (%RDY值會包含%MLMTD的。與設置了**CPU limit**有關)
 -  通常 %RDY > 10% 情況:
    * % RDY 約等於 %USED 表示 OK
    * % RDY 與 %USED 相差太多 表示 不 OK
 -  通常 (%RDY - %MLMTD) > 20%,表示CPU資源不足