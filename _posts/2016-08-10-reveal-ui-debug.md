---
layout: post
title: Reveal - 界面调试利器
tag: iOS
date: 2016-08-10 09:33:32 +09:00
---

## Reveal

* 时时查看 真机或模拟器 的UI 显示 情况;
* 界面动态修改UI 控件的参数,无需代码,无需重启程序 一切 xib 中有的参数,都可以调整,包括约束更新
* 对于越狱机器 , 还可以在逆向工程中大展拳脚, 学习他人的 布局技巧

## Reveal 的安装与配置问题

然而,上述好处的 所有种种, 前提是 你配置好了 相关的 软件.
下面我就将我遇到的相关问题 和 解决办法 罗列出来, 希望其他人不要再踩坑.

### 动态方式配置 Reveal   <推荐方式>

> OC模拟器

首先创建`lldb`指令文件 
`vim ~/.lldbinit`
千万 千万不要粘贴错误. 如果经过一下步骤,仍然出问题, 先检查一遍这个文件. 
检查方法,首先将本隐藏文件显示出来,方法如下
// 显示所有文件, 包括隐藏文件

```
defaults write com.apple.finder AppleShowAllFiles Yes && killall Finder
```
如果不想使用命令行,推荐CommanderOnePro软件

将`lldb`指令输入到本文件中. 要注意粘贴的时候不要漏掉任何字符

```
command alias reveal_load_sim expr (void*)dlopen("/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/libReveal.dylib", 0x2);
command alias reveal_load_dev expr (void*)dlopen([(NSString*)[(NSBundle*)[NSBundle mainBundle]               pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0    x4], 0x2);
command alias reveal_start expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]           postNotificationName:@"IBARevealRequestStart" object:nil];
command alias reveal_stop expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]            postNotificationName:@"IBARevealRequestStop" object:nil];
```

注意:这几个`lldb`指令,只使用于`OC`的项目,而且只是模拟器.

> OC真机
 
会遇到下面的问题 

```
error: no known method '-cStringUsingEncoding:'; cast the message send to the method's return type
error: 1 errors parsing expression
```
解决方法:
①第一步: 

```
command alias reveal_load call (void*)dlopen((char*)[(NSString*)[(NSBundle*)[NSBundle mainBundle] pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0x4], 0x2);
```

该指令是用`reveal_load`替换原来的`reveal_load_sim`和`reveal_load_dev`  指令,`This will ensure that if Reveal is already loaded, calling the command again will not try to load the library a second time.`

[cStringUsingEncoding问题 的答案链接 ](http://support.revealapp.com/discussions/questions/52969-integrating-reveal-no-longer-working)

建议将`.lldbinit`文件中的指令修改为 :

```
command alias reveal_load call (void*)dlopen((char*)[(NSString*)[(NSBundle*)[NSBundle mainBundle] pathForResource:@"libReveal" ofType:@"dylib"] cStringUsingEncoding:0x4], 0x2);
command alias reveal_start expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]           postNotificationName:@"IBARevealRequestStart" object:nil];
command alias reveal_stop expr (void)[(NSNotificationCenter*)[NSNotificationCenter defaultCenter]            postNotificationName:@"IBARevealRequestStop" object:nil];
```
模拟器中添加断点:
![reveal_load_sim](/assets/post/reveal_load_sim.png)

而且此方法在切换真机与模拟器时,不需要更换断点的指令

② 第二步:
在`applicationDidBecomeActive`方法中调用 `loadReveal`方法

``` 
#import <dlfcn.h>

#pragma mark - Reveal

- (void)loadReveal
{
    if (NSClassFromString(@"IBARevealLoader") == nil)
    {
        NSString *revealLibName = @"libReveal"; // or @"libReveal-tvOS" for tvOS targets
        NSString *revealLibExtension = @"dylib";
        NSString *error;
        NSString *dyLibPath = [[NSBundle mainBundle] pathForResource:revealLibName ofType:revealLibExtension];
        
        if (dyLibPath != nil)
        {
            NSLog(@"Loading dynamic library: %@", dyLibPath);
            void *revealLib = dlopen([dyLibPath cStringUsingEncoding:NSUTF8StringEncoding], RTLD_NOW);
            
            if (revealLib == NULL)
            {
                error = [NSString stringWithUTF8String:dlerror()];
            }
        }
        else
        {
            error = @"File not found.";
        }
        
        if (error != nil)
        {
            NSString *message = [NSString stringWithFormat:@"%@.%@ failed to load with error: %@", revealLibName, revealLibExtension, error];
            UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"Reveal library could not be loaded"
                                                                           message:message
                                                                    preferredStyle:UIAlertControllerStyleAlert];
            [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
            [[[[[UIApplication sharedApplication] windows] firstObject] rootViewController] presentViewController:alert animated:YES completion:nil];
        }
    }
}
//-------------------------------------------
// 细节发布版本的屏蔽此方法: 
# DEBUG
       [self loadReveal];
#endif
//-------------------------------------------
```


③ 第三步: 
真机测试时,需要`xcode`和真机在同一个局域网下.

真机测试时,证书签名的问题`ambiguous`说明你指定了多个开发者帐号
解决办法: 

![build-setting.png](/assets/post/build-setting.png)

###  swift 项目
报错: `swift` 没有`(void*)` 类型,所以要自己删除`(void*)`,其他指令不便

###  静态方式 Reveal.framework

[周静的 - iOS开发中集成Reveal](http://blog.devzeng.com/blog/ios-reveal-integrating.html) 

报错
添加编译指令`-ObjC -lz -framework Reveal`时
多处报错`duplicate symbol` ,这是因为 与项目中其他的静态库发生冲突所致.
解决办法 : 具体分析报错原因,一般设置Other Flag 的相关配置就可以了

### 使用CocoaPods集成

[界面调试神器Reveal的三种集成方式](http://www.sxt.cn/info-8708-u-13398.html)


## 参考链接

* [周静的 - iOS开发中集成Reveal](http://blog.devzeng.com/blog/ios-reveal-integrating.html)

* [界面调试神器Reveal的三种集成方式](http://www.sxt.cn/info-8708-u-13398.html)

* [cStringUsingEncoding 问题的答案链接 ](http://support.revealapp.com/discussions/questions/52969-integrating-reveal-no-longer-working)

