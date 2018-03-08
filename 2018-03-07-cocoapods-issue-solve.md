---
layout: post
title:  Cocoapods 的常见问题与警告
tag: Cocoapods
date: 2018-03-03 10:34:20 +09:00
---

### TextEdit编辑Podfile后的警告

```
[!] Smart quotes were detected and ignored in your Podfile. 
To avoid issues in the future, you should not use TextEdit for editing it. 
If you are not using TextEdit, you should turn off smart quotes in your editor of choice.
```

一般是因为引入了错误的标点符号

比如推荐 `''` 而不是 `‘ ’`

