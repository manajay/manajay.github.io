---
layout: post
title: 正确安装Homebrew
tag: Homebrew,包管理工具
date: 2017-01-19 12:23:32 +09:00
---

## 安装位置

`/usr/local` 这个才是正确位置, 不需要你`sudo` 破坏原系统文件.

## Clean Install

### 如果以前安装过,建议首先清理一下系统环境

```
cd `brew --prefix`
rm -rf Cellar$ brew prune
rm -rf Library .git .gitignore bin/brew README.md share/man/man1/brew
rm -rf ~/Library/Caches/Homebrew
```

### Install

* 注意前面的  `/usr/bin/ruby -e` 一定要有

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
* Homebrew 会将套件安装到独立目录，并将文件软链接至 /usr/local 。


* 自检

`brew doctor`  
如果显示 
`Your system is ready to brew`
恭喜你 安装成功了~

* 一般`brew`都有的包

```
autoconf		libksba			readline
automake		libtool			ruby
cocoapods		libyaml			zlib
coreutils		openssl			zsh-autosuggestions
libgpg-error		pkg-config
```


## 参考链接

* [Homebrew macOS 不可或缺的套件管理器](http://brew.sh/index_zh-cn.html)

* [全新安装 Mac OS Sierra (10.12) 并使用 HomeBrew 安装 ZSH + MNMP (Mac + Nginx + MySQL + PHP) 开发环境](https://laravel-china.org/topics/3129)
* [如何快速正确的安装 Ruby, Rails 运行环境](https://ruby-china.org/wiki/install_ruby_guide)

* [Homebrew 完全卸载](http://www.jianshu.com/p/18772092ee6b)



