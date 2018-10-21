---
title: jsDoc学习-标签总结
date: 2017-09-27 22:46:04
category: "代码规范"
tags: ['js', '代码规范']
copyright: true
---
##	类和构造函数
	@class -- 此函数旨在需要使用”new”关键字调用（构造函数）或ES6中class。
		@constructor
	@classdesc -- 使用的后面的蚊子来描述整个类。
	@abstract -- 这个成员必须在继承的子类中重写。
		@virtual
	@static -- 记录一个静态成员。
	@access -- 指定该成员的访问级别（私有private、保护protected、公共public）。
		@access private 等价于 @private
		@access protected 等价于 @protected
		@access public 等价于 @public
	@alias -- 标记成员有一个别名。
	@extends -- 指名这个子类继承至哪个父类，后面需要加父类名。
		@augments
	@instance -- 记录一个实例成员。
	@interface -- 这是别人可以实现的一个接口。
	
##	对象
	@borrows -- 这个对象使用另一个对象的某些东西。
	@lends -- 将一个字面量对象的所有属性标记为某个标识符（类或模块）的成员。
	@mixes -- 此对象混入了另一个对象中的所有成员。
	@mixin -- 记录一个mixin（混入）对象。
	@name  -- 记录一个对象的名称。
	@namespace -- 记录一个命名空间对象。
	@property -- 记录一个对象的属性。
		@prop
	@typedef -- 记录一个自定义的类型。
	@type -- 记录一个对象的类型。
	@variation -- 区分具有相同名称的不同的对象。
	
##	方法
	@callback -- 描述一个回调函数。
	@function -- 描述一个函数或方法。
		@func
		@method
	@returns -- 记录一个函数的返回值。
		@return
	@this -- this关键字的指向。


##	说明
	@author -- 指定项目的作者。
	@constant -- 记录一个对象作为一个常量。
		@const
	@default -- 记录默认值。
		@defaultvalue
	@copyright -- 描述一些版权信息。
	@since -- 该方法添加于该版本，建议使用。
	@deprecated -- 该方法已弃用，不建议使用。
	@description -- 描述一个标识。
		@desc
	@enum -- 描述一个相关属性的集合。
	@example -- 提供一个如何使用描述项的例子。
	@external -- 标识一个外部的类，命名空间，或模块。
	@file -- 描述一个文件。
		@fileoverview
		@overview
	@global -- 记录一个全局对象。
	@ignore -- 忽略文档中的一个标识。
	@implements -- 这个表示实现一个接口。
	@inheritdoc -- 指明这个标识应继承其父类的文档。
	@inner -- 描述一个内部对象。
	@kind -- 表示的类型。
	@license -- 表示你的代码采用何种软件许可协议。
	@member -- 记录一个成员。
		@var
	@memberof -- 标明这个标识属于哪个父级标识。
	@override -- 指明一个标识符覆盖其父类同名的标识符。
	@param -- 记录传递给一个函数的参数。
		@arg 
		@argument
	@readonly -- 标记为只读的。
	@see -- 更多详细参阅其他一些文档。
	@summary -- 完整描述的一个简写版本。
	@throws -- 说明可能会被抛出什么样的错误。
	@todo -- 记录一个将要完成的任务。
	@tutorial -- 插入一个连接到包含教程文件。
	@version -- 记录版本信息。
	
##	事件
	@event -- 描述一个事件。
	@listens -- 列出一个标识的监听事件。
	@fires -- 描述事件这个方法可能会触发。
		@emits
	
##	模块
	@exports -- 表示一个由javascript模块导出的成员。
	@module -- 记录一个javascript模块。
	@requires -- 这个文件需要一个javascript模块。
	
##	内联标签
	@link -- 连接到文档中的另一个项目。
		@linkcode
		@linkplain
	@tutorial -- 链接到一个教程。
	
##	使用
1.	安装jsdoc3 
	npm install -g jsdoc
2.	在cmd 中 (此时会在test.js的同级目录产出out目录，存放生成的API文档)
	jsdoc test.js
		默认配置情况下out目录中产出：
			fonts、scripts、styles目录顾名思义。
			index.html -- API文档首页
			global.html -- 全局的（成员和方法）
			index.js.html -- 源代码页面
3.	相关配置
	启用jsdoc有两种方式：命令行参数 和 conf.json配置
	JSDoc命令行参数	
		JSDoc命令行几个常用参数有以下几个：
		-c, --configure 指定configuration file
		-d, --destination 指定输出路径，默认./out
		-e, --encoding 设定encoding，默认utf8
		-p, --private 将private注释输出到文档，默认不输出
		-P, --package 指定package.json file
		-r, --recurse 查询子目录
		-t, --template 指定输出文档template
		-u, --tutorials 指定教程路径，默认无
		例如：jsdoc -T --match tag --verbose
	JSDoc配置文件
		默认的配置文件：conf.json.EXAMPLE
		```
		{
			"tags": {
				"allowUnknownTags": true, // 允许使用自定义tag
				"dictionaries": ["jsdoc","closure"] // 定义tag集
			},
			"source": {
				"includePattern": ".+\\.js(doc)?$", // 将以.js, .jsdoc结尾的文件作为源文件
				"excludePattern": "(^|\\/|\\\\)_" // 忽略以_开头的文件夹及文件
			},
			"plugins": [],
			"templates": {
				"cleverLinks": false,
				"monospaceLinks": false
			}
		}
		```
		例如：jsdoc -c /path/to/conf.json
		
>	参考文档：
	[jsdoc](http://www.css88.com/doc/jsdoc/inline-tags.html)
	[jsdoc3](https://github.com/jsdoc3/jsdoc)
	[使用 JSDoc 3 自动生成 JavaScript API 文档](http://www.moodpo.com/archives/jsdoc3-tutorial.html)




