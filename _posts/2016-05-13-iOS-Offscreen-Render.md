---
layout: post
title: iOS离屏渲染
tag: iOS
date: 2016-05-14 23:14:30 +09:00
---

[离屏渲染obj中国]: http://objccn.io/issue-3-1/

#### 文章内容主要来自 [seedante]

[seedante]: http://www.jianshu.com/users/7ba5d9065301/latest_articles

## 1.什么是离屏渲染

obj 中国有一篇文章专门提到了离屏渲染的问题,文章中提到⎡直接将图层合成到当前显示屏幕的帧缓冲区中,比先在屏幕外面创建新的缓冲区,然后渲染到纹理中,最后将结果渲染到当前显示屏幕的帧缓冲区中,性能要好的多⎦  

* 主要原因是环境切换 * 转换环境到屏幕外缓冲区* 转换屏幕到帧缓冲区*
* 触发屏幕渲染后 这种情况发生在 每一帧 ,界面如果在滚动过程中 有大量的离屏渲染发生会严重影响帧率

## 2. 影响离屏渲染的原因

* GPU版本的离屏渲染 ->
更改 mask,shadow,Group opacity,edge antialiasing 
* CPU 版本的离屏渲染 -> 
使用 Core Graphic 里面的绘制 API 也会触发离屏渲染 ,比如 drawRect:

* 系统圆角的离屏渲染

```
view.layer.cornerRadius = aRadius;
view.layer.maskToBounds = true 
```

## 3. 圆角解决方案 

* 异步绘制圆角
* 本地图片 让 UI 提供圆角图片
* 使用遮罩 ,边缘让背景色 和 周边颜色 保持一致 ;中间为透明色



## 4. 本文中涉及的基础知识

### 4.1  UIView 与 CALayer 的关系

* CALayer 负责显示内容 contents 
* UIView 负责为其提供内容,以及负责处理触摸事件,参与响应链
* CALayers have their own background , border and contents
* contents 必须为一个 CGImage 才能显示

### 4.2 UIImageView 

* UIImage 是对CGImage 的一个轻量的封装 (UIImage is a lightweight wrapper around CGImage )
* CALayer also has CGImage as contents
* CGImage backed by file or data,eventually by bitmap

### 4.3 RoundedCorner 

* cornerRadius 的英文说明
* Setting the radius to a value greater than 0.0 causes the layer to begin drawing rounded corners on its background. By default,the corner radius does not apply to the image in the layer's contents property ;it applies only to the background color and border of the layer . However, setting the masksToBounds property to YES causes the content to be clipped to the rounded corners.
* 只对前景框和背景色起作用

### 4.4 重绘方案的优化

* 将图像重新绘制为圆角图像相当于多了一份,要不要缓存? 
* A 第一次重绘后将这些圆角图像缓存在磁盘里,第二次加载直接使用缓存的圆角图像;
* B 直接保存在内存里,在内存比较吃紧时显然不是个好选择
* C 不缓存,和系统圆角一样,每次都重绘,浪费电量 

### 4.5 文本视图类上实现圆角 

* UTextField 自带圆角效果
* UILabel  UITextView 首先保证contents 呈现透明的背景色 ,只需要设置 layer 的 backgroundColor ,再加上 cornerRadius 就 OK

* 阴影设置 shadowPath ,默认为 nil




