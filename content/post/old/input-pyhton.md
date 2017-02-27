+++
description = ""
slug = "input-pyhton"
draft = false
title = "輸入 for Pyhton"
date = 2016-10-24T08:24:00Z
author = "Alibaba"
categories = ["Python"]
tags = ["Python"]

+++

## raw_input Example  (只適用於 2.x python )

```
#!/ussr/bin/env python
Name=raw_input('Please Input your Name ')
Age=raw_input('Please Input your Age ')

Summary='''My Summary Name: %s Age:  %s''' %(Name,Age)                         
print Summary
```
## input等效於 raw_input (只適用於 3.x python)

```
#!/ussr/bin/env python
Name=input('Please Input your Name ')
Age=input('Please Input your Age ')

Summary='''My Summary Name: %s Age:  %s''' %(Name,Age)
print(Summary)
```