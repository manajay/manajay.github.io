---
layout: post
title: Spring初识
tag: Spring
date: 2017-09-04 18:52:46 +09:00
---

新手入门认识，有认识错误，请严厉批评


![spring-framework框架](/assets/post/spring-framework-runtime.png)

### 核心概念

* IoC
* AOP

IoC核心是 顶层 组件与应用的基础，所有的其他功能都是在这个基础上面发展而来的。

##### Ioc 

控制反转 也可以说是 依赖注入**DI** 。
通俗的将，以前我们写代码，需要自己管理 一个对象的生命周期，初始化，销毁，各个对象的以来管理。

小项目还好，如果一个项目庞大起来，整个对象关系就好像一张密密麻麻的蜘蛛网一样， 只是建立 对象的依赖就已经很耗费精力了。

而 Spring 的IoC这一功能，将程序员从这一繁重的业务中解脱出来，对象的管理都放在了Spring自己的BEAN 容器中处理。 

我们只需要配置好 Context xml的配置文件就可以在代码中轻松的使用这些对象了。

也同时 有利于 模块的解耦

---------------

* Spring BeanFactory 容器
* Spring ApplicationContext 容器

------------------

* 定义 
* 作用域
* 生命周期
* 后置处理器
* 
* 实例化 
* 配置
* 装配 
* 
* 注入
* 构造注入
* 属性注入
* 注解

------------

##### AOP

面向切面编程，与平时的OOP 面向对象编程 区分开来的思想，是OOP的补充 ，它是利用动态代理 实现的

**技术点**
* 动态代理

**解决的问题或者说应用场景**
主要是为了处理业务的交叉关注点问题，比如一些公共服务
* 日志收集
* 缓存
* 事务管理
* 安全检查
* 对象池管理
> 对象池化的基本思路是：创建多个对象并管理，使用时借出对象，用完归还对象，等下一次需要这种对象的时候，再拿出来重复使用，从而在一定程度上减少频繁创建对象所造成的开销。用于充当保存对象的“容器”的对象，被称为“对象池”（Object Pool，或简称Pool）。
> 
> spring不是真正意义上的对象池，它只是一个对象管理的容器。 因为spring容器里面大部分是 singleton 或者 prototype 没有状态的区分。这也是它效率不好的地方。

## Spring 层次

大略分为下面几层 ，一共20多个模块

* 核心 Core 
* AOP 
* data access
* web

### 核心模块  

#### 核心组件

* Context：  也就是 IoC容器
* Bean ： 对象通过配置文件的方式，由Spring来管理对象存储空间，生命周期的分配
* Core ： Spring 发现、建立和维护Bean之间关系的一揽子工具，其中最重要的是 Resource

三者是相互联系，依赖的

![image.png](/assets/post/spring-dependency.png)

### aop

* AOP
* Aspects
* Instrumentation
* Messaging

![image.png](/assets/post/spring-dependency-aop.png)

### 持久层

* JDBC  java 关于数据库连接的api接口
* ORM 关系型数据库
* OXM
* JMS 消息队列
* Transactions 事务管理


![image.png](/assets/post/spring-dependency-full.png)

### Web

* web
* Servlet
* Portlet
* Structs --> WebSocket


![image.png](/assets/post/spring-dependency-web.png)

![image.png](/assets/post/spring-dependency-messaging.png)


## 参考链接

[spring framework体系结构及内部各模块jar之间的maven依赖关系](https://www.cnblogs.com/ywlaker/p/6136625.html)



