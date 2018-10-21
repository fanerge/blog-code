---
title: Generator函数的用法
date: 2017-11-11 15:48:46
tags: 'js'
categories: 'js'
copyright: true
---

#	Generator函数的定义
从语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。
执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield表达式，定义不同的内部状态（yield在英语里的意思就是“产出”）。

#	Generator函数的简单示例
	```
	function* g() {
	  yield 'hello';
	  yield 'world';
	  return 'ending';
	}

	var demo = g();
	```
PS：上面代码定义了一个 Generator 函数g，它内部有两个yield表达式（hello和world），即该函数有三个状态：hello，world 和 return 语句（结束执行）。
<span style="color: red">返回的是，遍历器对象（Iterator Object）。例如这里，{ value: 'hello', done: false }</span>
#	yield 表达式	
由于 Generator 函数返回的遍历器对象，只有调用next方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。yield表达式就是暂停标志。
	（1）遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。
	（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。
	（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。
	（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

#	for...of 循环
	可以遍历 数组 和 实现了 Symbol.iterator 方法的对象。
for...of循环可以自动遍历 Generator 函数时生成的Iterator对象，且此时不再需要调用next方法。
	```
	function *foo() {
	  yield 1;
	  yield 2;
	  yield 3;
	  yield 4;
	  yield 5;
	  return 6;
	}

	for (let v of foo()) {
	  console.log(v);
	}
	// 1 2 3 4 5
	```

#	Generator.prototype.next()	
yield表达式本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。
next() 方法返回一个包含属性 done 和 value 的对象。该方法也可以通过接受一个参数用以向生成器传值。
	```
	function* gen() { 
	  yield 1;
	  yield 2;
	  yield 3;
	}

	var g = gen(); // "Generator { }"
	g.next();      // "Object { value: 1, done: false }"
	g.next();      // "Object { value: 2, done: false }"
	g.next();      // "Object { value: 3, done: false }"
	g.next();      // "Object { value: undefined, done: true }"
	```
	
#	Generator.prototype.throw()
Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。	
	
#	Generator.prototype.return() 
Generator 函数返回的遍历器对象，还有一个return方法，可以返回给定的值，并且终结遍历 Generator 函数。
	```
	function* gen() {
	  yield 1;
	  yield 2;
	  yield 3;
	}

	var g = gen();

	g.next()        // { value: 1, done: false }
	g.return('foo') // { value: "foo", done: true }
	g.next()        // { value: undefined, done: true }
	```
PS：遍历器对象g调用return方法后，返回值的value属性就是return方法的参数foo。并且，Generator函数的遍历就终止了，返回值的done属性为true，以后再调用next方法，done属性总是返回true。
如果return方法调用时，不提供参数，则返回值的value属性为undefined。

#	比较一下多个异步操作的编码
step1完成才能做step2，step2完成才能做step3，step3完成才能做step4

##	回调函数(
	```
	step1(function (value1) {
	  step2(value1, function(value2) {
		step3(value2, function(value3) {
		  step4(value3, function(value4) {
			// Do something with value4
		  });
		});
	  });
	});
	```
PS：层数多了就形成了回调地狱。

##	Promise 组织代码
	```
	Q.fcall(step1)
	  .then(step2)
	  .then(step3)
	  .then(step4)
	  .then(function (value4) {
		// Do something with value4
	  }, function (error) {
		// Handle any error from step1 through step4
	  })
	  .done();
	```
PS：是不是代码稍微要清晰一些了。

##	Generator 组织代码
	```
	function* longRunningTask() {
	  try {
		var value1 = yield step1();
		var value2 = yield step2(value1);
		var value3 = yield step3(value2);
		var value4 = yield step4(value3);
		// Do something with value4
	  } catch (e) {
		// Handle any error from step1 through step4
	  }
	}
	
	function scheduler(task) {
	  setTimeout(function() {
		var taskObj = task.next(task.value);
		// 如果Generator函数未结束，就继续调用
		if (!taskObj.done) {
		  task.value = taskObj.value
		  scheduler(task);
		}
	  }, 0);
	}
	
	scheduler(longRunningTask());
	```

>	参考文档：
	[Generator 函数的语法](http://es6.ruanyifeng.com/#docs/generator)
	[MDN-Generator](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator)
	[ES6 Generator 函数的使用](http://blog.csdn.net/jiangbo_phd/article/details/51820642)





