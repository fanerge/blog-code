---
title: 深入浅出Node.js-读书笔记
date: 2017-10-09 20:09:51
category: "NodeJS"
tags: ['js','读书笔记','服务端']
---
最近在研究node.js,正赶上国庆长假，回趟老家。在网上找了一本电子书籍《深入浅出Node.js》，利用这个假期学习一下。
##	Node.js 基础知识
###	chrome 与 Node 工作原理
	chrome：HTML + JavaScript + WebKit + V8 >> 中间层 >> 网卡 + 硬盘 + 显卡 + ...
	Node：JavaScript + V8 >> 中间层（libuv）>> 网卡 + 硬盘 + 显卡 + ...
	说明：libuv 是 Node 的新跨平台抽象层，用于抽象 Windows 的 IOCP 及 Unix 的 libev。
		作者打算在这个库的包含所有平台的差异性。
###	异步I/O
	```
	$.get('url', (data) => {
		console.log(data);
	});
	```
	异步是并行的基础。
	单线程，不适合大量计算占用 CPU 导致无法继续调用异步I/O。
##	模块机制
###	CommonJS 规范
	模块引用：
		var math = require('math');	
	模块定义：
		exports.add = () => {
			var a = 1,
				b = 3;
			return a + b;
		}
	exports 是 module 的属性。
	Node 引入模块的步骤：
		路径分析
		文件定位
		编译执行
###	核心模块
	文件模块 >> 核心模块（JavaScript） >> 内建模块（C/C++）
##	异步I/O
###	Node 的异步I/O
	Node 自身的执行模型 -- 事件循环。
	单线程、事件循环、观察者和I/O线程池
###	非I/O的异步API
	定时器：setTimeout()\setInterval()
	process.nextTick()
		在事件循环的下一次循环中调用 callback 回调函数。
		效果是将一个函数推迟到代码书写的下一个同步方法执行完毕时或异步方法的事件回调函数开始执行时；
		与setTimeout(fn, 0) 函数的功能类似，但它的效率高多了。
	setImmediate()
		nextTick()的回调函数执行的优先级要高于setImmediate();	
		process.nextTick()属于idle观察者,setImmediate()属于check观察者.在每一轮循环检查中,idle观察者先于I/O观察者,
		I/O观察者先于check观察者.
##	异步编程
###	函数式编程
	高阶函数：以函数作为参数或返回值。
	偏函数用法：通过指定部分参数来产生一个新的定制函数的形式就是偏函数。
	```
	const isType = function (type){
		return function (obj) {
			return Object.prototype.toString.call(obj) == '[object ' + type + ']';
		}; 
	}
	var isString = isType('String');
	```
###	异步编程解决方案
	事件发布/订阅模式（事件绑定）
	promise
	generator
	async-await
###	并发方案
	eventproxy
	[async](https://github.com/caolan/async)
##	内存控制
##	理解Buffer	
	Buffer 是一个像Array的对象，但它主要用于操作字节。
	Buffer 对象
		var buf = new Buffer('string', 'utf-8');

###	Buffer 的转换
		支持的字符串编码类型：
		ASCII\UTF-8\UTF-16LE/UCS-2\Base64\Binary\Hex
	字符串转 Buffer	
	```
	var buf = new Buffer(str, [encoding]);
	buf.write(string, [offset], [length], [encoding]);
	```
	Buffer 转字符串
	buf.toString([encoding], [start], [end]);
##	网络编程
	TCP 和 UDP 属于网络传输层协议，HTTP 属于应用层协议。
###	TCP（传输控制协议）
	创建TCP 服务器端 net 模块
	TCP 服务的事件
###	构建UDP 服务（用户数据包协议）
	创建UDP套接字 dgram模块
	创建UDP 服务器端
	创建UDP 客户端
	UDP 套接字事件
###	构建HTTP 服务
	HTTP 超文本传输协议。
	http 模块
	HTTP 客户端
###	构建WebSocket 服务
	以前的方案：Comet（彗星）技术细节为：长轮询（long-polling）或iframe流（streaming）。
	长轮询原理：客户端向服务器断发送请求，服务端只在超时或有数据响应时断开连接（res.end()），
		客户端在接收到数据或者超时后重新发送请求。
	iframe流原理：iframe 是很早就存在的一种 HTML 标记， 通过在 HTML 页面里嵌入一个隐蔵帧，
		然后将这个隐蔵帧的 SRC 属性设为对一个长连接的请求，服务器端就能源源不断地往客户端输入数据。
		通过iframe里的内容进行长时间的请求，当需要传输内容时通过调用父页面js方法来实现页面展示，以此达到comet所需要的效果。
	WebSocket原理：WebSocket是一种在单个TCP连接上进行全双工通讯的协议。
		WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在WebSocket API中，
		浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。
###	网络服务与安全
	Node在网络安全上提供了3个模块
1.	crypto -- 主要用于加密和解密，SHA1、MD5。
2.	tls -- 类似于net模块，它是建立在TLS/SSL加密的TCP连接上。
3.	https -- 类似于http模块，他它是建立于安全的连接之上。
	TLS/SSL
	TLS/SSL是一个公钥/私钥的结构，它是一个非对称的结构。
	每个服务器断和客户端都有自己的公私钥。
	公钥要来加密要传输的数据，私钥用来解密接收到的数据。
	Node在底层采用的是openssl实现TLS/SSL。
	数字证书：CA（Certificate Authority，数字证书认证中心）
	HTTPS服务
	HTTPS服务就是工作在TLS/SSL上的HTTP。
> 参考文档
	[朴灵-深入浅出Node](https://www.baidu.com/s?ie=utf8&oe=utf8&wd=%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BAnode&tn=98010089_dg&ch=1)















