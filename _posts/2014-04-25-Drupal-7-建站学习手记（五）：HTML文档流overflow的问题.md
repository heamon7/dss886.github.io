---
layout: post
title: "Drupal 7 建站学习手记（五）：HTML文档流overflow的问题"
description: "Drupal 网站开发中的一些问题及其解决办法"
category: Drupal
tags: [Drupal, Drupal 7, overflow, position, relative, absolute]
date: 2014-05-05 13:30:24
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 背景

项目要求网站首页放Views生成的区块，并且要求有`更多`链接。

Views生成的区块默认的`更多`链接只能选在列表上方和下方

下图是默认在上方的样式图：

![image1]({{ site.url }}/images/post_images/2014.5/1.png)

为了美观，我将`更多`链接上移了若干个像素：

~~~css
.more-link {
  position: absolute;
  top: 10px;
  left: 390px;
}
~~~

效果图：

![image2]({{ site.url }}/images/post_images/2014.5/2.png)

### 问题

