---
title: koa学习-模版引擎
date: 2017-09-17 17:29:05
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
###	koa2加载模板引擎
	koa-views - 为koa模版渲染中间件
	ejs - 模版引擎
	关键代码：
	```
	const views = require('koa-views')
	const path = require('path')
	app.use(views(path.join(__dirname, './view'), {
		extension: 'ejs'
	}));

	app.use(async (ctx) => {
		if (ctx.url === '/') {
		   let title = 'hello koa2';
			await ctx.render('index', {
				title,
			});     
		} else {
			let title = 'other';
			await ctx.render('index', {
				title,
			});
		}
		
	});
	```
###	ejs模板引擎
	```
	
	```
















>	参考文档：
	[koa-note](https://chenshenhai.github.io/koa2-note/note/request/post-use-middleware.html)
	[koa-views](https://www.npmjs.com/package/koa-views)
	[ejs](https://www.npmjs.com/package/ejs)
	