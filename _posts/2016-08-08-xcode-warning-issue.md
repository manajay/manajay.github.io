---
layout: post
title: 开发中 warning 的 解决方案
tag: iOS开发技巧
date: 2016-08-08 10:05:24 +09:00
---

## 过期方法的 warning 消除

``` objc
#pragma clang diagnostic push
#pragma clang diagnostic ignored "警告标识的描述"  // 例如 -Wdeprecated-declarations
//    过期的方法      //
#pragma clang diagnostic pop

//  for example 

#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wdeprecated-declarations"
        return [self.text sizeWithFont:self.font constrainedToSize:CGSizeMake(CGFLOAT_MAX, height) lineBreakMode:self.lineBreakMode];
#pragma clang diagnostic pop

//警告标识的描述 的查找方法 
step 1) find "show the issue navigator"

step 2) find the issue 

step 3) reveal in log 

step 4) read message such as 
UILabel+Extension.m:48:27: warning: 'sizeWithFont:constrainedToSize:lineBreakMode:' is deprecated: first deprecated in iOS 7.0 - Use -boundingRectWithSize:options:attributes:context:
 
 
 [-Wdeprecated-declarations] // 描述标识 

```

## architecture x86_64
warning: no rule to process file 'README.md' of type net.daringfireball.markdown for architecture x86_64.
[引用地址](http://xcodar.blogspot.com/2015/01/warning-no-rule-to-process-file-of-type.html)

``` objc
we can resolve that things with simply following step:-

Step 1) Select Project Navigator
Step 2) Select your project
Step 3) Select your targetStep 
4) Select Build PhasesStep 
5) Move files which we don't want the compiler to process from Compile Sources to Copy Bundle Resources

```
解决方式2 

[引用位置](http://stackoverflow.com/questions/22778021/xcode-warning-no-rule-to-process-file-when-build-phases-has-this-file/26728623)

``` objc
Select the project target
Select the Build Phases
Expand the Compile Source
Remove the Header file (Reachability.h)
note : for removing the Reachability.h file from Compile Source, first select the file and then press the - button

If you need the header, then make sure that it is added to the "Headers" list below "Compile Sources".

```

