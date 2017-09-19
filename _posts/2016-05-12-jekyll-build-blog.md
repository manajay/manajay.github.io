---
layout: post
title: Jekyll<一>环境
tag: ruby,Jekyll
date: 2016-05-12 22:57:24 +09:00
---

IT人必须弄个自己的博客
我选择的是基于`Github`上的`github-pages` 之`Jekyll`静态博客.

##   gem 源被墙
使用国外服务器 下载文件经常会遇到这种问题 

```
// 使用 gem sources 查看当前的源
// 更换gem源
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
```
不要淘宝的源<已停止更新>了

* 其他的 [Homebrew](http://manajay.github.io/homebrew-clean-install/)-[Ruby](http://manajay.github.io/mac-ruby-install/)-[翻墙](http://manajay.github.io/2016/06/vpn-google/)可以去我的其他文章查看

##  Gem依赖问题

本人建立的博客使用的是`onevcat`大神的模板,下载模板的时候,会有一个`Gemfile.lock`文件,里面指定了所依赖的`gem`版本号.
如果你是`iOS开发者`,应该知道`Cocoapods`的`podfile.lock`文件了

那么对于博客来说,(在你安装完`bundle` 之后,这些大部分`gem` 其实已经安装完毕,并且默认的版本都是最新的),如果没有需要手动`gem install gem名称`
解决版本问题(软件有版本之分,gem也是有的,不同版本的功能可以有差异),最简单的方式是,删除`Gemfile.lock`,再次`jekyll serve`运行,基本就可以了.


本文参考了下面的部署方法
##  [jekyll 本地部署链接]

[jekyll 本地部署链接]: http://pizida.com/technology/2016/03/03/use-jekyll-create-blog-on-github/

