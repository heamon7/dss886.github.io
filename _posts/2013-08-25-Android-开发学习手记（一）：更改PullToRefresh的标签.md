---
layout: post
title: "Android 开发学习手记（一）：分别更改PullToRefresh的上下标签"
description: "Android开发中的一些问题及其解决办法"
category: Android
tags: [Android, PullToRefresh, 分别, 更改, 标签]
date: 2013-08-25 00:56
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 前言

`PullToRefresh`是一个Android上很成熟的下拉刷新的开源控件，目前托管在`GitHub`上：<https://github.com/chrisbanes/Android-PullToRefresh>

### 问题

使用过程中发现，当`PullToRefresh`的`Mode`设为`BOTH`时，即上下都可以刷新时，下拉/上拉默认的英文都是：`Pull to refresh`

可是上拉、下拉的英文都是`Pull`，汉字总不能都写`下拉刷新`吧？

### 探究

粗看了一眼，有这个方法：

~~~java
listView.setRefreshingLabel(String refreshingLabel);
~~~

然后发现它被弃用了：

~~~
Deprecated. You should now call this method on the result ofgetLoadingLayoutProxy().
~~~

调用`getLoadingLayoutProxy()`，发现它还是只有

~~~java
setPullLabel(String)
setReleaseLabel(String)
setRefreshingLabel(String)
~~~

等几个方法，设置以后上下的标签都变了，怎么办？

研究了俩小时。。。发现除了`getLoadingLayoutProxy()`，还有这一个：

~~~java
getLoadingLayoutProxy(boolean includeStart, boolean includeEnd)
~~~

哦，原来得到`Proxy`的时候可以指定是`Start`还是`End`。

### 解决

如果想使上下标签显示不同的文字，可以这样设置：

~~~java
listView.getLoadingLayoutProxy(true,false).setPullLabel("下拉加载上一页");
listView.getLoadingLayoutProxy(false,true).setPullLabel("上拉加载下一页");
listView.getLoadingLayoutProxy(true,true).setReleaseLabel("松开加载");
listView.getLoadingLayoutProxy(true,true).setRefreshingLabel("正在加载");
~~~

解决问题！