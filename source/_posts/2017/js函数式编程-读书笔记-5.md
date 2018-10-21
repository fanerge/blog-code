---
title: js函数式编程-递归
date: 2017-08-09 21:07:36
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	递归

####	自吸收函数（调用自己的函数）
	```
	function myLength (ary) {
		if (_.isEmpty(ary)) {
			return 0;
		} else {
			return 1 + myLength(_.rest(ary));
		}
	}
	let dd = myLength([1, 2, 3]);
	console.log(dd); // 3
	
	function cycle(times, ary) {
		if (times <= 0) {
			return [];
		} else {
			return cat(ary, cycle(times -1, ary));
		}
	}
	let dd = cycle(2, [1, 2, 3]);
	console.log(dd); // [1, 2, 3, 1, 2, 3]
	
	*_.zip*
	function cycle(times, ary) {
		if (times <= 0) {
			return [];
		} else {
			return cat(ary, cycle(times -1, ary));
		}
	}
	let dd = _.zip(['a', 'b', 'c'], [1, 2, 3]);
	console.dir(dd); // [['a', 1],['b', 2], ['c', 3]]
	```
	// 深度优先自递归搜索
	```
	*andify 组合函数*
	function andify () {
		let preds = _.toArray(arguments);
		return function () {
			let args = _.toArray(arguments);
			let everything = function (ps, truth) {
				if (_.isEmpty(ps)) {
					return truth;
				} else {
					return _.every(args, _.first(ps))
							&& everything(_.rest(ps), truth);
				}
			};
			return everything(preds, true);
		};
	}
	let evenNums = andify(_.isNumber, isEven);
	console.log(evenNums(4, 2)); // true
	
	*andify 组合函数*
	function orify () {
		let preds = _.toArray(arguments);
		return function () {
			let args = _.toArray(arguments);
			let something = function (ps, truth) {
				if (_.isEmpty(ps)) {
					return truth;
				} else {
					return _.some(args, _.first(ps))
							|| something(_.rest(ps), truth);
				}
			};
			return something(preds, true);
		};
	}
	let evenNums = orify(_.isNumber, isEven);
	console.log(evenNums(1, 2)); // true
	```

####	相互关联函数（函数调用其他会再调用回他的函数）
	```
	function evenSteven(n) {
		if (n === 0) {
			return true;
		} else {
			return oddJohn(Math.abs(n) - 1);
		}
	}

	function oddJohn(n) {
		if (n === 0) {
			return false;
		} else {
			return evenSteven(Math.abs(n) - 1);
		}
	}
	let dd = evenSteven(3);
	console.log(dd); // false

	*flat 展开嵌套数组*
	function flat(array) {
		if (_.isArray(array)) {
			return cat.apply(cat, _.map(array, flat));
		} else {
			return [array];
		}
	}
	let dd = flat([[1, 2], [3, 4]]);
	console.log(dd) // [1, 2, 3, 4]
	
	*clone 浅clone*
	let x = [{a: [1, 2, 3], b: 42}, {c: {d: []}}]
	let y = _.clone(x);
	console.log(y) // [{a: [1, 2, 3], b: 42}, {c: {d: []}}]	
	
	*deepClone 深clone*
	function deepClone(obj) {
		if (!existy(obj) || !_.isObject(obj)) {
			return obj;
		}
		let temp = new obj.constructor();
		for (let key in obj) {
			if (obj.hasOwnProperty(key)) {
				temp[key] = deepClone(obj[key]);
			}
		}
		return temp;
	}
	let x = [{a: [1, 2, 3], b: 42}, {c: {d: []}}]
	let y = deepClone(x);
	x[1]['c']['d'] = 100;
	console.log(_.isEqual(x, y)); // false
	
	// 遍历嵌套数组的数组
	function visit(mapFun, resultFun, array) {
		if (_.isArray(array)) {
			return resultFun(_.map(array, mapFun));
		} else {
			return resultFun(array);
		}
	}
	let dd = visit(_.identity, _.isNumber, 42);
	console.log(dd); // true
	```

####	太多递归了
####	递归是一个底层操作
>	源代码仓库
	[函数式编程-读书笔记](https://github.com/fanerge/Functional-reading-notes)	
























