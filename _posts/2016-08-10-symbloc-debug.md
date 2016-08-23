---
layout: post
title: UI调试相关
date: 2016-08-10 09:33:32.000000000 +09:00
---

# 一、 Reveal - 界面调试 利器
[@Manajay:我的简书](http://www.jianshu.com/users/089cf0c28ed5/latest_articles)

## 1. 作为调试 UI 的神器, reveal 所带来的好处 真是无以言表.

>时时查看 真机或模拟器 的UI 显示 情况;
>
> 界面动态修改UI 控件的参数,无需代码,无需重启程序 一切 xib 中有的参数,都可以调整,包括约束更新
>
>对于越狱机器 , 还可以在逆向工程中大展拳脚, 学习他人的 布局技巧

## 2. Reveal 的 安装 与配置 问题

然而,上述好处的 所有种种, 前提是 你配置好了 相关的 软件.
下面我就将我遇到的相关问题 和 解决办法 罗列出来, 希望其他人不要再踩坑.

### 2.1 动态方式 配置 reveal   <推荐方式>

* 创建 llbd指令文件 
```vim ~/.lldbinit ```
千万 千万不要粘贴错误. 如果经过一下步骤,仍然出问题, 先检查一遍这个文件. 

检查方法, 首先 将本隐藏文件 显示出来 ,方法如下

```
// 显示所有文件, 包括隐藏文件
defaults write com.apple.Finder AppleShowAllFiles YES 
//回车，重启Finder.

// 如果先 隐藏, 将 YES 改为 NO 即可.
```
如果不想使用命令行, 推荐 Commander One Pro 软件

* 将 lldb 指令 输入到 本文件中. 还是要注意粘贴的时候 不要漏掉 任何字符

```
command alias reveal_load_sim expr (void*)dlopen("/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/libReveal.dylib", 0x2);
command alias reveal_load_dev expr (void*)dlopen([(NSString*)[(NSBundle*)[NSBundle mainBundle]               pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0    x4], 0x2);
command alias reveal_start expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]           postNotificationName:@"IBARevealRequestStart" object:nil];
command alias reveal_stop expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]            postNotificationName:@"IBARevealRequestStop" object:nil];

```

** 注意** 我查过所有的 博客文章, 基本都是使用的上述的 lldb 命令.
不过后来我在 做真机测试的时候, 遇到了下面的问题 

```
error: no known method '-cStringUsingEncoding:'; cast the message send to the method's return type
error: 1 errors parsing expression

```

真机并不能够联通 reveal.  经过多方 Google, 终于解决了 此问题.  方法 如下:

```
command alias reveal_load call (void*)dlopen((char*)[(NSString*)[(NSBundle*)[NSBundle mainBundle] pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0x4], 0x2);

```

** 该指令是用 reveal_load  替换原来的reveal_load_sim 和 reveal_load_dev  指令, 问题回答者说 This will ensure that if Reveal is already loaded, calling the command again will not try to load the library a second time.**
[cStringUsingEncoding问题 的答案链接 ](http://support.revealapp.com/discussions/questions/52969-integrating-reveal-no-longer-working)

所以 本人建议将 .lldbinit文件中的指令修改为 :

```
command alias reveal_load call (void*)dlopen((char*)[(NSString*)[(NSBundle*)[NSBundle mainBundle] pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0x4], 0x2);
command alias reveal_start expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]           postNotificationName:@"IBARevealRequestStart" object:nil];
command alias reveal_stop expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]            postNotificationName:@"IBARevealRequestStop" object:nil];
```
** 这样你就不需要在 模拟器指令断点 和 真机指令断点上切换, 而且也不会有上述问题**

* 其余步骤 请参考 [周静的 - iOS开发中集成Reveal](http://blog.devzeng.com/blog/ios-reveal-integrating.html)

* 问题 **动态方式 使用真机测试的时候 ,  需要 xcode 和 真机在 同一个网络下面 才能够使用 reveal 调试**
* 问题 ** 使用脚本 签名的问题  **
``` ambiguous 说明 你指令了多个 开发者帐号```
解决办法: 

![build Setting.png](http://upload-images.jianshu.io/upload_images/1435355-3a4647916bae1451.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###  2.2 静态方式 Reveal.framework

>步骤参照[周静的 - iOS开发中集成Reveal](http://blog.devzeng.com/blog/ios-reveal-integrating.html) , 这里只讲述问题

* 添加编译指令``` -ObjC -lz -framework Reveal ``` 的时候 遇到 问题 
```多处报错  duplicate symbol  ``` ,这是因为 与项目中其他的静态库发生冲突所致 ! 
* 解决办法 : 暂时没找到, 所以我采用了 动态的方式 

### 3. CocoaPods

* 参考 链接 [界面调试神器Reveal的三种集成方式](http://www.sxt.cn/info-8708-u-13398.html)


### 注意 发布版本一定要将 reveal 的 

## 参考链接

[周静的 - iOS开发中集成Reveal](http://blog.devzeng.com/blog/ios-reveal-integrating.html)
[界面调试神器Reveal的三种集成方式](http://www.sxt.cn/info-8708-u-13398.html)
[cStringUsingEncoding 问题的答案链接 ](http://support.revealapp.com/discussions/questions/52969-integrating-reveal-no-longer-working)


# 二、 symbolic breakpoint 符号断点

```
// 约束冲突
UIViewAlertForUnsatisfiableConstraints

```

