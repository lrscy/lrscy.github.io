---
layout: post
title: Ubuntu 16.04下Github配置
date: 2017-05-01
tags: [Ubuntu, Github, SSH]
categories: [Config]

---

# 前言

本文记录了我配置Github时候的全过程。

本文是在Ubuntu 16.04LTS下配置的，如果使用其他操作系统请自行查找对应命令或解决方案。

以下代码区域，带有`$`打头的表示需要在控制台（终端或称命令行）下面执行（不包括`$`符号）

# Git安装

Ubuntu下需要安装git，输入命令如下：
``` bash
$ sudo apt-get install git
```

# Github账户注册

你需要一个Github的账户才能使用github.io服务。所以去`github.com`点击右上角的`Sign Up`注册即可。

# SSH配置
我们需要使用ssh来和github上的远程仓库进行通信，所以需要检查和配置ssh。

首先需要检查电脑上现有的ssh key:
``` bash
$ cd ~/.ssh
```
如果提示：`No such file or dictionary` 则说明你是第一次使用git。

如果你是第一次使用git，那么你需要做如下工作。否则可以跳过此节。

生成新的SSH KEY:
``` bash
$ ssh-keygen -t rsa -C "youraddress@youremail.com"
```
这里有几点需要注意的：
1. `-C`中的C是大写字母
2. `youraddress@youremail.com`是你Github的注册邮箱
其余的一路回车就能完成了。其中.pub是公钥，需要给到远程主机上，这点我们下一节再讲。

如果你需要连接多ssh终端，详见「[Ubuntu16.04下(多)SSH配置](/2017/05/02/Ubuntu-SSH-config)」。

# Github配置

仅仅本地配置好了是不够的，你需要让你的github账户认识你。所以你需要按照以下步骤操作：
1. 去登录github账户，在右上角点击头像找到Settings
2. 点进去后点击左侧栏中的SSH and GPG keys
3. 点击右侧New SSH key
4. 随意输入个能带便当前机器的名字，并把本地.ssh目录下生成的关于github的.pub文件拷贝进来
5. 保存即可

# Github连接检查

当一切配置妥当后，在终端中输入
``` bash
$ ssh -T git@github.com
```
如果你是第一次输入此命令，可能遇到如下提示：
``` bash
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is xx:xx...xx:xx.
Are you sure you want to continue connecting (yes/no)?
```
此时输入yes即可，然后会出现：
``` bash
Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```
此时表明你的github配置完成了

# 本地个人信息配置

此时你已经可以通过SSH连接到Github了，但是还有一些个人信息需要完善才能够和Github愉快的通信。

Git会依据本地设定的用户名和邮箱向远程主机提交更改，Github也是依据这些信息进行权限管理的。如果你当前只使用一个Github帐号，那么你需要以下两个命令来完善本地个人信息设定：
``` bash
$ git config --global user.name "your github name"
$ git config --global user.email "youraddress@youremail.com"
```
上述命令中your github name是你的github用户名，youraddress@youremail.com是你github的注册邮箱。

如果你需要配置多Github账户，则需要用如下命令将–global所设置的参数撤销掉：
``` bash
$ git config --global unset user.name
$ git config --global unset user.email
```
在每个git根目录下自行建立个人信息，命令如下：
``` bash
$ git config user.name "your github name"
$ git config user.email "youraddress@youremail.com"
```
上述命令中`your github name`是你的github用户名，`youraddress@youremail.com`是你github的注册邮箱。

至此，Github配置已全部完成。
