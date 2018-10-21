---
title: js函数式编程-4
date: 2017-07-22 21:15:25
category: "函数式编程"
tags: '函数式编程'
---
## 特百惠
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### 强大的容器
	函数式的程序：即通过管道把数据在一系列纯函数间传递的程序。
	```
1.	// 先创建一个容器（container），这个容器必须能够装载任意类型的值，一个存放宝贵的数据的特殊盒子。
	var Container = function (x) {
		this._value = x;
	};	
	// 作用容器的构造器（constructor）
	Container.of = function (x) {
		return new Container(x);
	};
	// 测试容器
	var test = Container.of(Container.of({name: 'fanerge'}))	
	console.log(test)
	```
2. 	第一个functor
	```
	// 操作容器中的值
	// (a -> b) -> Container a -> Container b
	Container.prototype.map = function (f) {
		return Container.of(f(this._value))
	};	
	// 使用map方法
	Container.of(3).map(function (two) {
		return two + 2;
	});
	// Container(5)
	Container.of('fanerge').map(function (str) { 
		return str.toUpperCase()
	});
	// Container('FANERGE')
	// 因为我们能够在不离开 Container 的情况下操作容器里面的值。这是非常了不起的一件事情。Container 里的值传递给 map 函数之后，就可以任我们操作；操作结束后，为了防止意外再把它放回它所属的 Container。这样做的结果是，我们能连续地调用 map，运行任何我们想运行的函数。
	```
	**functor 是实现了 map 函数并遵守一些特定规则的容器类型。**
	把值装进一个容器，而且只能使用 map 来处理它，让容器自己去运用函数能给我们带来什么好处？答案是抽象，对于函数运用的抽象。当 map 一个函数的时候，我们请求容器来运行这个函数。不夸张地讲，这是一种十分强大的理念。
3.	薛定谔的Maybe
	```
	var Maybe = function (x) {
		this._value = x;
	};

	Maybe.of = function (x) {
		return new Maybe(x);
	};

	Maybe.prototype.isNoting = function () {
		return (this._value == null || this._value === undefined)
	};

	Maybe.prototype.map = function (f) {
		return this.isNoting() ? Maybe.of(null) : Maybe.of(f(this._value));
	};

	Maybe.of('Malkovich Malkovich').map(_.match(/a/ig)) // Maybe(['a', 'a']
	// map 完全有能力以 curry 函数的方式来“代理”任何 functor
	var map = curry(function (f, any_functor_at_all) {
		return any_functor_at_all.map(f);
	});
	// safeHead :: [a] -> Maybe(a)
	var safeHead = function (xs) {
		return Maybe.of(xs[0]);
	};
	var streetName = _.compose(map(_.prop('street')), safeHead, _.prop('address'));
	streetName({addresses: [{street: "Shady Ln.", number: 4201}]}); // Maybe("Shady Ln.")
	```
	*购物的例子*
	```
	// withdraw :: Number -> Account -> Maybe(Account)
	var withdraw = curry(function (amount, account) {
		return account.balance >= amount ?
			Maybe.of({balance: acount.balance - amount}) :
			Maybe.of(null);
	})
	// finishTransaction :: Account -> String
	var finishTransaction = compose(remainingBalance, updateLedger); //// <- 假定这两个函数已经在别处定义好了
	// getTwenty :: Account -> Maybe(String)
	var getTwenty = compose(map(finishTransaction), withdraw(20));
	getTwenty({balance: 200.00}); // Maybe("Your balance is $180.00")
	getTwenty({ balance: 10.00}); // Maybe(null)	
	```
	```
	// maybe :: b -> (a -> b) -> Maybe a -> b	
	var may = curry(function (x, f, m) {
		return m.isNoting() ? x : f(m._value);
	});
	// getTwenty :: Account -> String
	var getTwenty = compose(
		maybe("You're broke!", finishTransaction), withdraw(20)
	);	
	getTwenty({ balance: 200.00}); // "Your balance is $180.00"
	getTwenty({ balance: 10.00}); // "You're broke!"
	```
	
### "纯"错误处理
	```
	Left 和 Right 是我们称之为 Either 的抽象类型的两个子类。
	var Left = function(x) {
	  this.__value = x;
	}
	Left.of = function(x) {
	  return new Left(x);
	}
	Left.prototype.map = function(f) {
	  return this;
	}

	var Right = function(x) {
	  this.__value = x;
	}
	Right.of = function(x) {
	  return new Right(x);
	}
	Right.prototype.map = function(f) {
	  return Right.of(f(this.__value));
	}
	// 就像 Maybe 可以有个 maybe 一样，Either 也可以有一个 either。两者的用法类似，但 either 接受两个函数（而不是一个）和一个静态值为参数。
	//  either :: (a -> c) -> (b -> c) -> Either a b -> c
	var either = curry(function(f, g, e) {
	  switch(e.constructor) {
		case Left: return f(e.__value);
		case Right: return g(e.__value);
	  }
	});

	//  zoltar :: User -> _
	var zoltar = compose(console.log, either(id, fortune), getAge(moment()));

	zoltar({birthdate: '2005-12-12'});
	// "If you survive, you will be 10"
	// undefined

	zoltar({birthdate: 'balloons!'});
	// "Birth date could not be parsed"
	// undefined
	```
	**取到容器里的东西**
	```
	var IO = function (f) {
		this.__value = f;
	};
	IO.of = function (x) {
		return new IO(function () {
			return x;
		});
	};
	IO.prototype.map = function (f) {
		return new IO(_.compose(f, this.__value));
	};
	// io_window_ :: IO Window
	var io_window = new IO(function () { return window;})
	io_window.map(function (win) { return win.innerWidth }) // IO(1430)
	io_window.map(_.prop('location')).map(_.prop('href')).map(split('/')); // IO(["http:", "", "localhost:8000", "blog", "posts"])
	// $ :: String -> IO [DOM]
	var $ = function(selector) {
	  return new IO(function(){ return document.querySelectorAll(selector); });
	}
	$('#myDiv').map(head).map(function(div){ return div.innerHTML; }); // IO('I am some inner html')
	```
	**解析url参数**
	```
	纯代码库: lib/params.js
	var url = new IO(function () { return window.location.href; });
	// toPairs = String -> [[String]]
	var toPairs = compose(map(split('='), split('&'));
	// params :: String -> [[String]]
	var params = conmpose(toPairs, last, split('?'));
	// findParam = String -> Io Maybe [String]
	var findParam = function (key) {
		return map(compose(Maybe.of, filter(compose(eq(key), head)), params), url);
	};
	// 调用 __value() 来运行它！
	findParam('searchTerm').__value();
	// Maybe(['searchTerm', 'wafflehouse'])
	```

### 异步任务
	```
	// Postgres.connect :: Url -> IO DbConnection
	// runQuery :: DbConnection -> ResultSet
	// readFile :: String -> Task Error String
	// Pure application
	//=====================
	//  dbUrl :: Config -> Either Error Url
	var dbUrl = function(c) {
	  return (c.uname && c.pass && c.host && c.db)
		? Right.of("db:pg://"+c.uname+":"+c.pass+"@"+c.host+"5432/"+c.db)
		: Left.of(Error("Invalid config!"));
	}
	//  connectDb :: Config -> Either Error (IO DbConnection)
	var connectDb = compose(map(Postgres.connect), dbUrl);
	//  getConfig :: Filename -> Task Error (Either Error (IO DbConnection))
	var getConfig = compose(map(compose(connectDB, JSON.parse)), readFile);
	// Impure calling code
	//=====================
	getConfig("db.json").fork(
	  logErr("couldn't read file"), either(console.log, map(runQuery))
	);
	```
### 一些理论
	```
	// identity
	map(id) === id;
	// composition
	compose(map(f), map(g)) === map(compose(f, g));
	```
参考：
> [js函数式编程](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch7.html#自由定理)	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
参考：
> [js函数式编程](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch7.html#自由定理)