---
layout: post
title:  Git 的常用命令
tag: Git
date: 2018-03-15 18:34:20 +09:00
---

## tag 操作

* `git tag` 列出本地的tag 
* `git push --tags` 推送本地tag到远程服务器
* `git tag -d xxx` 然后 `git push origin :refs/tags/xxx` 两步操作将远程服务器的某个tag删除

## 其他

* `git status` 查看当前的本地仓库状态 
*  `git pull origin master` 从远程库拉取最新的文件到本地库,防止 直接推送报错
*  `git add .` 添加 当前目录 所有的 未保存文件 到本地库
*  `git commit -m "注释内容"` 将刚才 添加到本地库的文件 这一系列行为 的 注释添加上
*  `git push origin master` 推送本地库的修改文件到远程库
* `git stash` 将本地的修改内容添加到 本地库的暂存区,主要用于 拉取远程文件时出现冲突 而不能拉取,但又不想将修改的内容添加到本地库的问题
* `git stash pop` 将本地库暂存区的文件 再释放出来,继续编辑
* `git branch -a` 列出本地所有的分支,a 就是 all
* `git checkout 分支名称` 根据分支名,切换分支 ,如果没有则 切换失败
* `git checkout -b 分支名称` 根据分支名称 切换分支, 如果没有就创建一个



