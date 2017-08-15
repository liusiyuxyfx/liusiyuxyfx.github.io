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

## linux mint 中文字体发虚解决方法

`sudo vim /etc/fonts/conf.d/69-language-selector-zh-cn.conf `
贴入以下代码

```xml
<?xml version="1.0"?>
<fontconfig>

<match target="pattern">
<test qual="any" name="family">
<string>serif</string>
</test>
<edit name="family" mode="prepend" binding="strong">
<string>WenQuanYi Bitmap Song</string>
<string>WenQuanYi Micro Hei</string>
<string>WenQuanYi Zen Hei</string>
<string>AR PL UMing CN</string>
<string>AR PL ShanHeiSun Uni</string>
<string>Bitstream Vera Serif</string>
<string>DejaVu Serif</string>
<string>AR PL UKai CN</string>
<string>AR PL ZenKai Uni</string>
</edit>
</match> 
<match target="pattern">
<test qual="any" name="family">
<string>sans-serif</string>
</test>
<edit name="family" mode="prepend" binding="strong">
<string>Bitstream Vera Sans</string>
<string>DejaVu Sans</string>
<string>WenQuanYi Bitmap Song</string>
<string>WenQuanYi Micro Hei</string>    
<string>WenQuanYi Zen Hei</string>
<string>AR PL UMing CN</string>
<string>AR PL ShanHeiSun Uni</string>
<string>AR PL UKai CN</string>
<string>AR PL ZenKai Uni</string>
</edit>
</match> 
<match target="pattern">
<test qual="any" name="family">
<string>monospace</string>
</test>
<edit name="family" mode="prepend" binding="strong">
<string>Bitstream Vera Sans Mono</string>
<string>DejaVu Sans Mono</string>
<string>WenQuanYi Micro Hei Mono</string>
<string>WenQuanYi Zen Hei Mono</string>
<string>WenQuanYi Bitmap Song</string>
<string>AR PL UMing CN</string>
<string>AR PL ShanHeiSun Uni</string>
<string>AR PL UKai CN</string>
<string>AR PL ZenKai Uni</string>
</edit>
</match> 

</fontconfig>
```

注销重启
