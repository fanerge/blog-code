---
title: koa学习-cookie/session
date: 2017-09-17 16:06:20
date: 2017-09-17 13:17:50
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
###	koa2使用cookie
	koa提供了从上下文直接读取、写入cookie的方法
1.	ctx.cookies.get(name, [options]) 读取上下文请求中的cookie
2.	ctx.cookies.set(name, value, [options]) 在上下文写入cookie
	关键代码：
	```
	const cookies = require('cookies');
	app.use(async (ctx) => {
		if (ctx.url === '/') {
			// 设置cookies
			ctx.cookies.set('username', 'fanerge222', {
				domain: 'localhost', // 写cookie所在的域名
				path: '/', // 写cookie所在的路径
				maxAge: 10 * 60 * 1000, // cookie有效时长
				expires: new Date('2017-08-18'), // cookie失效时间
				httpOnly: false, // 是否只用于http请求中获取
				overwrite: false // 是否允许重写
			});
			ctx.body = 'cookies写入成功'
		} else {
			// 获取cookie
			const cookie = ctx.cookies.get('username'); // 'fanerge222'
			ctx.body = cookie   
		}
	})
	```
###	koa2实现session
	koa2原生功能只提供了cookie的操作，但是没有提供session操作。session就只用自己实现或者通过第三方中间件实现。
	在koa2中实现session的方案有以下几种
1.	如果session数据量很小，可以直接存在内存中
2.	如果session数据量很大，则需要存储介质存放session数据
	数据库存储方案
1.	将session存放在MySQL数据库中
2.	需要用到中间件
	koa-session-minimal 适用于koa2 的session中间件，提供存储介质的读写接口 。
	koa-mysql-session 为koa-session-minimal中间件提供MySQL数据库的session数据读写操作。
3.	将sessionId和对应的数据存到数据库
4.	将数据库的存储的sessionId存到页面的cookie中
5.	根据cookie的sessionId去获取对于的session信息
	关键代码：	
	```
	const session = require('koa-session-minimal')
	const MysqlSession = require('koa-mysql-session')
	
	// 配置存储session信息的mysql
	let store = new MysqlSession({
	  user: 'root',
	  password: 'root',
	  database: 'koa_demo',
	  host: '127.0.0.1',
	})

	// 存放SESSION_ID的cookie配置
	let cookie = {
	  maxAge: '', // cookie有效时长
	  expires: '',  // cookie失效时间
	  path: '', // 写cookie所在的路径
	  domain: '', // 写cookie所在的域名
	  httpOnly: '', // 是否只用于http请求中获取
	  overwrite: '',  // 是否允许重写
	  secure: '',
	  sameSite: '',
	  signed: '',

	}

	// 使用session中间件
	app.use(session({
	  key: 'SESSION_ID',
	  store: store, // 使用数据库来存储
	  cookie: cookie // 将SESSION_ID存在cookie中
	}))

	app.use( async ( ctx ) => {

	  // 设置session
	  if ( ctx.url === '/set' ) {
		ctx.session = {
		  user_id: Math.random().toString(36).substr(2),
		  count: 0
		}
		ctx.body = ctx.session
	  } else if ( ctx.url === '/' ) {

		// 读取session信息
		ctx.session.count = ctx.session.count + 1
		ctx.body = ctx.session
	  } 

	})
	```
>	参考文档：
	[koa-note](https://chenshenhai.github.io/koa2-note/note/request/post-use-middleware.html)
	[koa-static](https://www.npmjs.com/package/koa-static)
	[koa-session-minimal](https://www.npmjs.com/package/koa-session-minimal)
	[koa-mysql-session](https://www.npmjs.com/package/koa-mysql-session)
	
	
	