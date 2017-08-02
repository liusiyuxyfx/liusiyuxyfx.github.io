---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第七章数据规整化：清理，转换，合并，重塑
tags: Python
---

* content
{:toc}

第七章笔记





## 合并数据集

* pandas.merge可根据一个或多个键将不同DataFrame中的行连接起来。

*  pandas.concat可以沿着一条轴将多个对象堆叠到一起

* 实例方法combine_first可以将重复数据编接在一起，用一个对象中的值填充另一个对象中的缺失值。

### 数据库风格的DataFrame合并

数据集的合并（merge）或连接（join）运算是通过一个或多个键连接起来的。

merge中通过on指定键值，结果中的键是交集，其他不重叠的键将被去除。设置how = “outer”外链接可以取并集，how中的left和right则可以取左边或右边DataFrame的全集。

多对多链接产生的是行的笛卡尔积

#### merge函数的参数

|参数|说明|
|--|--|
|left_on|参与合并的左侧DataFrame|
|right_on|参与合并的右侧DataFrame|
|left_index|将左侧的行索引引用作其连接键|
|right_index|类似于left_index|
|sort|根据连接键对合并后的数据进行排序，默认为True，有时在处理大数据集时，禁用该选项可获得更好的性能|
|suffixes|字符串元祖，用于追加到重叠列名的末尾，默认为('_x','_y')。例如，如果左右两个DataFrame对象都有“data”，则结果中就会出现“data_x", "data_y"|
|copy|设置为False，可以再某些特殊情况下避免将数据复制到结果数据结构中，默认总是复制|

#### 索引上的合并

对于层次化索引的合并

```python
In [20]: righth
Out[20]: 
             event1  event2
Nevada 2001       0       1
       2000       2       3
Ohio   2000       4       5
       2000       6       7
       2001       8       9
       2002      10      11

In [21]: lefth
Out[21]: 
   data    key1  key2
0   0.0    Ohio  2000
1   1.0    Ohio  2001
2   2.0    Ohio  2002
3   3.0  Nevada  2001
4   4.0  Nevada  2002

In [22]: pd.merge(lefth,righth,left_on = ['key1','key2'],right_index = True)
Out[22]: 
   data    key1  key2  event1  event2
0   0.0    Ohio  2000       4       5
0   0.0    Ohio  2000       6       7
1   1.0    Ohio  2001       8       9
2   2.0    Ohio  2002      10      11
3   3.0  Nevada  2001       0       1

#可见需用left_on或right_on指定连接键
```

#### 轴向连接



