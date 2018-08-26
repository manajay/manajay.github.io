---
layout: post
title:  OpenVPN 的简易使用教程
tag: OpenVPN
date: 2018-02-06 16:04:20 +09:00
---

在OpenVPN中，如果用户访问一个远程的虚拟地址（属于虚拟网卡配用的地址系列，区别于真实地址），则操作系统会通过路由机制将数据包（TUN模式）或数据帧（TAP模式）发送到虚拟网卡上，服务程序接收该数据并进行相应的处理后，会通过SOCKET从外网上发送出去。这完成了一个单向传输的过程，反之亦然。当远程服务程序通过SOCKET从外网上接收到数据，并进行相应的处理后，又会发送回给虚拟网卡，则该应用软件就可以接收到。(摘自维基百科)

> 简而言之, 外网可以通过**OpenVPN**, 转发到公司内网,借此访问一些受限的资源

### 软件下载

#### Tunnelblick

* [百度网盘下载地址](https://pan.baidu.com/s/1htgHFN)

* [Tunnelblick官网](https://tunnelblick.net/)

#### openvpn 

* [openvpn的百度网盘下载地址](https://pan.baidu.com/s/1htqlfso)
* [openvpn官网](https://openvpn.net/)

### 安装

![](/assets/post/OpenVPN/15179002976237.jpg)

![](/assets/post/OpenVPN/15179003296210.jpg)

### 配置 mac 的 tunnelblick
最重要的一步, 安装配置文件
![](/assets/post/OpenVPN/15179003672615.jpg)

这里只需要在本地找到配置文件, 双击它就会被 `Tunnelblick` 自动识别的.

![](/assets/post/OpenVPN/15179004512252.jpg)

![](/assets/post/OpenVPN/15179005555140.jpg)

双击配置文件

![](/assets/post/OpenVPN/15179005748425.jpg)

连接

![](/assets/post/OpenVPN/15179006094420.jpg)

叮叮叮 连接成功

![](/assets/post/OpenVPN/15179006726359.jpg)


### 配置 windows的openvpn


1. 安装`openvpn`客户端
双击`openvpn-install-2.4.3-I602.exe`进行安装

2. 拷贝配置文件
将文件夹 配置文件 中的四个文件拷贝至客户端中
如安装路径为 `D:\Program Files\OpenVPN\config`，将上面截图的四个文件拷贝至此文件夹中

3. 运行客户端
通常桌面会自动生成图标`OpenVPN GUI`，双击即可打开

4. 连接vpn
桌面右下角会出现客户端图标，右键单击选择`connect`，弹出连接日志，连接成功后会自动隐藏，使用即可。


> 配置文件 ,一般需要运维配置与提供


### 参考链接

* [OpenVPN维基百科](https://zh.wikipedia.org/zh-hans/OpenVPN)

* [如何通过Tunnelblick轻松设置Mac上的OpenVPN](http://mos86.com/55473.html)

