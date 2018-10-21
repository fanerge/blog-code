---
title: js设计模式-适配器模式
date: 2017-10-21 11:22:06
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	适配器模式介绍
定义：适配器模式（Adapter）是将一个类（对象）的接口（方法或属性）转化成客户希望的另外一个接口（方法或属性），
	适配器模式使得原本由于接口不兼容而不能一起工作的那些类（对象）可以一些工作。速成包装器（wrapper）。
使用场景：比如，当系统中某个接口的结构已经无法满足我们现在的业务需求，但又不能改动这个接口，
	因为可能原来的系统很多功能都依赖于这个接口，改动接口会牵扯到太多文件。
	因此应对这种场景，我们可以很快地想到可以用适配器模式来解决这个问题。	

#	适配两个库		
1.	下面我们来实现从Prototype库的$函数到YUI的get方法的转换。
这两个函数的功能比较相似，不过先看看她们在接口方面的差别：
Prototype库的$函数	
```
function $(){
	var elments = new Array();
	for(var i=0; i<arguments.length; i++){
		var element = arguments[i];
		if(typeof element == 'string'){
			element = document.getElementById(element);
		}
		if(arguments.length == 1){
			return element;
		}
		elments.push(element);
	}
	return elements;
}
```
YUI的get方法	
```
YAHOO.util.Dom.get = function(el){
	if(YAHOO.lang.isString(el)){
		return document.getElementById(el);
	}
	if(YAHOO.lang.isArray(el)){
		var c = [];
		for(var i= 0, len=el.length; i<len; i++){
			c[c.length] = YAHOO.util.Dom.get(el[i]);
		}
		return c;
	}
	if(el){
		return el;
	}
	return null;
}
```
2.	分析二者的区别：get具有一个参数，这个参数可以是一个HTML元素、字符串或者由字符串或HTML元素组成的数组，
与此不同，$函数没有正式列出参数，而是允许客户传入任意数目的参数，不管是字符串还是HTML元素都行。
3.	这里两种转换的适配器就应该这样写
$ --> get
```
function $2getAdapter(){
	return YAHOO.util.Dom.get(arguments);
}
对于从Prototype改投YUI的人应该如下使用
$ = $2getAdapter;
这样就可以继续使用 $ 方法了。
```
get --> $
```
function get2$Adapter(el){
	return $.apply(window, el instanceof Array ?el: [el]);
}
对于从YUI改投Prototype的人应该如下使用
YAHOO.util.Dom.get = get2$Adapter;
这样就可以继续使用 YAHOO.util.Dom.get 方法了。
```
	
#	总结
**	注意：适配器模式尽量少使用，就类似于在衣服上打补丁。特别是在接口还没有确定的时候使用，因为这样后期不利于维护，相反，这个时候我们应该重新思考我们的接口设计是否合理。 
**
那合适使用适配器模式好呢？如果有以下情况出现时，建议使用：
1.	使用一个已经存在的对象，但其方法或属性接口不符合你的要求；
2.	你想创建一个可复用的对象，该对象可以与其它不相关的对象或不可见对象（即接口方法或属性不兼容的对象）协同工作；
3.	想使用已经存在的对象，但是不能对每一个都进行原型继承以匹配它的接口。对象适配器可以适配它的父对象接口方法或属性。
	
另外，适配器模式和其它几个模式可能容易让人迷惑，这里说一下大概的区别：
1.	适配器和桥接模式虽然类似，但桥接的出发点不同，桥接的目的是将接口部分和实现部分分离，从而对他们可以更为容易也相对独立的加以改变。而适配器则意味着改变一个已有对象的接口。
2.	装饰者模式增强了其它对象的功能而同时又不改变它的接口，因此它对应程序的透明性比适配器要好，其结果是装饰者支持递归组合，而纯粹使用适配器则是不可能的。
3.	代理模式在不改变它的接口的条件下，为另外一个对象定义了一个代理。		
		
		
		
		
		
		
		
		
		









>	参考文档：
	[腾讯-曽探](http://www.alloyteam.com/2012/10/commonly-javascript-design-patterns-adapter-mode/)
	[深入理解JavaScript系列（39）：设计模式之适配器模式](http://www.cnblogs.com/TomXu/archive/2012/04/11/2435452.html)
	[JS设计模式——11.适配器模式](http://www.cnblogs.com/JChen666/p/3658551.html)























