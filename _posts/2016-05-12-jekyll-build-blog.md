---
layout: post
title: jekyll打造自https://manajay己的博客 
tag: [jekyll, 博客]
date: 2016-05-12 22:57:24 +09:00
---

IT人必须弄个自己的博客
我选择的是基于`Github`上的`github-pages`, 使用的是`Jekyll`构建静态博客.

步骤与关注点有
* `github-pages`: github创建同名的代码仓库
* [jekyll模板](http://jekyllthemes.org/)下载
* `mac`安装`ruby`, 注意版本号
* `mac`安装模板文件`gemfile`中列出的`gem`库, 注意版本号
* `jekyll`的使用
* `github`源码的管理

## github-pages
博客是架设在`github`上,基于`github-pages`,所以首先是建议一个`github-pages`的项目. 
[官方参考教程](https://pages.github.com/)

* 前往 [GitHub](https://github.com/), 创建一个仓库,名称为 `username.github.io`, 这里**username** 是你在GitHub上的用户名或者组织名称.


## jekyll模板

* 前往 [jekyll模板](http://jekyllthemes.org/)

![jekyllthemes](/assets/post/jekyllthemes.png)

进入详情页,下载模板源码,如图
![](/assets/post/download-jekyll-theme.png)

源码如下:

```
.
├── Gemfile
├── Gemfile.lock
├── LICENSE.txt
├── README.md
├── Rakefile
├── Screenshots
│   ├── hanuman-1.jpg
│   ├── hanuman-2.jpg
│   ├── hanuman-3.jpg
│   ├── hanuman-4.jpg
│   └── hanuman.jpg
├── _config.yml
├── _data
│   ├── author.yml
│   └── navigation.yml
├── _includes
│   ├── analytics.html
│   ├── footer.html
│   ├── head.html
│   ├── header.html
│   ├── metadata.json
│   └── styles.scss
├── _layouts
│   ├── default.html
│   ├── page.html
│   └── post.html
├── _posts
│   ├── 2017-11-27-media.md
│   ├── 2017-11-28-code.md
│   ├── 2017-11-29-sample.md
│   └── 2017-11-30-style-guide.md
├── _sass
│   ├── _syntax-highlighting.scss
│   └── bourbon
│       ├── _bourbon-deprecated-upcoming.scss
│       ├── _bourbon.scss
│       ├── addons
│       │   ├── _button.scss
│       │   ├── _clearfix.scss
│       │   ├── _directional-values.scss
│       │   ├── _ellipsis.scss
│       │   ├── _font-family.scss
│       │   ├── _hide-text.scss
│       │   ├── _html5-input-types.scss
│       │   ├── _position.scss
│       │   ├── _prefixer.scss
│       │   ├── _retina-image.scss
│       │   ├── _size.scss
│       │   ├── _timing-functions.scss
│       │   ├── _triangle.scss
│       │   └── _word-wrap.scss
│       ├── css3
│       │   ├── _animation.scss
│       │   ├── _appearance.scss
│       │   ├── _backface-visibility.scss
│       │   ├── _background-image.scss
│       │   ├── _background.scss
│       │   ├── _border-image.scss
│       │   ├── _border-radius.scss
│       │   ├── _box-sizing.scss
│       │   ├── _calc.scss
│       │   ├── _columns.scss
│       │   ├── _filter.scss
│       │   ├── _flex-box.scss
│       │   ├── _font-face.scss
│       │   ├── _font-feature-settings.scss
│       │   ├── _hidpi-media-query.scss
│       │   ├── _hyphens.scss
│       │   ├── _image-rendering.scss
│       │   ├── _keyframes.scss
│       │   ├── _linear-gradient.scss
│       │   ├── _perspective.scss
│       │   ├── _placeholder.scss
│       │   ├── _radial-gradient.scss
│       │   ├── _transform.scss
│       │   ├── _transition.scss
│       │   └── _user-select.scss
│       ├── functions
│       │   ├── _assign.scss
│       │   ├── _color-lightness.scss
│       │   ├── _flex-grid.scss
│       │   ├── _golden-ratio.scss
│       │   ├── _grid-width.scss
│       │   ├── _modular-scale.scss
│       │   ├── _px-to-em.scss
│       │   ├── _px-to-rem.scss
│       │   ├── _strip-units.scss
│       │   ├── _tint-shade.scss
│       │   ├── _transition-property-name.scss
│       │   └── _unpack.scss
│       ├── helpers
│       │   ├── _convert-units.scss
│       │   ├── _gradient-positions-parser.scss
│       │   ├── _is-num.scss
│       │   ├── _linear-angle-parser.scss
│       │   ├── _linear-gradient-parser.scss
│       │   ├── _linear-positions-parser.scss
│       │   ├── _linear-side-corner-parser.scss
│       │   ├── _radial-arg-parser.scss
│       │   ├── _radial-gradient-parser.scss
│       │   ├── _radial-positions-parser.scss
│       │   ├── _render-gradients.scss
│       │   ├── _shape-size-stripper.scss
│       │   └── _str-to-num.scss
│       └── settings
│           ├── _asset-pipeline.scss
│           ├── _prefixer.scss
│           └── _px-to-em.scss
├── about.md
├── assets
│   └── images
│       ├── favicon.png
│       ├── hanuman.png
│       ├── logo.png
│       ├── radhakrishna.jpg
│       ├── shiva.jpg
│       └── tree.jpg
├── feed.xml
├── hanuman-0.1.0.gem
├── hanuman-0.2.0.gem
├── hanuman.gemspec
└── index.html
```

## ruby安装

jekyll等一系列工具都是基于ruby的,所以首先要安装ruby环境.虽然Mac自带了`ruby`,但是因为版本过低,所以还是要使用高些的版本才可以安装并运行`jekyll`
参考文章: [Mac上Ruby的管理](https://manajay.com/2017/01/mac-ruby-install/)

## gemfile插件安装

ruby环境安装成功后, 就需要安装 **Gemfile**文件内的 `gem`依赖, 类似于`iOS`项目依赖管理工具`cocoapods`的依赖文件**podfile**. 

范例中的`Gemfile`文件内的依赖有如下几个

```
source 'https://rubygems.org'

gem "jekyll", "~> 3.6.2"
gem 'jekyll-compose', group: [:jekyll_plugins]
gem "github-pages", "~> 168"
gem "rake", "~> 12.3.0"
gem 'jekyll-paginate'
```

所以需要执行命令

``` ruby
gem install jekyll --version=3.6.2
gem install github-pages --version=168
gem install rake --version=12.3.0
gem install jekyll-paginate
gem install jekyll-compose
```
安装好需要的`gem`插件. 

这里需要注意的是`gem` 的官方源`https://rubygems.org/`被墙,无法正常更新,需要切换到`https://gems.ruby-china.org/` 这个`ruby-china`的替代源.
不要使用淘宝的源<已停止更新>了

所以要更换gem源
```
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
```

最后查看当前的源, 是不是已经替换成`https://gems.ruby-china.org/`
```
gem sources -l
```

* `gem`依赖问题

本人建立的博客使用的是`onevcat`大神的模板,下载模板的时候,会有一个`Gemfile.lock`文件,里面指定了所依赖的`gem`版本号.

如果你是`iOS开发者`,应该知道`Cocoapods`的`podfile.lock`文件了

gem安装ruby的应用,默认的版本都是最新的. 那些没有安装的依赖,需要手动`gem install gem-name --version=xxx`,一般是按照`Gemfile`的版本安装,如果你认为自己安装了, 运行`jekyll`却报错没有相关的依赖,很大的可能是因为版本问题导致的.

最简单的方式是,删除`Gemfile.lock`,再次`jekyll serve`运行,基本就可以了.

![jekyll-serve](/assets/post/jekyll-serve.png)

打开serve地址(http://127.0.0.1:4000)就可以看到博客的首页

## jekyll的使用

[jekyll官方教程](https://jekyllrb.com/docs/usage/)

* `_config.yml` 文件主要用于配置博客的架构, 名称,头像, url地址等信息

* `_post`目录, 里面放的是**markdown**语法的`md`文件,`jekyll`运行时就会将这些`md`文件按照一定的格式生成`html`文件,并放在`_site`目录下.
* `assets`目录. 放置文章中的图片
* `_includes` 放置博文页面的各部分头尾等`html`
* `_layouts`目录 放置 博文的页面主题结构`html`
* `_sass`目录 放置用于页面布局的`css`

套用别人的模板, 要清楚别人的页面结构,做好 `_config.yml`个人信息配置, 然后就可以使用 `markdown`语法, 开心的写博客了.

推荐`markdown`的mac客户端 [MWeb](http://zh.mweb.im/index.html)

## github源码的管理

* 源码管理用 `Git`

相关的语法学习 [廖雪峰-Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 参考文章

* [翻墙](https://manajay.github.io/2016/06/vpn-google/)

* [jekyll官方安装教程](http://jekyllcn.com/docs/installation/)

