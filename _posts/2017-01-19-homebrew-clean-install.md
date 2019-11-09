## 安装位置

> `/usr/local` 这个才是正确位置, 这里不需要用户输入`sudo` (sudo属于系统级别的操作命令,所以极有可能破坏原系统文件,造成巨大隐患).

## Install 安装

### 一、 Install 注意前面的  `/usr/bin/ruby -e` 一定要有, 这样Homebrew 会将套件安装到独立目录，并将文件软链接至 /usr/local 

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
-----

#### 注意!!!! 如果安装成功,直接跳到步骤二, 如果失败, 最好先将之前安装过程产生的文件卸载干净!!!!, 如果本地没有安装文件,请不要尝试卸载操作

如果不清楚操作步骤, 尤其是 `rm`的危险操作, 请不要尝试卸载这个步骤!!! 

-----------------------


#### 下面是卸载清理的步骤

> (rm 是一个特别危险的行为, 一定要明确自己是删除的指定的目录, 不建议`rm -rf *` `rm -rf /` )

```
cd /usr/local
# 这里是 brew 的安装地址, 一定要明确这里的路径是否是正确的, 是不是有子文件夹 Cellar, 如果没有可能是你之前没有安装过 `brew`, 那么跳过该步骤和下面的几个 rm 命令
rm -rf Cellar 
brew prune # 修复命令, 当前版本不支持可以不执行
rm -rf ~/Library/Caches/Homebrew # 执行之前一定要明确该路径下有指定文件夹 ~/Library/Caches/Homebrew
```

---------------


### 二、 安装后自检

`brew doctor`  
如果显示 
`Your system is ready to brew`
恭喜你 安装成功了~

### 三、 工具包推荐

下面开始快乐的安装工具了, 一般brew 安装的都是无关界面的工具


```
appledoc		jenkins			pcre
autoconf		libtool			pkg-config
autojump		libyaml			protobuf
automake		maven			python3
brew-cask-completion	maven@3.3		readline
chisel			mysql			ruby
cmake			nginx			sqlite
cocoapods		node			tomcat@7
curl			ocaml			watchman
flow			ocamlbuild		xz
gdbm			oclint			zsh-syntax-highlighting
gettext			openssl
gnu-getopt		openssl@1.1
```
上面的都是`brew`安装的工具

* `brew search xxxx`  搜索软件xxx
* `brew install xxxx`  安装软件
* `brew uninstall xxxx` 卸载软件

### 那么问题来了 一些需要界面的`app` 如何安装呢

* 神奇的 `brew cask`来了 
* `brew install brew-cask-completion` 安装 `brew cask`

* 举个例子 

```
brew cask search sublimetext // 查询sublime-text的安装包
打印结果
==> Exact Match
caskroom/cask/sublime-text

说明 有个 sublime-text 的安装包

brew cask install sublime-text 等待下载就好了
```

#### 例如使用 brew cask 下载 eclipse

* `brew cask search eclipse`
打印结果如下: 

```

==> Partial Matches
caskroom/cask/eclipse-cpp
caskroom/cask/eclipse-ide
caskroom/cask/eclipse-installer
caskroom/cask/eclipse-java
caskroom/cask/eclipse-jee
caskroom/cask/eclipse-modeling
caskroom/cask/eclipse-php
caskroom/cask/eclipse-platform
caskroom/cask/eclipse-ptp
caskroom/cask/eclipse-rcp
caskroom/cask/eclipse-smarthome-designer
caskroom/cask/nodeclipse
caskroom/cask/eclipse-cpp
caskroom/cask/eclipse-ide
caskroom/cask/eclipse-installer
caskroom/cask/eclipse-java
caskroom/cask/eclipse-jee
caskroom/cask/eclipse-modeling
caskroom/cask/eclipse-php
caskroom/cask/eclipse-platform
caskroom/cask/eclipse-ptp
caskroom/cask/eclipse-rcp
caskroom/cask/eclipse-smarthome-designer
caskroom/cask/nodeclipse
```

* `brew cask install eclipse-jee` , 这里我只需要 jee的安装包 开发企业级产品

## 参考链接
* [Homebrew macOS 不可或缺的套件管理器](http://brew.sh/index_zh-cn.html)
* [全新安装 Mac OS Sierra (10.12) 并使用 HomeBrew 安装 ZSH + MNMP (Mac + Nginx + MySQL + PHP) 开发环境](https://laravel-china.org/topics/3129)
* [如何快速正确的安装 Ruby, Rails 运行环境](https://ruby-china.org/wiki/install_ruby_guide)
* [Homebrew 完全卸载](http://www.jianshu.com/p/18772092ee6b)
* [homebrew及homebrew cask安装与使用](http://www.jianshu.com/p/c829b5bbf701)
