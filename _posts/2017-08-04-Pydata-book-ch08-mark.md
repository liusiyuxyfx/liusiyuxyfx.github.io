---
layout: post
author: SiYu Liu
categories: Python
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

也可以直接传入字符串`'颜色标记线型'`

```python
In [5]: axes[0, 0].plot(rand(30).cumsum(), 'k<--')

In [6]: axes[0, 1].plot(rand(30).cumsum(), color = 'k', linestyle = '--', marker = '<')
```
![pc2](https://raw.githubusercontent.com/liusiyuxyfx/liusiyuxyfx.github.io/master/resources/images/2017-08-04-pydata-book-ch08-mark/pc2.png) 

### 刻度,标签和图例

 pyplot接口的xlim方法控制图标的范围,xticks控制刻度位置, xticklabels控制刻度标签.
 使用方法有如下两种:
 
 * 调用时不带参数, 则返回当前的参数值. 
 * 调用时带参数, 则设置参数值.
 
 所以这些方法都是对当前或最近创建的AxesSubplot起所用的. 它们各自对应subplot对象上的两个方法,如xlim对应ax.get_xlim, ax.set_xlim.
 
#### 设置标题, 轴标签, 刻度以及刻度标签

要修改x轴刻度,可用set_xticks, set_xticklabels,前者可以告诉matplotlib刻度在具体哪些数值位值,后者可以将这些标签更改为任何其他的值(如字符串),最后可用set_title为图标设置一个标题, 用set_xlabel为x轴设置一个名称.

要想修改y轴,只需要将x换为y即可 .

#### 添加图例(legend)

在创建subplot时,传入label属性给其命名, 同过ax.legend(loc = 'bes't') 就可创建图例,loc用来表示图例的位置..

### 注释以及在Subplot上绘图

可以通过text, arrow, annotate等函数添加文本, 箭头, 其他图形等注解,text可在指定坐标(x, y)绘制文本,还可自定义格式.

绘制图形可以先创建图形,再通过ax.add_patch(图形)将其添加到subplot中.

### 将图标保存到文件
利用plt.savefig可以将图标保存到文件,也可以通过figure的实例方法savefig来保存.

可以设置dpi和bbox_inches(可以减除当前图表的空白部分).

savefig也可把图片保存到数组.

#### FIgure.savefig选项

|参数|说明|
|:--|:--|
|fname|含有文件路径的字符串或Python的文件型对象. 图像格式由文件扩展名推断得出|
|dpi|图像分辨率,默认100|
|facecolor, edgecolor|图像的背景色,默认白色|
|format|显式设置文件格式|
|bbox_inches|图表需要保存的部分,如果设置'tight',则将尝试剪除空白部分|

### matplotlib配置

matplotlib/mpl-data中详细写了配置选项.

# pandas中的绘图函数

### 线型图

默认情况下,Series和DataFrame生成的都是线形图.

#### Series.plot方法的参数

|参数|说明|
|:--|:--|
|label|用于图例的标签|
|ax|要在骑上进行绘制的matplotlib Subplot对象.如果没有设置,则使用当前Subplot|
|style|风格字符串|
|alpha|图表的填充不透明度|
|kind|可以是line, bar, barh, kde|
|logy|在y轴上 使用对数标尺|
|use_index|将对象的索引用作刻度标签|
|rot|旋转刻度标签|
|xticks, yticks|x,y轴刻度值|
|xlim, ylim|x,y轴刻度界限|
|grid|显式轴网络线(默认打开)|

#### DataFrame.plot方法

|参数|说明|
|:--|:--|
|Subplots|将各个DataFrame列会知道单独的subplot中|
|sharex, sharey|如果subplos = True, 则公用一个x, y轴|
|figsize|表示图像大小的元祖|
|title|表示图像标题的字符|
|legend|添加一个Subplot图例|
|sort_columns|以字母表顺序绘制各列,默认使用当前列顺序|

### 其他图形

详见:[pandas-visualization](http://pandas.pydata.org/pandas-docs/stable/visualization.html) 