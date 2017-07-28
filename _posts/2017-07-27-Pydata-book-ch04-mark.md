---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第四章NumPy笔记 
tags: Python
---
Numpy的一些函数和特性简记




* content
{:toc}

## 参数
xx.ndim 数组长度
xx.shape 维度大小
xx.dtype 数据类型

## 创建函数

|函数|说明|
|:--|:--|
|array(xx)|将输入数据xx（列表，元祖，数组或其他序列类型）转换为ndarray。要么推断出dtype，要么显式指定dtype。默认直接复制输入数据|
|asarray|将输入转换为ndarray，如果输入本身就是一个ndarray就不进行复制|
|arange|返回一个有序的ndarray|
|ones，ones_like|根据指定的形状和dtype创建一个全1数组，ones_like以另一个数组为参数，并根据其形状和dtype创建一个全1数组|
|zeros，zeros_like|全0数组|
|empty，empty_like|创建新数组，只分配内存空间但不填充任何值|
|eye，identity|创建一个正方形N×N单位矩阵（对角线为1，其余为0）|

## 数据类型

|类型|类型代码|类型|类型代码|
|:--|:--|:--|:--|
|int8, uint8|i1, u1|int16, uint16| i2, u2|
|int32, uint32|i4, u4|int64, uint 64|i4, u4|
|float16|f2|float32|f4 or f|
|float64|f8 or d|float128|f16 or g|
|complex64, complex128|c8, c16(复数)|complex256|c32|
|bool|?|object(python对象)|O|
|string_|S|unicode_|U|

* 数据类型的转换  
xx.astype(dtype)

## 数组与标量之间的计算

* vectorization(矢量化)
即大小相等的数组之间的任何算术运算都会将运算应用到元素级
* broadcaSsting（不同大小的数组之间的运算)
具体见12章

## 基本的索引和切片

数组切片跟列表切片最重要的区别在于，数组切片是原是数组的视图。这意味着数据不会被复制，视图上的任何修改都会直接反映到源数组上：
```python
In[57]: arr_slice = arr[5:8]
In[58]: arr_slice[1] = 12345
In[59]: arr
Out[59]: array([0, 1, 2, 3, 4, 12, 12345, 12, 8, 9])
In[60]: arr_slice[:] = 64
In[61]: arr
Out[61]: array([0, 1, 2, 3, 4, 64, 64, 64, 8, 9])
```
可以看到，对视图arr_slice的操作反映到了源数组arr上

** 如果要复制切片数组，则需要arr_slice.copy() **

* 高维度数组元素索引

```python
In [2]: arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

In [3]: arr[0][2]
Out[3]: 3

In [4]: arr[0, 2]
Out[4]: 3
#

```

* 高纬度数组切片

```python
In [5]: arr = np.array([[0, 1, 2],[4, 5, 6], [7, 8, 9]])

In [6]: arr[:2]
Out[6]: 
array([[0, 1, 2],
       [4, 5, 6]])

In [7]: arr[:2, 1:]
Out[7]: 
array([[1, 2],
       [5, 6]])
#即索引和切片可以一起去使用，返回的同样是视图
```

* 布尔型索引

```python
In [24]: data[(names == 'bob') | (names == 'bin')]
Out[24]: 
array([[-1.51820833,  0.93871624, -0.82567594, -0.84420024],
       [ 1.32281996,  2.35355066, -0.82956821, -0.23053934]])

In [25]: data = numpy.random.randn(6, 4)

In [26]: names
Out[26]: 
array(['bob', 'jashon', 'richard', 'bin', 'google', 'chill'], 
      dtype='<U7')

In [27]: data[(names == 'bob') | (names == 'bin')]
Out[27]: 
array([[ 0.74605977,  0.35704256,  0.27657912, -0.87375882],
       [ 0.03110279,  0.06189641, -0.07518686, -0.42962777]])

In [28]: data[- ((names == 'bob') | (names == 'bin'))]

Out[28]: 
array([[-0.81259874, -1.63085304,  1.20093301, -0.01940712],
       [ 0.34489263, -1.70169785,  0.10215123, -0.0672559 ],
       [ 1.89754785, -0.19561268,  0.64153751, -0.50974257],
       [-0.10947979,  0.0805697 ,  0.96664486, -1.08070463]])

In [29]: mask = (names == 'bob') | (names == 'bin')

In [30]: mask
Out[30]: array([ True, False, False,  True, False, False], dtype=bool)

```

* 通过布尔型数组设置值

例：将data中的所有复制设置为0

```python
In [31]: data[data < 0] = 0

In [32]: data
Out[32]: 
array([[ 0.74605977,  0.35704256,  0.27657912,  0.        ],
       [ 0.        ,  0.        ,  1.20093301,  0.        ],
       [ 0.34489263,  0.        ,  0.10215123,  0.        ],
       [ 0.03110279,  0.06189641,  0.        ,  0.        ],
       [ 1.89754785,  0.        ,  0.64153751,  0.        ],
       [ 0.        ,  0.0805697 ,  0.96664486,  0.        ]])
```

* 花式索引（Fancy indexing）