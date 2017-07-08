---
layout: post
title:  "Github Pages + Jeckyll 搭建博客过程记录"
author: SiYu Liu
Date: 2017-07-08
categories: jekyll
tags: jekyll
---
* content
{:toc}

    System : Linux mint 18.1
	Tools : Githhub Pages + Jekyll
本文记录了个人搭建博客搭建过程，参考多方资料，详情见文末.

---
1.软件的安装  
参考 jekyll.com.cn，需要安装的环境有
```
   sudo apt-get install Ruby 
   sudo apt-get install RubyGems
   sudo gem install jekyll
   sudo gem install boundle
   sudo apt-get install nodejes
```  







---
2.jekyll使用  
  * 执行`jekyll new myblog`
  * `cd myblog`
  此时，博客就已经安装好了，我们可以看到基本的配置文件以及文件夹都在里面了

----
3.本地部署服务器  
  为了避免初期平凡的修改网页而不停地`git push`，我们可以将博客部署在本地，通过访问`http://localhost:4000/`来在本地预览博客。
  * 参照资料，此处使用了bundle
  * `bundle update`
  * `bundle exec jekyll serve`
  接下来就可以通过访问`http://127.0.0.1:4000/`来访问博客了

---
4.模板  
网上有丰富的模板，以满足对网页设计没有经验的人，比如我.  
本博客采用了HyG的模板，详情参考其README文件
`https://github.com/Gaohaoyang/gaohaoyang.github.io/blob/master/README-zh-cn.md`


---
5.配置
在myblog文件下的**_config.yml**文件，是用来配置博客的,可以参照下载的模板内的REAMDE文件来进行编辑。

---
参考资料：  
1. [Hyq's github](https://github.com/Gaohaoyang/gaohaoyang.github.io/blob/master/README-zh-cn.md)  
2. [Hyq's blog](https://gaohaoyang.github.io/)  
3. [Jason_Yuan - 通过Github Pages和Jekyll搭建个人博客](http://www.jianshu.com/p/3f355c7872d5)   
4. [阮一峰 - 搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)  
5. [jekyll 官网](http://jekyll.com.cn/)

