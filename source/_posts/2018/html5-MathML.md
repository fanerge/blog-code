---
title: MathML
date: 2018-01-22 20:44:44
tags: 'html5'
categories: 'html5'
copyright: true
---
#	介绍
Mathematical Markup Language (MathML) 是一个用于描述数学公式、符号的一种 XML 标记语言。
MathML 是一个用于标记数学表达式的 XML 词汇表，它包含两个子语言：Presentation MathML 和 Content MathML。
Presentation MathML 主要负责描述数学表达式的布局（因此可与 TeX 或更早的 SGML 标记语言相比较，SGML 用于描述诸如 ISO 12083 之类格式的数学表达式的布局）。
Content MathML 主要负责标记表达式的某些含义或数学结构。MathML 的这一方面受到 OpenMath 语言的很大影响，在 MathML3 中，与 OpenMath 更为贴近。
##	示例
```
<math xmlns="http://www.w3.org/1998/Math/MathML">
	<mrow>
		<msup><mi>a</mi><mn>2</mn></msup>
		<mo>+</mo>	
		<msup><mi>b</mi><mn>2</mn></msup>
		<mo>=</mo>
		<msup><mi>c</mi><mn>2</mn></msup>
	</mrow>
</math>
```
![demo](http://www.runoob.com/wp-content/uploads/2015/12/mathml1.jpg)
#	MathML 元素
这是一份关于 MathML 呈现型元素的、按字母表排序的清单。
MathML元素的细节和在桌面浏览器与移动设备浏览器上的兼容性情况。
##	math
`<math>  （顶层元素）`
##	A
```
<maction>   （绑定动作到子表达式）
<maligngroup> （对齐分组）
<malignmark>  （对齐点）
```
##	E
```
<menclose> （包含的内容）
<merror> （包含的语法错误消息）
```
##	F
```
<mfenced> （圆括号）
<mfrac> （因子）
```
##	G
`<mglyph> （显示非标准符号）`
###	I
`<mi> （标识符）`
##	L
```
<mlabeledtr>（表格或矩阵中的行标签）
<mlongdiv>（长除法记号）
```
##	M
`<mmultiscripts> （惯例和张量指标）`
##	N
`<mn> （数量）`
##	O
```⁤⁤⁤
<mo> （运算符）
<mover> （上标）
```
##	P
```
<mpadded>（内容周围的填充空间）
<mphantom> （预留空间的不可见内容）
```
##	R
```
<mroot> （带指定根数的根号）
<mrow> （分组后的子表达式）
```
##	S
```
<ms> （字符串字面量）
<mscarries> （诸如进位的附注）
<mscarry> （单位进位， 
<mscarries>的子元素）
<msgroup> （在 <mstack> 和 
<mlongdiv>元素中分组后的若干行）
<msline> （在 <mstack> 内部的水平行）
<mspace> （空格）
<msqrt> （不带根数的平方根）
<msrow> （在<mstack>元素中的行）
<mstack> （堆叠式对齐）
<mstyle> （样式变更）
<msub> （下角标）
<msup> （上角标）
<msubsup> （上下角标对）
```
##	T
```
<mtable> （表格或矩阵）
<mtd> （表格或矩阵中的单元格）
<mtext> （文本）
<mtr> （表格或矩阵中的行）
```
##	U
```
<munder> （下标）
<munderover> （上标-下标对）
```
##	other
```
<semantics> （语义附注的容器）
<annotation> （数据附注）
<annotation-xml>  （XML 附注）
```
#	MathML 属性
关于MathML属性的参考文档。用这些属性可以修改这些元素的显示效果。
<iframe heigth="50vh" width="100%" src="https://developer.mozilla.org/zh-CN/docs/Web/MathML/Attribute">
</iframe>







PS：MathML的 mstyle 和 math 元素接受所有 MathML 的描述元素。
	请参阅MathML中值（values）和单位的注释值。





















>	参考文档：
	[MDN-MathML](https://developer.mozilla.org/zh-CN/docs/Web/MathML)
	[html5-mathml](http://www.runoob.com/html/html5-mathml.html)
	[MathML 介绍](https://www.ibm.com/developerworks/cn/xml/x-mathml3/)
































