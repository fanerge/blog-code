---
title: js函数式编程-由函数构建函数
date: 2017-08-08 19:02:29
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	由函数构建函数

####	函数式组合的精华
	函数组合的精华：使用现有的零部件来建立新的行为，这些新行为同样也成为了已有的零部件。
	```
	function dispatch() {
		let funs = _.toArray(arguments);
		let size = funs.length;
		return function (target) {
			let ret = undefined;
			let args = _.rest(arguments);
			for(let funIndex = 0; funIndex < size; funIndex++){
				let fun = funs[funIndex];
				ret = fun.apply(fun, construct(target, args));
				if (existy(ret)) return ret;	
			}
			return ret;
		}
	}

	let str = dispatch(invoker('toString', Array.prototype.toString),
		invoker('toString', String.prototype.toString)
		)
	let dd = str('a');

	console.log(dd) // 'a'
	```

####	柯里化（Currying）
	```
	function rightAwayInvoker() {
		let args = _.toArray(arguments);
		let method = args.shift();
		let target = args.shift();
		return method.apply(target, args);
	}
	let dd = rightAwayInvoker(Array.prototype.reverse, [1, 2, 3]);
	console.log(dd) // [3, 2, 1]
	```
	柯里化的方向（默认向右）
	```
	function rigthCurryDiv(d) {
		return function (n) {
			return n/d;
		};
	}
	let divideBy10 = rigthCurryDiv(10); // 被除数
	let dd = divideBy10(2); // 除数
	var d1 = rigthCurryDiv(10)(2)
	console.log(d1); // 0.2
	
	// 柯里化一个参数
	function curry(fun) {
		return function(arg) {
			return fun(arg);
		};
	}
	let dd = ['11', '11', '11' ,'11'].map(curry(parseInt))
	console.log(dd) // ['11', '11', '11' ,'11']
	
	// 柯里化两个参数
	function curry2(fun) {
		return function(secondArg) {
			return function(firstArg) {
				return fun(firstArg, secondArg);
			}
		}
	}
	function div(n, d) {
		return n / d;
	}
	let dd = curry2(div)(10)(50);
	console.log(dd) // 5
	
	function toHex(n) {
		let hex = n.toString(16);
		return (hex.length < 2) ? [0, hex].join('') : hex;
	}
	function rgbToHexString(r, g, b) {
		return ['#', toHex(r), toHex(g), toHex(b)].join('');
	}
	let dd = rgbToHexString(255, 255, 255);
	console.log(dd) // #ffffff
	```

####	部分应用
	```
	function partial (fun) {
		let pargs = _.rest(arguments);
		return function () {
			let args = cat(pargs, _.toArray(arguments));
			return fun.apply(fun, args);
		};
	}
	var div10by2by4by5000partial = partial(div, 10, 2, 4, 5000);
	console.log(div10by2by4by5000partial()) // 5
	
	// 局部应用实战：前置条件
	function condition1() {
		let validators = _.toArray(arguments);
		return function (fun, arg) {
			let errors = mapcat(function(isValid){
				return isValid(arg) ? [] : [isValid.message];
			}, validators);
			if (!_.isEmpty(errors)) {
				throw new Error(error.join(','));
			}
			return fun(arg);
		};
	}
	let sqrPre = condition1(validator('arg must not be zero', complement(zero)),
		validator('arg must be a number', _.isNumber));
	let dd = sqrPre(_.identity, 10);
	console.log(dd)
	```
####	通过组合端至端的拼接函数
	```
	let isntString = _.compose(function (x) { return !x; }, _.isString);
	let dd = isntString([]);
	console.log(dd)	// true
	```
>	源代码仓库
	[函数式编程-读书笔记](https://github.com/fanerge/Functional-reading-notes)























































































