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

__创建__   ：` obj = Series([4, 7, -5 ,3], index = ['a', 'b', 'd', 'c']) `

__多项查询__ ： ` obj[['a', 'c', 'd']]  `

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

DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值，字符串，布尔值等）。DataFrame既有行索引也有列索引，它可以被看做由Series组成的字典。

__创建__  :

`DataFrame(data, index = , columns = )`

index用来指定行序，columns用来指定列序，匹配不上则自动填充缺省值。

__索引__ :

通过类似字典标记的方式或属性的方式，可将DataFrame的列获取为一个Series。

返回的Series拥有原DataFrame相同的索引，且其name属性也已经被相应的设置好了。

__赋值__

如果将列表或数组复制给某个列时，其长度必须跟DataFrame的长度相匹配。
若赋值的是一个Series，就会精确匹配DataFrame的索引，所有空位都会被天上缺失值。

为不存在的列复制会创建一个新的列。

__通过索引方式返回的列只是相应
的数据的视图而已，并不是副本。因此，对返回的Series所做的任何就地修改都会反映到源DataFrame上。通过Series的copy方法即可显式的复制列__

__另一种常见的数据形式：嵌套字典（字典的字典）__

```python
In [5]: pop = {'Nevada': {2001: 2.4, 2002: 2.9},
   ...: 'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}

In [6]: frame = pd.DataFrame(pop)

In [7]: frame
Out[7]: 
      Nevada  Ohio
2000     NaN   1.5
2001     2.4   1.7
2002     2.9   3.6
```

可以看到，若将嵌套字典传给DataFrame，它会被解释为：外层字典的键作为列，内层键则作为行索引

可以通过.T进行转置。如果未显式的指定索引，则内层字典的键会被合并，排序易形成最终的索引。

DataFrame的index和column都有name属性。

跟Series一样，values属性也会以二维ndarray的形式返回DataFrame
