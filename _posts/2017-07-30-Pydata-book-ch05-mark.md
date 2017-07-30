---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第五章Pandas笔记 
tags: Python
---

* content
{:toc}

本篇记录了pandas的一些特性和功能





## 两个重要数据结构

#### Seires

Series是一种类似于一维数组的对象，它由一组是数据（各种numpy的数据类型）以及一组与之相关的索引组成。

series.values ： 值， serie.index ： 索引

字符串表现形式，索引在左，值在右边，若没有给出索引，则默认创造0~n-1的整数型索引。

** 创建 **  ：` obj = Series([4, 7, -5 ,3], index = ['a', 'b', 'd', 'c']) `

** 多项查询 ** ： ` obj[['a', 'c', 'd']] `

也可以通过一个字典来创建series

若同时传入一个给出字典和一个索引，则会通过索引在字典中查询值，没有的则为NaN（在pandas中用于表示缺失或NA值）。isnull和notnull可以用于检测缺失数据：

```python
In [8]: sdata
Out[8]: {'fate': 4000, 'jack': 2000, 'tom': 100}

In [9]: sindex
Out[9]: ['tom', 'jack', 'fate', 'califonia']

In [10]: obj = pd.Series(sdata, index = sindex)

In [11]: obj
Out[11]:
tom           100.0
jack         2000.0
fate         4000.0
califonia       NaN
dtype: float64

In [12]: pd.isnull(obj)
Out[12]:
tom          False
jack         False
fate         False
califonia     True
dtype: bool

In [13]: pd.notnull(obj)
Out[13]:
tom           True
jack          True
fate          True
califonia    False
dtype: bool
```

Series对象本身及其索引都有一个name属性，该属性跟pandas其他的关键功能关系密切:

```python
obj.name = 'population'
obj.index.name = 'state'
```

obj的索引可以通过赋值的方式就地修改

#### DataFrame

