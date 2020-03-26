---
layout: post
title: Cocos2d-x v3.16 屏幕显示偏移
date: 2018-01-26 20:28:36
tags: [Cocos2d-x, Display Offset]
categories: [Cocos2d-x]
description: Cocos2d-x v3.16 屏幕显示偏移

---

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
