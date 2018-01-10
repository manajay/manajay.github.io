---
layout: post
title:  MongoDB系列 01 - MongoDB与相关技术
tag: MongoDB
date: 2018-01-10 14:22:50 +09:00
---

![nosql vs sql](/assets/post/nosql-sql.jpeg)

### MongoDB 简介

`MongoDB`是一种,分布式文件存储,文档导向的数据库管理系统，由`C++`撰写而成，以此来解决应用程序超大规模数据存储的问题. 它是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的. 

* 支持多语言
* 键值对数据存储
* 强大的查询功能
* 索引排序
* 允许在服务端执行脚本
* 高性能、易部署、易使用，存储数据非常方便
* 支持主从同步, 由于操作都是在主机，从机将复制任何更改的数据。


## MongoDB 相关技术的比较

> 相关技术: `NoSQL(NoSQL = Not Only SQL )，意即"不仅仅是SQL" `

### RDBMS vs NoSQL  

#### RDBMS 特点

- 高度组织化结构化数据 
- 结构化查询语言`SQL`
- 数据和关系都存储在单独的表中。 
- 数据操纵语言`DML`，数据定义语言`DDL` 
- 严格的一致性
- 基础事务

#### NoSQL特点

- 代表着不仅仅是`SQL`
- 没有声明性查询语言
- 没有预定义的模式
-键 - 值对存储，列存储，文档存储，图形数据库
- 最终一致性，而非`ACID`属性
- 非结构化和不可预知的数据
- `CAP`定理 
- 高性能，高可用性和可伸缩性
 

## MongoDB的安装

### 1. Linux 系统 

* 安装

```shell
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz    # 下载

tar -zxvf mongodb-linux-x86_64-3.0.6.tgz                                   # 解压

mv  mongodb-linux-x86_64-3.0.6/ /usr/local/mongodb                         # 将解压包拷贝到指定目录

export PATH=/usr/local/mongodb/bin:$PATH # 配置执行路径的环境变量

mkdir -p /data/db # 设置默认的数据库路径
```

* 启动命令

```shell
./mongod
```
 

*  配置文件 `mongod.conf`




### 2. Mac OS 系统

* 使用`homebrew`安装`mongodb`

```shell
 brew install mongodb
```

* 成功标志

```
To have launchd start mongodb now and restart at login:
  brew services start mongodb
Or, if you don't want/need a background service you can just run:
  mongod --config /usr/local/etc/mongod.conf
==> Summary
🍺  /usr/local/Cellar/mongodb/3.6.1: 19 files, 288.5MB
```   

* 启动时 调整配置

```shell
mongod --config /usr/local/etc/mongod.conf
```
 
*  配置文件 `mongod.conf`


## 应用场景的考虑

**适合场景**

* 超大规模数据存储
* 网站数据
* 缓存
* 高伸缩性的场景
* 用于对象及`JSON`数据的存储

**不适合场景**

* 高度事务性系统
* 传统的商业智能应用
* 复杂的跨文档(表)级联查询

### 相关链接

* [MongoDB 维基百科](https://zh.wikipedia.org/zh-hans/MongoDB)
* [NoSQL 维基百科](https://zh.wikipedia.org/wiki/NoSQL)
* [nosql RUNOOB.COM](http://www.runoob.com/mongodb/nosql.html)
* [MongoDB RUNOOB.COM](http://www.runoob.com/mongodb/mongodb-tutorial.html)
* [MongoDB、Cassandra 和 HBase 三种 NoSQL 数据库比较](http://blog.jobbole.com/91923/)
* [The SQL vs NoSQL Difference: MySQL vs MongoDB](https://medium.com/xplenty-blog/the-sql-vs-nosql-difference-mysql-vs-mongodb-32c9980e67b2)
* [几款主流 NoSql 数据库的对比](https://www.cnblogs.com/vajoy/p/5471308.html)


