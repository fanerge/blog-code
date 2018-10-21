---
title: ramda无参数风格和对象的相关操作
date: 2017-08-22 19:36:23
category: "函数式编程"
tags: ['js','函数式编程','ramda']
---
继续函数式编程的学习。

####	Pointfree 风格（无参数风格）
	```
	var forever21 = age => R.ifElse(R.gte(R.__, 21), R.always(21), R.inc)(age);
	// console.log(forever21(23)); // 21
	
	// pointfree风格的
	var forever21 = R.ifElse(R.gte(R.__, 21), R.always(21), R.inc);
	// console.log(forever21(21));
	// 我们刚刚让 age 消失了。这就是 Pointfree 风格。注意，这两个版本所做的事情完全一样。我们仍然返回一个接受年龄的函数，但并未显示的指定 age 参数。
	```
	
####	多元函数（多参数函数）
	```
	var titlesForYear = R.curry((year, books) => {
	  R.pipe(
		R.filter(publishedInYear(year)),
		R.map(book => book.title)    
	  )(books);
	});
	// console.log(titlesForYear(2012)(books));

	var titlesForYear = year => {
	  R.pipe(
		R.filter(publishedInYear(year)),
		R.map(book => book.title)
	  )
	};
	console.log(titlesForYear(2012));
	```

####	重构为 pointfree 风格的代码
	```
	// 改写18岁投票系统
	const wasBornInCountry = person => person.birthCountry === OUR_COUNTRY
	const wasNaturalized = person => Boolean(person.naturalizationDate)
	const isOver18 = person => person.age >= 18
	const isCitizen = person => wasBornInCountry(person) || wasNaturalized(person)
	const isEligibleToVote = person => isOver18(person) && isCitizen(person)
	
	const isEligibleToVote = person => both(isOver18, isCitizen)(person)
	const isCitizen = either(wasBornInCountry, wasNaturalized)
	const isEligibleToVote = both(isOver18, isCitizen)
	```

####	读取对象属性
	```
	const wasBornInCountry = person => person.birthCountry === OUR_COUNTRY
	const wasNaturalized = person => Boolean(person.naturalizationDate)
	const isOver18 = person => person.age >= 18
	const isCitizen = either(wasBornInCountry, wasNaturalized)
	const isEligibleToVote = both(isOver18, isCitizen)
	
	// 改写为
	const wasBornInCountry = person => equals(person.birthCountry, OUR_COUNTRY)
	const wasNaturalized = person => Boolean(person.naturalizationDate)
	const isOver18 = person => gte(person.age, 18)
	```

#####	prop
	定义：取出对象中指定属性的值。如果不存在，则返回 undefined。
	R.prop('x', {x: 100}); //=> 100
	```
	var wasBornInCountry = R.compose(R.equals('CHINA'), R.prop('birthCountry'))
	var wasNaturalized = R.compose(Boolean, R.prop('naturalizationDate'))
	var isOver18 = R.compose(R.gte(R.__, 18), R.prop('age'))
	console.log(R.both(wasBornInCountry, isOver18)(person1)); // true
	```

#####	pick
	定义：返回对象的部分拷贝，其中仅包含指定键对应的属性。如果某个键不存在，则忽略该属性。
	R.pick(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, d: 4}

#####	has/hasIn
	has定义：如果对象自身含有指定的属性，则返回 true；否则返回 false。
	R.has('name')({name: 'alice'});
	hasIn定义：如果对象自身或其原型链上含有指定的属性，则返回 true；否则返回 false。

#####	path
	定义：取出给定路径上的值。
	R.path(['a', 'b'], {a: {b: 2}}); //=> 2
	
#####	propOr / pathOr
	propOr定义：propOr 和 pathOr 像是 prop/path 与 defaultTo 的组合。如果在目标对象中找不到属性或路径的值，它们允许你提供默认值
	propOr('<Unnamed>', 'name', person);

#####	keys / values
	keys 返回一个包含对象中所有属性名称的数组，values 返回这些属性的值组成的数组。
	

#####	对属性增、删、改、查
	···	
	assoc/assocPath
	assoc：浅复制对象，然后设置或覆盖对象的指定属性。
	R.assoc('c', 3, {a: 1, b: 2}); //=> {a: 1, b: 2, c: 3}
	assocPath：浅复制对象，设置或覆盖即将创建的给定路径所需的节点，并将特定值放在该路径的末端。
	R.assocPath(['a', 'b', 'c'], 42, {a: {b: {c: 0}}}); //=> {a: {b: {c: 42}}}
	
	dissoc/dissocPath/omit
	dissoc：删除对象中指定 prop 属性。
	R.dissoc('b', {a: 1, b: 2, c: 3}); //=> {a: 1, c: 3}
	dissocPath：浅复制对象，删除返回对象中指定路径上的属性。
	R.dissocPath(['a', 'b', 'c'], {a: {b: {c: 42}}}); //=> {a: {b: {}}}
	omit：删除对象中给定的 keys 对应的属性。
	R.omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, c: 3}
	```
	
####	属性转换
	evolve：递归地对 object 的属性进行变换，变换方式由 transformation 函数定义。
	```
	var tomato  = {firstName: '  Tomato ', data: {elapsed: 100, remaining: 1400}, id:123};
	var transformations = {
	  firstName: R.trim,
	  lastName: R.trim, // Will not get invoked.
	  data: {elapsed: R.add(1), remaining: R.add(-1)}
	};
	R.evolve(transformations, tomato); //=> {firstName: 'Tomato', data: {elapsed: 101, remaining: 1399}, id:123}
	```

####	合并对象
	merge：合并两个对象的自身属性（不包括 prototype 属性）。如果某个 key 在两个对象中都存在，使用后一个对象对应的属性值。
	```
	R.merge({ 'name': 'fred', 'age': 10 }, { 'age': 40 });
	//=> { 'name': 'fred', 'age': 40 }
	// 定义一个反转合并的函数(用前面的同名属性覆盖后面的同名属性)
	 reverseMerge：const reverseMerge = R.flip(merge)
	```

####	总结
	本节学习了：pointfree(无参数风格编程) 
	对象属性：读取(prop、pick、has、path、propOr、pathOr、keys、values、)
	增删改查(assoc、assocPath、dissoc、dissocPath、omit)
	属性转换(evolve)
	合并对象(merge)
>	参考文献：
	[JavaScript函数编程-Ramdajs](http://www.cnblogs.com/whitewolf/p/javascript-functional-programming-Ramdajs.html)
	[Thinking in Ramda系列文章](https://zhuanlan.zhihu.com/p/27446536)