---
layout: post
title:  iOS组件化(基于Cocoapods)-简单使用 
tag: [iOS, 组件化]
date: 2017-06-30 12:42:46 +09:00
---

### 注册 trunk 

```
pod trunk register xxxx@gmail.com manajay
```

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

### 验证本地的组件库是否有效

```
pod lib lint
```

测试一

```
pod lib lint --no-clean LJNetwork.podspec

 -> LJNetwork (1.0.0)
    - ERROR | attributes: Missing required attribute `summary`.
    - ERROR | [iOS] unknown: Encountered an unknown error (The `LJNetwork` pod failed to validate due to 1 error:
    - ERROR | attributes: Missing required attribute `summary`.

) during validation.

Pods workspace available at `/var/folders/86/kvr1rbk53zgb0xpkljyv2vn80000gn/T/CocoaPods-Lint-20180308-31173-4h194x-LJNetwork/App.xcworkspace` for inspection.

[!] LJNetwork did not pass validation, due to 2 errors.
```

修改 `summary` 后
测试二

```
pod lib lint --no-clean LJNetwork.podspec

 -> LJNetwork (1.0.0)
    - WARN  | [iOS] swift: The validator used Swift 3.2 by default because no Swift version was specified. To specify a Swift version during validation, add the `swift_version` attribute in your podspec. Note that usage of the `--swift-version` parameter or a `.swift-version` file is now deprecated.

Pods workspace available at `/var/folders/86/kvr1rbk53zgb0xpkljyv2vn80000gn/T/CocoaPods-Lint-20180308-31243-i7bisy-LJNetwork/App.xcworkspace` for inspection.
[!] LJNetwork did not pass validation, due to 1 warning (but you can use `--allow-warnings` to ignore it).
```

注意如果出现`WARN`字样 可以使用`pod repo push XXXSpec xxx.podspec --allow-warnings` 忽略警告

测试三 

```
pod lib lint --no-clean --allow-warnings  LJNetwork.podspec

 -> LJNetwork

 -> LJNetwork (1.0.0)
    - WARN  | xcodebuild:  ReachabilitySwift/Reachability/Reachability.swift:95:42: warning: plaform condition appears to be testing for simulator environment; use 'targetEnvironment(simulator)' instead

Pods workspace available at `/var/folders/86/kvr1rbk53zgb0xpkljyv2vn80000gn/T/CocoaPods-Lint-20180308-32416-1hp85wj-LJNetwork/App.xcworkspace` for inspection.

LJNetwork passed validation.
```

* 出现ERROR的话, 则必须解决

### 关联远程库, 设置`tag` 并推送到 远程库

```
git tag xxx
git push -u origin master // 首次推送
git push --tags
```

### 创建自己的私有远程管理库 

* 创建一个私有的远程库
* 本地添加私有远程库的索引库 `pod repo add XXXSpec  https://git.XXX/XXXSpec.git`
* 本地的索引库检查 `pod repo` ,除了`'https://github.com/CocoaPods/Specs.git'` 还有自己的

### 推送组件的 `podspec`文件到自己的私有远程管理库

* cd 到 组件`podspec`文件目录下
* `pod repo push XXXSpec xxx.podspec ` 推送该组件的`podspec`文件
* 远程私有管理库中查看是否将`xxx.podspec`文件推送到了远程

```
pod trunk --allow-warnings push LJNetwork.podspec
Updating spec repo `master`
Validating podspec
 -> LJNetwork (1.0.0)
    - WARN  | xcodebuild:  ReachabilitySwift/Reachability/Reachability.swift:95:42: warning: plaform condition appears to be testing for simulator environment; use 'targetEnvironment(simulator)' instead

Updating spec repo `master`

--------------------------------------------------------------------------------
 🎉  Congrats

 🚀  LJNetwork (1.0.0) successfully published
 📅  March 8th, 02:04
 🌎  https://cocoapods.org/pods/LJNetwork
 👍  Tell your friends!
```

### 使用 私有组件

* 安装`Cocoapod` 
* 添加私有仓库的管理源地址 `pod repo add XXSpec https://xxx/XXSpec.git` // 注意 这里因为是私有库 所以需要用户名 密码来获取私有管理库的资源

#### 搜索自己的私有库组件 `pod search`

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

------

* 自己项目中的组件升级

```
pod update --no-repo-update
```

### <补充> Cocoapods公有仓库推送

#### 登录

```
pod trunk register mailname@gmail.com 'username' --description='xxxx'
[!] Please verify the session by clicking the link in the verification email that has been sent to manajay.dlj@gmail.com
```

用于已有用户的登录 获取session

* 检验登录状态 `pod trunk me`

```
  - Name:     Manajay
  - Email:    manajay.dlj@gmail.com
  - Since:    xxxx xxth, xxxx xx:xx
  - Pods:
    - NJAlertView
    - LJNetwork
  - Sessions:
    - August xxth, xxxx xx:xx  - December xxth, xxx xx:xx. IP: xxxxx  Description: custom alertView
    - June xxth, xxxx xx:xx   -  November xxth, xxxx xx:xx. IP: xxxx
    - August xxth, xxx xx:xx - December xxth, xxxx xx:xx. IP: xxxxxxx
    - March xxth, xx:xx        -          July xxth, xx:xx. IP: xxxxx Description: xx xx
```

### 链接

* [Xcode project中引入Cocoapods管理](https://github.com/schillerGao/Cocoapods-private-spec)
* [iOS应用架构谈 开篇](https://casatwy.com/iosying-yong-jia-gou-tan-kai-pian.html)
* [iOS的组件化思路分享](http://www.jianshu.com/p/ffc065bf4843)
* [基于 CocoaPods 和 Git 的 iOS 工程组件化实践](https://skyline75489.github.io/post/2016-3-19_ios_modularization_practice.html)
* [SDWebImage.podspec学习](https://github.com/rs/SDWebImage/blob/master/SDWebImage.podspec)
* [iOS 组件化 —— 路由设计思路分析](http://www.jianshu.com/p/76da56b3bd55)

* [iOS开发之将自己的项目上传到CocoaPods](https://rakuyomo.github.io/2017/08/21/28-iOS%E5%BC%80%E5%8F%91%E4%B9%8B%E5%B0%86%E8%87%AA%E5%B7%B1%E7%9A%84%E9%A1%B9%E7%9B%AE%E4%B8%8A%E4%BC%A0%E5%88%B0CocoaPods/)


