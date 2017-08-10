---
layout: post
author: SiYu Liu
categories: Python
title: 《利用python进行数据分析》第十二章 numpy的高级应用
tags: Python
---

* content
{:toc}

Numpy的高级应用






# 高级数组操作

### 数组重塑

通过reshape函数传入一个表示新形状的元组即实现数据重塑.

作为参数的形状的其中一位可以使-1, 他表示维度的大小由数据本身推断而来

```python
In [4]: arr = np.arange(15)

In [5]: arr.reshape((5, -1))
Out[5]: 
array([[ 0,  1,  2],
       [ 3,  4,  5],
       [ 6,  7,  8],
       [ 9, 10, 11],
       [12, 13, 14]])
```

数组的shape属性也可以被传入reshape

与reshape将一位数组转换为多维数组的运算过程相反的运算通常成为扁平化(flattening)或散开(raveling)

```python
array([[ 0,  1,  2],
       [ 3,  4,  5],
       [ 6,  7,  8],
       [ 9, 10, 11],
       [12, 13, 14]])

In [8]: arr.ravel()
Out[8]: array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])
```

如果没有必要,ravel不会产生源数据的副本, 而faltten方法与ravel方法效果一样,不过它总是返回数据的副本.


### C和Fortran顺序

NumPy数组是按行优先顺序创建的. 在空间方面, 这就意味着, 对于一个二维数组, 每行的数据项是被存放在相邻内存位置上的.
另一种顺序是按列优先顺序, 它意味着每列中的数据项是被存放在相邻的内存位置上的.

行和列优先顺序被分别成为C和Fortran顺序..

像reshape和reval这样的函数,可以接受一个表示数组数据存放顺序的order参数,可以是'C'或'F'.

* c/行优先顺序:先经过更高的维度(轴1会先于轴0被处理)
* Fortran/列优先顺序:后经过更高的维度(轴0会先于轴1)

###　数组的合并和拆分

numpy.concatenate可以指定轴讲一个由数组组成的序列(如元祖, 列表等)连接到一起.

split可以将一个数组沿指定轴分为多个数组

```python
In [11]: arr
Out[11]: 
array([[-0.98993953,  0.4958673 ],
       [-0.62946866, -0.92446272],
       [ 0.12772694, -0.40620568],
       [-0.69696886,  0.38629111],
       [ 0.55425885, -0.51186329]])

In [12]: first, second, third = np.split(arr, [1, 3])

In [14]: first
Out[14]: array([[-0.98993953,  0.4958673 ]])

In [15]: second
Out[15]: 
array([[-0.62946866, -0.92446272],
       [ 0.12772694, -0.40620568]])

In [16]: third
Out[16]: 
array([[-0.69696886,  0.38629111],
       [ 0.55425885, -0.51186329]])
```

#### 数组连接函数

![s1](https://raw.githubusercontent.com/liusiyuxyfx/liusiyuxyfx.github.io/master/resources/images/2017-08-09-pydata-book-ch12-mark/Selection_006.png) 

#### 堆叠辅助类：r_和c_

NumPy命名空间中有两个特殊对象--r_, c_。他们可以使数组的堆叠更加简单

### 元素的重复操作：title和repeat

运用repeat和tile函数，可以将数组中的各个元素重复一定次数

```python
In [5]: arr
Out[5]:
array([[-0.69406457,  1.15405242],
       [ 0.07380656,  1.9142753 ]])

In [6]: arr.repeat(2, axis = 0)
Out[6]:
array([[-0.69406457,  1.15405242],
       [-0.69406457,  1.15405242],
       [ 0.07380656,  1.9142753 ],
       [ 0.07380656,  1.9142753 ]])
```

如果没有设置轴向，则数组会被扁平化

tile的功能是沿制定轴向堆叠数组的副本。

```python
In [7]: arr
Out[7]:
array([[-0.69406457,  1.15405242],
       [ 0.07380656,  1.9142753 ]])

In [9]: np.tile(arr, 2)
Out[9]:
array([[-0.69406457,  1.15405242, -0.69406457,  1.15405242],
       [ 0.07380656,  1.9142753 ,  0.07380656,  1.9142753 ]])

In [10]: np.tile(arr, (2, 1))
Out[10]:
array([[-0.69406457,  1.15405242],
       [ 0.07380656,  1.9142753 ],
       [-0.69406457,  1.15405242],
       [ 0.07380656,  1.9142753 ]])
``` 

### 花式索引的等价函数：take和put

take用于获取单个轴向上的选区，put用于设置

```python
In [12]: indx = [7,1, 2, 6]

In [16]: arr
Out[16]: array([  0, 100, 200, 300, 400, 500, 600, 700, 800, 900])

In [17]: arr.take(indx)
Out[17]: array([700, 100, 200, 600])

In [18]: arr.put(indx, 42)

In [19]: arr
Out[19]: array([  0,  42,  42, 300, 400, 500,  42,  42, 800, 900])
```

take对其他轴只要传入axis即可

put不接受axis，它只在数组的扁平化版本上进行索引。

# 广播

* 数组与标量值运算，会将所有数组元素与标量值运算
	应用，e.g.可减去列平均值的方式对数组的每一列进行距平化处理
   
### 广播的原则

__如果两个数组的后缘维度（即从末尾开始算起的维度）的轴长度相符或其中一方的长度为1，则认为他们是广播兼容的。广播会在缺失和（或）长度为1的维度上进行__

### 沿其他轴向广播

对于三维，在三维中任何一位上广播就是将数据重塑为兼容的形状。

__根据广播的原则，较小的广播维必须为1,因此，有了一个普遍的方法，即专门为广播而添加一个长度为1的新轴。__Numpy提供了一种通过索引机制插入周的特殊语法。即用np.newaxis属性及“全”切片来插入新轴

```python
In [2]: arr = np.zeros((4, 4))

In [3]: arr_3d = arr[:, np.newaxis, :]

In [4]: arr_3d.shape
Out[4]: (4, 1, 4)

In [5]: arr_1d = np.random.normal(size = 3)

In [6]: arr_1d[:, np.newaxis]
Out[6]:
array([[-0.01742007],
       [-0.08216411],
       [-0.57801927]])

In [7]: arr_1d[np.newaxis, :]
Out[7]: array([[-0.01742007, -0.08216411, -0.57801927]])

In [8]: arr_1d
Out[8]: array([-0.01742007, -0.08216411, -0.57801927])

In [9]: arr_1d[:,np.newaxis].shape
Out[9]: (3, 1)

In [10]: arr_1d[np.newaxis, :].shape
Out[10]: (1, 3)
```

e.g: 对三维数组的轴二距平化

```python
In [11]: arr = np.random.randn(3, 4, 5)

In [12]: depth_means = arr.mean(2)

In [13]: depth_means
Out[13]:
array([[-0.00637177, -0.03586819,  1.37198105, -0.27245499],
       [ 1.12252824,  0.12668646, -0.10432869, -0.74985578],
       [-0.14900454, -0.67811276,  0.27229968,  0.34820621]])

In [15]: demeaned  = arr - depth_means[:, :, np.newaxis]

In [16]: demeaned.mean(2)
Out[16]:
array([[  2.08166817e-17,   0.00000000e+00,   8.88178420e-17,
          6.66133815e-17],
       [ -8.88178420e-17,   0.00000000e+00,   0.00000000e+00,
          1.11022302e-17],
       [  0.00000000e+00,  -6.66133815e-17,   4.44089210e-17,
          3.33066907e-17]])
```

#### 距平化的通用方法

```python

def demean_axis(arr, axis = 0):
	means = arr.mean(axis)
    indexer = [slice(None)]*arr.ndim
    indexer[axis] = np.newaxis
    return arr - mean[indexer]
```

### 通过广播设置数组的值

通过索引机制即可设置。

# ufunc高级应用

### ufunc实例方法

NumPy的各个二元ufunc都有一些用于执行特定矢量化运算的特殊方法。

reduce接受一个数组参数，并通过一系列的二元运算对其值进行聚合（可制定轴向）

e.g.用np.add.reduce对数组哥哥元素进行求和

```python
In [17]: arr = np.arange(10)

In [18]: np.add.reduce(arr)
Out[18]: 45

In [21]: arr.sum()
Out[21]: 45
```

#### ufunc的方法

|方法|说明|
|:--|:--|
|reduce（x）|通过连续执行原始运算的方式对值进行聚合|
|accumulate（x）|聚合值，保留所有局部聚合结果|
|reduceat（x， bins）|“局部”约简（groupby）。约简数据的各个切片以产生聚合型数组|
|outer（x， y）|对x和y中的每对元素应用原始运算。结果数组的形状为x.shape+y.shape|

### 自定义ufunc

numpy.frompyfunc接受一个pyhton函数以及两个分别表示输入输出参数数量的整数

e.g元素及加法函数

```python
In [22]: def add_elements(x, y):
    ...:     return x + y

In [23]: add_them = np.frompyfunc(add_elements, 2, 1)

In [25]: add_them (np.arange(8), np.arange(8))
Out[25]: array([0, 2, 4, 6, 8, 10, 12, 14], dtype=object)
```

另一个方法numpy.vectorize。没有frompyfunc强大，但在类型推断方面更智能。

# 结构化和记录式数组

结构化数组是一种特殊的ndarray，其中的各个元素都可以被看做c语言中的结构体

```python
In [26]: dtype = [('x', np.float64), ('y', np.int32)]

In [28]: sarr = np.array([(1.5, 6), (np.pi, -2)], dtype = dtype)

In [29]: sarr
Out[29]:
array([( 1.5       ,  6), ( 3.14159265, -2)],
      dtype=[('x', '<f8'), ('y', '<i4')])
```

最典型的办法是元祖列表定义结构化dtype，各元祖格式为(field_name, field_data_type).

### 嵌套dtype和多维字段

定义结构化dtype时，可以设置一个形状

#NumPy的matrix类

索引形式：单行或列会议二维形式返回，且使用星号（*）的惩罚直接就是矩阵乘法
