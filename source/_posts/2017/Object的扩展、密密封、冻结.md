---
title: Object的扩展、密密封、冻结
date: 2017-10-10 19:56:38
category: "js"
tags: ['js']
copyright: true
---
##	扩展特性
	Object.isExtensible(obj)
		判断一个对象是可扩展(是否能有新的属性添加到它)。
	Object.preventExtensions(obj)
		可以对对象的属性进行修改和删除，不能向自身添加属性但可以向其原型添加属性。
	示例：
	```
	// Object.create(proto[, propertiesObject]);
	var obj = {
		a: 1,
		b: 2
	};
	console.log(Object.isExtensible(obj)); // true
	Object.preventExtensions(obj);
	console.log(Object.isExtensible(obj)); // false
	// obj.a = 3; // 可以修改原有属性
	// delete obj.a; // 可以删除原有属性
	// obj.c = 3; // 不能自身添加属性		
	console.log(obj);
	```
##	密封特性
	Object.isSealed()
		判断一个对象是密封的。
	Object.seal()
		密封对象是指那些不能添加新的属性，不能删除已有属性，以及不能修改已有属性的可枚举性、可配置性、可写性，但可能可以修改已有属性的值的对象。
		属性不可配置的效果就是属性变的不可删除，以及一个数据属性不能被重新定义成为访问器属性。
		将所有现有的属性标记为不可配置。现在的属性的值仍然可以改变,只要它们是可写的。
	示例：
	```
	var obj = {
	  prop: function() {},
	  foo: 'bar'
	};
	var o = Object.seal(obj);
	console.log(o === obj); // true
	Object.isSealed(obj); // === true

	obj.foo = 'quux'; // 可以改变属性

	// 不能改变属性访问器，会抛出错误
	Object.defineProperty(obj, 'foo', {
	  get: function() { return 'g'; }
	}); // throws a TypeError

	// 不能添加新属性
	obj.quaxxor = 'the friendly duck';

	// 不能删除原有属性
	delete obj.foo;

	// 可以更改属性，只要它是可写的
	Object.defineProperty(obj, 'foo', {
	  value: 'eit'
	}); 
	```
##	冻结特性
	Object.isFrozen()
	Object.freeze()
		防止新的属性被添加到它;防止现有的属性被移除;
		和防止现有的属性,或他们的可数性,可配置性,或可写性,被改变了,它还可以防止原型被改变了。
		该方法返回对象处于冻结状态。
	示例：
	```
	var obj = {
	  prop: function() {},
	  foo: 'bar'
	};

	// 返回已被冻结的对象
	var o = Object.freeze(obj);

	// o === obj; // true
	Object.isFrozen(obj); // === true

	// 改变原有属性失败
	obj.foo = 'quux'; // silently does nothing
	// 添加属性失败
	obj.quaxxor = 'the friendly duck';
	// 删除原有属性失败
	delete obj.foo; // throws a TypeError
	console.log(obj);

	// 重新配置原有属性失败
	Object.defineProperty(obj, 'ohai', { value: 17 });
	Object.defineProperty(obj, 'foo', { value: 'eit' });

	// 向原型中添加属性失败
	Object.setPrototypeOf(obj, { x: 20 })
	obj.__proto__ = { x: 20 }
	```
##	浅冻结与深冻结
	如该方法 MDN 的描述所述，倘若一个对象的属性是一个对象，
	那么对这个外部对象进行冻结，内部对象的属性是依旧可以改变的，这就叫浅冻结，
	若把外部对象冻结的同时把其所有内部对象甚至是内部的内部无限延伸的对象属性也冻结了，这就叫深冻结。
	```
	(function () {
		obj = {
			internal :{}
		};
		Object.freeze(obj);//浅冻结
		obj.internal.a = "aValue";
		console.log(obj.internal.a);//"aValue"

		//想让一个对象变得完全冻结,冻结所有对象中的对象,可以使用下面的函数.
		function deepFreeze(o){
			var prop,propKey;
			Object.freeze(o);//首先冻结第一层对象
			for(propKey in o){
				prop = o[propKey];
				if(!o.hasOwnProperty(propKey) || !(typeof prop === "object") || Object.isFrozen(prop)){
					continue;
				}
				deepFreeze(prop);//递归
			}
		}
			
		deepFreeze(obj);
		obj.internal.b = "bValue";//静默失败
		console.log(obj.internal.b);//undefined
	})();
	```
>	参考文档：
	[浅谈 JS 对象之扩展、密封及冻结三大特性](https://segmentfault.com/a/1190000003894119)
	[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)