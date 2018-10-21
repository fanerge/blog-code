---
title: js面向对象编程-"封装"（Encapsulation）
date: 2017-08-13 10:44:00
category: "面向对象编程"
tags: ['js','面向对象编程']
---
重新复习 -- js面向对象编程知识

###	封装
	```
	function Cat (name, color) {
		this.name = name; // 使用动态作用域this
		this.color = color;
	}
	Cat.prototype.type = '猫科动物';
	Cat.prototype.eat = function () {
		console.log('吃老鼠');
	};

	let cat1 =new Cat('大毛', '黄色');
	let cat2 =new Cat('小毛', '黑色');
	```
	
####	constructor --- 每个实例都有一个constructor属性，指向它们的构造函数
	// console.log(cat1.constructor == Cat) true

####	instanceof --- 验证原型对象与实例之间的关系
	// console.log(cat1 instanceof Cat) true
	// console.log(cat1.type); 猫科动物
	// console.log(cat1.type === cat2.type) true 都从原型中获得
	// cat1.eat() 吃老鼠

#####	isPrototypeOf() --- 某个proptotype对象和某个实例之间的关系。
	// console.log(Cat.prototype.isPrototypeOf(cat1)) true

#####	hasOwnProperty() --- 每个实例对象都有一个hasOwnProperty()方法，用来判断某一个属性到底是本地属性，还是继承自prototype对象的属性。
	// console.log(cat1.hasOwnProperty('name')); true
	// console.log(cat1.hasOwnProperty('eat')); false

#####	in运算符 --- 某个实例是否含有某个属性，不管是不是本地属性
	// console.log('name' in cat1); true
	// console.log('eat' in cat1); true
	

####	总结对象属性的遍历
	1.	in --- 遍历**可枚举的自身属性和继承属性**
	2.	Object.getOwnPropertyNames() --- 遍历所有的自身属性
	3.	Object.keys(obj) --- 遍历**可枚举的自身属性**，返回一个属性数组

>	参考：
	[阮老师-Javascript面向对象编程（一）：封装](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html)
























