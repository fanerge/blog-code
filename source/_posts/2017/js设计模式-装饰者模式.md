---
title: js设计模式-装饰者模式
date: 2017-10-16 22:42:20
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	装饰者模式
定义：装饰者(decorator)模式能够在不改变对象自身的基础上，在程序运行期间给对象动态的添加职责。
	装饰者用于通过重载方法的形式添加新功能，该模式可以在被装饰者前面或者后面加上自己的行为以达到特定的目的。
	与继承相比，装饰者是一种更轻便灵活的做法。
![图解装饰者模式](http://oxpnrlb4j.bkt.clouddn.com/%E8%A3%85%E9%A5%B0%E8%80%85%E5%8C%85%E8%A3%85%E6%B5%81%E7%A8%8B.png)
普通对象被装饰者包裹起来，就形成了装饰者模式。	
举例：
	雷霆战机（吃道具的例子）
		
#	雷霆战机（吃道具的例子）
介绍：现在我们假设正在开发一个小游戏--雷霆战机， 
	最开始我们使用最渣的飞机，只能发射普通子弹；
	吃一颗星，可以发射普通子弹和发射散弹 ；
	再吃一颗，可以发射普通子弹和散弹和跟踪导弹。
// 一级飞机	
```
var plane = {
	fire: function(){
		console.log('发射普通子弹');
	}
}
plane.fire(); // '发射普通子弹'
```
// 二级飞机	
```
var fire1 = plane.fire;
var shot = function() {
	console.log('发射散弹');
};
plane.fire = function () {
	fire1();
	shot();
};
plane.fire(); // '发射普通子弹' '发射散弹'
```
// 三级飞机
```
var fire2 = plane.fire;
var track = function() {
	console.log('发射跟踪导弹');
};
plane.fire = function () {
	fire2();
	track();
};
plane.fire(); // '发射普通子弹' '发射散弹' '发射跟踪导弹'
```
![装饰者执行过程](http://oxpnrlb4j.bkt.clouddn.com/%E8%A3%85%E9%A5%B0%E8%80%85%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B.png)
PS：这样给对象动态的增加职责的方式就没有改变对象自身，一个对象放入另一个对象就形成了一条装饰链（一个聚合对象）， 而上面的shot和track也就是装饰者、装饰函数 ，当函数执行时，会把请求转给链中的下一个对象。		
#	在 FUNCTION 原型上封装通用的装饰函数
// 在原函数之前执行	
```
Function.prototype.before=function(beforefn) {
	var _this = this; // 保存旧函数的引用
	return function() { // 返回包含旧函数和新函数的“代理”函数
		beforefn.apply(this,arguments); // 执行新函数,且保证this不被劫持,新函数接受的参数						   
		return _this.apply(this,arguments); // 也会被原封不动的传入旧函数,新函数在旧函数之前执行
	};
};	
```
// 在原函数之后执行	
```
Function.prototype.after = function(afterfn) {
	var _this = this;
	return function() {
		var ret = _this.apply(this,arguments);
		afterfn.apply(this,arguments);
		return ret;
	};
};
```
	
#	封装成单独函数（不污染原型）
// 在原函数之前执行
```
var before = function(fn, before) {
	return function() {
		before.apply(this, arguments);
		return fn.apply(this, arguments);
	};
};	

// 使用
before(func1, func2);
```
// 在原函数之后执行
```
var after = function(fn, after) {
	return function() {
		var ret = fn.apply(this,arguments);
		after.apply(this,arguments);
		return ret;
	};
};	

// 使用
after(func1, func2);
```
PS：代码来源于--腾讯.曾探《JavaScript设计模式与开发实践》，但能很好的说明装饰者模式在js实际项目中的应用。
上面封装的函数可以直接在项目中使用。
>	参考文档：
[JavaScript设计模式----装饰者模式](http://blog.csdn.net/yisuowushinian/article/details/51934008)
[深入理解JavaScript系列（29）：设计模式之装饰者模式](http://www.cnblogs.com/TomXu/archive/2012/02/24/2353434.html)