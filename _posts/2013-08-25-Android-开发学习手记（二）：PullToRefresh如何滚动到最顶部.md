---
layout: post
title: "Android 开发学习手记（二）：PullToRefresh如何滚动到最顶部"
description: "Android开发中的一些问题及其解决办法"
category: Android
tags: [Android, PullToRefresh]
date: 2013-08-25 01:15
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 问题

一般如果用`ListView`，让它滚动到顶部，是这样写的：

~~~java
if (!listView.isStackFromBottom()) {
	listView.setStackFromBottom(true);
}
listView.setStackFromBottom(false);
~~~

但是，使用`PullToRefreshListView`以后，发现该对象竟然没有`setStackFromBottom()`方法！

### 探究

翻翻它的源码，发现是这样的：

~~~java
public class PullToRefreshListView extends PullToRefreshAdapterViewBase<ListView>{...}
~~~

它并不是继承于`ListView`，所以也无法将这个对象`cast`到`ListView`。

但是，实际上`PullToRefreshListView`的主体确实是一个`ListView`，那么如何使用属于`ListView`的方法呢？

### 原因

Google了半天，终于在`stackoverflow`上找到了答案：`Retaining scroll position on Pull To Refresh`

`PullToRefresh`为了实现各种不同的`View`的下拉刷新，并不是简单的继承自`ListView`，而是采用了泛型。

实际上可以理解为在`ListView`（或者其他想要实现下拉刷新的View）外面包了一层`ParentView`

想要得到里面的`ListView`，有这样一个方法：

~~~java
listView.getRefreshableView();
~~~

### 解决

因此，想要让它回到顶部，代码如下：

~~~java
ListView mlist = listView.getRefreshableView();
if (!(mlist).isStackFromBottom()) {
	mlist.setStackFromBottom(true);
}
mlist.setStackFromBottom(false);
~~~

解决问题！