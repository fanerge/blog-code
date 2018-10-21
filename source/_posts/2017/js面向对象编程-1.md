---
title: js面向对象编程-构造函数的继承
date: 2017-08-13 14:52:40
category: "面向对象编程"
tags: ['js','面向对象编程']
---
重新复习 -- js面向对象编程知识，本文介绍-对象之间的"继承"的五种方法。

####	先来两个构造函数（父类和子类）
	现有一个"动物"对象的构造函数
	```
	function Animal () {
		this.species = "动物";
	}
	```
	再来一只"猫"对象的构造函数
	```
	function Cat (name, color) {
		this.name = name;
		this.color = color;
	}
	```
	
####	一、构造函数绑定
	使用call或apply方法，将父对象的构造函数绑定在子对象上。	
	```
	function Cat (name, color) {
		Animal.call(this, arguments); // 构造函数绑定-继承
		this.name = name;
		this.color = color;
	}

	var cat1 = new Cat('小白', '红色');
	console.log(cat1); // {species: "动物", name: "小白", color: "红色"}
	```
	
####	二、prototype模式
	```
	function Animal () {
		this.species = "动物";
	}

	function Cat (name, color) {
		this.name = name;
		this.color = color;
	}
	Cat.prototype = new Animal(); // prototype模式-继承
	Cat.prototype.constructor = Cat; // 重新将constructor指向Cat

	var cat1 = new Cat('小白', '红色');
	console.log(cat1.species); // 动物
	```
	
####	三、直接继承prototype	
	第三种方法是对第二种方法的改进。由于Animal对象中，不变的属性都可以直接写入Animal.prototype。所以，我们也可以让Cat()跳过 Animal()，直接继承Animal.prototype。
	```
	function Animal () { }
	Animal.prototype.species = '动物';

	function Cat (name, color) {
		this.name = name;
		this.color = color;
	}
	  
	Cat.prototype = Animal.prototype; // 直接继承prototype
	Cat.prototype.constructor = Cat;

	var cat1 = new Cat('小白', '红色');
	console.log(cat1.species); // 动物
	```
	与前一种方法相比，这样做的优点是效率比较高（不用执行和建立Animal的实例了），比较省内存。缺点是 Cat.prototype和Animal.prototype现在指向了同一个对象，那么任何对Cat.prototype的修改，都会反映到Animal.prototype。
	console.log(Animal.prototype.constructor); // Cat
	
####	四、利用空对象作为中介
	```
	function Animal () { }
	Animal.prototype.species = '动物';

	function Cat (name, color) {
		this.name = name;
		this.color = color;
	}

	// 空对象作为中介
	let F = function () {};
	F.prototype = Animal.prototype;

	Cat.prototype = new F(); 
	Cat.prototype.constructor = Cat;

	var cat1 = new Cat('小白', '红色');
	console.log(cat1.species); // 动物
	```
	还可以单独封装成方法
	```
	let extend = function (child, parent) {
		let F = function () {};
		F.prototype = parent.prototype;
		child.prototype = new F();
		child.prototype.constructor = child;
	};
	extend(Cat, Animal);
	let cat1 = new Cat('小白', '红色');
	console.log(cat1.species); // 动物
	```
	
####	五、拷贝继承
	简单说，如果把父对象的所有属性和方法，拷贝进子对象。	
	```
	function Animal () { }
	Animal.prototype.species = '动物';

	function Cat (name, color) {
		this.name = name;
		this.color = color;
	}

	// 拷贝继承
	function extend2 (child, parent) {
		let p = parent.prototype;
		let c = child.prototype;
		for (let i in p) {
			c[i] = p[i];
		}
	}
	extend2(Cat, Animal);
	let cat1 = new Cat('小白', '红色');
	console.log(cat1.species) // 动物
	```
	
>	参考：
	[阮老师-Javascript面向对象编程（二）：构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	