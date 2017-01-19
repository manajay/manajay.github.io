---
layout: post
title: 正确安装Mac系统下的Ruby
tag: Ruby,系统依赖
date: 2017-01-19 11:09:25.000000000 +09:00
---

推荐大家在做这些事情的时候最好配置VPN,被墙的感觉简直糟透了
参考我的文章[实现网络自由, 随意Google,看 YouTube 视频](http://manajay.github.io/2016/06/vpn-google/)

## 1. RVM 安装 ruby
mac安装使用Ruby 最安全方便的方式就是使用RVM

## 1.2 安装RVM

### 1.2.1 官方推荐安装RVM方式
不过我尝试安装的时候 官方推荐的方式每次都失败,

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1435355-181ace7352d095d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
// 1.
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

//问题是 Mac 没有自带 `gpg` 所以每次都失败
//之后曾经安装了 `gpg` 然而还是发现显示没有`rvm`资源
//更换服务器 
Note: hkp://keys.gnupg.net can sometimes timeout you may want to use different keyserver - hkp://pgp.mit.edu
//还是没用

// 尝试 gpg的解决办法也失败
curl -sSL https://rvm.io/mpapis.asc | gpg --import -

// 2. 所以这个第二步一直没用上
\curl -sSL https://get.rvm.io | bash -s stable
```
[官方的参考文章](http://www.cnblogs.com/higkoo/articles/deploy_ruby_by_rvm.html)

### 1.2.2 离线安装方式
但是我发现了另一个特别好用的方法 `rvm offline`
就是下载安装包 离线安装`RVM`    (; 
具体方法[官方Offline安装](https://rvm.io/rvm/offline)

```
// 离线包
curl -sSL https://github.com/rvm/rvm/tarball/stable -o rvm-stable.tar.gz
// 创建文件夹
mkdir rvm && cd rvm
// 解包
tar --strip-components=1 -xzf ../rvm-stable.tar.gz
// 安装 
./install --auto-dotfiles
// 加载
source ~/.rvm/scripts/rvm
// if --path was specified when instaling rvm, use the specified path rather than '~/.rvm'
```

## 1.3 安装 ruby

```
// 查询 ruby的版本
rvm list known
// 下载指定的版本
rvm install 2.4.0
// 将系统的ruby切换为下载的版本
rvm use 2.4.0  --default
```

## 2.  Homebrew 安装 Ruby
  `Mac`系统默认安装有`ruby`, 但是大家大家在使用一些`ruby`东西的使用,经常会遇到`You don't have write permissions for...` 等类似没有操作权限的问题,一般简单的操作就是 在终端命令前面添加 `sudo` 赋予指定以系统权限即可.
这样操作不好的地方在于,` Mac`自己集成的`Ruby`,一般为了系统安全与稳定死不允许用户执行这种操作,万一搞乱了,想要恢复原状只能是重装系统. 
但是使用`Homebrew`就可以方便的管理`Ruby`的包,想删除简单的命令即可搞定,而且隔离系统自带的`Ruby`,两者相安无事.
参考链接:
[Mac OS X 下使用 Ruby Gem 的两个坑](https://argcv.com/articles/4429.c)
[Mac OS X 上安装 Ruby](https://github.com/ruby-china/homeland/wiki/Mac-OS-X-上安装-Ruby)

#### 2.1. 先安装Homebrew

[参考我的Homebrew文章](http://manajay.github.io/2017/01/homebrew-clean-install/)

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### 2.2 安装`Ruby`

```
// 1.先更新homebrew
brew update 
// 2. 使用homebrew的安装指令
brew install ruby
```

##  如有问题,请issue我



