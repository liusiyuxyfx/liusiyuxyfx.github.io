---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第八章 绘图和可视化
tags: Python
---

* content
{:toc}

第八章笔记,以pylab模式启动ipython(ipyhton --pylab)





# matplotlib API入门

matplotlib API函数都位于matplotlib.pyplot模块中

`import matplotlib.pyplot as plt`

### Figure 和 Subplot

matplotlib的图像都位于Figure对象中,可用plt.figure创建一个新的figure.

#### pyplot.subplots的选项

|参数|说明|
|:--|:--|
|nrows|subplot的行数|
|ncols|subplot的列数|
|sharex|所有subplot应该使用相同的x轴 刻度(调节xlim将会影响所有subplot)|
|sharey|所有subplot应该使用相同的Y轴 刻度(调节ylim将会影响所有subplot)||
|subplot_kw|用于创建各subplot的关键字字典|
|**fig_kw|创建figure时的其他关键字, 如plt.subplots(2, 2, figsize = (8, 6))|

#### 调整subplot周围的间距

利用
`subplots_adjust(left  = None, bottom =None, right = none, top = None, wspace = None, hspace = None)`

wspace和hspace用于控制宽度和高度的百分比,可用作subplot之间的间距.

e.g.:

```python
In [14]: fig, axes = plt.subplots(2, 2, sharex = True, sharey = True)

In [16]: for i in range (2):
    ...:     for j in range(2):
    ...:         axes[i][j].hist(randn(500), bins = 50, color = 'k', alpha = 0.5)
    ...:

In [17]: plt.subplots_adjust(wspace = 0, hspace = 0)
```
![pc1](https://raw.githubusercontent.com/liusiyuxyfx/liusiyuxyfx.github.io/master/resources/images/2017-08-04-pydata-book-ch08-mark/pc1.png) 

### 颜色,标记和线型

可以使用color属性设置颜色,linestyle属性设置线型,marker属性设置标记.

要也可以直接传入字符串`'颜色标记线型'`

```python
In [5]: axes[0, 0].plot(rand(30).cumsum(), 'k<--')

In [6]: axes[0, 1].plot(rand(30).cumsum(), color = 'k', linestyle = '--', marker = '<')
```
![pc2](https://raw.githubusercontent.com/liusiyuxyfx/liusiyuxyfx.github.io/master/resources/images/2017-08-04-pydata-book-ch08-mark/pc2.png) 

