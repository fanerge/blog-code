---
title: ramda入门和函数组合
date: 2017-08-20 21:37:12
category: "函数式编程"
tags: ['js','函数式编程','ramda']
---
继续函数式编程的学习。

####	总结Ramda与Lodash和Underscore的优势
#####	自动柯里化
	```
	// 一map函数为例，解释Ramda的自动柯里化 
	// map函数解释：param1为对集合每一项进行处理并返回同类型的数据，param2需要处理的集合（Array或Object）
	// 第一种，为使用柯里化
	var map3 = R.map(function (item) {
	  return item * item;
	}, [1, 2, 3, 4]); 
	// console.log(map3); // [1, 4, 9, 16]
	
	// 第一种curry化（转化成单参数函数）
	var map1 = R.map(function (item) {
	  return item * item;
	});
	var map2 = map1([1, 2, 3, 4]);
	// console.log(map2); // [1, 4, 9, 16]
	```
	
#####	函数参数优先于数据
	```
	// 函数优先于数据
	var map4 = R.map(R.multiply(2), [1, 2, 3, 4]);
	// console.log(map4); // [2, 4, 6, 8]
	var map5 = R.map(R.multiply(2))([1, 2, 3, 4]);
	// console.log(map5); // [2, 4, 6, 8]
	```

####	Ramdajs的组合
	```
	// ramdajs的compose组合
	var users = [
	  { 'user': 'user1',  'age': 36 },
	  { 'user': 'user2',    'age': 40 },
	  { 'user': 'user3', 'age': 18 }
	];
	// R.pluck(k)[array] === R.map(R.prop(k), array)
	var pluck1 = R.pluck('user')(users);
	var pluck2 = R.map(R.prop('user'))(users);

	// compose为函数组合（从右到左）
	var pluck3 = R.compose(R.join(','), R.pluck('user'))(users);
	// console.log(pluck3); // user1,user2,user3
	// pipe为函数组合（从左到右）
	var pipe1 = R.pipe(R.pluck('user'), R.join('*'))(users);
	// console.log(pipe1); // user1*user2*user3
	
	// 依次获得用户的出生年
	var compose1 = R.compose(R.join(','), R.map(R.subtract(new Date().getFullYear())), R.pluck('age'))(users);
	console.log(compose1); // 1981,1977,1999
	
	// 获得最年轻的用户信息
	var userMin = R.compose(R.head, R.sortBy(R.prop('age')))(users);
	var userMax = R.compose(R.last, R.sortBy(R.prop('age')))(users);
	var userMax1 = R.compose(R.head, R.reverse, R.sortBy(R.prop('age')))(users);
	console.log(userMin, userMax); // {user: "user3", age: 18} {user: "user2", age: 40}
	console.log(userMax1); // {user: "user2", age: 40}
	```
	**纯函数**纯函数是没有副作用的函数。它不会给任何外部变量赋值，不会获取输入，不会产生 "输出"，不会对数据库进行读写，不会修改输入参数等。
	纯函数的基本思想是：相同的输入，永远会得到相同的输出。
	**数据不变性**函数式编程的另一个重要概念是 "Immutability"，"Immutability" 是指 "数据不变性"。
	当以 immutable 方式工作时，一旦定义了某个值或对象，以后就再也不会改变它了。这意味着不能更改已有数组中的元素或对象中的属性。
	开始以函数式思维思考最简单的方式是，使用集合迭代函数代替循环。
	```
	foreach
	for (const value of Array) {
		console.log(value);
	}
	forEach(value => console.log(value), Array);
	
	map
	map(x => x*2, [1, 2, 3]); // [2, 4, 6]
	
	filter/reject(互补)
	const isEven = x => x % 2 === 0;
	filter(isEven, [1, 2, 3]); // [2]
	reject(isEven, [1, 2, 3]); // [1, 3]
	
	find
	find(isEven, [1, 2, 3, 4]); // 2
	
	reduce
	const add = (accum, value) => accum + value;	
	reduce(add, 5, [1, 2, 3, 4]); // 15
	```
	
####	函数组合
#####	Complement
	Complement：对函数的返回值取反。接受一个函数 f，返回一个新函数 g。
	```
	const isEven = x => x % 2 === 0;
	var complement1 = R.find(isEven, [1, 1, 2, 4]); // 2
	var complement2 = R.find(R.complement(isEven), [1, 2, 2, 4]); // 1
	```
#####	Both/Either
	both：该函数调用两个函数，并对两函数返回值进行与（&&）操作。
	either：该函数调用两个函数，并对两函数返回值进行或（||）操作。	
	投票系统（投票资格的条件：在本国出生，或者后来加入该国国籍，且年满18岁。）
	```
	// 判断是否有投票的权利
	let person1 = {
	  birthCountry: 'CHINA',
	  naturalizationDate: false,
	  age: 20
	};
	const wasBornInCountry = person => person.birthCountry === 'CHINA';
	const wasNaturalized = person => Boolean(person.naturalizationDate);
	const isOver18 = person => person.age >= 18;
	var isCitizen = R.either(wasBornInCountry, wasNaturalized); // 在本国出生，或者后来加入该国国籍。
	var isEligibleToVote = R.both(isCitizen, isOver18);
	var vote1 = isEligibleToVote(person1); // true
	```

#####	Pipelines(管道)
	pipe：从左往右执行函数组合。最左边的函数可以是任意元函数（参数个数不限），其余函数必须是一元函数。
	```
	var multiply = (num1, num2) => num1 * num2;
	var addOne = num => num + 1;
	var square = num => num * num;
	var operate1 = R.pipe(multiply, addOne, square)(1, 2); // ((1*2)+1)^2 = 9
	```
	compose：从右往左执行函数组合（右侧函数的输出作为左侧函数的输入）。最右侧函数可以是任意元函数（参数个数不限），其余函数必须是一元函数。
	```
	var operate2 =R.compose(square, addOne, multiply)(1, 2); // 9
	```
	
####	总结
	本节学习了：函数、纯函数、IMMUTABILITY、foreach（递归替代循环）、map、filter、reject、find、reduce、
	函数组合的方法：complement、either、both、pipe、compose、
	
>	参考文献：
	[JavaScript函数编程-Ramdajs](http://www.cnblogs.com/whitewolf/p/javascript-functional-programming-Ramdajs.html)
	[Thinking in Ramda系列文章](https://zhuanlan.zhihu.com/p/27446536)


























































