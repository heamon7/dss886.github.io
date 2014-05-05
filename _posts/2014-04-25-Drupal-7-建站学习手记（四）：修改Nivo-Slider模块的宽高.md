---
layout: post
title: "Drupal 7 建站学习手记（三）：修改Nivo Slider模块的宽高"
description: "Drupal 网站开发中的一些问题及其解决办法"
category: Drupal
tags: [Drupal, Drupal 7, Nivo Slider, 图片, 宽高]
date: 2014-05-05 11:34:36
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 背景

`Nivo Slider`模块默认大小是用的`height: 100%, width 100%`，

但IE7及以下的浏览器是不支持百分比宽高的，

而我的项目目标用户基本都是使用XP系统，项目需求是必须兼容IE7。

因此需要对其CSS修改成绝对像素大小。

### 问题

修改之后却出现了问题，因为用户上传的图片长宽比是不一样的，

指望用户每次上传的时候先用PS裁剪一下明显不现实，

于是我在CSS里将其拉伸了，这样就不会导致图片只显示一部分。

~~~css
.block-nivo-slider img {
  width: 450;
  height: 250px;
}
~~~

**但是**，`Nivo Slider`在每次幻灯片切换前图片都会变成未拉伸的状态。

这样幻灯片切换的时候就感觉图片在“跳动”。

### 探究

明明已经写死了`img`的宽高，为什么切换前会变回来呢？

初步断定是因为`Nivo Slider`模块在控制切换的JS里有改变图片的宽高。

翻了一下`Nivo Slider`模块的JS，发现是压缩过的，改起来比较麻烦。

于是又祭出万能的CSS大法了！- -！

### 解决

仔细分析后，发现JS是在改变`img`元素的`height`属性

我们可以用`min-height`和`max-height`属性让`height`的改变无效：

~~~css
.block-nivo-slider img {
  width: 450;
  max-height: 250px;
  min-height: 250px;
}
~~~

问题解决！