---
title: koa学习-debug
date: 2017-09-19 19:38:51
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
###	环境
	node环境 8.x +
	chrome 60+
	index.js
	```
	const Koa = require('koa')
	const app = new Koa()

	app.use( async ( ctx ) => {
	  ctx.body = 'hello koa2'
	})

	app.listen(3000, () => {
	  console.log('[demo] start-quick is starting at port 3000')
	})
	```
###	进行调试
	node --inspect index.js
	在控制台上点击node调试，会弹出新窗口，此时就可以进入调试--如打断点。
>	参考文档：
		[debug](https://chenshenhai.github.io/koa2-note/note/debug/info.html)