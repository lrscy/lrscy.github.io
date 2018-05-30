---
layout: post
title: 在Github上备份Hexo博客
date: 2018-01-26 12:57:56
tags: [Hexo, Github]
categories: [Config]

---

# 前言

由于之前忘记备份Hexo博客的markdown文件，在重做系统时候还忘记备份博客了，导致现在不得不重新从网页上扒下来之前的文章重新写一遍，十分耗费精力。因此在网上找了下如何备份Hexo博客，在此记录下。

目前假设Git和Github环境已经配置好了，如果没有配置好详见「[Ubuntu16.04下Github配置](/2017/05/01/Ubuntu-Github-config)」。
Git相关操作请参考[廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。

# 备份博客

目前假设本地Hexo博客已经初始化，如果没有配置好Hexo博客详见「[Ubuntu16.04下从零起步搭建配置github.io博客————Hexo](/2017/11/10/Ubuntu-Github-io-config-Hexo)」。

## 创建新分支

在Github.io上建立博客时已经开了一个新仓库了，如果再开另一个仓库存放源代码有点浪费，因此采用建立新分支的方法备份博客。

虽然理论上什么时候创建新分支来备份都可以，但是还是建议在建立博客的时候就创建备份分支。（然而我中途才想起来-.-）

本地Git建立新分支命令如下：
``` bash
$ git checkout -b BRANCHNAME
```
`BRANCHNAME`是自定义的新分支的名字，建议起为`hexo`。

## 建立.gitignore

建立`.gitignore`文件将不需要备份的文件屏蔽。个人的`.gitignore`文件如下：
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

## 在Github上备份

通过如下命令将本地文件备份到Github上。

假设目前在hexo博客的根目录下。
``` bash
$ git add .
$ git commit -m "Backup"
$ git push origin hexo
```
这样就备份完博客了且在Github上能看到两个分支(`master`和`hexo`)。

## 设置默认分支

在Github上你的github.io仓库中设置默认分支为`hexo`。这样有助于之后恢复博客。`master`分支时默认的博客静态页面分支，在之后恢复博客的时候并不需要。

## 个人备份习惯

个人而言习惯于先备份文件再生成博客。即先执行`git add .`,`git commit -m "Backup"`,`git push origin hexo`将博客备份完成，然后执行`hexo g -d`发布博客。

# 恢复博客

目前假设本地Hexo博客基础环境已经搭好，如果没有配置好Hexo博客基础环境详见「[Ubuntu16.04下从零起步搭建配置github.io博客————Hexo](/2017/11/10/Ubuntu-Github-io-config-Hexo#基础环境搭建)」。

## 克隆项目到本地

输入下列命令克隆博客必须文件(`hexo`分支)：
``` bash
$ git clone https://github.com/yourgithubname/yourgithubname.github.io
```

## 恢复博客

在克隆的那个文件夹下输入如下命令恢复博客：
``` bash
$ npm install hexo-cli
$ npm install
$ npm install hexo-deployer-git
```
在此不需要执行`hexo init`这条指令，因为不是从零搭建起新博客。

完成喵～
