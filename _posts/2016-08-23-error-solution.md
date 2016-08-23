---
layout: post
title: 项目中常见的 error 及 解决方案
date: 2016-08-23 14:15:32.000000000 +09:00
---

## 1. 集成SSZipArchive
```
 error: include of non-modular header inside framework module 'SSZipArchive.ioapi
 could not build module 'SSZipArchive'
```

**Answer:**
Set "Allow Non-modular includes" in your project settings
![](/source/14719329182701.png)



