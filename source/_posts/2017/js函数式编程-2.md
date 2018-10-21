---
title: js函数式编程-2
date: 2017-07-20 19:11:05
category: "函数式编程"
tags: '函数式编程'
---
## 代码组合（compose）
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### 先上手一个组合函数的例子
	```
	var compose = function (f, g) { // 执行顺序为从右到左
		return function (x) {
			return f(g(x));
		}
	}
	// f 和 g 都是函数，x 是在它们之间通过“管道”传输的值。
	var toUppercase = function (x) { return x.toUpperCase() };
	var exclaim = function (x) { return `${x}!` };
	var shout = compose(exclaim, toUppercase);
	var test = shout('I am fanerge')
	console.log(test) // I AM FANERGE!
	```

### 组合的特性
	1. 结合律（associativity）	
	```
	var associative = compose(f, compose(g, h)) === compose(compose(f, g), h)
	// 必须满足相同的执行顺序及h-g-f（从右到左）
	var loudLastUpper = compose(exclaim, toUpperCase, head, reverse);
	// 或
	var last = compose(head, reverse);
	var loudLastUpper = compose(exclaim, toUpperCase, last);
	// 或
	var last = compose(head, reverse);
	var angry = compose(exclaim, toUpperCase);
	var loudLastUpper = compose(angry, last);
	```
	**其实只要把握好（从右向左执行的顺序即可）**
	2. pointfree模式
	定义：永远不必说出你的数据，函数无须提及将要操作的数据是什么样的。
	```
	//非 pointfree，因为提到了数据：word
	var snakeCase = function (word) {
		return word.toLowerCase().replace(/\s+/ig, '_');
	}
	// pointfree
	var snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase)
	```
	
### debug
	```
	// 正确做法：每个函数都接受一个实际的参数
	var latin = compose(map(angry), reverse) // angry = compose(exclaim, toUpperCase)
	latin(["frog", "eyes"]) // ['EYES!','FROG!']

	// 不纯的 trace 函数来追踪代码的执行情况。
	var trace = curry(function(tag, x){
	  console.log(tag, x);
	  return x;
	});
	// 使用
	var dasherize = compose(join('-'), toLower, trace("after split"), split(' '), replace(/\s{2,}/ig, ' '));
	// after split [ 'The', 'world', 'is', 'a', 'vampire' ]

### 组合背后的理论 --- 范畴学（category theory）
	作用：范畴学主要处理对象（object）、态射（morphism）和变化式（transformation）
	**对象的搜集**
	对象就是数据类型，例如 String、Boolean、Number 和 Object 等等。
	通常我们把数据类型视作所有可能的值的一个集合（set）。
	**态射的搜集**
	态射是标准的、普通的纯函数。
	**态射的组合**
	var g = function(x){ return x.length; };
	var f = function(x){ return x === 4; };
	var isFourLetterWord = compose(f, g);
	var test = isFourLetterWord('fanerge') // false
	**identity 这个独特的态射**
	identity 这个独特的态射
	```
	// id 的实用函数
	var id = function (x) { return x };
	// 结论成立
	compose(id, f) === compose(f, id) === f;
	总结：组合像一系列管道那样把不同的函数联系在一起，数据就可以也必须在其中流动——毕竟纯函数就是输入对输出
	
	```

参考：
> [js函数式编程](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch7.html#自由定理)
	
	
	
	
	
	
	
	
	
	
	
	
	



















