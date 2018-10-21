---
title: Web Worker
date: 2018-01-26 21:06:15
tags: 'html5'
categories: 'html5'
copyright: true
---
[先放上demo，打开控制台试试](https://fanerge.github.io/H5WebWorker/static/)
#	什么是Web Worker
Web Workers 使得一个Web应用程序可以在与主执行线程分离的后台线程中运行一个脚本操作。这样做的好处是可以在一个单独的线程中执行费时的处理任务，从而允许主（通常是UI）线程运行而不被阻塞/放慢。
局限性：在worker内不能直接操作DOM节点，或者使用window对象的默认方法和属性。
#	Worker特性检测
	```
	if(window.Worker){
		// todo
	} else {
		// 不支持web Worker
	}	
	```
PS：假设页面为index.html，页面js为main.js，这里的path是相对于index.html到该worker.js。
#	专用Worker
##	生成一个专用worker
```
var myWorker = new Worker('worker.js');
```
PS：假设页面为index.html，页面主线程js为main.js，这里的path是相对于index.html到该worker.js。
##	主线程js和Worker的通信（数据交互）
###	主线程js（main.js 用来生成 myWorker）
```
let myWorker
if(window.Worker){
	// todo
	myWorker = new Worker('./js/worker.js')
} else {
	// 不支持web Worker
	alert('不支持web Worker')
}
let app = new Vue({
  el: '#app',
  data: {
    num: 1000000,
  },
  methods: {
	computed () {
		console.log(`Message posted to worker=${this.num}`);
		// 重点在这里，向 myWorker 发送我们 input 中的值，让它为去做耗时的计算
		myWorker.postMessage(this.num)
	}  
  }
})
```
PS：如果想发送多个消息，可以这样myWorker.postMessage([msg1, msg2...])，对应接收的e.data对象也就是一个数组了，若是对象的话需要序列化，接收的时候需要反序列化。
###	myWorker脚本代码
```
// 有数据发过来，就触发
onmessage = function(e) {
	let num1 = e.data;
	let num2 = 0;
	
	console.time('计算耗时')
	for(let i = 0; i < num1; i++){
		num2 += i;
	}
	console.timeEnd('计算耗时')
	
	console.log(`Worker 计算结果=${num2}`)
	
	// 当我们计算出结果，应该回传
	postMessage(num2);
}
```
我向一个Worker发送一个较大num，然后求出该1到num的整数和。
在页面中input的值分别为：1000000、10000000、100000000各执行了一次计算，最后一次花费了11s左右（算的上耗时计算了吧）。
下面是我的测试截图
![](http://p26lefllv.bkt.clouddn.com/WebWorker.png)
就这么简单，我们就实现了主线程js和WebWorker的双向通信。
##	终止worker
###	在主线程中终止
如果你需要从主线程中立刻终止一个运行中的worker，可以调用worker的terminate 方法：
`myWorker.terminate();`
worker 线程会被立即杀死，不会有任何机会让它完成自己的操作或清理工作。
###	在Worker中终止（自杀）
`close()`

##	处理错误
当然我们刚刚仅仅考虑了正常情况，还有需要错误等待我们处理呢？
当 worker 出现运行中错误时，它的 onerror 事件处理函数会被调用。
它会收到一个扩展了 ErrorEvent 接口的名为 error的事件。
该事件不会冒泡并且可以被取消；为了防止触发默认动作，worker 可以调用错误事件的 preventDefault() 方法。
错误事件有以下三个用户关心的字段：
```
message
	可读性良好的错误消息。
filename
	发生错误的脚本文件名。
lineno
	发生错误时所在脚本文件的行号。
```
##	生成subworker
如果需要的话 worker 能够生成更多的 worker。这就是所谓的subworker，它们必须托管在同源的父页面内。
而且，subworker 解析 URI 时会相对于父 worker 的地址而不是自身页面的地址。这使得 worker 更容易记录它们之间的依赖关系。
##	在Worker中引入脚本与库
Worker 线程能够访问一个全局函数importScripts()来引入脚本，该函数接受0个或者多个URI作为参数来引入资源。
```
importScripts();                       
importScripts('cube.js');                
importScripts('cube1.js', 'cube2');     
```
#	共享Worker（SharedWorker）
一个共享worker可以被多个脚本使用——即使这些脚本正在被不同的window、iframe或者worker访问。
由于SharedWorker 与 专有Worker 非常相似，这里我只是提一下它们的区别。
读者若需要做测试的话，可以考虑在2个html页面中的javascript代码使用的是同一个worker。
##	生成一个共享worker
```
var myWorker = new SharedWorker('worker.js');
// 父级线程中的调用
myWorker.port.start();
// worker线程中的调用, 假设port变量代表一个端口  
port.start(); 
```
一个非常大的区别在于，与一个共享worker通信必须通过端口对象——一个确切的打开的端口供脚本与worker通信（在专用worker中这一部分是隐式进行的）。
在使用start()方法打开端口连接时，如果父级线程和worker线程需要双向通信，那么它们都需要调用start()方法。
##	共享worker中消息的接收和发送
###	主线程发送消息给Worker
改写我们的computed方法（vue组件中）
```
computed () {
	console.log(`Message posted to worker=${this.num}`);
	// 重点在这里，向 myWorker 发送我们 input 中的值，让它为去做耗时的计算
	myWorker.port.postMessage(this.num)
}
```
###	Worker接收到消息并处理及回传
```
onconnect = function(e) {
	var port = e.ports[0];

	port.onmessage = function(e) {
		// 同样e.data为主线程发送的数据
		console.log(e.data)
		//复杂的计算
		let result = e.data*1000*23*3
		// Worker需要回传至主线程
		port.postMessage(result);
	}
}
```
###	主线程接收并处理消息
```
myWorker.port.onmessage = function(e) {
	result2.textContent = e.data;
	console.log('Message received from worker');
}
```
PS：总结差异，主线程和Worker都要执行start()，通信时需要带上port。
#	线程安全
Worker接口会生成真正的操作系统级别的线程，如果你不太小心，那么并发(concurrency)会对你的代码产生有趣的影响。然而，对于 web worker 来说，与其他线程的通信点会被很小心的控制，这意味着你很难引起并发问题。
#	内容安全策略
##	CSP
CSP全称Content Security Policy为了页面内容安全而制定的一系列防护策略. 通过CSP所约束的的规责指定可信的内容来源（这里的内容可以指脚本、图片、iframe、fton、style等等可能的远程的资源）。
可以限制如下资源的加载：
```
	script-src：外部脚本
	style-src：样式表
	img-src：图像
	media-src：媒体文件（音频和视频）
	font-src：字体文件
	object-src：插件（比如 Flash）
	child-src：框架
	frame-ancestors：嵌入的外部资源（比如<frame>、<iframe>、<embed>和<applet>）
	connect-src：HTTP 连接（通过 XHR、WebSockets、EventSource等）
	worker-src：worker脚本
	manifest-src：manifest 文件
```
除了Content-Security-Policy，还有一个Content-Security-Policy-Report-Only字段，表示不执行限制选项，只是记录违反限制的行为。
但它必须与report-uri选项配合使用。
`
Content-Security-Policy-Report-Only: default-src 'self'; ...; report-uri /my_amazing_csp_report_parser;
`
##	运用
###	meta标签
`<meta http-equiv="Content-Security-Policy" content="script-src 'self'; object-src 'none'; style-src cdn.example.org third-party.org; child-src https:">`
###	服务器上设置
以下语句设置在请求头部（Header）
`
Content-Security-Policy "default-src 'self';"
`
如果要为Worker指定CSP策略，可以为Worker脚本的请求的响应的头部设置CSP策略。
这时这个Worker会继承它所属的文档或者创建它的Worker的CSP策略。 
##	worker中数据的接收与发送
在主页面与 worker 之间传递的数据是通过拷贝，而不是共享来完成的。
传递给 worker 的对象需要经过序列化，接下来在另一端还需要反序列化。
>	参考手册
	[测试html5专用线程与共享线程的区别](http://balance9.iteye.com/blog/1992118)
	[MDN-WebWorker](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers)
	[WebWorkerAPI](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API)
	[vue](https://cn.vuejs.org/v2/api/#v-model)
	[HTML5 Web Workers](http://www.runoob.com/html/html5-webworkers.html)
	[CSP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy__by_cnvoid)


