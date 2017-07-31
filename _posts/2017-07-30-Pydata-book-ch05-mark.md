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

#### 索引对象（index对象）

pandas的索引对象负责管理轴标签和其他元数据（比如轴名称等）。构建Series或DataFrame时，所用到的任何数组或其他序列的标签都会被转换成一个index。

Index对象是不可修改的（immutable），因此用户不能对其进行修改，这样才能使Index对象在多个数据结构之间安全共享。

除了长得像数组，index功能也类似于一个固定大小的集合：

Index的方法和属性

|方法|说明|
|:--|:--|
|append|链接另一个Index对象，产生一个新的Index|
|diff|计算差集，并得到一个Index|
intersection|计算交集|
|union|计算并集|
|isin|计算一个指示各值是否都包含在参数集合中的布尔型数组|
|delete|删除索引i处的元素，并得到新的Index|
|drop|删除传入的值，并得到新的Index|
|insert|将元素插入到索引i处，并得到新的index|
|is_monotonic| 当各元素均大于等于前一个元素时，返回True|
|is_unique|当Index没有重复值时，返回True|
|unique|计算Index中唯一值的数组|

## 基本功能

#### 重新索引（reindex）

pandas对象的一个重要方法是reindex，起作用是创建一个适应新索引的新对象.

* Series

当调用Series的reindex时，将会根据新索引进行重排，若某个索引值当前不存在，就引入缺失值，可通过fill_value属性设置缺失值。

```python
In [5]: obj = pd.Series(['blue', 'purple', 'yellow'], index = [0, 2, 4])

In [6]: obj.reindex(['a', 'b', 'c', 'd', 'e'], fill_value = 0)
Out[6]: 
a    0
b    0
c    0
d    0
e    0
dtype: object
```
对于时间序列之类的有序数据，重新索引可能需要做一些插值处理，method选项可达到此目的。

method选项

|参数|说明|
|:--|:--|
|ffill或pad|前向填充值|
|bfill或backfill|后向填充值|

e.g:

```python
In [11]: obj = pd.Series(['blue', 'purple', 'yellow'], index = [0, 2, 4])

In [12]: obj.reindex(range(6), method = 'ffill')
Out[12]: 
0      blue
1      blue
2    purple
3    purple
4    yellow
5    yellow
dtype: object
```

* DataFrame

对于DataFrame，reindex可以修改(行)索引,列，或两个都修改。

如果仅传入一个序列，则会重新索引行，使用columns关键字可以重新索引列，也可以同时对行和列进行重新索引，而插值只能按行应用。

```python
In [18]: frame
Out[18]: 
   Ohio  Texas  California
a     0      1           2
c     3      4           5
d     6      7           8

In [19]: states
Out[19]: ['Texas', 'Utah', 'California']

In [20]: frame.reindex(columns = states)
Out[20]: 
   Texas  Utah  California
a      1   NaN           2
c      4   NaN           5
d      7   NaN           8
```

利用ix的标签索引功能，重新索引功能将更加简洁

```python
In [26]: frame.ix[['a', 'b', 'c', 'd'], states]
Out[26]: 
   Texas  Utah  California
a    1.0   NaN         2.0
b    NaN   NaN         NaN
c    4.0   NaN         5.0
d    7.0   NaN         8.0
```

* reindex函数的参数

|参数|说明|
|--|--|
|index|用作索引的新序列，可以使index实例，也可以是其他序列型的python数据结构，index会被完全使用|
|method|插值方式|
|fill_value|在重新索引时，需要引入缺失值时使用的替代值|
|limit|向前或向后填充时的最大填充量|
|level|在MultiIndex的指定级别上匹配简单索引，否则选取其子集|
|copy|默认为True，无论如何都复制；如为False，则新旧相等就不复制|

#### 丢弃指定轴上的项（drop）

drop方法可以丢弃某条轴上的一个或多个项，可传入一个索引数组或列表，drop返回的是一个在指定轴上删除了指定值的新对象。

对于DataFrame，如要删除列，则必须指出 `axis = 1`，而删除行则不需要。

#### 索引，选取和过滤

Series索引(obj[...]）的工作方式类似于Numpy数组的索引，只不过Series的索引值不只是整数。

其中切片操作可以用标签的切片运算，也可以用普通的python切片，但这两种方法是不同的。
__用标签切片是包含末端的__

还可以通过布尔表达式或数组来进行选取。

```python
In [34]: data
Out[34]: 
          one  two  three  four
ohio        0    1      2     3
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

In [35]: data[data['three'] > 5]
Out[35]: 
          one  two  three  four
Colorado    4    5      6     7
Utah        8    9     10    11
New York   12   13     14    15

In [36]: data < 5
Out[36]: 
            one    two  three   four
ohio       True   True   True   True
Colorado   True  False  False  False
Utah      False  False  False  False
New York  False  False  False  False

In [37]: data[data < 5] = 0

In [38]: data
Out[38]: 
          one  two  three  four
ohio        0    0      0     0
Colorado    0    5      6     7
Utah        8    9     10    11
New York   12   13     14    15
```

也可通过ix进行索引，格式 `ix[行，列]`

DataFrame的索引选项

|类型|说明|
|--|--|
|obj[val]|选取DataFrame的单个列或一组列。在一些特殊情况下会比较便利：布尔型数组（过滤行），切片（行切片），布尔型DataFrame（更具条件设置值）|
|obj.ix[val]|选取DataFrame的单个行或一组行|
|obj.ix[val1, val2]|同时选取行列|
|reindex方法|xxx|
|icol, irow方法|根据整数位置选取单列或单行，并返回一个Series|
|xs方法|更具标签选取单行或单列，并返回一个Series|
|get_value,  set_value方法|根据行标签和列标签选取单个值|


#### 算数运算和数据对齐

pandas最重要的一个功能是，它可以对不同索引的对象进行算术运算。在将对象相加时，如果存在不同的索引对，则结果的索引就是索引对的并集。 

自动的数据对齐操作在不重叠（overlapping）的索引处引入了NA值，缺失值会在算术运算中传播。

```python
In [46]: s1
Out[46]: 
a    7.3
c   -2.5
d    3.4
e    1.5
dtype: float64

In [47]: s2
Out[47]: 
a   -2.1
c    3.6
e   -1.5
f    4.0
g    3.1
dtype: float64

In [48]: s1 + s2
Out[48]: 
a    5.2
c    1.1
d    NaN
e    0.0
f    NaN
g    NaN
dtype: float64
```

#### 在算术方法中填充

当算术运算时，不希望采用NA来填充不重叠处，而是填充其原有值，则不采用算术运算符，而采用如下方法

|方法|说明|
|--|--|
|add|用于加法|
|sub|用于减法|
|div|用于除法|
|mul|用于乘法|

可用fill_value属性指定缺失值

#### DataFrame和Series之间的运算



## 注意

* DataFrame的索引
data[val]返回的是列，而data.ix[val]返回的是行。

* 非重叠（overlapping）
“飞机”和“轮机”都有“机”，所以重叠， ”高富帅“和”矮穷矬“不重叠。
