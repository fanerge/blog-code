---
title: Promise的用法
date: 2017-11-11 10:31:27
tags: 'js'
categories: 'js'
copyright: true
---
ES6出来了很久，Promise也一直在用，现在总结一下具体用法。

#	Promise 定义
Promise 对象用于一个异步操作的最终完成（或失败）及其结果值的表示。所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。
![MDN](https://mdn.mozillademos.org/files/8633/promises.png)
	这里使用MDN的图片
	
#	基本方法
##	创造了一个 Promise 实例

	```
	const promise = new Promise(function(resolve, reject) {
	  // ... some code

	  if (/* 异步操作成功 */){
		resolve(value);
	  } else {
		reject(error);
	  }
	});
	```
	
##	对 Promise 实例成功 或 失败 做相应的处理
	```
	promise.then(function(value) {
	  // success
	}, function(error) {
	  // failure
	});
	```
	
#	Promise.prototype.then()
Promise 实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。它的作用是为 Promise 实例添加状态改变时的回调函数。then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。

##	分为三种调用形式（注意最后面需要带着错误处理函数）
1.	promise.then((resolve) => { // 成功的处理函数 }, (reject) => { // 错误的处理函数 })
2.	promise.then((resolve) => { // 成功的处理函数 }).then((resolve) => { // 成功的处理函数 }, (reject) => { // 错误的处理函数 })
3.	promise.then(null, (reject) => { // 错误的处理函数 })

##	返回值
返回一个<span style="color: red">新的</span>Promise 对象，从而达到链式调用。

#	Promise.prototype.catch()
Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。
	```
	promise.then(function(resolve) {
	  // 成功的处理函数
	}).catch(function(error) {
	  console.log(error);
	});
	```

#	Promise.all()
##	用法
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
	```
	const p = Promise.all([p1, p2, p3]);
	```
PS：Promise.all方法接受一个数组或具有Iterator 接口的对象作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。

##	返回值
（1）只有p1、p2、p3的状态都变成resolved，p的状态才会变成resolved，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

#	Promise.race()
##	用法
Promise.race方法同样是将多个Promise实例，包装成一个新的Promise实例。
	```
	const p = Promise.race([p1, p2, p3]);
	```
PS：Promise.race方法的参数与Promise.all方法一样，如果不是 Promise 实例，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。
##	返回值
只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

#	Promise.resolve()
有时需要将现有对象转为Promise对象，Promise.resolve方法就起到这个作用。
	```
	const jsPromise = Promise.resolve($.ajax('/whatever.json'));
	```
PS：将jQuery生成的deferred对象，转为一个新的Promise对象。

##	根据不同参数，返回结果情况
1.	参数是一个Promise实例
	如果参数是Promise实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。
2.	参数是一个thenable对象（thenable对象指的是具有then方法的对象）
	Promise.resolve方法会将这个对象转为Promise对象，然后就立即执行thenable对象的then方法。
3.	参数不是具有then方法的对象，或根本就不是对象
	如果参数是一个原始值，或者是一个不具有then方法的对象，则Promise.resolve方法返回一个新的Promise对象，状态为resolved。
4.	不带有任何参数
	Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的Promise对象。

#	Promise.reject()
Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。
下面两种形式一样：
	```
	const p = Promise.reject('出错了');
	// 等同于
	const p = new Promise((resolve, reject) => reject('出错了'))

	p.then(null, function (s) {
	  console.log(s)
	});
	// 出错了
	```

#	自己部署有用的方法	
##	done()
###	解决的问题
Promise对象的回调链，不管以then方法或catch方法结尾，要是最后一个方法抛出错误，都有可能无法捕捉到（因为Promise内部的错误不会冒泡到全局）。因此，我们可以提供一个done方法，总是处于回调链的尾端，保证抛出任何可能出现的错误。

###	部署 done 方法
	```
	Promise.prototype.done = function (onFulfilled, onRejected) {
	  this.then(onFulfilled, onRejected)
		.catch(function (reason) {
		  // 抛出一个全局错误
		  setTimeout(() => { throw reason }, 0);
		});
	};
	```
	
###	使用
	```
	asyncFunc()
		.then(f1)
		.catch(r1)
		.then(f2)
		.done();
	```

##	finally()
###	解决的问题
finally方法用于指定不管Promise对象最后状态如何，都会执行的操作。它与done方法的最大区别，它接受一个普通的回调函数作为参数，该函数不管怎样都必须执行。

###	部署finally方法
	```
	Promise.prototype.finally = function (callback) {
	  let P = this.constructor;
	  return this.then(
		value  => P.resolve(callback()).then(() => value),
		reason => P.resolve(callback()).then(() => { throw reason })
	  );
	};
	```
###	使用
下面是一个例子，服务器使用Promise处理请求，然后使用finally方法关掉服务器。
	```
	server.listen(0)
		.then(function () {
		// run test
		})
		.finally(server.stop);
	```

#	Promise.try()
<span style="color: red">目前还为提案，Promise 库Bluebird、Q和when，提供了这个方法。</span>
##	使用场景
实际开发中，经常遇到一种情况：不知道或者不想区分，函数f是同步函数还是异步操作，但是想用 Promise 来处理它。因为这样就可以不管f是否包含异步操作，都用then方法指定下一步流程，用catch方法处理f抛出的错误。	
##	使用
	```
	Promise.try(f) // 这里不需要管 f 是同步还是异步函数。
	  .then(...)
	  .catch(...)
	```
#	应用
我们可以将图片的加载写成一个Promise，一旦加载完成，Promise的状态就发生变化。
	```
	const preloadImage = function (path) {
	  return new Promise(function (resolve, reject) {
		const image = new Image();
		image.onload  = resolve;
		image.onerror = reject;
		image.src = path;
	  });
	};
	
	preloadImage
		.then((reslove) => { console.log('图片加载成功了哦！') })
		.catch((reject) => { console.log('图片加载失败了哦！') })
	```
	
>	参考文档：
	[MDN--Promiese](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
	[阮一峰--Promise](http://es6.ruanyifeng.com/#docs/promise)