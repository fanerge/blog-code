---
title: 懂这些，你将能构建更安全的Web应用
date: 2018-06-27 22:05:43
tags: '安全'
categories: '安全'
copyright: true
---
#	浏览器安全
##	同源策略（Same Origin Policy）
同源策略限制了一个源（origin）中加载的文档或脚本与其他源（origin）中的资源交互的方式。这是一种用来隔离潜在恶意文档的关键安全机制。
源的定义：如果两个页面的协议、域名、端口（如果有指定）和都相同，则两个页面具有相同的源。
##	浏览器沙箱Sandbox 和 多线程架构
浏览器为我们提供了一个独立的沙箱环境，尽量来保障浏览器的安全，也有部分浏览器采用一个 tab 页面一个线程，这样多线程架构增强了浏览器的安全（即使某个线程崩溃了，也不至于浏览器崩溃）。 
##	恶意网址拦截（大多基于黑名单）
常见的黑名单获取：[PhishTank恶意网址黑名单](https://www.phishtank.com/)、Google 的SafeBrowsing API、EVSSL证书
##	更安全的浏览器进化
IE8 的 XSS Filter功能
Firefox 的 CSP（Content Security Policy）

#	XSS（跨站脚本攻击Cross Site Script）
##	定义
XSS攻击全称跨站脚本攻击，是为不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为XSS，XSS是一种在web应用中的计算机安全漏洞，它允许恶意 Web 用户将代码植入到提供给其它用户使用的页面中。
XSS分为三种类型：1.反射型XSS 2.存储型XSS 3.DOM Based XSS（通过修改页面的DOM节点形成的XSS）
##	危害
Cookie劫持（一般Cookie作为登录凭证，通过HttpOnly可以防止）、构造GET与POST请求、XSS钓鱼、识别用户浏览器、CSS HIstory Hack（访问过的连接会使用:visited的样式）、获取用户的真实IP地址（前提如用户安装了Java环境JRE等）、XSS Worm
##	解决方案
CSP(Content Security Policy)、对特殊字符转义，不要相信任何来自用户的输入（包括请求体、queryString甚至是请求Headers）
1.	响应头 HttpOnly 有效保护 Cookie
2.	XSS Filter输入检查 排除、转义特殊字符
3.	输出检查
4.	安全的编码函数 HtmlEncode （将特殊字符转换为实体字符）
5.	只需一种编码吗 浏览器解析 htmlparser 优先于 JavaScript Parser

##	对于处理富文本开发：
1.	我们应该让事件被严格禁止，不包含iframe、script、base、form等危险标签。
2.	使用白名单，避免使用黑名单。
3.	OWASP 开源的 XSS Filter 项目（Antisamy--Java 和 .NET）（HTML Purifier--PHP） 

[XSS漏洞的原理](https://www.zhuyingda.com/blog/article.html?id=2)

#	CSRF（跨站点脚本Cross-site request forgery）
##	原理
是指在黑客已经将代码植入受害用户的浏览器访问的页面的前提下，以“受害用户”的身份向服务端发起一个伪造的http请求，从而实现服务器 CRUD 来执行读写操作。
##	解决方案
验证码，我们应该使用验证码作为防御 CSRF 的辅助手段，而不应该作为最主要的解决方案（毕竟不可能所有操作都加上验证码，多次验证码不利于用户体验）。
Referer Check ，常见应用为防止图片盗链，直白的讲就是后端需要检查请求头中的 Referer 字段，是否为我们期望的"源"。
Anti CSRF Token，需要保证使用足够安全的随机数生产算法或真随机数生成器，只有用户和服务器共同持有，才能保证安全。
PS：Referer 首部包含了当前请求页面的来源页面的地址，即表示当前页面是通过此来源页面里的链接进入的。
[CSRF漏洞的原理](https://www.zhuyingda.com/blog/article.html?id=5)

#	clickjacking（点击劫持）
##	原理
点击劫持其实是一种视觉上的欺骗手段，攻击者将一个透明的、不可见的iframe覆盖在一个网页上，通过调整iframe页面位置，诱使用户在页面上进行操作，在不知情的情况下用户的点击恰好是点击在iframe页面的一些功能按钮上，其实还也可使用 img 来代替 iframe，这就是后面要说的 XSIO。
HTML5的Drag 和 Drop API会发生数据窃取，分别在 Drag 隐藏一个 iframe ，在 Drop 中 隐藏一个 textarea ，在 drop 事件时就可以获取来自 drag 的数据，从而形成数据劫持，event.dataTransfer.getData()
##	防御
frame busting：禁止iframe的嵌套[JS防止潜入](https://fanerge.github.io/2018/%E9%9D%A2%E8%AF%95%E6%9D%82%E9%A1%B9.html)
X-Frame-Options：防止或限制网页内嵌（http头部X-FRAME-OPTIONS）
CSP的frame-ancestors：指定了一个可以包含frame，iframe，object，embed，or applet等元素的有效来源。
PS：http头部X-FRAME-OPTIONS为非标准的（但所有浏览器都支持），你可以使用CSP的frame-ancestors（标准属性）。
同样还有触屏劫持（TapJacking）实现原理基本类似。
[ClickJacking漏洞的原理](https://www.zhuyingda.com/blog/article.html?id=6)

#	window.name 的妙用
window.name 属性可设置或返回存放窗口的名称的一个字符串。
因为 window 对象是浏览器的窗体，并非 document 对象，因此许多时候 window 对象不受同源策略的限制。
可以利用该属性实现跨域、跨页面传递数据[可以使用postMessage来进行跨源通信](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)。

#	XSIO
原理：没有限制图片的position属性为absolute，导致可以控制一张图片出现在网页的任意位置。
[百度的XSIO](https://blog.csdn.net/inject2006/article/details/3057045)

#	iframe 的 sandbox 属性
可以防止以下操作：
1.	访问父页面的DOM（从技术角度来说，这是因为相对于父页面iframe已经成为不同的源了）
2.	执行脚本
3.	通过脚本嵌入自己的表单或操作表单
4.	对cookie、本地存储Storage、本地数据库IndexDB的读写

PS：sandbox 属性有 allow-same-origin（允许同源访问）、allow-top-navigation（允许访问顶层窗口）、allow-forms（允许提交表单）、allow-script（允许执行脚本）

#	a标签 和 area标签的安全问题
当a链接有target="_blank"属性时，必须添加rel="noreferrer noopener"，不然新产生的页面可以通过window.opener来获取到父窗口的window对象。
`<a href="www.baidu.com" target="_blank" rel="noreferrer noopener" >`


#	CSP (内容安全策略)
内容安全策略 (CSP, Content Security Policy) 是一个附加的安全层，用于帮助检测和缓解某些类型的攻击，包括跨站脚本 (XSS) 和数据注入等攻击。 这些攻击可用于实现从数据窃取到网站破坏或作为恶意软件分发版本等用途。
其实CSP的本质是以白名单的机制对网站加载或执行的资源起作用。
##	适用方式
1.	可以通过配置你的网络服务器返回  Content-Security-Policy  HTTP头部 ( 有时你会看到一些关于X-Content-Security-Policy头部，它是旧版本)。
2.	在html页面中meta元素中使用，如下
```
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">
```

#	隐私与:visited选择器
##	为什么说:visited选择器暴露了用户隐私？
曾经，CSS选择器 :visited 被网站用来查看用户的浏览记录。通过使用 getComputedStyle() 或其他方法扫描用户的浏览记录来获取用户访问了哪些网站。这很容易实现，不仅能够判断用户是否曾经访问过这个页面，还能猜测出大量的用户身份信息。
不过现代浏览器已经做了相应的修复和只能给已访问链接设置少量的样式（color、background-color、border-color、outline-color、fill 和 stroke 等）

#	跨域资源共享（CORS）
CORS属于HTTP访问控制特性，以下内容大多针对于XMLHttpRequest，有些并不适用于 Fetch 。
当一个资源从与该资源本身所在的服务器不同的域或端口请求一个资源时，资源会发起一个跨域 HTTP 请求。
比如，站点 `http://domain-a.com` 的某 HTML 页面通过 `img` 的 `src` 请求 `http://domain-b.com/image.jpg`。网络上的许多页面都会加载来自不同域的CSS样式表，图像和脚本等资源。
出于安全原因，浏览器限制从脚本内发起的跨源HTTP请求或者返回结果被浏览器拦截了。 例如，XMLHttpRequest和Fetch API遵循同源策略。 这意味着使用这些API的Web应用程序只能从加载应用程序的同一个域请求HTTP资源，除非使用CORS头文件。
PS：跨域并非不一定是浏览器限制了发起跨站请求，也可能是跨站请求可以正常发起，但是返回结果被浏览器拦截了。最好的例子是 CSRF 跨站攻击原理，请求是发送到了后端服务器无论是否跨域！注意：有些浏览器不允许从 HTTPS 的域跨域访问 HTTP，比如  Chrome 和 Firefox，这些浏览器在请求还未发出的时候就会拦截请求，这是一个特例。）
跨域资源共享（ CORS ）机制允许 Web 应用服务器进行跨域访问控制，从而使跨域数据传输得以安全进行。浏览器支持在 API 容器中（例如 XMLHttpRequest 或 Fetch ）使用 CORS，以降低跨域 HTTP 请求所带来的风险。

#	X-Frame-Options 响应头
X-Frame-Options HTTP 响应头是用来给浏览器指示允许一个页面可否在 `frame`（已废弃）, `iframe` 或者 `object` 中展现的标记。网站可以使用此功能，来确保自己网站的内容没有被嵌到别人的网站中去，也从而避免了点击劫持 (clickjacking) 的攻击。
##	使用 X-Frame-Options
X-Frame-Options 有三个值:
DENY--表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。
SAMEORIGIN--表示该页面可以在相同域名页面的 frame 中展示。
ALLOW-FROM uri--表示该页面可以在指定来源的 frame 中展示。
PS：如果设置为 DENY，不光在别人的网站 frame 嵌入时会无法加载，在同域名页面中同样会无法加载。另一方面，如果设置为 SAMEORIGIN，那么页面就可以在同域名页面的 frame 中嵌套。

#	前端反爬虫
该课题是在腾讯的[IMWeb前端博客上看到的，整理于此，非常感谢。](http://imweb.io/topic/595b7161d6ca6b4f0ac71f05)
[反击爬虫，前端工程师的脑洞可以有多大？](http://litten.me/2017/07/09/prevent-spiders/)
##	FONT-FACE拼凑式
[猫眼电影](http://maoyan.com/films/#content)-页面使用了font-face定义了字符集，并通过unicode去映射展示。
##	BACKGROUND拼凑式
美团-与font的策略类似，美团里用到的是background拼凑。数字其实是图片，根据不同的background偏移，显示出不同的字符。
##	字符穿插式
[微信公众号文章](https://mp.weixin.qq.com/s?__biz=MzI0MDYwNjk2OA==&mid=2247484365&idx=4&sn=291a93e8a4ce6e90d3b6ef8b98fe09c4&chksm=e919085ade6e814cc037ecf6a873f22da0e492911a4e539e6f8fdeff022806b4d248c4d54194&scene=4)-某些微信公众号的文章里，穿插了各种迷之字符，并且通过样式把这些字符隐藏掉。
##	 伪元素隐藏式
[汽车之家](https://car.autohome.com.cn/config/series/3170.html)-把关键的厂商信息，做到了伪元素的content里（如汽车厂家）。
##	元素定位覆盖式
[去哪网](https://flight.qunar.com/site/oneway_list.htm?searchDepartureAirport=%E5%B9%BF%E5%B7%9E&searchArrivalAirport=%E5%8C%97%E4%BA%AC&searchDepartureTime=2018-06-27&searchArrivalTime=2018-06-29&nextNDays=0&startSearch=true&fromCode=CAN&toCode=BJS&from=qunarindex&lowestPrice=null)-对于一个4位数字的机票价格，先用四个i标签渲染，再用两个b标签去绝对定位偏移量，覆盖故意展示错误的i标签，最后在视觉上形成正确的价格。
##	IFRAME异步加载式
[网易云音乐](http://music.163.com/#/song?id=424477863)-页面一打开，html源码里几乎只有一个iframe，并且它的src是空白的：about:blank。接着js开始运行，把整个页面的框架异步塞到了iframe里面。
##	字符分割式
[全网代理IP](http://www.goubanjia.com/)-他们会先把IP的数字与符号分割成dom节点，再在中间插入迷惑人的数字，如果爬虫不知道这个策略，还会以为自己成功拿到了数值；不过如果爬虫注意到，就很好解决了。
##	字符集替换式
[去哪儿移动侧](https://m.flight.qunar.com/ncs/page/flightlist?depCity=%E5%8C%97%E4%BA%AC&arrCity=%E4%B8%8A%E6%B5%B7&goDate=2018-06-27&from=touch_index_search&child=0&baby=0&cabinType=0)-html里明明写的3211，视觉上展示的却是1233。原来他们重新定义了字符集，3与1的顺序刚好调换得来的结果
PS：常用的后端反爬有 User-Agent + Referer检测、账号及Cookie验证、验证码、IP限制频次。

#	子资源完整性（SRI,Subresource Integrity）
子资源完整性(Subresource Integrity)是允许浏览器检查其获得的资源（例如从 [CDN](https://developer.mozilla.org/zh-CN/docs/Glossary/CDN) 获得的）是否被篡改的一项安全特性。它通过验证获取文件的哈希值是否和你提供的哈希值一样来判断资源是否被篡改。
子资源完整性 (SRI) 是一种安全功能，允许浏览器验证所获取的文件 (比如，从一个 CDN 内容分发网络) 是无意外操作而交付的。它的工作原理是允许你提供一个获取文件必须匹配的加密哈希。
##	SRI 如何工作
使用 内容分发网络 (CDN) 在多个站点之间共享脚本和样式表等文件可以提高站点性能并节省带宽。然而，使用CDN也存在风险，如果攻击者获得对 CDN 的控制权，则可以将任意恶意内容注入到 CDN 上的文件中 （或完全替换掉文件)，因此可能潜在地攻击所有从该 CDN 获取文件的站点。
子资源完整性通过确保 Web 应用程序获得的文件未经第三方注入或其他任何形式的修改来降低这种攻击的风险。
##	如何使用 SRI
将使用 base64 编码过后的文件哈希值写入你所引用的 script 或 link 标签的 integrigy 属性值中即可启用子资源完整性功能。
PS：integrity 值分成两个部分，第一部分指定哈希值的生成算法（目前支持 sha256、sha384 及 sha512），第二部分是经过 base64 编码的实际哈希值，两者之间通过一个短横（-）分割。
integrity 值可以包含多个由空格分隔的哈希值，只要文件匹配其中任意一个哈希值，就可以通过校验并加载该资源。
###	内容安全策略（CSP）和子资源完整性（SRI）共同使用
你可以根据内容安全策略来配置你的服务器使得指定类型的文件遵守 SRI。这是通过在 CSP 头部添加 require-sri-for 指令实现的：
```
// 这条指令规定了所有 JavaScript 都要有 integrity 属性，且通过验证才能被加载。
Content-Security-Policy: require-sri-for script;
// 你也可以指定所有样式表也要通过 SRI 验证：
Content-Security-Policy: require-sri-for style;
```
你也可以对两者都加上验证。
##	生成 SRI 哈希的工具
###	openssl 在命令行
你可以用 openssl 在命令行中执行如下命令来生成 SRI 哈希值：
`cat FILENAME.js | openssl dgst -sha384 -binary | openssl enc -base64 -A`	     
[在线生成 SRI 哈希值的工具](https://www.srihash.org/)
下面以知乎的一个js文件为例
```
// 原js文件地址
https://unpkg.zhimg.com/za-js-sdk@2.8.3/dist/zap.js
// 生成的脚本标签
<script src="https://unpkg.zhimg.com/za-js-sdk@2.8.3/dist/zap.js" integrity="sha384-B4YDh2AljLezOmNwiezobW8FJbJQfyZxm1SksT7THfKULK6SVxN+dRNSvLxEmXtA" crossorigin="anonymous"></script>
```
###	 shasum 在命令行
`shasum -b -a 384 FILENAME.js | xxd -r -p | base64`
[MDN-SRI](https://developer.mozilla.org/zh-CN/docs/Web/Security/%E5%AD%90%E8%B5%84%E6%BA%90%E5%AE%8C%E6%95%B4%E6%80%A7)

#	用户密码是泄漏的原因？
 HTTPS 协议旨在保护用户数据在网络上不被窃听（机密性） 和不被篡改（完整性）。处理用户数据的网站应该使用 HTTPS 协议保护他们的用户不受黑客的侵害。如果网站使用 HTTP 协议而不是 HTTPS 协议，窃取用户信息（比如他们的登录凭证）将会轻而易举。这曾经被 [Firesheep](http://codebutler.github.io/firesheep/) 很好地演示过。
这里罗列出密码所牵涉到的安全问题：
1.	在HTTP之上运行登录表单. 即使表单的action对象是HTTPS链接,用户的登录表单信息也会受到威胁,因为攻击者能够通过用户修改用户接收到的页面(例如,攻击者插入键盘记录脚本来盗取用户输入的密码.他们还能改变表单目的页从而将敏感信息传递到受他们控制的服务器).
2.	在表单的action链接中使用HTTP链接.在这种情况下,用户输入的任何信息都将以明文方式通过网络传递.这样,从密码离开用户的电脑到密码到达服务器过程中,用户的密码将清楚地展现在任何嗅探用户网络的人眼前.
3.	在网页iframe中递交登录表单(或是嵌入在HTTP frame中的HTTPS frame).即使最上层页面是HTTPS,但在HTTP iframe中包含密码域和在HTTP页面中包含密码域是没有区别的.攻击者同样能够修改这个页面以及偷取用户信息.
4.	有时网页需要用户名及密码,但实际上却没有存储这些敏感的信息.例如,一个新闻页面可能存储一个用户想要再次阅读的文章,却没有存储任何关于这位用户的其他信息.这个新闻站点的网页开发者可能没有动力去对于提高他的网站的安全性以及保护他们用户的信息.不幸的是,密码重用也是一个大问题.用户可能在不同的站点使用相同的密码(新闻网页,社交网络,电子邮箱及其银行).因此即使通过用户名及密码登陆你的网页对你来说不是很大的问题,对于重复使用相同用户名及密码来登陆他们银行账户的用户来说却是一个极大的威胁.网络攻击者正变得越来越聪明.他们在一个网站同时盗取用户名及密码然后在另一个可能能给他们带来金钱的网站上使用这些密码.

#	弱签名算法
在签署数字证书时，哈希算法的完整性是决定证书安全性的关键因素。哈希算法的弱点可能导致攻击者在某些情况下能够获得伪造的证书。由于技术的升级和已知的新型攻击，此类攻击的可行性已经大为提升。因此，不推荐使用旧算法，对于旧算法的支持最终也会停止。
##	MD5
对基于MD5的签名的支持已在2012年初停止。
##	SHA-1
基于SHA-1的签名非常普遍；截至2015年5月，大约45%的数字证书皆使用此算法。但是，SHA-1已经过时因而不再推荐使用。
SHA-1的证书将从2017开始不再被主流浏览器厂商视为安全的。
##	SHA-2（推荐使用）
SHA-2是一个哈希算法家族，其中包括SHA-256和SHA-512。截至2015年，SHA-2家族被认为足够安全强大。许多证书颁发机构颁发新的证书使用SHA-256。

#	SQL注入
##	原理
通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。
##	解决方案
使用预编译语句、使用存储过程、检查数据类型、使用安全函数（编码特殊字符）、最小权限原则（是否可操作数据库）
PS：防御SQL注入的最佳方式，就是使用预编译语句，绑定变量。

#	MIMT(中间人攻击Man-in-the-middle-attacks)
##	原理
Client <--> Proxy Server(Middle Man这里可能存在攻击) <--> Web Server真实的服务器
Client 发出的请求 和 Web Server返回的数据都经过Proxy Server 转发，这个Proxy Server 就起到了一个Middle Man的作用，如果这个“中间人” 够黑，那么整个代理过程的数据都可以由这个“中间人”控制，“中间人”可以进行截取敏感数据、代码注射、Proxp worm操作。
##	解决方案
启用虚拟专用网(VPN)、https(传输报文加密)
[web中间人攻击的威胁](https://www.zhuyingda.com/blog/article.html?id=7)

#	DDOS（分布式拒绝服务Distributed Denial of Service）
##	定义
最基本的DDOS攻击就是利用合理的服务请求来占用过多的服务资源，从而使合法用户无法得到服务的响应。
##	解决方案
限制请求频率、高防服务器、黑名单、DDoS 清洗、CDN（隐藏真实IP及分流）
##	类别
###	SYN flood（网络层DDOS）
[什么是SYN Flood攻击?](https://www.cnblogs.com/popduke/p/5823801.html)
对抗措施：SYN Cookie 为每一个IP地址分配一个Cookie，并统计每个IP地址的访问频率。如果短时间内收到大量的来自同一个IP地址的数据包，则认为是受到攻击，之后来自这个IP地址的包将被丢失。
###	Challenge Collapasar（应用层DDOS）
原理：对一些消耗资源较大的应用页面不断发起正常的请求（查询数据库、读写硬盘文件等），以达到消耗服务端资源的目的。
对抗措施：在应用中针对每个"客户端"做一个请求频率的限制，可以通过IP地址与Cookie定位一个客户端。
若IP地址改变（代理服务器）或Cookie清空，应该使用应用代码要做好性能优化（常用数据转移到内存中），在网络架构上做好优化（利用负载均衡、CDN、镜像站点等分流，减轻主站的压力）。
[知乎-什么是 DDoS 攻击？](https://www.zhihu.com/question/22259175)

#	文件上传漏洞
##	产生的问题
1.	上传文件是Web脚本语言
2.	上传文件是木马、病毒文件，诱骗用户或者管理员下载执行
3.	上传文件是钓鱼图片或包含了脚本的图片，在某些浏览器会被作为脚本执行

##	解决方案
文件上传的目录设置为不可执行
判断文件类型（使用白名单而不是黑名单）
使用随机数改写文件名和文件路径
单独设置文件服务器的域名
##	案列
###	绕过文件上传检查功能
UI原本允许上传JPG图片，那么可以构造文件名（需要修改POST包）为xxx.php[\0].JPG，其中[\0]为十六进制的0x00字符，.JPG绕过了应用的上传文件类型判断，但对于服务器来说，此文件因为0x00字符截断的关系，最终会变成xxx.php。
对应的解决方案，可以检查附件头信息（同样也可以使用脚本语言来伪造一个合法头信息，这时应该提供不让脚本执行的容器，Web Server将其当做静态文件来解析，从而避免）。
##	Apache文件解析问题
在Apache1.x、Apache2.x中，对文件名的解析就存在以下特性。
Apache对于文件的解析是从后往前解析的，知道遇到一个Apache认识的文件类型为止。
Apache的mime.types文件配置了Apache能认识那些文件。

#	认证与会话管理
##	认证和授权？
认证的目的是为了认出用户是谁，而授权的目的是为了决定用户能够做什么。
##	常见的认证方式
密码认证
多因素认证（手机号+身份证+护照等等）
Session 与认证（对SessionID加密后保存在Cookie中，Cookie随HTTP请求发送，且受浏览器同源策略的保护）
##	常见攻击
Session Fixation 攻击：登录前和登录后，重写 Session ID。
##	单点登录（SSO）
SSO英文全称Single Sign On，单点登录。SSO是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。它包括可以将这次主要的登录映射到其他应用中用于同一个用户的登录的机制。它是目前比较流行的企业业务整合的解决方案之一。
OpenID：OpenID 是一个以用户为中心的数字身份识别框架，它具有开放、分散性。OpenID 的创建基于这样一个概念：我们可以通过 URI （又叫 URL 或网站地址）来认证一个网站的唯一身份，同理，我们也可以通过这种方式来作为用户的身份认证。

#	访问控制
##	垂直权限管理
主要为基于角色的访问控制（Role-Based Access Control）RBAC
一个用户有多个角色，一个角色有一个权限集合，最小粒度为每个权限（权限码）。
有以下几个分类
###	基于URL的访问控制
如不同角色能访问不同的页面
```
<sec:http>
	<sec:intercept-url pattern="/admin" access="ROLE_ADMIN"/>
	<sec:intercept-url pattern="/user/*" access="ROLE_USER"/>
</sec:http>
```
###	基于method的访问控制
如限定角色调用方法
```
@PreAuthorize("hasRole('ROLE_USER')")
public void create(Contact contact)
```
###	基于表达式的访问控制
如角色和IP地址验证
`hasRole('admin') and hasIpAddress('192.168.1.0/24')`
##	水平权限管理
###	什么叫水平权限？
用户A只能访问用户A的数据，不能通过构造URL等手段访问到用户B的数据。
##	OAuth（Open Authorization）
OAUTH协议是一个在不提供用户名和密码的情况下，授权第三方应用访问 Web 资源的安全协议。
OAuth1.0中分为3个角色（Consumer：消费方 Client、Service Provider：服务提供方 Server、User：用户 Resource Owner）
如我们在人人网，想要导入用户MSN里的好友
这里人人网为消费方；MSN为服务提供方、用户为资源拥有者

#	cookie防篡改（实现思路）
1.	服务端提供一个签名生成算法secret
2.	根据方法生成签名secret(wall)=34Yult8i
3.	将生成的签名放入对应的Cookie项username=wall|34Yult8i。其中，内容和签名用|隔开。
4.	服务端根据接收到的内容和签名，校验内容是否被篡改（签名和服务器之前生成的不一致则表示cookie被篡改了）。


[Cookie防篡改机制](https://juejin.im/post/5b02fe326fb9a07ab1117c82)

>	参考文档：
[浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)
[CSP 策略指令](https://developer.mozilla.org/zh-CN/docs/Web/Security/CSP/CSP_policy_directives)
[X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)
[PUBLIC SUFFIX LIST](https://publicsuffix.org/)
[CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
[X-Frame-Options](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/X-Frame-Options)
[SRI Hash Generator](https://www.srihash.org/)
[HTTPS](https://en.wikipedia.org/wiki/HTTPS)
[Let's Encrypt，免费好用的 HTTPS 证书](https://imququ.com/post/letsencrypt-certificate.html)


















