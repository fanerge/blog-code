---
title: jsDoc学习-入门知识
date: 2017-09-24 19:47:00
category: "代码规范"
tags: ['js', '代码规范']
copyright: true
---
##	jsDoc介绍
定义：JSDoc是一个根据javascript文件中注释信息，生成JavaScript应用程序或库、模块的API文档 的工具。		
jsDoc注释：是以 /\*\* 来作为jsDoc注释的开始，其它如 /\* 和 /\*** 的注释都会被jsDoc解析器忽略。
	在webstorm中自动生成jsDoc注释
	示例：
	```
	/**
	 * Book类，代表一本书
	 * @param title
	 * @param anthor
	 * @param pageNum
	 * @constructor
	 */
	function Book (title, anthor, pageNum ) {
		// 实例属性
		this.title = title;
		this.anthor = anthor;
		this.pageNum = pageNum || 0;
	}
	Book.prototype = {
		/**
		 * 获取书本的标题
		 * @returns {*}
		 */
		getTitle : function () {
			return this.title;
		},
		/**
		 * 获取书本的作者
		 * @returns {*}
		 */
		getAnthor: function  () {
			return this.anthor;
		},
		/**
		 * 设置书本的页码
		 * @param pageNum
		 */
		setPageNum: function (pageNum) {
			this.pageNum = pageNum;
		}
	};
	```
##	入门
###	Namepaths in JSDoc 3（JSDoc 3中的名称路径）
	如果涉及到一个JavaScript变量，这个变量在文档中的其他地方，你必须提供一个唯一标识符映射到该变量。
	名称路径提供了一种这样做的方法，并且消除实例成员，静态成员和内部变量之间的歧义。
	jsDoc中名称路径的基本语法示例：
1.	myFunction
2.	MyConstructor
3.	MyConstructor#instanceMember（实例成员）
4.	MyConstructor.staticMember（静态成员）
5.	MyConstructor~innerMember（内部成员）
	示例：
	```
	/**
	 * Person类，一个人
	 * @param name
	 * @constructor
	 */
	const  Person = function (name) {
		this.name = name;
		/**
		 * 这是一个实例成员
		 * @returns {string}
		 */
		this.say = () => {
			return 'I\'am an instance.';
		};
		/**
		 * 这是一个内部成员
		 * @returns {string}
		 */
		function say () {
			return 'I\' am an inner.';
		};
	};
	/**
	 * 这是一个静态成员
	 * @returns {string}
	 */
	Person.say = function () {
		return 'I\'am an static.';
	};
	/**
	 * 实例化一个人
	 * @type {Person}
	 */
	const person1 = new Person('fanerge');
	// Person#say 使用实例方法
	person1.say();
	// Person.say 使用静态方法
	Person.say();
	// Person~say 使用内部方法，但这里不能访问到
	```
	使用名称路径也有一些特殊的情况：@module名称由"module:"前缀，
	@external 名称由"external:"前缀，@event名称由"event:"前缀。
###	JSDoc中的命令行参数
	使用JSDoc最基本的，像这样使用：
	`/path/to/jsdoc yourSourceCodeFile.js anotherSourceCodeFile.js ...`
	其中...是生成文档文件的路径。
	JSDoc支持大量的命令行选项，其中许多选项有长和短两种形式。
	选项	描述
	-a <value>, --access <value>	只显示特定access方法属性的标识符： private, protected, public, or undefined, 或者 all（表示所有的访问级别）。默认情况下， 显示除private标识符以外的所有标识符。
	-c <value>, --configure <value>	JSDoc配置文件的路径。默认为安装JSDoc目录下的conf.json或conf.json.EXAMPLE。
	-d <value>, --destination <value>	输出生成文档的文件夹路径。JSDoc内置的Haruki模板，使用console 将数据转储到控制台。默认为./out。
	--debug	打印日志信息，可以帮助调试JSDoc本身的问题。
	-e <value>, --encoding <value>	当JSDoc阅读源代码时假定使用这个编码，默认为 utf8。
	-h, --help	显示JSDoc的命令行选项的信息，然后退出。
	--match <value>	只有运行测试，其名称中包含value。
	--nocolor	当运行测试时，在控制台输出信息不要使用的颜色。在Windows中，这个选项是默认启用的。
	-p, --private	将标记有[@private 标签][tags-private.md]的标识符也生成到文档中。默认情况下，不包括私有标识符。
	-P, --package	包含项目名称，版本，和其他细节的package.json文件。默认为在源路径中找到的第一个package.json文件。
	--pedantic	将错误视为致命错误，将警告视为错误。默认为false。
	-q <value>, --query <value>	一个查询字符串用来解析和存储到全局变量env.opts.query中。示例：foo=bar&baz=true。
	-r, --recurse	扫描源文件和导览时递归到子目录。
	-R, --readme	用来包含到生成文档的README.md文件。默认为在源路径中找到的第一README.md 文件。
	-t <value>, --template <value>	用于生成输出文档的模板的路径。默认为templates/default，JSDoc内置的默认模板。
	-T, --test	运行JSDoc的测试套件，并把结果打印到控制台。
	-u <value>, --tutorials <value>	导览路径，JSDoc要搜索的目录。如果省略，将不生成导览页。查看导览说明，以了解更多信息。
	-v, --version	显示JSDoc的版本号，然后退出。
	--verbose	日志的详细信息到控制台JSDoc运行。默认为false。
	-X, --explain	以JSON格式转储所有的doclet到控制台，然后退出。

	示例：
	使用配置文件/path/to/my/conf.json，为./src目录的中文件生成文档，并保存输出到./docs目录中：
		/path/to/jsdoc src -r -c /path/to/my/conf.json -d docs
	运行所有JSDoc的测试，其名称包含 tag，并记录每个测试信息：
		/path/to/jsdoc -T --match tag --verbose
###	用conf.json配置JSDoc
	Configuration File(配置文件)
		要自定义JSDoc的行为，可以使用JSON格式的配置文件格式化JSDoc，使用-c选项，例如： jsdoc -c /path/to/conf.json。
		默认的jsDoc配置文件conf.json.EXAMPLE
		```
		{
			"tags": {
				"allowUnknownTags": true,
				"dictionaries": ["jsdoc","closure"]
			},
			"source": {
				"includePattern": ".+\\.js(doc)?$",
				"excludePattern": "(^|\\/|\\\\)_"
			},
			"plugins": [],
			"templates": {
				"cleverLinks": false,
				"monospaceLinks": false
			}
		}
		```
	Specifying input files(指定输入文件)
		source选项组，结合给JSDoc命令行的路径，确定哪些文件要用JSDoc生成文档。 
		```
		"source": {
			"include": [ /* array of paths to files to generate documentation for */ ],
			"exclude": [ /* array of paths to exclude */ ],
			"includePattern": ".+\\.js(doc)?$",
			"excludePattern": "(^|\\/|\\\\)_"
		}
		```
	Incorporating command-line options into the configuration file(合并命令行选项到配置文件)
		它有可能把许多JSDoc的命令行选项放到配置文件，而不用在命令行中指定它们。
		要做到这一点，只要在conf.json的opts部分中使用的相关选项的longnames,值是该选项的值。
		```
		"opts": {
			"template": "templates/default",  // same as -t templates/default
			"encoding": "utf8",               // same as -e utf8
			"destination": "./out/",          // same as -d ./out/
			"recurse": true,                  // same as -r
			"tutorials": "path/to/tutorials", // same as -u path/to/tutorials
		}
		```
	Plugins（插件）
		要启用插件，只要添加它们的路径（相对于JSDoc文件夹）到plugins数组中就可以了。
		示例：
		例如，以下将包括 markdown 插件，它转换 markdown格式的文本为HTML，和“summarize”插件，该自动生成的每个的doclet的摘要：
		```
		"plugins": [
			"plugins/markdown",
			"plugins/summarize"
		]
		```
	Tags and tag dictionaries(标签和标签字典)
		tags选项控制哪些JSDoc标签允许被使用和解析。
		```
		"tags": {
			"allowUnknownTags": true,
			"dictionaries": ["jsdoc","closure"]
		}
		```
###	配置JSDoc的默认模板
	JSDoc的默认模板中提供了几个选项，您可以使用自定义外观和内容来生成文档。
	要使用这些选项，您必须为JSDoc创建一个配置文件，并在配置文件中设置相应的选项。
	
	Generating pretty-printed source files（生成适合打印的文档）
		默认情况下，JSDoc的默认模板为你的源文件生成适合打印的文档。在文档中，它还链接到那些适合的打印文件。
		要禁用适合打印的文件，设置选项templates.default.outputSourceFiles为false。
		使用该选项也将删除文档中链接到源文件的连接。此选项在JSDoc3.3.0及更高版本上是可用的。
	Copying static files to the output directory(复制静态文件到输出目录)
		SDoc的默认模板会自动复制一些静态文件，如CSS样式表，到输出目录。
		在JSDoc3.3.0或更高版本，你可以告诉默认模板复制附加静态文件到输出目录。
		例如，您可能希望复制一个图像的目录到输出目录，所以你可以在你的文档中显示这些图像。
	Showing the current date in the page footer（在页脚显示当前日期）
		默认情况下，JSDoc的默认模板总是在生成文档的页脚显示当前日期。
		在JSDoc3.3.0或更高版本，可以通过设置选项templates.default.includeDate为false来忽略当前日期。
	Showing longnames in the navigation column（在导航栏中显示长文件名）
		默认情况下，JSDoc的默认模板在导航列中显示每个标识符缩写的名字。
		例如，标识符my.namespace.MyClass将简单地称为显示MyClass。相反,要显示完整的长名称，设置选项templates.default.useLongnameInNav为true。
		此选项在JSDoc3.4.0及更高版本中可用。
	Overriding the default template's layout file（重写默认模板的布局文件）
		默认的模板使用名为 layout.tmpl 的文件 指定每个生成文档的页面中的页眉和页脚。
		特别是，每个生产的文档页面会加载该文件定义了CSS和JavaScript文件。在JSDoc3.3.0或更高版本，可以指定使用自己的layout.tmpl文件，它允许你加载自己的自定义CSS和JavaScript文件，去除或替代，标准的文件。
		要使用此功能，设置选项templates.default.layoutFile的路径到你的自定义布局文件。
		路径是相对于config.json文件，当前的工作目录，和JSDoc目录的相对路径，按照这个顺序。
###	块标签和内联标签
	JSDoc支持两种不同类型的标签：
1.		块标签, 这是在一个JSDoc注释的最高级别。
2.		内联标签, 块标签文本中的标签或说明。
	块标签通常会提供有关您的代码的详细信息，如一个函数接受的参数。
		块标签总是以 at 符号（@）开头。除了JSDoc注释中最后一个块标记，每个块标签后面必须跟一个换行符。
	内联标签通常链接到文件的其他部分，类似于HTML中的锚标记（<a>）。
		内联标签也以 at 符号（@）开。然而，内联标签及其文本必须用花括号（{ and }）括起来。 { 表示行内联标签的开始，而}表示内联标签的结束。
		如果你的标签文本中包含右花括号（}），则必须用反斜线（ \ ）进行转义。
		在内联标签后,你并不需要使用一个换行符。
	示例：
	```
	/**
	 * Shoe类，一双鞋子
	 * @param color
	 * @param size
	 * @constructor
	 */
	const Shoe = function (color, size){
		this.color = color;
		this.size = size;
	};
	/**
	 * Set shoe's color. Use {@link Shoe#setSize} to set the shoe size
	 * @param color
	 */
	Shoe.prototype.setColor = function (color) {
		this.color = color;
	};
	/**
	 * 设置鞋子的尺寸
	 * @param size
	 */
	Shoe.prototype.setSize = function (size) {
		this.size = size;
	};
	/**
	 * 设置花边的颜色和样式
	 * @param color
	 * @param type
	 */
	Shoe.prototype.setLaceType = function (color, type) {
		// ...
	};
	var shoe1 = new Shoe('red', 12);
	```
###	关于JSDoc插件
	Creating and Enabling a Plugin（创建并启用插件）
		创建并启用新JSDoc插件,需要两个步骤：
			创建一个包含你的插件代码的JavaScript模块.
			将该模块添加到JSDoc配置文件的plugins数组中。你可以指定一个绝对或相对路径。
			如果使用相对路径，JSDoc按照相对于配置文件所在的目录，当前的工作目录和JSDoc安装目录的顺序搜索插件。
	Authoring JSDoc 3 Plugins（创建JSDoc3插件）
		JSDoc 3的插件系统广泛的控制着解析过程。一个插件可以通过执行以下任何一项操作，影响解析结果：
1.		定义事件处理程序
2.		定义标签
3.		定义一个抽象语法树节点的访问者
	Event Handlers（事件处理程序）
		事件: parseBegin -- JSDoc开始加载和解析源文件之前，parseBegin事件被触发。
		事件: fileBegin -- 当解析器即将解析一个文件fileBegin事件触发。
		事件: beforeParse -- beforeParse事件在解析开始之前被触发。
		事件: jsdocCommentFound -- 每当JSDoc注释被发现,jsdocCommentFound事件就会被触发。	
		事件: symbolFound -- 当解析器在代码中遇到一个可能需要被文档化的标识符时，symbolFound 事件就会被触发。
		事件: newDoclet -- newDoclet事件是最高级别的事件。新的doclet已被创建时，它就会被触发。
		事件: fileComplete -- 当解析器解析完一个文件时，fileComplete 事件就会被触发。
		事件: parseComplete -- JSDoc解析所有指定的源文件之后，parseComplete事件就会被触发。
		事件: processingComplete -- JSDoc更新反映继承和借来的标识符的解析结果后，processingComplete事件被触发。
	Tag Definitions （标签定义）
		添加标签到标签字典是影响文档生成的一个中级方式。
		在一个newDoclet事件被触发前，JSDoc注释块被解析以确定可能存在的说明和任何JSDoc标签。
		当一个标签被发现，如果它已在标签字典被定义，它就会被赋予一个修改doclet的机会。
	The Dictionary（字典）
	Node Visitors（节点访问者）
	Reporting Errors(报告错误)
###	使用Markdown插件
	Enabling the Markdown plugin(启用markdown插件)
		要启用markdown插件，只要将字符串plugins/markdown添加到JSDoc配置文件的plugins数组中即可。
	Converting Markdown in additional JSDoc tags（在额外的JSDoc标签中转换Markdown）
		默认情况下，markdown插件只处理特定JSDoc标签的markdown文本。
		您可以通过添加一个 markdown.tags属性到你的JSDoc配置文件中，来处理的其他标签中的markdown文本。
	Excluding the default tags from Markdown processing（剔除markdown默认处理的标签）
		为了防止Markdown插件处理任何默认JSDoc标签，添加一个markdown.excludeTags属性到您的JSDoc配置文件。
	Hard-wrapping text at line breaks （用换行符换行文本）	
		默认情况下，Markdown插件不处理换行符换行的文本。
	Adding ID attributes to headings（添加ID属性到标题标签）
		默认情况下，Markdown插件不会给每个HTML标题标签添加id 属性。
###	Tutorials 教程
	JSDoc允许你的API文档的页面旁边包含教程。
	您可以使用此功能来为您的API提供详细的使用说明，如“入门”指南或实现一个功能的一步一步的过程。
	
	Adding tutorials（添加教程）
		添加教程到您的API文档，可以通过--tutorials 或 -u 选项运行JSDoc，并提供JSDoc要搜索的教程目录。
		```
		jsdoc -u path/to/tutorials path/to/js/files
		```
	Configuring titles, order, and hierarchy （配置标题，顺序和层次结构）
		默认情况下，JSDoc使用的文件名作为教程标题，并且所有的教程都在同一层次。
		您可以使用JSON文件为每个教程提供标题并指示文档中的教程应如何排序和分组。
	Linking to tutorials from API documentation（从API文档链接到教程）
		@tutorial 块标签
		{@tutorial} 内联标签
###	包含Package（包）文件
	包文件包含的信息对你的项目文档是很有用的，比如该项目的名称和版本号。
	当JSDoc生成的文档的时候,可以自动使用项目中package.json文件中的信息。
	示例，默认的模板在文档中显示项目的名称和版本号。
		在源路径中包含一个包文件:
			jsdoc path/to/js path/to/package/package.json
		使用 -P/--package 选项：
			jsdoc --package path/to/package/package-docs.json path/to/js	
###	包含 README 文件
	有两种方法可以将 README 文件中的信息合并到您的文档：
	在源路径中包含一个 README 文件:
		jsdoc path/to/js path/to/readme/README.md
	使用 -R/--readme 选项：
		jsdoc --readme path/to/readme/README path/to/js
>	参考文档：
	[jsDoc文档](http://www.css88.com/doc/jsdoc/)	