---
title: jsDoc学习-标签3
date: 2017-09-26 21:28:50
category: "代码规范"
tags: ['js', '代码规范']
copyright: true
---
###	@override
	@override标签指明一个标识符覆盖其父类同名的标识符。
	下面的例子说明一个方法如何重写父类的方法。
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

	/**
	 * Open the socket.
	 * @override
	 */
	Socket.prototype.open = function() {
		// ...
	};
	```
###	@param
	@param标签提供了对某个函数的参数的各项说明，包括参数名、参数数据类型、描述等。
	别名：@arg @argument
	下面的示例演示如何在 @param标签中包含名称，类型，和说明。
	```
	/**
	 * @param somebody
	 */
	function sayHello(somebody) {
		alert('Hello ' + somebody);
	}
	```
###	@private
	@private标签标记标识符为私有，或者不做一般用途使用。
	私有成员不会在生成文档中输出任何内容，除非JSDoc使用 -p/--private 命令行选项运行。
	语法：@private
	在下面的例子中，Documents和Documents.Newspaper会被输出到生成的文档中，但是Documents.Diary不会。
	```
	/** @namespace */
	var Documents = {
		/**
		 * An ordinary newspaper.
		 */
		Newspaper: 1,
		/**
		 * My diary.
		 * @private
		 */
		Diary: 2
	};
	```
###	@property
	@property标签很容易描述类，命名空间或其它对象的静态属性列表。
	别名：@prop
	例如，描述命名空间的默认属性及嵌套属性：
	```
	/**
	 * @namespace
	 * @property {object}  defaults               - The default values for parties.
	 * @property {number}  defaults.players       - The default number of players.
	 * @property {string}  defaults.level         - The default level for the party.
	 * @property {object}  defaults.treasure      - The default treasure.
	 * @property {number}  defaults.treasure.gold - How much gold the party starts with.
	 */
	var config = {
		defaults: {
			players: 1,
			level:   'beginner',
			treasure: {
				gold: 0
			}
		}
	};
	```
###	@protected
	@protected标签标记标识符为受保护的，通常情况下，受保护的成员只能在被继承的子类中或在模块内部可以访问。
	语法：@protected [{typeExpression}]
	在下面的例子中，该实例成员Thingy#_bar会被导出到生成的文档中，但使用注释说明它是被保护的。
	```
	/** @constructor */
	function Thingy() {
		/** @protected */
		this._bar = 1;
	}
	```
###	@public
	@public标签标记标识符为公开的。	
	```
	/**
	 * The Thingy class is available to all.
	 * @public
	 * @class
	 */
	function Thingy() {
		/**
		 * The Thingy~foo member. Note that 'foo' is still an inner member
		 * of 'Thingy', in spite of the @public tag.
		 * @public
		 */
		var foo = 0;
	}
	```
###	@readonly
	标记一个标识符为只读。jsdoc不会检查某个代码是否真是只读的，只要标上@readonly，在文档中就体现为只读的。
	例如，给getter标记为只读
	```
	/**
	 * Options for ordering a delicious slice of pie.
	 * @namespace
	 */
	var pieOptions = {
		/**
		 * A la mode.
		 * @readonly
		 */
		get aLaMode() {
			return this.plain + ' with ice cream';
		}
	};
	```
###	@requires
	@requires标签可以记录一个模块需要的依赖项。一个JSDoc注释块可以有多个@require标签。
	模块名可以被指定为 "moduleName" 或者 "module:moduleName";这两种形式将被解析为模块。
	语法：@requires <someModuleName>
	例如，使用@requires 标签
	```
	/**
	 * This class requires the modules {@link module:xyzcorp/helper} and
	 * {@link module:xyzcorp/helper.ShinyWidget#polish}.
	 * @class
	 * @requires module:xyzcorp/helper
	 * @requires xyzcorp/helper.ShinyWidget#polish
	 */
	function Widgetizer() {}
	```
###	@returns
	@returns 标签描述一个函数的返回值。语法和@param类似。
	别名：@return
	返回值的类型
	```
	/**
	 * Returns the sum of a and b
	 * @param {Number} a
	 * @param {Number} b
	 * @returns {Number}
	 */
	function sum(a, b) {
		return a + b;
	}
	```
###	@see
	@see标签表示可以参考另一个标识符的说明文档，或者一个外部资源。
	语法：@see <namepath>
		  @see <text>
	```
	/**
	 * Both of these will link to the bar function.
	 * @see {@link bar}
	 * @see bar
	 */
	function foo() {}
	```
###	@since
	@since标签标明一个类，方法，或其它标识符是在哪个特定版本开始添加进来的。
	语法：@since <versionDescription>
	例如，使用@since标签：
	```
	/**
	 * Provides access to user information.
	 * @since 1.0.1
	 */
	function UserRecord() {}
	```
###	@static
	@static标签标明一个在父类中的标识符不需实例即可使用。
	例如，在一个虚拟注释中使用@static	
	```
	/** @namespace MyNamespace */
	/**
	 * @function myFunction
	 * @memberof MyNamespace
	 * @static
	 */
	```
###	@summary
	@summary标签是完整描述的一个简写版本。它可以被添加到任何的doclet。
	语法：@summary Summary goes here.
	```
	/**
	 * A very long, verbose, wordy, long-winded, tedious, verbacious, tautological,
	 * profuse, expansive, enthusiastic, redundant, flowery, eloquent, articulate,
	 * loquacious, garrulous, chatty, extended, babbling description.
	 * @summary A concise summary.
	 */
	function bloviate() {}
	```
###	@this
	@this标签指明this关键字的指向。
	语法：@this <namePath>
	在下面的例子中，@this标签迫使"this.name"被描述为"Greeter#name"，而不是全局变量"name"。
	```
	/** @constructor */
	function Greeter(name) {
		setName.apply(this, name);
	}

	/** @this Greeter */
	function setName(name) {
		/** document me */
		this.name = name;
	}
	```
###	@throws
	@throws标签可以让你描述函数可能会抛出的错误。在一个JSDoc注释块中您可以包含多个@throws标签。
	语法：@throws free-form description
		  @throws {<type>}
		  @throws {<type>} free-form description
	例如，在type中使用@throws标签:
	```
	/**
	 * @throws {InvalidArgumentException}
	 */
	function foo(x) {}		
	```
###	@todo
	@todo标签可以让你记录要完成的任务。在一个JSDoc注释块中您可以包含多个@todo标签。
	语法：@todo text describing thing to do.
	```
	/**
	 * @todo Write the documentation.
	 * @todo Implement this function.
	 */
	function foo() {
		// write me
	}
	```
###	@tutorial
	@tutorial 标签插入一个指向向导教程的链接，作为文档的一部分。
	语法：@tutorial <tutorialID>
	在下面的例子中，MyClass的文档将链接到tutorial-1 和 tutorial-2标识符的教程。
	```
	/**
	 * Description
	 * @class
	 * @tutorial tutorial-1
	 * @tutorial tutorial-2
	 */
	function MyClass() {}
	```
###	@type
	@type标签允许你提供一个表达式，用于标识一个标识符可能包含的值的类型，或由函数返回值的类型。
	语法：@type {typeName1 | typeName2}
	```
	/** @type {(string|Array.<string>)} */
	var foo;
	/** @type {number} */
	var bar = 1;
	```
###	@typedef
	@typedef标签在描述自定义类型时是很有用的，特别是如果你要反复引用它们的时候。
	语法：@typedef [<type>] <namepath>
	这个例子定义了一个联合类型的参数，表示可以包含数字或字符串。
	```
	/**
	 * A number, or a string containing a number.
	 * @typedef {(number|string)} NumberLike
	 */
	```
###	@variation
	描述: 区分具有相同名称的不同的对象。
	语法：@variation <variationNumber>
###	@version
	@version标签后面的文本将被用于表示该项的版本。	
	```
	/**
	 * @version 1.2.3
	 * @tutorial solver
	 */
	function solver(a, b) {
		return b / a;
	}
	```
###	内联标签（inline Tags）
	{@link}内联标签创建一个链接到您指定的namepath或URL。当您使用{@link}标签，还可以提供几种不同的格式的链接文本。
	如果你不提供任何链接文本，JSDoc使用namepath或URL作为链接文字。
	别名：@linkcode  @linkplain
	语法：{@link namepathOrURL}
		  [link text]{@link namepathOrURL}
		  {@link namepathOrURL|link text}
		  {@link namepathOrURL link text (after the first space)}
	下面的例子显示了提供给{@link} 标签链接文本的所有方式
	```
	/**
	 * See {@link MyClass} and [MyClass's foo property]{@link MyClass#foo}.
	 * Also, check out {@link http://www.google.com|Google} and
	 * {@link https://github.com GitHub}.
	 */
	function myFunction() {}
	```
###	@tutorial
	{@tutorial}行内标签创建一个链接到您指定的教程标识符。当您使用{@tutorial}标签，您也可以提供几种不同的格式的链接文本。
	如果你不提供任何链接文本，JSDoc使用本教程的标题作为链接文字。
	语法：{@tutorial tutorialID}
		  [link text]{@tutorial tutorialID}
		  {@tutorial tutorialID|link text}
		  {@tutorial tutorialID link text (after the first space)}
	下面的例子显示了提供给{@tutorial}标签链接文本的所有方式
	```
	/**
	 * See {@tutorial gettingstarted} and [Configuring the Dashboard]{@tutorial dashboard}.
	 * For more information, see {@tutorial create|Creating a Widget} and
	 * {@tutorial destroy Destroying a Widget}.
	 */
	function myFunction() {}
	```
>	参考文档：
	[jsDoc文档](http://www.css88.com/doc/jsdoc/)