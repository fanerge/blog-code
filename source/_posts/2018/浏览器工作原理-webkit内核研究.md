---
title: 浏览器工作原理-webkit内核研究
date: 2018-03-03 10:10:00
tags: '浏览器'
keywords: 'webkit浏览器'
categories: 'webkit'
copyright: true
---
从年前就开始梳理浏览器的工作原理（主要对webkit内核的浏览器进行分析）到现在差不多快一个月了，本文意在帮助前端工程师了解浏览器的工作原理，并据此来优化我们的网页性能。
主要查阅了[WebKit技术内幕](https://book.douban.com/subject/25910556/)、[MDN](https://developer.mozilla.org/zh-CN/)、[W3C](https://www.w3.org/)等网站资料，下文中有若干图片摘自于《WebKit技术内幕》，在此表示感谢。
本文略长，如有不适，实属意外。如有不正确的地方，还望指正，毕竟传播真理才不会误导其他同学，共同进步才是目的。
#	浏览器的内核
浏览器内核由渲染引擎和JS引擎组成，不同的浏览器、即使同一浏览器不同型号可能渲染引擎和JS引擎都不一样。
##	渲染引擎
1)Trident渲染引擎 –> 老版本IE系列浏览器
2)Edge渲染引擎 -> Win10中IE浏览器
3)Gecko渲染引擎 –> Mozilla Firefox
4)Presto渲染引擎 –> Opera
5)KHTML渲染引擎 –> 早期的Safafi和Google Chrome
6)Webkit渲染引擎 -> 2001年后的Safari和Chrome以及国内的一些浏览器
7)Blink渲染引擎 -> 新版本的Chromium浏览器Google项目
##	JavaScript引擎
1)JScript引擎 –> IE系列浏览器
2)spiderMonkey引擎 –> Mozilla Firefox
3)V8引擎 –> Google Chrome
4)linear b/futhark引擎 –> Opera
##	浏览器渲染引擎的进度史
![浏览器渲染引擎的进度史](http://p4yvw0vpm.bkt.clouddn.com/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%B8%B2%E6%9F%93%E5%BC%95%E6%93%8E%E5%8F%91%E5%B1%95%E5%8E%86%E5%8F%B2.png)
为大家提供两个开发常用查询网站：
[该浏览器对html5的支持程度](http://html5test.com/)
[不同与Can I use](https://caniuse.com)

#	浏览器的渲染引擎及依赖模块分析
![渲染引擎的功能](http://p4yvw0vpm.bkt.clouddn.com/%E6%B8%B2%E6%9F%93%E5%BC%95%E6%93%8E%E7%9A%84%E5%8A%9F%E8%83%BD.png)
上图中虚线部分表示渲染引擎所提供的功能。
这里渲染引擎包含了JavaScript引擎，许多时候两者都不太区分。
下面的内容大部分都是基于这张图来分析的，我们将逐步解释从用户输入URL到页面展示给用户这个过程中都发生了什么？
我们先了解网页的基础知识。
#	网页基础知识
html -- 结构
css -- 样式
JavaScript -- 行为
还需要一些静态资源：png、gif、webp、MP4、font、svg等等。
由上面几部分就构成我们的网页。
##	输入URL到页面展示简图
![URL到页面展示](http://p4yvw0vpm.bkt.clouddn.com/url%E5%88%B0dom%E6%A0%91.png)
读者不要太关心上图所标的顺序，在某些时候可能会有出入。
##	浏览器内核各部分解释
>	HTML解释器：解释HTML文本的解释器，主要作用是将HTML文本解释成DOM树，DOM是一种文档表示方法。
CSS解释器：级联样式表的解释器，它的作用是为DOM中的各个元素对象计算出样式信息，从而为计算最后网页的布局提供基础设施。
布局：在DOM创建之后，webkit需要将其中的元素对象同样式信息结合起来，计算它们的大小位置等布局信息，形成一个能够表示这所有信息的内部比偶表示模型。
JavaScript引擎：使用JavaScript代码可以修改网页的内容，也能修改CSS的信息，JavaScript引擎能过解释JavaScript代码并通过DOM接口和CSSOM接口来修改网页内容和样式信息，从而改变渲染结果。
绘图：使用图形库将布局计算后的各个网页的节点绘制成图像结果。

PS：这些模块依赖许多其他基础模块，其中包括网络、存储、2D/3D图形、音频视频和图片解码器等。这里就不对基础模块做相应说明了。

下面，我就逐个过程进行分析，我这里省略一些非本文目的的过程，如DNS环节。

#	HTML解释器
先来看看HTML解释器工作原理
![HTML解释器工作原理](http://p4yvw0vpm.bkt.clouddn.com/html%E8%A7%A3%E9%87%8A%E5%99%A8.png)
字节流（Bytes）--> 字符流（Characters）--> 词语（Tokens）--> 节点 --> DOM树
>	首先是字节流，经过解码之后是字符流，然后通过词法分析器会被解释成词语（Tokens），时候经过分析器构建成节点，最后这些节点被组建成一棵DOM树。
词法分析：HTMLTokenizer 类（作用是词法分析，类似于状态机），输入的是字符串，输出的是一个个的词语。
XSSAuditor验证词语：XSSAuditor （验证词语流Token Stream）XSS指的是Cross Site Security，主要是针对安全方面的考虑。
词语到节点：webkit用来构建DOM节点，这一步骤由HTMLDocumentParser 类调用 HTMLTreeBuilder 类的 constructTree的函数来实现。
节点到DOM树：树中的元素节点创建属性节点等工作由HTMLConstructionSite类来完成，该类中包含一个 HTMLElementStack 作为保存元素节点的栈。 
JavaScript的执行：webkit将DOM树创建过程中需要执行得我Javascript代码交由HTMLScriptRunner类来负责。
DOM的事件机制：webkit中用EventTarget类来表示DOM规范中Events部分定义的事件目标，Node 节点继承自 EventTarget类，所以Node拥有EventTarget类的相关的方法。

这里需要提一下W3C新规范，影子（Shadow）DOM
Shadow DOM API的 ShadowRoot接口是一个DOM子树的根节点, 它与文档的主DOM树分开渲染。
[MDN-影子节点](https://developer.mozilla.org/zh-CN/docs/Web/API/ShadowRoot)
##	影子（Shadow）DOM	
定义：Shadow DOM 为Web组件中的 DOM和 CSS提供了封装。Shadow DOM 使得这些东西与主文档的DOM保持分离。
ShadowRoot 类继承自 DocumentFragment 类。
PS：可以使用document.createDocumentFragment 方法或者构造函数来创建一个空的 DocumentFragment.

#	CSS解释器和样式布局
先看看CSS怎么和DOM结合展示页面的呢？
![CSS+DOM形成简单页面](http://p4yvw0vpm.bkt.clouddn.com/css%E5%92%8Cdom%E6%A0%91%E5%88%B0%E7%BB%98%E5%88%B6%E7%BD%91%E9%A1%B5.png)
css解释器和规则匹配处于DOM树建立之后，RenderObject树建立之前，css解释器解释后的结果会保存起来，然后RenderObject树基于该结果来进行规范匹配和布局计算。
##	CSSOM（CSS Object Model）
CSSOM视图模块(CSSOM View Module)定义了一些 API，Web 开发人员使用这些 API 可以进行检查，也可以以编程方式更改文档及其内容的视觉属性，包括布局框定位、视区宽度和元素滚动。
document.styleSheets 可以查看当前页面的StyleSheetList对象，每个link、style都会产生 CSSStyleSheet 作为 StyleSheetList对象的value。
##	CSS解释器和规则匹配（本区块内容会使我们对css选择器权重有更好的理解）
DocumentStyleSheetCollection类（属于Document类），该类包含了所有CSS样式表，还包括了webkit的内部表示类CSSStyleSheet，它包含了CSS的href、类型、内容等信息。
CSS解释过程：css字符串经过css解释器处理后变成渲染引擎的内部规则的过程，使用CSSParser类来负责该过程。
在解释网页中自定义的CSS样式之前，实际上webkit渲染引擎会为每个网页设置一个默认样式，这也是我们为什么要重置浏览器样式的根本原因。
规则匹配：StyleResolver类为DOM的元素节点匹配样式，StyleResolver类根据元素的信息，例如标签名、类别等，从样式规则中查找最匹配的规则，然后将样式信息保存到新建的RenderStyle对象中。最后，这些RenderStyle对象被RenderObject类所管理和使用。
	其中，规则的匹配则是由ElementRuleCollector类来计算并获得，它根据元素的属性等信息，并从DocumentRuleSets类中获取规则集合，依次按照ID、CLASS、标签等选择器信息逐次匹配获得元素的样式。
	然后webkit对这些规则进行排序，对于该元素需要的样式属性，webkit选择从高优先级规则中选取，并将样式属性值返回。
这里，我引入一个不太相关的知识点，块格式化上下文（Block Formatting Context，BFC） 是Web页面的可视化CSS渲染的部分，是块级盒布局发生的区域，也是浮动元素与其他元素交互的区域。
[不太了解的同学，请异步MDN-BFC](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

#	webkit布局	
当webkit创建RenderObject对象之后，每个对象是不知道自己的位置、大小等信息的，webkit根据盒模型来计算他们的位置、大小等信息的过程称为布局计算/排版。
	布局计算分类：第一类是对整个RenderObject树进行的计算；第二类是对RenderObject树中某个子树的计算，常见于文本元素或者overflow：auto块的计算。
	布局计算：布局计算是一个递归的过程，这是因为一个节点的大小通常需要先计算它的子节点的位置、大小等信息。
##	扩展知识点
我们常说的reflow和repaint。涉及到元素的几何属性改变会造成reflow会降低性能（transform、opacity等属性不会造成reflow）。
扩展为什么说transform实现动画较直接设置几何属性性能较好？
1.webkit渲染过程：style -> Layout(reflow发生在这) -> Paint（repaint发生在这） -> Composite，transform是位于’Composite（渲染层合并）‘，而width、left、margin等则是位于‘Layout（布局）’层，这必定导致reflow。
2.现代浏览器针对transform等开启GPU加速。
	style -> Layout(reflow发生在这) -> Paint（repaint发生在这） -> Composite（transform发生在这个时候）	
由这个过程我们可以看出，这也是为什么发生reflow必定会发生repaint的根本原因。
[CSS Animation性能优化](https://www.w3cplus.com/animation/animation-performance.html)
[从重绘重排角度讲解transform的动画性能](https://segmentfault.com/a/1190000008650975)

#	渲染过程的一些理论
RenderObject树同其他树（如RenderLayer树等），构成了webkit渲染的主要基础设施。
##	RenderObject树（DOM树 -> RenderObject树）
![DOM到RenderObject](http://p4yvw0vpm.bkt.clouddn.com/DOM%E5%88%B0RenderObject.png)
一个RenderObject对象保存了为绘制DOM节点所需要的各种信息，例如样式布局信息，经过webkit的处理之后，RenderObject对象知道如何绘制自己。
下列情况会使DOM树节点创建一个RenderObject对象（DOM和RenderObject并非一一对应）。
1.DOM树的document节点。
2.DOM树种的可视节点，例如html、body、div等。而webkit不会为非可视化节点创建RenderObject节点，例如meta、script。
3.某些情况下webkit需要建立匿名的RenderObject节点，该节点不对应于DOM树种的任何节点，而是webkit处理上的需要，典型的例子例如匿名的RenderBlock节点。
在html组建页面结构时，webkit为了提升网页性能，会引入分层结构。
##	网页层次结构（css也会对网页的分层策略产生重要影响）
对于一个html文件webkit会为某些元素和它的子节点建立新层，这样webkit可以单独对某层操作提升性能，下列情况会产生新层。
1.video标签 -- webkit在新层中有效的处理视频解码器和浏览器之间的交互和渲染问题。
2.div、p等普通标签 -- 涉及到3D变换时。
3.canvas标签 -- 复杂的2D和3D绘图操作。
##	RenderLayer树
![RenderObject到RenderLayer](http://p4yvw0vpm.bkt.clouddn.com/RenderObject%E5%88%B0RenderLayer.png)
webkit会为网页的层次创建相应的RenderLayer对象。当某些类型RenderObject的节点或者某些css样式的RenderObject节点出现的时候，webkit就会为这些节点创建RenderLayer对象。
RenderLayer树是基于RenderObject树建立起来的一棵新树。RenderLayer节点和RenderObject节点不是一一对应关系，而是一对多的关系。
哪些情况下的RenderObject节点需要建立新的RenderLayer节点呢？
1.DOM树的Document节点对应的RenderView节点。
2.DOM树中的Document的子节点，也就是HTML节点对应RenderBlock节点。
3.显式的制定css位置的RenderObject节点。
4.有透明效果的RenderObject节点。
5.节点有溢出（overflow）、alpha或者反射效果的RenderObject节点。
6.使用Canvas 2D和3D（WebGL）技术的RenderObject节点。
7.Video节点对应的RenderObject节点。
##	渲染方式
绘图上下文（绘图上下文可以分成两种类型）：
	第一种是用来绘制2D图形的上下文，称之为2D绘图上下文（GraphicsContext）。
	第二种是绘制3D图形的上下文，称之为3D绘图上下文（GraphicsContext3D）。
网页的三种渲染方式：
1.软件渲染（CPU内存）
2.使用软件绘图的合成化渲染（GPU内存）css3D、WebGL
3.硬件加速的合成化渲染（GPU内存）
##	webkit软件渲染技术
在不需要硬件加速内容的时候（包括但不限于css3 3D变形、css3 3D变换、WebGL和视频），webkit就可以使用软件渲染技术来完成页面绘制。
对于每个RenderObject对象，需要三个阶段绘制自己：
第一阶段是绘制该层中所有块的背景和边框。
第二阶段是绘制浮动内容。
第三阶段是前景（Foreground），也就是内容部分、轮廓、字体颜色、大小等（内嵌元素的背景、边框等发生在这一阶段）。
##	硬件加速机制
硬件加速技术是指使用GPU的硬件能力来帮助渲染网页（GPU的作用主要是用来绘制3D图形并且性能特别好）。
###	Chrome的硬件加速机制
canvas开发，可以将画布分解为更小的画布，这样在更新时只需要更新小画布从而减少开销。
css3 3D变形技术，它能过让浏览器仅仅使用合成器来合成所有的层就可以达到动画效果（只触发Composite而不用触发style -> Layout(reflow发生在这) -> Paint）。
###	WebGL
WebGL是Khronous组织提出的一套基于3D图形定义的javascript接口。
它基于canvas元素，跟canvas2D不同的是，Web开发者可以使用3D图形接口来绘制各种3D图形。
###	css 3D变形
这里包括3D变形和动画。
webkit会建立一个新层来处理，从而提升性能。

#	JavaScript引擎
推动JavaScript运行速度提高的利器JIT（Just-In-Time）。
JIT：就是代码在目标平台上运行的时候，实时的把代码编译为目标机器上的机器码。
编译原理：
C++：源代码 --> 抽象语法树 --> 本地代码
Java：源代码 --> 抽象语法树  --> 字节码（跨平台） --> JIT --> 本地代码
##	V8的一些特性（这里太多了，读者可以自己深究）
常用的javascript引擎有v8和JavaScriptCore
工作原理
在js中，基本数据类型Boolean、Number、String、Null、Undefined、Symbol，其他数据都是对象。
###	数据表示
在V8中，数据的表示分成两个部分
	第一部分是数据的实际内容，它们是变长的，而且内容的类型也不一样，如String、对象等。
	第二部分是数据的句柄，句柄的大小是固定的，句柄中包含指向数据的指针。
###	数据的存储
Handle：句柄类，主要用来管理基础数据和对象，以便被垃圾回收器操作。
主要有两个类型，一个Local类（继承自Handle类），表示本地栈上的数据，所以比较轻量。
另一个是Persistent类（继承自Handle类）表示函数间的数据和对象访问。
对于整形数据，由Handle本身来存储，同时也为了快速访问。
其他的数据都是从堆中申请内存来存储它们，由于其他数据类型，受限于Handle的大小和变长等原因，都存储在堆中。
V8的延迟（deferred）特性：它使的许多javascript代码的编译直到运行的时候被调用到才会发生，这样可以减少时间开销。
![v8将源代码-本地代码](http://p4yvw0vpm.bkt.clouddn.com/javascript%E5%88%B0%E6%9C%AC%E5%9C%B0%E4%BB%A3%E7%A0%81.png)
##	扩展
###	常见的语言类型
机器语言（它是计算机唯一能直接执行的语言，电子计算机的机器指令是一列二进制数字。） 
汇编语言 汇编指令是机器指令便于记忆的书写格式，但他需要进过编译器转换为机器语言，这样机器才能执行。
###	使用setTimeout或setInterval较requestAnimationFrame的缺点？
时间间隔应该设置为多少才合适呢。
跟屏幕的分辨率有关吗（不同浏览器存在一个极小值）。
设置的时间会按照会准确执行吗。
动画会被平滑地显示效果吗。
回调函数时复杂的好还是简单的好呢。
[window.requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)

#	其他浏览器相关知识
##	插件和Javascript扩展
在早期的浏览器能力十分有限，Web前端开发者们希望能够通过一些机制来扩展浏览器的能力（插件机制如flash插件）。
###	NPAPI全称叫 Netscape plugin API
NPAPI是当今最流行的插件架构，几乎所有浏览器都支持，不过存在很大的安全隐患，插件可以窃取系统底层权限，发起恶意攻击。
###	PPAPI也就是Pepper Plugin API
2010年，Google开发了新的PPAPI，将外挂插件全部放到沙盒里运行，2012年Windows、Mac版本的Chrome浏览器先后升级了PPAPI Flash Player，并希望今年底之前彻底淘汰NPAPI。
##	JavaScript引擎的扩展机制
通过如下url参看当前chrome浏览器安装的extensions
chrome://extensions/ 
##	多媒体
###	WebRTC
WebRTC实现了基于网页的视频会议，标准是WHATWG 协议，目的是通过浏览器提供简单的javascript就可以达到实时通讯（Real-Time Communications (RTC)）能力。
[MDN-WebRTC](https://developer.mozilla.org/zh-CN/docs/Web/API/WebRTC_API)
最重要的方法：navigator.mediaDevices.getUserMedia(constraints)
还有Video、Audio等。
##	安全机制
第一部分是网页的安全，包括但是不限于网页数据安全传输、跨域访问、用户数据安全等。
第二部分是浏览器的安全，具体是指虽然网页或者Javascript代码有一些安全问题或者存在安全漏洞，浏览器也能够在运行它们的时候保证吱声的安全，不受到攻击从而泄漏数据或者使系统遭受破坏。
###	网页安全模型
安全模型基础：
域（Same Origin Policy）XMLHttpRequest、cookie的读写、DOM对象操作等。
XSS（Cross Site Scripting）执行跨域的js脚本代码。开发者可以将用户输入的数据进行字符转换来避免。webkit通过XSSAuditor对象帮我们过滤（默认开启）。
CSP （Content-Security-Policy）HTTP首部字段，内容安全策略（CSP）用于检测和减轻用于 Web 站点的特定类型的攻击，例如 XSS 和数据注入等。
CORS（Cross Origin Resource Sharing）跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站有权限访问哪些资源。
	具体服务端代码设置
```
// 请求头
header('Access-Control-Allow-Origin: http://arunranga.com'); // 
header('Access-Control-Allow-Methods: POST, GET, OPTIONS');
header('Access-Control-Allow-Headers: X-PINGARUNER');
// 响应头
Access-Control-Allow-Origin、
Access-Control-Allow-Credentials、
Access-Control-Allow-Headers、
Access-Control-Expose-Headers、
Access-Control-Allow-Methods、
Access-Control-Max-Age
```
Cross Document Messaging 通过 window.postMessage 和 message 事件来通信。
HTTPS（安全传输协议）
SPDY（读作“SPeeDY”）是Google开发的基于TCP的应用层协议，用以最小化网络延迟，提升网络速度，优化用户的网络使用体验。	SPDY核心思想为多路复用。	
QUIC（Quick UDP Internet Connection）是谷歌制定的一种基于UDP的低时延的互联网传输层协议。
####	CSP和CORS的区别：
CSP定义了网页自身能够访问的某些域和资源。
CORS定义一个网页如何才能访问被同源策略禁止的跨域资源，并规定了两者交互的协议和方式。
###	沙箱模型
浏览器的沙箱模型是利用系统提供的安全技术，让网页在执行过程中不会修改操作系统或者是访问系统中的隐私数据，而需要访问系统资源或者说是系统调用的时候，通过一个代理机制来完成。

#	chrome浏览其使用技巧（以实用性排列）
一下url直接输入在浏览器中，enter即可

URL | 作用 |
- | :-: |
chrome://inspect | 移动端网页调试
chrome://net-internals | net-internals是一套工具集合，用于帮助诊断网络请求与访问方面的问题，它通过监听和搜集 DNS，Sockets，SPDY，Caches等事件与数据来向开发者反馈各种网络请求的过程、状态以及可能产生影响的因素。如，查看DNS主机解析缓存chrome://net-internals/#dns
chrome://view-http-cache/ | 查看内部存储内容及其详情
chrome://downloads/ | 下载内容管理，其快捷键是Ctrl+J
chrome://extensions/ | 扩展管理
chrome://bookmarks/ | 书签管理  
chrome://history | 访问历史管理  
chrome://restart | 重启chrome浏览器 
chrome://apps | chrome网上应用店  
chrome://flags/ | 新特性管理 
chrome://dns | 查看DNS预取命名（从超链接等处来预测）  
chrome://quota-internals | 查看浏览器所使用磁盘空间配额 
chrome://settings  | 浏览器的设置
chrome://sync-internals | 查看chrome 的同步状态 
chrome://about/ | 查看所有chrome命令 
[期望加入一个技术氛围nice的团队-成都](https://fanerge.github.io)
	

	

	

	

	

	
	


