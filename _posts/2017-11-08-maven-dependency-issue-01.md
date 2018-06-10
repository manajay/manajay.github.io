---
layout: post
title:  maven 依赖冲突示例<一>
tag: Maven
date: 2017-11-08 17:42:31 +09:00
---


### 问题描述

简单叙述 `java.lang.NoClassDefFoundError: org/springframework/core/MethodClassKey`

查找源码 发现 `MethodClassKey` 这个.类是在`org.springframework.transaction.interceptor.AbstractFallbackTransactionAttributeSource.getCacheKey`中被调用的,而项目中的**spring 4.2.6 Release** 的 `AbstractFallbackTransactionAttributeSource` 中 是没有`getCacheKey`这个方法, 说明项目存在jar包冲突问题. 

下面是完整的报错情况. 

```

java.lang.NoClassDefFoundError: org/springframework/core/MethodClassKey

	at org.springframework.transaction.interceptor.AbstractFallbackTransactionAttributeSource.getCacheKey(AbstractFallbackTransactionAttributeSource.java:133)
	at org.springframework.transaction.interceptor.AbstractFallbackTransactionAttributeSource.getTransactionAttribute(AbstractFallbackTransactionAttributeSource.java:91)
	at org.springframework.test.context.transaction.TransactionalTestExecutionListener.beforeTestMethod(TransactionalTestExecutionListener.java:173)
	at org.springframework.test.context.TestContextManager.beforeTestMethod(TestContextManager.java:265)
	at org.springframework.test.context.junit4.statements.RunBeforeTestMethodCallbacks.evaluate(RunBeforeTestMethodCallbacks.java:74)
	at org.springframework.test.context.junit4.statements.RunAfterTestMethodCallbacks.evaluate(RunAfterTestMethodCallbacks.java:86)
	at org.springframework.test.context.junit4.statements.SpringRepeat.evaluate(SpringRepeat.java:84)
	at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.runChild(SpringJUnit4ClassRunner.java:254)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.runChild(SpringJUnit4ClassRunner.java:89)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.springframework.test.context.junit4.statements.RunBeforeTestClassCallbacks.evaluate(RunBeforeTestClassCallbacks.java:61)
	at org.springframework.test.context.junit4.statements.RunAfterTestClassCallbacks.evaluate(RunAfterTestClassCallbacks.java:70)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.run(SpringJUnit4ClassRunner.java:193)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:68)
	at com.intellij.rt.execution.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:47)
	at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:242)
	at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:70)
Caused by: java.lang.ClassNotFoundException: org.springframework.core.MethodClassKey
	at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
	... 24 more

```


### 解决思路 

idea中 点击下面的方法

![maven-conflict-01](http://p3q1ykanf.bkt.clouddn.com/201806/maven-conflict-01.png)

发现该方法为 `tx-4.3.7.RELEASE`
![maven-conflict-02](http://p3q1ykanf.bkt.clouddn.com/201806/maven-conflict-02.png)


使用 maven的分支工具 `Dependency Analyzer` 查看 `tx`的相关信息如下
![maven-conflict-03](http://p3q1ykanf.bkt.clouddn.com/201806/maven-conflict-03.png)


项目中`redis`的依赖 `tx`包版本为`4.3.7.RELEASE`, 所以只要排除这个依赖,再重新`reimport`一下`maven`即可


解决的pom文件更改如下, 即 排除了 `spring-tx`

```
<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-redis</artifactId>
			<version>${spring.data.redis}</version>
			<exclusions>
				<exclusion>
					<artifactId>spring-tx</artifactId>
					<groupId>org.springframework</groupId>
				</exclusion>
			</exclusions>
		</dependency>
```

### 总结

* 熟悉 `maven`的生命周期
* 熟悉`maven`的依赖管理
* 增加 `deug` 报错后的排错信息查询能力. 





