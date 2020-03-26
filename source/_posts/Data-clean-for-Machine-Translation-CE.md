---
title: 数据清洗工具及相关使用方法
date: 2018-03-20 14:50:09
tags: [NLP, NMT]
categories: [NLP]
mathjax: true

---

最近跟着老师参加了CWMT评测任务，稍微整理下目前用到的数据清洗相关的工具及使用方法。本人目前操作全在Ubuntu16.04下进行。

<!-- more -->

# 中文分词工具ansj

## 安装ansj

[Ansj的Github地址](https://github.com/NLPchina/ansj_seg)

首先需要下载两个jar包[`ansj_seg`](https://oss.sonatype.org/content/repositories/releases/org/ansj/ansj_seg/)和[`nlp-lang`](https://oss.sonatype.org/content/repositories/releases/org/nlpcn/nlp-lang/)。一般来说都下载最新的jar包都不会有问题，只用下文件夹内名字最短的哪个jar包就可以了。如果需要用老版的`ansj_seg`的话，要去jar包中的`.pom`文件中看下对应`nlp-lang`的jar包，然后下载。

下载完了后在IDE中将jar包引入即可。

## 使用方法

目前我使用的分词方式时精准分词，函数为`ToAnalysis.parse()`。其余方法请移步[Ansj的wiki](https://github.com/NLPchina/ansj_seg/wiki/%E5%88%86%E8%AF%8D%E6%96%B9%E5%BC%8F)查看。
分词后的输出是带有词性的，使用`toStringWithOutNature`函数可以去掉词性。

核心代码样例：
``` java
Result parse = ToAnalysis.parse( tmpString );
String resultStr = parse.toStringWithOutNature( " " );
```

# 英文分词工具tokenizer.perl

`tokenizer.perl`是统计机器翻译系统moses的一个小工具，可以用来对英文德文等进行分词。

## 安装tokenizer.perl

因为该脚本并不是独立运行的，其需要moses自带的一些词库。moses整体不大，建议整体都下载下来。从[moses的Github](https://github.com/moses-smt/mosesdecoder)下载即可。该工具位置在`[mosesdecoder dir]/scirpts/tokenizer/`目录下。

## 使用方法

建议将待分词文件放在上述文件夹下进行分词操作。输入如下命令进行分词：
``` bash
$ perl tokenizer.perl -l en < [untokenized file] > [tokenized file]
```
其中：
1. `-l`是语言选择，这里选择`en`即英文。
2. `<`代表输入，`[untokenized file]`是待分词文件
3. `>`代表输出，`[tokenized file]`是分词后的输出文件，如果这个文件不存在则会创建同名文件。 

其他用法可以输入下述命令查看：
``` bash
$ perl tokenizer.perl -h
```

# 词对齐工具fast\_align

## 安装fast\_align

安装过程翻译自[fast\_align的Github](https://github.com/clab/fast_align)

### 环境配置

在Ubuntu系统上输入以下命令配置环境，其他系统请自行搜索相对应的安装包：
``` bash
$ sudo apt install libgoogle-perftools-dev libsparsehash-dev
```
如果系统中没有安装`cmake`请输入以下命令进行安装：
``` bash
$ sudo apt install cmake
```

### 安装

进入`fast_align`文件夹，顺序执行下述命令进行安装：
``` bash
$ mkdir build
$ cd build
$ cmake ..
$ make
```
上述命令中`..`表示的是上一层文件夹，即`fast_align`文件夹。
安装完成后出现`fast_align`和`atool`两个文件即表示成功安装。

## 使用方法

如果平行预料库源语言和目标语言文件是分开的，则需要手动将其合并成一个文件，格式为：[源语言句子] ||| [目标语言句子]。注意，在"|||"前后是有空格分开的。

样例Python程序：
``` python
#!/bin/python
# -*- encoding: utf-8 -*-

import sys

def merge( src, trg, out ):
    with open( src ) as fs, with open( trg ) as ft, with open( out ) as fo:
        for lines, linef in zip( fs, ft ):
            fo.write( lines.strip() + " ||| " + linef.strip() )

def main():
    if len( sys.argv ) != 4:
        print "USAGE: ./python merge.py [src file] [trg file] [out file]"
    else:
        merge( sys.argv[1], sys.argv[2], sys.argv[3] )

if __name__ == "__main__":
    main()
```

在`build`文件夹下执行下述命令进行正向词对齐：
``` bash
$ ./fast_align -i [filename] -d -o -v > [align_forward_filename]
```
然后在`build`文件夹下执行下述命令进行反向词对齐：
``` bash
$ ./fast_align -i [filename] -d -o -v -r > [align_reverse_filename]
```
最后在`build`文件夹下执行下述命令进行双向词对齐获得最终词对齐文件：
``` bash
$ ./atool -i [align_forward_filename] -j [align_reverse_filename] -c grow-diag-final-and > [align_file_name]
```
生成文件格式为`i-j`的格式，`i`代表`-i`文件中该行第`i`个词，`j`代表`-j`文件中该行第`j`个词。得到最终结果后，中间结果可以删除掉。

## 注意

fast\_align本身线程不安全，尽量不要一次执行多个fast\_align程序。
