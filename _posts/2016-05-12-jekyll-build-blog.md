---
layout: post
title: jekyll环境
tag: ruby
date: 2016-05-12 22:57:24.000000000 +09:00
---

# 终端命令好辛苦-ಥ_ಥ
从前天网上一直在整个人博客的事情, 本来很简单的事,可能因为 Mac 电脑系统升级,自带的 ruby 和安装的 gem,jekyll 都是新的版本,中间报各种各样难懂的错误,Google 了两天还是没有解决,真心累啊 

##  1. 没有权限

* 原因1 是 mac 系统 默认将 rmv 安装到了 root 目录,这样你再使用gem 的时候会 要求你提升 访问权限 ,命令前 加 sudo
* 原因2 遇到 /usr/bin 没有权限的问题 ,这个路径要高于 root 权限,即便 sudo 也是没有效果的; 解决办法是 更改 安装路径 /usr/local/bin 就好了 



##  2. gem jekyll 版本错误

本人建立的博客是基于 jekyll 的 ,使用的是 onevcat 的模板,中间安装jekyll 的时候遇到的问题是,jekyll 依赖的 gem 总是缺失, 报错出来 指定了某个版本,当时一点都不懂,就 sudo 强制 安装了 指定的 gem,特别麻烦,最后安装结束 ,执行 bundle exec jekyll serve命令 却又报莫名的错误,好像有两个线程执行jekyll ,让 kill 掉一个.

其实,对于 gem 版本的问题 解决起来特别简单, 模板中 有个 Gemfile.lock文件,里面指定了 jekyll 依赖 gem 的版本号,在这儿更改为 当前系统已安装的 gem 路径就可以了 
(在你安装完 bundle 之后,这些 gem 其实已经安装完毕,并且默认的版本都是最新的)

所以当你 安装 jekyll 结束 以后记得要 经常  bundle update 




##  3. gem 源被墙
使用国外服务器 下载文件经常会遇到这种问题 

使用 gem sources -l 查看当前的源 

更换源为 https://gems.ruby-china.org/

命令为 :   
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/

不使用 淘宝的源<已停止更新>了 


本文参考了下面的部署方法 ,点击链接 进入查看

##  [jekyll 本地部署链接]


[jekyll 本地部署链接]: http://pizida.com/technology/2016/03/03/use-jekyll-create-blog-on-github/

