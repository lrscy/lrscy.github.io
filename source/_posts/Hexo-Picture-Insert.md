---
layout: post
title: Hexo中图片插入问题
date: 2018-01-26 09:59:05
tags: [Hexo, Markdown]
categories: [Config]

---

# 前言

在用Hexo搭建博客时有时需要插入图片，但是原生Hexo对图片管理的支持不是很好。此篇博客记录利用插件和Hexo的配置解决这个问题。

# \_config.yml配置更改

在Hexo根目录下，`_config.yml`文件管理整个Hexo的配置设置。其中要开启`post_asset_folder`，即：
``` yml
post_asset_folder: true
```
更改完成后，每新生成一片文章，就会在同级目录下生成一个名字相同的相对应的文件夹。图片存在该文件夹下即可。

# 安装插件

由于原生Hexo资源文件夹在生成真正博客时地址转换有问题，需要安装插件进行修正。执行如下命令安装插件：
``` bash
npm install https://github.com/CodeFalling/hexo-asset-image --save
```
当安装完成后就可以在写Markdown时很容易的使用资源文件夹下的图片了。

# 使用教程

在插入图片时只要使用如下Markdown语法即可
``` markdown
![](文章名字/图片名字.后缀)
```
不知道我是不是因为在`_config.yml`中开启了`relative_link`，我采用如下方式插入图片：
``` markdown
![](图片名字.后缀)
```
这点以后再探究好了喵～
