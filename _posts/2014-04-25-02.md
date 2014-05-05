---
layout: post
title: "Drupal 7 建站学习手记（三）：Nivo Slider模块与business主题不兼容的问题"
description: "Drupal 网站开发中的一些问题及其解决办法"
category: Drupal
tags: [Drupal, Drupal 7, Nivo Slider, business, 不兼容, 报错]
date: 2014-04-25 20:48
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 前言

`Nivo Slider`是Drupal的一个制作幻灯片的模块，效果比`views slideshow`好得多

今天在尝试安装使用这个模块的时候却颇费了一番功夫。

使用`Nivo Slider`需要安装以下几个模块：

1. `Nivo Slider`模块本身，这个不用说；
2. `Libraries API`，相当多的模块依赖这个库，应该都已经装了；
3. `Nivo Slider jQuery plugin`，这个是`Nivo Slider`的一些界面动画所依赖的jQuery库；
4. `jquery_update`，这个是使当前jQuery库升级到最新版的模块；

### 问题

问题就出在`jquery_update`这个模块上，启用这个模块后，我的`business`主题二级菜单失效了

看了一下浏览器的log，是`business/js/superfish.js`报错：

![image11]({{ site.url }}/images/post_images/2014.4/11.jpg)

### 探究

打开`superfish.js`看看：

![image12]({{ site.url }}/images/post_images/2014.4/12.jpg)

顶部有版权声明和开源许可，貌似是一个使用jQuery的开源组件？

因为是启用了`jquery_update`模块才报错的，推测是有可能它使用的jQuery版本与最新版不符导致的。

去网上搜了一下，果然有`Superfish`这个组件，最新版是1.9

下载下来最新的`Superfish.js`复制进`business/js`覆盖，菜单栏果然没问题了。

![image13]({{ site.url }}/images/post_images/2014.4/13.png)

接下来用`Nivo Slider`设置首页幻灯片，可是图片是显示，就是不动啊，看下log：

![image14]({{ site.url }}/images/post_images/2014.4/14.jpg)

看来还是jQuery的问题，又百度了一下这个错误提示

找到了这个：<http://blog.csdn.net/dracotianlong/article/details/18195141>

jQuery 1.7版本以后，live接口被删除了，必须换成on

### 解决

按照错误提示找到`sites/all/nivo-slider/jquery.nivo.slider.pack.js`

打开，把里面的两处`live()`改成`on()`，保存、刷新页面。

bingo，问题解决！