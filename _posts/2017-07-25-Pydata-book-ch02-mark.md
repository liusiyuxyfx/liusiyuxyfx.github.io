---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第二章笔记 
tags: Python Anaconda
---

* plot绘制的图片x的存储  
```pyhton
x.figure.savefig('xxx.png')
```
* plot图片显示的方法**
```python
import matplotlib.pyplot as plt
plt.show()#即会弹出小窗口显示图片
```
* 关于书上说的一定要用pylab模式才能打开图片
在Ipyhton shell中输入`%pylab`即可，然而我没有输入，通过plt.show()也可以打开。
经过查询后，发现pylab是一个包，侧重ipython交互实验,用了它以后就相当于自动导入了以下库
```python
import numpy
import matplotlib
from matplotlib import pylab, mlab, pyplot
np = numpy
plt = pyplot
from IPython.display import display
from IPython.core.pylabtools import figsize, getfigs
from pylab import *
from numpy import *
```
