---
title: html5-svg开发手册
date: 2018-01-30 20:28:14
tags: 'svg'
keywords: 'svg'
categories: 'html5'
copyright: true
---
#  基础
##	svg的使用
1.  img的src
2.	background-image: url()
3.	object: data
4.	embed: src
5.	foreignObject
foreignObject元素允许包含外来的XML命名空间，其图形内容是别的用户代理绘制的。
##	样式的写法（优先级逐渐降低）
1.内联style
`<g style="fill: red;"></g>`
2.class
```
<g class="font"></g>
<style type="text/css"><![CDATA[
.font {
	fill: red;
}
]]></style>
```
3.外链样式表
`<?xml-stylesheet href="style.css" type="text\css" ?>`
4.样式属性
`<g fill="red"></g>`
##	other
```
<![CDATA[解析器忽略的内容]]>
作用：xml解析器忽略解析，将表示为纯文本。
```
默认用户坐标（视口svg的width和height）
指定用户坐标（viewBox属性）
保持宽高比（SVG的宽高和viewBox的宽高比是不一样），使用preserveAspectRatio属性
preserveAspectRatio="xMidYMid meet"
第1个值表示，viewBox如何与SVG viewport对齐；第2个值表示，如何维持高宽比（如果有）。

#	属性
##	全局属性
class、style
##	样式属性
color、display、opacity、overflow
fill、fill-opacity、fill-rule
stroke、stroke-dasharray、stroke-dashoffset、stroke-linecap、stroke-linejoin、stroke-miterlimit、stroke-opacity、stroke-width
##	动画事件属性
onbegin, onend, onload, onrepeat
##	动画属性目标属性
attributeType, attributeName
##	动画定时属性
begin, dur, end, min, max, restart, repeatCount, repeatDur, fill
##	动画值属性
calcMode, values, keyTimes, keySplines, from, to, by, autoReverse, accelerate, decelerate
##	动画累加属性
additive, accumulate
##	条件处理属性
requiredExtensions, requiredFeatures, systemLanguage.
##	核心属性
id, xml:base, xml:lang, xml:space
##	文档事件属性
onabort, onerror, onresize, onscroll, onunload, onzoom
##	过滤器原始属性
height, result, width, x, y
##	图形事件属性
onactivate, onclick, onfocusin, onfocusout, onload, onmousedown, onmousemove, onmouseout, onmouseover, onmouseup

#	svg 元素
##	基本形状元素
###	line
line元素是一个SVG基本形状，用来创建一条连接两个点的线。
属性：x1、y1、x2、y2
`<line x1="0" y1="0" x2="5" y2="5"></line>`
###	rect	
用来创建矩形，基于一个角位置以及它的宽和高。它还可以用来创建圆角矩形。
属性：x、y、width、height、rx、ry
`<rect x="0" y="0" width="10" height="10" rx="3" ry="3"></rect>`
###	circle
用来创建圆,基于一个圆心和一个半径。
属性：cx、cy、r
`<circle cx="10" cy="10" r="5"></circle>`
###	ellipse
用来创建一个椭圆，基于一个中心坐标以及它们的x半径和y半径。
利用transform属性椭圆的倾斜。
属性：cx、cy、rx、ry
`<ellipse cx="60" cy="60" rx="50" ry="25"/>`
###	polygon
polygon元素定义了一个由一组点左边的构成的闭合多边形形状。
属性：points
`<polygon points="100,40 100,80 60,100 20,80 20,40"/>`
###	polyline
用来创建一系列直线连接多个点。典型的一个polyline是用来创建一个开放的形状，最后一点不与第一点相连。
属性：points
`<polyline fill="none" stroke="black" points="20,100 40,60 70,80 100,20"/>`
###	path
path元素是用来定义形状的通用元素。所有的基本形状都可以用path元素来创建。
属性：d（data）、pathLength
PS：d属性有下列值（大写字母为绝对坐标，小写字母为相对坐标）
M（m）-x，y-移动到给定坐标。
L（l）-x，y-绘制一条到给定坐标的线，可以提供多组坐标来绘制折线。
H（h）-x-绘制一条到给定x坐标的水平线。
V（v）-y-绘制一条到指定y坐标的竖线。
A（a）-rx ry x-axis-rotation large-arc-flag sweep-flag x y。
Q（q）-x1，y1，x，y-绘制一条从当前点到（x，y），控制点为（x1，y1）的二次贝塞尔曲线。
T（t）-x，y-绘制一条从当前点到（x，y）的二次贝塞尔曲线，控制点是前一个Q命令的控制点的中心对称点。如果没有前一条曲线，当前点会被用作控制点。
C（c）-x1，y1，x2，y2，x，y-绘制一条从当前点到（x，y）的三次贝塞尔曲线，x1和x2分别为开始和终点控制点。
S（s）-x2，y2，x，y-绘制一条从当前点到（x，y）的三次贝塞尔曲线，使用x2作为终点控制点，开始控制点为前一个C命令的终点控制点的中心对称点。
`<path d="M 100 100 L 300 100 L 200 300 z" />`
PS：x-axis-rotation为x轴旋转角度，large-arc-flag为角度大小，sweep-flag（弧线方向）
large-arc-flag决定弧线是大于还是小于180度，0表示小角度弧，1表示大角度弧。
sweep-flag表示弧线的方向，0表示从起点到终点沿逆时针画弧，1表示从起点到终点沿顺时针画弧。



##	功能元素
###	title（提升可访问性）
SVG绘图中的每个窗口元素或图形元素都可以提供一个title描述性字符串，该描述只能是纯文本。
title元素必须是它的父元素的第一个子元素。
```
<g>
   <title>SVG Title Demo example</title>
   <rect x="10" y="10" width="200" height="50" style="fill:none; stroke:blue; stroke-width:1px"/>
	<desc>提升可访问性</desc>
</g>
```
###	desc（提升可访问性）
SVG绘画中的每个容器元素或图形元素都可以提供一个desc描述性字符串，这些描述只是纯文本的。
###	defs
SVG 允许我们定义以后需要重复使用的图形元素， 建议把所有需要再次使用的引用元素定义在defs元素里面。
需要使用\<use\>元素来呈现defs定义的元素。
```
<defs>
	<linearGradient id="Gradient01">
		<stop offset="20%" stop-color="#39F" />
		<stop offset="90%" stop-color="#F3F" />
	</linearGradient>
</defs>
<rect x="10" y="10" width="60" height="10" fill="url(#Gradient01)"  />
PS：radialGradient、linearGradient、pattern等元素必须要放在defs元素中
```

###	use
use元素在SVG文档内取得目标节点，并在别的地方复制它们。
属性：x、y、width、height、href
```
<defs>
    <g id="Port">
      <circle style="fill: inherit;" r="10"/>
    </g>
</defs>
<use x="50" y="10" href="#Port" />
<use x="50" y="30" href="#Port" class="classA"/>
```
###	a
使用 SVG 的锚元素\<a\>定义一个超链接。
属性：href、target
```
<a href="www.alipay.com" target="_blank">
    <rect height="30" width="120" y="0" x="0" rx="15"/>
    <text fill="white" text-anchor="middle" y="21" x="60">SVG on MDN</text>
</a>
```
###	clipPath
clipPath用于指定可绘制区域（超出了剪切路径所指定的区域，将不会被绘制。）。
属性：clipPathUnits="userSpaceOnUse'或'objectBoundingBox"。第二个值childern一个对象的边框，会使用掩码的一小部分单位（默认："userSpaceOnUse"）"
```
<defs>
    <clipPath id="myClip">
      <circle cx="30" cy="30" r="20"/>
    </clipPath>
</defs>
<rect x="10" y="10" width="100" height="100" clip-path="url(#myClip)"/>
```
PS：其他元素通过clip-path="引用剪贴路径和引用剪贴路径交叉"
###	color-profile
该元素允许描述用于图像的颜色配置文件。
属性：local、name、rendering-intent、href
###	foreignObject
foreignObject元素允许包含外来的XML命名空间，其图形内容是别的用户代理绘制的。这个被包含的外来图形内容服从SVG变形和合成。
foreignObject元素通常与 switch 元素和requiredExtensions属性联用，来做兼容。
###	image
SVG文档中的SVG元素包含图像信息。它表现为图像文件或者其他SVG文件。
属性：x、y、width、height、href、preserveAspectRatio（控制图像比例）
###	linearGradient
linearGradient元素用来定义线性渐变，用于图形元素的填充或描边。
```
<defs>
    <linearGradient id="MyGradient">
        <stop offset="5%"  stop-color="green"/>
        <stop offset="95%" stop-color="gold"/>
    </linearGradient>
</defs>
<rect fill="url(#MyGradient)" x="10" y="10" width="100" height="100"/>
```
属性：gradientUnits、gradientTransform、x1、y1、x2、y2、spreadMethod、href
###	radialGradient
radialGradient用来定义径向，用于图形元素的填充或描边。
属性：gradientUnits、gradientTransform、cx、cy、r、fx、fy、spreadMethod、href
```
<defs>
    <radialGradient id="exampleGradient">
      <stop offset="10%" stop-color="gold"/>
      <stop offset="95%" stop-color="green"/>
    </radialGradient>
</defs>
<circle fill="url(#exampleGradient)" cx="60" cy="60" r="50"/>
```
###	stop
一个渐变上的颜色坡度，是用stop元素定义的。
stop元素可以是linearGradient、radialGradient的子元素。


###	marker
marker元素定义了在特定的path、line、polyline、polygon上绘制的箭头或者多边标记图形。
属性：marker-end、marker-mid、marker-start、markerUnits、refx、refy、markerWidth、markerHeight、orient
```
<defs>
    <marker id="Triangle" viewBox="0 0 10 10" refX="1" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" />
    </marker>
</defs>
<polyline points="10,90 50,80 90,20" fill="none" stroke="black" stroke-width="2" marker-end="url(#Triangle)" />
```
###	mask
在SVG中，你可以指一个透明的遮罩层和当前对象合成，形成背景。属性mask用来引用一个遮罩元素。
属性：maskUnits、maskContentUnits、x、y、width、height
###	metadata
metadata是数据的结构化数据。
###	pattern
使用预定义的图形对一个对象进行填充或描边，就要用到pattern元素，在下轴或y轴上重复。
```
<defs>
    <pattern id="Triangle" width="10" height="10" patternUnits="userSpaceOnUse">
		<polygon points="5,0 10,10 0,10"/>
	</pattern>
</defs>
<circle cx="60" cy="60" r="50" fill="url(#Triangle)"/>
```
###	text
text元素定义了一个由文字组成的图形。注意：我们可以将渐变、图案、剪切路径、遮罩或者滤镜应用到text上。
属性：x、y、dx、dy、text-anchor、rotate、textLength、lengthAdjust
```
<text x="0" y="20" transform="rotate(30 20,40)">
    SVG Text Rotation example
</text>
```
###	textPath
textPath使用path来展示文字。
属性：startOffset、method、spacing、href
```
<path id="MyPath" fill="none" stroke="red" d="M10,90 Q90,90 90,45 Q90,10 50,10 Q10,10 10,40 Q10,70 45,70 Q70,70 75,50" />
<text>
	<textPath href="#MyPath">
	  The quick brown fox jumps over the lazy dog.
	</textPath>
</text>
```
###	tspan
在text元素中，利用内含的tspan元素，可以调整文本和字体的属性以及当前文本的位置、绝对或相对坐标值。
属性：x、y、dx、dy、rotate、textLength、lengthAdjust
```
<text x="15" y="30">
    You are 
    <tspan>not</tspan> 
    a banana
</text>
```
###	view
view元素是查看图片的一个限定方法，就像一个缩放级别或者一个详细视图。
属性：viewBox、preserveAspectRatio、zoomAndPan、viewTarget



##	不显示元素
###	g
元素g是用来组合对象的容器，对与transform、属性会作用与子元素。
通过\<use\>	元素来实现组合对象的复制。
```
<g stroke="green" fill="white" stroke-width="5">
    <circle cx="25" cy="25" r="15" />
    <circle cx="40" cy="25" r="15" />
</g>
```
###	script
一个SVG脚本元素等同于HTML中的script元素。
###	set
set元素可以用来设定一个属性值，并为该值赋予一个持续时间。
###	style
style元素元素样式表直接在SVG内容中间嵌入。
###	svg
如果svg不是根元素，svg 元素可以用于在当前文档内嵌套一个独立的svg片段 。
这个独立片段拥有独立的视口和坐标系统。
属性：version、baseProfile、x、y、width、height、preserveAspectRatio、contentScriptType、contentStyleType、viewBox
###	switch
switch元素对它的直接子元素上的属性requiredFeatures、属性requiredExtensions 和 属性systemLanguage按照顺序进行评估，然后处理和呈现第一个评估为true的子元素。
属性：allowReorder
```
<switch>
	<text systemLanguage="ar">مرحبا</text>
	<text systemLanguage="ja">こんにちは</text>
	<text>☺</text>
   </switch>
```
###	symbol
symbol元素用来定义一个图形模板对象，它可以用一个use元素实例化。
symbol元素对图形的作用是在同一文档中多次使用，添加结构和语义。
属性：preserveAspectRatio、viewBox
```
<symbol id="sym01" viewBox="0 0 150 110">
	<circle cx="50" cy="50" r="40" stroke-width="8" stroke="red" fill="red"/>
	<circle cx="90" cy="60" r="40" stroke-width="8" stroke="green" fill="white"/>
</symbol>
<!-- actual drawing by "use" element -->
<use href="#sym01" x="0" y="0" width="100" height="50"/>
<use href="#sym01" x="0" y="50" width="75" height="38"/>
```

##	动画元素
###	animate
动画元素放在形状元素的内部，用来定义一个元素的某个属性根据时间点如何改变。
属性：attributeName、attributeType（CSS/XML）、from、to、dur、repeatCount
```
<rect x="10" y="10" width="100" height="100">
    <animate attributeType="XML" attributeName="x" from="-100" to="120" dur="10s" repeatCount="indefinite"/>
</rect>
```
###	animateMotion
animateMotion元素导致引用的元素沿着运动路径移动。
属性：calcMode、path、keyPoints、rotate、origin
```
<path d="M10,110 A120,120 -45 0,1 110 10 A120,120 -45 0,1 10,110" stroke="lightgrey" stroke-width="2"  fill="none" id="theMotionPath"/>
<circle cx="" cy="" r="5" fill="red">
    <animateMotion dur="6s" repeatCount="indefinite">
		<mpath xlink:href="#theMotionPath"/>
    </animateMotion>
</circle>
```
###	mpath
animateMotion元素的 mpath 子元素使 animateMotion 元素能够引用一个外部的 path 元素作为运动路径的定义。
属性：href
###	animateTransform
animateTransform元素变动了目标元素上的一个变形属性，从而允许动画控制转换、缩放、旋转或斜切。
属性：by、from、to、type
```
<polygon points="60,30 90,90 30,90">
    <animateTransform attributeName="transform"
		attributeType="XML"
		type="rotate"
		from="0 60 70"
		to="360 60 70"
		dur="10s"
		repeatCount="indefinite"/>
</polygon>
```
##	滤镜元素
###	通用属性
x、y、width、height属性置顶应用滤镜的画布的尺寸。
filterUnits指定用来定义滤镜范围的单位。
primitiveUnits为滤镜基元中的各种长度值指定坐标系统。
###  filter
filter元素作用是作为原子滤镜操作的容器。它不能直接呈现。可以利用目标SVG元素上的filter属性引用一个滤镜。
属性：x、y、width、height、filterRes、filterUnits、primitiveUnits、href
```
<filter id="blurMe">
	<feGaussianBlur in="SourceGraphic" stdDeviation="5"/>
</filter>
<circle cx="60"  cy="60" r="50" fill="green" />
<circle cx="170" cy="60" r="50" fill="green" filter="url(#blurMe)" />
```
###	feBlend
feBlend滤镜把两个对象组合在一起，使它们受特定的混合模式控制。这类似于图像编辑软件中混合两个图层。该模式由属性mode定义。
属性：in、in2、mode
```
<defs>
    <filter id="spotlight">
		<feFlood result="floodFill" x="0" y="0" width="100%" height="100%" flood-color="green" flood-opacity="1"/>
		<feBlend in="SourceGraphic" in2="floodFill" mode="multiply"/>
    </filter>
</defs>
<image xlink:href="/files/6457/mdn_logo_only_color.png" x="10%" y="10%" width="80%" height="80%" style="filter:url(#spotlight);"/>
```
###	feColorMatrix（颜色转换滤镜）
该滤镜基于转换矩阵对颜色进行变换。每一像素的颜色值(一个表示为[R,G,B,A] 的矢量)都经过矩阵乘法计算出的新颜色。
属性：in、type、values
###	feComponentTransfer
SVG滤镜基元对每个像素执行颜色分量的数据重映射.它允许进行像亮度调整,对比度调整,色彩平衡或阈值的操作。
属性：in
###	feFuncR
该滤镜为它的父<feComponentTransfer>元素的输入图形的红色成分定义了变换函数。
属性：type、tableValues、slope、intercept、amplitude、exponent、offset
###	feFuncG
该滤镜为它的父feComponentTransfer元素的输入图形的绿色成分定义了变换函数。
###	feFuncB
该滤镜为它的父feComponentTransfer元素的输入图形的蓝色成分定义了变换函数。
###	feFuncA
该滤镜为它的父feComponentTransfer元素的输入图形的alpha成分定义了变换函数。
###	feComposite（合成滤镜）
该滤镜执行两个输入图像的智能像素组合，在图像空间中使用以下Porter-Duff合成操作之一：over、in、atop、xor
属性：in、in2、operator、k1、k2、k3、k4
###	feConvolveMatrix
feConvolveMatrix元素应用了一个矩阵卷积滤镜效果。一个卷积在输入图像中把像素与邻近像素组合起来制作出结果图像。
属性：in、order、kernelMatrix、divisor、bias、targetX、targetY、edgeMode、kernelUnitLength、preserveAlpha
###	feDiffuseLighting（散开照明滤镜）
滤镜光照一个图像，使用alpha通道作为隆起映射。
属性：in、surfaceScale、diffuseConstant、kernelUnitLength
###	feDisplacementMap
映射置换滤镜，该滤镜用来自图像中从in2到空间的像素值置换图像从in到空间的像素值。
属性：in、in2、scale、xChannelSelector、yChannelSelector
###	feDistantLight（平行光滤镜）
该滤镜定义了一个距离光源，可以用在灯光滤镜feDiffuseLighting元素或feSpecularLighting元素的内部。
属性：azimuth、elevation
###	feFlood
该滤镜用flood-color元素定义的颜色和flood-opacity元素定义的不透明度填充了滤镜子区域。
属性：flood-color、flood-opacity
###	feGaussianBlur（高斯模糊滤镜）
该滤镜对输入图像进行高斯模糊，属性stdDeviation中指定的数量定义了钟形。
属性：in、stdDeviation
```
<filter id="blurMe">
    <feGaussianBlur in="SourceGraphic" stdDeviation="5" />
</filter>
<circle cx="170" cy="60" r="50" fill="green" filter="url(#blurMe)" />
```
###	feImage（图片滤镜）
feImage滤镜从外部来源取得图像数据，并提供像素数据作为输出（意味着如果外部来源是一个SVG图像，这个图像将被栅格化。）
属性：preserveAspectRatio、href
###	feMerge（合并滤镜）
feMerge滤镜允许同时应用滤镜效果而不是按顺序应用滤镜效果。利用result存储别的滤镜的输出可以实现这一点，然后在一个feMergeNode子元素中访问它。
```
<filter id="feOffset" x="-40" y="-20" width="100" height="200">
    <feMerge>
		<feMergeNode in="blur2" />
		<feMergeNode in="SourceGraphic" />
    </feMerge>
</filter>
<rect x="40" y="40" width="100" height="100" style="stroke: #000000; fill: green; filter: url(#feOffset);" />
```
###	feMergeNode
feMergeNode元素拿另一个滤镜的结果，让它的父feMerge元素处理。
属性：in
###	feMorphology（扩张滤镜）
该滤镜用来侵蚀或扩张输入的图像。它在增肥或瘦身效果方面特别有用。
属性：in、operator、radius
###	feOffset（位移滤镜）
该输入图像作为一个整体，在属性dx和属性dy的值指定了它的偏移量。
属性：in、dx、dy
```
<filter id="offset" width="180" height="180">
    <feOffset in="SourceGraphic" dx="60" dy="60" />
</filter>
<rect x="0" y="0" width="100" height="100" stroke="black" fill="green" filter="url(#offset)"/>
```
###	fePointLight（点光源滤镜）
SVG创建一个点光源效果。
属性：x、y、z
###	feSpecularLighting（镜子照明滤镜）
该滤镜照亮一个源图形，使用alpha通道作为隆起映射。
属性：in、surfaceScale、specularConstant、specularExponent、kernelUnitLength
###	feSpotLight（斑点照明滤镜）
feSpotLight元素是一种光源元素，用于SVG文件。
属性：x、y、z、pointsAtX、pointsAtY、pointsAtZ、specularExponent、limitingConeAngle
###	feTile（平铺滤镜）
输入图像是平铺的，结果用来填充目标。它的效果近似于一个pattern图案对象。
属性：in
###	feTurbulence
该滤镜利用Perlin噪声函数创建了一个图像。它实现了人造纹理比如说云纹、大理石纹的合成。
属性：baseFrequency、numOctaves、seed、stitchTiles、type

>	参考文档：
[MDN-SVG系列资料](https://developer.mozilla.org/zh-CN/docs/Web/SVG)
[svg-属性参考](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute#Transfer_function_attributes)
[掘金翻译-深入浅出 SVG](https://github.com/xitu/gold-miner/blob/master/TODO1/an-in-depth-svg-tutorial.md)