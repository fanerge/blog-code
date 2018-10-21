---
title: CSS技巧
date: 2018-06-13 22:29:35
tags: 'css'
categories: 'css'
copyright: true
---
本文总结日常CSS技巧，大多收集于网络、[《CSS揭秘》](http://www.ituring.com.cn/book/1695)
##	半透明边框
思路：默认 background 是从  border-box 裁切，我们可以通过 background-clip 属性来改变 background 裁切区域如 padding-box，在使用 rgba 或者 hsla 来指定边框颜色即可。 
```
.border {
	border: 10px solid hsla(0,0%,100%,.5);
	background: white;
	background-clip: padding-box;
}
```
[半透明边框]( play.csssecrets.io/translucent-borders)
PS：根据 stack context 层级关系 background 在 border 下层。
##	多重边框
###	box-shadow 方案
思路：首先要知道 box-shadow 的参数：vl hl blurl spreadl color inset，并且支持多重阴影。
[demo]( play.csssecrets.io/multiple-borders)
PS：box-shadow 不影响布局（不占用空间）、不受 box-sizing 的控制、不适用于增加热点区域。
###	outline 方案
只适用于两层边框。
PS：当 border 为圆角时，outline 不会贴合元素的圆角，需要使用 box-shadow 来填补。 
##	灵活的背景定位
###	background-position 的扩展语法方案
`background-position: right 20px bottom 10px;`
PS：背景定位于 right 的 20px处，bottom 的 10px处。
[bg-position](play.csssecrets.io/extended-bg-position)
###	background-origin 方案
background-origin 属性指定 background-position 属性应该是相对位置。
[background-origin](play.csssecrets.io/background-origin)
###	calc() 方案
`background-position: calc(100% - 20px) calc(100% - 10px);`
PS：需要注意的是，运算符前后都需要保留一个空格，例如：width: calc(100% - 10px)。
[background-position-calc](play.csssecrets.io/background-position-calc)
##	边框内圆角
我们知道box-shadow是会紧贴border-radius圆角边的，但是，描边outline并不会与圆角边border-radius贴合，我们可以将两者组合，通过box-shadow去填补描边outline所产生的间隙来达到我们想要的效果。
```
.box {
	border-radius: 8px;
    outline: 6px solid #b4a078;
    box-shadow: 0 0 0 5px #b4a078; // 用于填充 outline 不能紧靠 border-radius
}
```
[边框内圆角](https://lhammer.cn/You-need-to-know-css/#/inner-rounding)
##	条纹背景
###	横向条纹（默认横向渐变）
如形成三条间隔条纹背景。
`background: linear-gradient(#fb3 33.3%,#58a 0, #58a 66.6%, yellowgreen 0);`
PS：#58a 0 的作用为直接填充 > 33.3% 的部分。
[horizontal-stripes]( play.csssecrets.io/horizontal-stripes)
###	垂直条纹
```
background: linear-gradient(to right, /* 或 90deg */#fb3 50%, #58a 0);
background-size: 30px 100%;
```
[vertical-stripes](play.csssecrets.io/vertical-stripes)
###	斜向条纹
```
background: linear-gradient(45deg, #fb3 25%, #58a 0, #58a 50%, #fb3 0, #fb3 75%, #58a 0);
background-size: 42.426406871px 42.426406871px;
```
[diagonal-stripes](play.csssecrets.io/diagonal-stripes)
[diagonal-stripes-60deg](play.csssecrets.io/diagonal-stripes-60deg)
###	灵活的同色系条纹
```
background: #58a;
background-image: repeating-linear-gradient(30deg, hsla(0,0%,100%,.1), hsla(0,0%,100%,.1) 15px, transparent 0, transparent 30px);
```
[subtle-stripes]( play.csssecrets.io/subtle-stripes)
##	复杂的背景图案
[网格](play.csssecrets.io/blueprint)
[波点](play.csssecrets.io/polka)
[角向渐变](play.csssecrets.io/test-conic-gradient)
[css3patterns](http://lea.verou.me/css3patterns/)
[图案库](http://bennettfeely.com/gradients/)
##	连续的图像边框
设置多层背景，再通过 background-clip 来裁剪各层背景。
[图像边框](play.csssecrets.io/continuous-image-borders)
[信封背景]( play.csssecrets.io/vintage-envelope)
[蚂蚁行军边框](play.csssecrets.io/marching-ants)
[footnote]( play.csssecrets.io/footnote)
##	自适应的椭圆
前提为 width === height
圆形：`border-radius: 100px;`
椭圆：`border-radius: 100px / 75px;`
适应性椭圆：`border-radius: 50%;`
PS：50%; === 50% / 50%;分别为该元素的宽的50%和高的50%。
[适应性椭圆](play.csssecrets.io/ellipse)
适应性的半椭圆：`border-radius: 100% 0 0 100% / 50%;`
PS：上面的写法，分别为四个角设置。
[适应性的半椭圆](play.csssecrets.io/half-ellipse)
四分之一椭圆：`border-radius: 100% 0 0 0;`
PS：其中一个角的水平和垂直半径值都需要是100%，而其他三个角都不能设为圆角。
[quarter-ellipse](play.csssecrets.io/quarter-ellipse)
##	平行四边形
需求为让容器元素为平行四边形，但文本不可倾斜。
###	嵌套元素方案
[抵消策略]( play.csssecrets.io/parallelograms)
PS：对容器进行 skew() 变形，对内容再应用一次反向的 skew() 变形，从而抵消容器的变形效果
###	伪元素方案
[parallelograms-pseudo](play.csssecrets.io/parallelograms-pseudo)
PS：把所有样式（背景、边框等）应用到伪元素上，然后再对伪元素进行变形。
##	菱形图片
###	基于变形的方案
[diamond-images]( play.csssecrets.io/diamond-images)
PS：运用 rotate(-45deg) 再使用 scale(1.42) 填满菱形区域。
###	裁切路径方案
`clip-path: polygon(50% 0, 100% 50%, 50% 100%, 0 50%);`
[diamond-clip](play.csssecrets.io/diamond-clip)
##	切角效果
###	线性渐变
[线性渐变](play.csssecrets.io/bevel-corners-gradients)
###	弧形切角
[径向渐变](play.csssecrets.io/scoop-corners)
###	内联 SVG 与 border-image 方案
相关 SVG 支持，以背景形式引入 SVG。
[bevel-corners]( play.csssecrets.io/bevel-corners)
###	裁切路径方案
主要使用 clip-path 属性。
[bevel-corners-clipped](play.csssecrets.io/bevel-corners-clipped)
##	梯形标签页
需求为让容器元素为梯形，但文本不可倾斜。
```
transform: perspective(.5em) rotateX(5deg);
```
[trapezoid-tabs](play.csssecrets.io/trapezoid-tabs)
PS：把所有样式（背景、边框等）应用到伪元素上，然后再对伪元素进行变形。
##	简单的饼图
###	基于 transform 的解决方案
[pie-animated](play.csssecrets.io/pie-animated)
[pie-static](play.csssecrets.io/pie-static)
###	SVG 解决方案
[pie-svg](play.csssecrets.io/pie-svg)
##	box-shadow
box-shadow: hl vl blur spread color;
PS：hl为水平位置，vl为垂直位置，blur为模糊距离（模糊面积越大，阴影就越大越淡），spread为阴影大小（取正值时，阴影扩大；取负值时，阴影收缩）。	
###	单侧投影
`box-shadow: 0 5px 4px -4px black;`
[shadow-one-side](play.csssecrets.io/shadow-one-side)
PS：第四个参数会根据你指定的值去扩大或（当指定负值时）缩小投影的尺寸。
###	对边投影	
```
box-shadow: 5px 0 5px -5px black,
			-5px 0 5px -5px black;
```
[shadow-opposite-sides](play.csssecrets.io/shadow-opposite-sides)
PS：利用 box-shadow 可以设置多重阴影。
##	不规则投影
###	filter 之 drop-shadow()
[drop-shadow滤镜与box-shadow区别应用](https://www.zhangxinxu.com/wordpress/2016/05/css3-filter-drop-shadow-vs-box-shadow/)
[demo](https://www.zhangxinxu.com/study/201605/drop-shadow-point-to.html)
[demo1]( play.csssecrets.io/drop-shadow)
PS：drop-shadow 没有 inset，不能叠加，有颜色就会有阴影（不特定于盒模型）。
##	染色效果
###	基于滤镜的方案
[color-tint-filter](play.csssecrets.io/color-tint-filter)
[滤镜文档](http://www.runoob.com/cssref/css3-pr-filter.html)
###	基于混合模式的方案
background-blend-mode 属性定义了背景层的混合模式（图片与颜色）。
[color-tint](play.csssecrets.io/color-tint)
###	毛玻璃效果
[frosted-glass](play.csssecrets.io/frosted-glass)
PS：伪类来实现效果，原本元素放文本，就不会导致文本模糊。
##	折角效果
###	45°折角的解决方案
```
background: #58a; /* 回退样式 */
background: linear-gradient(to left bottom,
	transparent 50%, rgba(0,0,0,.4) 0)
	no-repeat 100% 0 / 2em 2em,
	linear-gradient(-135deg,
	transparent 1.5em, #58a 0);
```
[folded-corner](play.csssecrets.io/folded-corner)
###	其他角度的解决方案
[folded-corner-realistic](play.csssecrets.io/folded-corner-realistic)
[folded-corner-mixin](play.csssecrets.io/folded-corner-mixin)
##	连字符断行
CSS 属性 hyphens 告知浏览器在换行时如何使用连字符连接单词。可以完全阻止使用连字符，也可以控制浏览器什么时候使用，或者让浏览器决定什么时候使用。
[hyphenation](play.csssecrets.io/hyphenation)
##	插入换行
[line-breaks](play.csssecrets.io/line-breaks)
PS：有一个 Unicode 字符是专门代表换行符的： 0x000A ① 。在 CSS 中，这个字符可以写作 "\000A" ，或简化为 "\A" ，类似于 br 标签。
##	文本行的斑马条纹
```
padding: .5em;
line-height: 1.5;
background: beige;
background-size: auto 3em;
background-origin: content-box;
background-image: linear-gradient(rgba(0,0,0,.2) 50%,
transparent 0);
```
[zebra-lines](play.csssecrets.io/zebra-lines)
##	调整 tab 的宽度
tab-size 属性规定制表符（tab）字符的空格长度，只对 textarea 和 pre 有效。
[tab-size](play.csssecrets.io/tab-size)
##	连字
font-variant-ligatures
```
font-variant-ligatures: common-ligatures no-discretionary-ligatures no-historical-ligatures;
```
##	未来的文本下划线
text-decoration-color 用于自定义下划线或其他装饰效果的颜色。
text-decoration-style 用于定义装饰效果的风格（比如实线、虚线、波浪线等）。
text-decoration-skip 用于指定是否避让空格、字母降部或其他对象。
text-underline-position 用于微调下划线的具体摆放位置。
[underlines](play.csssecrets.io/underlines)
##	现实中的文字效果
[凸版印刷效果](play.csssecrets.io/letterpress)
[空心字效果](play.csssecrets.io/stroked-text)
[文字外发光效果](play.csssecrets.io/glow)
[文字凸起效果]( play.csssecrets.io/extruded)
##	环形文字
[SVG实现](play.csssecrets.io/circular-text)
##	选用合适的鼠标光标
###	提示禁用状态
```
:disabled, [disabled], [aria-disabled="true"] {
	cursor: not-allowed;
}
```
[disabled](play.csssecrets.io/disabled)
###	隐藏鼠标光标
适用于播放 video 等情形。
```
cursor: url('transparent.gif'); // 兼容低版本
cursor: none;
```
##	扩大可点击区域
###	Fitts 法则 或 菲茨定律 或 费茨法则
人机交互的一个法则
Fitts 法则认为，人类移动到某个目标区域所需的最短时间是由目标距离与目标宽度之比所构成的对数函数。
###	border 增加用户交互区域
```
border: 10px solid transparent;
box-shadow: 0 0 0 1px rgba(0,0,0,.3) inset;
background-clip: padding-box;
```
[hit-area-border](play.csssecrets.io/hit-area-border)
###	伪类增加用户交互区域
```
button {
	position: relative;
}
button::before {
	content: '';
	position: absolute;
	top: -10px; right: -10px;
	bottom: -10px; left: -10px;
}
```
[hit-area](play.csssecrets.io/hit-area)
##	自定义复选框
[checkboxes](play.csssecrets.io/checkboxes)
[toggle-buttons](play.csssecrets.io/toggle-buttons)
##	通过阴影来弱化背景
###	伪元素方案
```
body.dimmed::before {
	position: fixed;
	top: 0;
	right: 0;
	bottom: 0;
	left: 0;
	z-index: 1;
	background: rgba(0,0,0,.8);
}
```
###	box-shadow 方案
```
box-shadow: 0 0 0 50vmax rgba(0,0,0,.8);
```
[box-shadow](box-shadow: 0 0 0 50vmax rgba(0,0,0,.8);)
###	backdrop 方案
[backdrop](play.csssecrets.io/native-modal)
PS：dialog 元素（ <dialog> 元素可以由它的 showModal() 方法显示出来），那么根据浏览器的默认样式，它会自带一个遮罩层（ ::backdrop 伪元素）。
##	通过模糊来弱化背景
```
main.de-emphasized {
	filter: blur(3px) contrast(.8) brightness(.8);
}
```
[滤镜效果](play.csssecrets.io/deemphasizing-blur)
##	滚动提示
[上下滚动](play.csssecrets.io/scrolling-hints)
##	交互式的图片对比控件
[image-slider](play.csssecrets.io/image-slider)
##	自适应内部元素
width 新添了一些属性，如min-content。
```
figure {
	max-width: 300px;
	max-width: min-content;
	margin: auto;
}
figure > img { max-width: inherit; }
```
[intrinsic-sizing](play.csssecrets.io/intrinsic-sizing)
##	精确控制表格列宽
```
table {
	table-layout: fixed;
	width: 100%;
}
```
[table-column-widths](play.csssecrets.io/table-column-widths)
##	根据兄弟元素的数量来设置样式
###	相当于li:only-child
li:first-child:nth-last-child(1)
###	当列表正好包含四项时，命中所有列表项
li:first-child:nth-last-child(4),
li:first-child:nth-last-child(4) ~ li
###	当列表至少包含四项时，命中所有列表项
li:first-child:nth-last-child(n+4),
li:first-child:nth-last-child(n+4) ~ li
###	当列表最多包含四项时，命中所有列表项
li:first-child:nth-last-child(-n+4),
li:first-child:nth-last-child(-n+4) ~ li 
###	当列表包含2～6项时，命中所有列表项
li:first-child:nth-last-child(n+2):nth-last-child(-n+6),
li:first-child:nth-last-child(n+2):nth-last-child(-n+6) ~ li
[styling-sibling-count](play.csssecrets.io/styling-sibling-count)
##	满幅的背景，定宽的内容
[fluid-fixed](play.csssecrets.io/fluid-fixed)
##	垂直居中
###	基于绝对定位的解决方案
```
main {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
}
```
###	基于视口单位的解决方案
```
main {
	width: 18em;
	padding: 1em 1.5em;
	margin: 50vh auto 0; // 这里不能使用50%，详细请了解包含快知识
	transform: translateY(-50%);
}
```
###	基于 Flexbox 的解决方案
```
.container {
	display: flex;
}
main {
	margin: auto; // 水平和垂直都可以居中
}
```
###	基于 Flexbox 的解决方案（匿名容器）
PS：匿名容器即为没有被标签包裹的文本节点
```
.container {
	display: flex;
	align-items: center;
	justify-content: center;
}
```
##	紧贴底部的页脚
下列 header、main、footer 为 body 的子元素。
###	固定高度的解决方案
```
main {
	min-height: calc(100vh - footerHeight);
	/* 避免内边距或边框搞乱高度的计算： */
	box-sizing: border-box;
}
```
###	flex的解决方案
```
body {
	display: flex;
	flex-flow: column;
	min-height: 100vh;
}
main { 
	// 自动伸展并占满所有的可用空间
	flex: 1; 
}
```
##	 缓动效果（动画和过渡）
[回弹动画](play.csssecrets.io/bounce)
[弹性过渡](play.csssecrets.io/elastic)
PS：对颜色过渡时小心，RGB 三个通道的值是独立进行插值运算的，在过渡过程中会产生其他颜色。
一般通过 transition-property 设置指定过渡属性来避免。
###	逐帧动画
运动曲线适用性：贝塞尔曲线适用于平滑运动， steps(步进数, [start || end])适用于逐帧动画
PS：参数一为步进数（把动画分为多少步，然后在逐步运行），参数二用于指定动画在每个循环周期的什么位置发生帧的切换动作。
[逐帧动画]( play.csssecrets.io/frame-by-frame)
###	闪烁效果
animation-direction 属性定义是否循环交替反向播放动画。
reverse	动画反向播放。	
alternate	动画在奇数次（1、3、5...）正向播放，在偶数次（2、4、6...）反向播放。	
alternate-reverse	动画在奇数次（1、3、5...）反向播放，在偶数次（2、4、6...）正向播放。
[闪烁效果]( play.csssecrets.io/blink)
###	打字动画
[打字动画](play.csssecrets.io/typing)
###	状态平滑的动画
animation--play-state 属性指定动画是否正在运行或已暂停。
paused	指定暂停动画
running	指定正在运行的动画
[指定暂停动画](play.csssecrets.io/state-animations)
##	::first-letter
定义：::first-letter会选中某 block-level element（块级元素）第一行的第一个字母，并且文字所处的行之前没有其他内容（如图片和内联的表格） 。
1.	::first-letter 伪元素生效的前提，常见的标点符号、各类括号和引号在::first-letter 伪元素眼中全部都是“辅助类”字符，不会作为第一个字符计算。
2.	与::before使用（::before 若有字符，会参与计算及 伪类的 content 的字符会被::first-letter生效）。
3.	::first-letter 伪元素可以生效的 CSS 属性有：字体属性、背景属性、color、padding、border、margin等。

PS：“辅助类”包括·@#%&*()（）[]【】{}:："“”;；'‘’》《,，.。？?!！…*、/\。
[iScroll, smooth scrolling for the web](https://github.com/cubiq/iscroll/)
##	CJK（中文/日文/韩文）两端对齐
text-align: justify;
text-justify: inter-ideograph;
##	用户交互反馈（通用的按钮及连接交互反馈）
通用的连接和按钮交互反馈，原理为：background-color 总是在最底下的位置，所以这里的 background-image 一定是覆盖在按钮等元素背景色之上的，不会影响按钮原来的背景色。
```
a[href]:active, button:active {
	background-image: linear-gradient(to top, rgba(0,0,0,.05), rgba(0,0,0,.05));
}
```
##	元素的显示与隐藏	
###	script 标签
如果希望元素不可见，同时不占据空间，辅助设备无法访问，同时不渲染，不进行资源加载。
```
<script type="text/html">
	<img src="1.jpg">
</script>
```
PS：script标签隐藏内容获取使用 script.innerHTML
###	 display:none 隐藏
如果希望元素不可见，同时不占据空间，辅助设备无法访问，但资源有加载，DOM 可访问。
```
.dn {
	display: none;
}
```
###	visibility: hidden 隐藏
如果希望元素不可见，同时不占据空间，辅助设备无法访问，但显隐的时候可以有 transition 淡入淡出效果。
```
.hidden {
	position: absolute;
	visibility: hidden;
}
```
如果希望元素不可见，不能点击，辅助设备无法访问，但占据空间保留。
```
.hidden {
	visibility: hidden;
}
```
###	 clip 剪裁隐藏
如果希望元素不可见，不能点击，不占据空间，但键盘可访问。
```
.clip {
	position: absolute;
	clip: rect(0 0 0 0);
}
.out {
	position: relative;
	left: -999em;
}
```
###	 relative 隐藏
如果希望元素不可见，不能点击，但占据空间，且键盘可访问。
```
.lower {
	position: relative;
	z-index: -1;
}
```
###	透明度隐藏
如果希望元素不可见，但可以点击，而且不占据空间，则可以使用透明度。
```
.opacity {
	position: absolute;
	opacity: 0;
	filter: Alpha(opacity=0);
}
```
如果单纯希望元素看不见，但位置保留，依然可以点可以选，则直接让透明度为 0。
```
.opacity {
	opacity: 0;
	filter: Alpha(opacity=0);
}
```
PS：img元素，设置 display:none 在所有浏览器下依旧都会请求图片资源（浪费了宽带）。
###	流向的改
direction（改变水平流向）
unicode-bidi（文字流向）
writing-mode（改变 CSS 世界纵横规则）
writing-mode: lr-tb | tb-rl | tb-lr (IE8+);
writing-mode: horizontal-tb | vertical-rl | vertical-lr;
##	1px边框（移动端）
###	box-shadow/border + transform
```
.border {
	width: 100%;
	position: relative;
}
.border::after {
	content:'';
	position: absolute;
	bottom: 0;left: 0;
	width: 100%;
	box-shadow: 0 0 0 1px red;
	transform-origin: 0 bottom;
	transform: scaleY(.5);
}
```
PS：还可以结合 @media (min-resolution: xdppx)做进一步处理。

##	background
background-origin: padding-box; 默认
background-position: top 20px left 20px; 参照点为默认padding-box
background-size: x y;
圆锥渐变
background: conic-gradient(red, yellow, lime, aqua, blue, fuchsia, red);
background缩写语法
background:bg-color bg-image position/bg-size bg-repeat bg-origin bg-clip bg-attachment initial|inherit;
##	 border-radius
1.	可以通过'/'设置水平水平和垂直半径，如`border-radius: 100px / 75px;`
2.	它不仅可以接受长度值，还可以接受百分比值（这个百分比值会基于元素的尺寸进行解析，即宽度用于水平半径的解析，而高度用于垂直半径的解析。）。
3.	

##	white-space
指定元素内的空白怎样处理。
normal	默认。空白会被浏览器忽略。
pre	空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。
nowrap	文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。
pre-wrap	保留空白符序列，但是正常地进行换行。
pre-line	合并空白符序列，但是保留换行符。
inherit	规定应该从父元素继承 white-space 属性的值。
##	other
line-height 的百分比时相对于 font-size 计算的。
vertical-align 的百分比时相对于 line-height 计算的。
ex 一个 ex 是一个字体的 x-height。 (x-height 通常是字体尺寸的一半。) 
ch 数字 0 的宽度，使用场景（需要配合等宽字体）：全数字输入框：手机号等。
font-weight 运行原理：字体不同粗细需要字体文件是否存在该粗细的字体。
font-style  同样有效的前提为字体文件中存在该类型的字体，italic 和 oblique。
font-family: system-ui; // 让网页的字体跟系统走，，网站字体能时时刻刻与时俱进。
text-transform 属性控制文本的大小写，支持capitalize、uppercase、lowercase。
适用场景：身份证输入，最后以为X，帮助用户转为大写；验证码输入，帮助用户转为大写。
::backdrop CSS 伪元素 是在任何处于全屏模式的元素下的即刻渲染的盒子（并且在所有其他在堆中的层级更低的元素之上）。















