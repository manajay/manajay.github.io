---
layout: post
title: webView 的 iOS 与 js 交互
subtitle: iOS 与 js 交互的几种方式
author: manajay
date: 2016-05-31 16:17:13 +0800
categories: webView js
tag: web 
---

# 1. web 给 原生 传递数据
* web 网页的源代码

``` objc
var res = {"title":" 端午礼包提回去，丈母娘不满意来找我！！！","url":"http:\/\/app.xxx.com\/news\/rd\/content_wap_39865.shtml","image":"http:\/\/www.xxx.com\/v\/b\/images\/2016\/5\/30\/20165301464599344828_167.jpg","summary":" 端午礼包提回去，丈母娘不满意来找我！！！"};
function item_info(){
	return JSON.stringify(res);
}
try {
	window.share.shareData(JSON.stringify(res));
} catch(e) {
}
```

* 在 webView 的 - (void) webViewDidFinishLoad:(UIWebView *)webView 方法中 获取

```  objc
// 使用 stringByEvaluatingJavaScriptFromString 调用 js 方法
NSString *info = [webView stringByEvaluatingJavaScriptFromString:@"item_info()"];
NSDictionary *shareData = [NSJSONSerialization JSONObjectWithData:[info dataUsingEncoding:NSUnicodeStringEncoding] options:NSJSONReadingAllowFragments error:nil];
```

# 2. Web 调用 原生的功能 

主要是 使用 自定义协议头
在 web 中的按钮 或者 点击链接的中 写一个方法function, 然后调用 类似


``` objc

window.location.href='playvideo://tide?url=http://123.125.148.27:96/video/gesila.mp4';"

```
其中 playvideo:// 为自定义的协议头

然后原生代码中,使用 webView 的代理方法  即 重定义技术 

```
- (BOOL) webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType

 NSURL *url = request.URL;
    if ([url.scheme isEqualToString:@"playvideo"]) {
        NSArray *querys = [url.query componentsSeparatedByString:@"&"];
        NSString *videoUrl = nil;
        for (NSString *query in querys) {
            if ([query rangeOfString:@"url="].location != NSNotFound) {
                videoUrl = [query substringFromIndex:4];
                break;
            }
        }
使用 & 分割 想要传递的数据参数 就可以了
```
