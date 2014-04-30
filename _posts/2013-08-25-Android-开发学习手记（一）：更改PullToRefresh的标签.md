---
layout: post
title: "Android 开发学习手记（一）：分别更改PullToRefresh的上下标签"
description: "Android开发中的一些问题及其解决办法"
category: Android
tags: [Android, PullToRefresh]
date: 2013-08-25 00:56
image:
  feature: abstract-6.jpg
comments: true
share: true
---

这篇文章主要介绍如何分别更改PullToRefresh组件的上拉和下滑标签。

PullToRefresh是一个Android上很成熟的下拉刷新的开源控件，目前托管在GitHub上：<https://github.com/chrisbanes/Android-PullToRefresh>

当PullToRefresh的Mode设为BOTH时，即上下都可以刷新时，下拉/上拉默认的英文都是：“Pull to refresh”

可是上拉、下拉的英文都是Pull，汉字总不能都写“下拉刷新”吧？

粗看了一眼，有这个方法：

	listView.setRefreshingLabel(String refreshingLabel);

然后发现它被弃用了：

	Deprecated. You should now call this method on the result ofgetLoadingLayoutProxy().

调用getLoadingLayoutProxy()，发现它还是只有`setPullLabel(String)`、`setReleaseLabel(String)`、`setRefreshingLabel(String)`等几个方法，设置以后上下的标签都变了，怎么办？

研究了俩小时。。。发现除了`getLoadingLayoutProxy()`，还有这一个：

	getLoadingLayoutProxy(boolean includeStart, boolean includeEnd)

哦，原来得到Proxy的时候可以指定是Start还是End

如果想使上下标签显示不同的文字，可以这样设置：

	listView.getLoadingLayoutProxy(true,false).setPullLabel("下拉加载上一页");
	listView.getLoadingLayoutProxy(false,true).setPullLabel("上拉加载下一页");
	listView.getLoadingLayoutProxy(true,true).setReleaseLabel("松开加载");
	listView.getLoadingLayoutProxy(true,true).setRefreshingLabel("正在加载");

解决问题！