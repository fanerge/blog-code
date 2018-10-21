---
title: 前端常识-gj
date: 2018-02-09 20:36:22
tags: '前端'
keywords: '前端面试'
categories: '前端面试'
copyright: true
password: 8746053
---
##	React virtualDOM（batching）  
在React中，render执行的结果返回的并不是真正的DOM节点，结果仅仅是轻量级的JavaScript对象，我们称之为virtual DOM。
通过 React 的 diff，再由虚拟 DOM 来确保只对界面上真正变化的部分进行实际的DOM操作，这样就极大提升了性能。
batching把所有的DOM操作搜集起来，一次性提交给真实的DOM。
>DOM 操作真正的问题在于每次操作都会触发布局的改变、DOM树的修改和渲染。所以，当你一个接一个地去修改30个节点的时候，就会引起30次（潜在的）布局重算，30次（潜在的）重绘。
当你在这个单独的 virtualDOM 树上也一个接一个地修改30个节点的时候，它不会每次都去触发重绘，所以修改节点的开销就变小了。
一旦你要把这些改动传递给真实DOM，之前所有的改动就会整合成一次DOM操作。这一次DOM操作引起的布局计算和重绘可能会更大，但是相比而言，整合起来的改动只做一次，减少了（多次）计算。

[React中一个没人能解释清楚的问题——为什么要使用Virtual DOM](https://www.jianshu.com/p/f75c1f0af3f0)
##	React diff
React diff 作为 Virtual DOM 的加速器，其算法上的改进优化是 React 整个界面渲染的基础。
计算一棵树形结构转换成另一棵树形结构的最少操作，是一个复杂且值得研究的问题。传统 diff 算法通过循环递归对节点进行依次对比，效率低下，算法复杂度达到 O(n^3)，其中 n 是树中节点的总数。
React 通过制定大胆的策略，将 O(n^3) 复杂度的问题转换成 O(n) 复杂度的问题。
diff 策略（---为具体比对）
1.Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计 --- tree diff
2.拥有相同类的两个组件将会生成相似的树形结构，拥有不同类的两个组件将会生成不同的树形结构 --- component diff
3.对于同一层级的一组子节点，它们可以通过唯一 id 进行区分 --- element diff
###	tree diff
React 通过 updateDepth 对 Virtual DOM 树进行层级控制，只会对相同颜色方框内的 DOM 节点进行比较，即同一个父节点下的所有子节点。当发现节点已经不存在，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。这样只需要对树进行一次遍历，便能完成整个 DOM 树的比较。
![tree-diff](http://p26lefllv.bkt.clouddn.com/ms_%E5%9B%BD%E9%99%85.jpg)
###	component diff
React 是基于组件构建应用的，对于组件间的比较所采取的策略也是简洁高效。
1.如果是同一类型的组件，按照原策略继续比较 virtual DOM tree。
2.如果不是，则将该组件判断为 dirty component，从而替换整个组件下的所有子节点。
3.对于同一类型的组件，有可能其 Virtual DOM 没有任何变化，如果能够确切的知道这点那可以节省大量的 diff 运算时间，因此 React 允许用户通过 shouldComponentUpdate() 来判断该组件是否需要进行 diff。
![component diff](http://p26lefllv.bkt.clouddn.com/ms_%E5%9B%BD%E9%99%851.jpg)
###	element diff
当节点处于同一层级时，React diff 提供了三种节点操作，分别为：INSERT_MARKUP（插入）、MOVE_EXISTING（移动）和 REMOVE_NODE（删除）。
1.INSERT_MARKUP，新的 component 类型不在老集合里， 即是全新的节点，需要对新节点执行插入操作。
2.MOVE_EXISTING，在老集合有新 component 类型，且 element 是可更新的类型，generateComponentChildren 已调用 receiveComponent，这种情况下 prevChild=nextChild，就需要做移动操作，可以复用以前的 DOM 节点。
3.REMOVE_NODE，老 component 类型，在新集合里也有，但对应的 element 不同则不能直接复用和更新，需要执行删除操作，或者老 component 不在新集合里的，也需要执行删除操作。
React 提出优化策略：允许开发者对同一层级的同组子节点，添加唯一 key 进行区分，这也是为什么React建议我们在列表项目中添加key属性的原因！

![element diff](http://p26lefllv.bkt.clouddn.com/ms_%E5%9B%BD%E9%99%852.jpg)
[知乎专栏-react diff，写的很不错](https://zhuanlan.zhihu.com/purerender/20346379)
##	webkit 渲染机制
先看下简单版的（我们从浏览器地址栏输入网址开始到web页面被完整的呈现在眼前做了哪些事，暂不考虑DNS缓存、本地资源缓存）
网址被DNS解析为IP地址 -> 通过IP地址建立TCP连接 -> 发送HTTP请求 -> 服务器处理请求并返回响应 ->  浏览器解析渲染页面 -> 断开TCP连接
###	渲染过程
一般渲染引擎主要包括HTML解释器、CSS解释器、Javascript引擎、布局、绘图等模块。
HTML解释器 ：HTML解释器的工作就是将网络或者本地磁盘获取到的HTML网页和资源从字节流解释成DOM树的结构（首先是字节流，经过解码之后是字符流，然后通过词法分析器会被解释成词语（TOKENS），经过语法分析器构建成节点，最后这些节点被组建成一颗DOM树）
CSS解释器 ：CSS字符串被CSS解释器处理后变成渲染引擎的内部规则表示。（样式规则建立完成之后，webkit会保存规则结果，当DOM的节点建立之后，webkit会为可视化节点选择合适的样式信息，即作样式规则匹配）
Javascript引擎 ：将Javascript代码处理并执行，一个Javascript引擎可以包括以下几个部分
　　编译器 -> 主要工作是将源代码编译成抽象语法树，在某些引擎中还包括将抽象语法树转换为字节码（JavascriptCore 引擎）。
　　解释器  -> 在某些引擎中，解释器主要是接收字节码，解释执行字节码，同时也依赖垃圾回收机制等。
　　JIT工具 -> 将字节码或者抽象语法树转换为本地代码 （V8 引擎）。
　　垃圾回收器和分析工具。
布局 ：计算RenderObject对象的位置、大小等信息。
绘图 ：将构建好的渲染内部表示模型使用图形库绘制出来。
![webkit渲染过程](http://p26lefllv.bkt.clouddn.com/webkitflow.png)
[WEBKIT渲染不可不知的这四棵树](http://www.sohu.com/a/115715208_472885)
[webkit 渲染机制](https://www.cnblogs.com/tianheila/p/6413586.html)
##	http1.1&2较1有哪些新东西？
###	长连接
HTTP 1.0需要使用keep-alive参数来告知服务器端要建立一个长连接，而HTTP1.1默认支持长连接。
###	节约带宽
HTTP 1.1支持只发送header信息(不带任何body信息)，如果服务器认为客户端有权限请求服务器，则返回100，否则返回401。客户端如果接受到100，才开始把请求body发送到服务器。这样当服务器返回401的时候，客户端就可以不用发送请求body了，节约了带宽。
另外HTTP还支持传送内容的一部分。这样当客户端已经有一部分的资源后，只需要跟服务器请求另外的部分资源即可。这是支持文件断点续传的基础。
###	HOST域
现在可以web server例如tomat，设置虚拟站点是非常常见的，也即是说，web server上的多个虚拟站点可以共享同一个ip和端口。
###	多路复用
HTTP2.0使用了多路复用的技术，做到同一个连接并发处理多个请求，而且并发请求的数量比HTTP1.1大了好几个数量级。
###	数据压缩
HTTP1.1不支持header数据的压缩，HTTP2.0使用HPACK算法对header的数据进行压缩，这样数据体积小了，在网络上传输就会更快。
###	服务器推送
当我们对支持HTTP2.0的web server请求数据的时候，服务器会顺便把一些客户端需要的资源一起推送到客户端，免得客户端再次创建连接发送请求到服务器端获取。这种方式非常合适加载静态资源
##	前端需要注意哪些SEO
1.合理的title、description、keywords：搜索对着三项的权重逐个减小，title值强调重点即可，重要关键词出现不要超过2次，而且要靠前，不同页面title要有所不同；description把页面内容高度概括，长度合适，不可过分堆砌关键词，不同页面description有所不同；keywords列举出重要关键词即可
2.语义化的HTML代码，符合W3C规范：语义化代码让搜索引擎容易理解网页
3.重要内容HTML代码放在最前：搜索引擎抓取HTML顺序是从上到下，有的搜索引擎对抓取长度有限制，保证重要内容一定会被抓取
4.重要内容不要用js输出：爬虫不会执行js获取内容（目前chrome浏览器可以了）
5.少用iframe：搜索引擎不会抓取iframe中的内容
6.非装饰性图片必须加alt
7.提高网站速度：网站速度是搜索引擎排序的一个重要指标
##	点击穿透
如何产生：
	现在有两层DOM结构（但不嵌套），底层和弹出层（底层在弹出层下面且弹出层的投影在底层内部），弹出层有一个 touchend 事件，底层有一个 click 事件。
当点击弹出层就会触发 touchend 事件（弹出层立即消失，这时事件的 target 为弹出层），300ms后触发 click 事件（由于弹出层消失了，这时事件的 target 就为底层了）。
看出来了吗？这样就发生了‘点击穿透’。
产生的原因：
	click事件延迟且弹出层消失了。
解决方案：
	1.只用touch事件
	2.只用click事件（不推荐只用click事件，这样所有点击都有延迟了，实在要使用可以使用事件库 fastclick）
	3.可以延迟（>300ms,好像不太科学）弹出层消失
[点击穿透原理及解决](http://blog.csdn.net/qq_17746623/article/details/55805425)
##	服务器’推‘技术
webSocket、Comet、轮询
Comet主要是利用客户端向服务器发出请求时，服务器发回响应内容，并利用javascript建立一个长时间链接的“长连接”，这个连接在没有接收到服务器或者没有到达连接时间限制时会一直等待服务器的消息，如果服务器有消息传来，立即显示最新信息。长连接每隔一段时间会重新向服务器发出连接请求。服务器在有新消息产生的时候立即检查消息的接收方是否存在长连接，如果存在马上发送，如果没有则不发送。

##	web开发中会话跟踪的方法有哪些
1.cookie（一般不能存关键字段，最好存sessionID配合session使用）
2.session
3.url重写
4.隐藏input
5.ip地址
##	img的title和alt有什么区别
title是global attributes之一，用于为元素提供附加的advisory information。通常当鼠标滑动到元素上的时候显示。
alt是 img 的特有属性，是图片内容的等价描述，用于图片无法加载时显示、读屏器阅读图片。可提图片高可访问性，除了纯装饰图片外都必须设置有意义的值，搜索引擎会重点分析。
##	doctype是什么,举例常见doctype及特点
```
<!doctype>声明必须处于HTML文档的头部，在<html>标签之前，HTML5中不区分大小写
<!doctype>声明不是一个HTML标签，是一个用于告诉浏览器当前HTMl版本的指令
现代浏览器的html布局引擎通过检查doctype决定使用兼容模式还是标准模式对文档进行渲染，一些浏览器有一个接近标准模型。
在HTML4.01中<!doctype>声明指向一个DTD，由于HTML4.01基于SGML，所以DTD指定了标记规则以保证浏览器正确渲染内容
HTML5不基于SGML，所以不用指定DTD
```
###	常见dotype
```
HTML4.01 strict：不允许使用表现性、废弃元素（如font）以及frameset。声明：<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
HTML4.01 Transitional:允许使用表现性、废弃元素（如font），不允许使用frameset。声明：<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
HTML4.01 Frameset:允许表现性元素，废弃元素以及frameset。声明：<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
XHTML1.0 Strict:不使用允许表现性、废弃元素以及frameset。文档必须是结构良好的XML文档。声明：<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
XHTML1.0 Transitional:允许使用表现性、废弃元素，不允许frameset，文档必须是结构良好的XMl文档。声明： <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
XHTML 1.0 Frameset:允许使用表现性、废弃元素以及frameset，文档必须是结构良好的XML文档。声明：<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
HTML 5: <!doctype html>
```
##	HTML全局属性(global attribute)有哪些
accesskey:设置快捷键，提供快速访问元素如aaa在windows下的firefox中按alt + shift + a可激活元素
class:为元素设置类标识，多个类名用空格分开，CSS和javascript可通过class、classList属性获取元素
contenteditable: 指定元素内容是否可编辑
contextmenu: 自定义鼠标右键弹出菜单内容
data-*: 为元素增加自定义属性
dir: 设置元素文本方向
draggable: 设置元素是否可拖拽
dropzone: 设置元素拖放类型： copy, move, link
hidden: 表示一个元素是否与文档。样式上会导致元素不显示，但是不能用这个属性实现样式效果
id: 元素id，文档内唯一
lang: 元素内容的的语言
spellcheck: 是否启动拼写和语法检查
style: 行内css样式
tabindex: 设置元素可以获得焦点，通过tab可以导航
title: 元素相关的建议信息
translate: 元素和子孙节点内容是否需要本地化
##	什么是web语义化,有什么好处
web语义化是指通过HTML标记表示页面包含的信息，包含了HTML标签的语义化和css命名的语义化。
HTML标签的语义化是指：通过使用包含语义的标签（如h1-h6）恰当地表示文档结构
css命名的语义化是指：为html标签添加有意义的class，id补充未表达的语义，如Microformat通过添加符合规则的class描述信息
为什么需要语义化：
1.去掉样式后页面呈现清晰的结构
2.搜索引擎更好地理解页面，有利于收录
3.便团队项目的可持续运作及维护
5.盲人使用读屏器更好地阅读
##	HTTP method
一台服务器要与HTTP1.1兼容，只要为资源实现GET和HEAD方法即可。
GET是最常用的方法，通常用于请求服务器发送某个资源。
HEAD与GET类似，但服务器在响应中值返回首部，不返回实体的主体部分
PUT让服务器用请求的主体部分来创建一个由所请求的URL命名的新文档，或者，如果那个URL已经存在的话，就用干这个主体替代它
POST起初是用来向服务器输入数据的。实际上，通常会用它来支持HTML的表单。表单中填好的数据通常会被送给服务器，然后由服务器将其发送到要去的地方。
TRACE会在目的服务器端发起一个环回诊断，最后一站的服务器会弹回一个TRACE响应并在响应主体中携带它收到的原始请求报文。TRACE方法主要用于诊断，用于验证请求是否如愿穿过了请求/响应链。
OPTIONS方法请求web服务器告知其支持的各种功能。可以查询服务器支持哪些方法或者对某些特殊资源支持哪些方法。
DELETE请求服务器删除请求URL指定的资源。
##	HTTP request报文结构是怎样的
首行是Request-Line包括：请求方法，请求URI，协议版本，CRLF
首行之后是若干行请求头，包括general-header，request-header或者entity-header，每个一行以CRLF结束
请求头和消息实体之间有一个CRLF分隔
根据实际请求需要可能包含一个消息实体
一个请求报文例子如下：
```
GET /Protocols/rfc2616/rfc2616-sec5.html HTTP/1.1
Host: www.w3.org
Connection: keep-alive
Cache-Control: max-age=0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.153 Safari/537.36
Referer: https://www.google.com.hk/
Accept-Encoding: gzip,deflate,sdch
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
Cookie: authorstyle=yes
If-None-Match: "2cc8-3e3073913b100"
If-Modified-Since: Wed, 01 Sep 2004 13:24:52 GMT

name=fanerge&age=26
```
##	HTTP response报文结构是怎样的
首行是状态行包括：HTTP版本，状态码，状态描述，后面跟一个CRLF
首行之后是若干行响应头，包括：通用头部，响应头部，实体头部
响应头部和响应实体之间用一个CRLF空行分隔
最后是一个可能的消息实体
响应报文例子如下：
```
HTTP/1.1 200 OK
Date: Tue, 08 Jul 2014 05:28:43 GMT
Server: Apache/2
Last-Modified: Wed, 01 Sep 2004 13:24:52 GMT
ETag: "40d7-3e3073913b100"
Accept-Ranges: bytes
Content-Length: 16599
Cache-Control: max-age=21600
Expires: Tue, 08 Jul 2014 11:28:43 GMT
P3P: policyref="http://www.w3.org/2001/05/P3P/p3p.xml"
Content-Type: text/html; charset=iso-8859-1

{"name": "qiu", "age": 25}
```
##	如何进行网站性能优化
###	content方面
减少HTTP请求：合并文件、CSS精灵、inline Image
减少DNS查询：DNS查询完成之前浏览器不能从这个主机下载任何任何文件。方法：DNS缓存、将资源分布到恰当数量的主机名，平衡并行下载和DNS查询
避免重定向：多余的中间访问
使Ajax可缓存
非必须组件延迟加载
未来所需组件预加载
减少DOM元素数量
将资源放到不同的域下：浏览器同时从一个域下载资源的数目有限，增加域可以提高并行下载量
减少iframe数量
不要404
###	Server方面
使用CDN
添加Expires或者Cache-Control响应头
对组件使用Gzip压缩
配置ETag
Flush Buffer Early
Ajax使用GET进行请求
避免空src的img标签
###	Cookie方面
减小cookie大小
引入资源的域名不要包含cookie
###	css方面
将样式表放到页面顶部
不使用CSS表达式
使用不使用@import
不使用IE的Filter
###	Javascript方面
将脚本放到页面底部
将javascript和css从外部引入
压缩javascript和css
删除不需要的脚本
减少DOM访问
合理设计事件监听器
###	图片方面
优化图片：根据实际颜色需要选择色深、压缩
优化css精灵
不要在HTML中拉伸图片
保证favicon.ico小并且可缓存
###	移动方面
保证组件小于25k
Pack Components into a Multipart Document
[yahoo Best Practices for Speeding Up Your Web Site](https://developer.yahoo.com/performance/rules.html)
##	什么是渐进增强
渐进增强是指在web设计时强调可访问性、语义化HTML标签、外部样式表和脚本。保证所有人都能访问页面的基本内容和功能同时为高级浏览器和高带宽用户提供更好的用户体验。
核心原则如下:
所有浏览器都必须能访问基本内容
所有浏览器都必须能使用基本功能
所有内容都包含在语义化标签中
通过外部CSS提供增强的布局
通过非侵入式、外部javascript提供增强功能
end-user web browser preferences are respected
##	HTTP状态码及其含义
1XX：信息状态码
100 Continue：客户端应当继续发送请求。这个临时相应是用来通知客户端它的部分请求已经被服务器接收，且仍未被拒绝。客户端应当继续发送请求的剩余部分，或者如果请求已经完成，忽略这个响应。服务器必须在请求之后向客户端发送一个最终响应。
101 Switching Protocols：服务器已经理解力客户端的请求，并将通过Upgrade消息头通知客户端采用不同的协议来完成这个请求。在发送完这个响应最后的空行后，服务器将会切换到Upgrade消息头中定义的那些协议。
2XX：成功状态码
200 OK：请求成功，请求所希望的响应头或数据体将随此响应返回
201 Created：
202 Accepted：
203 Non-Authoritative Information：
204 No Content：
205 Reset Content：
206 Partial Content：
3XX：重定向
300 Multiple Choices：
301 Moved Permanently：
302 Found：
303 See Other：
304 Not Modified：
305 Use Proxy：
306 （unused）：
307 Temporary Redirect：
4XX：客户端错误
400 Bad Request:
401 Unauthorized:
402 Payment Required:
403 Forbidden:
404 Not Found:
405 Method Not Allowed:
406 Not Acceptable:
407 Proxy Authentication Required:
408 Request Timeout:
409 Conflict:
410 Gone:
411 Length Required:
412 Precondition Failed:
413 Request Entity Too Large:
414 Request-URI Too Long:
415 Unsupported Media Type:
416 Requested Range Not Satisfiable:
417 Expectation Failed:
5XX: 服务器错误
500 Internal Server Error:
501 Not Implemented:
502 Bad Gateway:
503 Service Unavailable:
504 Gateway Timeout:
505 HTTP Version Not Supported:





