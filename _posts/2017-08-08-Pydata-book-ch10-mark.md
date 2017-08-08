---
layout: post
author: SiYu Liu
categories: Python
title: 《利用python进行数据分析》第十章 时间序列
tags: Python
---

* content
{:toc}

第十章时间序列笔记





# 日期和时间数据类型及工具

主要用到datetime， time， calendar模块

datetime模块中的数据类型

|类型|说明|
|:--|:--|
|date|一公历形式存储日期（年，月，日）|
|time|将时间存储为（时，分，秒，毫秒）|
|datetime|存储日期和时间|
|timedelta|表示两个datetime值之间的差（日，秒，毫秒）|

### 字符串和datetime的相互转换

利用str或strftime方法（传入一个格式化字符串），datatime对象和pandas的Timestamp对象可以被格式化为字符串。

```python
In [17]: stamp = datetime(2011, 1, 3)               In [18]: str(stamp) 
Out[18]: '2011-01-03 00:00:00' 

In [19]: stamp.strftime('%y-%m-%d')                  Out[19]: '11-01-03'
```

datetime.strptime也可以用这些格式化编码将字符串转换为日期:

```python
In [23]: value = '2001-01-03'

In [24]: datetime.strptime(value , '%Y-%m-%d')
Out[24]: datetime.datetime(2001, 1, 3, 0, 0)

In [25]: datestrs = ['7/6/2001', '8/6/2001']
In [27]: [datetime.strptime(x, '%m/%d/%Y') for x in datestrs]
Out[27]: [datetime.datetime(2001, 7, 6, 0, 0), datetime.datetime(2001, 8, 6, 0, 0)]
```
对于一些常见日期格式，可以用dateutil包中的parser.parse方法更方便的处理

```python
In [30]: from dateutil.parser import parse

In [31]: parse('2011-01-03')
Out[31]: datetime.datetime(2011, 1, 3, 0, 0)

In [32]: parse('Jan 31, 1997 10:45 PM')
Out[32]: datetime.datetime(1997, 1, 31, 22, 45)

In [33]: parse('6/11/2011', dayfirst = True)
Out[33]: datetime.datetime(2011, 11, 6, 0, 0)
#对于国际通用格式中日出现在月之前的情况，可用dayfirst = True解决
```

pandas的to_datetime可以解析多种不同的日期形式

#### datetime格式定义

|代码|说明|
|:--|:--|
|%Y|4位数年|
|%y|2位数年|
|%m|2位数月[01, 12]|
|%d|2位数日[01, 31]|
|%H|24小时制时|
|%l|12小时制时|
|%M|2位数的分[00, 59]|
|%S|秒[00, 61]|
|%w|用整数表示的星期几|
|%U|每年的第几周[00, 53], 周日是第一天，每第一个星期天之前的那几天是第0周|
|%W|同%U，但周一被认为是第一天|
|%z|以+HHMM或-HHMM表示UTC时区偏移量|
|%F|%Y-%m-%d简写|
|%D|%m/%d/%y简写|

# 时间序列基础

### 以时间戳为索引的Series

可用datatime作为Series的索引，不同索引的时间序列之间的算术运算会自动按日期对齐

### 索引， 选取， 子集构造

TimeSeries的方法与Series 相同，也可由datetime进行

### 带有重复索引的时间序列

对非唯一时间戳的数据进行聚合。一个办法是使用groupby，并传入level=0（索引的唯一一层）

# 日期的范围，频率以及移动

resample可将时间序列转换为一个具有固定频率的时间序列

### 生成日期范围

pandas.date_range可用于生成指定长度的DatetimeIndex

```python
In [50]: index = pd.date_range('4/1/2012', '6/1/2012')

In [51]: index
Out[51]:
DatetimeIndex(['2012-04-01', '2012-04-02', '2012-04-03', '2012-04-04', ... ,'2012-06-01'],dtype='datetime64[ns]', freq='D')
```

默认情况，data_range会产生按天计算的时间点。如果只传入起始或结束日期，那就还得传入一个表示一段时间的数字。

```python
In [52]: pd.date_range(start = '4/1/2012', periods = 20)
Out[52]:
DatetimeIndex(['2012-04-01', ... , '2012-04-18', '2012-04-19', '2012-04-20'],
              dtype='datetime64[ns]', freq='D')

In [56]: pd.date_range(end = '6/1/2012', periods = 20)
Out[56]:
DatetimeIndex(['2012-05-13', ..., '2012-05-31', '2012-06-01'],
              dtype='datetime64[ns]', freq='D')
```

freq可以设定频率

date_range默认会保留起始和结束日期的时间戳信息，

normalize = True可以产生一组被规范化到午夜的时间戳

### 频率和日期偏移量

pandas中的频率是由一个基础频率和一个乘数组成的.基础频率通常以一个字符串别名表示. 对于每个基础频率, 都有一个被称为日期偏移量的对象与之对应,例如按小时计算的频率可以用Hour类对应

利用`freq='4H', freq = '3M'`这样的属性即可定义整数倍频率

大部分频率都可通过加法进行连接

```python
In [59]: from pandas.tseries.offsets import Hour, Minute

In [60]: Hour(2) + Minute(30)
Out[60]: <150 * Minutes>
```
也可以直接传入频率字符`freq = '2h30min'

#### 时间序列的基础频率

![时间序列的基础频率](https://raw.githubusercontent.com/liusiyuxyfx/liusiyuxyfx.github.io/master/resources/images/2017-08-08-pydata-book-ch10-mark/s1.png)

### 移动（超前和滞后）数据

移动（shifting）指的是沿着时间轴将数据前移或后移。Series和DataFrame都有一个shift方法用于执行单纯的迁移或后移操作，保持索引不变。

