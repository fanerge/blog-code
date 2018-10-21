---
title: js函数式编程-6
date: 2017-07-24 21:25:38
category: "函数式编程"
tags: '函数式编程'
---
## Applicative Functor
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### 应用 applicative functor
	```
	Container.of(2).chain(function(two) {
	  return Container.of(3).map(add(two));
	});
	```
	
### 瓶中之船	
	```
	// ap 就是这样一种函数，能够把一个 functor 的函数值应用到另一个 functor 的值上。
	Container.prototype.ap = function(other_container) {
	  return other_container.map(this.__value);
	}
	Container.of(add(2)).ap(Container.of(3)); // Container(5)

	// all together now
	Container.of(2).map(add).ap(Container.of(3)); // Container(5
	```

### 协调与激励
	```
	// 帮助函数：
	//  $ :: String -> IO DOM
	var $ = function(selector) {
	  return new IO(function(){ return document.querySelector(selector) });
	}

	//  getVal :: String -> IO String
	var getVal = compose(map(_.prop('value')), $);

	// Example:
	//  signIn :: String -> String -> Bool -> User
	var signIn = curry(function(username, password, remember_me){ /* signing in */  })

	IO.of(signIn).ap(getVal('#email')).ap(getVal('#password')).ap(IO.of(false));
	// IO({id: 3, email: "gg@allin.com"})
	```

### 定律
	```
	同一律（identity）
		A.of(id).ap(v) == v
		var v = Identity.of("Pillow Pets");
		Identity.of(id).ap(v) == v
	同态（homomorphism）
		A.of(f).ap(A.of(x)) == A.of(f(x))
		Either.of(_.toUpper).ap(Either.of("oreos")) == Either.of(_.toUpper("oreos"))
	互换（interchange）
		v.ap(A.of(x)) == A.of(function(f) { return f(x) }).ap(v)
		var v = Task.of(_.reverse);
		var x = 'Sparklehorse';
		v.ap(Task.of(x)) == Task.of(function(f) { return f(x) }).ap(v)
	组合（composition）
		A.of(compose).ap(u).ap(v).ap(w) == u.ap(v.ap(w));
		var u = IO.of(_.toUpper);
		var v = IO.of(_.concat("& beyond"));
		var w = IO.of("blood bath ");
		IO.of(_.compose).ap(u).ap(v).ap(w) == u.ap(v.ap(w))
	```
	
> [js函数式编程](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch7.html#自由定理)
































