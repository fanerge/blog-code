---
title: js函数式编程-5
date: 2017-07-24 19:41:34
category: "函数式编程"
tags: '函数式编程'
---
## Monad
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### pointed functor
	of 方法不是用来避免使用 new 关键字的，而是用来把值放到默认最小化上下文（default minimal context）中的。
>   pointed functor 是实现了 of 方法的 functor。

	```
	这里的关键是把任意值丢到容器里然后开始到处使用 map 的能力。
	IO.of("tetris").map(concat(" master")); // IO("tetris master")
	Maybe.of(1336).map(add(1)); // Maybe(1337)
	```
	
### 混合比喻
1. 例子
	```
	var fs = require('fs');

	//  readFile :: String -> IO String
	var readFile = function(filename) {
	  return new IO(function() {
		return fs.readFileSync(filename, 'utf-8');
	  });
	};

	//  print :: String -> IO String
	var print = function(x) {
	  return new IO(function() {
		console.log(x);
		return x;
	  });
	}

	//  cat :: IO (IO String)
	var cat = compose(map(print), readFile);

	cat(".git/config")
	// IO(IO("[core]\nrepositoryformatversion = 0\n"))
	```
2.	剥开洋葱
	```
	我说过 monad 像洋葱，可以使用一个叫作 join 的方法，来剥开。
	var mmo = Maybe.of(Maybe.of("nunchucks"));
	// Maybe(Maybe("nunchucks"))
	mmo.join();
	// Maybe("nunchucks")
	
	var ttt = Task.of(Task.of(Task.of("sewers")));
	// Task(Task(Task("sewers")));
	ttt.join()
	// Task(Task("sewers"))

	```
>	monad 是可以变扁（flatten）的 pointed functor。
	一个 functor，只要它定义个了一个 join 方法和一个 of 方法，并遵守一些定律，那么它就是一个 monad。

	为Maybe实现join方法
	```
	Maybe.prototype.join = function () {
		return this.isNothing() ? Maybe.of(null) : this.__value;
	};
	
	Example
	//  join :: Monad m => m (m a) -> m a
	var join = function(mma){ return mma.join(); }

	//  firstAddressStreet :: User -> Maybe Street
	var firstAddressStreet = compose(
	  join, map(safeProp('street')), join, map(safeHead), safeProp('addresses')
	);

	firstAddressStreet(
	  {addresses: [{street: {name: 'Mulburry', number: 8402}, postcode: "WC2N" }]}
	);
	// Maybe({name: 'Mulburry', number: 8402})
	```

### chain 函数
	```
	// 使用 chain封装（map的后面调用join）
	//  chain :: Monad m => (a -> m b) -> m a -> m b
	var chain = curry(function(f, m){
	  return m.map(f).join(); // 或者 compose(join, map(f))(m)
	});
	
	// map/join
	var firstAddressStreet = compose(
	  join, map(safeProp('street')), join, map(safeHead), safeProp('addresses')
	);

	// chain
	var firstAddressStreet = compose(
	  chain(safeProp('street')), chain(safeHead), safeProp('addresses')
	);
	
	Example
	Maybe.of(3).chain(function(three) {
	  return Maybe.of(2).map(add(three));
	});
	// Maybe(5);
	```
	
### 理论
	```
	 // 结合律
	compose(join, map(join)) == compose(join, join)
	```
	总结：monad 能够借给我们从盒子中取出的值，而且知道我们会在结束使用后还给它。
	monad 让我们深入到嵌套的运算当中，使我们能够在完全避免回调金字塔（pyramid of doom）情况下，为变量赋值，运行有序的作用，执行异步任务等等。





















