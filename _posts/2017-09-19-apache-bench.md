---
layout: post
title: ApacheBench的 认知与安装
tag: Test
date: 2017-03-22 14:05:46 +09:00
---


## 介绍

* [Web性能压力测试工具之ApacheBench（ab）详解及概念普及](http://www.jianshu.com/p/a7682a76695c)

* [超实用压力测试工具－ab工具](http://www.jianshu.com/p/43d04d8baaf7)


## 遇到的问题

* [ab问题解决方案](https://www.douban.com/note/501373268/)
* [ab常见问题汇总](http://www.jianshu.com/p/00b47a551d8c)

```
1. 使用apache 的ab做压力测试时，当压力过大，
例如请求1000000次，在没有执行完 
就报apr_poll:The timeout specified has expired错误
2. apr_pollset_poll: The timeout specified has expired (70007) -超时问题
```

* [apache ab 测试 apr_socket_connect(): 由于目标机器积极拒绝 无法连接](http://www.jianshu.com/p/dcdf40029a3c)

* [ab压力测试-突破最大线程数](http://www.jianshu.com/p/8f39dc823052)

## AB 的使用
* [Apache自带压力测试工具AB的使用方法](http://www.jianshu.com/p/1f3acc45ca89)

## 扩展了解

* [httpd的性能测试工具ab和httpd三种工作模式](http://www.jianshu.com/p/8fc63d865aef)

* [白盒测试中的几种用例覆盖方法](http://www.jianshu.com/p/66545552b20c)

