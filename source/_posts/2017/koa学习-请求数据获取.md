---
title: koa学习-请求数据获取
date: 2017-09-17 11:13:16
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
###	GET请求数据获取
	获取GET请求数据源头是koa中request对象中的query方法或querystring方法，
	query返回是格式化好的参数对象，querystring返回的是请求字符串。	
	由于ctx对request的API有直接引用的方式，所以获取GET请求数据有两个途径。
1.	是从上下文中直接获取
		请求对象ctx.query，返回如 { a:1, b:2 }
		请求字符串 ctx.querystring，返回如 a=1&b=2
2.	是从上下文的request对象中获取
		请求对象ctx.request.query，返回如 { a:1, b:2 }
		请求字符串 ctx.request.querystring，返回如 a=1&b=2
	关键代码：
	```
	app.use(async (ctx) => {
		let url = ctx.url;
		let request = ctx.request;
		// 使用ctx.request对象获取
		let req_query = request.query;
		let req_querystring = request.querystring;
		// 使用ctx对其request属性的引用获取
		let ctx_query = ctx.query;
		let ctx_querystring = ctx.querystring;
		ctx.body = {
			url,
			req_query,
			req_querystring,
			ctx_query,
			ctx_querystring
		};
	});
	```
	输出：
	测试url：http://localhost:3000/?name=fanerge&age=323
	```
	{
		url: "/?name=fanerge&age=323",
		req_query: {
			name: "fanerge",
			age: "323"
		},
		req_querystring: "name=fanerge&age=323",
		ctx_query: {
			name: "fanerge",
			age: "323"
		},
		ctx_querystring: "name=fanerge&age=323"
	}
	```
###	POST请求参数获取
	解析出POST请求上下文中的表单数据
	新建一个工具函数库：./util/index.js
	```
	function parseQueryStr (queryStr) {
		let queryData = {};
		let queryStrList = queryStr.split('&');
		console.log(queryStrList);
		for (let [index, queryStr] of queryStrList.entries()) {
			let itemList = queryStr.split('=');
			queryData[itemList[0]] = decodeURIComponent(itemList[1]);
		}
		return queryData;
	}

	function parsePostData (ctx) {
		return new Promise((resolve, reject) => {
			try {
				let postData = "";
				ctx.req.addListener('data', (data) => {
					postData += data;
				});
				ctx.req.addListener('end', ()=> {
					let parseData = parseQueryStr(postData);
					resolve(parseData);
				});
			} catch (e) {
				conole.error(e);
				reject(e);
			}
		});
	}

	module.exports= {
		parsePostData,
	};
	```
	post.js关键代码
	```
	const {parsePostData} = require('./util/index');

	app.use(async (ctx) => {
		if (ctx.url === '/' && ctx.method === 'GET') {
			// 当get请求是返回表单页面
			let html = `
				<h1>koa2 request post demo</h1>
			  <form method="POST" action="/">
				<p>userName</p>
				<input name="userName" /><br/>
				<p>nickName</p>
				<input name="nickName" /><br/>
				<p>email</p>
				<input name="email" /><br/>
				<button type="submit">submit</button>
			  </form>
			`;
			ctx.body = html;
		} else if (ctx.url === '/' && ctx.method === 'POST') {
			let postData = await parsePostData(ctx);
			ctx.body = postData;
		} else {
			ctx.body = '没找到'
		}
	});
	```
	提交表单的数据
	```
	{
		userName: "67",
		nickName: "89",
		email: "0"
	}
	```
###	koa-bodyparser中间件
	对于POST请求的处理，koa-bodyparser中间件可以把koa2上下文的formData数据解析到ctx.request.body中
	关键代码：
	```
	const bodyParser = require('koa-bodyparser'); // 引入该模块
	app.use(bodyParser()); // 使用ctx.body解析中间件
	app.use(async (ctx) => {
		if (ctx.url === '/' && ctx.method === 'GET') {
			// 当GET请求时候返回表单页面
			let html = `
			  <h1>koa2 request post demo</h1>
			  <form method="POST" action="/">
				<p>userName</p>
				<input name="userName" /><br/>
				<p>nickName</p>
				<input name="nickName" /><br/>
				<p>email</p>
				<input name="email" /><br/>
				<button type="submit">submit</button>
			  </form>
			`
			ctx.body = html
		} else if (ctx.url === '/' && ctx.method === 'POST') {
			// 当POST请求的时候，中间件koa-bodyparser解析POST表单里的数据，并显示出来
			let postData = ctx.request.body;
			ctx.body = postData;
		} else {
			ctx.body = '没找到404';
		}
	});
	```
>	参考文档：
	[koa-note](https://chenshenhai.github.io/koa2-note/note/request/post-use-middleware.html)
	[koa-bodyparser](https://www.npmjs.com/package/koa-bodyparser)
	
	
	
	
	
	
	
	
	
	
	
	
	
	