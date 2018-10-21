---
title: js设计模式-中介者模式
date: 2017-10-16 22:41:28
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	中介者模式
定义：中介者模式的作用就是解除对象与对象之间的紧耦合关系。增加一个中介者对象后，
所有的相关对象都通过中介者对象来通信，而不是互相引用，所以当一个对象发生改变时，
只需要通知中介者对象即可。中介者使各对象之间耦合松散，而且可以独立地改变它们之间的交互。
中介者模式使网状的多对多关系变成了相对简单的一对多关系。
举例：
	手机购买页面（颜色、数量、内存、价格）
	MVC模式（控制层便是位于表现层与模型层之间的中介者。）
		
#	MVC 模式
我们应该很熟悉 MVC 三层模型实体模型（Model）、视图表现层（View）还有控制层（Control/Mediator）。	
```
/** 模擬 Model, View, Controller */
var M = {}, V = {}, C = {};

/** Model 負責存放資料 */
M.data = "hello world";

/** View 負責將資料輸出到螢幕上 */
V.render = (M) => { alert(M.data); }

/** Controller 作為一個 M 和 V 的橋樑 */
C.handleOnload = () => { V.render(M); }

/** 在網頁讀取的時候呼叫 Controller */
window.onload = C.handleOnload;
```
MVC 模式中的Control/Mediator 层，就是本设计模式的中介者 (它必须拿到 View 和 Model 的引用)。
	
#	手机购买页面
##	HTML 部分
```
<body>
	选择颜色: 
	<select id="colorSelect">
		<option value="">请选择</option>
		<option value="red">红色</option>
		<option value="blue">蓝色</option>
	</select>
	选择内存: 
	<select id="memorySelect">
		<option value="">请选择</option>
		<option value="32G">32G</option>
		<option value="16G">16G</option>
	</select>
	输入购买数量: <input type="text" id="numberInput"/><br/>
	<!--输入部分结束-->
	
	您选择了颜色: <div id="colorInfo"></div><br/>
	您选择了内存: <div id="memoryInfo"></div><br/>
	您输入了数量: <div id="numberInfo"></div><br/>
	<button id="nextBtn" disabled="true">请选择手机颜色和购买数量</button>
<body>
```
##	JS部分
```
// 各种手机库存（通常来自于后端，这里前端进行模拟）
var goods = { 
	"red|32G": 3,
	"red|16G": 0,
	"blue|32G": 1,
	"blue|16G": 6
};

// 中介者
var mediator = (function(){
	// 获得所有节点的引用，以便对其进行操作（中介者必许获得对其他对象的引用）
	var colorSelect = document.getElementById( 'colorSelect' ),
		memorySelect = document.getElementById( 'memorySelect' ),
		numberInput = document.getElementById( 'numberInput' ),
		colorInfo = document.getElementById( 'colorInfo' ),
		memoryInfo = document.getElementById( 'memoryInfo' ),
		numberInfo = document.getElementById( 'numberInfo' ),
		nextBtn = document.getElementById( 'nextBtn' );
	return {
		changed( obj ){
			var color = colorSelect.value, // 颜色
				memory = memorySelect.value,// 内存
				number = numberInput.value, // 数量
				stock = goods[ color + '|' + memory ]; // 颜色和内存对应的手机库存数量
			if ( obj === colorSelect ){ // 如果改变的是选择颜色下拉框
				colorInfo.innerHTML = color;
			}else if ( obj === memorySelect ){
				memoryInfo.innerHTML = memory;
			}else if ( obj === numberInput ){
				numberInfo.innerHTML = number;
			}
			if ( !color ){
				nextBtn.disabled = true;
				nextBtn.innerHTML = '请选择手机颜色';
				return;
			}
			if ( !memory ){
				nextBtn.disabled = true;
				nextBtn.innerHTML = '请选择内存大小';
				return;
			}
			if ( ( ( number - 0 ) | 0 ) !== number - 0 ){ // 输入购买数量是否为正整数
				nextBtn.disabled = true;
				nextBtn.innerHTML = '请输入正确的购买数量';
				return;
			}
			nextBtn.disabled = false;
			nextBtn.innerHTML = '放入购物车';
		}
	}
})();

// 与中介者联系起来，事件函数
colorSelect.onchange = function(){
	mediator.changed( this );
};
memorySelect.onchange = function(){
	mediator.changed( this );
};
numberInput.oninput = function(){
	mediator.changed( this );
};
```
PS：代码来源于--腾讯.曾探《JavaScript设计模式与开发实践》，但能很好的说明中介者模式在js实际项目中的应用。
PS：这里共有手机颜色、手机内存、手机数量的选择和展示共6个对象，和一个中介者对象。
	中介者必须获得这6个对象的应用，当每个对象发生变化时都需要通知中介者，中介者再来执行具体操作。
		
>	参考文档：
	[js之中介模式](http://blog.csdn.net/linhongyong/article/details/53439723)
	[wiki-MVC](https://zh.wikipedia.org/wiki/MVC)