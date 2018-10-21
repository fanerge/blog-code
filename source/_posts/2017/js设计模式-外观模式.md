---
title: js设计模式-外观模式
date: 2017-10-22 22:17:50
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	外观模式基础
定义：外观模式（Facade）为子系统中的一组接口提供了一个一致的界面，此模块定义了一个高层接口，这个接口使得这一子系统更加容易使用。
外观模式不仅简化类中的接口，而且对接口与调用者也进行了解耦。外观模式经常被认为开发者必备，它可以将一些复杂操作封装起来，并创建一个简单的接口用于调用。
外观模式经常被用于JavaScript类库里，通过它封装一些接口用于兼容多浏览器，外观模式可以让我们间接调用子系统，从而避免因直接访问子系统而产生不必要的错误。
外观模式的优势是易于使用，而且本身也比较轻量级。但也有缺点外观模式被开发者连续使用时会产生一定的性能问题，因为在每次调用时都要检测功能的可用性。

#	兼容浏览器事件绑定
下面是一段未优化过的代码，我们使用了外观模式通过检测浏览器特性的方式来创建一个跨浏览器的使用方法。
```
const addEvent = function(el, ev, fn) {
	if (el.addEventListener) {
		el.addEventListener(ev, fn, false);
	} else if (el.attachEvent) {
		el.attachEvent(`on${ev}`, fn);
	} else {
		el.[`on${ev}`] = fn;
	}
};
```
PS：这里有一个问题，我们每调用一次 addEvent 函数就是执行一次 if-else 判断。
#	兼容浏览器阻止冒泡、默认事件
```
let N = window.N || {};
N.tools = {
	stopPropagation(e) {
		if (e.stopPropagation) {
			e.stopPropagation();
		} else {
			e.cancelBubble = true;
		}
	},
	preventDefault(e) {
		if (e.preventDefault) {
			e.preventDefault();
		} else {
			e.returnValue = false;
		}
	},
	stopEvent (e) {
		this.stopPropagation(e);
		this.preventDefault(e);
	}
};
```

>	参考文档：
	[深入理解JavaScript系列（30）：设计模式之外观模式](http://www.cnblogs.com/TomXu/archive/2012/02/28/2353448.html)
	[javascript设计模式-外观模式](http://www.cnblogs.com/linda586586/p/4237093.html)