---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第九章 数据聚合与分组运算
tags: Python
---

* content
{:toc}

第九章笔记





# GroupBy技术

### 通过groupby函数进行分组.

```python
In [5]: df
Out[5]: 
      data1     data2 key1 key2
0 -0.555336  0.042416    a  one
1  0.958967 -2.043175    a  two
2 -1.117306  0.459813    b  one
3 -0.088721 -0.241567    b  two
4  0.866751 -0.980636    a  one

In [6]: grouped = df['data1'].groupby(df['key1'])

In [7]: grouped
Out[7]: <pandas.core.groupby.SeriesGroupBy object at 0x7f0fabd2dba8>

In [8]: grouped.mean()
Out[8]: 
key1
a    0.423461
b   -0.603014
Name: data1, dtype: float64

In [9]: means = df['data1'].groupby([df['key1'], df['key2']]).mean()

In [10]: means
Out[10]: 
key1  key2
a     one     0.155708
      two     0.958967
b     one    -1.117306
      two    -0.088721
```

groupby的size方法可以返回一个含有分组大小的Serires

### 对分组进行迭代

GroupBy对象支持迭代

```python
In [12]: for k, group in df.groupby('key1'):
    ...:     print(k)
    ...:     print(group)
    ...:     
a
      data1     data2 key1 key2
0 -0.555336  0.042416    a  one
1  0.958967 -2.043175    a  two
4  0.866751 -0.980636    a  one
b
      data1     data2 key1 key2
2 -1.117306  0.459813    b  one
3 -0.088721 -0.241567    b  two
```
对于多重键,元祖的第一个元素是由键值组成的元祖:

```python
In [13]: for (k1, k2), group in df.groupby(['key1', 'key2']):
    ...:     print(k1, k2)
    ...:     
a one
a two
b one
b two
```

### 选取一个或一组列

对于由DataFrame产生的GroupBy对象,如果用一个或一组列名对其进行索引,就能实现选取部分列进行聚合的目的.

```python
df.groupby('key1')['data1'] 是 df['data1'].groupby(df['key1'])的语法糖
df.groupby('key2')[['data2']]是df[['data2']].groupby(df['key1'])的语法糖
```

### 通过字典或Series进行分组

可通过字典对列或行进行再分组

```python
In [19]: mp1={'a':'red', 'b': 'red', 'c':'blue', 'd':'blue', 'e':'red'}

In [20]: mp2={'Joe':'student', 'Steve':'Teacher','Wes':'student','Jim':'Teacher', 'Tr
    ...: avis':'student'}

In [21]: people
Out[21]: 
               a         b         c         d         e
Joe    -0.011533  1.187409  0.784298  1.554540 -1.127226
Steve  -0.369398 -0.723837 -0.962265 -1.644787  1.057001
Wes    -1.873815       NaN       NaN  0.258940 -0.657095
Jim     0.701703 -0.088170 -2.570053 -0.000445 -1.210623
Travis  1.106946 -0.464509 -0.533230  1.033774  0.488791

In [24]: people.groupby(mp1, axis = 1).sum()
Out[24]: 
            blue       red
Joe     2.338839  0.048650
Steve  -2.607052 -0.036235
Wes     0.258940 -2.530911
Jim    -2.570499 -0.597090
Travis  0.500544  1.131229

In [25]: people.groupby(mp2).sum()
Out[25]: 
                a         b         c         d         e
Teacher  0.332305 -0.812007 -3.532318 -1.645233 -0.153622
student -0.778402  0.722900  0.251068  2.847254 -1.295529
```

Series也有同样的功能, 它可以被看做一个固定大小的映射.如果Series作为粉组件,则pandas会检查Series以确保其索引跟分组轴是对齐的.

### 通过函数进行分组

可以根据python的函数进行分组,如想要根据字符串的长度进行分组,只要传入len即可
`people.groupby(len)`

甚至函数,字典,列表,数组,Series混用都可以

### 根据索引级别分组

通过level关键字传入级别便获或名称,axis指定轴

# 数据聚合

GroupBy有已经定义好的聚合方法,也可以自己写聚合函数,通过`group.agg(函数)`方法调用

### 面向列的多函数应用

agg函数可传入一组用(name,function)元祖组成的列表,也可以直接传入function,前者会以name命名列,后者则以function命名

```python
In [56]: group.agg([('foo', 'mean') , ('bar', np.std), 'sum', 'max'])
Out[56]: 
              total_bill                                 tip            \
                     foo       bar      sum    max       foo       bar   
sex    smoker                                                            
Female No      18.105185  7.286455   977.68  35.83  2.773519  1.128425   
       Yes     17.977879  9.189751   593.27  44.30  2.931515  1.219916   
Male   No      19.791237  8.726566  1919.75  48.33  3.113402  1.489559   
       Yes     22.284500  9.911845  1337.07  50.81  3.051167  1.500120  
```

# 分组级运算和转换

### transform

transform(founction)会将function广播出去

### apply

apply会将待处理的对象拆分成多个片段,然后对个片段调用传入的函数,最后尝试将各片段组合到一起.

# 透视表和交叉表

可通过pivot_table函数对行列进行聚合

### pivot_table的参数

![ ](https://raw.githubusercontent.com/liusiyuxyfx/liusiyuxyfx.github.io/master/resources/images/2017-08-05-pydata-book-ch09-mark/S1.png  "s1")

### 交叉表crosstab

crosstable是一种用于计算分组频率的特殊透视表.