---
title: Set和Map的用法
date: 2017-11-11 21:33:57
tags: 'js'
categories: 'js'
copyright: true
---
#	Set 详解
##	基础特性
Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。
ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
语法：new Set([iterable]);
参数：如果传递一个可迭代对象，它的所有元素将被添加到新的 Set中。如果不指定此参数或其值为null，则新的 Set为空。
返回值：一个新的Set对象。

##	来个简单实例
	```
	let set = new Set([1, 2, 3, 3]);
	console.log(set) // Set(3) {1, 2, 3}
	```
	
##	Set 实例的属性和方法
###	属性
1.	Set.prototype
	表示Set构造器的原型，允许向所有Set对象添加新的属性。
2.	Set.prototype.constructor
	返回实例的构造函数。默认情况下是Set。
3.	Set.prototype.size
	返回Set对象的值的个数。

###	方法
1.	Set.prototype.add(value)
	在Set对象尾部添加一个元素。返回该Set对象。
2.	Set.prototype.clear()
	移除Set对象内的所有元素。
3.	Set.prototype.delete(value)
	移除Set的中与这个值相等的元素，返回Set.prototype.has(value)在这个操作前会返回的值（即如果该元素存在，返回true，否则返回false）。Set.prototype.has(value)在此后会返回false。
4.	Set.prototype.has(value)
	返回一个布尔值，表示该值在Set中存在与否。
5.	Set.prototype.keys()
	与values()方法相同，返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值。
6.	Set.prototype.values()
	返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值。
7.	Set.prototype.entries()
	返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值的[value, value]数组。为了使这个方法和Map对象保持相似， 每个值的键和值相等。
8.	Set.prototype.forEach(callbackFn[, thisArg])
	按照插入顺序，为Set对象中的每一个值调用一次callBackFn。如果提供了thisArg参数，回调中的this会是这个参数。
9.	Set.prototype[@@iterator]()
	返回一个新的迭代器对象，该对象包含Set对象中的按插入顺序排列的所有元素的值
	
##	WeakSet
###	WeakSet 基础
WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。
1.	WeakSet 的成员只能是对象，而不能是其他类型的值。
2.	WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
###	WeakSet 具有的方法
	WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员。
	WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员。
	WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在 
	```
	const ws = new WeakSet();
	const obj = {};
	
	ws.add(window);
	ws.add(obj);
	
	ws.has(window); // true
	
	ws.delete(window);
	ws.has(window);    // false
	```

#	Map
##	Map 出现的背景
JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

##	基础特性
语法：new Map([iterable])
参数：iterable
	Iterable 可以是一个数组或者其他 iterable 对象，其元素或为键值对，或为两个元素的数组。 每个键值对都会添加到新的 Map。null 会被当做 undefined。

##	简单的Map 实例
	```
	let map = new Map();

	map.set(-0, 123);
	map.get(+0) // 123

	map.set(true, 1);
	map.set('true', 2);
	map.get(true) // 1

	map.set(undefined, 3);
	map.set(null, 4);
	map.get(undefined) // 3

	map.set(NaN, 123);
	map.get(NaN) // 123
	```

##	实例的属性和操作方法
###	属性
1.	Map.prototype
	表示 Map 构造器的原型。 允许添加属性从而应用于所有的 Map 对象。
2.	Map.prototype.constructor
	返回一个函数，它创建了实例的原型。默认是Map函数。
3.	Map.prototype.size
	返回Map对象的键/值对的数量。

###	方法
1.	Map.prototype.clear()
	移除Map对象的所有键/值对 。
2.	Map.prototype.delete(key)
	移除任何与键相关联的值，并且返回该值，该值在之前会被Map.prototype.has(key)返回为true。之后再调用Map.prototype.has(key)会返回false。
3.	Map.prototype.has(key)
	返回一个布尔值，表示Map实例是否包含键对应的值。
4.	Map.prototype.get(key)
	返回键对应的值，如果不存在，则返回undefined。
5.	Map.prototype.set(key, value)
	设置Map对象中键的值。返回该Map对象。
6.	Map.prototype.keys()
	返回一个新的 Iterator对象， 它按插入顺序包含了Map对象中每个元素的键 。
7.	Map.prototype.values()
	返回一个新的Iterator对象，它按插入顺序包含了Map对象中每个元素的值 。
8.	Map.prototype.entries()
	返回一个新的 Iterator 对象，它按插入顺序包含了Map对象中每个元素的 [key, value] 数组。
9.	Map.prototype.forEach(callbackFn[, thisArg])
	按插入顺序，为 Map对象里的每一键值对调用一次callbackFn函数。如果为forEach提供了thisArg，它将在每次回调中作为this值。
10.	Map.prototype[@@iterator]()
	返回一个新的Iterator对象，它按插入顺序包含了Map对象中每个元素的 [key, value] 数组。

##	WeakMap
###	WeakMap 的基础
	WeakMap结构与Map结构类似，也是用于生成键值对的集合。
	WeakMap与Map的区别有两点：
1.	WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。
2.	WeakMap的键名所指向的对象，不计入垃圾回收机制。

###	WeakMap 的方法
1.	get()
2.	set()
3.	has()
4.	delete()

###	WeakMap 的用途
WeakMap 应用的典型场合就是 DOM 节点作为键名。
	```
	let myElement = document.getElementById('logo');
	let myWeakmap = new WeakMap();

	myWeakmap.set(myElement, {timesClicked: 0});

	myElement.addEventListener('click', function() {
	  let logoData = myWeakmap.get(myElement);
	  logoData.timesClicked++;
	}, false);
	```
PS：上面代码中，myElement是一个 DOM 节点，每当发生click事件，就更新一下状态。我们将这个状态作为键值放在 WeakMap 里，对应的键名就是myElement。一旦这个 DOM 节点删除，该状态就会自动消失，不存在内存泄漏风险。

>	参考文档：
	[MDN-Set](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set)
	[MDN-Map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)
	[阮一峰-Set和Map](http://es6.ruanyifeng.com/#docs/set-map)











