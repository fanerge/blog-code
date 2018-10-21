---
title: js面向对象编程-非构造函数的继承
date: 2017-08-13 16:08:41
category: "面向对象编程"
tags: ['js','面向对象编程']
---
重新复习 -- js面向对象编程知识，本文介绍-对象之间的非构造函数实现"继承"。

####	先来两个对象（父类和子类）
	```
	let Chinese = {
		nation: '中国'
	};

	let Doctor = {
		career: '医生'
	};	
	```

####	一、object()方法
	```
	let Chinese = {
		nation: '中国'
	};
	
	function object (o) {
		function F () {};
		F.prototype = o;
		return new F();
	}
	
	let Doctor = object(Chinese);
	Doctor.career = '医生';
	console.log(Doctor.nation) // 中国
	```
	这个object()函数，其实只做一件事，就是把子对象的prototype属性，指向父对象，从而使得子对象与父对象连在一起。

####	浅拷贝
	把父对象的属性，全部拷贝给子对象，也能实现继承。
	```
	let Chinese = {
		nation: '中国'
	};

	function extendCopy (p) {
		let c = {};
		for (let i in p) {
			c[i] = p[i];
		}
		return c;
	}
	let Doctor = extendCopy(Chinese);
	Doctor.career = '医生';
	console.log(Doctor.nation) // 中国
	```
	存在问题：只拷贝基本类型，对于引用类型拷贝内存地址，存在父对象别篡改的可能。

####	深拷贝
	所谓"深拷贝"，就是能够实现真正意义上的数组和对象的拷贝。它的实现并不难，只要递归调用"浅拷贝"就行了。
	```
	let Chinese = {
		nation: '中国',
		cities: ['成都', '北京']
	};
	function deepCopy(p, c) {
		var c = c || {};
		for (let i in p) {
			if (typeof p[i] === 'object') {
				c[i] = (p[i].constructor === Array) ? [] : {};
				deepCopy(p[i], c[i]);
			} else {
				c[i] = p[i];
			}
		}
		return c;
	} 
	let Doctor = deepCopy(Chinese);
	Doctor.career = '医生';
	console.log(Doctor.cities)  // ["成都", "北京"]	
	```

>	参考：
	[阮老师-Javascript面向对象编程（三）：非构造函数的继承](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance_continued.html)



































