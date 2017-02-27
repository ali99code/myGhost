+++
categories = ["Python"]
tags = ["Python"]
description = ""
slug = "list_comprehensions"
draft = false
title = "Python 的 生成式 (List Comprehensions)...待續"
date = 2017-02-26T11:55:26Z
author = "Alibaba"

+++


## 串列生成式
- 語法: [運算式 for 項目 in 可佚代項目] 或是 [運算式 for 項目 in 可佚代項目 if 條件式]
- 一個典型例子,列出1 - 100 屬於偶數是哪些

傳統寫法

```
#!/usr/bin/env python
yy=[]
for xx in range(1,101):
    if xx % 2 == 0:
    ┆   yy.append(xx)
print(yy)
```

使用串列生成式寫法,是不是很簡潔只要一列就可解決。

```
#!/usr/bin/env python
print([xx for xx in range(1,101) if xx % 2 == 0])
```

## 字典生成式
- 語法: {鍵運算式:值運算式 for 運算式 in 可佚代項目}
- 看例子,有一個字串為 "jkjireddrrgkQQkfdcSSSewfjwfwwszzssEEWW",要計算每個大寫或小寫字母出現的次數。
看盤重點在 **letter_count={world:strT.count(world) for world in set(strT)}**
```
#!/usr/bin/env python                                                                                                                 
strT="jkjireddrrgkQQkfdcSSSewfjwfwwszzssEEWW"                                                                                         
letter_count={world:strT.count(world) for world in set(strT)}                                                                         
for  world, worldcount in  letter_count.items():                                                                                      
    print(world +  " ==> "+  str(worldcount))
```

## 參考資料:
- [Introducing-Python](http://www.books.com.tw/products/0010690075)