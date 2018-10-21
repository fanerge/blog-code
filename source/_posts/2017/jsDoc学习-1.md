---
title: jsDoc学习-常用示例
date: 2017-09-25 19:51:34
category: "代码规范"
tags: ['js', '代码规范']
copyright: true
---
##	jsDoc 示例
###	ES 2015 Classes
	Documenting a simple class（文档化一个简单的类）	
		演示了如何通过一个构造函数，两个实例方法和一个静态方法文档化一个简单的类
		```
		class Point {
			/**
			 * 构造函数
			 * @param x
			 * @param y
			 */
			constructor (x, y) {
				this.x = x;
				this.y = y;
			}

			/**
			 * 实例方法
			 * @returns {*}
			 */
			getX () {
				return this.x;
			}

			/**
			 * 实例方法
			 * @returns {*}
			 */
			getY () {
				return this.y;
			}

			/**
			 * 静态方法
			 * @param str
			 */
			static fromString (str) {
				// ...
			}
		}
		var de = new Point('1','2');
		console.log(de.getX()); // '1'
		```
	Extending classes（扩展类）
		当您使用 extends关键字来扩展一个现有的类的时候，你还需要告诉JSDoc哪个类是你要扩展的。
		```
		/** Class representing a point. */
		class Point {
			/**
			 * 构造函数
			 * @param x
			 * @param y
			 */
			constructor (x, y) {
				this.x = x;
				this.y = y;
			}

			/**
			 * 实例方法
			 * @returns {*}
			 */
			getX () {
				return this.x;
			}

			/**
			 * 实例方法
			 * @returns {*}
			 */
			getY () {
				return this.y;
			}

			/**
			 * 静态方法
			 * @param str
			 */
			static fromString (str) {
				// ...
			}
		}
		/** Class representing a point
		 * @extends Point
		 */
		class Dot extends  Point {
			/**
			 * 构造函数
			 * @param x
			 * @param y
			 * @param width
			 */
			constructor (x, y, width) {
				super(x,y);
				this.width = width;
			}

			/**
			 * 实例方法
			 * @returns {*}
			 */
			getWidth () {
				return this.width;
			}
		}

		var dd = new Dot(1, 2, 3);
		console.log(dd.getX()); // 1
		```
###	ES 2015 Modules
	Module identifiers（模块标识符）
		当你描述一个 ES 2015 module（模块）时，您将使用@module 标签来描述模块的标识符。
		```
		/**
		 * @module my/shirt
		 */
		import * as myShirt from 'my/shirt';
		```
		当您使用一个 JSDoc namepath（名称路径）从另一个JSDoc注释中引用一个模块，您必须添加前缀module:。
		例如，如果你想模块my/pants的文档 连接到模块my/shirt，您可以使用@see 标签来描述my/pants。
		```
		/**
		 * Pants module.
		 * @module my/pants
		 * @see module:my/shirt
		 */
		```
		同样，模块中每个成员的namepath （名称路径）将以module: 开始，后面跟模块名字。
		例如，如果你的my/pants模块输出一个Jeans类，并且Jeans 有一个名为hem的实例方法，
		那么这个实例方法longname（长名称）是module:my/pants.Jeans#hem。
	Exported values （导出值）
		```
		/** The name of the module. */
		export const name = 'mixer';

		/** The most recent blended color. */
		export var lastColor = null;

		/**
		 * Blend two colors together.
		 * @param {string} color1 - The first color, in hexidecimal format.
		 * @param {string} color2 - The second color, in hexidecimal format.
		 * @return {string} The blended color.
		 */
		export function blend(color1, color2) {}

		// convert color to array of RGB values (0-255)
		function rgbify(color) {}

		export {
			/**
			 * Get the red, green, and blue values of a color.
			 * @function
			 * @param {string} color - A color, in hexidecimal format.
			 * @returns {Array.<number>} An array of the red, green, and blue values,
			 * each ranging from 0 to 255.
			 */
			rgbify as toRgb
		}
		```
###	CommonJS Modules
	Module identifiers（模块标识符）	
		例如，如果你想模块my/pants的文档 连接到模块my/shirt，您可以使用@see 标签来描述my/pants。
		```
		/**
		 * Pants module.
		 * @module my/pants
		 * @see module:my/shirt
		 */
		```
	Properties of the 'exports' object（'exports'对象的属性）
		例如，方法添加到导出对象。
		```
		/**
		 * Shirt module.
		 * @module my/shirt
		 */

		/** Button the shirt. */
		exports.button = function() {
			// ...
		};

		/** Unbutton the shirt. */
		exports.unbutton = function() {
			// ...
		};
		```
	Values assigned to local variables （值分配给局部变量）
		例如，longname（长名称）定义在 @alias 标签中。
		```
		/**
		 * Shirt module.
		 * @module my/shirt
		 */

		/**
		 * Wash the shirt.
		 * @alias module:my/shirt.wash
		 */
		var wash = exports.wash = function() {
			// ...
		};
		```
		例如，JSDoc注释放在exports.wash之前。
		```
		/**
		 * Shirt module.
		 * @module my/shirt
		 */

		var wash =
		/** Wash the shirt. */
		exports.wash = function() {
			// ...
		};
		```
	Values assigned to 'module.exports' （值分配给'module.exports'）
		Object literal assigned to 'module.exports'（对象字面量分配给'module.exports'）
		例如：对象字面量分配给'module.exports'。
		```
		/**
		 * Color mixer.
		 * @module color/mixer
		 */

		module.exports = {
			/** Blend two colors together. */
			blend: function(color1, color2) {
				// ...
			},

			/** Darken a color by the given percentage. */
			darken: function(color, percent) {
				// ..
			}
		};
		```
		例如，通过属性定义，分配给module.exports。
		```
		/**
		 * Color mixer.
		 * @module color/mixer
		 */

		module.exports = {
			/** Blend two colors together. */
			blend: function(color1, color2) {
				// ...
			}
		};

		/** Darken a color by the given percentage. */
		module.exports.darken = function(color, percent) {
			// ..
		};
		```
		Function assigned to 'module.exports'（函数分配给'module.exports'）
		例如，函数分配给'module.exports'。
		```		
		/**
		 * Color mixer.
		 * @module color/mixer
		 */

		/** Blend two colors together. */
		module.exports = function(color1, color2) {
			// ...
		};
		```
		例如，构造函数分配给'module.exports'。
		```
		/**
		 * Color mixer.
		 * @module color/mixer
		 */

		/** Create a color mixer. */
		module.exports = function ColorMixer() {
			// ...
		};
		```
		String, number, or boolean assigned to 'module.exports'（字符串，数字，或布尔值分配给'module.exports'）
		例如，字符串分配给'module.exports'。
		```
		/**
		 * Module representing the word of the day.
		 * @module wotd
		 * @type {string}
		 */

		module.exports = 'perniciousness';
		```
		Values assigned to 'module.exports' and local variables （值分配给'module.exports'和局部变量）
		例如，对象字面量分配给一个局部变量和module.export。
		```
		/**
		 * Color mixer.
		 * @exports color/mixer
		 */
		var mixer = module.exports = {
			/** Blend two colors together. */
			blend: function(color1, color2) {
				// ...
			}
		};
		```
		Properties added to 'this'（属性添加到'this'）
		例如，属性添加到一个模块的'this'对象。
		```
		/** @module bookshelf */

		/** @class */
		this.Book = function(title) {
			/** The title of the book. */
			this.title = title;
		}
		```
###	AMD Modules
	Module identifiers（模块标识符）
		```
		/**
		 * Pants module.
		 * @module my/pants
		 * @see module:my/shirt
		 */
		```
	Function that returns an object literal（函数返回一个对象字面量）
		例如，函数返回一个对象字面量。
		```
		define('my/shirt', function() {
		   /**
			* A module representing a shirt.
			* @exports my/shirt
			*/
			var shirt = {
				/** The module's `color` property. */
				color: 'black',

				/** @constructor */
				Turtleneck: function(size) {
					/** The class' `size` property. */
					this.size = size;
				}
			};

			return shirt;
		});
		```
	Function that returns another function（函数返回另一个函数）
		例如，函数返回另一个函数。
		```
		/**
		 * A module representing a jacket.
		 * @module my/jacket
		 */
		define('my/jacket', function() {
			/**
			 * @constructor
			 * @alias module:my/jacket
			 */
			var Jacket = function() {
				// ...
			};

			/** Zip up the jacket. */
			Jacket.prototype.zip = function() {
				// ...
			};

			return Jacket;
		});
		```
	Module declared in a return statement （模块声明在return语句中）
		例如，模块声明在return语句中。
		```
		/**
		 * Module representing a shirt.
		 * @module my/shirt
		 */

		define('my/shirt', function() {
			// Do setup work here.

			return /** @alias module:my/shirt */ {
				/** Color. */
				color: 'black',
				/** Size. */
				size: 'unisize'
			};
		});
		```
	Module object passed to a function（模块对象传递给一个函数）
		例如，模块对象传递给一个函数。
		```
		define('my/jacket', function(
			/**
			 * Utility functions for jackets.
			 * @exports my/jacket
			 */
			module) {

			/**
			 * Zip up a jacket.
			 * @param {Jacket} jacket - The jacket to zip up.
			 */
			module.zip = function(jacket) {
				// ...
			};
		});
		```
	Multiple modules defined in one file（多模块定义在一个文件中）
		例如，多模块定义在一个文件中。
		```
		// one module
		define('html/utils', function() {
			/**
			 * Utility functions to ease working with DOM elements.
			 * @exports html/utils
			 */
			var utils = {
				/** Get the value of a property on an element. */
				getStyleProperty: function(element, propertyName) { }
			};

			/** Determine if an element is in the document head. */
			utils.isInHead = function(element) { }

			return utils;
			}
		);

		// another module
		define('tag', function() {
			/** @exports tag */
			var tag = {
				/** @class */
				Tag: function(tagName) {
					// ...
				}
			};

			return tag;
		});
		```		
>	参考文档：
	[jsDoc文档](http://www.css88.com/doc/jsdoc/)	