---
title: Proxy和Reflect的用法
date: 2017-11-11 18:51:05
tags: 'js'
categories: 'js'
copyright: true
---
#	Proxy 详解
##	Proxy 定义
Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
##	Proxy 语法
	```
	// 目标对象，需要被拦截 或 处理的对象，数组，函数甚至是其他的代理器
	let target = {};
	// 拦截器对象
	let handler = {
		get(target, name){
			// 这里如果 target 没有name属性的话，就设定一个默认值
			return name in target ? target[name] : 27;
		}
	};
	var proxy = new Proxy(target, handler);

	console.log(target.name); // undefined
	console.log(proxy.name); // 27
	```
PS：Proxy 对象的所有用法，都是上面这种形式，不同的只是handler参数的写法。其中，new Proxy()表示生成一个Proxy实例，target参数表示所要拦截的目标对象，handler参数也是一个对象，用来定制拦截行为。

##	Proxy 支持的拦截操作一览，一共13种	
1.	get(target, propKey, receiver)：拦截对象属性的读取，比如proxy.foo和proxy['foo']。
2.	set(target, propKey, value, receiver)：拦截对象属性的设置，比如proxy.foo = v或proxy['foo'] = v，返回一个布尔值。
3.	has(target, propKey)：拦截propKey in proxy的操作，返回一个布尔值。
4.	deleteProperty(target, propKey)：拦截delete proxy[propKey]的操作，返回一个布尔值。
5.	ownKeys(target)：拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。
6.	getOwnPropertyDescriptor(target, propKey)：拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。
7.	defineProperty(target, propKey, propDesc)：拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。
8.	preventExtensions(target)：拦截Object.preventExtensions(proxy)，返回一个布尔值。
9.	getPrototypeOf(target)：拦截Object.getPrototypeOf(proxy)，返回一个对象。
10.	isExtensible(target)：拦截Object.isExtensible(proxy)，返回一个布尔值。
11.	setPrototypeOf(target, proto)：拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
12.	apply(target, object, args)：拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
13.	construct(target, args)：拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。

##	Proxy.revocable() 可撤销的代理
Proxy.revocable方法返回一个可取消的 Proxy 实例。
	```
	let target = {};
	let handler = {};

	let {proxy, revoke} = Proxy.revocable(target, handler);

	proxy.foo = 123;
	console.log(proxy.foo);

	revoke(); // 取消Proxy实例
	console.log(proxy.foo); // TypeError: Revoked
	```
PS：Proxy.revocable方法返回一个对象，该对象的proxy属性是Proxy实例，revoke属性是一个函数，可以取消Proxy实例。上面代码中，当执行revoke函数之后，再访问Proxy实例，就会抛出一个错误。
Proxy.revocable的一个使用场景是，目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问。

##	this 的问题
虽然 Proxy 可以代理针对目标对象的访问，但它不是目标对象的透明代理，即不做任何拦截的情况下，也无法保证与目标对象的行为一致。主要原因就是在 Proxy 代理的情况下，目标对象内部的this关键字会指向 Proxy 代理。
	```
	const target = {
	  m: function () {
		console.log(this === proxy);
	  }
	};
	const handler = {};

	const proxy = new Proxy(target, handler);

	target.m() // false
	proxy.m()  // true
	```

##	Proxy 的应用
###	扩展数组的属性和方法
需求：对于数组对象，有时候我们只想要得到数组中对象的某个键值内容。
	```
	var AddSomeFunctionHandler={
		get:function(obj,prop){

			if(prop in obj){
				return obj[prop]   // 按数组默认方式访问元素
			}

			if(prop === 'name'){
				return obj.map(o=>o.name)
			}

		}
	}

	var PersonArray=[{
		name:"Alice",age:23
	},{
		name:"Bob",age:45
	},{
		name:"Mike",age:27
	}]

	var p= new Proxy(PersonArray,AddSomeFunctionHandler)
	console.log(p.name) // ["Alice", "Bob", "Mike"]
	```

#	Reflect 详解
##	Reflect 定义
Reflect对象与Proxy对象一样，也是 ES6 为了操作对象而提供的新 API。
Reflect对象的设计目的有这样几个：
1.	将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，从Reflect对象上可以拿到语言内部的方法。
2.	修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。
3.	让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
4.	Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。

##	Reflect 静态方法
Reflect对象一共有13个静态方法。
1.	Reflect.apply(target, thisArg, args)
2.	Reflect.construct(target, args)
3.	Reflect.get(target, name, receiver)
4.	Reflect.set(target, name, value, receiver)
5.	Reflect.defineProperty(target, name, desc)
6.	Reflect.deleteProperty(target, name)
7.	Reflect.has(target, name)
8.	Reflect.ownKeys(target)
9.	Reflect.isExtensible(target)
10.	Reflect.preventExtensions(target)
11.	Reflect.getOwnPropertyDescriptor(target, name)
12.	Reflect.getPrototypeOf(target)
13.	Reflect.setPrototypeOf(target, prototype)
PS：上面这些方法的作用，大部分与Object对象的同名方法的作用都是相同的，而且它与Proxy对象的方法是一一对应的。

##	Reflect.get(target, name, receiver)
Reflect.get方法查找并返回target对象的name属性，如果没有该属性，则返回undefined。
	```
	var myObject = {
	  foo: 1,
	  bar: 2,
	  get baz() {
		return this.foo + this.bar;
	  },
	}

	Reflect.get(myObject, 'foo') // 1
	Reflect.get(myObject, 'bar') // 2
	Reflect.get(myObject, 'baz') // 3
	```
	
##	实例--使用 Proxy 实现观察者模式
观察者模式（Observer mode）指的是函数自动观察数据对象，一旦对象有变化，函数就会自动执行。
如果你还不懂[观察者模式]()
	```
	//添加观察者
	const queuedObservers = new Set();
	const observe = fn => queuedObservers.add(fn);

	//proxy 的set 方法
	function set(target, key, value, receiver) {
		const result = Reflect.set(target, key, value, receiver);
		queuedObservers.forEach(observer => observer());
		return result;
	}
	//创建proxy代理
	const observable = obj => new Proxy(obj, {set});
	//被观察的 对象
	const person = observable({
		name: '张三',
		age: 20
	});

	observe(print);
	console.log(person.name); // 张三
	person.name = '李四';
	console.log(person.name); // 李四
	```
	
>	参考文档：
	[阮一峰-Reflect](http://es6.ruanyifeng.com/#docs/reflect)
	[用es6 （proxy 和 reflect）轻松实现 观察者模式](https://www.cnblogs.com/WhiteHorseIsNotHorse/p/7016010.html)
	




