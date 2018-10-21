---
title: js设计模式-桥接模式
date: 2017-10-22 08:57:14
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	桥接模式基础
定义：桥接模式（Bridge）将抽象部分与它的实现部分分离，使它们都可以独立地变化。
使用场景：事件回调函数、请求接口之间进行桥接、用于连接公开的API代码和私用实现的代码
根据javascript语言的特点，我们将其简化成2个角色：
（1）扩充抽象类
（2）具体实现类

#	最简单的桥接模式
```
var each = function (arr, fn) {
	for (var i = 0; i < arr.length; i++) {
		var val = arr[i];
		if (fn.call(val, i, val, arr)) {
			return false;
		}
	}
};
var arr = [1, 2, 3, 4];
each(arr, function (i, v) {
	arr[i] = v * 2;
});
```
PS：在这个例子中，抽象部分是each函数，也就是上面说的扩充抽象类，实现部分是fn，即具体实现类。抽象部分和实现部分可以独立的进行变化。这个例子虽然简单，但就是一个典型的桥接模式的应用。

#	事件监控
抽象类 click 事件
```
addEvent(element, 'click', getBeerByIdBridge);
// 提供一个桥梁 将 抽象类和实现类链接起来
// 作为桥梁
function getBeerByIdBridge (e) {
　　getBeerById(this.id, function(beer) {
　　　　console.log('Requested Beer: '+beer);
　　});
}
```
实现类
```
function getBeerById(id, callback) {
	// 通过ID发送请求，然后返回数据
	asyncRequest('GET', 'beer.uri?id=' + id, function(resp) {
		// callback response
		callback(resp.responseText);
	});
}
```
PS：这里的getBeerByIdBridge就是我们定义的桥，用于将抽象的click事件和getBeerById连接起来，同时将事件源的ID，以及自定义的call函数（console.log输出）作为参数传入到getBeerById函数里。

#	用于连接公开的API代码和私用实现的代码
```
var Public=function(){
	// 定义的私有变量
	var secret = 3;
	// 该函数返回了私有变量，外界可以通过该方法访问该私有变量
	this.privilegedGetter = function(){
		 return secret;
	}
}
var o = new Public();
var data =o.privilegedGetter();
```
PS：如果一个公用的接口抽象了一些也许应该属于私用性的较复杂的任务，那么可以使用桥接模式来收集某些私用性的信息。可以用一些具有特殊权利的方法作为桥梁以便访问私用变量空间。这一特例中的桥接性函数又称特权函数。
#	用桥接模式联结多个类
```
var Class1 =function(a,b,c){
	this.a =a;
	this.b = b;
	this.c = c;
}
var Class2 =function(d){
	this.d = d;
}
var BridgeClass =function(a,b,c,d){
	   this.one = new Class1(a,b,c);
	   this.two = new Class2(d);
}
```
PS：这看起来很像是----适配器，的确如此。但要注意到本例中实际上并没有客户系统要求提供数据。它只不过是用来接纳大量数据并将其发送给责任方的一种辅助性手段。此外，BridgeClass也不是一个客户系统已经实现的现有接口。引入这个类的目的只不过是要桥接一些类而已。
	
>	参考文档：
	[深入理解JavaScript系列（44）：设计模式之桥接模式](http://blog.csdn.net/pigpigpig4587/article/details/25993191)
	[Javascript设计模式理论与实战：桥接模式](http://www.cnblogs.com/lrzw32/p/4957643.html)