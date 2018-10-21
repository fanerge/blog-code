---
title: vue-基础知识
date: 2018-08-14 21:23:58
tags: 'vue'
categories: 'vue'
copyright: true
---
#	vue双向绑定原理
##	单向与双向
Model>View（单向）
Model<>View（单向）
##	双向的原理
通过数据劫持和发布者-订阅者模式的方式来实现。
1.	数据劫持主要通过 Object.defineProperty(obj, prop, descriptor) 的set和get方法执行对应的改变视图的方法。
2.	new Proxy(target, handler) 来实现数据劫持。


































