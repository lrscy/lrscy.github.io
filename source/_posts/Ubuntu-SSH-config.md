---
layout: post
title: Ubuntu16.04下(多)SSH配置
date: 2017-05-02
tags: [Ubuntu, SSH]
categories: [Config]

---

# 前沿

本文记录了我配置同机多SSH时候的全过程。
本文是在Ubuntu 16.04 LTS下配置的，如果使用其他操作系统请自行查找对应命令或解决方案。
以下代码区域，带有`$`打头的表示需要在控制台（终端或称命令行）下面执行（不包括`$`符号）

# SSH安装

Ubuntu 16.04 LTS自带openssh客户端，但是不带openssh服务器端。如果需要`ssh localhost`连接本地，那么需要安装`openssh-server`，命令如下：
``` bash
$ sudo apt install openssh-server
```
如果你的机器上没有装客户端的话需要执行以下命令安装：
``` bash
$ sudo apt install openssh-client
```
至此，基础环境已经安装完了。

# 单SSH配置

此节操作只适用于只需要免密码登录一个远程服务器的同学，如果你需要同时SSH远程登录多个服务器，那么请直接跳至下一节。

对于单SSH配置，你只需要输入以下命令即可，此时为默认状态，你的个人信息不会被加入进去：
``` bash
$ ssh-keygen -t rsa
```
如果你需要将个人信息（如邮箱）加入进去（比如远程访问Github时），那么请执行如下命令：
``` bash
$ ssh-keygen -t rsa -C "youraddress@youremail.com"
```
这里有几点需要注意的：
1. `-C`中的C是大写字母
2. `youraddress@youremail.com`是你Github的注册邮箱

对于上述两条命令，如果你远程登录时候需要设置密码，那么请在其提示时候输入密码。然后一路回车即可。 然后你会发现在你的家目录（`~`或者`/home/username/`）下多了一个叫做`.ssh`的文件夹，里面有两个文件`id_rsa`和`id_rsa.pub`。这两个就是SSH生成的文件，你需要将公钥传给远程服务器（Github相关问题详见[Ubuntu16.04下Github配置](/2017/05/01/Ubuntu-Github-config)博文），然后就可以免密码远程登录了。

# 多SSH配置

## 建立不同的SSH配置文件

首先你需要在家目录（`~`或者`/home/username/`）下建立一个`.ssh`文件夹，命令如下：
``` bash
$ mkdir .ssh
```
然后进入该文件夹（建议进入）：
``` bash
$ cd ~/.ssh
```
在该文件夹下，输入以下命令，后面我会介绍每个参数的作用：
``` bash
$ ssh-keygen -t rsa -f ~/.ssh/id_rsa.blabla
```
如果你需要将个人信息（如邮箱）加入进去（比如远程访问Github时），那么请执行如下命令：
``` bash
$ ssh-keygen -t rsa -f ~/.ssh/id_rsa.blabla -C "youraddress@youremail.com"
```
下面我们来分析下这两个命令。
1. `-t` `rsa`是指定你的加密算法。
2. `-f`是指定你的文件存储位置，我建议存在`~/.ssh`文件夹中。文件明明我建议按照我的格式写，`blabla`是该SSH配置文件的用途，比如`.github`, `.localhost`之类的。
3. `-C`注意这里C是大写字母。这里填写你的邮箱地址（顺便提一句配置Github时一定要填写你的注册邮箱，详见[Ubuntu16.04下Github配置](/2017/05/01/Ubuntu-Github-config)）。

## 建立索引

因为你有多对SSH配置文件（`.blabla`和`.blabla.pub`是一对私钥和公钥），所以在远程登陆时，系统需要知道你需要将哪份私钥和远程的公钥进行匹配。所以你需要一个索引文件`config`。输入如下命令建立该文件：
``` bash
$ touch config
```
文件格式如下：
``` bash
Host name
    HostName hostname
    User username
    IdentityFile filepath
```
一个config文件中可以有多个上述结构，每个结构之间建议用一个空行隔开。下面解析下这个结构。
1. `Host`就是个名字，每个结构之间不得重复
2. `HostName`是远程主机的域名，比如github.com, localhost之类或者是一个固定的IP地址。
3. `User`就是你登录该远程主机的用户名。
4. `IdentityFile`就是对应该主机的私钥的文件路径。依上述教程，应为`~/.ssh/id_rsa.blabla`。

# 登录localhost

在配置软件环境时，有软件需要免密码登录`localhost`，也就是免密码登录本机。此时，你需要在`.ssh`目录下建立`authorized_keys`文件，命令如下：
``` bash
$ touch authorized_keys
```
建立此文件的目的是存储已知的SSH公钥信息。此时你需要将localhost的公钥复制进来。
如果你是单SSH配置，则需要把`id_rsa.pub`文件复制进来。命令如下：
``` bash
$ cat id_rsa.pub >> authorized_keys
```
如果你是多SSH配置，依上述教程，你需要把`id_rsa.localhost.pub`复制进来，命令如下：
``` bash
$ cat id_rsa.localhost.pub >> authorized_keys
```
然后你就可以免密码登录localhost了。另外，如果需要添加其他已知SSH公钥的话，直接往`authorized_keys`中添加即可。

# 常见提示及应对方法

## 第一次登录

第一次远程登录一个新的主机时一般会出现如下提示：
``` bash
The authenticity of host 'xxx.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx:xx...xx:xx.
Are you sure you want to continue connecting (yes/no)?
```
输入yes即可。然后可能会出现如下警告，意味着要永久存储该机器的特征信息，不用理睬即可：
``` bash
Warning: Permanently added ‘xxx.com,xx.xx.xx.xx’ (RSA) to the list of known hosts.
```

## known\_hosts文件

当你访问远程主机时，系统会记录远程主机的特征信息，这些信息都存储在`known_hosts`里面。如果你不小心删掉了的话，也没什么事请，就是下一次进行SSH链接时还会出现第一次登录的提示，按照提示输入yes即可。
