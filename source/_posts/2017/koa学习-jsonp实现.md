---
title: koa学习-jsonp实现
date: 2017-09-18 21:35:36
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
###	原生koa2实现jsonp
	```
	const koa = require('koa');

	const app = new koa();

	app.use(async (ctx) => {
		// 判断是否为jsonp的请求
		if (ctx.method === 'GET' && ctx.url.split('?')[0] === '/getData.jsonp') {
			// 获取jsonp的callback
			let callbackName = ctx.query.callback || 'callback';
			let returnData = {
				success: true,
				data: {
					text: 'this is a jsonp api',
					time: new Date().getTime()
				}
			} 
			// jsonp的script字符串
			let jsonpStr = `;${callbackName}(${JSON.stringify(returnData)})`;      
			// 用text/javascript，让请求支持跨域获取
			ctx.type = 'text/javascript';
			// 输出jsonp字符串
			ctx.body = jsonpStr
		} else {
			ctx.body = 'hello jsonp';
		}
	});

	app.listen(3004, () => {
		console.log('[demo] jsonp is starting at port 3004')
	});
	```
###	koa-jsonp中间件实现
	```
	const koa = require('koa');
	const jsonp = require('koa-jsonp');

	const app = new koa();

	app.use(jsonp());

	app.use(async (ctx) => {
		let returnData = {
			success: true,
			data: {
				text: 'this is a jsonp api',
				time: new Date().getTime(),
			}
		};
		// 直接输出json
		ctx.body = returnData;
	});

	app.listen(3004, () => {
		console.log('[demo] jsonp is starting at port 3004')
	});
	```
>	参考文档：
	[koa-note](https://chenshenhai.github.io/koa2-note/note/request/post-use-middleware.html)
	[koa-jsonp](https://www.npmjs.com/package/koa-jsonp)