---
title: js设计模式-备忘录模式
date: 2017-11-06 20:00:30
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	备忘录模式的基础
定义：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样就可以将该对象恢复到原先保存的状态
作用：在我们的开发中偶尔会遇到这样一种情况，需要对用户的行为进行撤销。要想实现撤销，首先需要保存软件系统的历史状态，当用户执行撤销时用之前的状态覆盖当前状态。本节介绍的备忘录模式提供了一种状态恢复的实现机制，使得用户可以方便的回到一个特定的历史步骤。备忘录模式在js中经常用于数据缓存。		
使用场景：分页控件、撤销组件	
		
#	分页控件
```
var Page = function(){
	let page = 1,
		cache = {},
		data;
	return function( page ){
		if ( cache[ page ] ){
				data =  cache[ page ];
				render( data );
		}else{
				Ajax.send( 'cgi.xx.com/xxx', function( data ){
				   cache[ page ] = data;
				   render( data );
				});
		}
	}
}();
```
PS：分页控件, 从服务器获得某一页的数据后可以存入缓存。以后再翻回这一页的时候，可以直接使用缓存里的数据而无需再次请求服务器。	

>	参考文档：
[《javascript设计模式 – 备忘录模式》](http://www.isjs.cn/?p=998)
[【Javascript设计模式14】-备忘录模式](http://www.alloyteam.com/2012/10/commonly-javascript-design-patterns-memorandum-mode/)	
		
		
		
		
		
		