---
layout: post
title: "Android 开发学习手记（三）：PullToRefresh的setOnTouchListener()无效的问题"
description: "Android开发中的一些问题及其解决办法"
category: Android
tags: [Android, PullToRefresh]
date: 2013-08-25 01:15
comments: true
share: true
---

如果直接给PullToRefreshListView设置OnTouch()，会发现没有反应，这个函数根本没有被调用。

我之前的篇文章探讨过PullToRefresh的实质：[PullToRefresh如何滚动到最顶部，以及PullToRefresh的实质]()

想要给ListView设置setOnTouchListener()，直接给PullToRefreshListView设置是没有用的，要使用：

	listView.getRefreshableView().setOnTouchListener(new OnTouchListener(){...});

至于为什么在PullToRefreshListView的setOnTouchListener()里放Log都不显示（根本没调用），

这个问题仍然值得探讨，如果有人有答案，欢迎留言。