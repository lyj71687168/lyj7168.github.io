---
layout: post
title: '关于vue的生命周期'
tags: [code]
---


就像一个人的生命中会经历很多阶段，每个阶段做不同的事情。vue生命周期就是指vue实例或者组件从诞生到消亡的每个阶段。在这个阶段的前后都可以设置一些函数(钩子函数)当做事件来调用。

![lifecycle]({{site.img_url}}/lifecycle.png){:.center .middle-img}
如上图，在created钩子函数之前一步data和methods已经准备好，可以在created函数里面调用里面的数据和方法了。

在mounted钩子函数中，页面已经加载完毕了。

当数据发生变化时，首先会触发beforeUpdate钩子函数。
