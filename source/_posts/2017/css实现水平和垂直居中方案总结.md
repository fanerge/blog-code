---
title: css实现水平和垂直居中方案总结
date: 2017-10-19 22:20:48
tags: ['css']
categories: 'css'
copyright: true
---
#	已知宽高元素水平和垂直居中
##	position:absolute、top和left50%和margin:-height/2px 0 0 -width/2px;
###	html 代码
	```
	<div class="container">
	 123
	</div> 
	```
###	css 代码
	```
	.container {
	   position:absolute;
	   top:50%;
	   left:50%;
	   margin:-25px 0 0 -50px;
	   width: 100px;
	   height: 50px;
	   font-size: 40px;
	   color: #fff;
	   background-color: rgba(0,0,0,.8)
	 }
	```
##	position:fixed、top和right和bottom和left0和margin:auto;
###	html 代码
	```
	<div class="container">
	 123
	</div> 
	```
###	css代码
	```
	.container {
	   position:fixed;
	   top:0;
	   right:0;
	   bottom:0;
	   left:0;
	   margin:auto;
	   width: 100px;
	   height: 50px;
	   font-size: 40px;
	   color: #fff;
	   background-color: rgba(0,0,0,.8)
	 }
	```
#	未知宽高元素水平和垂直居中	
	总结一些常用的不定高宽元素居中的方式，以备使用。
	以下代码均不考虑兼容性，如需使用，请自行处理浏览器兼容性。
##	display 为 table 布局（父容器为display: table; 子元素为display: table-cell;）
###	html 代码
	```
	<div id="container">  
      <span id="inner">  
       123123
      </span>  
	</div>  
	```
###	css 代码
	```
	#container{  
		display: table;  
		padding: 60% 30%;
		width: 100vw;
		height: 100vh;
		text-align: center;  
	}  
	#inner{  
		display: table-cell;  
		vertical-align: middle; 
		font-size: 40px;
		color: #fff;
		background-color: rgba(0, 0, 0, .8); 
	} 
	```
	PS：
	水平居中一般采用两种方式:
		1.块级元素：定宽 + margin:0 auto;（由于是不定宽高，所以这里不适用）；
		2.行内元素：作用于父级元素 text-algin：center；（可以内联化，视情况而定）	
	垂直方向居中：
		使用display:table-cell;vertical-algin:middle;
##	flex 布局实现
	注意：flex 方法兼容 IE10及以上版本
###	html 代码
	```
	<div class="container">
		<span class="toast">上传成功</span>
	</div>
	```
###	css 代码
	```
	/*父容器*/
	.container {
		position: fixed;
		width: 100vw;
		height: 100vh;
		display: flex;
		justify-content: center;
		align-items: center;
	} 
	/*子项目*/
	.toast {
		padding: 20px;
		font-size: 40px;
		color: #fff;
		background-color: rgba(0, 0, 0, .8);
	}
	```
	PS：display: flex; // 为父容器使用 flex 布局
		justify-content: center; // 决定子项目在主轴的对齐方式
		align-items: center; // 决定子项目在交叉轴轴的对齐方式
		
##	position + transform 方法实现
	注意：flex 方法兼容 IE9及以上版本
###	html 代码
	```
	<span class="toast">上传成功</span>
	```
###	css 代码
	```
	.toast {
		position: fixed;
		top: 50%; // 50vh
		left: 50%; // 50vw
		transform: translate(-50%, -50%);
		padding: 20px;
		font-size: 40px;
		color: #fff;
		background-color: rgba(0, 0, 0, .8);
	}
	```
	PS：对于position: fixed; 之后的 top和left都为50%，这个50%为浏览器窗口宽高的50%，
		对于transform中translate(-50%, -50%)，这个50%为自身元素的宽高的50%。
		或许上面改写成top: 50vh; left: 50vw; 更容易理解。

>	参考文档：
	[不定元素宽高用css实现内容水平和垂直都居中](http://www.tangshuang.net/3197.html)
	[不定宽高div水平垂直居中](http://jcao54.iteye.com/blog/2256953)
	[flex 布局](http://www.runoob.com/w3cnote/flex-grammar.html)
	[滴滴出行-再谈自适应垂直居中](https://juejin.im/post/586b94e5ac502e12d62d4ab6)