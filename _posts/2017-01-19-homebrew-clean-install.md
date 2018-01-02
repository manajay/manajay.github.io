---
layout: post
title: 正确安装homebrew
tag: homebrew
date: 2017-01-19 12:23:32 +09:00
---

* 正确安装位置 `/usr/local`, 不需要你`sudo` 破坏原系统文件.


### 如果以前安装过,建议首先清理一下系统环境

```
cd `brew --prefix`
rm -rf Cellar$ brew prune
rm -rf Library .git .gitignore bin/brew README.md share/man/man1/brew
rm -rf ~/Library/Caches/Homebrew
```

### 安装命令

* 注意前面的  `/usr/bin/ruby -e` 一定要有

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

* `homebrew` 会将套件安装到独立目录，并将文件软链接至 `/usr/local` 。
* `brew doctor` 自检,查看安装后的`brew`是否有问题, 如果显示 `Your system is ready to brew`, 那就表示安装成功了~
* `brew list`显示已经安装的包

```
adns			gmp			libimobiledevice	maven@3.3		pinentry		tomcat
autoconf		gnupg			libksba			mysql			pkg-config		tree
automake		gnutls			libplist		nettle			protobuf		usbmuxd
brew-cask-completion	icu4c			libtasn1		nginx-full		python			x264
carthage		jenkins			libtool			node			python3			xvid
cocoapods		lame			libunistring		npth			readline		xz
coreutils		libassuan		libusb			openssl			redis
ffmpeg			libffi			libxml2			openssl@1.1		rtmp-nginx-module
gdbm			libgcrypt		libyaml			p11-kit			sqlite
gettext			libgpg-error		makedepend		pcre			swiftlint
```


## 参考链接

* [Homebrew macOS 不可或缺的套件管理器](http://brew.sh/index_zh-cn.html)

* [全新安装 Mac OS Sierra (10.12) 并使用 HomeBrew 安装 ZSH + MNMP (Mac + Nginx + MySQL + PHP) 开发环境](https://laravel-china.org/topics/3129)
* [如何快速正确的安装 Ruby, Rails 运行环境](https://ruby-china.org/wiki/install_ruby_guide)

* [Homebrew 完全卸载](http://www.jianshu.com/p/18772092ee6b)

## 有问题,请issue我

