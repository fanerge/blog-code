---
title: js函数传参方式--按值传递
date: 2017-10-17 20:39:15
tags: ['js']
categories: 'js'
copyright: true
---
一直以为js函数传参方式--按引用传递，原来我一直错了。通过查阅资料彻底了解js函数传参是按值传递的。
**要搞清楚js函数传参方式，我们先需要具备一些基础知识。**
#	数据类型
	基本类型值: Undefined,Null,Boolean,Number,String。
	引用类型值: Object,Array,Function,Date等。
	
#	变量的复制
##	基本类型的复制
		众所周知，js中变量的基本类型和引用类型保存方式是不同的，这也就导致变量复制时也就不同了。
	如果从一个变量向另一个变量复制基本类型的值时，会将前者的值克隆一个，然后将克隆的值赋值到后者，
	因此这两个值是完全独立的，只是他们的value相同而已。
	```
	var num1 = 10;
	var num2 = num1;
	console.log(num2);//10
	```
		上面的num1中被保存的值为10，当把num1的值赋值给num2时，num2的值也为10。
	但是这两个10是完全独立的，num2中的10只是被克隆出来的，相当于我写了一个word文档，
	把它放到了num1的文件夹中，然后我再复制这个word文档，就叫word副本吧，然后把这个副本放到num2的文件夹下，
	这两个word文档是完全一样的，修改任何一个都不会影响另一个。
	```
	num2 += 1;
	console.log(num1); //10
	console.log(num2); //11
	```
##	引用类型的复制
	```
	var obj1 = {
	  name : "111"
	};
	var obj2 = obj1;
	console.log(obj2.name); //111
	obj2.name = "222";
	console.log(obj1.name); //222
	```
		第一次打印出的结果为“111”，这个我们很容易理解，但是第二次打印出来的是“222”，有点莫名其妙了。
	这就是引用类型和基本类型的不同之处了。复制对象时并不会在堆内存中新生成一个一模一样的对象，
	只是多了一个保存指向这个对象指针的变量罢了。将obj1的值复制给obj2，而这个值的副本实际上是一个指针，
	这个指针指向存储在堆中的一个对象，也就是说创建了一个新的内存地址传给了obj2，obj1和obj2两个变量同时指向了同一个Object，
	当去改变这个对象时，他们的值都会改变，也就是说他们中任何一个作出的改变都会反映在另一个身上。
	下面的简易图可能更明了些。
![引用复制](http://oxpnrlb4j.bkt.clouddn.com/%E5%BC%95%E7%94%A8%E5%A4%8D%E5%88%B6.png)

#	函数参数的传递（按值传递）
##	基本类型传递参数（）	
	```
	var count = 10;
	function num(num1){
	   num1 = 1;
	   return num1;
	}
	var result = num(count);
	console.log(result); //1
	console.log(count); //10，并未变成1
	```
		这个例子很容易理解，实际就是创建了一个count的副本，然后把count的副本的值传入参数中，
	因为函数中定义了参数的值，所以1就将10覆盖了，最后的result返回1，而count并未发生变化。
##	引用类型传递参数（按值传递）
	```
	var person  = {
		name : "Tom"
	};
	function obj(peo){
		// PS：这里 peo 对象和 person 对象的内存地址一模一样，所以后面两个才会同时改变
		peo.name = "Jerry";
		return peo;
	}
	var result = obj(person);
	console.log(result.name); // Jerry
	console.log(person.name); // Jerry
	```
		在上面的例子中，把person复制传入obj()中，peo和person指向了同一个对象，而在peo中修改了name属性，
	其实修改了它们共同指向的对象的name属性，相对应的外部person所引用的name属性也就改变了，
	所以打印出来的为Jerry。其实这个乍一看，感觉引用类型的参数是按照引用传递的，
	这就是我最初犯得错误。
	再来看一个例子。
	```
	var person = {
		name : "Tom"
	}; 
	function obj(peo){
		// PS：这里在函数里面新建了一个对象 peo，其内存地址和 person 对象内存地址不一样，所以两个对象不相干涉 
		peo = {
		   name : "Jerry"
		};
		return peo;
	}
	var result = obj(person);
	console.log(result.name);// Jerry
	console.log(person.name);// Tom
	```
		上面的例子中，在函数中重新定义了一个对象，也就是现在堆内存中有两个对象，
	外部的person指向的是老的对象，被传入参数后指向的是新定义的对象，所以调用后返回的值是新定义的对象的值。
	如果是参数是按引用传递的，那么person.name打印出来的结果为Jerry，从这点可以得出参数是按值传递的（有的地方叫做按共享传递）。
	
#	总结
**JavaScript中的函数不存在按引用传递，所有参数都是按值传递！
	引用类型的变量本就是一个引用，它的值是堆内存中Object的地址，
	当使用按值传递时传递的值本就是一个地址，所以在函数中对参数进行操作会影响到函数外对应的变量。**
>	参考文档：
	[JavaScript中函数参数的按值传递与按引用传递（即按地址传递）](http://www.cnblogs.com/dgjamin/p/4337677.html)
	[JS函数参数都是按值传递的！](http://blog.csdn.net/allenliu6/article/details/52516605)
	[js函数中参数的传递](http://www.cnblogs.com/open-wang/p/5208606.html)




