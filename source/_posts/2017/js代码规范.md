---
title: js代码规范
date: 2017-10-12 20:56:39
category: "代码规范"
tags: ['js', '代码规范']
copyright: true
---
Airbnb JavaScript Style Guide，这是业界中比较权威的js编码规范，先学习这个规范，后期项目配合ESLint指定良好的代码规范。
##	类型
1.	基本类型：直接存取基本类型。
	String 字符串
	Number 数值
	Boolean 布尔类型
	null
	undefined
2.	复制类型：通过引用的方式存取复杂类型。
	Object 对象
	Array 数组
	Function 函数
##	引用
1.	对不可变的引用使用 const 避免使用 var。
		const 声明的变量不可以重新赋值，而 var 可以。
2.	对可变的引用使用 let 避免使用 var。
3.	注意 let 和 const 都是块级作用域。
##	对象
1.	使用字面值创建对象。
2.	如果你的代码在浏览器环境下执行，别使用 保留字 作为键值。这样的话在 IE8 不会运行。
3.	使用同义词替换需要使用的保留字。
4.	创建有动态属性名的对象时，使用可被计算的属性名称。
	```
	function getKey(k) {
		return `a key named ${k}`;
	}
	const obj = {
		id: 5,
		name: 'San Francisco',
		[getKey('enabled')]: true,
	};
	```
5.	使用对象方法的简写。
	```
	const atom = {
	  value: 1,
	  // 方法简写
	  addValue(value) {
		return atom.value + value;
	  }
	};
	```
6.	使用对象属性值的简写。
	```
	const lukeSkywalker = 'Luke Skywalker111';

	const obj = {
		lukeSkywalker,
	};
	console.log(obj.lukeSkywalker); // 'Luke Skywalker111'
	```
7.	在对象属性声明前把简写的属性分组（也就是说把简写属性放在一起）。
##	数组
1.	使用字面值创建数组。
2.	向数组添加元素时使用 Arrary#push 替代直接赋值。	
	```
	const someStack = [];

	// bad
	someStack[someStack.length] = 'abracadabra';

	// good
	someStack.push('abracadabra');
	```	
3.	使用拓展运算符 ... 复制数组。	
	```
	let array = ['1','2'];
	console.log([...array]);
	```
4.	使用 Array#from 把一个类数组对象转换成数组。
	```
	const foo = document.querySelectorAll('.foo');
	const nodes = Array.from(foo);
	```
##	解构
1.	使用解构存取和使用多属性对象。
	```
	// good
	function getFullName(obj) {
		const { firstName, lastName } = obj;
		return `${firstName} ${lastName}`;
	}

	// best
		function getFullName({ firstName, lastName }) {
		return `${firstName} ${lastName}`;
	}
	```
2.	对数组使用解构赋值。
	```
	const arr = [1, 2, 3, 4];
	const [first, second] = arr;
	console.log(first, second); // 1, 2
	```
3.	需要回传多个值时，使用对象解构，而不是数组解构。	
	```
	function processInput(input) {
		// then a miracle occurs
		return { left, right, top, bottom };
	}

	// 调用时只选择需要的数据
	const { left, right } = processInput(input);
## 字符串
1.	字符串使用单引号 ''。 
2.	字符串超过 80 个字节应该使用字符串连接号换行。\	
3.	过度使用字串连接符号可能会对性能造成影响。可换用 + 	
4.	程序化生成字符串时，使用模板字符串代替字符串连接。
##	函数
1.	使用函数声明代替函数表达式。	
2.	立即调用的函数表达式 (IIFE)	
	```
	(() => {
	  console.log('Welcome to the Internet. Please follow me.');
	})();
	```
3.	永远不要在一个非函数代码块（if、while 等）中声明一个函数，把那个函数赋给一个变量。
	浏览器允许你这么做，但它们的解析表现不一致。
	可以这样
	```
	let test;
	if (currentUser) {
	  test = () => {
		console.log('Yup.');
	  };
	}
	```
4.	永远不要把参数命名为 arguments。这将取代原来函数作用域内的 arguments 对象。
5.	不要使用 arguments。可以选择 rest 语法 ... 替代。
6.	直接给函数的参数指定默认值，不要使用一个变化的函数参数。
	```
	function handleThings(opts = {}) {
	  // ...
	}
	```
7.	直接给函数参数赋值时需要避免副作用。
##	箭头函数
1.	当你必须使用函数表达式（或传递一个匿名函数）时，使用箭头函数符号。
	```
	[1, 2, 3].map((x) => {
		return x * x;
	});
	```
2.	如果一个函数适合用一行写出并且只有一个参数，那就把花括号、圆括号和 return 都省略掉。
	如果不是，那就不要省略。
##	Classes 和 Constructors
1.	总是使用 class。避免直接操作 prototype 。
	```
	// good
	class Queue {
		constructor(contents = []) {
		  this._queue = [...contents];
		}
		pop() {
		  const value = this._queue[0];
		  this._queue.splice(0, 1);
		  return value;
		}
	}
	var demo = new Queue([1, 2, 3]);
	console.log(demo.pop()); // 1
	```
2.	使用 extends 继承。
	```
	class PeekableQueue extends Queue {
		constructor (args) {
			super(args);
		}
		peek() {
			return this._queue[0];
		}
	}
	```
3.	方法可以返回 this 来帮助链式调用。
	```
	class Jedi {
		jump() {
			this.jumping = true;
			return this;
		}

		setHeight(height) {
			this.height = height;
			return this;
		}
	}

	const luke = new Jedi();
	luke.jump()
		.setHeight(20);
	```
4.	可以写一个自定义的 toString() 方法，但要确保它能正常运行并且不会引起副作用。
	```
	class Jedi {
		constructor(options = {}) {
			this.name = options.name || 'no name';
		}

		getName() {
			return this.name;
		}

		toString() {
			return `Jedi - ${this.getName()}`;
		}
	}
	```
##	模块
1.	总是使用模组 (import/export) 而不是其他非标准模块系统。
	```
	// ok
	import AirbnbStyleGuide from './AirbnbStyleGuide';
	export default AirbnbStyleGuide.es6;

	// best
	import { es6 } from './AirbnbStyleGuide';
	export default es6;
	```
2.	不要使用通配符 import。
	```
	// bad
	import * as AirbnbStyleGuide from './AirbnbStyleGuide';

	// good
	import AirbnbStyleGuide from './AirbnbStyleGuide';
	```
3.	不要从 import 中直接 export。
	```
	import { es6 } from './AirbnbStyleGuide';
	export default es6;
	```
##	Iterators & Generators
1.	不要使用 iterators。使用高阶函数例如 map() 和 reduce() 替代 for-of。
	```
	const numbers = [1, 2, 3, 4, 5];

	// good
	let sum = 0;
	numbers.forEach((num) => sum += num);

	// best (use the functional force)
	const sum = numbers.reduce((total, num) => total + num, 0);
	```
##	属性
1.	普通属性使用 . 来访问对象的属性。	
2.	当通过变量访问属性时使用中括号 []。
##	变量
1.	一直使用 const 来声明变量，如果不这样做就会产生全局变量。我们需要避免全局命名空间的污染。
2.	 单独声明每一个变量。
	```
	const items = getItems();
	const goSportsTeam = true;
	const dragonball = 'z';
	```
3.	将所有的 const 和 let 分组。
4.	在你需要的地方给变量赋值，但请把它们放在一个合理的位置。
## 提升
1.	var 声明会被提升至该作用域的顶部，但它们赋值不会提升。
	let 和 const 被赋予了一种称为「暂时性死区（Temporal Dead Zones, TDZ）」的概念。
2.	匿名函数表达式的变量名会被提升，但函数内容并不会。
3.	命名的函数表达式的变量名会被提升，但函数名和函数函数内容并不会。
4.	函数声明的名称和函数体都会被提升。
##	比较运算符 & 等号
1.	优先使用 === 和 !== 而不是 == 和 !=。
2.	条件表达式例如 if 语句通过抽象方法 ToBoolean 强制计算它们的表达式并且总是遵守下面的规则
	对象 被计算为 true
	Undefined 被计算为 false
	Null 被计算为 false
	布尔值 被计算为 布尔的值
	数字 如果是 +0、-0、或 NaN 被计算为 false, 否则为 true
	字符串 如果是空字符串 '' 被计算为 false，否则为 true
3.	使用简写。
##	代码块
1.	使用大括号包裹所有的多行代码块。
2.	如果通过 if 和 else 使用多行代码块，把 else 放在 if 代码块关闭括号的同一行。
##	注释
1.	使用 /** ... */ 作为多行注释。包含描述、指定所有参数和返回值的类型和值。
	配合 JSDoc 完美
2.	使用 // 作为单行注释。在评论对象上面另起一行使用单行注释。
	在注释前插入空行。
3.	给注释增加 FIXME 或 TODO 的前缀可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。
	这将有别于常见的注释，因为它们是可操作的。
	使用FIXME -- need to figure this out 或者 TODO -- need to implement。
4.	使用 // FIXME: 标注问题。
	```
	class Calculator {
		constructor() {
			// FIXME: shouldn't use a global here
			total = 0;
		}
	}
	```
5.	使用 // TODO: 标注问题的解决方式。
	```
	class Calculator {
	  constructor() {
		// TODO: total should be configurable by an options param
		this.total = 0;
	  }
	}
	```
##	空白
1.	使用 2 个空格作为缩进。
2.	在花括号前放一个空格。
3.	在控制语句（if、while 等）的小括号前放一个空格。
	在函数调用及声明中，不在函数的参数列表前加空格。
4.	使用空格把运算符隔开。
5.	在文件末尾插入一个空行。
6.	在使用长方法链时进行缩进。使用前面的点 . 强调这是方法调用而不是新语句。
7.	在块末和新语句前插入空行。
##	逗号
1.	行首不要加逗号。
2.	增加结尾的逗号: 需要。
##	分号
1.	每个语句都使用分号。
2.	IIFE 函数前添加分号。
	```
	// good (防止函数在两个 IIFE 合并时被当成一个参数)
	;(() => {
	  const name = 'Skywalker';
	  return name;
	})();
	```
##	类型转换
1.	在语句开始时执行类型转换。
2.	显式转换字符串。
	const reviewScore = 9;
	const totalScore = String(reviewScore);
3.	对数字使用 parseInt 转换，并带上类型转换的基数。
	```
	const val = Number(inputValue);

	const val = parseInt(inputValue, 10);
	```
4.	如果因为某些原因 parseInt 成为你所做的事的瓶颈而需要使用位操作解决性能问题时，留个注释说清楚原因和你的目的。
5.	小心使用位操作运算符。数字会被当成 64 位值，但是位操作运算符总是返回 32 位的整数。
	位操作处理大于 32 位的整数值时还会导致意料之外的行为。
6.	转换为buer。
	```
	const age = 0;
	const hasAge = Boolean(age);
	const hasAge = !!age;
	```
##	命名规则
1.	避免单字母命名。命名应具备描述性。
2.	使用驼峰式命名对象、函数和实例。
3.	使用帕斯卡（大驼峰）式命名构造函数或类。
4.	使用下划线 _ 开头命名私有属性。
5.	别保存 this 的引用。使用箭头函数或 Function#bind。
	```
	function foo() {
	  return () => {
		console.log(this);
	  };
	}
	
	function foo() {
		return function() {
			console.log(this);
		}.bind(this)
	}
	```
6.	如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。
	```
	// file contents
	class CheckBox {
	  // ...
	}
	export default CheckBox;

	import CheckBox from './CheckBox';
	```
7.	当你导出默认的函数时使用驼峰式命名。
	你的文件名必须和函数名完全保持一致。
8.	当你导出单例、函数库、空对象时使用帕斯卡式命名。
	```
	const AirbnbStyleGuide = {
	  es6: {
	  }
	};

	export default AirbnbStyleGuide;
	```
##	存取器
1.	属性的存取函数不是必须的。	
2.	如果你需要存取函数时使用 getVal() 和 setVal('hello')。
	```
	dragon.getAge();
	dragon.setAge(25);
	```
3.	如果属性是布尔值，使用 isVal() 或 hasVal()。
4.	创建 get() 和 set() 函数是可以的，但要保持一致。
	```
	class Jedi {
	  constructor(options = {}) {
		const lightsaber = options.lightsaber || 'blue';
		this.set('lightsaber', lightsaber);
	  }

	  set(key, val) {
		this[key] = val;
	  }

	  get(key) {
		return this[key];
	  }
	}
	```
##	事件
1.	当给事件附加数据时（无论是 DOM 事件还是私有事件），传入一个哈希而不是原始值。
	这样可以让后面的贡献者增加更多数据到事件数据而无需找出并更新事件的每一个处理器。
	```
	$(this).trigger('listingUpdated', { listingId : listing.id });

	$(this).on('listingUpdated', function(e, data) {
	  // do something with data.listingId
	});
	```
##	Jquery
1.	使用 $ 作为存储 jQuery 对象的变量名前缀。
	const $sidebar = $('.sidebar');
2.	缓存 jQuery 查询。
	```
	function setSidebar() {
	  const $sidebar = $('.sidebar');
	  $sidebar.hide();

	  $sidebar.css({
		'background-color': 'pink'
	  });
	}
	```
3.	对 DOM 查询使用层叠 $('.sidebar ul') 或 父元素 > 子元素 $('.sidebar > ul')。
4.	对有作用域的 jQuery 对象查询使用 find。
>	参考文档：
	[Airbnb JavaScript 代码规范（ES6）](https://www.kancloud.cn/kancloud/javascript-style-guide/43153)
	[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript#airbnb-javascript-style-guide-)













































