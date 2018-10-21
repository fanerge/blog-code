---
title: koa学习-中间件和路由
date: 2017-09-16 21:24:00
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
##	koa中间件的制作和使用
###	koa模版
	```
	var koa = require('koa');
	const app = new koa();
	// 中间件
	app.use(async (ctx) => {
		ctx.body = 'hello koa2';
	});
	app.listen(3000);
	```
###	async/await
	```
	function getSyncTime () {
		return new Promise((resolve, reject) => {
			try {
				let startTime = new Date().getTime();
				setTimeout(() => {
					let endTime = new Date().getTime();
					let data = endTime - startTime;
					resolve(data);
				}, 500);
			} catch (e) {
				reject(e);
			}
		});
	}

	async function getSyncData () {
		let time = await getSyncTime();
		let data = `endTime - startTime = ${time}`;
		console.log(data);
	}
	getSyncData();
	```
###	koa2简析结构
	源码文件：
	├── lib
	│   ├── application.js
	│   ├── context.js
	│   ├── request.js
	│   └── response.js
	└── package.json
	文件说明：
	application.js 是整个koa2 的入口文件，封装了context，request，response，以及最核心的中间件处理流程。
	context.js 处理应用上下文，里面直接封装部分request.js和response.js的方法
	request.js 处理http请求
	response.js 处理http响应
###	koa中间件开发和使用
	以日志中间件为例
	middleware/logger-async.js
	```
	function log (ctx) {
		console.log(`请求方法：${ctx.method}, host：${ctx.header.host}, url：${ctx.url}`);
	}

	module.exports = function () {
		return async function (ctx, next){
			log(ctx);
			await next();
		};
	};
	```
	使用该中间件logger
	```
	const loggerAsync  = require('./middleware/logger-async');
	// 使用日志中间件
	app.use(loggerAsync());
	```
##	koa路由中间件
###	koa2原生路由实现
	获取url：ctx.request.url
	后端路由：根据前端get请求的url地址，后端返回对应的view页面达到。
###	koa-router中间件
	安装koa-router中间件
		npm install --save koa-router
	使用koa-router
	```
	// 子路由1
	let home = new Router();
	home.get('/', async (ctx) => {
		let html = `
			<ul>
				<li><a href="/">/</a></li>
				<li><a href="/page">/page</a></li>
				<li><a href="/page/helloword">/page/helloword</a></li>
				<li><a href="/page/chengdu">/page/chengdu</a></li>
				<li><a href="/page/ss">not found</a></li>
			</ul>
		`;
		ctx.body = html;
	});

	// 子路由2
	let page = new Router();
	page.get('/', async (ctx) => {
		ctx.body = 'page主页';
	})
	.get('/chengdu', async (ctx) => {
		ctx.body = '欢迎来到成都';
	})
	.get('/helloword', (ctx) => {
		ctx.body = 'helloword page';
	});

	// 子路由3-Not found
	let notFound = new Router();
	notFound.get('*', async (ctx) => {
		ctx.body = '没有找到！';
	});

	// 装载所有的子路由
	let router = new Router();
	router.use('/', home.routes(), home.allowedMethods());
	router.use('/page', page.routes(), page.allowedMethods());
	router.use('*', notFound.routes(), notFound.allowedMethods());

	// 加载路由中间件
	app.use(router.routes())
	.use(router.allowedMethods());
	```
>	参考文档：
	[koa2-note](https://chenshenhai.github.io/koa2-note/note/request/get.html)
	[koa-router官方文档](https://www.npmjs.com/package/koa-router)

	
	
	
	
	
	
	
	
	