---
title: koa学习-静态资源加载
date: 2017-09-17 13:17:50
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
###	原生koa2实现静态资源服务器
	一个http请求访问web服务静态资源，一般响应结果有三种情况
1.	访问文本，例如html，js，css，png，jpg，gif
2.	访问静态目录
3.	找不到资源，抛出404错误
	这里由于代码量过多，请查看源代码
	[这里由于代码量过多，请查看源代码]()
###	koa-static中间件使用
	koa-static作为koaweb框架的静态服务器中间件
	使用关键代码:
	```
	// 导入koa-static模块
	const static = require('koa-static');
	// 设置静态资源的更目录
	const staticPath = './static';
	// 使用静态资源中间件
	app.use(static(path.join(__dirname, staticPath)));
	```	
>	参考文档：
	[koa-note](https://chenshenhai.github.io/koa2-note/note/request/post-use-middleware.html)
	[cookies](https://www.npmjs.com/package/cookies)