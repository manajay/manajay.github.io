---
layout: post
title:  MWeb 配置 七牛云 图床
tag: 工具
date: 2018-03-31 14:06:20 +09:00
---

`Preference`偏好 -> `Publishing`发布服务

![](/assets/post/mweb/api.jpg)


* `Name` 名称可以随便填写, 主要是为了标志此云存储空间的名称 ,`Bucket Name` 同理
* `API URL` 此处地址的填写指南可以点击[[?]](https://developer.qiniu.com/kodo/manual/1671/region-endpoint)号按钮,跳转到七牛云的存储区域域名列表

![](/assets/post/mweb/storage-area.jpg)

根据下面我的七牛云空间信息

![](/assets/post/mweb/config.jpg)

所以我选择的是华东区域的服务器域名

* `Access Key` & `Secret Key` 的配置

`个人中心` -> `密钥管理` 面板查找

![](/assets/post/mweb/token.jpg)

* `Image URL Prefix`图片前缀 也就是上面的七牛空间详情图的 **测试域名** 地址 
* *注意这里的域名必须以 http://开头*  否则生成的图片地址会有多余的前缀
* `Image URL Suffix` 图片后缀可以不用填写

测试一下吧

![](/assets/post/mweb/all-config.jpg)

> 最后实战 
![](/assets/post/mweb/test.jpg)


## 参考链接

* [MWeb 七牛云图床配置 ](http://zh.mweb.im/mweb-1.9.3-release.html)

