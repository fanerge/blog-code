---
title: ramda无参数风格编程 (Pointfree Style)和声明式编程
date: 2017-08-21 20:37:43
category: "函数式编程"
tags: ['js','函数式编程','ramda']
---
继续函数式编程的学习。

####	部分应用（Partial Application）
	在上篇文章中，简单的函数链式调用（"pipeline"）时，其中的被调用函数都是一元的（除了首个函数）。但如果要使用多元函数呢？
	例如，假设有一个书籍对象的集合，我们想要找到特定年份出版的所有图书的标题。
	```
	var books = [
	  {year: 1991, title: '11111'},
	  {year: 1991, title: '22222'},
	  {year: 1992, title: '33333'}
	];
	const publishedInYear = (book, year) => book.year === year;
	const titlesForYear = (books, year) => {
	  const selected = R.filter(book => publishedInYear(book, year), books);
	  return R.map(book => book.title, selected);
	};
	var book1 = titlesForYear(books, 1991); // ['11111', '22222'] 
	```
	
#####	高阶函数
	获取或返回其他函数的函数称为 "高阶函数"。
	```
	// 高阶函数
	var publishedInYear = function (year) {
	  return function (book) {
		return book.year === year;
	  };
	};
	// 箭头函数
	// var publishedInYear = year => book => book.year === year;
	var titlesForYear = (books, year) => {
	  const selected = R.filter(publishedInYear(year), books);
	  return R.map(book => book.title, selected);
	}; 
	console.log(titlesForYear(books, 1992)); // '33333'
	```

#####	部分应用函数
	partial/partialRight（部分应用）
	定义：这两个函数可以让我们不必一次传递所有需要的参数，也可以调用函数。它们都返回一个接受剩余参数的新函数，当所有参数都传入后，才会真正调用被包裹的原函数。
	```
	// 例子，只是想检查一本书是否是在指定年份出版的。
	var publishedInYear = (book, year) => book.year === year;
	var titleForYear = (books, year) => {
	  const selected = R.filter(R.partialRight(publishedInYear, [year]), books);
	  return R.map(book => book.title, selected);
	};
	// console.log(titleForYear(books, 1992)); // ['33333']
	
	```
	
#####	柯里化(Curry)	
	定义：一个柯里化了的函数是一系列高阶一元函数，将多参数函数转化为单参数函数。
	一般来说，我只有需要在多个地方对同一个函数使用 partial 的时候，才会对函数进行柯里化。
	```
	var publishedInYear = R.curry((year, book) => book.year === year);
	var titlesForYear = (books, year) => {
	  const selected = R.filter(publishedInYear(year), books);
	  return R.map(book => book.title, selected);
	};
	console.log(titlesForYear(books, 1992)); // ['33333']
	var book2 = publishedInYear(1992, {year: 1992, title: '33333'}); // true
	```

#####	顺序错误的参数
	filp：交换函数前两个参数的位置。	
	```
	var mergeThree = (a, b, c) => [].concat(a, b, c);
	console.log(R.flip(mergeThree)(1, 2, 3)); // [2, 1, 3]
	```
	__或placeholder (占位符)
	定义：柯里化函数的参数占位符。允许部分应用于任何位置的参数。
	更通用的选择是使用 "placeholder" 参数（__）
	假设有一个三元柯里化的函数，并且我们想传入第一个和最后一个参数，中间参数后续再传，应该怎么办呢？我们可以使用 "占位符" 作为中间参数：
	```
	const threeArgs = curry((a, b, c) => { /* ... */ });
	const middleArgumentLater = threeArgs('value for a', R.__, 'value for c');
	
	var publishedInYear = R.curry((year, book) => book.year === year)
	var titlesForYear = R.curry((year, books) =>
	  R.pipe(
		R.filter(publishedInYear(year)),
		R.map(book => book.title)
	  )(books)
	)
	// console.log(titlesForYear(1991, books)); // ["11111", "22222"]
	```
	
####	声明式编程

#####	命令式 vs 声明式
	命令式编程中，程序员需要告诉计算机怎么做来完成任务。
	声明式编程，程序员只需告诉计算机我想要什么，然后计算机自己理清如何产生结果。
	```
	var square = x => R.multiply(x, x);
	var operate = R.pipe(
	  R.multiply,
	  R.add(1),
	  square
	  );
	var operate3 = operate(3, 4); // 169
	// add(1) 与增量运算符（++）非常相似，但 ++ 修改了被操作的值，因此它是 "mutation" 的。
	// 所以使用R.add(1),R.subtract(1)代替++ 和 --
	// Ramda提供了R.inc和R.dec
	R.dec(42); //=> 41
	R.negate(42); //=> -42 取反
	```
	
#####	比较(Comparison)	
	使用R.gt()、R.gte()、R.lt()、R.lte()、R.equals()代替>、>=、<、<=、===
	identical：如果两个参数是完全相同，则返回 true，否则返回 false。如果它们引用相同的内存，也认为是完全相同的。
	isEmpty：检测给定值是否为其所属类型的空值，若是则返回 true ；否则返回 false 。
	isNil：检测输入值是否为 null 或 undefined 。
	```
	// 改写投票的例子
	var wasBornInCountry = person => R.equals(person.birthCountry, 'CHINA');
	var wasNaturalized = person => Boolean(person.naturalizationDate);
	var isOver18 = person => R.gte(person.age, 18);
	var isCitizen = R.either(wasBornInCountry, wasBornInCountry);
	var isEligibleToVote = R.both(isOver18, isCitizen);
	console.log(isEligibleToVote({
	  birthCountry: 'CHINA', 
	  naturalizationDate: false,
	  age: 22
	})); // true
	```
	
#####	逻辑(Logic)	
	both 和 either 来代替 && 和 || 运算符。使用 complement 代替 !。
	我以下列方式进行分类：and、or 和 not 用于处理数值；both、either 和 complement 用于处理函数。
	```
	var setting = {};
	var lineWith = setting.lineWith || 80;
	var lineWith = R.defaultTo(80, setting.lineWith);
	console.log(lineWith); //使用后者代替前者
	```
	
#####	条件(Conditionals)	
	R.ifElse 代替 if-else
	```
	var forever21 = age => age >= 21 ? 21 : age + 1
	// console.log(forever21(19)); // 20
	var forever21 = age => R.ifElse(R.gte(R.__, 21), () => 21, R.inc)(age)
	// console.log(forever21(19)); // 20
	```
	
#####	constants (常量)
	```
	R.always(42) 代替 42
	R.T() === true 忽略所有参数
	console.log(R.T(false)); // true
	R.F() === false 忽略所有参数
	```
	
##### identity (恒等)	
	输出恒等于输入。如，a => a
	```
	identity：将输入值原样返回。适合用作默认或占位函数。
	nthArg；返回一个函数，该函数返回它的第 n 个参数。
	const alwaysDrivingAge = age => ifElse(lt(__, 16), always(16), a => a)(age)
	```
	
#####	when 和 unless	
	```
	var alwaysDrivingAge = age => R.unless(R.gte(R.__, 16), R.always(16))(age)
	console.log(alwaysDrivingAge(15)); // 16
	```
	
#####	cond	
	作用：来代替 switch 语句或链式的 if...then...else 语句
	```
	const water = temperature => R.cond([
	  [R.equals(0),   R.always('water freezes at 0°C')],
	  [R.equals(100), R.always('water boils at 100°C')],
	  [R.T,           temp => `nothing special happens at ${temp}°C`]
	])(temperature)
	console.log(water(23)); // nothing special happens at 23°C
	```
####	总结
	本节学习了：高阶函数、部分应用函数、柯里化（Curry）、改变参数顺序的方法（flip、plachhokder__、pipeline）
	常用的声明式编程替换命令式编程：add、subtract、multiply、divide等等
	
>	参考文献：
	[JavaScript函数编程-Ramdajs](http://www.cnblogs.com/whitewolf/p/javascript-functional-programming-Ramdajs.html)
	[Thinking in Ramda系列文章](https://zhuanlan.zhihu.com/p/27446536)


	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	





>	参考文献：
	[JavaScript函数编程-Ramdajs](http://www.cnblogs.com/whitewolf/p/javascript-functional-programming-Ramdajs.html)
	[Thinking in Ramda系列文章](https://zhuanlan.zhihu.com/p/27446536)