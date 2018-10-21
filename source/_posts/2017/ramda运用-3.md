---
title: ramda数组的操作和透镜
date: 2017-08-23 20:09:42
category: "函数式编程"
tags: ['js','函数式编程','ramda']
---
继续函数式编程的学习。

####	数据不变性和数组

#####	读取数组元素
	nth -- 类型于对象prop
	slice -- 类似于对象pick
	contains -- 类似于对象的has
	```
	const numbers = [10, 20, 30, 40, 50, 60]
	R.nth(3, numbers) // => 40  (0-based indexing)
	R.nth(-2, numbers) // => 50 (negative numbers start from the right)
	R.slice(2, 5, numbers) // => [30, 40, 50] (see below)
	R.contains(20, numbers) // => true
	```
	nth(0) === head
	nth(length-1) === last
	tail -- 访问除首个元素之外的所有元素的函数
	init -- 除最后一个元素之外的所有元素的方法
	take(N) -- 前 N 个元素
	takeLast(N) -- 后 N 个元素
	```
	const numbers = [10, 20, 30, 40, 50, 60]
	R.head(numbers) // => 10
	R.tail(numbers) // => [20, 30, 40, 50, 60]
	R.last(numbers) // => 60
	R.init(numbers) // => [10, 20, 30, 40, 50]
	R.take(3, numbers) // => [10, 20, 30]
	R.takeLast(3, numbers) // => [40, 50, 60]
	```

#####	增、删、改数组元素
	```
	insert：将元素插入到 list 指定索引处。
	R.insert(2, 'x', [1,2,3,4]); //=> [1,2,'x',3,4]
	update：替换数组中指定索引处的值。
	R.update(1, 11, [0, 1, 2]);     //=> [0, 11, 2]
	append：在列表末尾拼接一个元素。
	R.append('tests', ['write', 'more']); //=> ['write', 'more', 'tests']
	prepend：在列表头部之前拼接一个元素。
	R.prepend('fee', ['fi', 'fo', 'fum']); //=> ['fee', 'fi', 'fo', 'fum']
	concat：连接列表或字符串。
	R.concat([4, 5, 6], [1, 2, 3]); //=> [4, 5, 6, 1, 2, 3]
	// 反转数组拼接
	const concatAfter = R.flip(R.concat)
	remove：删除列表中从 start 开始的 count 个元素。
	R.remove(2, 3, [1,2,3,4,5,6,7,8]); //=> [1,2,6,7,8]
	without：求第二个列表中，未包含在第一个列表中的任一元素的集合。
	R.without([1, 2], [1, 2, 1, 3, 4]); //=> [3, 4]
	drop：删除给定 list，string 或者 transducer/transformer（或者具有 drop 方法的对象）的前 n 个元素。
	R.drop(1, ['foo', 'bar', 'baz']); //=> ['bar', 'baz']
	dropLast：删除 "list" 末尾的 n 个元素。
	R.dropLast(1, ['foo', 'bar', 'baz']); //=> ['foo', 'bar']
	```
	
#####	变换元素	
	```
	adjust：将数组中指定索引处的值替换为经函数变换的值。
	R.adjust(R.add(10), 1, [1, 2, 3]);     //=> [1, 12, 3]
	```

####	透镜（Lenses）

#####	什么是透镜？
	透镜将 "getter" 和 "setter" 函数组合为一个单一模块。Ramda 提供了一系列配合透镜一起工作的函数。
	可以将透镜视为对某些较大数据结构的特定部分的聚焦、关注。

#####	如何创建透镜
	lens：返回封装了给定 getter 和 setter 方法的 lens 。 getter 和 setter 分别用于 “获取” 和 “设置” 焦点（lens 聚焦的值）。
	```
	var xLens = R.lens(R.prop('x'), R.assoc('x'));
	R.view(xLens, {x: 1, y: 2});            //=> 1
	R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
	// 还有三个便捷函数
	LensProp：创建关注对象某一属性的透镜。
	lensPath: 创建关注对象某一嵌套属性的透镜。
	lensIndex: 创建关注数组某一索引的透镜。
	```

#####	我能用它做什么呢？
	Ramda 提供了三个配合透镜一起使用的的函数：
		view：读取透镜的值。
		set：更新透镜的值。
		over：将变换函数作用于透镜。
	```
	var xLens = R.lens(R.prop('x'), R.assoc('x'));
	R.view(xLens, {x: 1, y: 2});            //=> 1
	R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
	R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
	// 区别:
	// set 和 over 会按指定的方式对被透镜关注的属性进行修改，并返回整个新的对象。
	```

####	总结
	本节学习了：某些较大数据结构的处理 -- 透镜。
	数组：读取(nth、slice、contains、head、last、tail、init、take、takeLast)
	增、删、改(insert、update、append、prepend、concat、remove、without、drop、dropLast)
	变换(adjust)
>	参考文献：
	[JavaScript函数编程-Ramdajs](http://www.cnblogs.com/whitewolf/p/javascript-functional-programming-Ramdajs.html)
	[Thinking in Ramda系列文章](https://zhuanlan.zhihu.com/p/27446536)