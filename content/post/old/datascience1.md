+++
title = "ndarray ,  Series , DataFrame For Python ... 未完成"
date = 2016-12-10T02:36:10Z
author = "Alibaba"
categories = ["Python","DataScience"]
tags = ["Python","DataScience"]
description = ""
slug = "datascience1"
draft = false

+++

## 三種資料結構  : ndarray ,  Series , DataFrame
###### ndarray 由 NumPy 這個套件所提供的 ,處理 一維及多維陣列的儲存運算。
###### Series : 具有 index 一維的ndarray ,由 pandas 套件所提共的。pandas 是建構在 Numpy的基礎上

###### DataFrame 是由 Pandas 套件所提供的。此資料結構像是spreadsheet(excel 那樣表格)~這邊演示使用Python 3.x
- 利用 pip 先安裝pandas
```
# pip install pandas
# pip list --format=columns
```
- 創建資料 (方式1)
```
>>> df=pd.DataFrame({'pid':['p01','p02','p03'],'cost':[100,300,1000]})
>>> df
   cost  pid
0   100  p01
1   300  p02
2  1000  p03
```
- 創建資料 (方式2)
```
>>> df2=pd.DataFrame([{'pid':'p01','cost':100},{'pid':'p02','cost':300},{'pid':'p03','cost':1000}])
>>> df2
   cost  pid
0   100  p01
1   300  p02
2  1000  p03
```
- 取值 By **欄位**
```
>>> df2['cost']
0     100
1     300
2    1000
Name: cost, dtype: int64

>>> df2[['cost','pid']]
   cost  pid
0   100  p01
1   300  p02
2  1000  p03
```
- 取值 By **列** (取第1 ,2 筆資料)
```
>>> df2[0:2]
   cost  pid
0   100  p01
1   300  p02
```
- 取值 By **條件**
```
>>> df2[df2['cost'] <200]
   cost  pid
0   100  p01
```
- 新增資料 By **欄位**
```
>>> df3=pd.DataFrame()
>>> df3['cid']=['c01','c02','c03','c04','c05']
>>> df3
   cid
0  c01
1  c02
2  c03
3  c04
4  c05
```
- 新增資料 By **列**
```
>>> df2=df2.append(pd.DataFrame([{'pid':'p04','cost':350},{'pid':'p05','cost':1500},{'pid':'p06','cost':3500}]))
>>> df2
   cost  pid
0   100  p01
1   300  p02
2  1000  p03
0   350  p04
1  1500  p05
2  3500  p06

>>> df2=df2.reset_index(drop=True)
>>> df2
   cost  pid
0   100  p01
1   300  p02
2  1000  p03
3   350  p04
4  1500  p05
5  3500  p06
```
- 修改欄位名稱
```
>>> df2=df2.rename(columns={'cost':'spent'})
>>> df2
   spent  pid
0    100  p01
1    300  p02
2   1000  p03
3    350  p04
4   1500  p05
5   3500  p06
```
- 修改第一列資料
```
>>> df2[0:1]=pd.DataFrame([{'pid':'p001','spent':1900}])
>>> df2[0:1]
   spent   pid
0   1900  p001
```
- 刪除欄位
```
>>> del df2['pid']
>>> df2
   spent
0   1900
1    300
2   1000
3    350
4   1500
5   3500


```
- 刪除欄位(使用drop方法)
```
>>> df=df.drop('pid',axis=1)
>>> df
   cost
0   100
1   300
2  1000
```
- 刪除列
```
>>> df2
   spent
0   1900
1    300
2   1000
3    350
4   1500
5   3500
>>> df2=df2.drop([0,1])
>>> df2
   spent
2   1000
3    350
4   1500
5   3500

>>> df2=df2.reset_index(drop=True)
>>> df2
   spent
0   1000
1    350
2   1500
3   3500
```