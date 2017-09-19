---
layout: post
title: 状态模式
tag: 设计模式
date: 2017-09-19 16:47:46 +09:00
---

### 讲解

* 类的行为由状态决定
* 解决哪些问题: 如果一个对象的行为受其状态的约束,随着状态的改变,其行为也随之改变时
* 如何发现:如果代码中存在过多的`if-else或者switch语句`,可以考虑这种可能

* 开闭原则 : 在面向对象编程领域中，开闭原则规定“软件中的对象（类，模块，函数等等）应该对于扩展是开放的，但是对于修改是封闭的”，这意味着一个实体是允许在不改变它的源代码的前提下变更它的行为。


### 代码
[Demo](https://github.com/manajay/DesignPattern-Java/tree/master/src/main/java/com/java/designpattern/StatePattern)



