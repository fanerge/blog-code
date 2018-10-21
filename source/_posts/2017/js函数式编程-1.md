---
title: js函数式编程
date: 2017-07-18 21:52:23
category: "函数式编程"
tags: '函数式编程'
---
## 刚下班回家，最近打算利用空余时间撸函数式编程
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*
### 一等公民的函数
1. 与其他数据类型一样，可以把函数存在数组里，当作参数传递，赋值给变量...等等。
	
### 纯函数的理解
1. 定义：纯函数的定义是因为对相同的输入它保证能返回相同的输出。	
	```
	var xs = [1,2,3,4,5];
	// 纯的
	xs.slice(0,3);
	//=> [1,2,3]
	xs.slice(0,3);
	//=> [1,2,3]
	
	// 不纯的
	xs.splice(0,3);
	//=> [1,2,3]
	xs.splice(0,3);
	//=> [4,5]
	```
2. 纯函数中的副作用
	副作用的定义：只要是跟函数外部环境发生的交互就都是副作用（更改文件系统、往数据库插入记录）
3. 纯函数的特点
	可缓存性（Cacheable）
	可移植性／自文档化（Portable / Self-Documenting）
	可测试性（Testable）
	合理性（Reasonable）
	合理性（Reasonable）

### 柯里化（curry）
1. 定义：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
	```
	var add = function (x) {
		return function (y) {
			return x + y;
		}
	}
	var addTen = add(10)
	console.log(addTen(1)) // 11
	```
2. 示例
	```
	var curry = require('lodash').curry; // lodash.js是一个工具函数库
	var match = curry(function (what, str) {
		return str.match(what)
	})
	// 使用
	**match(/\s+/g, 'hello world') // [' ']**
	**match(/\s+/g)('hello world') // [' ']**
	var hasSpaces = match(/\s+/g) // function (x) { return x.match(/\s+/g) }
	hasSpaces('hello world') // [' ']
	
	// 再来一个列子
	var curry = require('lodash').curry; // lodash.js是一个工具函数库
	var replace = curry(function(what, replacement, str) {
	  return str.replace(what, replacement);
	})
	var noVowels = replace(/[aeiou]/ig) (传入第一个参数)// function (replacement, x) { return x.replace(/[aeiou]/ig, replacement) }
	var censored = noVowels('*') (传入第二个参数)// function (x) { return x.replace(/[aeiou]/ig, '*') }
	var end = censored('Chocolate Rain') (传入第三个参数)// 'Ch*c*l*t* R**n'
	```
	通过传递一到两个参数调用函数，就能得到一个记住了这些参数的新函数。
	只传给函数一部分参数通常也叫做局部调用（partial application）
	
	**高阶函数：参数或返回值为函数的函数（higher order function）**
	
	当我们谈论纯函数的时候，我们说它们接受一个输入返回一个输出。
	curry 函数所做的正是这样：每传递一个参数调用函数，就返回一个新函数处理剩余的参数。这就是一个输入对应一个输出啊。
	*记于2017-07-19 23:14*
> 参考[JS函数式编程指南](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch4.html#总结)





























