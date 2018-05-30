---
layout: post
title: Ubuntu 16.04下从零起步搭建配置github.io博客——Hexo
date: 2017-11-10
tags: [Ubuntu, Github.io, Hexo]
categories: [Config]

---

# 前言

本文利用Github io和Hexo搭建静态博客。主题更换等问题请到Hexo Theme里寻找并替换。

继上次用Jekyll搭建博客后，又忙了很多其他事情，接触到了Hexo。因此决定将博客从Jekyll换到Hexo。

本人是个前端小白，按照网上众多教程搭建时候依旧踩了很多坑，在此记录下来以便有相同问题的同学可以快速解决。

搭建Github io静态博客所需基础之基础的知识如下：
- [Git](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [Github](https://github.com/)
- [Markdown](http://wowubuntu.com/markdown/)
- [Hexo](https://hexo.io/zh-cn/index.html)

本文是在Ubuntu 17.04环境下配置的，如果使用其他操作系统请自行查找对应命令或者解决方案。

以下代码区域，带有`$`打头的表示需要在控制台（终端或称命令行）下面执行（不包括`$`符号）。如果出现权限不足提示请在命令最前面加上`sudo`再执行。

本文几乎所有命令都可以直接拷到控制台（终端或称命令行）内直接执行而不用理解其具体含义（除非特殊表明需要修改），但是强烈不建议这么做！！！

我是基于Hexo模板搭建的博客，所以不会讲如何从零手敲出一个博客样式出来，但会比较详细的讲模板中哪里需要修改。

# 基础环境搭建

## Git/Github/SSH配置

详见我的博客「[Ubuntu16.04下Github配置](/2017/05/01/Ubuntu-Github-config)」。

## Node.js安装

安装Hexo前需要安装Node.js，本人安装的是Node.js 8。

对于Ubuntu系列系统，执行以下两个命令：
``` bash
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt install -y nodejs
```
如果系统中没有安装`curl`，执行下述命令安装：
``` bash
$ sudo apt install curl
```
安装完成后需要检查下`node`和`npm`的版本：
``` bash
$ node -v
$ npm -v
```
分别显示出版本号就算安装完成了～
本站搭建时`node`版本为`v8.9.1`，`npm`版本为`5.5.1`。

## 参考资料

[Node.js官网](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)。

# Hexo本地建站

## Hexo安装

Hexo安装非常简单，上述环境搭建好后只需执行以下命令即可：
``` bash
$ npm install -g hexo-cli
```
本站搭建时Hexo的版本是3.4.0。

## Hexo本地建站

首先通过终端进入希望建站的文件夹内（例如`~/Hexo`），执行以下命令：
``` bash
$ hexo init
```
该命令要求建站文件夹是全空的文件夹。如果之前在该文件夹内建立了git等文件（夹），请先移出文件夹，建站完成后再移回来。

下述命令会在该文件夹下建立所有需要的文件。接下来安装依赖包：
``` bash
$ npm install
```
至此，Hexo本地博客已经搭建完成。对，你没看错～

然后执行以下命令来浏览本地站点
``` bash
$ hexo generate
$ hexo server
```
- `hexo generate`是用来编译生成站点，每次对站点内容编辑后都要进行该项操作。可以简化为`hexo g`。
- `hexo server`是用来启动本地站点，执行后即可在浏览器中输入localhost:4000查看。可以简化为`hexo s`。

## 参考资料

[Hexo 官方文档](https://hexo.io/zh-cn/docs/)

# Github io部署博客

## \_config.yml参数设置

部署配置在\_config.yml文件的末尾，默认样子如下：
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
type:
repo:
```
修改后如下：
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
type: git
repo: http://github.com/xxx/xxx.github.io.git(xxx是Github账户名称)
branch: master
```
- 由于是部署到Github中，所以`type`是`git`。
- `repo`是指Github对应仓库的SSH地址。点击该仓库页面右侧绿色download，里面的地址就是`SSH`地址。
- `branch`是指上传到Github的哪个分支，如果没特殊需求选择`master`就可以。特殊需求请自行填写上传哪个branch。

## 插件安装

为了部署到Github上，需要安装hexo-deployer-git插件，命令如下：
``` bash
$ npm install hexo-deployer-git --save
```

## 最终部署

最终部署需要输入以下两个命令：
``` bash
$ hexo generate
$ hexo deploy
```
- `hexo generate`同上。
- `hexo deploy`将hexo部署到Github io上。可以简化为`hexo d`。

上传后需要等待几分钟，然后就可以在浏览器中输入`xxx.github.io`来欣赏了喵～

## 参考资料

[Hexo 部署](https://hexo.io/zh-cn/docs/deployment.html)
