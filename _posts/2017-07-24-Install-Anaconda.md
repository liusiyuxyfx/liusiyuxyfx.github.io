---
layout: post
author: SiYu Liu
categories: Pyhton
title: Linux下利用Anaconda安装科学计算环境
tags: Python Anaconda
---

&nbsp;&nbsp;&nbsp;最近在学《利用python进行数据分析》，书上epd进行讲解，但是epd已经升级为cnopy，而cnopy官网下载速度实在是太慢了，在此情况下，我找到了一个很好用的东西-**Anaconda**。
&nbsp;&nbsp;&nbsp;Anaconda集成了一系列python下科学计算的包，非常易于安装和管理，同时，anaconda支持最新版本的python，这里主要记录一下Anaconda的使用及一些包的安装过程。

**本文参考：http://www.jianshu.com/p/2f3be7781451**  

-----









###安装Anaconda
&nbsp;&nbsp;&nbsp;在官网下载.sh文件后，通过bash执行安装程序  
`bash Anaconda3-4.4.0-Linux-x86_64.sh`  
一路Enter即可完成安装

###conda的使用
 * 设置国内镜像
conda 的服务器在国外，但是清华TUNA镜像有Anaconda的仓库，所以第一件事是修改conda镜像：
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
#添加tuna镜像
conda config --set show_channel_urls yes
#设置搜索时显示通道地址
```

* 环境管理
如有多个不同python版本，我们可以通过该功能自由切换  
以python3.6为例
```
conda create --name python36 python=3.6  
#创建环境  
source activate python36  
#将默认环境修改为新创建的环境  
source deactivate python36  
#返回默认环境  
conda remove --name python36 -all  
#删除已有环境  
```
* 包管理
```
conda list
#查看当前环境下已安装的包
conda update <package>
#更新
conda install <package>
#安装
conda remove <package>
#删除
```
