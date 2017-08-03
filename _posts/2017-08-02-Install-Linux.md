---
layout: post
author: SiYu Liu
categories: Linux
title: 安装Linux后的一些配置
tags: :Linux
---

* content
{:toc}

记录一些Linux的个人配置






## 键盘映射-Esc,Ctrl,Caps_Lock三键对调

利用xmodmap

```
vim ~/.Xmodmap
```

粘贴如下内容

```
remove Lock = Caps_Lock
remove Control = Control_L
keycode 9 = Caps_Lock NoSymbol Caps_Lock
keycode 66 = Control_L NoSymbol Control_L
keycode  37 = Escape NoSymbol Escape
add Lock = Caps_Lock
add Control = Control_L
```

立即生效

```
$ xmodmap ~/.Xmodmap
```