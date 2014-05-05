---
layout: post
title: "Drupal 7 建站学习手记（二）：更改Custom Search模块的样式"
description: "Drupal 网站开发中的一些问题及其解决办法"
category: Drupal
tags: [Drupal, Drupal 7, Custom Search, 水平, 垂直]
date: 2014-04-25 15:38
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 前言

项目需要用到自定义搜索框，Drupal自带的区块只能显示一个简单的搜索框，进入到搜索页面才能进行高级搜索。

Custom Search 可以在提供比系统自带搜索区块更多的功能，如下图：

![image8]({{ site.url }}/images/post_images/2014.4/8.png)

### 问题

Custom Search 有很多设置选项，标签、显示的文字啊什么的，

美中不足的是，它的排版是垂直排列的，而项目要求这三个元件水平排列，在同一行显示，

### 探究

我在设置里并没有找到可以水平排列的选项，粗粗看了一眼它的模版文件，也没找到代码位置

那就只好祭出万能的 CSS 修改大法了！

1. 这三个元件的父div靠右对齐
2. 文本框左移，分类框上移、左移，搜索按钮上移

~~~css
.region-header{
  text-align: right;
  margin-bottom: -40px;
}
   
.form-item-custom-search-blocks-form-1 {
  margin-right: 150px;
}

.form-item-custom-search-types {
  margin: -42px 49px 0 0;
}

#edit-submit {
  margin-top: -28px;
}
~~~

#### 效果图

![image9]({{ site.url }}/images/post_images/2014.4/9.jpg)

### 问题

但是有个问题，注意到我修改“搜索”按钮的时候是用的id定位的：`#edit-submit`

后来发现了一些问题，在很多页面——比如文章的编辑页面有别的提交按钮，这个时候这个搜索按钮的id会变成`edit-submit--2`等，可是它的`class`也是跟别的提交按钮共用的。跟文本框和分类框不一样，`Custom Search`并没有给这个按钮自定义`class`名，我无法精确定位搜索按钮。

### 探究

想了一想，CSS好像有属性选择器，试一下：

~~~css
input[value=搜索] {
  margin-top: -28px;
}
~~~

貌似可以了，嗯，输入文字点击，进到高级搜索和搜索结果页面：

![image10]({{ site.url }}/images/post_images/2014.4/10.jpg)

摔！所有的搜索按钮都跟着上移了....

在这个问题上卡了半天，一直在琢磨怎么定位这个搜索按钮...

突然灵光一闪，CSS是不是还有个兄弟元素选择器啊？

试试看：

~~~css
.form-item-custom-search-types+.form-actions {
  margin-top: -28px;
}
~~~

bingo！完美！

...好吧，我承认这样写代码有点傻....不过好歹问题解决了！~

--

update 2014-4-25 :

突然发现自己果然有点傻，把三个框全部设`display: inline`就行了....