---
layout: post
title: Ubuntu 16.04下TensorFlow安装
date: 2018-02-02 13:29:40
tags: [Ubuntu, Tensorflow]
categories: [Config]

---

# 前言

TensorFlow安装环境为`Ubuntu 16.04.3 LTS`，GPU为`GT 750M`。

假设目前已经安装好了`CUDA8`，如果没有安装请依照「[Ubuntu 16.04下CUDA Tookit 8安装](/2018/02/01/Ubuntu-CUDA-Tookit-8-Install)」进行安装。

如果有一定英语能力的同学最好请移步[官网](https://www.tensorflow.org/install/install_linux)进行下载安装，尽管可能需要下科学上网。

<!-- more -->

# 基础环境搭建

本文采用Virtualenv环境进行搭建，这样能将TensorFlow运行于一个分离开的Python环境下，免得今后出错时对整个系统产生影响。

## CUPTI环境搭建

CUPTI库能够提高可以提高CUDA的性能，官方要求安装。

如果CUDA Tookit >= 8.0，输入如下命令安装：
``` bash
$ sudo apt install cuda-command-line-tools
```
并且在CUDA安装时所设定的`LD_LIBRARY_PATH`添加路径至如下状态：
``` bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/extras/CUPTI/lib64
```
如果CUDA Tookit <= 7.5或者上述命令无法执行，请输入如下命令安装CUPTI库：
``` bash
$ sudo apt-get install libcupti-dev
```

## pip环境搭建

下面两行中选择一行执行即可，依据个人Python版本选择：
``` bash
$ sudo apt install python-pip  python-dev  python-virtualenv # for Python 2.7
$ sudo apt install python3-pip python3-dev python-virtualenv # for Python 3.x
```
本人选择的是`Python2.7`，因为`Ubuntu 16.04.3`默认`Python`为`Python 2.7`，改默认会影响`ibus-pinyin`（输入法）的更新和运行。

## Virtualenv环境搭建

下面两行中选择一行执行即可，依据个人Python版本选择：
``` bash
$ virtualenv --system-site-packages targetDirectory            # for Python 2.7
$ virtualenv --system-site-packages -p python3 targetDirectory # for Python 3.x
```
其中`targetDirectory`是隔离环境的根目录，需要自行设定。个人设定是`~/tensorflow`，下文暂且按照这个目录进行介绍。

至此，基础观景搭建完成。

# TensorFlow安装

## 进入Virtualenv环境

输入如下命令进入Virtualenv环境：
``` bash
$ source ~/tensorflow/bin/activate      # bash, sh, ksh, or zsh
$ source ~/tensorflow/bin/activate.csh  # csh or tcsh
```
请依据自身`shell`环境选择，一般Ubuntu原生使用的是`sh`。

这里可以在`~/.bashrc`中输入如下命令使得命令简化：
``` bash
alias tensorflow="source ~/tensorflow/bin/activate"      # bash, sh, ksh, or zsh
alias tensorflow="source ~/tensorflow/bin/activate.csh"  # csh or tcsh
```
请依据自身的`shell`环境选择第一条还是第二条。其中`tensorflow`可以自行拟定。

当编辑完后在命令行中输入如下命令使其立即生效：
``` bash
$ source ~/.bashrc
```
以后在命令行中输入`tensorflow`即可调出该`Virtualenv`环境。

当前环境应该如下：
``` bash
(tensorflow) xxx@xxx:path$
```
1. `tensorflow`即`Virtualenv`文件夹名称，下文按照`tensorflow`描述。
2. `xxx@xxx:path`与未进入`Virtualenv`环境时无差别。下文中将简化描述为`(tensorflow) $`

## 更新pip

为了确保pip version >= 8.1，输入如下命令更新pip：
``` bash
(tensorflow) $ pip install --upgrade pip  # for Python 2.7
(tensorflow) $ pip3 install --upgrade pip # for Python 3.x
```

## 安装TensorFlow

如果系统中的`CUDA Tookit`和`cuDNN`都是最新版，请从以下四条命令中选择一个来安装TensorFlow：
``` bash
(tensorflow) $ pip install --upgrade tensorflow      # for Python 2.7
(tensorflow) $ pip3 install --upgrade tensorflow     # for Python 3.x
(tensorflow) $ pip install --upgrade tensorflow-gpu  # for Python 2.7 and GPU
(tensorflow) $ pip3 install --upgrade tensorflow-gpu # for Python 3.x and GPU
```
前两条是仅使用CPU的版本，后两条的版本能够使用GPU。

如果系统中的`CUDA Tookit`和`cuDNN`并非最新版，按照上述安装很可能会出现问题导致重装。因此安装前请先完成如下几步：

1. 请先确定系统中的`cuDNN`和`CUDA Tookit`时想匹配的，具体版本匹配详见[官网](https://developer.nvidia.com/rdp/cudnn-download)。
2. 请在[TensorFlow的Github Release](https://github.com/tensorflow/tensorflow/releases)中寻找符合自己`CUDA Tookit`和`cuDNN`的版本。
3. 依照选好的版本拼凑网址。

拼凑网址如下，请依据CPU以及GPU从两条中选择一条：
```
# for CPU only
https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-x.x.x-cpxx-none-linux_x86_64.whl
# for GPU support
https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-x.x.x-cpxx-none-linux_x86_64.whl
```
其中：
1. `x.x.x`是从`release`中选择出的需要的版本。
2. `cpxx`是本机`python`的版本，例如：`2.7.xx`为`27`，`3.5.xx`的版本为`35`，以此类推。
3. `storage.googleapis.com`可能需要科学上网才能访问。

然后进行Tensorflow的安装，输入如下命令：
``` bash
(tensorflow) $ pip install yourhttpsite  # for Python 2.7
(tensorflow) $ pip3 install yourhttpsite # for Python 3.x
```
`yourhttpsite`就是拼凑出来的网址。

如果pip提示无法从代理处下载文件，则可以先将文件从网址下载下来并存在`~`（即家目录）或者自定义目录下，然后通过如下命令安装：
``` bash
(tensorflow) $ pip install filename  # for Python 2.7
(tensorflow) $ pip3 install filename # for Python 3.x
```
`filename`即下载的`.whl`文件（可能需要输入完整路径）。

# TensorFlow的使用

## 启动TensorFlow环境

请参照[进入Virtualenv环境](#进入Virtualenv环境)步骤进行。

## 测试TensorFlow环境

先进入TensorFlow环境，然后输入如下代码进行测试：
``` python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```
输出`Hello, TensorFlow!`且无报错即可。如果报错请参考[官网](https://www.tensorflow.org/install/install_linux#CommonInstallationProblems)或者上网寻找解决方案。

## 退出TensorFlow环境

在环境中输入`deactivate`即可，即：
``` bash
(tensorflow) $ deactivate
```

# TensorFlow的卸载

卸载时直接将其所在的Virtualenv文件夹删除即可，命令如下：
``` bash
$ rm -rf targetDirectory
```
`targetDirectory`即Virtualenv环境文件夹。
