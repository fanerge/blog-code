---
title: js函数式编程-纯度、不变性和更改政策
date: 2017-08-10 21:13:43
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	纯度、不变性和更改政策

####	纯度
	纯函数
	1.其结果只能从它的参数的值来计算
	2.不能依赖于能被外部操作改变的数据
	3.不能改变外部状态

####	不变性
	```
	// 不变性
	function summ (array) {
		let result = 0;
		let sz = array.length;
		for (let i = 0; i < sz; i++) {
			result += array[i];
		}
		return result;
	}
	let dd = summ(_.range(1, 11));
	console.log(dd); // 55
	
	// 递归
	function summRec (array, seed) {
		if (_.isEmpty(array)){
			return seed;
		} else {
			return summRec(_.rest(array), _.first(array) + seed)
		}
	}
	let dd = summRec(_.range(1, 11), 0);
	console.log(dd); // 55
	
	// 冻结和克隆
	Object.freeze(a) // 浅冻结
	// 深冻结
	function deepFreeze(obj) {
		if (!Object.isFrozen(obj)) {
			Object.freeze(obj);
		}
		for (let key in obj) {
			if (!obj.hasOwnProperty(key) || !_.isObject(obj[key])) {
				continue;
			}
			deepFreeze(obj[key]);
		}
	}
	let x = [{a: [1, 2, 3], b: 42}, {c: {d: []}}];
	deepFreeze(x);
	x[0] = null;
	console.dir(x); // [{a: [1, 2, 3], b: 42}, {c: {d: []}}]
	```

####	控制变化的政策
	用容器封装数据，不能直接更改数据。
	```
	function Container (init) {
		this._value = init;
	}
	Container.prototype = {
		update: function (fun) {
			let args = _.rest(arguments);
			let oldValue = this._value;

			this._value = fun.apply(this, constructor(oldValue, args));
			return this._value;
		}
	};
	let aNumber = new Container(42);
	let dd = aNumber.update(function (n) { return n + 1; });
	console.log(dd) // 43
	```
>	源代码仓库
	[函数式编程-读书笔记](https://github.com/fanerge/Functional-reading-notes)




































































