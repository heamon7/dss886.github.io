---
layout: post
title: "Drupal 7 建站学习手记（一）：UTF-8 BOM格式"
description: "Drupal 网站开发中的一些问题及其解决办法"
category: Drupal
tags: [Drupal, Drupal 7, UTF-8, BOM, 空行]
date: 2014-04-24 17:06
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 前言

最近接到一个活，是用Drupal7搭一个网站，实现简单的分角色发布文章的功能，架构是`XAMPP+PHP+MySQL+Drupal7`。
以下是我在实际项目中遇到的一些问题和解决办法。

### 问题

我下载了一个Drupal 7的主题，然后覆写它的模板，以实现自定义布局。
覆写完以后发现，页面上方会多出一栏空行。

正常的网页：

![image1]({{ site.url }}/images/post_images/2014.4/1.jpg)

不正常的网页，顶部多了一行空行：

![image1]({{ site.url }}/images/post_images/2014.4/2.jpg)

### 探究

F12审查元素看看：

![image1]({{ site.url }}/images/post_images/2014.4/3.png)

这让我百思不得其解啊，拿修改前的和修改后的文件一行一行的对比，完全没有问题啊！

![image1]({{ site.url }}/images/post_images/2014.4/4.jpg)

然后突然看到了的底下状态栏，两个文件好像不一样：

![image1]({{ site.url }}/images/post_images/2014.4/5.jpg)

### 解决

UTF-8 with BOM是什么东西？试着改回成UTF-8试试，改回来果然就好了！

### 原因

百度了一下，UTF-8 BOM：

~~~plain
BOM——Byte Order Mark，就是字节序标记
 
在UCS 编码中有一个叫做"ZERO WIDTH NO-BREAK SPACE"的字符，它的编码是FEFF。而FFFE在UCS中是不存在的字符，所以不应该出现在实际传输中。UCS规范建议我们在传输字节流前，先传输 字符"ZERO WIDTH NO-BREAK SPACE"。这样如果接收者收到FEFF，就表明这个字节流是Big-Endian的；如果收到FFFE，就表明这个字节流是Little- Endian的。因此字符"ZERO WIDTH NO-BREAK SPACE"又被称作BOM。
 
UTF-8不需要BOM来表明字节顺序，但可以用BOM来表明编码方式。字符"ZERO WIDTH NO-BREAK SPACE"的UTF-8编码是EF BB BF。所以如果接收者收到以EF BB BF开头的字节流，就知道这是UTF-8编码了。
 
UTF- 8编码的文件中，BOM占三个字节。如果用记事本把一个文本文件另存为UTF-8编码方式的话，用UE打开这个文件，切换到十六进制编辑状态就可以看到开 头的FFFE了。这是个标识UTF-8编码文件的好办法，软件通过BOM来识别这个文件是否是UTF-8编码，很多软件还要求读入的文件必须带BOM。可是，还是有很多软件不能识别BOM。

在Firefox早期的版本里，扩展是不能有BOM的，不过Firefox 1.5以后的版本已经开始支持BOM了。现在又发现，PHP也不支持BOM。PHP在设计时就没有考虑BOM的问题，也就是说他不会忽略UTF-8编码的文件开头BOM的那三个字符。
~~~

果然，PHP是不支持BOM的，会把BOM头显示成一个空字符。

为什么我会把修改过的文件保存成有BOM的UTF-8呢？

想到项目刚开始的时候我用的是Notepad++，后来用的是Sublime Text 3，会不会是编辑器的问题？

果不其然，Notepad++的保存格式是这样的：

![image1]({{ site.url }}/images/post_images/2014.4/6.png)

Sublime Text 3的保存格式是这样的：

![image1]({{ site.url }}/images/post_images/2014.4/7.jpg)

摔！两个编辑器默认的UTF-8根本不是一个东西啊！

从此成了Notepad++一生黑！！！