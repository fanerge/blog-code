---
title: js函数式编程-变量的作用域和闭包
date: 2017-07-30 10:42:20
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	变量的作用域和闭包

####	全局作用域
	动态作用域 -- this（函数调用时确定的）
	静态作用域（词法作用域）-- 函数声明时确定的
	
####	词法作用域
	定义：所谓的词法作用域其实是指作用域在词法解析阶段既确定了,不会改变。
	```
	var aVariable = 'outer';
	function afunc(){
		let aVariable = 'middle';
		return _.map([1, 2, 3], function (e){
			let aVariable = 'in'; // 使用最内层的aVariable
			return [aVariable,e].join(' ');
		});
	}
	console.log(afunc()) // ["in 1", "in 2", "in 3"]
	```
	
####	动态作用域
	```
	var globals = {};
	function makeBindFun(resolver){
		return function(k, v){
			let stack = globals[k] || [];
			globals[k] = resolver(stack, v);
			return globals;
		}
	}
	var stackBinder = makeBindFun(function(stack, v){
		stack.push(v);
		return stack;
	});

	var stackUnBinder = makeBindFun(function(stack, v){
		stack.pop(v);
		return stack;
	});

	var dd = stackBinder('sd', 'dd')
	console.log(dd) //{sd: ['dd']}
	```
	
	javascript的动态作用域（对this引用的讨论）
	```
	*bind* 锁定this使其不被更改
	function globalThis() {
		return this;
	}
	var nopeThis = _.bind(globalThis, 'nope');
	nopeThis.call('wat') // 'nope';
	
	*bindAll* 锁定this引用到对应的命名函数
	var target = {
		name: 'the right value',
		aux: function () {
			return this.name;
		}
	}
	var dd = _.bindAll(target, 'aux')
	console.log(target.aux.call('wat')) // the right value
	```
	
####	函数作用域
	```
	*clone* 克隆一个对象
	function f(){
		this['a'] = 200;
		return this['a'] + this['b'];
	}
	var globals = {'b': 2};
	var dd = f.call(_.clone(globals))
	console.log(dd) // 202
	```
	
####	闭包
	模拟闭包
	```
	function whatWasTheLocal() {
		let CAPTURED = "Oh hai";
		return function () {
			return "The local was:" + CAPTURED;
		}
	}
	var dd = whatWasTheLocal();
	console.log(dd()) // The local was:Oh hai
	
	function createWeirdScaleFunction(FACTOR) {
		return function (v) {
			this['FACTOR'] = FACTOR;
			let captures = this;
			return _.map(v, _.bind(function (n) {
				return (n * this['FACTOR']);	
			}, captures));
		}
	}

	let scale10 = createWeirdScaleFunction(10);
	let dd = scale10.call({}, [3, 4, 5])
	console.log(dd)
	```
	遮蔽
	```
	var shadowed = 220;
	function argShadow(shadowed) {
		return ['value is', shadowed].join();
	}

	var dd = argShadow()
	console.log(dd)
	```
	使用闭包
	```
	let pingpong = (function () {
		let PRIVATE = 0;
		return {
			inc: function (n) {
				return PRIVATE += n;
			},
			dec: function (n) {
				return PRIVATE -= n;
			}
		};
	})();

	let dd = pingpong.inc(10);
	console.log(dd) // 10
	```
	闭包的抽象
	```
	function plucker (FIELD) {
		return function (obj) {
			return (obj && obj[FIELD])
		}
	}
	let best = {title: 'infinite jest', author: 'dfw'}
	let getTitle = plucker('title')
	let dd = getTitle(best);
	console.log(dd) // infinite jest
	```	
>	源代码仓库
	[函数式编程-读书笔记](https://github.com/fanerge/Functional-reading-notes)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	