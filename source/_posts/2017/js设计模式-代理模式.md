---
title: js设计模式-代理模式
date: 2017-10-15 20:24:36
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	代理模式
定义：为一个对象提供一个代用品或占位符，以便控制对它的访问。
	代理对象和本体对象实现了同样的接口，并且会把任何方法调用传递给本体对象；
举例：
	图片预加载、图片懒加载、
	合并HTTP请求（代理收集一定时间内的所有HTTP请求，然后一次性发给服务器）、
	惰性加载（通过代理处理和收集一些基本操作，然后仅在真正需要本体的时候才加载本体）、
	缓存代理（缓存请求结果、计算结果）

#	缓存代理
```
// 先实现具体的两个算法
const mult = function() {
	let a = 1;
	for (let i = 0; i < arguments.length; i++) {
		a *= arguments[i];
	}
	return a;
};
const plus = function() {
	let a = 0;
	for (let i = 0; i < arguments.length; i++) {
		a += arguments[i];
	}
	return a;
};

// 创建缓存代理
const createProxyFactory = function(fn) {
	let cache = {}; // 保存计算的结果
	// 使用闭包在内存中保留对cache的引用
	return function() {
		let args = Array.from(arguments).join(','); // 将所有参数转化为字符串作为缓存的 key
		if (args in cache) {
			return cache[args];
		} else {
			return cache[args] = fn.apply(this, arguments);
		}
	};
};

// 使用代理对象
const proxyMult = createProxyFactory(mult);
const proxyPlus = createProxyFactory(plus);
console.log(proxyMult(1,2,3,4)); // 24
console.log(proxyPlus(1,2,3,4)); // 10
```
PS：这里每次进行同类的计算时（乘法和加法两类），先判断缓存对象cache中是否存在该参数连接成的字符串作为key的属性。
如果有，则直接从cache中读取，否则就进行计算并保存其结果。
	
#	虚拟代理
虚拟代理：某一个花销很大的操作，可以通过虚拟代理的方式延迟到这种需要它的时候才去创建
例：使用虚拟代理实现图片懒加载
下面以虚拟代理来说明：
```
// 本体对象
const imgFunc = (function() {
	const imgNode = document.createElement('img');
	document.body.appendChild(imgNode);
	return {
		setSrc(src){
			imgNode.src = src;
		}
	}
})();

// 代理对象
const proxyImage = (function() {
	const img = new Image();
	img.onload = function() {
		imgFunc.setSrc(this.src);
	};
	return {
		setSrc(src){
			imgFunc.setSrc('./loading.gif');
			img.src = src;
		}
	};
})();

// 使用代理对象
proxyImage.setSrc('./reality.png');
```
PS：图片懒加载的方式：先通过一张loading图占位，然后通过异步的方式加载图片，等图片加载好了再把完成的图片加载到img标签里面。
这里讲述一下代理对象做了那些事：
1.创建了一个 Image 对象，并为其绑定了 onload 事件。
2.将 imgNode 先设置为 './loading.gif' 加载的菊花图。
3.当 Image 对象加载完真实的图片，也就是上文的 './reality.png' ,将 imgNode 设置为 './reality.png'。 
	
>	参考文档：
	[js设计模式（9）---代理模式](http://www.cnblogs.com/shamoguying1140/p/3169206.html)
	[JavaScript设计模式](https://juejin.im/post/59df4f74f265da430f311909)
	[ js用高阶函数动态创建缓存代理](http://blog.csdn.net/hi_xiexialing/article/details/54605065)
