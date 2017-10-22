---
layout: post
title: python 语法警告
date: 2017-10-21 19:59:36 +0300 
tags: [Warning,Python] 
---

## 语法的指导规则是 `PEP 8`

### 异常捕获太宽泛

```
too broad exception clause
```

当你使用 `try: exception:` 捕获异常时,最好明确指出这是哪种异常;

```
import logging


try: 
    pass
except Exception as e:
    logging.exception(e) 
```

如果确实要忽略这个问题, 在`try:` 前一行 添加 `# noinspection PyBroadException`

```
# noinspection PyBroadException
try: 
    pass
exception:
    pass
```

### 作用域内外变量名重名 

下面书写就会出先警告`Shadows name 'root_url' from outer scope`

```python

 def crawl(self, root_url):
        # 限制抓取的链接数
        count = 1
        self.urls.add_new_url(root_url)


# 在同一个文件中 调用此函数
root_url = 'www.baidu.com'
crawl(root_url)
```

解决办法:

```python

 def crawl(self, root_url):
        # 限制抓取的链接数
        count = 1
        self.urls.add_new_url(root_url)


# 在同一个文件中 调用此函数, 不要使用同名变量,改用其他变量 url
url = 'www.baidu.com'
crawl(url)
```


### Method xxx may be 'static'

方法可能是静态方法;
建议的愿意是,这个方法, 没有使用到self中的属性

```python
class HtmlDownloader(object):

    @staticmethod
    def download(url):
        if url is None:
            return None

        res = urllib2.urlopen(url)
        if res.getcode() != 200:
            return None
        return res.read()
```

### This dictionary creation could be rewritten as a dictionary literal.

字面量字典可能会被覆盖

``` python
dic = {}
dic['name'] = 'jack'
```

解决办法: 

``` python
dic = dict()
dic['name'] = 'jack'
```

### unexpected spaces around keyword / parameter equals

关键字参数和默认值参数的前后不要加空格

`class_` 是关键字,在它附近的等号不要有空格


```python
title_node = soup.find('dd', class_="lemmaWgt-lemmaTitle-title").find('h1')
```


### blank line at end of file 

文件最后多了一个空白行

python文本的最后只有一个空白行.


### 'ascii' codec can't encode characters in position 

问题是 字符串编码异常 
我们知道字符串有两种表示方式:

* utf-8 字符串 也就是 str 类型
* unicode 字符串

而对于编码则有

* ascii
* gbk

不同的字符串 对于编码的要求如下:


``` python
# 用 ascii 编码含中文的 unicode 字符串  
u.encode('ascii')  # 错误，因为中文无法用 ascii 字符集编码  
                   # UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-3: ordinal not in range(128)  
  
# 用 gbk 编码含中文的 unicode 字符串  
u.encode('gbk')  # 正确，因为 '关关雎鸠' 可以用中文 gbk 字符集表示  
                 # '\xb9\xd8\xb9\xd8\xf6\xc2\xf0\xaf'  
                 # 直接 print 上面的 str 会显示乱码，修改环境变量为 zh_CN.GBK 可以看到结果是对的  
  
# 用 ascii 解码 utf-8 字符串  
s.decode('ascii')  # 错误，中文 utf-8 字符无法用 ascii 解码  
                   # UnicodeDecodeError: 'ascii' codec can't decode byte 0xe5 in position 0: ordinal not in range(128)  
  
# 用 gbk 解码 utf-8 字符串  
s.decode('gbk')  # 不出错，但是用 gbk 解码 utf-8 字符流的结果，显然只是乱码  
                 # u'\u934f\u51b2\u53e7\u95c6\u5ea8\u7b2d'  
```

以上摘自博客[也谈 Python 的中文编码处理](http://in355hz.iteye.com/blog/1860787)

所以对于中文编码如果出现上述问题,调整编码策略即可.




