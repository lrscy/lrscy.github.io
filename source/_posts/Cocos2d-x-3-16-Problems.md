---
layout: post
title: Cocos2d-x v3.16踩过的坑
date: 2018-01-26 16:35:23
tags: [Cocos2d-x, Android Studio]
categories: [Cocos2d-x]

---

# 前言

最近刚碰关于Cocos2d-x的知识，也上网查过很多资料，但是很少有讲最新v3.16的博客。因此在此记录下使用v3.16时候遇上的坑。且会不定期更新该篇博客。

# 屏幕显示偏移

在v3.16前，屏幕上显示Sprites, Nodes都是直接使用Director类获取可视区大小并直接在上面定位Nodes的，例如：
``` c++
auto size = Director::getInstance()->getVisibleSize();
auto sprite = Sprite::create( "HelloWorld.png" );
sprite.setPosition( size.width / 2, size.height / 2 );
```
目前不知道时从哪个版本开始的，直接使用上述代码会导致`sprite`下偏移(沿y轴)一定量但无左右偏移(沿x轴)。查找资料未果后看Helloworld样例，发现其用到了Director类中另一个变量纠正这个问题：
``` c++
Vec2 origin = Director::getInstance()->getVisibleOrigin();
```
这里`origin`是真正的屏幕左下角的位置，所有屏幕上的显示的图像都要依据这个坐标进行调整。也就是说上述代码要改成：
``` c++
auto size = Director::getInstance()->getVisibleSize();
Vec2 origin = Director::getInstance()->getVisibleOrigin();
auto sprite = Sprite::create( "HelloWorld.png" );
sprite.setPosition( origin.x + size.width / 2, origin.y + size.height / 2 );
```
这时，屏幕偏移问题解决了，`sprite`也在屏幕的真正的正中央。

# Android Studio添加新类

在使用Android Studio编辑Cocos2d-x项目时，如果想添加一个Class进去，除了建立相对应的`.h`和`.cpp`以外，还需要让编译配置文件知道这个文件属于该项目。然而Android Studio 3.x版本自动同步时并不能将新类中的`.cpp`问家加入编译配置文件中。

后来发现在左侧`External Build Files`中，有个叫做`Android.mk`的文件，需要在其中的`LOCAL_SRC_FILES`变量中加入新的`.cpp`文件。即：
``` mk
LOCAL_SRC_FILES := $(LOCAL_PATH)/hellocpp/main.cpp \
                   $(LOCAL_PATH)/../../../Classes/AppDelegate.cpp \
                   $(LOCAL_PATH)/../../../Classes/HelloWorldScene.cpp \
                   $(LOCAL_PATH)/../../../Classes/Hello.cpp
```
想必大家都读得懂怎么向这个变量中加入新的文件，其中`Hello.cpp`就是我添加的新`.cpp`文件。

To be continue...
