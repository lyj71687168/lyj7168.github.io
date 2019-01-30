---
layout: post
title: '关于vue的MVVM框架理解'
tags: [code]
---


![MVVM]({{site.img_url}}/MVVMPattern.png){:.center}

Vue框架借鉴了MVVM的思想，MVVM是Model-View-ViewModel的简写。让我们将视图和业务逻辑分开，不再需要获取DOM元素进行业务逻辑的处理。

Model是业务逻辑相关的数据对象，View是呈现出来的界面，ViewModel是视图模型。

从上图中可以看出，ViewModel作为中间的角色，取出 Model 的数据，而这个数据往往是需要我们经过处理后呈现到页面上的，那么ViewModel便承担了这个任务，在Vue中，把vue实例看做ViewModel,所以官方文档中vue实例简写为vm。ViewModel进行业务逻辑处理后的数据通过双向数据绑定到View视图上，当view视图产生变化后，ViewModel可以把数据更新至Model。


