# 项目中常见的 error 及 解决方案

## 1. 集成SSZipArchive
```
 error: include of non-modular header inside framework module 'SSZipArchive.ioapi
 could not build module 'SSZipArchive'
```

**Answer:**
Set "Allow Non-modular includes" in your project settings
![](/source/14719329182701.png)



