---
layout: post
title: Cocos2d-x v3.16 Android Studio添加新类
date: 2018-02-03 20:29:01
tags: [Cocos2d-x, Android Studio]
categories: [Cocos2d-x]

---

在使用Android Studio编辑Cocos2d-x项目时，如果想添加一个Class进去，除了建立相对应的`.h`和`.cpp`以外，还需要让编译配置文件知道这个文件属于该项目。然而Android Studio 3.x版本自动同步时并不能将新类中的`.cpp`问家加入编译配置文件中。

<!-- more -->

后来发现在左侧`External Build Files`中，有个叫做`Android.mk`的文件，需要在其中的`LOCAL_SRC_FILES`变量中加入新的`.cpp`文件。即：
``` mk
LOCAL_SRC_FILES := $(LOCAL_PATH)/hellocpp/main.cpp \
                   $(LOCAL_PATH)/../../../Classes/AppDelegate.cpp \
                   $(LOCAL_PATH)/../../../Classes/HelloWorldScene.cpp \
                   $(LOCAL_PATH)/../../../Classes/Hello.cpp
```
想必大家都读得懂怎么向这个变量中加入新的文件，其中`Hello.cpp`就是我添加的新`.cpp`文件。
