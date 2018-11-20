---
title: 网页长度相关API
date: 2018-11-05 21:23:12
tags: 'js'
categories: 'js'
copyright: true
---
#	元素的宽高
clientWidth、clientHeight：是指元素内容+内边距大小，不包括边框、外边距、滚动条部分。
offsetWidth、offsetHeight：是指元素内容+内边距大小+边框大小，不包括外边距和滚动条部分。
scrollWidth、scrollHeight：是指元素内容+内边距+对应方向的溢出部分。
#	元素的位置
clientLeft、clientTop：是指元素的内边距的外边缘和边框的外边缘的距离（其实边框的宽度）。
offsetLeft、offsetTop：元素的边框的外边缘距离与已定位的父容器（offsetparent）的左边距离（不包括元素的边框和父容器的边框）。
scrollTop、scrollLeft：获取或设置一个元素垂直或水平滚动的像素数。
pageXOffset、pageYOffset：返回文档在窗口左上角水平和垂直方向滚动的像素。
#	鼠标位置
event.clientX、event.clientY // 鼠标相对于视口左上角X,Y坐标（不包括工具栏和滚动条）
event.pageX、event.pageY // 鼠标相对于文档左上角X,Y坐标
event.offsetX、event.offsetY // 鼠标相对于事件源元素（srcElement）的X,Y坐标（只有ie支持）
event.screenX、event.screenY // 鼠标相对于用户显示器屏幕左上角的X,Y坐标。
#	其他度量
##	视口大小
document.documentElement.clientWidth
document.documentElement.clientHeight
##	页面实际大小
document.documentElement.scrollWidth
document.documentElement.scrollHeight
##	屏幕大小
window.screen.width
window.screen.height
##	屏幕可用宽度（去除状态栏）
window.screen.availWidth
window.screen.availHeight
##	窗口的内高度、内宽度（文档显示区域+滚动条）
window.innerWidth
window.innerHeight
##	窗口的外高度、外宽度
window.outerWidth
window.outerHeiht
##	返回元素的大小及其相对于视口的位置
ele.getBoundingClientRect()
##	返回一个指向客户端中每一个盒子的边界矩形的矩形集合 
ele.getClientRects()
##	返回一个应用活动样式表并解析这些值可能包含的任何基本计算后报告元素的所有CSS属性的值
window.getComputedStyle(element, [pseudoElt])