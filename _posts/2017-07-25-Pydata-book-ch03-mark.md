---
layout: post
author: SiYu Liu
categories: Pyhton
title: 《利用python进行数据分析》第三章Ipython笔记 
tags: Python
---
Ipython的功能及快捷键简记。




     
## 常用键盘快捷键  

|命令|说明|
|:--|:--|
|Ctrl-P|上一条输入的命令|
|Ctrl-N|下一条输入的命令|
|Ctrl-R|按行读取的反向历史搜索|
|Ctrl-C|终止当前执行的代码|
|Ctrl-A|移动到行首|
|Ctrl-E|移动到行末|
|Ctrl-K|删除至行末|
|Ctrl-U|删除至行首|
|Ctrl-F|向前移动一个字符|
|Ctrl-b|向后移动一个字符|
|Ctrl-L|清屏|

## 常魔术命令  

|命令|说明|
|:--|:--|
|%quickref|显示IPython的快速参考|
|%magic|显示所有魔术命令的详细文档|
|%debug|从最新的异常跟踪的底部进入交互式调试器|
|%hist|打印命令的输入历史|
|%pbd|在异常发生后自动进入调试器|
|%paste|执行剪贴板中的Python代码|
|%cpaste|打开一个特殊提示符一遍手工粘贴执行的Python代码|
|%reset|删除interactive命名空间中的全部变量/名称|
|%page OBJECT|通过分页器打印出OBJECT|
|%run xx.py|在Ipython中执行一个Python脚本文件|
|%prun statement|通过cProfile执行statement，并打印分析器的输出结果|
|%time statement|报告statement的执行时间|
|%timeit statement|多只执行以计算系综平均执行时间|
|%who %who_is %whos|显示interactive命名空间中的定义的变量|
|%xdel variable| 删除variable，并尝试清除其在Ipython中的对象上的一切引用|

## 与操作系统交互  

|命令|说明|
|:--|:--|
|!cmd|在系统shell中执行cmd|
|output = ！cmd args|执行cmd，并将stdout存放在output中|
|%alias *alias_name cmd*|为系统shell命令定义别名|
|%bookmark|使用Ipython的目录书签系统|
|%cd *directory*|将系统工作目录更改|
|%pwd|返回当前工作目录|
|%pushd *directory*|将当前目录入栈，并转向目标目录|
|%popd|弹出栈顶目录，并转向该目录|
|%dirs|返回一个含有当前目录栈的列表|
|%dhist|打印目录访问历史|
|%env|以dict形式返回系统环境变量|


## 绘图方面
* 基于Qt的富GUI控制台
`ipython qtconsole --pylab=inline`  
* matplotlib集成与pylab模式
执行`ipython --pylab`即可集成matplotlib

