---
title: 梳理下浏览器对象模型知识（BOM）
date: 2018-03-12 20:41:17
tags: 'BOM'
categories: 'BOM'
copyright: true
---
本文系统的梳理了下BOM的5个对象（有一些非标准属性及方法），这里暂且不考虑兼容性。下文中的图片你可能可不太清楚，可以点击后面的链接下载大图查看，注红色部分为常使用的属性或方法。
#	BOM介绍
BOM(Browser Object Model) 是指浏览器对象模型，是用于描述这种对象与对象之间层次关系的模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象（Screen、Location、History、Navigator）。
浏览器对象模型的构成
![浏览器对象模型的构成](http://p5hb0ypha.bkt.clouddn.com/BOM.svg)
#	Window对象
Window对象，它表示浏览器窗口，在浏览器中最顶层的对象。
在浏览器中，每个标签具有自己的 window 对象 。也就是说，同一个窗口的标签之间不会共享一个 window 对象。
##	Window对象的属性
![Window对象的属性](http://p5hb0ypha.bkt.clouddn.com/window%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%B1%9E%E6%80%A7.svg)
[看不清，点这里](http://p5hb0ypha.bkt.clouddn.com/window%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%B1%9E%E6%80%A7.svg)
##	Window对象的方法
![Window对象的方法](http://p5hb0ypha.bkt.clouddn.com/window%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%96%B9%E6%B3%95%281%29.svg)
[看不清，点这里](http://p5hb0ypha.bkt.clouddn.com/window%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%96%B9%E6%B3%95%281%29.svg)
#	Location对象
Location 对象表示其链接到的对象的位置（URL）。所做的修改反映在与之相关的对象上。 
Document 和 Window 接口都有这样一个链接的Location，分别通过 Document.location和Window.location 访问。
![Location对象](http://p5hb0ypha.bkt.clouddn.com/location%E5%AF%B9%E8%B1%A1.svg)
[看不清，点这里](http://p5hb0ypha.bkt.clouddn.com/location%E5%AF%B9%E8%B1%A1.svg)
#	History对象
History 对象允许操作浏览器的曾经在标签页或者框架里访问的会话历史记录。
![History对象](http://p5hb0ypha.bkt.clouddn.com/history%E5%AF%B9%E8%B1%A1.svg)
[看不清，点这里](http://p5hb0ypha.bkt.clouddn.com/history%E5%AF%B9%E8%B1%A1.svg)
#	Navigator对象
Navigator 接口表示用户代理的状态和标识。 它允许脚本查询它和注册自己进行一些活动。
![Navigator对象](http://p5hb0ypha.bkt.clouddn.com/navigator%E5%AF%B9%E8%B1%A1.svg)
[看不清，点这里](http://p5hb0ypha.bkt.clouddn.com/navigator%E5%AF%B9%E8%B1%A1.svg)
#	Screen对象
Screen 对象包含有关用户屏幕的信息。
![Screen对象](http://p5hb0ypha.bkt.clouddn.com/screen%E5%AF%B9%E8%B1%A1.svg)
[看不清，点这里](http://p5hb0ypha.bkt.clouddn.com/screen%E5%AF%B9%E8%B1%A1.svg)
#	document对象
Document 对象提供了一些在浏览器服务中作为页面内容入口点而加载的一些页面，也就是 DOM 树。
![document对象](http://p5hb0ypha.bkt.clouddn.com/document%E5%AF%B9%E8%B1%A1.svg)
[看不清，点这里](http://p5hb0ypha.bkt.clouddn.com/document%E5%AF%B9%E8%B1%A1.svg)












