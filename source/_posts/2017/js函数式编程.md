---
title: js函数式编程基本概念
date: 2017-08-03 20:00:21
category: "函数式编程"
tags: '函数式编程'
---
#	高阶函数
满足下列条件之一则为高阶函数：
1.	接受一个或多个函数作为输入。
2.	输出一个函数。

#	函数的合成（Compose）
如果一个值要经过多个函数，才能变成另外一个值，就可以把所有中间步骤合并成一个函数，这叫做"函数的合成"（compose）。
通用 compose 函数
```
let compose = function(...args) {
	let len = args.length,
		count = len - 1,
		result;
	return function f1(...args1) {
		result = args[count].apply(this, args1);
		if (count <= 0) {
			count = len - 1;
			return result;
		} else {
			count--;
			return f1.call(null, result);
		}
	}
}
```
PS：具有以下特点
1.	第一函数接受参数, 其他函数接受的上一个函数的返回值
2.	第一个函数的参数是多元的, 其他函数的一元的
3.	自右向左执行

#	函数柯里化（curry）
把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数的函数或者计算结果（参数足够的情况）。
```
let curry = function(fn) {
	let limit = fn.length;
	return function judgeCurry (...args) {
		if (args.length >= limit) {
			return fn.apply(null, args);
		} else {
			return function(...args2) {
				return judgeCurry.apply(null, args.concat(args2));                                     
			}
		}
	}
}
```
PS：具有以下特点
1.	当传入参数少于原本需要的参数个数就返回一个函数
2.	当传入参数等于原本需要的参数个数就返回计算结果

#	总结
##	纯函数
输入输出数据流全是显式(函数与外界交换数据只有一个唯一渠道——参数和返回值)的。
非纯函数的操作读取全局变量/修改全局变量
##	函数副作用
函数副作用是指函数在正常工作任务之外对外部环境所施加的影响（修改外部全局变量）。
##	Compose
函数组合，单一职责原则
如果一个值要经过多个函数，才能变成另外一个值，就可以把所有中间步骤合并成一个函数，这叫做"函数的合成"（compose）。
##	pointfree
无参编程，函数无须提及将要操作的数据是什么样的，结合curry和compose。
就是使用一些通用的函数，组合出各种复杂运算。
`var snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase);`
##	Curry
函数柯里化，只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
##	偏函数
所谓偏函数，就是固定一个函数的一个或者多个参数，返回一个新的函数，这个函数用于接受剩余的参数。
##	高阶函数
一个函数的参数或者返回值里有函数。






