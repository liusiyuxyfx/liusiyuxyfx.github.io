---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第六章数据加载,存储与文件格式
tags: Python
---

* content
{:toc}

记录了第六章学习笔记。






## 读写文本格式的数据

pandas提供了一些用于表格行数据读取为DataFrame对象的函数。

#### pandas中的解析函数

|函数|说明|
|--|--|
|read_csv|从文件,URL,文件型对象中加载带分隔符的数据。默认分隔符为逗号|
|read_table|从文件，URL，文件对象中加载带分隔符的数据。默认为制表符|
|read_fwf|读取定宽列格式数据（也就是说，没有分隔符）|
|read_clipboard|读取剪贴板中的数据。|

类型推断(type inference)-你不需要指定列的类型到底是数值还是整数，字符串等。日期和其他自定义类型的处理需要多花点时间。

#### red_csv/read_table函数的参数

|参数|说明|
|--|--|
|path|表示文件系统位置，URL，文件型对象的字符串|
|sep或delimiter|用于对各字段进行拆分的字符序列或正则表达式|
|header|用作列名的行号。默认为0（第一行），如果没有header就应设置为None|
|index_col|用作行索引的列编号或列名。可以是单个名称/数字或由多个名称/数字组成的列表（层次化索引）|
|names|用于结果的列名列表，结合header = None|
|skiprows|需要忽略的行数（从文件开始处算起），或需要跳过的行号列表（从0开始）|
|na_values|一组用于替换NA的值|
|comment|用于将注释信息从行尾拆分出去的字符（一个或多个）|
|parse_dates |尝试将数据解析为日期，，默认False。如果为True，则尝试解析所有列。此外，还可以指定需要解析的一组列表或列名。如果列表的元素为列表或元祖，就会将多个列组合到一起再进行日期解析工作（例如，日期/时间分别位于两个列中）|
|keep_date_col|如果链接多列解析日期，则保持参与连接的列。默认为False。|
|converters|由列号/列名跟函数之间的映射关系组成的字典|
|dayfirst|当解析有歧义的日期时，将其看做国际格式|
|date_parser|用于解析日期的函数|
|nrows|需要读取的行数|
|iterator|返回一个TextParser以便逐块读取文件|
|chunksize|文件块的大小（用于迭代）|
|skip_footer|需要忽略的行数（从文件末尾处算起）|
|verbose|打印各种解析器输出信息，比如“非数值列中缺失值的数量”等|
|encoding|用于unicode的文本编码格式|
|squeeze|如果数据经解析后仅含一列，返回Series|
|thousands|千分位分隔符，如“，”或“."|

#### 逐块读取文本文件

 可以通过nrows指定只读取几行。
 
 可以通过chunksize设定区块大小并对文件逐块迭代。返回一个TextParser。
 TextParser还有一个get_chunk方法，它使你可以读取任意大小的块。
 
#### 将数据写出到文本格式

数据可以被输出为分隔符格式的文件
to_csv, from_csv

#### 手工处理分隔

若csv文件中含有畸形的文件，则易使read_table 出毛病。

对于任何单字符分隔符文件，可以通过Python内置的csv模块将已打开的文件或文件型的对象传给csv.reader。

```python
import csv
f = open('path')
reader = csv.reader(f)
```

对这个reader进行迭代将会为每行产生一个元祖（并移除了所有的引号）

```python
In [49]: lines = list(csv.reader(open('ch06/ex7.csv')))

In [50]: header, values = lines[0], lines[1:]

In [51]: header
Out[51]: ['a', 'b', 'c']

In [52]: values
Out[52]: [['1', '2', '3'], ['1', '2', '3', '4']]


In [55]: data_dict = {h: v for h, v in zip(header, zip(*values))}
#一个*代表接受的是一个元祖，两个*代表接受的是一个字典
In [56]: data_dict
Out[56]: {'a': ('1', '1'), 'b': ('2', '2'), 'c': ('3', '3')}
```

CSV语支选项

|参数|说明|
|--|--|
|delimiter|用于分隔字段的单字符。默认为逗号|
|lineterminator|用于写操作的行结束符，默认为”\r\n"。读操作将忽略此选项，它能认出跨平台的行结束符|
|quotechar|用于带由特殊字符（如分隔符）的字段的引用符号。默认为“ " ”|
|quoting|引用约定|
|skipinitialspace|忽略分隔符后免得空白符。默认为False|
|doublequote|如何处理字段内的引用符号。如为True，则双写|
|escapechar|用于对分隔符进行转义的字符串|

#### Json数据
 
通过json.load可以加载json文件，json.dumps将python对象转换为json格式。

#### XML和HTML：web信息收集

用lxml.html处理html，再用lxml.objectify做一些xml处理。

用urllib2打开数据的URL

```python
from  lxml.html import pasrse
from urllib2 import urlopen
#python新版本中，urllib2已经不能使用，而是用urllib.request
In [61]: parsed = parse(urlopen('http://finance.yahoo.com/q/op?s=AAPL+Options'))

In [62]: doc = parsed.getroot()

In [63]: links = doc.findall('.//a')
```
通过个对象的get方法和text_content方法可以获得内容。

