---
title: Web知识
date: 2018-05-14 20:53:23
tags: Web
categories: '前端面试'
copyright: true
---
本文所有知识点，点到即止，详细内容请看各部分的连接。
##	点击穿透（多见于移动端模态框等浮层）
产生：上层元素触发touch事件->上层元素消失（300ms之内）->底层元素click事件触发
PS：touch事件之后会有300ms延迟在执行click事件是因为，在这300ms中若再次tap行为则认定为double tap事件，否则就触发click事件。
方案：只用touch事件、只用click事件（不推荐）、fastclick类库等
[点击穿透原理及解决](https://blog.csdn.net/qq_17746623/article/details/55805425)
##	CSP（内容安全策略Content-Security-Policy）
定义：内容安全策略 (Content Security Policy) 是一个附加的安全层，用于帮助检测和缓解某些类型的攻击，包括跨站脚本 (XSS) 和数据注入等攻击。
[CSP 策略指令](https://developer.mozilla.org/zh-CN/docs/Web/Security/CSP/CSP_policy_directives)
##	CORS
定义：CORS (跨域资源共享)是一个系统, 包括传输的 HTTP headers, 其确定是否阻止或完成从该资源所在的域外的另一个域的网页上的受限资源的请求。
PS：同源安全策略( same-origin security policy)默认禁止“跨域”请求. CORS 给予Web服务器跨域访问控制, 启用安全的跨域数据传输。
[CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)
##	BFC（块格式化上下文Block Formatting Context）
定义：块格式化上下文（Block Formatting Context）是Web页面的可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。
如何触发BFC：
• html 根元素；
• float 的值不为 none；
• overflow 的值为 auto、scroll 或 hidden；
• display 的值为 table-cell、table-caption 和 inline-block 中的任何一个；
• position 的值不为 relative 和 static。

[块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
##	CRLF
CRLF实际上是两个字符：CR是Carriage Return（ASCII 13，\r），LF是Line Feed（ASCII 10，\n）。
\r\n这两个字符是用于表示换行的，其十六进制编码分别为 0x0d、0x0a。

##	containing block（包含块）
定义：在 CSS2.1 中，很多框的定位和尺寸的计算，都取决于一个矩形的边界，这个矩形，被称作是包含块( containing block )。
PS：主要作用是以百分比（相关于包含块）计算自身的width、height、top、left、padding、margin等css Layout 属性。
[我所了解的CSS包含块](https://fanerge.github.io/2018/%E4%B8%80%E6%AC%A1%E8%AF%A6%E7%BB%86%E7%9A%84%E5%8C%85%E5%90%AB%E5%9D%97%E5%AD%A6%E4%B9%A0.html)	
##	FOUC（无样式内容闪烁Flash Of Unstyled Content）
定义：指的是加载网页时出现的短暂的CSS样式失效。
方案：head头部放css、避免使用import
[FOUC](https://www.cnblogs.com/fsjohnhuang/p/6739064.html)
##	Hack
定义：由于不同的浏览器对CSS的支持程度不同，同样CSS的样式代码在不同浏览器当中的表现可能出现不一致。为了让所有浏览器样式统一，有时需要为某种浏览器设置不同于其他浏览器的“专属样式”。
方案：属性前缀法、选择器前缀法、条件注释法（<!--[if lt IE 8]-->）
##	Domain Name（域名）
域名（英语：Domain Name），简称域名、网域，是由一串用点分隔的名字组成的Internet上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位（有时也指地理位置）。
顶级域名又称为跟域名（TLD）共1058+（.com、.net、.cn等等）
13个根域名服务器（不代表只有13台服务器，事实上517+台服务器）
##	DNS（网域名称系统Domain Name System）
网域名称系统（DNS，Domain Name System，有时也简称为域名）是因特网的一项核心服务，它作为可以将域名和IP地址相互映射的一个分布式数据库，能够使人更方便的访问互联网，而不用去记住能够被机器直接读取的IP地址数串。
##	a链接问题
a链接使用绑定 mousedown 事件且 event.preventDefault() 会导致 :active 伪类失效（Firefox）。
PS：Firefox 认为 mousedown 事件在 :active 之前发生。
##	min-width、max-width、width、!important优先级
如果min-width、max-width、width、!important同时存在时，优先级顺序：min-width >> max-width >> !important >> width
##	巧用css 兄弟选择器（如导航分割线）
```
<a href="">登录</a><a href="">注册</a>
// 这样排除第一个元素
a + a:before {
	content: "";
	font-size: 0;
	padding: 10px 3px 1px;
	margin-left: 6px;
	border-left: 1px solid gray;
}
```
##	img 在firefox和其他浏览器的差异
```
img {
	// width 和 height 无效，需要设置display: inline-block;
	width: xx;
	height: xx;
}
```
##	关于百分比设置下列属性
left（right）、width、padding、margin、background-position、text-index设置值为百分比时，其计算基准为[包含块](https://fanerge.github.io/2018/%E4%B8%80%E6%AC%A1%E8%AF%A6%E7%BB%86%E7%9A%84%E5%8C%85%E5%90%AB%E5%9D%97%E5%AD%A6%E4%B9%A0.html)的width值
top（bottom）、height设置值为百分比时，其计算基准为[包含块](https://fanerge.github.io/2018/%E4%B8%80%E6%AC%A1%E8%AF%A6%E7%BB%86%E7%9A%84%E5%8C%85%E5%90%AB%E5%9D%97%E5%AD%A6%E4%B9%A0.html)height值
##		滚动容器底部留白
滚动容器底部留白使用 margin-bottom，使用 padding-bottom 存在兼容性问题。
##	margin 合并（只存在垂直方向）
1.	相邻兄弟元素 margin 合并。
2.	父级和第一个、父级和最后一个子元素。（虽然是在子元素上设置的 margin-top，但实际上就等同于在父元素上设置了 margin-top）
3.	空块级元素的 margin 合并。

PS：合并规则，正正取大值，正负值相加，负负最小值。
##	margin: auto;的理解
margin: auto; 表示对剩余空白进行分配。
下面元素 .son 的 margin-left 为 300 - 200 -80 = 20px
```
.father {
	width: 300px;
}
.son {
	width: 200px;
	margin-right: 80px;
	margin-left: auto; // 20px
}
```
使用 margin 来进行 right 对齐
```
.father {
	width: 300px;
}
.son {
	width: 200px;
	margin-left: auto; // 此时 auto 值为100px
	// flot: right; 等价
}
```
PS：块级元素的左中右对齐使用 margin ，内联元素使用 text-align 控制左中右对戏。
水平和垂直居中
```
.father {
	width: 300px; height:150px;
	position: relative;
}
.son {
	position: absolute; // 很关键
	top: 0; right: 0; bottom: 0; left: 0;
	width: 200px; height: 100px;
	margin: auto; // 很关键
}
```
##	border 的一些秘密
border-width 不支持百分比值（outline、box-shadow、text-shadow同样）
border-style:double 至少 3px 才有效果。
border-style:dashed 不同浏览器不一致（虚线颜色区的宽高比以及颜色区和透明区的宽度比例），如chrome上为方形ie为圆形。
thin（1px）、medium（默认值3px）、thick（4px）。
##	border 技巧
等腰三角形
```
div {
	width: 0;
	border-width: 10px 20px;
	border-style: solid;
	border-color: #f30 transparent transparent;
}
```
直角三角形
```
div {
	width: 0;
	border-width: 10px 20px;
	border-style: solid;
	border-color: #f30 #f30 transparent transparent;
}
```
边框 3D 效果
```
div {
	width: 10px; height: 10px;
	border: 10px solid;
	border-color: #f30 #00f #396 #0f0;
}
```
[等高布局](http://www.cnblogs.com/xiaohuochai/p/5457127.html)
##	css 度量单位 ex
相对长度单位。相对于字符“x”的高度。通常为字体高度的一半。
如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。
作用：基于ex单位的天然垂直居中对齐效果
##	行距
行距 = line-height - font-size
##	line-height:1.5、line-height:150%和 line-height:1.5em 表现一样，为什么重置css时只能用第一种呢？
因为继承细节有所差别，如果使用数值作为 line-height 的属性值，那么所有的子元素继承的都是这个值（如1.5）；但是，如果使用百分比值或者长度值作为属性值，那么所有的子元素继承的是最终的计算值（16px*150%=24px，此时继承的就是24px这个值，不是150%）。
##	vertical-align
作用：只能应用于内联元素以及 display 值为 table-cell 的元素。
定义：该属性定义行内元素的基线相对于该元素所在行的基线的垂直对齐。
线类，如 baseline（默认值）、top、middle、bottom；
文本类，如 text-top、text-bottom；
上标下标类，如 sub、super；
数值百分比类，如 20px、2em、20%等
PS：其实middle为基线往上 1/2 x-height 处（ x-height 为 x 的高度）
##	消除图片下间隙
1.	display:block;
2.	vertical-align:top; // top，text-top，bottom，text-bottom 均可
3.	font-size:0; // 父级设置
4.	overflow:hidden;
5.	float:left;

[CSS图片下面产生间隙的6种解决方案](http://www.cnblogs.com/luojianqun/archive/2013/07/08/3177969.html)
##	float
规范约定了浮动元素和内联元素在一行显示。
浮动元素会生成一个块级框，而不论它本身是何种元素。
##	clip属性
fixed 固定定位的剪裁
最佳可访问性隐藏（clip: rect(0,0,0,0)）
##	 stacking context（层叠上下文）
层叠上下文，英文称作 stacking context，是 HTML 中的一个三维的概念。
###	层叠水平（stacking level)（逐渐升高）
1.	层叠上下文（background/border） -- 充当背景色
2.	负的z-index
3.	block块状水平盒子 -- 布局
4.	float浮动盒子
5.	inline水平盒子（inline/inline-block/inline-table） -- 内容
6.	z-index:anto 或 看成 z-index:0
7.	正的z-index

一般情况都会满足满足上面的规则，详情可以看下面demo：
<p data-height="265" data-theme-id="dark" data-slug-hash="pKverE" data-default-tab="css,result" data-user="fanerge" data-embed-version="2" data-pen-title="stackingContext" class="codepen">See the Pen <a href="https://codepen.io/fanerge/pen/pKverE/">stackingContext</a> by 余真帆 (<a href="https://codepen.io/fanerge">@fanerge</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

###	如何触发一个元素形成堆叠上下文？
1.	根元素 (HTML),
2.	z-index 值不为 "auto"的 绝对/相对定位，
3.	一个 z-index 值不为 "auto"的 flex 项目 (flex item)，即：父元素 display: flex|inline-flex，
4.	opacity 属性值小于 1 的元素（参考 the specification for opacity），
5.	transform 属性值不为 "none"的元素，
6.	mix-blend-mode 属性值不为 "normal"的元素，
7.	filter值不为“none”的元素，
8.	perspective值不为“none”的元素，
9.	isolation 属性被设置为 "isolate"的元素，
10.	position: fixed
11.	在 will-change 中指定了任意 CSS 属性，即便你没有直接指定这些属性的值
12.	-webkit-overflow-scrolling 属性被设置 "touch"的元素

PS：使用了上述属性就会形成一个stacking context（堆叠上下文）。此时，要对两者进行层叠排列，就需要 z-index ，z-index 越高的层叠层级越高。
做了一个上述情况的demo：
<p data-height="265" data-theme-id="dark" data-slug-hash="JZoWWp" data-default-tab="css,result" data-user="fanerge" data-embed-version="2" data-pen-title="stackingContext2" class="codepen">See the Pen <a href="https://codepen.io/fanerge/pen/JZoWWp/">stackingContext2</a> by 余真帆 (<a href="https://codepen.io/fanerge">@fanerge</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

[层叠顺序与堆栈上下文知多少](https://www.cnblogs.com/coco1s/p/5899089.html)

##	requestIdleCallback
window.requestIdleCallback()会在浏览器空闲时期依次调用函数， 这就可以让开发者在主事件循环中执行后台或低优先级的任务，而且不会对像动画和用户交互这样延迟触发而且关键的事件产生影响。函数一般会按先进先调用的顺序执行，除非函数在浏览器调用它之前就到了它的超时时间。
[你应该知道的requestIdleCallback](https://segmentfault.com/a/1190000014457824)

##  敏捷开发的原则编辑
###  快速迭代
相对那种半年一次的大版本发布来说，小版本的需求、开发和测试更加简单快速。一些公司，一年发布仅2~3个版本，发布流程缓慢，它们仍采用瀑布开发模式，更严重的是对敏捷开发模式存在误解。
###  让测试人员和开发者参与需求讨论
需求讨论以研讨组的形式展开最有效率。研讨组，需要包括测试人员和开发者，这样可以更加轻松定义可测试的需求，将需求分组并确定优先级。 同时，该种方式也可以充分利用团队成员间的互补特性。如此确定的需求往往比开需求讨论大会的形式效率更高，大家更活跃，参与感更强。
###  编写可测试的需求文档
开始就要用“用户故事”（User Story）的方法来编写需求文档。这种方法，可以让我们将注意力放在需求上，而不是解决方法和实施技术上。过早的提及技术实施方案，会降低对需求的注意力。
###  多沟通，尽量减少文档
任何项目中，沟通都是一个常见的问题。好的沟通，是敏捷开发的先决条件。在圈子里面混得越久，越会强调良好高效的沟通的重要性。
团队要确保日常的交流，面对面沟通比邮件强得多。
###  做好产品原型
建议使用草图和模型来阐明用户界面。并不是所有人都可以理解一份复杂的文档，但人人都会看图。
###  及早考虑测试
及早地考虑测试在敏捷开发中很重要。传统的软件开发，测试用例很晚才开始写，这导致过晚发现需求中存在的问题，使得改进成本过高。较早地开始编写测试用例，当需求完成时，可以接受的测试用例也基本一块完成了。 








