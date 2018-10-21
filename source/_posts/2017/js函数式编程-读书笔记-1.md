---
title: js函数式编程-一等函数与Applicative编程
date: 2017-07-27 21:02:47
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	一等函数与Applicative编程

####	函数式一等公民
	函数式编程语言应该是促进创造和使用函数的。
	函数式一等公民：函数可以作为数组的成员、对象的方法、存储为变量等等。
	```
	// 例如存储为数组的成员
	var fortytwos = [42, function () { return 42 }]
	```
	高阶函数的定义：以函数作为参数或者返回函数。
	多种javascript编程方式
		命令式编程 -- 通过详细描述行为的编程方式。
		基于原型的面向对象编程 -- 基于原型对象及其实例的编程方式。
		元编程 -- 对JavaScript执行模型数据进行编写和操作的编程方式。	

###	Applicative编程
	定义：函数A作为参数提供给函数B。	
	```
	var nums = [1, 2, 3, 4, 5];
	
	*map* 遍历集合并对其每一个值调用一个函数，返回结果的集合。
	function doubleAll(array) {
		return _.map(array, function (n) {
			return n*2;	
		});
	}
	console.log(doubleAll(nums)) // [2, 4, 6, 8, 10]
	
	*reduce* 利用函数将值的集合合并成一个值，该函数接收一个累积值和本次处理的值
	function average (array) {
		let sum = _.reduce(array, function (a, b){
			return a + b;
		})
		return sum / array.length;
	}
	console.log(average(nums)) // 3
	
	*filter* 对集合每一个值调用函数，若返回为true并添加到新集合中去，并返回新集合
	function onlyEven(array) {
		return _.filter(array, function (n){
			return n%2 === 0	
		})
	}
	console.log(onlyEven(nums))
	
	集合中心编程
	var test = _.map({a: 1, b: 2}, function (v, k) {
		return [k, v];
	})
	console.dir(test) // [['a', 1], ['b', 2]]
	
	Applicative编程的其他实例
	
	*reduceRight*（从右到左）
	var nums = [100, 2, 25];
	function div(x, y) {
		return x/y;
	}
	var test = _.reduceRight(nums, div);
	console.log(test) // 0.125
	
	*find* 它接收一个集合和一个谓词函数，并返回该谓词为true时的第一个元素。
	// 其中谓词函数为：_.isEqual、_.isEmpty、_.isElement...
	var dd = _.find(['a', 'b', '3', 4], _.isNumber);
	console.log(dd) // 4
	
	*reject* 本质为_.filter的逆命题
	var dd = _.reject(['a', 'b', '3', 4], _.isNumber);
	console.log(dd) // ['a', 'b', '3']
	// 另一种实现
	function complement(pred) {
		return function () {
			return !pred.apply(null, _.toArray(arguments));
		};
	}
	var d1 = _.filter(['a', 'b', '3', 4], complement(_.isNumber))
	console.log(d1) // ['a', 'b', '3']
	
	*all* 当对于集合中所有元素都满足谓词时返回true，否则返回false
	var dd = _.all([1, 2, 3, '4'], _.isNumber)
	console.log(dd) // false
	
	*any* 当对于集合中有满足谓词时返回true，否则返回false
	var dd = _.any([1, 2, 3, '4'], _.isNumber)
	console.log(dd) // true
	
	*sortBy* 接收一个集合和函数来对该集合排序
	var people = [{name: 'Rick', age: 30},{name: 'Jaka', age: 24}];
	var list = _.sortBy(people, function (p) {
		return p.name
	})
	console.log(list) // [{name: 'Jaka', age: 24}, {name: 'Rick', age: 30}]
	
	*groupBy* 接收一个集合和一个条件函数，并返回一个对象，k为传入的条件，v为对应的元素
	var albums = [
		{title: 'sabba', genre: 'metal'},
		{title: 'scientist', genre: 'dub'},
		{title: 'undertow', genre: 'metal'},
	]
	var list = _.groupBy(albums, function (a) {
		return a.genre;
	})
	console.dir(list)
	
	*countBy* 接收一个集合和一个条件函数，并返回一个对象，k为传入的条件，v为对应的元素的个数
	var albums = [
		{title: 'sabba', genre: 'metal'},
		{title: 'scientist', genre: 'dub'},
		{title: 'undertow', genre: 'metal'},
	]
	var list = _.countBy(albums, function (a) {
		return a.genre;
	})
	console.dir(list) // {metal: 2, dub: 1}
	```
	
	定义几个Applicative函数
	```
	*cat* 合并数组
	function cat() {
		let head = _.first(arguments);
		if (existy(head)) {
			return head.concat.apply(head, _.rest(arguments));
		} else {
			return [];
		}
	}
	cat([1, 2, 3], [4, 5]) // [1, 2, 3, 4, 5]
	
	*construct* 将元素放置在数组前方
	function construct(head, tail) {
		return cat([head], _.toArray(tail));
	}
	construct(42, [1, 2, 3]) // [42, 1, 2, 3]
	```

###	数据思考
	```
	*keys* 以数组的形式返回对象的key
	var zombie = {name: 'bub', film: 'day'}
	console.log(_.keys(zombie)) // ['name', 'film']
	*values* 以数组的形式返回对象的value
	console.log(_.keys(zombie)) // ['bub', 'day']
	
	*pluck* 接收一个对象数组和一个字符串，并返回在给定的键数组中对应对象的值
	var authorList = _.pluck([
		{title: 'chthon', author: 'anthony'},
		{title: 'grendel', author: 'grendel'},
		{title: 'after'}
	], 'author');
	
	*pairs* 接收一个对象，并把它变成这个嵌套数组
	var demo = _.pairs({title: 'chthon', author: 'anthony'})
	console.log(demo) // [['title', 'chthon'],['author', 'anthony']]
	
	*object* 将其重新组合成一个新的对象
	var zombie = {name: 'bub', film: 'day'}
	var dd = _.object(_.map(_.pairs(zombie), function (pair) {
		return [pair[0].toUpperCase(), pair[1]]
	}))
	console.log(dd)	// {NAME: "bub", FILM: "day"}
	
	*invert* 翻转key和value
	var zombie = {name: 'bub', film: 'day'}
	var dd = _.invert(zombie);
	console.log(dd) // {bub: "name", day: "film"}
	
	*defaults* 用于扩充传入的对象，如为某个属性添加默认值
	var dd = _.pluck(_.map([
			{title: 'chthon', author: 'anthony'},
			{title: 'grendel', author: 'gardner'},
			{title: 'after'},
		], function (obj) {
			return _.defaults(obj, {author: 'Unknown'})
		}),
	'author');
	console.log(dd)	// ["anthony", "gardner", "Unknown"]
	
	*pick* 根据参数筛选对象
	*omit* 根据参数筛选对象
	var person = {
		name: 'romy',
		token: 'j3900',
		password: '3333'
	};
	var info = _.pick(person, 'token', 'password')
	var other = _.omit(person, 'token', 'password')
	console.log(info) // {token: "j3900", password: "3333"}
	console.log(other) // {name: 'romy'}
	
	*findWhere* 接收一个对象数组，返回第一个与参数给出的条件匹配的对象
	var library = [
		{title: 'sicp', isbn: '0234', ed: 1},
		{title: 'sicp', isbn: '0235', ed: 2},
		{title: 'joy', isbn: '0236', ed: 1},
	];
	var dd = _.findWhere(library, {title: 'sicp', ed: 2})
	console.log(dd) // {title: "sicp", isbn: "0235", ed: 2} 	
	
	*where* 接收一个对象数组，返回所有条件匹配的对象
	var library = [
		{title: 'sicp', isbn: '0234', ed: 1},
		{title: 'sicp', isbn: '0235', ed: 2},
		{title: 'joy', isbn: '0236', ed: 1},
	];
	var dd = _.where(library, {title: 'sicp'})
	console.log(dd) // {{title: 'sicp', isbn: '0234', ed: 1}, {title: 'sicp', isbn: '0235', ed: 2}}
	```
	
	表状（Tabel-Like）数据	
	```
	function project(table, keys){
		return _.map(table, function (obj){
			return _.pick.apply(null, construct(obj, keys));
		});
	}
	var editionResults = project(library, ['title', 'isbn'])
	// [
		{title: 'sicp', isbn: '0234'},
		{title: 'sicp', isbn: '0235'},
		{title: 'joy', isbn: '0236'},
	]
	
	*rename* 为对象属性设置别名
	function rename(obj, newNames){
		return _.reduce(newNames, function(o, nu, old){
			if (_.has(obj, okd)) {
				o[nu] = obj[old];
				return o;
			} else {
				return o;
			}
		},
		_.omit.apply(null, construct(obj, _.keys(newNames))));
	}
	var dd = {a: 1, b: 2}
	console.log(rename(dd, {'a': 'A'})) // {b: 2, A: 1}
	
	*as* 类似于SQL语句 SELECT ed as edition FROM library
	function as(table, newNames){
		return _.map(table, function(obj){
			return rename(obj, newNames);
		});
	}
	var library = [
			{title: 'sicp', isbn: '0234', ed: 1},
			{title: 'sicp', isbn: '0235', ed: 2},
			{title: 'joy', isbn: '0236', ed: 1},
		];
	var dd = as(library, {ed: 'edition'})
	console.log(dd)
	// [
			{title: 'sicp', isbn: '0234', edition: 1},
			{title: 'sicp', isbn: '0235', edition: 2},
			{title: 'joy', isbn: '0236', edition: 1},
		]
		
	*where* 类似于SQL的where语句
	//接收一个函数，作为对表中的每个对象的谓词。每当谓词返回false是，该对象不会出现在新表中
	function restrict(table, pred){
		return _.reduce(table, function(newTable, obj){
			if (truthy(pred(obj))) {
				return newTable;
			} else {
				return _.without(newTable, obj)
			}
		}, table)
	}

	var dd = restrict(library, function (book){
		return book.ed > 1;
	})
	console.log(dd) // [{title: 'sicp', isbn: '0235', ed: 2}]
	```	

###	总结
	1. 一等函数	
		可以存储在变量中
		可以存储在数组中的插槽中
		可以存储在对象的字段中
		...
	2. applicative编程
		_.map
		_.reduce
		_.filter
>	源代码仓库
	[函数式编程-读书笔记](https://github.com/fanerge/Functional-reading-notes)
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		























