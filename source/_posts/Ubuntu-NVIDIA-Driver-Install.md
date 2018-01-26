---
layout: post
title: Ubuntu系统NVIDIA显卡驱动安装
date: 2017-10-10
tags: [Ubuntu, NVIDIA]
categories: [Config]

---

# 前言

本文是在Ubuntu17.04系统上安装NVIDIA驱动。

在Ubuntu16.04 LTS上安装驱动时是从[NVIDIA 官网](https://www.geforce.cn/drivers)上下载.run文件然后按照提示安装，但是在Ubuntu17.04上并不成功。因此寻找到了一种更为简便的方法，记录在此。

如果要在本机安装NVIDIA Driver的话请先用手机等设备照下关键步骤然后再执行。

# 安装准备

## 查找合适的驱动

在[NVIDIA 官网](https://www.geforce.cn/drivers)上寻找合适的驱动，并记住其版本号。例如，384.98的版本号是384。

## 关闭图形界面

首先按住`Ctrl+Alt+F1`进入`tty1`模式，然后输入如下代码关闭图形界面（X Server）：
``` bash
$ sudo stop lightdm
```
`lightdm`指的是图形界面服务

## 卸载之前的NVIDIA显卡驱动

如果之前有尝试过其他驱动，则需要将其卸载步骤如下：
``` bash
$ sudo apt remove nvidia-*
```
如果采用的是.run文件安装的系统请采用如下命令卸载：
``` bash
$ sudo ./NVIDIA-*.run --uninstall
```
这里`NVIDIA-*.run`是下载的.run文件全名。

# 安装NVIDIA显卡驱动

因为无法通过.run方式安装驱动，因此采用从第三方源的方式安装驱动。

## 添加第三方源

这次采用的是`mamarley`源，添加该源的命令如下：
``` bash
$ sudo add-apt-repository ppa:mamarley/nvidia
```
然后进行更新源
``` bash
$ sudo apt update
```

## 安装NVIDIA显卡驱动

在添加好第三方源后采用apt方式安装，命令如下：
``` bash
$ sudo apt install nvidia-384
```
本文安装的是384版本，请按照个人不同的需求输入合适的驱动版本号。

至此，驱动已经安装完成，但是请不要立即启动`lightdm`服务。请先输入以下命令重启系统：
``` bash
$ reboot
```
## 检查安装结果

在命令行输入如下结果：
``` bash
$ nvidia-smi
$ nvidia-settings
```
输入`nvidia-smi`命令后输出一个表格即为正常。
输入`nvidia-settings`出现下图即为正常。
![NVIDIA-X-Server-Settings](NVIDIA-X-Server-Settings.png)

# 异常处理

如果安装显卡后出现循环登录不进入桌面的情况请执行“[安装准备](#安装准备)”中的“[关闭图形界面](#关闭图形界面)”和“[卸载之前的NVIDIA显卡驱动](#卸载之前的NVIDIA显卡驱动)”。

# 卸载驱动

卸载驱动请按照“卸载之前NVIDIA驱动”所述内容进行卸载并重启。
