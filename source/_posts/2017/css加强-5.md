---
title: css加强（多形状图像和心跳灯和竖着排的文字和面包屑导航）
date: 2017-09-04 21:55:45
category: "css"
tags: ['css','web开发','读书笔记']
---
在接下来的一段时间学习看云上购买的一本[Web开发实战](https://www.kancloud.cn/dennis/javascriptmethod/261472)书籍，来提升自己的软实力。
本部分为css实战部分。
####	多形状图像
	可以实现多形状图像
	border-radius: top right bottom left;
####	心跳灯
	```
	.heartbeat {
		width: 100px;
		height: 100px;
		background: red;
		animation: heartbeat .83s ease-in-out infinite;
	}
	@keyframes heartbeat {
		from {
			opacity:0.1;
		}
		50% {
			opacity:1;
		}
		to {
			opacity:0.1;
		}
	}
	```
####	竖着排的文字
	writing-mode: horizontal-tb | vertical-lr | vertical-rl;
		horizontal-tb表示水平方向自上而下的书写方式。
		vertical-rl表示垂直方向自右向左的书写方式
		vertical-lr表示垂直方向自左向右的书写方式
	IE
	writing-mode: lr-tb | tb-rl;
		lr-tb水平方向自左向右的书写方式
		tb-rl垂直方向自上而下的书写方式。
####	首字母下沉
	::first-letter伪元素选择器用于选取指定选择器的首字母。
	```
	p::first-letter {
		color:#c69c6d;
		float:left;
		font-size:5em;
		margin:0 .2em 0 0;
	}
	```
	
>	参考文档
1.	[web开发实战](https://www.kancloud.cn/dennis/javascriptmethod/261473)
3.	[linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient3.	[transform](http://www.runoob.com/cssref/css3-pr-transform.html)
	[代码仓库](https://github.com/fanerge/web-/tree/master)