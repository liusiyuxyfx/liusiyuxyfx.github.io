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

### 堆叠 