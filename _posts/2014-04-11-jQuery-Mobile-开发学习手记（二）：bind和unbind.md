---
layout: post
title: "jQuery Mobile 开发学习手记（二）：bind和unbind"
description: "jQuery Mobile移动开发中的一些问题及其解决办法"
category: jQuery Mobile
tags: [jQuery, jQuery Mobile, 按钮, 监听, 事件绑定, bind, unbind]
date: 2014-04-11 18:23
image:
  feature: abstract-6.jpg
comments: true
share: true
---

### 问题

在项目过程中，需要一组图片设置长按响应：长按弹出一个对话框，点击确认删掉当前图片，然后刷新页面。

在最开始的时候，我是这样写的：

~~~javascript
for(i = 0; i < bookNum; i++){
	(function(){
		var index = i;
		var bookHref = $('a#book_'+index);
		bookHref.bind("taphold", {index: index}, tapholdHandler);
	})();
}
function tapholdHandler(event) {
	$("#dialogDelete").popup("open");
	var dialogCommit = $('#dialogCommit');
	dialogCommit.click(function(){
		//删除书，然后刷新页面。
	});
	$('#dialogCancel').click(function(){
		$("#dialogDelete").popup("close");
	});
}
~~~

然后发现，第一次长按删除的时候是正常的，第二次开始就乱七八糟的，多删了很多东西。

通过各种输出log调试，发现删除第一张图片的时候正常，删除第二张图片的时候会连续删两张，删除第三张图片的时候会连续删三张……

### 探究

这就让我有点怀疑，是不是每次刷新页面的时候注册监听的问题？

百度了一下，果然如此，跟Java不同，JavaScript给界面元素绑定事件并不会替换原来绑定过的事件，而是会重复绑定。

还是上一节说的问题，Java的机制是“设置监听”，JavaScript的机制是“绑定事件”。
明白这一点后就好解决了。因为我删掉一张图片以后会刷新一下页面，即会重复绑定一次。

### 解决

解决办法就是在每次绑定之前，先用unbind解绑一次。

~~~javascript
for(i = 0; i < bookNum; i++){
	(function(){
		var index = i;
		var bookHref = $('a#book_'+index);
		bookHref.unbind("taphold",tapholdHandler);
		bookHref.bind("taphold", {nid: json_books[index].nid},tapholdHandler);
	})();
}
function tapholdHandler(event) {
	$("#dialogDelete").popup("open");
	var dialogCommit = $('#dialogCommit');
	dialogCommit.unbind("click");
	dialogCommit.click(function(){
		//删除书，然后刷新页面
	});
	$('#dialogCancel').click(function(){
		$("#dialogDelete").popup("close");
	});
}
~~~