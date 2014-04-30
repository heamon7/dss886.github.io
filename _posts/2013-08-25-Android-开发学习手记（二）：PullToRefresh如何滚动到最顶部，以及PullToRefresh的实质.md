---
layout: post
title: "Android 开发学习手记（二）：PullToRefresh如何滚动到最顶部，以及PullToRefresh的实质"
description: "Android开发中的一些问题及其解决办法"
category: Android
tags: [Android, PullToRefresh]
date: 2013-08-25 01:15
comments: true
share: true
---

如果用ListView，让它滚动到顶部，一般是这样写的：

	if (!listView.isStackFromBottom()) {
		listView.setStackFromBottom(true);
	}
	listView.setStackFromBottom(false);

但是，使用PullToRefreshListView以后，发现该对象竟然没有setStackFromBottom()方法！

翻翻它的源码，发现是这样的：

	public class PullToRefreshListView extends PullToRefreshAdapterViewBase<ListView>{...}

它并不是继承于ListView，所以也无法将这个对象cast到ListView。

但是，实际上PullToRefreshListView的主体确实是一个ListView，那么如何使用属于ListView的方法呢？

在百度、谷歌搜索了半天，终于在stackoverflow上找到了答案：Retaining scroll position on Pull To Refresh

PullToRefresh为了实现各种不同的View的下拉刷新，并不是简单的继承自ListView，而是采用了泛型。

实际上可以理解为在ListView（或者其他想要实现下拉刷新的View）外面包了一层ParentView

想要得到里面的ListView，有这样一个方法：

	listView.getRefreshableView();

因此，想要让它回到顶部，代码如下：

	ListView mlist = listView.getRefreshableView();
	if (!(mlist).isStackFromBottom()) {
		mlist.setStackFromBottom(true);
	}
	mlist.setStackFromBottom(false);

解决问题！