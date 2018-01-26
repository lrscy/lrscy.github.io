---
layout: post
title: Ubuntu16.04下从零起步搭建配置github.io博客——Jekyll
date: 2017-04-30
tags: [Ubuntu, Github.io, Jekyll]
categories: [Config]

---

# 前言

本文旨在将我配置github.io博客全过程展现出来，帮助从零起步的小白们一步一步的配置属于自己的github.io博客。如果过程正哪一步骤错了，请各位大佬指出，谢谢～

小白的入门门槛：

需要耐心，耐心，耐心
碰到问题自主去学习和在网上寻找答案

本文是在Ubuntu 16.04LTS环境下配置的，如果使用其他操作系统请自行查找对应命令或解决方案。

以下代码区域，带有`$`打头的表示需要在控制台（终端或称命令行）下面执行（不包括`$`符号）

本文几乎所有命令都可以直接拷到控制台（终端或称命令行）内直接执行而不用理解其具体含义（除非特殊表明需要修改），但是强烈不建议这么做！！！

我是基于Jekyll模板搭建的博客，所以不会讲如何从零手敲出一个博客样式出来，但会比较详细的讲模板中哪里需要修改。

# 基础环境搭建

## Git/Github/SSH配置

详见我的博客「[Ubuntu16.04下Gtihub配置](/2017/05/01/Ubuntu-Github-config)」。

## 本地Jekyll环境配置

因为github.io博客是基于Jekyll模板生成的，所以需要了解下Jekyll模板。其实本地不配置Jekyll也是可以的，不过后果就是无法进行本地预览。提交到Github上平均需要10分钟才能看到更改结果，所以我是建议本地配置下Jekyll的。

### Ruby安装配置

Jekyll是基于ruby的，所以要安装Jekyll还需要安装ruby, gem等。不过不要担心，我已经把雷都踩过了，你只需要跟着我一步步走就好。不过，我们不需要把Rails环境配上，因为我们暂时用不到。

详见我的博客「[Ubuntu16.04下Ruby基础配置](/2017/05/01/Ubuntu-Ruby-base-config)」。

### Jekyll安装配置

由于上一步已经安装好了Ruby和gem了，所以只需要下面一条命令就可以安上Jekyll：
``` bash
$ gem install jekyll
```
没了，嗯～

真的没骗你～喵～

至此基础环境搭建也就完成了。

# Jekyll目录介绍

我会对Jekyll目录进行一个简单粗略的介绍，让你知道每个目录大概都是做什么的，便于你以后查找需要修改的文件的位置。

Note: 我的博客是基于其他作者些的Jekyll主题改的，所以以下部分所述“不需要修改”皆出于此出发点。对于有前端基础的同学请自行忽略。

下面是目录树：
``` bash
blog
  _includes
    footer.html
    head.html
    header.html
  _layouts
    default.html
    page.html
    post.html
  _posts
    2017-04-30-Hello.md
  _sass
  _site
  css
    main.scss
  .gitignore
  .sass-cache
  _config.yml
  about.md
  feed.xml
  index.html
```

## \_include

这里都是网页模块文件，用来加载到你的布局或文章中。以后可能需要修改部分内容。
可以在其他文件中采用如下方法调用该文件夹内文件：
```
{ % include file %}
```
Note: 调用时候把{和%之间的空格去掉

## \_layouts

存放网页模板。每个网页只需要关注自己的内容。也基本上不用修改。

## \_posts

这里就是存放我们博文的地方了。文件名称非常非常关键，必须使用统一的格式：`YEAR-MONTH-DAY-TITLE.md`且中间不能有空格。例如：`2017-04-30-Hello.md`。不是此格式的博文不会被解析也不会在网站中显示。

## \_sass

存放网站用到的sass文件。基本上你不用管这里面做了些什么。

## \_site

在Jekyll解析完成这些文件后，会将最终的静态网站源代码默认的放在这个文件夹下来保存。这个文件夹最好添加进.gitignore文件中（这个问题我们后面再说）。

## css

存放网页样式文件。基本上也不用管。

## .sass-cache

sass的编译缓存文件。基本上你不用管这里面都是什么。

## \_config.yml

最重要的配置文件，这里面决定了Jekyll如何解析网站源代码。官方有给出配置文件详细信息，详情请见[这里](http://jekyll.com.cn/docs/configuration/)。

## \_plugins

你可能需要这个文件夹也可能不需要，按需建立此文件夹。这里用来存放Jekyll插件。

# 正式开始搭建博客

终于开始搭博客了喵～

## Github仓库建立

本节是在复述[官方教程](https://pages.github.com/)中的内容。

### 建立一个仓库

你需要在你的Github中建立一个新的仓库，名字必须是`{your github username}.github.io`，否则Github不会将其认为是Github博客。

### 克隆到本地

采用下述命令进行复制
``` bash
$ git clone https://github.com/username/username.github.io
```
这里的username是你的github用户名

至此，Github部分完结

## Jekyll模板

我的博客是基于Pithy主题建立的。Jekyll模板可以从Jekyll Theme上或大佬们的Github上下载。下载解压后直接将所有文件都拷贝到自己刚克隆下来的文件夹下即可。

### \_config.yml

解压后唯一需要修改的部分，修改其中所有你认识的英文，如果没有可以填的空着就好。

接下来你需要做的就是将其push到远程仓库里。如果你不熟悉Git操作命令，我强烈建议你去看下[廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。

## 添加.gitignore文件

上文介绍jekyll结构的时候说了句\_site要加入.gitignore文件中。.gitignore文件是git上传时要过滤掉的文件。依据自己需求改下就可以了。我目前的.gitignore文件如下：
``` bash
_site/*
.sass-cache
.jekyll-metadata
.swp
```
搭完了，嗯，你没看错，搭完了喵～

## 添加Mathjax支持

作为一个技术小白，就算再小白也有需要用到数学公式的时候。这时mathjax能满足你的大部分需求。Mathjax配置很简单，过程如下：

在`_include/head.html`中添加以下代码：
``` html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    TeX: { equationNumbers: { autoNumber: "AMS" } },
    tex2jax: {
      inlineMath: [ ['$','$'], ['\\(', '\\)'] ],
      displayMath: [ ['$$','$$'] ],
      processEscapes: true,
    }
  });
</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>
```
最后一行那里的src，有些博客包括官方教程些的都是`http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML`，然而在https下此网址被认为是不安全的而一些浏览器被屏蔽了（比如Chrome），所以需要换成这个网址。

然后你需要检查下`_config.yml`中是否有如下一行：
```
markdown: kramdown
```
如果没有添加上即可。这是在指定markdown的解释器，如果你想换成其他的也可以。

至此，Mathjax配置完成了。用在行内用`$`来包裹latex公式，行间公式需要用`$$`包裹住。例子如下：
```
# 第一种
blabla$x$blabla
# 第二种
$$
P(y|x)
$$
```

## 添加代码高亮

我选择的是Jekyll原生支持的rouge进行代码高亮。只需要在\_config.yml中添加一行：
``` bash
highlighter: rouge
```
即可高亮代码。

# 最后的检测

现在你已经配好了所有功能，在git仓库的根目录下运行`jekyll serve`即可以迅速在本地生成博客。通过浏览器访问`localhost:4000`即可看到成果啦喵～

最后的最后记得push到你Github的远程仓库中，然后就可以在网页上看到你自己的博客了。
