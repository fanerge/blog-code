---
title: Sass学习
date: 2017-09-20 19:41:05
category: "css"
tags: ['css','Sass','web']
---
Sass是在CSS语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能，这些拓展令 CSS 更加强大与优雅。
##	常用的指令
	在命令行中运行 Sass
		sass input.scss output.css
	监视单个 Sass 文件，每次修改并保存时自动编译
		sass --watch input.scss:output.css
	监视整个文件夹
		sass --watch app/scss:public/stylesheets
	开启debug信息
		sass --watch input.scss:output.css --debug-info
	选择编译格式并添加调试map
		sass --watch input.scss:output.css --style expanded --sourcemap
	编译添加调试map
		sass --watch input.scss:output.css --sourcemap
	编译格式
		sass --watch input.scss:output.css --style compact
##	使用变量
	sass让人们受益的一个重要特性就是它为css引入了变量。你可以把反复使用的css属性值 定义成变量，然后通过变量名来引用它们，而无需重复书写这一属性值。或者，对于仅使用过一 次的属性值，你可以赋予其一个易懂的变量名，让人一眼就知道这个属性值的用途。
	sass使用$符号来标识变量，如比如$highlight-color和$sidebar-width。
###	变量声明
1.	声明单个属性值
	$higlight-color: #f90;
2.	声明多个属性值
	$basic-border: 1px solid black;
3.	逗号分割多个属性值
	$plain-font: "Myriad Pro",Myriad,"Helvetica Neue",Helvetica,"Liberation Sans",Arial,sans-serif,sans-serif;
4.	变量的作用域
	当变量定义在css规则块内，那么该变量只能在此规则块内使用。
	如果它们出现在任何形式的{...}块中（如@media或者@font-face块）
	```
	$nav-color: #F90; // 全局声明
	nav {
	  $width: 100px; // 局部声明
	  width: $width; // 100px
	  color: $nav-color; // #f90
	}
	// 这意味着是你可以在样式表的其他地方定义和使用$width变量，不会对这里造成影响。
	```
5.	在声明变量时，变量值也可以引用其他变量。
	$highlight-color: #F90;
	$highlight-border: 1px solid $highlight-color;
###	变量引用
	如果你需要一个不同的值，只需要改变这个变量的值，则所有引用此变量的地方生成的值都会随之改变。
	```
	// 声明变量
	$highlight-color: #F90;
	$highlight-border: 1px solid $highlight-color;
	.selected {
	  border: $highlight-border; // 引用变量
	}
	```
###	变量命名
1.	变量名用中划线还是下划线分隔
	用中划线声明的变量可以使用下划线的方式引用，反之亦然。
	```
	$link-color: blue; // 中划线声明
	a {
	  color: $link_color; // 下划线引用
	}
	```
###	将局部变量升级为全局变量
	编译前
	```
	#main {
	  $width: 5em !global;
	  width: $width;
	}
	#sidebar {
	  width: $width;
	}
	```
	编译后
	```
	#main { 
		width: 5em; 
	}
	#sidebar { 
		width: 5em; 
	}
	```
###	数据类型 (Data Types)
	Interactive Shell
		color: #777 + #777; // color: #eeeeee;
	如果需要使用变量，同时又要确保 / 不做除法运算而是完整地编译到 CSS 文件中，只需要用 #{} 插值语句将变量包裹。
		```
		p {
		  $font-size: 12px;
		  $line-height: 30px;
		  font: #{$font-size}/#{$line-height};
		}
		```
	数字，1, 2, 13, 10px
	字符串，有引号字符串与无引号字符串，"foo", 'bar', baz
	颜色，blue, #04a3f9, rgba(255,0,0,0.5)
	布尔型，true, false
	空值，null
	数组 (list)，用空格或逗号作分隔符，1.5em 1em 0 2em, Helvetica, Arial, sans-serif
	maps, 相当于 JavaScript 的 object，(key1: value1, key2: value2)
##	嵌套CSS 规则
	```
	// 编译前
	#content {
	  article {
		h1 { color: #333 }
		p { margin-bottom: 1.4em }
	  }
	  aside { background-color: #EEE }
	}
	// 编译后
	#content article h1 { color: #333 }
	#content article p { margin-bottom: 1.4em }
	#content aside { background-color: #EEE }
	```
	当你同时要为一个容器元素及其子元素编写特定样式时，这种能力就非常有用了。
	```
	// 编译前
	#content {
	  background-color: #f5f5f5;
	  aside { background-color: #eee }
	}
	// 编译后
	#content { background-color: #f5f5f5 }
	#content aside { background-color: #eee }
	```
###	父选择器的标识符&
	作用于伪类:hover、:after等等
	```
	// 编译前
	article a {
	  color: blue;
	  &:hover { color: red }
	}
	// 编译后
	article a { color: blue }
	article a:hover { color: red }
	```
	父选择器标识符还有另外一种用法，你可以在父选择器之前添加选择器。
	举例来说，当用户在使用IE浏览器时，你会通过JavaScript在<body>标签上添加一个ie的类名，为这种情况编写特殊的样式如下
	```
	// 编译前
	#content aside {
	  color: red;
	  body.ie & { color: green }
	}
	// 编译后
	#content aside {color: red};
	body.ie #content aside { color: green }
	```
###	群组选择器的嵌套
	```
	// 编译前
	nav, aside {
	  a {color: blue}
	}
	// 编译后
	nav a, 
	aside a {
		color: blue
	}
	```
###	子组合选择器和同层组合选择器：>、+和~	
	上边这三个组合选择器必须和其他选择器配合使用，以指定浏览器仅选择某种特定上下文中的元素。	
	可以把它们放在外层选择器后边，或里层选择器前边。
	>  直接后代选择器
	+  紧接的相邻兄弟选择器
	~  同层全体组合选择器
###	嵌套属性
	除了CSS选择器，属性也可以进行嵌套。
	嵌套属性的规则是这样的：把属性名从中划线-的地方断开，在根属性后边添加一个冒号:，紧跟一个{ }块，把子属性部分写在这个{ }块中。
1.	属性的嵌套
	```
	// 编译前
	nav {
	  border: {
	  style: solid;
	  width: 1px;
	  color: #ccc;
	  }
	}
	// 编译后
	nav {
	  border-style: solid;
	  border-width: 1px;
	  border-color: #ccc;
	}
	```
2.	对于属性的缩写形式
	```
	// 编译前
	nav {
	  border: 1px solid #ccc {
	  left: 0px;
	  right: 0px;
	  }
	}
	// 编译后
	nav {
	  border: 1px solid #ccc;
	  border-left: 0px;
	  border-right: 0px;
	}
	```
##	导入SASS文件
	css有一个特别不常用的特性，即@import规则，它允许在一个css文件中导入其他css文件。
	使用sass的@import规则并不需要指明被导入文件的全名。你可以省略.sass或.scss文件后缀。
	@import "sidebar";这条命令将把sidebar.scss文件中所有样式添加到当前样式表中。
###	使用SASS部分文件
	sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。
	```
	你想导入themes/_night-sky.scss这个局部文件里的变量，
	你只需在样式表中写@import "themes/night-sky";。
	```
###	默认变量值
	你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值。
	如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。	
	使用sass的!default标签可以实现这个目的。
	对于通过@import导入的sass库文件，你可能希望导入者可以定制修改sass库文件中的某些值，非常有用。
	```
	$fancybox-width: 400px !default;
	.fancybox {
		width: $fancybox-width; // 400px
	}
	```
###	嵌套导入
	跟原生的css不同，sass允许@import命令写在css规则内。
	```
	// _blue-theme.scss
	aside {
	  background: blue;
	  color: white;
	}
	// 在主scss文件中index.scss
	.blue-theme {@import "blue-theme"}	
	// 类似于编译前
	.blue-theme {
		aside {
			background: blue;
			color: #fff;
		}
	}
	// 编译后
	.blue-theme aside {
		background: blue;
		color: #fff;
	}
	```
###	原生的CSS导入
	下列3种情况使用原生CSS@import，浏览器解析css时的额外下载。
	```
	被导入文件的名字以.css结尾；
	被导入文件的名字是一个URL地址（比如http://www.sass.hk/css/css.css），由此可用谷歌字体API提供的相应服务；
	被导入文件的名字是CSS的url()值。
	```
##	静默注释
	sass另外提供了一种不同于css标准注释格式/* ... */的注释语法，
	即静默注释，其内容不会出现在生成的css文件中。
	将 ! 作为多行注释的第一个字符表示在压缩输出模式下保留这条注释并输出到 CSS 文件中，通常用于添加版权信息。
	```
	body {
	  color: #333; // 这种注释内容不会出现在生成的css文件中(静默注释)
	  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
	}
	```
	当注释出现在原生css不允许的地方，如在css属性或选择器中，sass将不知如何将其生成到对应css文件中的相应位置，于是这些注释被抹掉。
	```
	body {
	  color /* 这块注释内容不会出现在生成的css中 */: #333;
	  padding: 1; /* 这块注释内容也不会出现在生成的css中 */ 0;
	}
	```
##	混合器
	混合器允许用户编写语义化样式的同时避免视觉层面上样式的重复。
	你可以通过sass的混合器实现大段样式的重用。
	混合器使用@mixin标识符定义。这个标识符给一大段样式赋予一个名字，这样你就可以轻易地通过引用这个名字重用这段样式。
	```
	// 声明混合器
	@mixin rounded-corners {
	  -moz-border-radius: 5px;
	  -webkit-border-radius: 5px;
	  border-radius: 5px;
	}
	// 使用混合器
	div p {
		color: red;
		@include rounded-corners;
	}
	```
###	何时使用混合器
	一条经验法则就是你能否为这个混合器想出一个好的名字。如果你能找到一个很好的短名字来描述这些属性修饰的样式，比如rounded-cornersfancy-font或者no-bullets，那么往往能够构造一个合适的混合器。
	如果你找不到，这时候构造一个混合器可能并不合适。
###	混合器中的CSS规则
	混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性。
	```
	// 定义一个嵌套css混合
	@mixin no-bullets {
	  list-style: none;
	  li {
		list-style-image: none;
		list-style-type: none;
		margin-left: 0px;
	  }
	}
	// 使用混合
	ul.plain {
	  color: #444;
	  @include no-bullets;
	}
	```
	编译后
	```
	ul.plain {
	  color: #444;
	  list-style: none;
	}
	ul.plain li {
	  list-style-image: none;
	  list-style-type: none;
	  margin-left: 0px;
	}
	```
###	给混合器传参
	可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式。
	// 声明带参数的混合
	```
	@mixin link-colors($normal, $hover, $visited) {
	  color: $normal;
	  &:hover { color: $hover; }
	  &:visited { color: $visited; }
	}
	```
	// 使用带参数的混合
	```
	// 使用1
	p a {
		@include link-colors(red, blue, white);
	}
	// 使用2（好处是表明每个参数的意思和顺序）
	a {
		@include link-colors(
		  $normal: blue,
		  $visited: green,
		  $hover: red
	  );
	}
	```
	编译后
	```
	p a { color: blue; }
	p a:hover { color: red; }
	p a:visited { color: green; }
	```
###	默认参数值
	```
	// 默认都使用$normal的值
	@mixin link-colors(
		$normal,
		$hover: $normal,
		$visited: $normal
	  )
	{
	  color: $normal;
	  &:hover { color: $hover; }
	  &:visited { color: $visited; }
	}
	```
	使用混合
	```
	@include link-colors(red)
	编译后
	p a {
		color: red; 
	}
	p a:hover {
		color: red; 
	}
	p a:visited {
		color: red; 
	}
	```
	使用混合
	```
	@include link-colors(red, blue, white)
	编译后
	p a {
		color: red; 
	}
	p a:hover {
		color: blue; 
	}
	p a:visited {
		color: white; 
	}
	```
##	使用选择器继承来精简CSS
	继承允许你声明类之间语义化的关系，通过这些关系可以保持你的css的整洁和可维护性。
1.	选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过@extend语法实现。
	编译前
	```
	//通过选择器继承继承样式
	.error {
	  border: 1px solid red;
	  background-color: #fdd;
	}
	.seriousError {
	  @extend .error;
	  border-width: 3px;
	}
	```
	编译后
	```
	.error, .seriousError {
	  border: 1px solid red;
	  background-color: #fdd; 
	}
	.seriousError {
	  border-width: 3px; 
	}
	```
2.	.seriousError不仅会继承.error自身的所有样式，任何跟.error有关的组合选择器样式也会被.seriousError以组合选择器的形式继承	
	```
	//.seriousError从.error继承样式
	.error a{  //应用到.seriousError a
	  color: red;
	  font-weight: 100;
	}
	h1.error { //应用到hl.seriousError
	  font-size: 1.2rem;
	}
	```
###	何时使用继承
	混合器主要用于展示性样式的重用，而类名用于语义化样式的重用。
	当一个元素拥有的类（比如说.seriousError）表明它属于另一个类（比如说.error），这时使用继承再合适不过了。
	综上所述你应该使用@extend。让.seriousError从.error继承样式，使两者之间的关系非常清晰。
	更重要的是无论你在样式表的哪里使用.error.seriousError都会继承其中的样式。
###	继承的高级用法
	接下来的这段代码定义了一个名为disabled的类，样式修饰使它看上去像一个灰掉的超链接。通过继承a这一超链接元素来实现
	编译前
	```	
	a {
		background: #fff;
	}
	.disabled {
	  color: gray;
	  @extend a;
	}
	```
	编译后
	```
	a, .disabled {
	  background: #fff; 
	}

	.disabled {
	  color: gray; 
	}
	```
###	继承的工作细节
	@extend背后最基本的想法是，如果.seriousError @extend .error， 那么样式表中的任何一处.error都用.error.seriousError这一选择器组进行替换。
	@extend的优点
		跟混合器相比，继承生成的css代码相对更少。因为继承仅仅是重复选择器，而不会重复属性，所以使用继承往往比混合器生成的css体积更小。如果你非常关心你站点的速度，请牢记这一点。
		继承遵从css层叠的规则。当两个不同的css规则应用到同一个html元素上时，并且这两个不同的css规则对同一属性的修饰存在不同的值，css层叠规则会决定应用哪个样式。相当直观：通常权重更高的选择器胜出，如果权重相同，定义在后边的规则胜出。
###	使用继承的最佳实践
	通常使用继承会让你的css美观、整洁。因为继承只会在生成css时复制选择器，而不会复制大段的css属性。
	避免这种情况出现的最好方法就是不要在css规则中使用后代选择器（比如.foo .bar）去继承css规则。
##	小知识点
1.	占位符选择器 %foo (Placeholder Selectors: %foo)
2.	字符串 (Strings)
		有引号字符串 与 无引号字符串 (unquoted strings)
3.	数组 (Lists)
		nth 函数可以直接访问数组中的某一项；
		join 函数可以将多个数组连接在一起；
		append 函数可以在数组中添加新值；
		@each 指令能够遍历数组中的每一项。
4.	Maps
		$map: (key1: value1, key2: value2, key3: value3);
5.	颜色 (Colors)
6.	运算 (Operations)
	所有数据类型均支持相等运算 == 或 !=，此外，每种数据类型也有其各自支持的运算方式。
	SassScript 支持数字的加减乘除、取整等运算 (+, -, *, /, %)，如果必要会在不同单位间转换值。
	字符串运算 (String Operations)
		```
		p {
		  cursor: e + -resize;
		}
		```
7.	布尔运算 (Boolean Operations)
	SassScript 支持布尔型的 and or 以及 not 运算。
8.	圆括号 (Parentheses)
		width: 1em + (2em * 3);
9.	插值语句 #{} (Interpolation: #{})
		$attr: border;
		#{$attr}-color: blue;
10.	& in SassScript
		父选择器标识符
11.	变量定义 !default (Variable Defaults: !default)
12.	@import 并可以嵌套
		导入其他scss文件
13.	@extend
14.	控制指令 (Control Directives)
	@if 声明后面可以跟多个 @else if 声明，或者一个 @else 声明。
		@if 1 + 1 == 2 { border: 1px solid; }
		@if 5 < 3 { border: 2px dotted; }
	@for
		```
		@for $i from 1 through 3 {
		  .item-#{$i} { width: 2em * $i; }
		}
		```
	@each
		```
		@each $animal in puma, sea-slug, egret, salamander {
		  .#{$animal}-icon {
			background-image: url('/images/#{$animal}.png');
		  }
		}
		```
	@while
	
		```
		$i: 6;
		@while $i > 0 {
		  .item-#{$i} { width: 2em * $i; }
		  $i: $i - 2;
		}
		```
##	总结
	变量（声明及使用）：
		可以声明全局和局部变量。声明方法：$var-name
	嵌套CSS规则：
		article a {
		  color: blue;
		  &:hover { color: red } // 父选择器的标识符&，此时代表 a 元素
		}
	导入SASS文件：
		在一个scss文件同导入其他scss文件。导入方法：@import "sidebar";
	静默注释：
		在scss文件中有注释，编译之后注释被抹掉。注释方法为：// 
	混合器：
		混合器允许用户编写语义化样式的同时避免视觉层面上样式的重复。
		定义混合器使用@mixin标识符。
		使用混合器@include。		
	使用选择器继承来精简CSS：
		继承允许你声明类之间语义化的关系，通过这些关系可以保持你的css的整洁和可维护性。
		这个通过@extend语法实现。	
>	参考文档：
	[Sass中文网](https://www.sass.hk)

























