---
layout: post
title: Cocos2dx-3.16-Android-Multi-Touch
date: 2018-02-11 20:40:58
tags: [Cocos2d-x, Android, Multi Touch]
categories: [Cocos2d-x]

---

网上有关Cocos2d-x v3.x版本的多点触控的资料并不多，在尝试时发现了如下几个坑。跳出坑最好的办法就是去读官方给出的Testapp的源码，这样能看快的了解到官方时如何使用各种功能的，模仿和学起来都很快且很到位。如果更有能力的去多看看API和源码也是很好的学习途径。

<!-- more -->

# onTouchesBegan/Moved/Ended/Cancelled函数的参数

网上众多教程基本都是基于2.x版本，所用函数名及参数有许多为`cc`开头，且触摸回调函数的参数基本为：
``` c++
void MoreTouches::ccTouchesMoved( cocos2d::CCSet *pTouches,
                                  cocos2d::CCEvent *pEvent );
```
在当前3.16版本中，其改为了
``` c++
void onTouchesBegan( const std::vector<cocos2d::Touch *> &touches,
                     cocos2d::Event *event );
```
不再使用`set`作为触摸点的存储结构，而是采用`vector`。

# onTouchesBegan/onTouchesEnded传入参数touches的问题

先说`onTouchesBegan`函数的传入参数。`onTouchesEnded`和`onTouchesBegan`的逻辑几乎是一样的。

开始参考网上多点触控的代码进行实验，但是在`onTouchesBegan`回调函数上永远出问题，后来发现`onTouchesBegan`回调函数的`touches`参数永远只存了一个变量（那你用vector存什么呀啊喂！）。在进行了众多测试以及上网寻找资料（不小心还挖了个坟emmmm）后决定仔细研究下官方的Testapp源码。

原先模仿网上函数写法的代码：
``` c++
void Hello::onTouchesBegan( const std::vector<cocos2d::Touch *> &touches,
                            cocos2d::Event *event ) {
        if( touches.size() >= 2 ) { ... }
        else { ... }
```
这段代码的`touches.size()`的值永远是`1`。官方给出的`MultiTouch`的源代码部分如下：
``` c++
static Map<int, TouchPoint *> s_map;

void MultiTouchTest::onTouchesBegan( const std::vector<Touch *> &touches,
                                     Event *event ) {
    for ( auto &item: touches ) { ... }
}
```
最开始看代码的时候还一脸欢喜，这官方给的例子不是明显的在说`touches`里面会存多变量的么。直到我用`log`打出来`touches.size()`后才“惊喜”的发现这值也是`1`（那你用vector干嘛呢遍历啥呢啊喂！）。

目前反推`onTouchesBegan`的设计逻辑是说一个指头的触摸激活一次`onTouchesBegan`，每个触摸的初始化单独做一次。这样的好处在于当多指相差时间很长才都按到屏幕上时，也能可以通过设计逻辑很容易的将其识别为多指操作，而不是多次单指操作。就算真的物理上是同时按住的时候也可能会给序列化处理成先后两次触摸。唯独麻烦的一点就是程序逻辑设计上要费点心思了。

目前本人的方法是将所有`onTouchesBegan`读取到的触摸都`push_back`到一个`vector`中，在`onTouchesMoved`中分类处理，最终每触发一次`onTouchesEnded`时就`pop_back`掉一次。至于为什么没用`stack`结构，因为处理`onTouchesMoved`处理时候的读取用`vector`方便些。
