---
title: js 中稀疏数组的一些知识
date: 2021-01-13 08:38:30
tags: javascript
keywords: javascript
copyright: true
---
##	什么是稀疏数组？
在说稀疏数组之前，你需要知道很多语言将数组的分为稀疏数组与密集数组（区别是数组的各个子元素是否有孔，我们称为"hole"）。也就是说稀疏数组中的元素之间可以有空隙，在那些仅有少部分项被使用的数组中，hole 可以大大减少内存空间的浪费。

##	V8中数组的实现
###	快数组（FAST ELEMENTS）
快数组是一种线性的存储方式。新创建的空数组，默认的存储方式是Fast Elements方式，快数组长度是可变的，可以根据元素的增加和删除来动态调整存储空间大小，内部是通过扩容和收缩机制实现，那来看下源码中是怎么扩容和收缩的。
####	Fast Holey Elements模式	
前面说了新创建的数组，默认是Fast Elements方式。在Fast Elements 模式中有一个扩展，是 Fast Holey Elements 模式。Fast Holey Elements 模式适合于数组中的空洞情况，即只有某些索引存有数据，而其他的索引都没有赋值的情况。在 Fast Holey Elements 模式下，当容量小于1024时，没有赋值的数组索引将会存储一个特殊的值，这样在访问这些位置时就可以得到 undefined。但是 Fast Holey Elements 同样会动态分配连续的存储空间，分配空间的大小由最大的索引值决定。
```
// 如下产生Fast Holey Elements的两种方式
let a = new Array(10);
console.log(a);
// (10) [empty × 10]

let b = [1, , , , 4];
console.log(b)
// (5) [1, empty × 3, 4]
```
###	慢数组（Dictionary Elements）
慢数组是一种字典的内存形式。不用开辟大块连续的存储空间，节省了内存，但是由于需要维护这样一个 HashTable，其效率会比快数组低。
在 Fast Elements 模式下，capacity 用于指示当前内存占用量大小，通常根据数组当前最大索引的值确定。在数组索引过大，超过 capacity 到一定程度( 由V8中 kMaxGap 常量决定，其值为 1024) ，数组将直接转化为 Dictionary Elements 模式。
[更多关于V8的知识请移步 justjavac 的专栏](https://www.zhihu.com/column/v8core)

##	常见数组方法如何来处理稀疏数组？
这里我对数组的 map、find、findIndex、filter、forEach、reduce方法做实验。
###	map 方法
```
var array = [1,,,2]
var mapArray = array.map((item, index) => {
	console.log(index)
	return item + 1;
})
// 方法执行是打印 0，3 似乎跳过了数组中 hole 元素
console.log(mapArray)
// (4) [2, empty × 2, 3]
```
[根据规范中定义的算法，如果被 map 调用的数组是稀疏数组，新数组将也是离散的保持相同的索引为空。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

###	find 方法
```
var array = [1,,,2]
var findArray = array.find((item, index) => {
	console.log(index)
	return item >= 2;
})
// 方法执行是打印 0，1，2，3 没有跳过 hole 元素
console.log(findArray)
// 2

```
###	findIndex 方法
```
var array = [1,,,2]
var findIndexArray = array.findIndex((item, index) => {
	console.log(index)
	return item >= 2;
})
// 方法执行是打印 0，1，2，3 没有跳过 hole 元素
console.log(findIndexArray)
// 3
```

###	filter 方法
```
var array = [1,,,2]
var filterArray = array.filter((item, index) => {
	console.log(index)
	return item >= 2;
})
// 方法执行是打印 0，3 似乎跳过了数组中 hole 元素
console.log(filterArray)
// [2]
```
###	forEach 方法
```
var array = [1,,,2]
array.forEach((item, index) => {
	console.log(index)
})
// 方法执行是打印 0，3 似乎跳过了数组中 hole 元素
```
###	reduce 方法
```
var array = [1,,,2]
var count = array.reduce((acc, item, index) => {
	console.log(index)
	return acc + item;
}, 0)
// 方法执行是打印 0，3 似乎跳过了数组中 hole 元素
console.log(count)
// 3
```

##	总结
本文只是想通过测试让大家更了解常见数组方法如何来处理稀疏数组，常见的数组方法如 map、forEach、filter、reduce 都会跳过数组中的 hole 元素。前端还有很多细节可挖掘，我们继续前行吧。






