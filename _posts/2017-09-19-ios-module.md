---
layout: post
title:  iOS组件化(基于Cocoapods)-简单使用 
tag: iOS
date: 2017-06-30 12:42:46 +09:00
---


### 创建本地私有库

* 创建 `pod lib create xxx`
* `Class` 中添加代码文件
* `Assets` 中添加图片等资源 : 注意获取资源使用 `[NSBundle bundleForClass:self]`,  注意图片要全名, 如果使用了资源,那么 `podfile`文件中添加描述`use_frameworks`!

### 更改本次的版本描述

* `podspec`文件 
* 注意 `version` 一定要对应 `git tag`
* `source_files`
* `dependency`
* `resource_bundles`


### 关联远程库, 设置`tag` 并推送到 远程库

```
git tag xxx
git push -u origin master // 首次推送
git push --tags
```

### 注册 trunk 

```
pod trunk register xxxx@gmail.com manajay
```

### 验证本地的组件库是否有效

`pod lib lint`

### 创建自己的私有远程管理库 

* 创建一个私有的远程库
* 本地添加私有远程库的索引库 `pod repo add XXXSpec  https://git.XXX/XXXSpec.git`
* 本地的索引库检查 `pod repo` ,除了`'https://github.com/CocoaPods/Specs.git'` 还有自己的

### 推送组件的 `podspec`文件到自己的私有远程管理库

* cd 到 组件`podspec`文件目录下
* `pod repo push XXXSpec xxx.podspec --allow-warnings`
* 远程私有管理库中查看是否将`xxx.podspec`文件推送到了远程

### 搜索自己的私有库组件 `pod search`

### 使用 私有组件

* 安装`Cocoapod` 
* 添加私有仓库的管理源地址 `pod repo add XXSpec https://xxx/XXSpec.git` // 注意 这里因为是私有库 所以需要用户名 密码来获取私有管理库的资源
* 创建项目
* 项目根目录创建 `Podfile`文件 `pod init`
* 编辑`Podfile`文件 

```
# 表示先去找私有，在找公有
source 'https://git.XXX/XXXSpec.git'
source 'https://github.com/CocoaPods/Specs.git'

use_frameworks!

pod 'xxx'
```

*  打开生成的xxx.xcworkspace文件


###  组件升级

* 私有远程库中的组件升级

```
pod repo push XXXSpec xxx.podspec --allow-warnings
```
* 项目中的组件升级

```
pod update --no-repo-update
```

### 链接

* [Xcode project中引入Cocoapods管理](https://github.com/schillerGao/Cocoapods-private-spec)
* [iOS应用架构谈 开篇](https://casatwy.com/iosying-yong-jia-gou-tan-kai-pian.html)
* [iOS的组件化思路分享](http://www.jianshu.com/p/ffc065bf4843)
* [基于 CocoaPods 和 Git 的 iOS 工程组件化实践](https://skyline75489.github.io/post/2016-3-19_ios_modularization_practice.html)
* [SDWebImage.podspec学习](https://github.com/rs/SDWebImage/blob/master/SDWebImage.podspec)
* [iOS 组件化 —— 路由设计思路分析](http://www.jianshu.com/p/76da56b3bd55)



