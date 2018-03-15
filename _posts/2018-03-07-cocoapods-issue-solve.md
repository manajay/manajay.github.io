---
layout: post
title:  Cocoapods 的常见问题与警告
tag: Cocoapods
date: 2018-03-03 10:34:20 +09:00
---

### TextEdit编辑Podfile后的警告

```
[!] Smart quotes were detected and ignored in your Podfile. 
To avoid issues in the future, you should not use TextEdit for editing it. 
If you are not using TextEdit, you should turn off smart quotes in your editor of choice.
```

一般是因为引入了错误的标点符号

比如推荐 `''` 而不是 `‘ ’`


### target覆盖了pod的配置问题 

示例

```
!] The `xxx [Debug]` target overrides the `FRAMEWORK_SEARCH_PATHS` build setting defined in `Pods/Target Support Files/Pods-client_ios_fm_a/Pods-client_ios_fm_a.debug.xcconfig'. This can lead to problems with the CocoaPods installation
    - Use the `$(inherited)` flag, or
    - Remove the build settings from the target.
```
类似的配置项有`ALWAYS_EMBED_SWIFT_STANDARD_LIBRARIES` `OTHER_CFLAGS`  与 `HEADER_SEARCH_PATHS` 
解决方式 
1) Use the `$(inherited)` flag
 
步骤: `Target` -> `Build Settings` 搜索上面的配置项, 并设置 `$(inherited)`

![pod-build-setting](/assets/post/cocoapod/pod-build-setting.jpg)

上图看似很奇怪的配置,却可以成功消除警告
2) Remove the build settings from the target
界面上我并没有找到去除配置的方式,可能有的配置项不允许删除吧


