---
title: koa学习-总结
date: 2017-09-28 20:22:32
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
##	一个koa示例
	```
	const koa = require('koa');
	const app = new koa();
	
	// 使用中间件
	app.use(); // @param function
	// 监听端口，开启服务
	app.listen(1314, () => {
		console.log('success');
	});
	```
##	Context 对象
	Koa 提供一个 Context 对象，表示一次对话的上下文（包括 HTTP 请求和 HTTP 回复）。
	koa 通过 ctx.request.accepts 设置期望返回的类型内容，默认为 text/plain。		
	koa 通过 ctx.response.type 指定返回类型。
	示例：常用的格式xml、json、html、text等。
		```
		app.use((ctx) => {
			if (ctx.request.accepts('xml')) {
				ctx.response.type = 'xml';
				ctx.response.body = '<data>我是xml</data>';
			} else {
				ctx.response.type = 'text';
				ctx.response.body = '我是text';
			}
		});
		```	
	示例：实际开发（网页模板）
		直接返回template.html
		```
		app.use(ctx => {
		  ctx.response.type = 'html';
		  ctx.response.body = fs.createReadStream('./views/template.html');
		});
		```
##	路由
	通过ctx.request.path可以获取用户请求的路径，由此实现简单的路由。
###	原生路由
		```
		const main = ctx => {
			ctx.response.type = 'html';
		  if (ctx.request.path === '/') {
			ctx.response.body = '<a href="/">Index Page</a>';
		  } else if (ctx.request.path === '/get') {
			ctx.response.body = '<a href="/">Get Page</a>';
		  } else {
			ctx.response.body = '<a href="/">other Page</a>';
		  }
		};
		```
###	koa-route 模块
	```
	const Koa = require('koa');
	const route = require('koa-route');
	const app = new Koa();

	const about = ctx => {
		ctx.response.type = 'html';
		ctx.response.body = '<p>about page</p>'
	};

	const user = ctx => {
		ctx.response.type = 'html';
		ctx.response.body = '<p>user page</p>'
	};

	const main = ctx => {
		ctx.response.type = 'html';
		ctx.response.body = 'hello world'
	};

	app.use(route.get('/', main)); // 主页
	app.use(route.get('/about', about)); // about页面
	app.use(route.get('/user', user)); // user页面
	 
	// app.use(main);
	app.listen(3000);
	```
##	静态资源 和 重定向
###	静态资源
	如果网站提供静态资源（图片、字体、样式表、脚本......）。
	koa-static 模块封装了这部分的请求。
	示例：请求本地12.js
	```
	const Koa = require('koa');
	const app = new Koa();
	const path = require('path');
	const serve = require('koa-static');

	const main = serve(path.join(__dirname));

	app.use(main);
	app.listen(3000);
	```
	说明：__dirname 表示 node.js 执行环境路径。
	请求资源： http://127.0.0.1:3000/12.js
###	重定向
	有些场合，服务器需要重定向（redirect）访问请求。
	比如，用户登陆以后，将他重定向到登陆前的页面。
	ctx.response.redirect()方法可以发出一个302跳转，将用户导向另一个路由。
	```
	const redirect = ctx => {
	  ctx.response.redirect('/');
	};

	const main = ctx => {
	  ctx.response.body = 'Hello World';
	};

	app.use(route.get('/', main));
	app.use(route.get('/redirect', redirect)); // 当请求路径为 /redirect 时，执行 redirect 方法，进行重定向
	```
	访问 http://127.0.0.1:3000/redirect ，浏览器会将用户导向根路由。
##	中间件
###	设计中间件
	Koa 的最大特色，也是最重要的一个设计，就是中间件（middleware）。
	示例：自己设计一个logger中间件。
	```
	/**
	* 定义日志中间件 logger
	* @param ctx {object} 上下文对象 
	* @param next {function} 下一个中间件执行权
	*/
	const logger = (ctx, next) => {
		console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
		next(); // 只要调用next函数，就可以把执行权转交给下一个中间件。
	};
	// 使用中间件
	app.use(logger);
	```
###	中间件特性
	多个中间件会形成一个栈结构（middle stack），以"先进后出"（first-in-last-out）的顺序执行。
	```
	const one = (ctx, next) => {
	  console.log('>> one');
	  next();
	  console.log('<< one');
	}

	const two = (ctx, next) => {
	  console.log('>> two');
	  next(); 
	  console.log('<< two');
	}

	const three = (ctx, next) => {
	  console.log('>> three');
	  next();
	  console.log('<< three');
	}

	app.use(one);
	app.use(two);
	app.use(three);
	```
	输出：
	>> one   // one中间件进栈
	>> two   // two中间件进栈
	>> three // three中间件进栈
	<< three // three中间件 出栈
	<< two   // two中间件 出栈
	<< one	 // one中间件 出栈	
###	异步中间件
	如果有异步操作（比如读取数据库），中间件就必须写成 async 函数。
	实例：异步 main 中间件
	```
	const fs = require('fs.promised');

	const main = async function (ctx, next) {
	  ctx.response.type = 'html';
	  ctx.response.body = await fs.readFile('./demos/template.html', 'utf8');
	};
	```
	fs.readFile是一个异步操作，必须写成异步中间件。
###	koa-compose模块可以将多个中间件合成为一个。
	示例：合并 logger 和 main 中间件。
	```
	const compose = require('koa-compose');

	const middlewares = compose([logger, main]);
	app.use(middlewares);
	```
##	错误处理
	如果代码运行过程中发生错误，我们需要把错误信息返回给用户。
###	500错误
	HTTP 协定约定这时要返回500状态码。
	Koa 提供了ctx.throw()方法，用来抛出错误，ctx.throw(500)就是抛出500错误。
	```
	const main = ctx => {
		ctx.throw(500);
	};
	```
###	404错误
	如果将ctx.response.status设置成404，就相当于ctx.throw(404)，返回404错误。
	```
	const main = ctx => {
	  ctx.response.status = 404; // ctx.throw(404);
	  ctx.response.body = 'Page Not Found';
	};
	```
###	处理错误的中间件	
	为了方便处理错误，最好使用try...catch将其捕获。
	但是，为每个中间件都写try...catch太麻烦，我们可以让最外层的中间件，负责所有中间件的错误处理。
	```
	// 最外层中间件
	const handler = async (ctx, next) => {
	  try {
		await next();
	  } catch (err) {
		ctx.response.status = err.statusCode || err.status || 500;
		ctx.response.body = {
		  message: err.message
		};
	  }
	};
	// 内层中间件
	const main = ctx => {
	  ctx.throw(404);
	};
	```
###	error 事件的监听
	运行过程中一旦出错，Koa 会触发一个error事件。
	监听这个事件，也可以处理错误。
	```
	const main = ctx => {
		ctx.throw(500); // 直接触发错误
	};
	// 监听错误
	app.on('error', (err, ctx) => {
		console.error('server error', err);
	});
	```
###	释放 error 事件	
	需要注意的是，如果错误被try...catch捕获，就不会触发error事件。
	这时，必须调用ctx.app.emit()，手动释放error事件，才能让监听函数生效。
	```
	const handler = async (ctx, next) => {
		try {
			await next();
		} catch (err) {
			ctx.response.status = err.statusCode || err.status || 500;
			ctx.response.type = 'html';
			ctx.response.body = '<p>Something wrong, please contact administrator.</p>';
			ctx.app.emit('error', err, ctx);
		}
	};

	const main = ctx => {
		ctx.throw(500);
	};

	app.on('error', function(err) {
		console.log('logging error ', err.message);
		console.log(err);
	});
	```
	main函数抛出错误，被handler函数捕获。
	catch代码块里面使用ctx.app.emit()手动释放error事件，才能让监听函数监听到。
##	Web App 的功能
###	Cookies
	ctx.cookies 用来读写 Cookie。
	```
	const main = function(ctx) {
		const n = Number(ctx.cookies.get('view') || 0) + 1;
		ctx.cookies.set('view', n);
		ctx.response.body = n + ' views';
	}
	```
###	表单
	Web 应用离不开处理表单。
	表单就是 POST 方法发送到服务器的键值对。
	koa-body模块可以用来从 POST 请求的数据体里面提取键值对。
	```
	const koaBody = require('koa-body');

	const main = async function (ctx) {
		const body = ctx.request.body;
		if (!body.name) ctx.throw(400, '.name required');
		ctx.body = { name: body.name };
	};

	app.use(koaBody());
	```
###	文件上传
	koa-body模块还可以用来处理文件上传。
	```
	const os = require('os');
	const path = require('path');
	const koaBody = require('koa-body');

	const main = async function(ctx) {
		const tmpdir = os.tmpdir();
		const filePaths = [];
		const files = ctx.request.body.files || {};

		for (let key in files) {
			const file = files[key];
			const filePath = path.join(tmpdir, file.name);
			const reader = fs.createReadStream(file.path);
			const writer = fs.createWriteStream(filePath);
			reader.pipe(writer);
			filePaths.push(filePath);
		}

		ctx.body = filePaths;
	};

	app.use(koaBody({ multipart: true }));
	```
>	参考文档：
	[阮一峰老师的koa教程](http://www.ruanyifeng.com/blog/2017/08/koa.html)
	[koa-router官方文档](https://www.npmjs.com/package/koa-router)











