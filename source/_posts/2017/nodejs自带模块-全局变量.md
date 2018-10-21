---
title: 全局变量-global
date: 2017-09-16 11:49:36
category: "NodeJS"
tags: ['js','web开发','服务端']
---
#	global - 全局变量
	global - 全局变量，相当于浏览器环境下的window对象。
##	__dirname
	作用：当前模块的文件夹名称。等同于 __filename 的 path.dirname() 的值。
##	__filename
	作用：当前模块的文件名称---解析后的绝对路径。
##	module
	作用：导出一个对象。
	module.exports 用于指定一个模块所导出的内容，即可以通过 require() 访问的内容。
##	exports
	作用：导出一个方法，为module.exports的简写形式。
PS:	module.exports和exports的区别
	每一个node.js执行文件，都自动创建一个module对象，同时，module对象会创建一个叫exports的属性，初始化的值是 {}
	module.exports本身不具备任何属性和方法。如果，Module.exports已经具备一些属性和方法，那么exports收集来的信息将被忽略。
##	require(path)
	作用：引入模块。
PS：module导入查找顺序。核心模块 -》 否则将执行下列查找
如果按确切的文件名没有找到模块，则 Node.js 会尝试带上 .js、.json 或 .node 拓展名再加载。
.js 文件会被解析为 JavaScript 文本文件，.json 文件会被解析为 JSON 文本文件。 .node 文件会被解析为通过 dlopen 加载的编译后的插件模块。
以 '/' 为前缀的模块是文件的绝对路径。 例如，require('/home/marco/foo.js') 会加载 /home/marco/foo.js 文件。
以 './' 为前缀的模块是相对于调用 require() 的文件的。 也就是说，circle.js 必须和 foo.js 在同一目录下以便于 require('./circle') 找到它。
当没有以 '/'、'./' 或 '../' 开头来表示文件时，这个模块必须是一个核心模块或加载自 node_modules 目录。
如果给定的路径不存在，则 require() 会抛出一个 code 属性为 'MODULE_NOT_FOUND' 的 Error。	

#	timer（定时器）
##	setImmediate(callback[, ...args])
	作用：创建 Immediate 对象。
##	clearImmediate(immediateObject)
	作用：取消一个由 setImmediate() 创建的 Immediate 对象。
##	setInterval(callback, delay[, ...args])
	作用：创建 Timeout 对象。
##	clearInterval(timeoutObject)
	作用：取消一个由 setInterval() 创建的 Timeout 对象。
##	setTimeout(callback, delay[, ...args])
	作用：创建 Timeout 对象。
##	clearTimeout(timeoutObject)
	作用：取消一个由 setTimeout() 创建的 Timeout 对象。
	
#	events（事件）
包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。
##	绑定监听事件
###	emitter.addListener(eventName, listener)
###	emitter.on(eventName, listener)
###	emitter.once(eventName, listener)
###	emitter.prependListener(eventName, listener)
###	emitter.prependOnceListener(eventName, listener)
PS：添加一个单次 listener 函数到名为 eventName 的事件的监听器数组的开头。 下次触发 eventName 事件时，监听器会被移除，然后调用。
##	触发事件
###	emitter.emit(eventName[, ...args])
PS：如果事件有注册监听返回 true，否则返回 false。
##	移除监听事件
###	emitter.removeListener(eventName, listener)	
###	emitter.removeAllListeners([eventName])	
##	其他
###	'newListener' 事件 
###	'removeListener' 事件
PS：当新的监听器被添加时，所有的 EventEmitter 会触发 'newListener' 事件；当移除已存在的监听器时，则触发 'removeListener'。
###	'error' 事件
PS：当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息。
我们一般要为会触发 error 事件的对象设置监听器，避免遇到错误后整个程序崩溃。
###	EventEmitter.defaultMaxListeners
###	emitter.setMaxListeners(n)
###	emitter.getMaxListeners()
###	emitter.listenerCount(eventName)
PS：返回正在监听名为 eventName 的事件的监听器的数量。
###	emitter.listeners(eventName)
PS：返回名为 eventName 的事件的监听器数组的副本。
###	emitter.eventNames()
PS：返回一个列出触发器已注册监听器的事件的数组。 数组中的值为string或symbol。
	
#	assert（断言）	
assert模块是Node的内置模块，主要用于断言。如果表达式不符合预期，就抛出一个错误,可用于测试模块功能，有少数几个是常用的。

#   console控制台
##  console.assert(value[, message][, ...args])
PS：一个简单的断言测试，验证 value 是否为真。 如果不为真，则抛出 AssertionError。 如果提供了 message，则使用 util.format() 格式化并作为错误信息使用。
##  console.clear()
PS：当 stdout 是一个 TTY 时，调用 console.clear() 将尝试清除 TTY。 当 stdout 不是一个TTY时，该方法什么都不做。
##  console.count([label])
PS：维护一个指定 label 的内部计数器并且输出到 stdout 指定 label 调用 console.count() 的次数。
##  console.countReset([label='default'])
PS：重置指定 label 的内部计数器。
##  console.log([data][, ...args])
console.info([data][, ...args])
console.debug(data[, ...args])
##  console.error([data][, ...args])
console.warn([data][, ...args])
PS：打印到 stderr，并带上换行符。
##  console.group([...label])
console.groupCollapsed()
PS：将后续行的缩进增加两个空格。
console.groupEnd()
PS：将后续行的缩进减少两个空格。
PS：打印到 stdout，并带上换行符。
##  console.time(label)
console.timeEnd(label)
PS：启动一个定时器，用以计算一个操作的持续时间。
##  console.trace([message][, ...args])
PS：打印字符串 'Trace :' 到 stderr ，并通过 util.format() 格式化消息与堆栈跟踪在代码中的当前位置。
##  console.dir(obj[, options])
PS：在 obj 上使用 util.inspect() 并打印结果字符串到 stdout。

#  process - 进程
process 对象是一个全局变量，它提供当前 Node.js 进程的有关信息，以及控制当前 Node.js 进程。
##  process 事件
###  beforeExit 事件
当 Node.js 的事件循环数组已经为空，并且没有额外的工作被添加进来，事件 'beforeExit' 会被触发。
### disconnect 事件
如果 Node.js 进程是由 IPC 通道的方式创建的，当 IPC 通道关闭时，会触发'disconnect'事件。
### exit 事件
两种情况触发：
显式调用 process.exit() 方法，使得 Node.js 进程即将结束；
Node.js 事件循环数组中不再有额外的工作，使得 Node.js 进程即将结束。
### message 事件
如果 Node.js 进程是由 IPC 通道的方式创建的，当子进程收到父进程发送的消息时(消息通过 childprocess.send() 发送），会触发 'message' 事件。
### rejectionHandled 事件
如果有 Promise 被 rejected，并且此 Promise在 Nodje.js 事件循环的下次轮询及之后期间，被绑定了一个错误处理器（例如使用 promise.catch()），会触发 'rejectionHandled' 事件。
### uncaughtException 事件
如果 Javascript 未捕获的异常，沿着代码调用路径反向传递回事件循环，会触发 'uncaughtException' 事件。 Node.js 默认情况下会将这些异常堆栈打印到 stderr 然后进程退出。 为 'uncaughtException' 事件增加监听器会覆盖上述默认行为。
### unhandledRejection 事件
如果在事件循环的一次轮询中，一个 Promise 被 rejected，并且此 Promise 没有绑定错误处理器，'unhandledRejection 事件会被触发。
### warning 事件
任何时候Node.js发出进程告警，都会触发'warning'事件。
##  信号事件
SIGUSR1、SIGTERM 、SIGINT、SIGPIPE等等
##	IPC（Inter-Process Communication，进程间通信）方法
###  process.channel
如果Node.js进程是由IPC channel方式创建的，process.channel属性保存IPC channel的引用。 如果IPC channel不存在，此属性值为undefined。
###  process.connected
如果Node.js进程是由IPC channel方式创建的， 只要IPC channel保持连接，process.connected属性就会返回true。 process.disconnect()被调用后，此属性会返回false。
###  process.disconnect()
如果 Node.js 进程是从IPC频道派生出来的, process.disconnect()函数会关闭到父进程的IPC频道，以允许子进程一旦没有其他链接来保持活跃就优雅地关闭。
###  process.pid
返回进程的PID。
###  process.ppid
返回父进程的id。
###  process.send(message[, sendHandle[, options]][, callback])
如果Node.js进程是通过进程间通信产生的，那么，process.send()方法可以用来给父进程发送消息。 接收到的消息被视为父进程的ChildProcess对象上的一个'message'事件。
##  process.abort()
使Node.js进程立即结束，并生成一个core文件。
##  process.arch
返回CPU体系机构字符串（arm、arm64）。
##  process.execPath
Node.js执行的路径。
##  process.argv
返回一个数组。[process.execPath, 当前执行js文件路径, 剩余的元素为其他命令行参数。]
##  process.argv0
保存Node.js启动时传入的argv[0]参数值的一份只读副本。
##  process.chdir(directory)
变更Node.js进程的当前工作目录，如果变更目录失败会抛出异常(例如，如果指定的目录不存在)。
##  process.config（可读可写）
返回一个Javascript对象。此对象描述了用于编译当前Node.js执行程序时涉及的配置项信息。
##  process.cpuUsage([previousValue])
返回包含当前进程的用户CPU时间和系统CPU时间的对象。
##  process.cwd()
返回 Node.js 进程当前工作的目录。
##  process.emitWarning(warning[, options])
process.emitWarning()方法可用于发出定制的或应用特定的进程警告。
process.emitWarning(warning[, type[, code]][, ctor])
##  process.env
返回一个包含用户环境信息的对象。
##  process.execArgv
返回当Node.js进程被启动时，Node.js特定的命令行选项。如：--harmony
##  process.execPath
process.execPath 属性，返回启动Node.js进程的可执行文件所在的绝对路径。
##  process.exit([code])
process.exit()方法以结束状态码code指示Node.js同步终止进程。
##  process.exitCode
当进程正常结束，或通过process.exit()结束但未传递参数时，此数值标识进程结束的状态码。
##  process.getegid()
返回Node.js进程的有效数字标记的组身份(See getegid(2))。
##  process.geteuid()
返回Node.js进程的有效数字标记的用户身份(See geteuid(2))。
##  process.getgid()
返回Node.js进程的数字标记的组身份(See getgid(2))。
##  process.getgroups()
返回数组，其中包含了补充的组ID。
##  process.getuid()
返回Node.js进程的数字标记的用户身份(See getuid(2))。
##  process.hrtime([time])
返回当前时间以[seconds, nanoseconds] tuple Array表示的高精度解析值， nanoseconds是当前时间无法使用秒的精度表示的剩余部分。
##  process.initgroups(user, extra_group)
读取/etc/group文件，并且初始化组访问列表，该列表包括了用户所在的所有组。
##  process.kill(pid[, signal])
将signal发送给pid标识的进程。
##  process.mainModule
提供了一种获取require.main的替代方式。
##  process.memoryUsage()
返回Node.js进程的内存使用情况的对象，该对象每个属性值的单位为字节。
##  process.nextTick(callback[, ...args])
将 callback 添加到"next tick 队列"。
 每次事件轮询后，在额外的I/O执行前，next tick队列都会优先执行。
##  process.noDeprecation
##  process.platform
返回字符串，标识Node.js进程运行其上的操作系统平台。
##  process.release
返回与当前发布相关的元数据对象，包括源代码和源代码头文件 tarball的URLs。
##  process.setegid(id)
为进程设置有效的用户组ID。
##  process.seteuid(id)
process.seteuid()方法为进程设置有效的用户ID。
##  process.setgid(id)
process.setgid() 为进程方法设置组ID。
##  process.setgroups(groups)
##  process.setuid(id)
设置进程的用户ID。
##  process.stderr
返回连接到stderr(fd 2)的流。
##  process.stdin
返回连接到 stdin (fd 0)的流。
##  process.stdout
返回连接到 stdout (fd 1)的流。
##  process.throwDeprecation
##  process.title
用于获取或设置当前进程在 ps 命令中显示的进程名字。
##  process.traceDeprecation
##  process.umask([mask])
用于返回或设置Node.js进程的默认创建文件的权限掩码。
##  process.uptime()
返回当前 Node.js 进程运行时间秒长。
##  process.version
返回Node.js的版本信息。
##  process.versions
返回一个对象，此对象列出了Node.js和其依赖的版本信息。

#   path（路径）
path 模块提供了一些工具函数，用于处理文件与目录的路径。
path 模块的默认操作会根据 Node.js 应用程序运行的操作系统的不同而变化（Windows与POSIX）。
要想在任何操作系统上处理 Windows 文件路径时获得一致的结果，可以使用 path.win32，如path.win32.basename('C:\\temp\\myfile.html');
要想在任何操作系统上处理 POSIX 文件路径时获得一致的结果，可以使用 path.posix，如path.posix.basename('/tmp/myfile.html');
##  path.basename(path[, ext])
返回一个 path 的最后一部分，类似于 Unix 中的 basename 命令。
##  path.delimiter
提供平台特定的路径分隔符：Windows 上是 ;POSIX 上是 :
##  path.dirname(path)
返回一个 path 的目录名，类似于 Unix 中的 dirname 命令。
##  path.extname(path)
返回 path 的扩展名，即从 path 的最后一部分中的最后一个 .（句号）字符到字符串结束。
##  path.format(pathObject)
会从一个对象返回一个路径字符串。 与 path.parse() 相反。
PS：
如果提供了 pathObject.dir，则 pathObject.root 会被忽略
如果提供了 pathObject.base 存在，则 pathObject.ext 和 pathObject.name 会被忽略
##  path.parse(path)
返回一个对象，对象的属性表示 path 的元素。
##  path.isAbsolute(path)
会判定 path 是否为一个绝对路径。
##  path.join([...paths])
使用平台特定的分隔符把全部给定的 path 片段连接到一起，并规范化生成的路径。
##  path.normalize(path)
会规范化给定的 path，并解析 '..' 和 '.' 片段。
##  path.relative(from, to)
返回从 from 到 to 的相对路径（基于当前工作目录）。
##  path.resolve([...paths])
会把一个路径或路径片段的序列解析为一个绝对路径。
##  path.sep
提供了平台特定的路径片段分隔符：Windows 上是 \POSIX 上是 /

#	url
url 模块提供了一些实用函数，用于 URL 处理与解析。

#	querystring（查询字符串）
querystring 模块提供了一些实用函数，用于解析与格式化 URL 查询字符串。
##	querystring.parse(str[, sep[, eq[, options]]])
该方法会把一个 URL 查询字符串 str 解析成一个键值对的集合。
##	querystring.unescape(str)
该方法是提供给 querystring.parse() 使用的，通常不直接使用。
##	querystring.stringify(obj[, sep[, eq[, options]]])
该方法通过遍历给定的 obj 对象的自身属性，生成 URL 查询字符串。
##	querystring.escape(str)	
该方法是提供给 querystring.stringify() 使用的，通常不直接使用。

#	Buffer（缓冲器）
在 ECMAScript 2015 (ES6) 引入 TypedArray 之前，JavaScript 语言没有读取或操作二进制数据流的机制。 Buffer 类被引入作为 Node.js API 的一部分，使其可以在 TCP 流或文件系统操作等场景中处理二进制数据流。

#	string_decoder（字符串解码器）
string_decoder 模块提供了一个 API，用于把 Buffer 对象解码成字符串，但会保留编码过的多字节 UTF-8 与 UTF-16 字符。

#	child_process（子进程）
child_process 模块提供了衍生子进程的功能，它与 popen(3) 类似，但不完全相同。 这个功能主要由 child_process.spawn() 函数提供。

#	cluster (集群)
Node.js在单个线程中运行单个实例。 用户(开发者)为了使用现在的多核系统，有时候,用户(开发者)会用一串Node.js进程去处理负载任务。

#	crypto (加密)
crypto 模块提供了加密功能，包含对 OpenSSL 的哈希、HMAC、加密、解密、签名、以及验证功能的一整套封装。

#	dgram (数据报)
dgram模块提供了 UDP 数据包 socket 的实现。

#	net (网络)
net 模块提供了创建基于流的 TCP 或 IPC 服务器(net.createServer())和客户端(net.createConnection()) 的异步网络 API。

#	dns (域名服务器)
1.使用底层操作系统工具进行域名解析，且无需进行网络通信。
2.连接到一个真实的 DNS 服务器进行域名解析，且始终使用网络进行 DNS 查询。

#	http
客户端与服务端通信。
Node.js 中的 HTTP 接口被设计成支持协议的许多特性。 比如，大块编码的消息。 这些接口不缓冲完整的请求或响应，用户能够以流的形式处理数据。

#	tls (安全传输层)
tls 模块是对安全传输层（TLS）及安全套接层（SSL）协议的实现，建立在OpenSSL的基础上。

#	https
HTTPS 是 HTTP 基于 TLS/SSL 的版本。在 Node.js 中，它被实现为一个独立的模块。

#	Error (错误)
所有由 Node.js 引起的 JavaScript 错误与系统错误都继承自或实例化自标准的 JavaScript <Error> 类，且保证至少提供类中的属性。

#	fs (文件系统)
文件 I/O 是对标准 POSIX 函数的简单封装。 通过 require('fs') 使用该模块。 所有的方法都有异步和同步的形式。

#	os (操作系统)
os 模块提供了一些操作系统相关的实用方法。

#	readline (逐行读取)
用于从可读流（如 process.stdin）读取数据，每次读取一行。

#	repl (交互式解释器)
repl 模块提供了一种“读取-求值-输出”循环（REPL）的实现，它可作为一个独立的程序或嵌入到其他应用中。

#	stream (流)
流（stream）在 Node.js 中是处理流数据的抽象接口（abstract interface）。 stream 模块提供了基础的 API 。使用这些 API 可以很容易地来构建实现流接口的对象。

#	tty（终端）

#	util (实用工具)
util 模块主要用于支持 Node.js 内部 API 的需求。 大部分实用工具也可用于应用程序与模块开发者。

#	V8（引擎）
v8 模块暴露了特定于V8版本内置到 Node.js 二进制文件中的API。

#	vm (虚拟机)
vm 模块提供了一系列 API 用于在 V8 虚拟机环境中编译和运行代码。

#	Zlib（压缩）
zlib模块提供通过 Gzip 和 Deflate/Inflate 实现的压缩功能。












