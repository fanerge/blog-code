---
title: 深入浅出Node.js-读书笔记
date: 2017-10-10 20:17:16
category: "NodeJS"
tags: ['js','读书笔记','服务端']
copyright: true
---
##	构建Web应用
###	基础功能
	常见的需求：
1.	请求方法的判断（保存在报文）
	常见的方法有：GET(查看)\POST(更新)\DELETE(删除)\PUT(新建)\CONNECT\HEAD
	通过req.method 来判断
2.	URL的路径解析（保存在报文）
	http://localhost:8080/a.html
	通过req.url 来查找
3.	URL中查询字符串解析（保存在报文）
	?foo=bar&baz=val
	使用Node提供的querystring 模块处理
4.	Cookie的解析（保存在报文）
	网站为了辨别用户身份而储存在用户本地终端（Client Side）上的数据（通常经过加密）。		
	http 为无状态协议。
	数据保存在客户端。
5.	Session（会话）的需求（保存在报文）
	数据保存在服务器端。
	1.基于Cookie 来实现用户和数据的映射。
		原理：在客户端只保存口令，发送请求是通过该口令再去查找对应的数据
	2.通过查询字符串来实现浏览器端和服务器端数据的对应。
		不推荐使用，风险大。
	两种存储方式：
	1.内存
	2.数据工具
		Redis是一个支持网络、基于内存、可选持久性的键值对存储数据库。
6.	Basic认证（保存在报文）
	当客户端与服务端进行请求时，允许通过用户名和密码实现的一种身份认证方式。
7.	数据上传
	思路：先判断数据的格式，再通过对应的解析方法解析。
	1.表单数据的解析
	先判断req.headers['content-type'] === 'application/x-www-formurlencoded'
		querystring.parse(req.rawBody);	
	2.其他格式
		json -- application/json
		xml -- application/xml
8.	任意格式文件的上传处理
	此时需要<form action="/upload" method="post" enctype="multipart/form-data"></form>
	
9.	缓存
	YSlow中提出的缓存规则：
	1.添加Expires 和 Cache-Control 到报文头中。
	2.配置ETags。
	3.让Ajax 可缓存。
	数据上传与安全
	1.内存限制（提交数据占用了所有内存）
		限制上传内容的大小，一旦超过限制，停止接收数据，并响应400状态码。
		通过流式解析，将数据导向到磁盘中，Node只保留文件路径等小数据。
	2.CSRF
		Cross-Site Request Forgery（跨站点请求伪造）
		
###	路由解析
	文件路径型
		静态文件：URL 的路径与网站目录的路径一致，无须转换。
		动态文件：在 MVC 模式流行起来之前，根据文件路径执行动态脚本也是基本的路由方式，
			它的处理原理是Web服务器根据URL路径找到对应的文件，如/index.asp或/index.php。
	MVC
		MVC 模型的主要思想是将业务逻辑按职责分离。
			控制器（Controller），一组行为的集合。
			模型（Model），数据相关的操作和封装。
			视图（View），视图的渲染。
			1.路由解析，根据URL寻找到对应的控制器和行为。
			2.行为调用相关的模型，进行数据操作。
			3.数据操作结束后，调用视图和相关数据进行页面渲染，输出到客户端。
		路由映射
		1.手工映射
			正则匹配
			参数解析
		2.自然映射
	RESTful		
		REST Representational State Transfer （表现层状态转化）		
		POST /user/fanerge 修改用户信息
		DELETE /user/fanerge 删除用户
		PUT /user/fanerge 新建用户		
		GET /user/fanerge 查询用户信息	
		请求方法	
###	中间件
	作用：middleware 来简化和隔离这些基础设施与业务逻辑之间的细节，使开发者更加关注在业务的开发。	
![中间件工作原理](/images/middleware.png)
	中间件设计格式（connect的设计）
	```
	var querystring = function (req, res, next) {
		// TODO
		next();
	}
	```
	
	使用中间件（串联多个中间件）
	```
	app.use('/user/:username', querystring, cookie, session, function (req, res) {
		// 这里处理具体的业务逻辑
	});
	```
	
	异常处理
		同步异常 -- try {} catch (err) {throw err} 
		异步异常
			需要把异常传递出来
			domain 模块	
###	页面渲染
	内容响应
		Content-Encoding: gzip
		Content-Length: 21170
		Content-Type: text/javascript; charset=utf-8
		MIME : Multipurpose Internet Mail Extensions	
		附件下载：有些MIME类的资源不需要在客户端中打开它，只需要弹出并下载它即可。
			Content-Disposition: inline(查看)/attachment(附件下载);
			例如：Content-Disposition: attachment; filename='filename.ext'; // 下载附件并为其命名
		响应JSON
		响应跳转	
	视图渲染
	模板
		如EJS、Pug等
		模板引擎
			语法分解。
			处理表达式。
			生成待执行的语句。
			与数据一起执行，生成最终字符串。
		with的应用	
			模板安全 XSS 解决方案 转义用户的输入
		模板逻辑
		集成文件系统	
		子模板	
		布局视图	
		模板性能
	Bigpipe
		BigPipe是一个重新设计的基础动态网页服务体系。
		前端加载技术，它的提出主要是为了解决重数据页面的加载问题。
		Bigpipe 的解决思路则是将页面分割成多个部分（pagelet），
		先向用户输出没有数据（框架），将每个部分逐步输出到前端，
		再最终渲染填充框架，完成整个网页的渲染。
		这个过程中需要前端js的参与，它负责将后续输出的数据渲染到页面上。
		1.页面布局框架（无数据的）。
		2.后端持续性的数据输出。
		3.前端渲染。
			bigpipe.ready() -- 以一个key注册一个事件。
			bigpipe.set() -- 触发一个事件，进行页面渲染。
##	玩转进程
###	多进程架构
	child_process 模块
	创建子进程
		1.spawn(): 启动一个子进程来执行命令。
		2.exec(): 启动一个子进程来执行命令，与spawn()不同的是其接口不同，
		它有一个回调函数获知子进程的状况。
		3.execFile(): 启动一个子进程来执行可执行文件。
		4.fork(): 与spawn()类似，不同点在于它创建Node的子进程只需指定要执行的Javascript文件模块即可。
		spawn()与exec()、execFile()不同的是，后两者创建时可以指定timeout属性设置超时时间，
		一旦创建的进程运行超过设定的时间将会被杀死。
		exec()与execFile()不同的是，exec()适合执行已有的命令，execFile()适合执行文件。
	进程间通信
		message事件 绑定发送事件
		send()方法 触发发送消息
	句柄传递
		child_process.send(message, [sendHandle]);
		句柄是一种可以用来标识资源的引用，它的内部包含了指向对象的文件描述符。
		比如句柄可以用来标识一个服务器端socket对象、客户端socket对象、UDP套接字、一个管道等。
###	集群稳定之路
	进程事件
		message
		error
		exit
		close
		disconnect
	自动重启
		自杀信号
		负载均衡（轮叫调度）	
			由主进程接受链接，将其依次分发给工作进程。
			cluster 模块
		状态共享
			第三方数据存储 通过轮询
			主动通知 当数据更新时，主动通知子进程。
###	Cluster 模块
		解决多核CPU的利用率问题。
		Cluster 工作原理
			该模块是 child_process 和 net 模块的组合应用。
		Cluster 事件
			fork
			online
			listening
			disconnect
			exit
			setup
##	测试
	测试驱动开发
###	单元测试
	测试代码编写的原则
	1.单一职责
	2.接口抽象
	3.层次分离
	单元测试介绍
	1.断言
		assert 模块
		单元测试中用来保证最小单元是否正常的检测方法。
		用于检查程序在运行时是否满足期望。
		ok(): 判断结果是否为真。
		equal(): 判断实际值余期望值是否相等。
		notEqual(): 判断实际值与期望值是否不相等。
		deepEqual(): 判断实际值余期望值是否深度相等（对象和数组的元素是否相等）。
		notDeepEqual(): 判断实际值与期望值是否不深度相等。
		strictEqual(): 判断实际值与期望值是否严格相等（===）。
		notStrictEqual(): 判断实际值与期望值是否不严格相等（!==）。
		throws(): 判断代码块是否抛出异常。
		doesNotThrow(): 判断代码块是否没有抛出异常。
		ifError(): 判断实际值是否为一个假值（null、undefined、0、''、false），若实际值为真值，将抛出异常。
	2.测试框架
		用于管理测试用例和生成测试报告。
		mocha 模块
		测试风格
			TDD 测试驱动开发
			BDD 行为驱动开发
		测试报告
			mocha --reporters
		测试代码的文件组织
		测试用例
		异步测试
		测试覆盖率
		mock 或者 muk
		私有方法的测试
			var lib = rewire('../lib/index.js'); // 需要测试方法所在的文件
			var litmit = lib.__get__('limit'); // 需要测试的方法
	3.工程化与自动化	
		工程化 -- Makefile
		持续集成 -- travis-ci
##	性能测试
	负载测试、压力测试和基准测试。
	基准测试：（对基本的方法如Array.protoryp.map 和 for循环的比较）
		benchmark 模块
	压力测试：（网络接口进行压力测试）
		常用的工具：ab、siege、http_load
	基准测试驱动开发
		1.写基准测试
		2.写/改代码
		3.收集数据
		4.找出问题
		5.回到第（2）步
	测试数据余业务数据的转换
		PV访问量（Page View），即页面访问量，每打开一次页面PV计数+1，刷新页面也是。
		UV访问数（Unique Visitor）指独立访客访问数，一台电脑终端为一个访客。
		TPS 是每秒内的事务数，比如执行了dml操作，那么相应的tps会增加；
		QPS 是指每秒内查询次数，比如执行了select操作，相应的qps会增加。
		QPS = PV/H (H为访问量集中的时间单位小时)。
##	产品化
	包括：工程化、架构、容灾备份、部署和运维。
###	项目工程化
	1.目录结构
		Web框架：Express、Koa、Egg
	2.构建工具
		合并静态文件、压缩文件大小、打包应用、编译模块。
		Makefile 
			一个工程中的源文件不计其数，其按类型、功能、模块分别放在若干个目录中，
			makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，
			哪些文件需要重新编译，甚至于进行更复杂的功能操作，
			因为 makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。
		Grunt
	3.编码规范
		JSLint JSHint ESLint
	4.代码审查
###	部署流程
	部署环境
	部署操作	
###	性能
	动静分离
		静态请求用 Nginx 和 CDN 来保存
	启用缓存
		Redis 和 Memcached
	多进程架构	
		读写分离
###	日志
	访问日志
	异常日志
	日志与数据库
	分割日志
###	监控报警
	业务逻辑型监控 和 硬件型监控
	监控	
		日志监控
		响应时间
		进程监控
		磁盘监控
		内存监控
		CPU占用监控
		CPU load监控
		I/O负载
		网络监控
		应用状态监控
		DNS 监控 -- 免费DNS监控服务 DNSPod
		报警的实现
			邮件报警 -- nodemailer 模块
			短信或电话报警 
		
	监控系统的稳定性
###	稳定性	
		多机器
		多机房
		容灾备份
###	异构共存
##	调试Node
	Debugger
		1.在代码中插入 debugger
		2.运行 node debug demo.js
	Node Inspector
		1.安装 npm install -g node-inspector
		2.错误堆栈
##	Node 编码规范
###	编码规范
	1.空格与格式 -- 采用2个空格缩进，而不是tab缩进。
	2.变量声明 -- 每个变量声明都应该带var。
	3.空格 -- 操作符前后加空格，如+、-、*、%、/、=等
	4.单双引号的问题 -- 只在html标签的属性中使用双引号，其余使用单引号。
		但在JSON中，严格的规范是要求使用字符串使用双引号，内容中出现双引号时需要转义。
	5.大括号的位置 -- 不需要另起一行
	6.逗号 -- 若逗号不在行结尾，前面需要一个空格。
	7.分号 -- 给表达式结尾添加分号。	
###	命名规范
	1.变量命名 -- 小驼峰式命名。
	2.方法命名 -- 小驼峰式命名，尽量采用动词或判断词汇。
		```
		var getUser = () => {};
		var isAdmin = () => {};
		```
	3.类命名（构造函数和Class） -- 大驼峰式命名。
	4.常量命名 -- 全大写字母和下划线。	
		```
			var PINK_COLOR = 'pink';
		```
		
	5.文件命名 -- 全小写字母和下划线。
		```
		child_process.js // 普通文件
		_linklist.js // 私有文件
		```
	6.包名 -- 不要包含 js 或 node 的字样，它们是重复的。
###	比较操作
	1.使用 === 替代 ==
	2.当遇到 0、undefined、null、false、''假值时，不需要使用 === 或 ==。	
###	字面量	
	尽量使用 {}、[]，不要使用 new Object() 和 new Array()	
###	作用域
	1.慎用with
	```
	with (obj) {
		foo = bar;
	}
	// 出现4中结果：obj.foo = obj.bar; obj.foo = bar; foo = obj.bar; foo = bar;
	```
	2.慎用eval()
###	数组与对象
	1.字面量格式 -- 结尾用逗号分隔，若分行，一行只能一个元素。
	2.for in 循环 -- 只能对对象使用，不能对数组使用。
		for in语句以任意顺序遍历一个对象的可枚举属性（包括原型上的属性）。
	3.不要把数组当对象使用	
###	异步
	1.异步回调函数的第一个参数应该是错误指示	
	2.执行传入的回调函数	
###	类与模块	
	1.类继承（Node推荐的类继承方式）
	```
	function Socket (options) {
		stream.Stream.call(this);
	}
	util.inherits(Socket, stream.Stream);
	```
	2.导出 -- 所有供外部调用的方法或变量均需要挂载在exports变量上。
		当需要将文件当做一个类导出时，需要通过如下方式挂载。
	```
	module.exports = Class;	
	```
###	注解规范
	采用 JSDoc
##	最佳实践
	1.冲突的解决原则 -- 入乡随俗
	2.给编辑器设置检测工具	
	3.版本控制中的hook	
	4.持续集成
##	搭建局域NPM仓库
> 参考文档
	[朴灵-深入浅出Node](https://www.baidu.com/s?ie=utf8&oe=utf8&wd=%E6%B7%B1%E5%85%A5%E6%B5%85%E5%87%BAnode&tn=98010089_dg&ch=1)


		
		
		
		
		
		
		
		
		
		
		
		



















