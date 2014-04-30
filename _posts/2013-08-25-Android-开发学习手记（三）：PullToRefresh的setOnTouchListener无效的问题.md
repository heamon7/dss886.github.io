---
layout: post
title: "Android 开发学习手记（三）：PullToRefresh的setOnTouchListener()无效的问题"
description: "Android开发中的一些问题及其解决办法"
category: Android
tags: [Android, PullToRefresh]
date: 2013-08-25 01:33
image:
  feature: abstract-6.jpg
comments: true
share: true
---

如果直接给PullToRefreshListView设置OnTouch()，会发现没有反应，这个函数根本没有被调用。

我之前的篇文章探讨过PullToRefresh的实质：[PullToRefresh如何滚动到最顶部，以及PullToRefresh的实质](http://www.dss886.com/android/2013/08/25/Android-%E5%BC%80%E5%8F%91%E5%AD%A6%E4%B9%A0%E6%89%8B%E8%AE%B0%EF%BC%88%E4%BA%8C%EF%BC%89%EF%BC%9APullToRefresh%E5%A6%82%E4%BD%95%E6%BB%9A%E5%8A%A8%E5%88%B0%E6%9C%80%E9%A1%B6%E9%83%A8%EF%BC%8C%E4%BB%A5%E5%8F%8APullToRefresh%E7%9A%84%E5%AE%9E%E8%B4%A8/)

想要给ListView设置setOnTouchListener()，直接给PullToRefreshListView设置是没有用的，要使用：

	listView.getRefreshableView().setOnTouchListener(new OnTouchListener(){...});

至于为什么在PullToRefreshListView的setOnTouchListener()里放Log都不显示（根本没调用），

这个问题仍然值得探讨，如果有人有答案，欢迎留言。