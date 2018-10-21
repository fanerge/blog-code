---
title: js设计模式-迭代器模式
date: 2017-10-21 13:15:11
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	迭代器模式介绍
定义：迭代器模式是指提供一种方法顺序访问一个聚合对象中的各个元素，而又不需要暴露该对象的内部表示。
使用的好处：迭代器模式可以把迭代的过程从业务逻辑中分离出来，在使用迭代器模式之后，即使不关心对象的内部构造，也可以按顺序访问其中的每个元素。
先看一个 jquery 迭代器
```
$.each(array, function (i, n) {
	console.log(`当前为第${i}项，值为${n}`);
});
```
	
#	封装迭代器
##	内部迭代器
定义：迭代函数内部已经定义好了迭代原则，它完全接手整个迭代过程，外部只需要一次初始调用。	
```
const each = function(array, callback) {
	for (let i = 0, len = array.length; i < len; i++) {
		callback.call(array[i], array[i], i); 
	}
};
```
PS：这里设计each函数时参考了[].forEach函数，callback第一个参数为 value，第二个参数为 index。
使用内部迭代器：
```
each([1,2,3,4], function (val, ind) {
	console.log( val + " - " + ind ); // 输出数组值和下标
});
```
PS：现在 ES5 已经实现了内部迭代器 [].foreEach(callback(currentValue, index, array)[, this]); 
详细的使用说明请参考[MDN-Array.prototype.forEach()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
##	外部迭代器
定义：外部迭代器必须显式地请求迭代下一个元素，外部迭代器增加了一些调用的复杂度，但相对的也增强了迭代器的灵活性，我们可以手工控制迭代的过程或者顺序。
```
2 var Iterator = function (obj) {
	var current = 0;

	var next = function(){
		current += 1;
	};

	var isDone = function(){
		return current >= obj.length;
	};

	var getCurrItem = function(){
		return obj[ current ];
	};

	return {
		next: next,
		isDone: isDone,
		getCurrItem: getCurrItem
	}
}
```
使用：判断两个数组是不是相等
```
var compare = function( iterator1, iterator2 ){
	while( iterator1.isDone() && iterator2.isDone() ){
		if( iterator1.getCurrItem() !== iterator2.getCurrItem() ){
			throw new Error( "iterator1 和 iterator2不相等" );
		}
		iterator1.next();
		iterator2.next();
	}
	console.log( "iterator1 和 iterator2相等" );
};

var iterator1 = Iterator( [1,2,3,4] );
var iterator2 = Iterator( [1,2,3,4] );

compare(iterator1,iterator2);
```
##	中止迭代器
定义：在迭代过程，通过判断是否退出迭代器。
```
var each = function( arry, callback ){
	for( var i = 0, l = arry.length; i < l; i++ ){
		// callback 的执行结果返回false，提前中止迭代
		if( callback( arry[i], i ) === false ){
			break;
		}
	}
};
```
使用：
```
each( [1,2,3,4,5], function ( n, i ) {
	if( n>3 ){ // n 大于3的时候中止循环
		return false;
	}
	console.log(n); // 输出 1 2 3
});
```
#	迭代器应用示例
需求：根据不同的浏览器获取相应的上传组件对象，将不同的上传对象封装到各自的函数里; 如果函数可用，则返回该对象，否则返回false，提示迭代器继续向下迭代。
定义上传组件方法：
```
// 将不同的上传对象封装到各自的函数里; 如果函数可用，则返回该对象，否则返回false，提示迭代器继续
var getActiveUploadObj = function(){
	try{
		return new ActiceXObject( "TXFTNActiveX.FTNUpload" ); // IE 上传控件
	}catch(e){
		return false;
	}
};
var getFlashUploadObj = function(){
	if( supportFlash() ){
		var str = "<object type='application/x-shockwave-flash'></object>";
		return $( str).appendTo( $("body") );
	}
	return false;
};
var getFormUpl0adObj = function(){
	var str = "<input type='file' type='file' class='ui-file' />"; // 表单上传
	return $( str).appendTo( $("body") );
};
```
实现迭代器：
```
//迭代器代码
var iteratorUploadObj = function(){
	for( var i = 0, fn; fn = arguments[ i++ ]; ){
		var uploadObj = fn();
		if( uploadObj !== false ){
			return uploadObj;
		}
	}
};

var uploadObj = iteratorUploadObj( getActiveUploadObj, getFlashUploadObj, getFormUpl0adObj );
```
PS：这里说明一下，如果前面的上传方案可以使用就使用，否则就使用后面的上传方案（但是要保证至少一种方式所有浏览器都能实现）。

>	参考文档：
	[【Javascript设计模式12】-迭代器模式](http://www.alloyteam.com/2012/10/commonly-javascript-design-patterns-iterator-mode/)
	[JavaScript设计模式 - 迭代器模式](http://www.cnblogs.com/Medeor/p/5017879.html)
	
	
	
	
	