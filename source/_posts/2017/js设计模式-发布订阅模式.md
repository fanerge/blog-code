---
title: js设计模式-发布订阅模式
date: 2017-10-17 21:39:45
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	发布-订阅模式（观察者模式）
定义：对象间的一种一对多的关系，让多个观察者对象同时监听某一个主题对象，当一个对象发生改变时，所有依赖于它的对象都将得到通知。
	
#	现实生活中的发布-订阅模式	
比如小红最近在淘宝网上看上一双鞋子，但是呢 联系到卖家后，才发现这双鞋卖光了，但是小红对这双鞋又非常喜欢，所以呢联系卖家，问卖家什么时候有货，卖家告诉她，要等一个星期后才有货，卖家告诉小红，要是你喜欢的话，你可以收藏我们的店铺，等有货的时候再通知你，所以小红收藏了此店铺，但与此同时，小明，小花等也喜欢这双鞋，也收藏了该店铺；等来货的时候就依次会通知他们。在上面的故事中，可以看出是一个典型的发布订阅模式，卖家是属于发布者，小红，小明等属于订阅者，订阅该店铺，卖家作为发布者，当鞋子到了的时候，会依次通知小明，小红等，依次使用旺旺等工具给他们发布消息。
	
#	实现发布-订阅模式的步骤
1.首先要想好谁是发布者(比如上面的卖家)。
2.然后给发布者添加一个缓存列表，用于存放回调函数来通知订阅者(比如上面的买家收藏了卖家的店铺，卖家通过收藏了该店铺的一个列表名单)。
3.最后就是发布消息，发布者遍历这个缓存列表，依次触发里面存放的订阅者回调函数。
	
#	发布-订阅模式的代码封装	
```
var Event = (function(){
	var list = {}, // 缓存列表
		  listen, // 监听函数
		  trigger, // 触发监听
		  remove; // 移除监听函数
		  listen = function(key,fn){
			if(!list[key]) {
				list[key] = [];
			}
			list[key].push(fn);
		};
		trigger = function(){
			var key = Array.prototype.shift.call(arguments),
				 fns = list[key];
			if(!fns || fns.length === 0) {
				return false;
			}
			for(var i = 0, fn; fn = fns[i++];) {
				fn.apply(this,arguments);
			}
		};
		remove = function(key,fn){
			var fns = list[key];
			if(!fns) {
				return false;
			}
			if(!fn) {
				fns && (fns.length = 0);
			}else {
				for(var i = fns.length - 1; i >= 0; i--){
					var _fn = fns[i];
					if(_fn === fn) {
						fns.splice(i,1);
					}
				}
			}
		};
		return {
			listen: listen,
			trigger: trigger,
			remove: remove
		}
})();

// 测试代码
function d1() {
	console.log('我是第二个color监听的函数!'); 
}
function d2() {
	console.log('我是第二个color监听的函数!');
}
// 测试代码
Event.listen("color", d1); // 在 list['color'] 中绑定 d1 函数
Event.listen("color", d2); // 在 list['color'] 中绑定 d2 函数
Event.remove('color', d1); // 在 list['color'] 中移除 d1 函数
Event.trigger("color"); // 我是第二个color监听的函数!
```
PS：此处代码来源于网络。
	
>	参考文档：
	[Javascript中理解发布--订阅模式](http://www.cnblogs.com/tugenhua0707/p/4687947.html)
	[学习笔记-js发布/订阅模式的简单实现](http://www.cnblogs.com/weebly/p/5279952.html)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	