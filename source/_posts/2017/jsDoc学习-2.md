---
title: jsDoc学习-标签1
date: 2017-09-25 21:03:22
category: "代码规范"
tags: ['js', '代码规范']
copyright: true
---
块标签（Block Tags）
###	@abstract
	该成员（一般指父类的方法）必须在继承的子类中实现（或重写）。
	别名：@virtual
	```
	/**
	 * 奶制品
	 * @constructor
	 */
	function DairyProduct () {}
	/**
	 * 检查奶制品的状态
	 * @abstract
	 * @return {boolean}
	 */
	DairyProduct.prototype.isSolid = function () {
	  throw new Error('must be implemented by subclass!');
	};
	
	/**
	 * 牛奶
	 * @constructor
	 */
	function Milk () {}
	Milk.prototype = new DairyProduct();
	// 这里重写了父类的isSolid方法
	Milk.prototype.isSolid = function () {
		return false;
	};
	var dd = new Milk();
	console.log(dd.isSolid()); // false
	```
###	@access
	指定该成员的访问级别（私有 private，公共 public，或保护 protected）
	语法：@access <private|protected|public>
		@access private 等价于 @private;
		@access protected 等价于 @protected;
		@access public 等价于 @public;
	```
	/**
	 * @constructor
	 */
	function Thingy () {
		/** @access private */
		var foo = 0;
		/** @access protected */
		this._bar = 1;
		/** @access public */
		this.pez = 2;
	}
	OR
	/**
	 * @constructor
	 */
	function Thingy () {
		/** @private */
		var foo = 0;
		/** @protected */
		this._bar = 1;
		/** @public */
		this.pez = 2;
	}
	```
###	alias
	标记成员有一个别名
	语法：@alias <aliasNamepath>
	例如，匿名的构造函数使用@alias。
	```
	Klass('trackr.CookieManager',
		/**
		 * 匿名函数
		 * @alias trackr.CookieManager
		 * @param kv
		 */
		function (kv) {
			/** trackr.CookieManager＃value */
			this.value = kv;
		}
	)
	```
	您也可以在一个立即调用的函数表达式（IIFE）中创建的成员中使用@alias标签。
	@alias标签告诉JSDoc，这些成员都暴露在IIFE作用域之外的。
	例如，命名空间的静态方法使用@alias。
	```
	/** @namespace */
	var Apple = {};
	(function (ns) {
		/**
		 * @namespace
		 * @alias Apple.Core
		 */
		var core = {};
		/** Documented as Apple.Core.seed */
		core.speed = function () {};
		ns.Core = core;
	})(Apple);
	```
	对于那些对象字面量中定义的成员，可以使用@alias标签替代的@lends标记。
	例如，对象常量使用@alias。
	```
	// Documenting objectA with @alias
	var objectA = (function() {
		/**
		 * Documented as objectA
		 * @alias objectA
		 * @namespace
		 */
		 /** @lends objectA */
		var x = {
			/**
			 * Documented as objectA.myProperty
			 * @member
			 */
			myProperty: 'foo'
		};

		return x;
	})();
	```
###	@auguments
	@augments or@extends标签指明标识符继承自哪个父类，后面需要加父类名。
	你可以使用这个标签来记录基于类和并基于原型的继承。
	别名：@extends
	语法：@auguments <namepath>
	```
	/**
	 * 动物
	 * @constructor
	 */
	function Animal () {
		this.alive = true;
	}

	/**
	 * 鸭子
	 * @constructor
	 * @auguments Animal
	 */
	function Duck () {}
	Duck.prototype = new Animal();
	Duck.prototype.speak = function () {
		if (this.alive) {
			alert('Quack!');
		}
	};
	var d = new Duck();
	d.speak();
	```
###	@author
	@author标签标识一个项目的作者。
	语法：@author <name> [<emailAddress>]
	```
	/**
	* @author fanerge <fanerge@qq.com>
	*/
	function MyClass () {};
	```
###	@borrows
	@borrows标签允许您将另一个标识符的描述添加到你的当前描述。
	语法：@borrows <that namepath> as <this namepath>
	在这个例子中，"trstr"函数存在文档，但"util.trim"只是使用不同的名称引用相同的功能。
	例如，复制trstr的文档描述给util.trim。
	```
	/**
	 * @namespace
	 * @borrows trstr as trim
	 */
	var util = {
		trim: trstr
	};

	/**
	 * Remove whitespace from around a string.
	 * @param {string} str
	 */
	function trstr(str) {
	}
	```
###	@callback
	描述一个回调函数。@Callback标签提供回调函数（可传递给其他函数）的描述，包括回调的参数和返回值。
	你可以包涵任何一个你能提供给@method标签。
	语法：@callback <namepath>
	例如,描述一个指定类回调。
	```
	/**
	 * @class
	 */
	function Requester() {}

	/**
	 * Send a request.
	 * @param {Requester~requestCallback} cb - The callback that handles the response.
	 */
	Requester.prototype.send = function(cb) {
		// code
	};

	/**
	 * This callback is displayed as part of the Requester class.
	 * @callback Requester~requestCallback
	 * @param {number} responseCode
	 * @param {string} responseMessage
	 */
	```
###	@class
	@class标签标明函数是一个构造器函数，意味着需要使用 new 关键字来返回一个实例，即使用 new 关键字实例化。
	别名：@constructor
	语法：@class [<type> <name>]
	例如，一个函数构建一个Person实例
	```
	/**
	 * 创建一个人
	 * @class
	 */
	function Person () {}
	var p = new Person();
	```
###	@classdesc
	@classdesc标签用于为类提供一个描述，这样和构造函数的描述区分开来。
	@classdesc标签应该与 @class (或 @constructor)标签结合使用。
	语法：@classdesc <some description>
	如下所示，一个类有两个添加描述的地方，一个适用于函数本身，而另一个一般适用于类。
	```
	/**
	 * This is a description of the MyClass constructor function.
	 * @class
	 * @classdesc This is a description of the MyClass class.
	 */
	function MyClass() {
	}
	```
###	@constant
	@constant 标签指明这个对象是一个常量。
	语法：@constant [<type> <name>]	
	例如，一个字符串常量表示红色。
	```
	/** @constant
		@type {string}
		@default
	*/
	const RED = 'FF0000';
	```
###	@constructs
	当使用对象字面量形式定义类（例如使用@lends标签）时，可使用@constructs标签标明这个函数用来作为类的构造实例。
	语法：＠constructs [name]
	例如， @constructs 和 @lends 结合使用
	```	
	var Person = makeClass(
    /** @lends Person.prototype */
    {
        /**
         * @constructs
         * @param name
         */
        initialize: function (name) {
            this.name = name;
        },
        /**
         * @param msg
         * @returns {*}
         */
        say: function (msg) {
            return `${this.name}says:${msg}`;
        }
    });	
	```
###	@copyright
	@copyright标签是用来描述一个文件的版权信息。一般和@file 标签结合使用。
	语法：@copyright <some copyright text>
	```
	/**
	* @file this is javascript
	* @copyright fanerge 2017
	*/
	```
###	@default
	@default标签可以让你记录标识的赋值。
	可以在标签后面跟上他的值，或者当值是一个唯一被分配的简单值(可以是：一个字符串，数字，布尔值或null)的时候，你可以让JSDoc从源代码中获取值，自动记录 。
	别名：defaultvalue
	语法：@default [<some value>]
	在本实例中,一个常量被记录。该常数的值为0xff0000。通过添加@default标签，这个值将自动添加到文档。
	```
	/**
	* @constant
	* @default
	*/
	const RED = '0xff0000';
	```
###	@deprecated
	@deprecated 标签指明一个标识在你代码中已经被弃用。
	语法：@deprecated [<some text>]
	例如，描述一个old函数从2.0版本开始已经被弃用
	```
	/**
	* @deprecated since version 2.0
	*/
	function old () {}
	```
###	@description
	@description标签允许您提供一般描述。
	该说明可能包括HTML标签。如果Markdown 插件启用的话，它也可包括Markdown格式。
	别名：@desc
	语法：@description <some description>
	例如，不用@description标签描述一个标识
	```
	/**
	 * Add two numbers.
	 * @param {number} a
	 * @param {number} b
	 * @returns {number}
	 */
	function add(a, b) {
		return a + b;
	}
	```
	例如，用@description标签描述一个标识
	```
	/**
	 * @param {number} a
	 * @param {number} b
	 * @returns {number}
	 * @description Add two numbers.
	 */
	function add(a, b) {
		return a + b;
	}
	```
###	@enum
	@enum标签描述一个静态属性值的全部相同的集合。
	枚举类似一个属性的集合，除了枚举自己的描述注释之外，属性都记录在容器内部的注释中。
	通常这种标签是与@ReadOnly结合使用，作为一个枚举通常表示常量的集合。
	语法：@enum [<type>]
	例如，一个数字枚举，表示的3种状态
	```
	/**
	 * Enum for tri-state values.
	 * @readonly
	 * @enum {number}
	 */
	var triState = {
		/** The true value */
		TRUE: 1,
		FALSE: -1,
		/** @type {boolean} */
		MAYBE: true
	};
	```
###	@event
	描述一个事件。@event标签允许您描述一个可触发的事件，一个典型的事件是由对象定义的一组属性来表示。
	标签来定义事件的具体类型，您可以使用@fires标记，以表明这个种方法可以触发该事件。
	你还可以使用@listens标签，以指示表明用这个表示来侦听该事件。	
	语法：@event <className>#[event:]<eventName>
	例如，描述一个作为事件的行数
	```
	/**
	 * Throw a snowball.
	 *
	 * @fires Hurl#snowball
	 */
	Hurl.prototype.snowball = function() {
		/**
		 * Snowball event.
		 *
		 * @event Hurl#snowball
		 * @type {object}
		 * @property {boolean} isPacked - Indicates whether the snowball is tightly packed.
		 */
		this.emit('snowball', {
			isPacked: this._snowball.isPacked
		});
	};
	```
###	@example
	提供一个如何使用描述项的例子。跟随此标签的文字将显示为高亮代码。
	```
	/**
	 * Solves equations of the form a * x = b
	 * @example
	 * // returns 2
	 * globalNS.method1(5, 10);
	 * @example
	 * // returns 3
	 * globalNS.method(5, 15);
	 * @returns {Number} Returns the value of x for the equation.
	 */
	globalNS.method1 = function (a, b) {
		return b / a;
	};
	```
###	@exports 
	@exports标签描述由JavaScript模块的exports或module.exports属性导出的任何内容。
	语法：@exports <moduleName>
	```
	/**
	 * A module that says hello!
	 * @module hello/world
	 */

	/** Say hello. */
	exports.sayHello = function() {
		return 'Hello world';
	};
	```
###	@external
	@external标签用来标识一个在当前包外部定义的类，命名空间，或模块。
	通过使用这个标签，你可以描述你的包的外部标识的扩展，或者您也可以提供关于 外部标识的相关信息给你的包的使用者。
	你也可以在任何其他JSDoc标签中引用外部标识的namepath（名称路径）。
	外部标识引用的路径名 始终需要使用external:前缀：（例如{@link external:Foo}或@augments external:Foo）。 但是，你可以省略@external标记的这个前缀。
	别名：@host
	下面的示例演示如何描述内置的String对象作为external，新的实例方法external:String#rot13
	```
	/**
	 * The built in string object.
	 * @external String
	 * @see {@link https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String|String}
	 */

	/**
	 * Create a ROT13-encoded version of the string. Added by the `foo` package.
	 * @function external:String#rot13
	 * @example
	 * var greeting = new String('hello world');
	 * console.log( greeting.rot13() ); // uryyb jbeyq
	 */
	```
>	参考文档：
	[jsDoc文档](http://www.css88.com/doc/jsdoc/)	