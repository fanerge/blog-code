---
title: js函数式编程-3
date: 2017-07-21 19:11:19
category: "函数式编程"
tags: '函数式编程'
---
## 实例应用
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### 声明式代码
1. 理解命令式与声明式
	```
	// 命令式
	var makes = [];
	for (let i, len = cars.length; i++) {
		makes.push(cars[i]make)	
	}
	
	// 声明式
	var makes = cars.map((car) => { return car.make; })
	总结：
	命令式的循环要求你必须先实例化一个数组，而且执行完这个实例化语句之后，解释器才继续执行后面的代码。然后再直接迭代 cars 列表，手动增加计数器，把各种零零散散的东西都展示出来...实在是直白得有些露骨。
	何收集，都有很大的自由度。它指明的是做什么，不是怎么做。因此，它是正儿八经的声明式代码。
	```
2. 例子
	```
	// 命令式 --- 硬编码了那种一步接一步的执行方式。
	var authenticate = function (form) {
		var user = toUser(form);
		return logIn(user);
	}
	// 声明式 --- 用户验证是 toUser 和 logIn 两个行为的组合。
	var authenticate = compose(logIn, toUser)
	```
3. 示例[函数式编程demo](https://github.com/fanerge/Functional-Programming)
	1.根据特定搜索关键字构造 url
	2.向 flickr 发送 api 请求 （不纯）
	3.把返回的 json 转为 html 图片
	4.把图片放到屏幕上 （不纯）
4. 有原则的重构
	// map 的组合律 
	var law = compose(map(f), map(g)) === map(compose(f, g))

### Hindley-Milner 类型签名
1. 初始类型
	类型签名作用：短短一行，就能暴露函数的行为和目的，可以用类型签名生成文档，也可以用注释来达到区分类型的目的。
2. Hindley-Milner 系统
	```
	// capitalize :: String -> String （表示输入为string，输出也为string）
	var capitalize = function (s) {
		return toUpperCase(head(s)) + toLowerCase(tail(s));
	}
	// 使用
	capitalize('fanerge') // 'Fanerge'
	
	//  id :: a -> a （同一类型的）
	var id = function(x){ return x; }
	
	//  map :: (a -> b) -> [a] -> [b]
	var map = curry(function(f, xs){
	  return xs.map(f);
	});
	// 说明
	给定一个从 a 到 b 的函数和一个 a 类型的数组作为参数，它就能返回一个 b 类型的数组。
	```
3. 缩小可能性范围（parametricity）
	```
	// head :: [a] -> a
	它接受 [a] 返回 a,参数是个数组
	```
4. 自由定理（free theorems）
	```
	// head :: [a] -> a
	compose(f, head) == compose(head, map(f));
	等式左边说的是，先获取数组的头部（译者注：即第一个元素），然后对它调用函数 f；等式右边说的是，先对数组中的每一个元素调用 f，然后再取其返回结果的头部。这两个表达式的作用是相等的，但是前者要快得多。
	```

参考：
> [js函数式编程](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch7.html#自由定理)

























