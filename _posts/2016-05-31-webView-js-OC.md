---
layout: post
title: webView 的 iOS 与 js 交互
subtitle: iOS 与 js 交互的几种方式
author: manajay
date: 2016-05-31 16:17:13 +0800
categories: webView js
tag: js 
---

# 1. js 代码1
```
var res = {"title":" 端午礼包提回去，丈母娘不满意来找我！！！","url":"http:\/\/app.qguiyang.com\/news\/rd\/content_wap_39865.shtml","image":"http:\/\/www.qguiyang.com\/v\/b\/images\/2016\/5\/30\/20165301464599344828_167.jpg","summary":" 端午礼包提回去，丈母娘不满意来找我！！！"};
function item_info(){
	return JSON.stringify(res);
}
try {
	window.share.shareData(JSON.stringify(res));
} catch(e) {
}
```
