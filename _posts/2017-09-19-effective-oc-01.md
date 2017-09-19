---
layout: post
title: 高效编码(一)
tag: Effective-OC
date: 2017-05-23 23:42:46 +09:00
---

#### 在类的头文件中尽量少运入其他头文件

1、 除非有必要,否则不要引入头文件. 一般来说,应该在类的头文件中使用向前引用来声明一个类,并在实现文件中,引入那些类的头文件.
好处: 减少编译时间,解决互相引用问题,降低类之间的耦合.
2、 如果无法使用向前引用,比如声明某个类遵守一项协议.这种情况下,尽量把声明移到"`class-continuation`分类"中. 
如果不行就把协议单独放在一个头文件中,然后将其引入.
两者的区别在于: 协议与遵循协议类的关系,是否有强烈的依赖关系.比如委托协议(`delegate protocol`).

#### 多用字面量语法,少用与之等价的方法

1. 应该使用字面量语法来创建字符串、数值、数组、字典。
2. 应该使用下标操作访问数组或者字典

> **优点** ： 简洁高效,更安全(对nil的处理),无关语法易读性强 
> **缺点** : 除了字符串外,所创建的对象必须属于`Foundation`框架,创建出的对象都是不可变的,如果想生成可变对象,需要`mutableCopy`一份

#### 多使用类型常量,少用#define 预处理指令

1. 不要用预处理指令定义常量. 这样定义出来的常量不含类型信息,编译器只是会在编译前据此执行查找和替换操作.即使有人重新定义了常量值,编译器也不会产生警告信息,这将导致程序中常量值不一致. 
2. 如果常量只在某个类中使用,不提供给外部.则建议在类的实现文件中使用`static const`来定义 "只在编译单元内可见的常量(translation-unit-specific constant)". 命名规范,因为不出现在全局符号表中,所以无须为其名称加前缀. 举例动画时长: ` static const NSTimeInterval kAnimationDuration = 0.3`(此种写法,常量只会出现在编译单元也就是某类的实现文件中,实际上,编译器根本不会创建符号,而是像`#define`预处理指令一样,把所有遇到的常量都替换成常值)
3. 对于全局符号表类型的常量,一般在头文件中使用`extern`来声明全局常量,并在相应的实现文件中定义其值. 命名规范:一般其名称前面会用与之相关的类名做前缀. 主要是因为`OC`没有命名空间`namespace`,全局变量会出现变量名冲突`duplicate symbol`的编译错误.

```
// header file
extern NSString *const DDLoginManagerDidLoginNotification;

// implementation file
NSString *const DDLoginManagerDidLoginNotification = @"DDLoginManagerDidLoginNotification";

```

这里`const` 修饰的 `DDLoginManagerDidLoginNotification`是一个字符串指针变量,不能够被修改.

#### 用枚举Enum表示状态、选项、状态码 ，禁止使用魔法数字

1、 应该使用枚举类标识状态机的状态,传递给方法的选项以及状态码等值,给这些值起易懂的名字.
2、 如果把传递给某个方法的选项为枚举类型,而多个选项又可以同时使用,则可以将枚举选项设置为**2**的幂数,以便通过按位与或操作将其结合起来

```
enum UIViewAutoresizing {
    UIViewAutoresizingNone                  = 0,
    UIViewAutoresizingFlexibleLeftMargin    = 1 << 0,
    UIViewAutoresizingFlexibleWidth         = 2 << 0,
    UIViewAutoresizingFlexibleRightMargin   = 3 << 0,
    UIViewAutoresizingFlexibleTopMargin     = 4 << 0,
    UIViewAutoresizingFlexibleHeight        = 5 << 0,
    UIViewAutoresizingFlexibleBottomMargin  = 6 << 0,
}
```

3、用`NS_ENUM`与`NS_OPTION`宏来定义枚举类型,并指明其底层数据类型. 这样做可以确保枚举是用开发者所选的底层数据类型来实现出来的,而不会采用编译器所选的类型.

```
/*!
 @typedef ChauffeureCarServiceState
 @brief 某地区开通专车服务的状态
 @constant ChauffeureCarServiceStateNone 未开通.
 @constant ChauffeureCarServiceStateEstablished 已开通
 */
typedef enum : NSUInteger {
    /** 专车服务 未开通*/
    ChauffeureCarServiceStateNone = 1,
    /** 专车服务 已开通*/
    ChauffeureCarServiceStateEstablished = 2,
} ChauffeureCarServiceState;

------------------------- snippet sample
/*!
 @typedef <#Enum Name#>
 @brief 状态
 @constant <#Enum State 0#> <#state 0 description#>.
 @constant <#Enum State 1#> <#state 1 description#>
 */
typedef enum : <#DataType#> {
    <#/** state 0 description */#>
    <#Enum State 0#> = <#Value 0#>,
    <#/** state 1 description */#>
    <#Enum State 1#> = <#Value 1#>,
} <#Enum Name#>;
```

4、在处理枚举类型的`switch`语句中不要实现默认`default`分支. 这样的话,加入新枚举之后,编译器就会提示开发者,`switch`语句并未处理所有的枚举.



