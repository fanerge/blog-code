---
title: js函数式编程-高阶函数
date: 2017-08-03 20:01:15
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	高阶函数

####	以其他函数为参数的函数
	如：_.map、_.reduce、_.filter等函数
	关于传递函数的思考：max、finder、best
	```
	*max 集合中选择最大值的项*
	var dd = _.max([1, 2, 3, 4, 5])
	console.log(dd) // 5
	var people = [{name: 'fed', age: 45}, {name: 'fan', age: 25}];
	var dd = _.max(people, function (item) {
		return item.age;
	});
	console.log(dd) // {name: "fed", age: 45}
	
	*finder *
	function finder(valueFun, bestFun, coll) {
		return _.reduce(coll, function (best, current) {
			var bestValue = valueFun(best);
			var currentValue = valueFun(current);
			return (bestValue === bestFun(bestValue, currentValue)) ? best : current; 
		});
	}
	var dd = finder(_.identity, Math.max, [1, 2, 3, 4, 5]);
	console.log(dd) // 5
	
	*best *
	function best(fun, coll){
		return _.reduce(coll, function (x, y) {
			return fun(x, y) ? x : y;	
		});	
	}
	var dd = best(function (x, y) {
		return x > y;
	}, [1, 2, 3, 4]);
	console.log(dd)// 4
	```
	关于传递函数的更多思考：重复、反复和条件迭代
	```
	function repeat(times, VALUE) {
		return _.map(_.range(times), function () { return VALUE; })
	}
	var dd = repeat(4, 'Major');
	console.log(dd) // ['Major','Major','Major','Major']
	
	// 使用函数，而不是值
	function repeatedly(times, fun) {
		return _.map(_.range(times), fun);
	}
	var dd = repeatedly(4, function () {
		return Math.floor((Math.random()*10) + 1)
	});
	console.log(dd)
	
	// 再次强调，"使用函数，而不是值"
	function iterateUntil(fun, check, init) {
		let ret = [];
		let result = fun(init);

		while (check(result)){
			ret.push(result);
			result = fun(result);
		}
		return ret;
	}

	let dd = iterateUntil(function (n){ return n+n}, function (n) { return n <= 1024}, 1)
	console.log(dd) // [2, 4, 8, 16, ...]
	```

####	返回其他函数的函数
	```
	function always(VALUE){
		return function (){
			return VALUE;
		}
	}
	var f = always(function () {})
	var g = always(function () {})
	console.log(f() === g()) // false
	
	function invoker(NAME, METHOD){
		return function(target){
			if (!existy(target)) fail("Must provide a target");
			let targetMethod = target[NAME];
			let args = _.rest(arguments);
			return doWhen((existy(targetMethod) && METHOD === targetMethod), function () {
				return targetMethod.apply(target, args);
			})
		}
	}
	let rev = invoker('reverse', Array.prototype.reverse);
	console.log(_.map([[1, 2, 3]], rev))
	
	// 捕获增量值
	function makeUniqueStringFunction(start) {
		let COUNTER = start;
		return function(prefix) {
			return [prefix, COUNTER++].join('');
		};
	}
	let dd = makeUniqueStringFunction(0);
	let d1 = dd('fanerge')
	let d2 = dd('fanerge')
	console.log(d1, d2); // fanerge0 fanerge1
	```

####	整合：对象校验器
	```
	function aMap(obj) {
		return _.isObject(obj);
	}
	function hasKeys() {
		let KEYS = _.toArray(arguments);
		let fun = function(obj){
			return _.every(KEYS, function (k){
				return _.has(obj, k);
			})
		};
		fun.message = cat(['Must have values for keys:'], KEYS).join(" ");
		return fun;
	}
	let checkCommand = checker(validator("Must be a map", aMap), hasKeys('msg', 'type'));
	console.log(checkCommand({msg: 'blah', type: 'display'})); //[]
	```
>	源代码仓库
	[函数式编程-读书笔记](https://github.com/fanerge/Functional-reading-notes)

































































