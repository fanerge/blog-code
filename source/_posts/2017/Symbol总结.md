---
title: Symbol总结
date: 2017-11-12 11:31:07
tags: 'js'
categories: 'js'
copyright: true
---
#	Symbol 基础
##	Symbol 引入的原因
ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。
ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

##	特性
语法：Symbol([description])
参数：description -- 可选的，字符串。symbol的description可以用于调试，但无法访问到symbol本身。

##	示例
	```
	let s1 = Symbol();
	let s2 = Symbol();

	s1 === s2 // false
	```

#	作为属性名的 Symbol
	```
	// 这里作为对象的属性使用，独一无二
	let s1 = Symbol();
	let a = {
		[s1]: '我是Symbol类型的'
	};

	console.log(a[s1]);
	```

#	属性名的遍历
Symbol 作为属性名，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。
<span style="color: red;">它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol 属性名。</span>
另一个新的API，Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

#	Symbol.for(key)，Symbol.keyFor(key)
##	Symbol.for(key)
我们希望重新使用同一个 Symbol 值，Symbol.for方法可以做到这一点。它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的Symbol值。如果有，就返回这个 Symbol 值，否则就新建并返回一个以该字符串为名称的 Symbol 值。
	```
	let s1 = Symbol.for('foo');
	let s2 = Symbol.for('foo');

	s1 === s2 // true
	```
	PS：上面代码中，s1和s2都是 Symbol 值，但是它们都是同样参数的Symbol.for方法生成的，所以实际上是同一个值。
		Symbol.for(key)与Symbol(desc)这两种写法，都会生成新的 Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。

##	Symbol.keyFor(key)
	Symbol.keyFor方法返回一个（全局）已登记的 Symbol 类型值的key。
	```
	let s1 = Symbol.for("foo");
	Symbol.keyFor(s1) // "foo"

	let s2 = Symbol("foo");
	Symbol.keyFor(s2) // undefined
	```
	PS：上面代码中，变量s2属于未登记的 Symbol 值，所以返回undefined。

#	内置的Symbol值
除了定义自己使用的Symbol值以外，ES6还提供了11个内置的Symbol值，指向语言内部使用的方法。

##	迭代 symbols

1.	Symbol.iterator
		一个返回一个对象默认迭代器的方法。使用 for...of。
2.	Symbol.asyncIterator（实验性API）
		一个返回对象默认的异步迭代器的方法。使用 for await of。
		
##	正则表达式 symbols
1.	Symbol.match
		一个用于对字符串进行匹配的方法，也用于确定一个对象是否可以作为正则表达式使用。使用 String.prototype.match().
2.	Symbol.replace
		一个替换匹配字符串的子串的方法. 使用 String.prototype.replace().
3.	Symbol.search
		一个返回一个字符串中与正则表达式相匹配的索引的方法。使用String.prototype.search().
4.	Symbol.split
		一个在匹配正则表达式的索引处拆分一个字符串的方法.。使用 String.prototype.split().
		
##	其他 symbols
1.	Symbol.hasInstance
		一个确定一个构造器对象识别的对象是否为它的实例的方法。使用 instanceof.
2.	Symbol.isConcatSpreadable
		一个布尔值，表明一个对象是否应该flattened为它的数组元素。使用Array.prototype.concat().
3.	Symbol.unscopables
		拥有和继承属性名的一个对象的值被排除在与环境绑定的相关对象外。
4.	Symbol.species
		一个用于创建派生对象的构造器函数。
5.	Symbol.toPrimitive
		一个将对象转化为基本数据类型的方法。
6.	Symbol.toStringTag
		用于对象的默认描述的字符串值。使用Object.prototype.toString().

>	参考文档：
	[阮一峰-Symbol](http://es6.ruanyifeng.com/#docs/symbol)
	[MDN-Symbol](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)	
	


