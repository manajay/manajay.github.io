---
layout: post
title: 项目 project 与 target 动态库 静态库的知识
tag: iOS
date: 2016-08-09 09:49:02 +09:00
---

**本文是阅读笔记, 原文在 最下面**

## 项目 project 与 target 动态库 静态库的知识

> 1. project就是一个项目，或者说工程，一个project可以对应多个target
> 2. targets之间完全没有关系。但target和project有关系，target的setting会从project settings中继承一部分
> 3. Copy Bundle Resources 是指生成的product的.app内将包含哪些资源文件
> 4. Compile Sources 是指将有哪些源代码被编译
> 5. Link Binary With Libraries 是指编译过程中会引用哪些库文件

## 编译变量 设置开发和发布版的宏名

![编译变量](/source/14707082912619.jpg)


```
#if DEBUG
// 这是正式版咯
#else
// 这是开发版咯
#endif
```
这里补充一种方法 ,
Xcode中可以利用Compiler Flags来设置宏
可以通过设置Compiler Flags来定义宏，然后就可以在代码中使用这些宏，来进行条件编译的操作。有三种方式设置：

```
// 在Target>Build Setting>Custom Compiler Flags>Other C Flags
OTHER_CFLAGS (Other C Flags)

// 在Target>Build Setting> Preprocessing > Preprocessor Macros
GCC_PREPROCESSOR_DEFINITIONS (Preprocessor Macros)

// 在Target>Build Setting> Packaging > Info.plist Preprocessor Definitions
INFOPLIST_PREPROCESSOR_DEFINITIONS (Info.plist Preprocessor Definitions)
```


## 依赖


在主工程的 Build Phases > Target Dependencies 中添加 Module，并且添加一个 New Copy Files Phase。

![Dependencies](/source/14707089854981.jpg)


这样，打包时会将生成的 Module.framework 添加到 main bundle 的根目录下。

# 引用
> 
[参考 travin 的文章: 我的阅读笔记](http://js.sunansheng.com/p/e304247ede59)
[参考 Leon_Lizi 静态库和动态库的链接 特别全](https://www.gitbook.com/book/leon_lizi/-framework-/details)

