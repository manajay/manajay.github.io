---
layout: post
title: python 构建博客
date: 2017-10-25 14:33:30 +0300 
tags: [Python] 
---

### 构建的环境 架构

* Django 4
* python 3
* sqlite3
* pycharm


### Django 的项目结构

一般创建的项目,初始化目录为

``` 

├── Blog
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-36.pyc
│   │   └── settings.cpython-36.pyc
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── appmodule
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── manage.py
└── templates

```

其中**Blog**目录下为项目的配置文件

* settings.py 设置信息
* urls.py 网站的url管理文件
* wsgi.py 兼容wsgi的文件

**appmodule**文件目录为项目的应用模块,一般对于一个网站,会有多个模块,那么每个模块都会对应一个`appmodule` 

* models.py 模型文件,对应sqlite数据库或者Mysql数据库
* views.py 视图文件, MVC 模式下的V


> manage.py 为项目的工具文件,django的很多功能都是使用这个文件执行得到的
> templates 模板文件,前端的html等文件


### 模型 - 数据库
数据库迁移的命令 - 用于模型更改好,同步表结构

```
python manage.py makemigrations
python manage.py migrate
```



