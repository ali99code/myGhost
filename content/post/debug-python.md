+++
description = ""
slug = "debug-python"
draft = false
title = "Debug 工具 for Python"
date = 2016-10-24T08:53:27Z
author = "Alibaba"
categories = ["Python"]
tags = ["Python"]

+++

## try/except
```
#!/usr/bin/env python
try:                                                                                                                                  
    myHours=raw_input('Enter Hours: ')
    myRate=raw_input('Enter Rate: ')
    mypay=int(myHours) * float(myRate)
    print(mypay)
except:
    print "The Error"
```
- 正常執行

```
# python test.py 
Enter Hours: 20
Enter Rate: 30
600.0
```
 
- 錯誤執行

```
# python test.py 
Enter Hours: ee
Enter Rate: eee
The Error
```

## 利用邏輯表達截斷有問題的語句
利用從左至右進行判斷, y!=0 and x/y >1 。當 y=0時,就會顯示 
False
```
>>> x=6
>>> y=1
>>> x/y >1
True
>>> x=6
>>> y=0
>>> x/y >1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: integer division or modulo by zero
>>> x=6
>>> y=0
>>> y!=0 and x/y >1
False
>>>
```