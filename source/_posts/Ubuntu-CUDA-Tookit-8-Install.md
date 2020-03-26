---
layout: post
title: Ubuntu 16.04下CUDA Tookit 8安装
date: 2018-02-01 11:39:17
tags: [Ubuntu, CUDA]
categories: [Config]

---

# 前言

前一阵重做系统把之前的安装教程的博客给弄丢了，现在重写一份做记录好了。目前老环境还是要`CUDA Tookit 8`（以下简称`CUDA8`），因此目前先不安装CUDA最新的`CUDA9`了。

本教程是在Ubuntu 16.04.3 LTS上进行的，其他版本的Ubuntu系统请自行实验，目前在Ubuntu 17.10系统上实验成功。

当前假设Ubuntu系统已经安装完成，目前直接进行CUDA8的安装。

<!-- more -->

# 基础准备

## 基础环境安装

安装CUDA8需要基础的编译环境，需要检测下系统上是否安装了`gcc`或`g++`。命令如下：
``` bash
$ gcc -v
$ g++ -v
```
如果任意一个出现版本信息就代表安装过了。如果都没有出现版本信息，则请采用如下命令安装：
``` bash
$ sudo apt install build-essential
```
安装完成即可，网上有说CUDA8不支持`g++ 5.0`以上的版本，目前本人没有遇上这个问题。

## 附加环境安装

有些人在上述基础环境下安装完CUDA8后会出现`Missing recommended library: libGLU.so`的提示。如果不确定当前环境想提前避免这个问题，请安装如下包：
``` bash
sudo apt install libglu1-mesa libxi-dev libxmu-dev libglu1-mesa-dev
```
至此，基础环境都安装完了。

# NVIDIA驱动安装

个人强烈建议先装NVIDIA驱动，因为CUDA8自带的驱动实在是容易出问题。驱动安装详见「[Ubuntu系统NVIDIA显卡驱动安装](/2017/10/10/Ubuntu-NVIDIA-Driver-Install)」。

# CUDA8安装配置

## CUDA8下载

先从[官网下载](https://developer.nvidia.com/cuda-downloads)CUDA的驱动。目前CUDA的最新版本是`CUDA9`，要下老版本的话请采用如下[官方网址](https://developer.nvidia.com/cuda-80-ga2-download-archive)，下载`Base Installer`和`Patch 2`。

个人建议先将文件都下载到家目录(`~`)底下，因为如果系统是中文环境的话，后续安装可能会出现文件夹名乱码的情况。安装完成后可以移动到自己想移动的位置。

## CUDA8安装

是否进入`tty1`环境看个人，本人之前各种重装驱动给吓怕了，不确定图形界面是否影响到了，因此直接进入了`tty1`环境进行CUDA8的安装。关闭图形界面的步骤如下：

1. 首先按住`Ctrl+Alt+F1`进入`tty1`
2. 输入用户名和密码
3. 执行`sudo service lightdm stop`命令关闭图形界面。

然后在安装文件所在目录下执行如下命令安装：
``` bash
$ sudo chmod 755 (CUDA Install File)
$ sudo ./(CUDA Install File)
```
其中`CUDA Install File`是个人CUDA安装文件的名字（包括文件后缀）。

安装关键过程如下（先后顺序记不清了）：

1. 安装过程中先阅读完一大串协议，按住`d`往下（这样跳得很快），直到最后。然后输入accept。
2. 会询问是否安装`NVIDIA DRIVER`，输入`n`。
3. 询问是否安装`OPENGL`时输入`n`。（记得有这个问题的，这里是个大坑！！）
4. 其他默认选择`y`或者`空着`（就是直接按回车）就行。

到最后可能会出现`INCOMPLETE INSTALL`，这里不用管。这是因为你没装它的驱动而已。如果出现`Missing recommended library: libGLU.so`的提示，请参考[附加环境安装](#附加环境安装)

等一切都安装好后重启即可。

## 补丁安装

是否进入`tty1`环境依旧看个人，具体参考上一节开头。

安装时执行如下命令安装：
``` bash
$ sudo chmod 755 (CUDA Patch File)
$ sudo ./(CUDA Patch File)
```
其中`CUDA Patch File`是之前下载的CUDA8的补丁文件`Patch 2`。安装过程和前一节类似，基本上直接一路默认就行。

等一切安装完了后重启即可。

## 环境设置

个人习惯在`/etc/profile.d`下设置环境变量。先进入该文件夹，然后执行如下命令：
``` bash
$ sudo touch cuda.sh
$ sudo vim cuda.sh
```
这里`vim`可以换成`gedit`或者其他熟悉的编辑器。如果不熟悉`vim`的同学无意间进去了，输入`:q`退出`vim`。

在编辑器中输入如下文字：
```
export CUDA_HOME=/usr/local/cuda-8.0
export PATH=$PATH:/usr/local/cuda-8.0/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-8.0/lib64
```
然后保存重启即可。

## cuDNN安装

`cuDNN`是个GPU加速库，能为深度学习网络的计算加速。在[官网](https://developer.nvidia.com/rdp/cudnn-download)下载。下载前可能需要先注册`NVIDIA DEVELOPER`。注册过程很简单，注册完成后选择支持`CUDA8.0`的`cuDNN`下载即可。

在下载目录解压后进入`cuda`文件夹，这里会见到`include`和`lib64`两个文件夹。这里建议在命令行下执行，因为会用到`sudo`进行暂时的root权限申请。输入如下命令进行安装：
``` bash
$ sudo cp include/cudnn.h /usr/local/cuda/include
$ sudo cp lib64/libcudnn* /usr/local/cuda/lib64
```
然后进入`/usr/local/cuda/lib64`文件夹中，执行如下命令：
``` bash
$ sudo rm libcudnn.so libcudnn.so.6
$ sudo ln -s libcudnn.so.6.0.20 libcudnn.so.6
$ sudo ln -s libcudnn.so.6 libcudnn.so
$ sudo ldconfig
```
这里`.6`和`.6.0.20`是下载的cuDNN的版本号，请依据个人下载的实际版本进行修改。

至此，cuDNN已经安装完成。

# 结尾

至此，整个CUDA Tookit 8的所有安装过程就完成了。
