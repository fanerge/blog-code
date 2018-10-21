---
title: HTTP模块
date: 2017-09-16 17:06:40
category: "NodeJS"
tags: ['js','web开发','服务端']
---
###	要使用 HTTP 服务器与客户端，需要 require('http')。
	Node.js 中的 HTTP 接口被设计成支持协议的许多特性。 比如，大块编码的消息。 这些接口不缓冲完整的请求或响应，用户能够以流的形式处理数据。
###	http.Agent 类
	Agent 负责为 HTTP 客户端管理连接的持续与复用。 它为一个给定的主机与端口维护着一个等待请求的队列，且为每个请求重复使用一个单一的 socket 连接直到队列为空，此时 socket 会被销毁或被放入一个连接池中，在连接池中等待被有着相同主机与端口的请求再次使用。 是否被销毁或被放入连接池取决于 keepAlive 选项。
1.	new Agent([options])
		作用：实例化一个代理 代理的配置选项。
		var agent = new Agent([options]);
2.	agent.createConnection(options[, callback])
		作用：创建一个用于 HTTP 请求的 socket 或流。
3.	agent.keepSocketAlive(socket)
		作用：在 socket 被请求分离的时候调用, 可能被代理持续使用。
4.	agent.reuseSocket(socket, request)
		作用：由于 keep-alive 选项被保持持久化, 在 socket 附加到 request 时调用。
5.	agent.destroy()
		作用：销毁当前正被代理使用的任何 socket。
6.	agent.freeSockets
		作用：返回一个对象，包含当前正在等待被启用了 keepAlive 的代理使用的 socket 数组。 不要修改该属性。
7.	agent.getName(options)
		作用：为请求选项的集合获取一个唯一的名称，用来判断一个连接是否可以被复用。
8.	agent.maxFreeSockets
		作用：默认为 256。 对于已启用 keepAlive 的代理，该属性可设置要保留的空闲 socket 的最大数量。
9.	agent.maxSockets
		作用：默认为不限制。 该属性可设置代理为每个来源打开的并发 socket 的最大数量。
10.	agent.requests
		作用：返回一个对象，包含还未被分配到 socket 的请求队列。 不要修改。
11.	agent.sockets
		作用：返回一个对象，包含当前正被代理使用的 socket 数组。 不要修改。
###	http.ClientRequest 类
	该对象在 http.request() 内部被创建并返回。 它表示着一个正在处理的请求，其请求头已进入队列。 请求头仍可使用 setHeader(name, value)、getHeader(name) 和 removeHeader(name) API 进行修改。
1.	abort事件
	当请求已被客户端终止时触发。 该事件仅在首次调用 abort() 时触发。
2.	aborted事件
	当请求已被服务器终止且网络 socket 已关闭时触发。
3.	connect事件
	每当服务器响应 CONNECT 请求时触发。
4.	continue事件
	当服务器发送了一个 100 Continue 的 HTTP 响应时触发，通常是因为请求包含 Expect: 100-continue。 这是客户端将要发送请求主体的指令。
5.	response事件
	当请求的响应被接收到时触发。 该事件只触发一次。
6.	socket事件
	当 socket 被分配到请求后触发。
7.	upgrade事件
	每当服务器响应 upgrade 请求时触发。 
8.	request.abort()
	标记请求为终止。 调用该方法将使响应中剩余的数据被丢弃且 socket 被销毁。
9.	request.aborted
	如果请求已被终止，则该属性的值为请求被终止的时间，从 1 January 1970 00:00:00 UTC 到现在的毫秒数。
10.	request.connection
11.	request.end([data[, encoding]][, callback])
	结束发送请求。
12.	request.flushHeaders()
	刷新请求头。
13.	request.setNoDelay([noDelay])
	一旦 socket 被分配给请求且已连接，socket.setNoDelay() 会被调用。
14.	request.setSocketKeepAlive([enable][, initialDelay])
	一旦 socket 被分配给请求且已连接，socket.setKeepAlive() 会被调用。
15.	request.setTimeout(timeout[, callback])
	一旦 socket 被分配给请求且已连接，socket.setTimeout() 会被调用。
16.	request.socket
	引用底层socket。
17.	request.write(chunk[, encoding][, callback])
	发送请求主体的一个数据块。
###	http.Server 类
	该类继承自 net.Server，且具有以下额外的事件
1.	checkContinue 事件
	每当接收到一个带有 HTTP Expect: 100-continue 请求头的请求时触发。
2.	checkExpectation 事件
	每当接收到一个带有 HTTP Expect 请求头（值不为 100-continue）的请求时触发。 
3.	clientError 事件
	如果客户端触发了一个 'error' 事件，则它会被传递到这里。
4.	close事件
	当服务器关闭时触发。
5.	connect事件
	每当客户端发送 HTTP CONNECT 请求时触发。
6.	connection事件
	当一个新的 TCP 流被建立时触发。 
7.	request事件
	每次接收到一个请求时触发。 
8.	upgrade事件	
	每当客户端发送 HTTP upgrade 请求时触发。
9.	server.close([callback])
	停止服务端接收新的连接。
10.	server.listen(handle[, callback])
	handle 对象可以被设为一个服务器或 socket（任何带有一个 _handle 成员的对象）、或一个 {fd: <n>} 对象。
11.	server.listen(path[, callback])
	启动一个 UNIX socket 服务器，并在给定的 path 上监听连接。
12.	server.listen([port][, hostname][, backlog][, callback])
	开始在指定的 port 和 hostname 上接受连接。
13.	server.listening
	返回一个布尔值，表示服务器是否正在监听连接。
14.	server.maxHeadersCount
	限制请求头的最大数量，默认为 2000。 如果设为 0，则没有限制。
15.	server.setTimeout([msecs][, callback])
	设置 socket 的超时时间。
16.	server.timeout
	socket 被认定为超时的空闲毫秒数。
17.	server.keepAliveTimeout
	服务器完成最后的响应之后需要等待的额外的传入数据的活跃毫秒数, socket 才能被销毁。
###	http.ServerResponse 类
	该对象在 HTTP 服务器内部被创建。 它作为第二个参数被传入 'request' 事件。
1.	close事件
	当底层连接在 response.end() 被调用或能够刷新之前被终止时触发。
2.	finish事件
	当响应已被发送时触发。 
3.	response.addTrailers(headers)
	该方法会添加 HTTP 尾部响应头（一种在消息尾部的响应头）到响应。
4.	response.connection
5.	response.end([data][, encoding][, callback])
	该方法会通知服务器，所有响应头和响应主体都已被发送，即服务器将其视为已完成。 
6.	response.finished
	返回一个布尔值，表示响应是否已完成。
7.	response.getHeader(name)
	读取一个已入队列但尚未发送到客户端的响应头。
8.	response.getHeaderNames()
	返回一个包含当前响应唯一名称的 http 头信息名称数组. 名称均为小写。
9.	response.getHeaders()
	返回当前响应头文件的浅拷贝。
10.	response.hasHeader(name)
	如果响应头当前有设置 name 头部，返回 true。
11.	response.headersSent	
	返回一个布尔值（只读）。 
12.	response.removeHeader(name)
	从隐式发送的队列中移除一个响应头。
13.	response.sendDate	
	当为 true 时，如果响应头里没有日期响应头，则日期响应头会被自动生成并发送。
14.	response.setHeader(name, value)	
	为一个隐式的响应头设置值。
15.	response.setTimeout(msecs[, callback])
	设置 socket 的超时时间为 msecs。
16.	response.socket
	引用底层socket。 
17.	response.statusCode
	当使用隐式的响应头时（没有显式地调用 response.writeHead()），该属性控制响应头刷新时将被发送到客户端的状态码。
18.	response.statusMessage
	使用隐式的响应头时（没有显式地调用 response.writeHead()），该属性控制响应头刷新时将被发送到客户端的状态信息。
19.	response.write(chunk[, encoding][, callback])
	如果该方法被调用且 response.writeHead() 没有被调用，则它会切换到隐式响应头模式并刷新隐式响应头。
20.	response.writeContinue()
	发送一个 HTTP/1.1 100 Continue 消息到客户端，表示请求主体可以开始发送。 参阅 Server 的 'checkContinue' 事件。
21.	response.writeHead(statusCode[, statusMessage][, headers])
	发送一个响应头给请求。 
###	http.IncomingMessage 类
	IncomingMessage 对象由 http.Server 或 http.ClientRequest 创建，并作为第一个参数分别递给 'request' 和 'response' 事件。 它可以用来访问响应状态、消息头、以及数据。
>	参考文档：
	[HTTP参考-api](http://nodejs.cn/api/http.html)