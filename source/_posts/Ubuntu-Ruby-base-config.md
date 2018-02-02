---
layout: post
title: Ubuntu 16.04下Ruby基础配置
date: 2017-05-01
tags: [Ubuntu, Ruby]
categories: [Config]

---

Note: 本文内容节选复述了[Ruby-China](https://ruby-china.org/wiki/install_ruby_guide)中的教程中的内容，其中有一两步和原版有出入（我踩到的坑）。如果需要看原版内容请点击上述链接。

本文是在Ubuntu 16.04 LTS环境下配置的，如果使用其他操作系统请自行查找对应命令或解决方案。

以下代码区域，带有$打头的表示需要在控制台（终端或称命令行）下面执行（不包括$符号）

由于ubuntu自带的ruby版本太老，所以需要从下列途径更新。

# 安装系统需要的包
``` bash
$ sudo apt-get install curl
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
其中curl是一种下载工具，在后续操作中需要用到。

# 安装RVM

RVM安装只需输入以下命令：
``` bash
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ curl -sSL https://get.rvm.io | bash -s stable
# 如果上面的连接失败，可以尝试: 
$ curl -L https://raw.githubusercontent.com/wayneeseguin/rvm/master/binscripts/rvm-installer | bash -s stable
```
如果其中某些命令出现权限不足提醒，则在前面添加sudo再执行即可。

接下来载入RVM环境
``` bash
$ source ~/.rvm/scripts/rvm
```
接下来有一点和教程中不太一样（我踩到的坑）。打开`~/.bash_profile`文件你可能发现如下一行：
``` bash
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
```
原文中说“新开的终端就不想要这么做了，会自动重新载入的”正因为RVM安装程序在`.bash_profile`中添加了这么一行。然而你可能发现现实很残酷，新开的终端并没有载入rvm环境。此时你需要在家目录（`～`）下的.bashrc文件中的末尾添加如下几行：
``` bash
# Add RVM to PATH for scripting. Make sure this is the last PATH variable change.
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"
```
其实也就是复制到.bashrc中而已。然后重启终端即可，如果还不行那么注销重新登录即可，如果还不行请重启机器。

然后修改RVM下的Ruby源，到Ruby China的镜像：
``` bash
$ echo "ruby_url=https://cache.ruby-china.org/pub/ruby" > ~/.rvm/user/db
```
检查下是否安装正确
``` bash
$ rvm -v
```

# 用RVM按转Ruby环境

``` bash
$ rvm requirements
$ rvm instll 2.4.0
```
这里的版本号（2.4.0）可以通过下属命令查看并可修改：
``` bash
$ rvm list
```

# 设置Ruby版本

RVM 安装好后，需要执行下述命令将指定版本设为系统默认版本
``` bash
$ rvm use 2.4.0 --default
```
这个时候你可以测试是否正确
``` bash
$ ruby -v
$ gem -v
```
这里你可以替换原有的gem源到Ruby China的源或者淘宝源，分别是：
``` bash
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
```
可以通过下属命令查看gem源：
``` bash
$ gem sources -l
```
接下来安装Bundler
``` bash
$ gem install bundler
```

# 设置Rails环境
输入以下命令就可以轻松安装上Rails了：
``` bash
$ gem install rails
```
测试是否安装正确
``` bash
$ rails -v
```
至此，Ruby基础安装教程到此结束喵～
