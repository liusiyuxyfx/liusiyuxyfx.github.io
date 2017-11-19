---
layout: post
author: SiYu Liu
categories:JavaEE
title: JavaEE课程设计问题记录
tags: JavaEE
---

* content
{:toc}

本文记录JavaEE课程设计过程中遇到的问题






##＃Ubuntu下使用jdbc的问题


* **报错:**java.lang.ClassNotFoundException: com.mysql.jdbc.Driver

* **解决方法:**将mysql jar包放到Tomcat目录下的lib文件中

* **原因:**在web项目中，当Class.forName("om.mysql.jdbc.Driver");时myeclipse是不会去查找字符串，不会去查找驱动的。

----