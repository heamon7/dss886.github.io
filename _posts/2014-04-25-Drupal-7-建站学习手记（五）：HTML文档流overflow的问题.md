---
layout: post
title: "Drupal 7 建站学习手记（五）：HTML文档流overflow的问题"
description: "Drupal 网站开发中的一些问题及其解决办法"
category: Drupal
tags: [Drupal, Drupal 7, overflow, position, relative, absolute, 文档流]
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

然而，当我在这个区块外面再套一层区块时，

（比如我这里用了`QuickTabs`模块，实际上是一个嵌套区块）

这个`更多`链接怎么都不显示了：

![image3]({{ site.url }}/images/post_images/2014.5/3.png)

### 探究

第一反应是z-index的问题，设了z-index还是不显示

Google一下，觉得有可能是`position: relative`的问题

将其所有父`div`标签全部加上`position: relative`属性，仍然不显示

真是百思不得其解

纠结了很久以后，最终将目标锁定在了父`div`标签的`overflow: hidden`属性上

CSS `Overflow`属性的定义：

| 值      |　描述                                                     |
|:-------:|:----------------------------------------------------------|
| visible |　默认值。内容不会被修剪，会呈现在元素框之外。             |
|----
| hidden  |　内容会被修剪，并且其余内容是不可见的。                   |
|----
| scroll  |　内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
|----
| auto    |　如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
|----
| inherit |　规定应该从父元素继承 overflow 属性的值。                 |
|----
{: rules="groups"}

`QuickTabs`模块的外层区块使用了`overflow: hidden`属性

内层元素“溢出”时，内容被div修剪掉了。

它的本意可能是为了不让内层的区块超出外层区块的大小，而打乱整个HTML文档流。

但是我这里的需求恰好就是要让内层的元素“溢出”来。

### 解决

将`QuickTabs`外层区块div元素的`overflow: hidden`改为`visible`就行了：

~~~css
.block-quicktabs .content {
  overflow: visible;
}
~~~

效果图：

![image4]({{ site.url }}/images/post_images/2014.5/4.png)

问题解决