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

普通的算数运算符只能匹配到列，并且沿着行向下广播
```python
In [8]: series
Out[8]: 
b    0.0
d    1.0
e    2.0
Name: Utah, dtype: float64

In [9]: frame
Out[9]: 
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0

In [10]: frame - series
Out[10]: 
          b    d    e
Utah    0.0  0.0  0.0
Ohio    3.0  3.0  3.0
Texas   6.0  6.0  6.0
Oregon  9.0  9.0  9.0
```

要想匹配行并在列上广播，必须使用算术方法。

```python
In [8]: series
Out[8]: 
b    0.0
d    1.0
e    2.0
Name: Utah, dtype: float64

In [9]: frame
Out[9]: 
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0

In [10]: frame - series
Out[10]: 
          b    d    e
Utah    0.0  0.0  0.0
Ohio    3.0  3.0  3.0
Texas   6.0  6.0  6.0
Oregon  9.0  9.0  9.0
```

传入的轴（axis）号就是希望匹配的轴。

#### 函数应用和映射

Numpy的ufunc（元素级数组方法）可用于操作pandas对象。

通过apply方法可以将函数应用到由各列或行所形成的一位数组上。

```python
In [15]: f = lambda x: x.max() - x.min()

In [16]: frame
Out[16]: 
          b     d     e
Utah    0.0   1.0   2.0
Ohio    3.0   4.0   5.0
Texas   6.0   7.0   8.0
Oregon  9.0  10.0  11.0

In [17]: frame.apply(f)
Out[17]: 
b    9.0
d    9.0
e    9.0
dtype: float64

In [18]: frame.apply(f, axis = 1)
Out[18]: 
Utah      2.0
Ohio      2.0
Texas     2.0
Oregon    2.0
dtype: float64
```

通过applymap方法可以应用元素级函数，如格式化各浮点值的字符

```python
In [19]: format = lambda x : '%.2f' % x

In [20]: frame.applymap(format)
\Out[20]: 
           b      d      e
Utah    0.00   1.00   2.00
Ohio    3.00   4.00   5.00
Texas   6.00   7.00   8.00
Oregon  9.00  10.00  11.00
```

Series有一个应用元素级函数的map方法：

#### 排序和排名

要对行或列索引进行排序（按字典顺序），可用sort_index方法，它返回一个已排序的对象。

* 排序（sorting）
sort（）
	* 对于DataFrame，则可以根据任意一个轴上的索引进行排序，只要给出axis 值，默认0.

		数据默认是按升序排序的，但是若给出`ascending = False`，则可以降序排序，

		by属性可已接受一个标签或者多个标签的列表，则会按照这些给出的标签进行排序

	* 对于Series，可使用order方法瞥视
	在排序时，任何缺失值默认被放到末尾。

* 排名（ranking）

rank（）
排名会增设一个排名值（从1开始，一直到数组中有效数据的数量）。它跟numpy.argsort产生的间接排序索引差不多，只不过它可以根据某种规则破坏平级关系。

通过method选项破坏平级关系
|method|说明|
|'average'|默认：在相等分组中，为各个值分配平均排名|
|'min'|使用整个分组的最小排名|
|'max'|使用整个分组的最大排名|
|'first'|按值在原始数据中出现的顺序分配排名|

#### 带有重复值的轴索引

可索引带有多个重复标签。

##汇总和计算描述统计

约简方法（如sum）的选项

|选项|说明|
|--|--|
|axis|约简的轴。DataFrame的行用0，列用1|
|skipna|排除缺失值，默认值为True|
|level|如果轴是层次化索引的（即MultiIndex），则根据level分组约简|

|描述和统计汇总|说明|
|--|--|
|count|非NA值的数量|
|describe|针对Series或各DataFrame列计算汇总统计|
|min，max|最大最小值|
|argmin，argmax|计算能够获取到最小值和最大值的索引位置（整数）|
|idmin，idmax|计算能够获取到最小值和最大值的索引值|
|quantile|计算样本的分位数（0到1）|
|sum|总和|
|mean|平均数|
|median|算术中位数|
|mad|平均绝对离差|
|var|方差|
|std|标准差|
|skew|样本值的偏度（三阶矩）|
|kurt|样本值的峰度（四阶矩）|
|cumsum|累积和|
|cummin，cummax|累计最大值和累计最小值|
|cumprod|样本值的累计积|
|diff|一阶差分（对时间序列很有用）|
|pct_change|计算百分数变化|

#### 相关系数与协方差
Series的corr方法用于计算两个Serires中重叠的，非NA的，按索引对齐的值的相关系数，cov用于计算协方差。

Dataframe的corr，cov方法用于以DataFrame的形式返回完整的相关系数或协方差矩阵

DataFrame的corrwith方法可以计算其列或行跟另一个Series或DataFrame之间的相关系数。传入一个Series将返回一个相关系数值Series（针对各列进行计算）

#### 唯一值，值计数，以及成员资格

|方法|说明|
|--|--|
|isin|计算一个表示“Series各值是否包含于传入的值序列中”的布尔型数组|
|unique|计算Series中的唯一数组，按发现的顺序返回|
|value_counts|返回一个Series，其索引为唯一值，其值为频率，按计算数值降序排列|

## 处理缺失数据

NaN表示浮点和肺腑点数组中的缺失数据，他只是一个便于被检测出来的标记。
python内置的None值也会被当做NaN处理

#### Na处理方法

|方法｜说明｜
|--|--|
|dropna|根据各标签的值中是否存在缺失数据对标签进行过滤，可通过阈值调节对缺失值的容忍度|
|fillna|用于填充缺失数据|
|isnull|返回一个含有布尔值的对象，这些布尔值表示那些值是缺失值NA，该对象的类型与源类型一样|
|notnull|isnull的否定式|

#### 滤除缺失数据

* Series可以通过dropna方法或者布尔型索引达到此目的

* DataFrame相对复杂一点，直接调用dropna方法将会默认丢弃任何含有缺失值的行。
	* 通过`how = ‘all`'可以只丢弃全为NA的那些行。
	* 通过`thresh = n`可以保留含有n项非Na数据的行。
	
#### 填充缺失数据

fillna函数的参数（续）

|参数|说明|
|--|--|
|axis|轴|
|inplace|修改调用者对象而不产生副本|
|limit|（对于前向和后向填充）可以连续填充的最大数量|

## 层次化索引

层次化索引（hierarchical indexing）是pandas的一项重要功能，它使你能在一个轴上拥有多个（两个以上）索引级别。抽象点说，它能使你以低纬度形式处理高维度数据。

Series：

```python
In [38]: data = pd.Series(np.random.randn(10),
    ...: index = [['a', 'a', 'a', 'b', 'b', 'b', 'c', 'c', 'd', 'd'],
    ...:          [1, 2, 3, 1, 2, 3, 1 ,2, 2, 3]])


In [39]: data
Out[39]: 
a  1    0.548957
   2    1.007071
   3    0.850420
b  1    1.708313
   2    0.674337
   3    1.331625
c  1   -0.043498
   2    0.632199
d  2    0.285033
   3   -0.715590
```

DataFrame：

```python
In [41]: frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
    ...:                     index = [['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
    ...:                     columns = [['Ohio', 'Ohio', 'Colorado'],
    ...:                                ['Green', 'Red', 'Green']])

In [42]: frame
Out[42]: 
     Ohio     Colorado
    Green Red    Green
a 1     0   1        2
  2     3   4        5
b 1     6   7        8
  2     9  10       11


In [44]: frame.columns.names = ['state', 'color']
In [46]: frame.index.names = ['key1', 'key2']

In [47]: frame
Out[47]: 
state      Ohio     Colorado
color     Green Red    Green
key1 key2                   
a    1        0   1        2
     2        3   4        5
b    1        6   7        8
     2        9  10       11
```

具有层次化索引的Series可以和DataFrame通过unstack()和stack()相互转化，详解见第七章。

#### 重排分级顺序

swaplevel（）接受两个级别编号或名称，并返回一个互换了级别的新对象。

Sortlevel则接受一个级别编号（类似于轴编号）并对按该级别进行排序。

#### 根据级别汇总统计

通过level选项可以进行汇总，默认axis = 0，可以选择行级别。
axis = 1，可以选择列级别。

#### 从数据中设置index

set_index 可以把某几个标签的数据设置成行级别，而其标签则为这些级别的names。

reset_index正相反。 

##其他话题

#### 整数索引

整数索引容易产生歧义，而且确实会产生错误，如：

```python
In [55]: ser
Out[55]: 
0    0.0
1    1.0
2    2.0
dtype: float64

In [56]: ser[-1]
```
就会产生key_Error错误，若非整数索引就不会产生。

如果需要可靠地，不考虑索引类型的，基于未知的索引，可以使用Series的iget_value(位置),和DataFrame的irow和icol方法。

#### 面板数据 （Panel）

略

## 注意

* DataFrame的索引
data[val]返回的是列，而data.ix[val]返回的是行。

* 非重叠（overlapping）
“飞机”和“轮机”都有“机”，所以重叠， ”高富帅“和”矮穷矬“不重叠。

* ix在未来可能会被移除，可以通过`loc[行，列]`或`iloc[行]`进行索引

* 按行求值，按列求值
按行求值（axis = 1）是指将列合并，按列求值（axis = 0）是指将行合并。         