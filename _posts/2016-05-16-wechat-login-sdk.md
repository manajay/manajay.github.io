---
layout: post
title: 微信登录 第三方应用
subtitle: 使用微信SDK 第三方登录功能
author: manajay
date: 2016-05-16 14:17:13 +0800
categories: 登录 微信
tag: 登录 微信
---

# 1. 微信SDK 的集成

# 2. 微信开发者平台注册

* 注册应用名 应用的APPID APPKEY ,藏家圈项目中,创建了新的微信注册帐号,1.2.5版本的时候,在配置文件 和 代码的宏定义中 更换了相应的APPID ,否则之前的分享和登录功能都不能使用 回调了!


# 3. 微信SDK 的使用

* 如果项目中 使用 SVN 管理的话, 千万注意要 手动用命令行 将SDK 的.a静态库文件,添加到版本库中,否则 SVN 是默认 ignore 静态库的,编译会报各种找不到文件 API 的错误
* 做微信的登录功能 ,自定义 UI 界面, 在要使用微信登录界面中,一定要判断用户 是否 安装了微信应用,```[WXApi isWXAppInstalled];```判断安装之后,才能让微信登录的图标显示出来,否则提交 Appstore 会审核不通过.
* 之后注册 微信的 APPID ```[WXApi registerApp:kWeixinId];``` 一般写成宏,这样可以方便的修改,不需要关心注册代码写在什么地方.
* 使用微信登录的代码


``` objc
[WXApi registerApp:kWeixinId]; //注册APPID
SendAuthReq *req = [[SendAuthReq alloc] init];//第三方程序要向微信申请认证，并请求某些权限
req.scope = @"snsapi_userinfo" ;//应用授权作用域，如获取用户个人信息则填写
req.state = @"123" ;//用于保持请求和回调的状态，授权请求后原样带回给第三方。该参数可用于防止csrf攻击 ,设置随机
[WXApi sendReq:req];
```
请求发出去后,会收到回调, 需要在当前控制器,调用 

``` objc
- (void) onResp:(BaseResp *)resp
{
    if (resp.errCode == WXSuccess) {// 如果成功就 解析resp的授权临时票据code参数
        SendAuthResp *authResp = (SendAuthResp *)resp;
        if (authResp.code) {//用户换取access_token的code，仅在ErrCode为0时有效
            [self handleWxCode:authResp.code];// 用户自己定义的方法,来发起请求,获取 token 等
        }
    }
}
```
```
* Appsecret 是应用接口使用密钥，泄漏后将可能导致应用数据泄漏、应用的用户数据泄漏等高风险后果；存储在客户端，极有可能被恶意窃取（如反编译获取Appsecret）；
* access_token 为用户授权第三方应用发起接口调用的凭证（相当于用户登录态），存储在客户端，可能出现恶意获取access_token 后导致的用户数据泄漏、用户微信相关接口功能被恶意发起等行为；
* refresh_token 为用户授权第三方应用的长效凭证，仅用于刷新access_token，但泄漏后相当于access_token 泄漏，风险同上。

```

