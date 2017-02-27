+++
author = "Alibaba"
categories = ["Python"]
tags = ["Python"]
description = ""
slug = "unicode-python"
draft = false
title = "Python2.x 與 Python3.x 在中文編碼差異"
date = 2016-11-15T18:03:14Z

+++

Python 3.x 支援 Unicode, 而 Python2.x 要呈現中文需在 Script 的開頭 標上 '# -*- coding: utf-8 -*-' 

## 將特殊字元轉換一般字元
```
>>> print r"Hello\tWorld"
Hello\tWorld
>>> print "Hello\\tWorld"
Hello\tWorld
```

## Unicode
無論平台、程式、語言是甚麼,Unicode為每一個字元提供一個獨一無二的號。

- Python 2.x 所有文字都是str實例 ,用前置 u 表示 使用 unicode型態封裝文字。

```
>>> aa=u'HelloWorld'
>>> type(aa)
<type 'unicode'>

>>> ss=u"峻"
>>> type(ss)
<type 'unicode'>
>>> len(ss)  #使用unicode,表1個字
1

>>> st="峻"
>>> type(st)
<type 'str'>
>>> len(st)  #表使用 3 個 Byte,位元組序列 
3
```

- Python 3 所有文字都是 unicode 字串

```
Python 3.4.4 (default, Mar 22 2016, 14:02:38) 
>>> st="峻"
>>> type(st)
<class 'str'>
>>> len(st) #使用unicode,表1個字。可以與Python 2.x比較 
1
```