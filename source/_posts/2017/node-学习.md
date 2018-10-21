---
title: node-学习（七天学会NodeJs）
date: 2017-09-05 22:32:47
category: "NodeJS"
tags: ['js','web开发','服务端','读书笔记']
---
####	NodeJs基础
#####	模块
	在编写每个模块时，都有require、exports、module三个预先定义好的变量可供使用。
######	require
	require函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。模块名可使用相对路径（以./开头），或者是绝对路径（以/或C:之类的盘符开头）。另外，模块名中的.js扩展名可以省略。
	```
	var foo1 = require('./foo');
	var foo2 = require('./foo.js');
	var foo3 = require('/home/user/foo');
	var foo4 = require('/home/user/foo.js');
	// foo1至foo4中保存的是同一个模块的导出对象。
	```
	还可以加载使用JSON文件
	```
	var data = require('./data.json');
	```
######	exports
	exports对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过require函数使用当前模块时得到的就是当前模块的exports对象。
	以下例子中导出了一个公有方法。
	```
	exports.hello = function () {
		console.log('fanerge');
	};
	```
######	module
	通过module对象可以访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象。
	例如模块导出对象默认是一个普通对象，如果想改成一个函数的话，可以使用以下方式。
	```
	module.exports = function () {
		console.log('fanerge');
	};
	```

####	代码的组织和部署
#####	模块路径的解析规则（先后顺序）
1.	内置模块
	如果传递给require函数的是NodeJS内置模块名称，不做路径解析，直接返回内部模块的导出对象，例如require('fs')。
2.	node_modules目录（第三方包）
	NodeJS定义了一个特殊的node_modules目录用于存放模块。例如某个模块的绝对路径是/home/user/hello.js，在该模块中使用require('foo/bar')方式加载模块时，则NodeJS依次尝试使用以下路径。
	```
	/home/user/node_modules/foo/bar
	/home/node_modules/foo/bar
	/node_modules/foo/bar
	```
3.	NODE_PATH环境变量
	与PATH环境变量类似，NodeJS允许通过NODE_PATH环境变量来指定额外的模块搜索路径。NODE_PATH环境变量中包含一到多个目录路径，路径之间在Linux下使用:分隔，在Windows下使用;分隔。例如定义了以下NODE_PATH环境变量：
	```
	NODE_PATH=/home/user/lib:/home/lib
	```
	当使用require('foo/bar')的方式加载模块时，则NodeJS依次尝试以下路径。
	```
	/home/user/lib/foo/bar
	/home/lib/foo/bar
	```
#####	包（package）
	我们已经知道了JS模块的基本单位是单个JS文件，但复杂些的模块往往由多个子模块组成。为了便于管理和使用，我们可以把由多个子模块组成的大模块称做包，并把所有子模块放在同一个目录里。在组成一个包的所有子模块中，需要有一个入口模块，入口模块的导出对象被作为包的导出对象。
	示例一个标准包：
	-cat:
		doc -- 包说明文档
		-lib -- 包具体代码
			head.js
			body.js
			index.js -- 入口文件
		tests -- 测试用例
		package.json -- 包的说明及依赖关系
	```
	// index.js具体代码
	var head = require('./head');
	var body = require('./body');
	exports.create = function (name) {
		return {
			name: name,
			head: head.create(),
			body: body.create()
		};
	};
	// package.json具体代码
	{
		"name": "cat",
		"main": "./lib/index.js" // 入口模块位置
	}
	```
#####	工程目录
	一个标准的工程目录
	/home/user/workspace/node-echo/ # 工程目录
		-bin/                       # 存放命令行相关代码
			node-echo
		+doc/                       # 存放文档
		-lib/                       # 存放API相关代码
			echo.js
		-node_modules/              # 存放第三方包
			babel
		+tests/                     # 存放测试用例
		package.json                # 元数据文件
		README.md					# 说明文件
#####	npm	
	安装第三方包
	```
	npm install argv
	```
	安装第三方包（特定版本）
	```
	npm install argv@0.0.1
	```
	批量安装
	还可以在package.json中dependencies字段中写入所有依赖包
	```
	"dependencies": {
        "argv": "0.0.2",
		...
    }
	// 在使用指令批量安装
	npm install
	```
	更新包
	```
	npm update <package>
	```
	清除NPM本地缓存
	```
	npm cache clear（用于对付使用相同版本号发布新版本代码的人）
	```
	
####	文件操作
#####	文件拷贝练手
	// copy.js
	```
	var fs = require('fs');
	function copy(src, dst) {
		fs.writeFileSync(dst, fs.readFileSync(src));
	}
	function main(argv) {
		copy(argv[0], argv[1]);
	}
	main(process.argv.slice(0, 2));
	// 进行拷贝
	node copy.js
	```
	以上程序使用fs.readFileSync从源路径读取文件内容，并使用fs.writeFileSync将文件内容写入目标路径。
	process是一个全局变量，可通过process.argv获得命令行参数。由于argv[0]固定等于NodeJS执行程序的绝对路径，argv[1]固定等于主模块的绝对路径
#####	文件操作有关的API
######	Buffer（数据块）
	NodeJS提供了一个与String对等的全局构造函数Buffer来提供对二进制数据的操作。
	```
	// 构造一个Buffer实例
	var bin = new Buffer([ 0x68, 0x65, 0x6c, 0x6c, 0x6f ]);
	// Buffer实例具有length属性和bin[index]
	bin[0]; // => 0x68;
	// Buffer实例转化指定编码的字符串
	var str = bin.toString('utf-8'); // => "hello"
	// 将字符串转化为指定编码的二进制数据
	var bin = new Buffer('hello', 'utf-8'); // => <Buffer 68 65 6c 6c 6f>
	// Buffer与字符串有一个重要区别。字符串是只读的，并且对字符串的任何修改得到的都是一个新字符串，原字符串保持不变。
	// 至于Buffer，更像是可以做指针操作的C语言数组。例如，可以用[index]方式直接修改某个位置的字节。
	```
	Buffer拷贝的例子
	```
	// 如果想要拷贝一份Buffer，得首先创建一个新的Buffer，并通过.copy方法把原Buffer中的数据复制过去。
	// 这个类似于申请一块新的内存，并把已有内存中的数据复制过去。
	var bin = new Buffer([0x68, 0x65, 0x6c, 0x6c, 0x6f]);
	var dup = new Buffer(bin.length);
	bin.copy(dup);
	dup[0] =0x46;
	console.log(bin, dup);
	```
######	Stream（数据流）
	Stream的使用场景：
1.	当内存中无法一次装下需要处理的数据时。
2.	一边读取一边处理更加高效时，我们就需要用到数据流。
	实例--将a.js拷贝到b.js
	```
	var fs = require('fs');
	var rs = fs.createReadStream(process.argv[1].slice(0, -7) + 'a.js');
	var ws = fs.createWriteStream(process.argv[1].slice(0, -7) + 'b.js');

	rs.on('data', function (chunk) {
		// 传入的数据是否写入目标
		if (ws.write(chunk) === false) {
				rs.pause();
			}
	});

	rs.on('end', function () {
		ws.end();
	});
	// 判断什么时候只写数据流已经将缓存中的数据写入目标，可以传入下一个待写数据了
	ws.on('drain', function () {
		rs.resume();
	});
	```
######	File System（文件系统）
	NodeJS通过fs内置模块提供对文件的操作。fs模块提供的API基本上可以分为以下三类：
1.	文件属性读写。
	其中常用的有fs.stat、fs.chmod、fs.chown等等。
2.	文件内容读写。
	其中常用的有fs.readFile、fs.readdir、fs.writeFile、fs.mkdir等等。
3.	底层文件操作。
	其中常用的有fs.open、fs.read、fs.write、fs.close等等
	同步API除了方法名的末尾多了一个Sync之外，异常对象与执行结果的传递方式也有相应变化。
	// 异步处理文件及异常处理
	```
	var fs = require('fs');
	fs.readFile(process.argv[1].slice(0, -7) + 'a.js', function (error, data) {
		if (error) {
			console.error(error);
		} else {
			console.log(data);
		}
	});
	```
	// 同步处理文件及异常处理
	```
	var fs = require('fs');
	try{
		var data = fs.readFileSync(process.argv[1].slice(0, -7) + 'a.js');
		console.log(data);
	}catch(err){
		console.error(err)
	}
	```
######	Path（路径）
	NodeJS提供了path内置模块来简化路径相关操作，并提升代码可读性。
	path.normalize(str)
		将传入的路径转换为标准路径，除了解析路径中的.与..外，还能去掉多余的斜杠。
	```
	var fs = require('fs');
	var path = require('path');
	var cache = {};
	function store (key, value) {
		cache[path.normalize(key)] = value;
		console.dir(cache);
	}
	store('/home', 1);
	store('/home/user', 44);
	// 标准化之后的路径里的斜杠在Windows系统下是\，而在Linux系统下是/。如果想保证任何系统下都使用/作为路径分隔符的话，需要用.replace(/\\/g, '/')再替换一下标准路径。
	```
	path.join()
	将传入的多个路径拼接为标准路径。该方法可避免手工拼接路径字符串的繁琐，并且能在不同系统下正确使用相应的路径分隔符。
	```
	path.join('foo/', 'baz/', '../bar'); // => "foo/bar"
	```
	path.extname()
	当我们需要根据不同文件扩展名做不同操作时，该方法就显得很好用。
	```
	path.extname('foo/bar.js'); // => ".js"
	```
#####	遍历目录
	遍历目录是操作文件时的一个常见需求。比如写一个程序，需要找到并处理指定目录下的所有JS文件时，就需要遍历整个目录。
######	递归算法
	计算N的阶乘
	```
	function factorial (n) {
		if (n === 1) {
			return 1;
		} else {
			return n * factorial(n-1);	
		}
	}
	// 使用递归算法编写的代码虽然简洁，但由于每递归一次就产生一次函数调用，在需要优先考虑性能时，需要把递归算法转换为循环算法，以减少函数调用次数。
	```
######	遍历算法
	目录是一个树状结构，在遍历时一般使用深度优先+先序遍历算法。
	同步遍历
	```
	function travel (dir, callback) {
		try {
			fs.readdirSync(dir).forEach(function (file) {
				var pathname = path.join(dir, file);
				if (fs.statSync(pathname).isDirectory()) {
					travel(pathname, callback);
				} else {
					callback(pathname);
				}
			});
		}catch(e){
			console.error(e);
		}
	}
	```
	异步遍历（有点复杂）
	```
	function travel(dir, callback, finish) {
		fs.readdir(dir, function (err, files) {
			(function next(i) {
				if (i < files.length) {
					var pathname = path.join(dir, files[i]);

					fs.stat(pathname, function (err, stats) {
						if (stats.isDirectory()) {
							travel(pathname, callback, function () {
								next(i + 1);
							});
						} else {
							callback(pathname, function () {
								next(i + 1);
							});
						}
					});
				} else {
					finish && finish();
				}
			}(0));
		});
	}
	```
#####	文本编码
我们常用的文本编码有UTF8和GBK两种，并且UTF8文件还可能带有BOM。在读取不同编码的文本文件时，需要将文件内容转换为JS使用的UTF8编码字符串后才能正常处理。
######	BOM的移除
	```
	function readText (pathname) {
		var bin = fs.readFileAync(pathname);
		if (bin[0] === 0xFF && bin[1] === oxBB && bin[2] === 0xBF) {
			bin = bin.slice(3);
		}
		return bin.toString('utf-8');
	}
	```
######	GBK转UTF8
	```
	// 第三方包转换编码
	var iconv = require('iconv-lite'); 
	function readGBKText(pathname) {
		var bin = fs.readFileSync(pathname);
		return iconv.decode(bin, 'gbk');
	}
	```
######	单字节编码
	不管大于0xEF的单个字节在单字节编码下被解析成什么乱码字符，使用同样的单字节编码保存这些乱码字符时，背后对应的字节保持不变。
	```
	function replace(pathname) {
		var str = fs.readFileSync(pathname, 'binary');
		str = str.replace('foo', 'bar');
		fs.writeFileSync(pathname, str, 'binary');
	}
	```

####	网络操作
#####	例子开启一个服务
	```
	http.createServer(function (request, response) {
		response.writeHead(200, {'Content-Type': 'text-plain'});
		response.end('Hello world\n');
	}).listen(8734);
	// 以上程序创建了一个HTTP服务器并监听8734端口，打开浏览器访问该端口http://127.0.0.1:8124/就能够看到效果。
	```
#####	网络相关的API
######	HTTP
	'http'模块提供两种使用方式：
1.	作为服务端使用时，创建一个HTTP服务器，监听HTTP客户端请求并返回响应。
2.	作为客户端使用时，发起一个HTTP客户端请求，获取服务端响应。
	在回调函数中，除了可以使用request对象访问请求头数据外，还能把request对象当作一个只读数据流来访问请求体数据。
	除了可以使用response对象来写入响应头数据外，还能把response对象当作一个只写数据流来写入响应体数据。
	```
	http.createServer(function (request, response) {
		var body = [];
		console.log(request.method);
		console.log(request.headers);
		request.on('data', function (chunk) {
			body.push(chunk);
		});
		request.on('end', function () {
			body = Buffer.concat(body);
			console.log(body.toString());
		});
	}).listen(8734);
	```
######	HTTPS	
	https模块与http模块极为类似，区别在于https模块需要额外处理SSL证书。
	创建一个HTTPS服务器
	```
	var options = {
        key: fs.readFileSync('./ssl/default.key'),
        cert: fs.readFileSync('./ssl/default.cer')
    };
	var server = https.createServer(options, function (request, response) {
			// ...
	});
	// 与创建HTTP服务器相比，多了一个options对象，通过key和cert字段指定了HTTPS服务器使用的私钥和公钥。
	```
	另外，NodeJS支持SNI技术，可以根据HTTPS客户端请求使用的域名动态使用不同的证书，因此同一个HTTPS服务器可以使用多个域名提供服务。
	```
	server.addContext('foo.com', {
		key: fs.readFileSync('./ssl/foo.com.key'),
		cert: fs.readFileSync('./ssl/foo.com.cer')
	});
	```
######	URL
	处理HTTP请求时url模块使用率超高，因为该模块允许解析URL、生成URL，以及拼接URL。
1.	url.parse()
	将一个URL字符串转换为URL对象
	```
	var url = require('url', [boolean], [boolean]);
	console.log(url.parse('http://user:pass@host.com:8080/p/a/t/h?query=string#hash'));
	第二个参数等于true时，该方法返回的URL对象中，query字段不再是一个字符串，而是一个经过querystring模块转换后的参数对象。
	第三个参数等于true时，该方法可以正确解析不带协议头的URL，例如//www.example.com/foo/bar。
	```
2.	url.format()
	允许将一个URL对象转换为URL字符串
3.	url.resolve()
	可以用于拼接URL
	```
	var dd = url.resolve('http://www.baidu.com/yzf/age/sex', '../va');
	// http://www.baidu.com/yzf/va

	```
######	Query String
	querystring模块用于实现URL参数字符串与参数对象的互相转换.
	querystring.parse()
		将字符串参数转化为对象URL参数
	querystring.stringify()
		将参数对象转化为URL参数字符串
######	Zlib
	zlib模块提供了数据压缩和解压的功能。当我们处理HTTP请求和响应时，可能需要用到这个模块。
	例子：使用zlib模块压缩HTTP响应体数据。
	这个例子中，判断了客户端是否支持gzip，并在支持的情况下使用zlib模块返回gzip之后的响应体数据。
	zlib.gzip()
		数据压缩
	zlib.gunzip()
		数据解压
	```
	http.createServer(function (request, response) {
		let i = 1024,
			data = '';
		while (i--) {
			data += 'x';
		}
		if (request.headers['accept-encoding'].includes('gzip')) {
			zlib.gzip(data, function (err, data) {
				response.writeHead(200, {
					'Content-Type': 'text/plain',
					'Content-Encoding': 'gzip'
				});
				response.end(data);
			})
		} else {
			response.writeHead(200, {
				'Content-Type': 'text/plain'
			});
			response.end(data);
		}
	}).listen(8888);
	```
######	Net
	net模块可用于创建Socket服务器或Socket客户端。
	使用Socket搭建一个HTTP服务器的例子。
	```
	net.createServer(function (conn) {
		conn.on('data', function (data) {
			conn.write([
				'HTTP/1.1 200 OK',
				'Content-Type: text/plain',
				'Content-Length: 12',
				'',
				'Hello World'
			].join('\n'));
		});
	}).listen(8888);
	```
	
####	进程管理
	NodeJS可以感知和控制自身进程的运行环境和状态，也可以创建子进程并与其协同工作，这使得NodeJS可以把多个程序组合在一起共同完成某项工作，并在其中充当胶水和调度器的作用。
	node.js调用终端简化目录拷贝
	```
	var child_process = require('child_process');
	var util = require('util');
	function copy(source, target, callback) {
		child_process.exec(
			util.format('cp -r %s/* %s', source, target), callback);
	}
	copy(process.argv[1].slice(0, -7) + 'copy1', process.argv[1].slice(0, -7) + 'copy2', function (err, data) {
		if (err) {
			console.log(err)
		}
		console.log(data)
	});
	```
#####	Process
	任何一个进程都有启动进程时使用的命令行参数，有标准输入标准输出，有运行权限，有运行环境和运行状态。
	另外需要注意的是，process不是内置模块，而是一个全局对象，因此在任何地方都可以直接使用。
#####	Child Process
	使用child_process模块可以创建和控制子进程。该模块提供的API中最核心的是.spawn，其余API都是针对特定使用场景对它的进一步封装，算是一种语法糖。
#####	Cluster
	cluster模块是对child_process模块的进一步封装，专用于解决单进程NodeJS Web服务器无法充分利用多核CPU的问题。使用该模块可以简化多进程服务器程序的开发，让每个核上运行一个工作进程，并统一通过主进程监听端口和分发请求。
应用场景
1.	如何获取命令行参数
		在NodeJS中可以通过process.argv获取命令行参数。
		但是比较意外的是，node执行程序路径和主模块文件路径固定占据了argv[0]和argv[1]两个位置，而第一个命令行参数从argv[2]开始。
		一般这样获取：process.argv.slice(2)
2.	如何退出程序
	```
	try {
		// ...
	} catch (err) {
		// ...
		process.exit(1); // 返回特定的状态码
	}
	```
3.	如何控制输入输出
	NodeJS程序的标准输入流（stdin）、一个标准输出流（stdout）、一个标准错误流（stderr）分别对应process.stdin、process.stdout和process.stderr，
	第一个是只读数据流，后边两个是只写数据流，对它们的操作按照对数据流的操作方式即可。
4.	如何降权
	在Linux系统下，我们知道需要使用root权限才能监听1024以下端口。但是一旦完成端口监听后，继续让程序运行在root权限下存在安全隐患，因此最好能把权限降下来。
	```
	http.createServer(callback).listen(80, function () {
		var env = process.env,
			uid = parseInt(env['SUDO_UID'] || process.getuid(), 10),
			gid = parseInt(env['SUDO_GID'] || process.getgid(), 10);
		process.setgid(gid);
		process.setuid(uid);
	});
	```
5.	如何创建子进程
	创建NodeJS子进程
	```
	var child = child_process.spawn('node', [ 'a.js' ]);
	child.stdout.on('data', function (data) {
		console.log('stdout: ' + data);
	});
	child.stderr.on('data', function (data) {
		console.log('stderr: ' + data);
	});
	child.on('close', function (code) {
		console.log('child process exited with code ' + code);
	});
	```
6.	进程间如何通讯	
	```
	/* parent.js */
	var child = child_process.spawn('node', [ 'child.js' ]);

	child.kill('SIGTERM');

	/* child.js */
	process.on('SIGTERM', function () {
		cleanUp();
		process.exit(0);
	});
	```
7.	进程间如何通讯
	如果父子进程都是NodeJS进程，就可以通过IPC（进程间通讯）双向传递数据。
	```
	/* parent.js */
	var child = child_process.spawn('node', [ 'child.js' ], {
			stdio: [ 0, 1, 2, 'ipc' ]
		});

	child.on('message', function (msg) {
		console.log(msg);
	});

	child.send({ hello: 'hello' });

	/* child.js */
	process.on('message', function (msg) {
		msg.hello = msg.hello.toUpperCase();
		process.send(msg);
	});
	```
8.	如何守护子进程
	守护进程一般用于监控工作进程的运行状态，在工作进程不正常退出时重启工作进程，保障工作进程不间断运行。
	```
	function spawn(mainModule) {
		var worker = child_process.spawn('node', [ mainModule ]);
		worker.on('exit', function (code) {
			if (code !== 0) {
				spawn(mainModule);
			}
		});
	}
	spawn('worker.js');
	```
####	异步编程
	NodeJS最大的卖点——事件机制和异步IO。
#####	回调
	在代码中，异步编程的直接体现就是回调。异步编程依托于回调来实现，但不能说使用了回调后程序就异步化了。
	```
	setTimeout(function () {
		console.log('我是setTimeout')
	}, 1000);
	console.log('hello');
	```
	理解js中如何实现异步
		JS本身是单线程的，无法异步执行，因此我们可以认为setTimeout这类JS规范之外的由运行环境提供的特殊函数做的
		事情是创建一个平行线程后立即返回，让JS主进程可以接着执行后续代码，并在收到平行进程的通知后再执行回调函数。
		我们仍然回到JS是单线程运行的这个事实上，这决定了JS在执行完一段代码之前无法执行包括回调函数在内的别的代码。
		也就是说，即使平行线程完成工作了，通知JS主线程执行回调函数了，回调函数也要等到JS主线程空闲时才能开始执行。
#####	代码设计模式
######	函数返回值
	使用一个函数的输出作为另一个函数的输入是很常见的需求。
	同步方式编写代码：
	```
	var output = fn1(fn2('input'));
	```
	异步方式编写代码：
	由于函数执行结果不是通过返回值，而是通过回调函数传递。
	```
	fn2('input', function (output2) {
		fn1(output2, function (output1) {
			// do something
		});
	});
	```
######	遍历数组	
	在遍历数组时，使用某个函数依次对数据成员做一些处理也是常见的需求。
	同步方式编写代码：
	```
	var len = arr.length;
	for (let i = 0; i < len; i++) {
		arr[i] = sync(arr[i]);
	}
	// 所有的数组项处理完，打算做的事
	```
	异步方式编写代码(异步串行遍历)：
	```
	((function next (i, len, callback) {
		if (i < len) {
			async(arr[i], function (value) {
				arr[i] = value;
				next(i + 1, len, callback);
			});
		} else {
			callback();
		}
	})(0, arr.length, function () {
		// 所有的数组项处理完，打算做的事
	}));
	```
	异步方式编写代码(异步并行遍历)：
	```
	((function (i, len, count, callback) {
		for (; i < len; i++) {
			(function (i) {
				async(arr[i], function (value) {
					arr[i] = value;
					if (++count === len) {
						callback();
					}
				})
			})(i);
		}
	})(0, arr.length, 0, function () {
		// 所有的数组项处理完，打算做的事
	}))
	```
######	异常处理
	JS自身提供的异常捕获和处理机制——try..catch..，只能用于同步执行的代码。
	同步异常处理：
	因为代码执行路径被打断了，我们就需要在异常冒泡到断点之前用try语句把异常捕获住，并通过回调函数传递被捕获的异常。
	```
	function sync(fn) {
		return fn();
	}

	try {
		sync(null);
		// Do something.
	} catch (err) {
		console.log('Error: %s', err.message);
	}
	```
	异步异常处理：
	但由于异步函数会打断代码执行路径，异步函数执行过程中以及执行之后产生的异常冒泡到执行路径被打断的位置时，如果一直没有遇到try语句，就作为一个全局异常抛出。
	因为代码执行路径被打断了，我们就需要在异常冒泡到断点之前用try语句把异常捕获住，并通过回调函数传递被捕获的异常。
#####	域（Domain）
	NodeJS提供了domain模块，可以简化异步代码的异常处理。
	一个域就是一个JS运行环境，在一个运行环境中，如果一个异常没有被捕获，将作为一个全局异常被抛出。NodeJS通过process对象提供了捕获全局异常的方法。
	```
	process.on('uncaughtException', function (err) {
		console.log('Error: %s', err.message);
	});

	setTimeout(function (fn) {
		fn();
	});
	```
	使用domain模块创建一个子域（JS子运行环境）。
	在子域内运行的代码可以随意抛出异常，而这些异常可以通过子域对象的error事件统一捕获。
	我们使用.create方法创建了一个子域对象，并通过.run方法进入需要在子域中运行的代码的入口点。
	```
	function async(request, callback) {
		// Do something.
		asyncA(request, function (data) {
			// Do something
			asyncB(request, function (data) {
				// Do something
				asyncC(request, function (data) {
					// Do something
					callback(data);
				});
			});
		});
	}
	http.createServer(function (request, response) {
		var d = domain.create(); // 创建子域

		d.on('error', function () {
			response.writeHead(500);
			response.end();
		});

		d.run(function () { // 子域运行入口
			async(request, function (data) {
				response.writeHead(200);
				response.end(data);
			});
		});
	});
	```
#####	陷阱
	无论是通过process对象的uncaughtException事件捕获到全局异常，还是通过子域对象的error事件捕获到了子域异常，在NodeJS官方文档里都强烈建议处理完异常后立即重启程序，而不是让程序继续运行。	
参考书籍：
	[七天学会NodeJs](http://nqdeng.github.io/7-days-nodejs/)
	[node中文](http://nodejs.cn/api/)	
代码仓库：[node学习源代码](https://github.com/fanerge/7day-NodeJs.git)