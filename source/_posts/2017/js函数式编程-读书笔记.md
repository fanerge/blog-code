---
title: js函数式编程-javascript函数式编程的简介
date: 2017-07-25 21:11:13
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	javascript函数式编程的简介

####	javascript案列
	```
	javascript函数式编程初试
	function splt (fn)　{
		return function (array) {
			return fn.apply(null, array)
		}
	}
	// example
	var addArrayElements = splt(function (x, y) {
		return x + y;
	})
	console.log(addArrayElements([1,2])) // 3
	
	function unsplat (fun) {
		return function () {
			return fun.call(null, _.toArray(arguments))
		};
	}
	let joinElments = unsplat(function (array) {
		return array.join(' ')
	})
	// example
	console.log(joinElments('@', '$', '*')) // @ $ *
	```
####	开始函数式编程
		直白的定义：函数式编程通过使用函数来将值转换成抽象单元，接着用于构建软件系统。
		以函数为抽象单元
		例子：
		```
		function parseAge (age) {
			if (!_.isString(age)) {
				throw new Error('expecting a string');
			}
			let a;
			console.log('attempting to parse an age')
			a = parseInt(age, 10);
			if (_.isNaN(a)) {
				console.log(["could not parse age:", age].join(' '));
				a = 0;
			}
			return a;
		}
		// example
		console.log(parseAge(34)) // throw Error
		
		// 如果我们想修改输出错误、信息和警告呈现的方式，较好的
		方式是将错误、信息和警告的概念抽象成不同的函数
		function fail(thing) {
			throw new Error(thing);
		}
		function warn(thing) {
			console.log(['WARNING:', thing].join(' '));
		}
		function note(thing) {
			console.log(['NOTE:', thing].join(' '));
		}

		function parseAge (age) {
			if (!_.isString(age)) {
				fail('Expecting a string');
			}
			let a;
			note('Attempting to parse an age')
			a = parseInt(age, 10);
			if (_.isNaN(a)) {
				warn(['Could not parse age:', age].join(' '));
				a = 0;
			}
			return a;
		}
		// example
		console.log(parseAge('df')) // throw Error
		```
		封装和隐藏（闭包）
		以函数为行为单位
		```
		function nativeNth(a, index) {
			return a[index];
		}
		var letters = ['a', 'b'];
		// example
		console.log(nativeNth(letters, 0)) // a
		
		// 
		function nth(a, index) {
			if (!_.isNumber(index)) {
				fail('Expected a number as the index');
			}
			if (!isIndexed(a)) {
				fail('Not supported on non-indexed type');
			}
			if ((index < 0) || (index > a.length -1)) {
				fail('Index value is out of bounds');
			}
			return a[index];
		}

		var letters = ['1', '2']
		console.log(nth(letters, 1))
		```
		数据抽象
			Javascript的对象原型模型是一个丰富且基础的数据方案。
		函数式javascript初试
			```
			// existy 定义事物的存在（js中null和undefined表示不存在）
			function existy(x) { return x != null }
			// example
			existy(null) // false
			
			truthy 用来判断一个对象是否应该被认为是true的同义词
			// js的原始真类型
			function truthy(x) {
				return (x !== false) && existy(x)
			}
			console.log(truthy('')) // true
		```
####	Underscore示例
####	总结				
		一种用于构建Javascript应用程序的技术称为“函数式编程”		
			1.确定抽象，并为其构建函数。
			2.利用已有的函数来构建更为复杂的抽象。
			3.通过将现有的函数传给其他的函数来构建更加复杂的抽象。

>	书籍【javascript函数式编程】
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			























