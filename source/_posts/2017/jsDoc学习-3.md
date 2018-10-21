---
title: jsDoc学习-标签2
date: 2017-09-26 20:26:15
category: "代码规范"
tags: ['js', '代码规范']
copyright: true
---
###	@file
	@file标签提供文件的说明。在文件开头的JSDoc注释部分使用该标签。
	别名：@fileoverview  @overview
	例如，文件描述
	```
	/**
	 * @file 这是正则js文件
	 * @author fanerge <fanerge@example.com>
	 */
	```
###	@fires
	@fires标签标明当一个方法被调用时将触发一个指定类型的事件，使用@event 标签来描述事件的内容。
	别名：@emits
	语法：@fires <className>#[event:]<eventName>
	例如，方法将触发"drain"事件
	```
	/**
	 * Drink the milkshake.
	 * @fires Milkshake#drain
	 */
	Milkshake.prototype.drink = function() {
		// ...
	};
	```
###	@function 
	标记一个对象作为一个函数，即使它可能不会出现在解析器中。它设置doclet的@kind为'function'。
	别名：@func  @method
	语法：@function [<FunctionName>]
	例如，使用@function标记为一个函数
	```
	/** @function */
	var paginate = paginateFactory(pages);
	```
###	global
	@global标签指定一个在文档的标识是为全局性的标识。
	JSDoc忽略这个标识在源文件中的实际作用范围。这个标记是在本地所定义标识时特别有用。
	例如，文档中的内部变量作为一个全局变量
	```
	(function() {
		/** @global */
		var foo = 'hello foo';

		this.foo = foo;
	}).apply(window);
	```
###	@ignore
	@ignore标签表示在你的代码中的注释不应该出现在文档中，注释会被直接忽略。这个标签优先于所有其他标签。
	在下面的例子中，@ignore标签， Jacket 和 Jacket#color 将不会出现在文档中
	```
	/**
	 * @class
	 * @ignore
	 */
	function Jacket() {
		/** The jacket's color. */
		this.color = null;
	}
	```
###	@implements
	@implements标签指示一个标识实现一个接口。
	语法：@implements {typeExpression}
	在下面的例子中，TransparentColor类实现Color接口，并添加了TransparentColor#rgba方法。
###	@inheritdoc
	@inheritdoc标签指示该标识应继承其父类的文档。在你的JSDoc注释中的任何其它标签都将被忽略。
	下面的例子显示了一个类的描述如何从它的父类继承文档。
	```
	/**
	 * @classdesc Abstract class representing a network connection.
	 * @class
	 */
	function Connection() {}

	/**
	 * Open the connection.
	 */
	Connection.prototype.open = function() {
		// ...
	};


	/**
	 * @classdesc Class representing a socket connection.
	 * @class
	 * @augments Connection
	 */
	function Socket() {}

	/** @inheritdoc */
	Socket.prototype.open = function() {
		// ...
	};
	```
###	@inner
	使用@inner标签将标明该标识符作为它父标识符的内部成员。这意味着它可以通过 "Parent~Child" 被引用。
	在下面的例子中，我们使用@inner迫使一个命名空间的成员被描述作为内部成员（默认情况下，这是一个静态成员）。
	这意味着，foo现在有了MyNamespace~foo新名字，而不是MyNamespace.foo。
	```
	/** @namespace */
	var MyNamespace = {
		/**
		 * foo is now MyNamespace~foo rather than MyNamespace.foo.
		 * @inner
		 */
		foo: 1
	};
	```
###	@instance
	使用@instance标签标明该标识符作为它父标识符的实例成员。
	这意味着它可以通过"Parent#Child"被引用。
	例如，使用@instance确定一个实例成员
	```
	/** @namespace */
	var BaseObject = {
		/**
		 * foo is now BaseObject#foo rather than BaseObject.foo.
		 * @instance
		 */
		foo: null
	};

	/** Generates BaseObject instances. */
	function fooFactory(fooValue) {
		var props = { foo: fooValue };
		return Object.create(BaseObject, props);
	}
	```
###	@interface
	@interface标签使一个标识符作为其他标识符的一个实现接口。 例如，你的代码可能定义一个父类，它的方法和属性被去掉。
	您可以将@interface标签添加到父类，以指明子类必须实现父类的方法和属性。
	语法：@interface [<name>]
	在下面的例子中，Color函数表示其它类可以实现的接口。
	```
	/**
	 * Interface for classes that represent a color.
	 * @interface
	 */
	function Color() {}
	/**
	 * Get the color as an array of red, green, and blue values, represented as
	 * decimal numbers between 0 and 1.
	 * @returns {Array&lt;number>} An array containing the red, green, and blue values,
	 * in that order.
	 */
	Color.prototype.rgb = function() {
		throw new Error('not implemented');
	};
	```
###	@kind
	@kind标签是用来指明什么样的标识符被描述（例如，一类或模块）。标识符kind 不同于标识符type（例如，字符串或布尔）。
	语法：@kind <kindName>
		kindName取值：class constant event external file function member mixin module namespace typedef
	```
	/**
	 * A constant.
	 * @kind constant
	 */
	const asdf = 1;
	```
###	@lends
	@lends标签允许你将一个字面量对象的所有成员标记为某个标识符（类或模块）的成员，就像他们是给定名称的标识符成员。
	你可能想这样做，如果你传递一个对象字面量给一个函数，创建一个成员为对象字面量的命名类。
	语法：@lends <namepath>
	实例，@lends标签告诉JSDoc，这一对象字面量的所有成员都会被“借”给"Person"类。
	```
	/** @class */
	var Person = makeClass(
		/** @lends Person */
		{
			initialize: function(name) {
				this.name = name;
			},
			say: function(message) {
				return this.name + " says: " + message;
			}
		}
	);
	```
###	@license
	@license标签标识你的代码采用何种软件许可协议。
	语法：@license <identifier>
	例如，这是在Apache 2.0 许可下分发的模块
	```
	/**
	 * Utility functions for the foo package.
	 * @module foo/util
	 * @license Apache-2.0
	 */
	```
###	@listens
	@listens 标签指示一个标识监听指定的事件。使用@event 标签来记录事件的内容。
	语法：@listens <eventName>
	下面的示例演示了如何记录名为module:hurler~event:snowball的事件，还有一个方法命名为module:playground/monitor.reportThrowage来监听事件。
###	@member
	@member标签记录成员基本种类（kind），比如"class", "function", 或者 "constant"。
	一个成员可以任选地具有一个类型以及名称。
	别名：@var
	语法：@member [<type>] [<name>]
	例如，Data#point上使用@member：
	```
	/** @class */
	function Data() {
		/** @member {Object} */
		this.point = {};
	}
	```
###	@memberof
	@memberof标签标明成员隶属于哪一个父级标识符。
	语法：@memberof <parentNamepath>
		  @memberof! <parentNamepath>
	事实上，它就是一个全局性的函数，但同事它也是Tools命名空间的一个成员，而这才是你想描述的。
	```
	/** @namespace */
	var Tools = {};

	/** @memberof Tools */
	var hammer = function() {
	};

	Tools.hammer = hammer;
	```
###	@mixes
	@mixes标签指示当前对象混入了OtherObjectPath对象的所有成员,被混入的对象就是一个@mixin。	
	语法：@mixes <OtherObjectPath>
###	@mixin
	您可以使用@mixin标签标识该对象是一个mixin（混入），旨在表明该对象的属性和方法混入到其他对象。
	然后，可以将@mixes标签 添加到使用了该 mixin（混入）的对象上。
	语法：@mixin [<MixinName>]
###	@module
	@module可以将当前文件标注为一个模块，默认情况下文件内的所有标识符都隶属于此模块，除非文档另有说明。
	@module [[{<type>}] <moduleName>]
	下面的示例演示了在一个模块中用于标识的namepaths。第一个标识符是模块私有的，或“内部”变量 - 它只能在模块内访问。第二个标识符是由模块导出一个静态函数。
	```
	/** @module myModule */

	/** will be module:myModule~foo */
	var foo = 1;

	/** will be module:myModule.bar */
	var bar = function() {};
	```
###	@name
	@name标签强制JSDoc使用这个给定的名称，而忽略实际代码里的名称。
	这个标签最好用于"虚拟注释"，而不是在代码中随时可见的标签，如在运行时期间产生的方法。
	语法：@name <namePath>
	下面的例子演示了如何使用@name标签描述一个函数，JSDoc通常不会识别。
	```
	/**
	 * @name highlightSearchTerm
	 * @function
	 * @global
	 * @param {string} term - The search term to highlight.
	 */
	eval("window.highlightSearchTerm = function(term) {};")
	```
###	@namespace
	@namespace标签指明对象是一个命名空间。你也可以书写一个虚拟JSDoc注释，通过使用代码来定义命名空间。
	语法：@namespace [{<type>}] <SomeName>]
	例如，对象上使用 @namespace 标签：
	```
	/**
	 * My namespace.
	 * @namespace
	 */
	var MyNamespace = {
		/** documented as MyNamespace.foo */
		foo: function() {},
		/** documented as MyNamespace.bar */
		bar: 1
	};
	```
>	参考文档：
	[jsDoc文档](http://www.css88.com/doc/jsdoc/)	