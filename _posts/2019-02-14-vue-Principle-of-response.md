---
layout: post
title: '关于vue的响应式原理'
tags: [code]
---


<!-- ![MVVM]({{site.img_url}}/MVVMPattern.png){:.center} -->

vue最独特的特性之一，是非侵入式的响应式系统，那么数据的改变是如何驱动视图更新的呢？了解响应式系统的工作原理，可以回避一些常见的问题。

当把一个普通的js对象传给Vue实例的data选项，Vue将遍历此对象的所有属性。并使用Object.defineProperty把这些属性全部转为getter/setter。

Object.defineProperty(obj,prop,descriptor),是Object对象的一个静态方法，直接在对象上定义新属性，或者修改对象上的现有属性，并返回该对象。该方法的第一个参数是用于定义属性的对象，第二个参数是要定义或修改的名称或者属性，第三个参数是正在定义或者修改属性的描述符。

一个getter是一个获取某个特定的属性的值的方法，一个setter是一个设定某个属性的值的方法。可以为一个对象定义getter或setter以支持新增的属性。



