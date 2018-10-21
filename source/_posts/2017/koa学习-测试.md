---
title: koa学习-测试
date: 2017-09-18 22:21:06
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
mocha 模块是测试框架
chai 模块是用来进行测试结果断言库，比如一个判断 1 + 1 是否等于 2
supertest 模块是http请求测试库，用来请求API接口
###	单元测试
	所需测试demo
	```
	const koa = require('koa');
	const app = new koa();

	const server = async (ctx, next) => {
		let result = {
			success: true,
			data: null
		};
		if (ctx.method === 'GET') {
			if (ctx.url === '/getString.json') {
				result.data = 'this is string data';
			} else if (ctx.url === '/getNumber.json'){
				result.data = 123456;    
			} else {
				result.success = false;
			}
			ctx.body = result;
			next && next();
		} else if (ctx.method === 'POST') {
			if (ctx.url === '/postData.json') {
				result.data = 'ok';
			} else {
				result.success = false;
			}
			ctx.body = result;
			next && next();
		} else {
			ctx.body = 'hello world';
			next && next();
		}
	};

	app.use(server);

	module.exports = app;

	app.listen(3004, () => {
		console.log('[demo] test-unit is starting at port 3004')
	});
	```
###	测试代码
	```
	const supertest = require('supertest')
	const chai = require('chai')
	const app = require('./../index')

	const expect = chai.expect
	const request = supertest( app.listen() )

	// 测试套件/组
	describe( '开始测试demo的GET请求', ( ) => {

	  // 测试用例
	  it('测试/getString.json请求', ( done ) => {
		  request
			.get('/getString.json')
			.expect(200)
			.end(( err, res ) => {
				// 断言判断结果是否为object类型
				expect(res.body).to.be.an('object')
				expect(res.body.success).to.be.an('boolean')
				expect(res.body.data).to.be.an('string')
				done()
			})
	  })
	})
	```
>	参考文档：
	[koa-note](https://chenshenhai.github.io/koa2-note/note/request/post-use-middleware.html)
	[koa-jsonp](https://www.npmjs.com/package/koa-jsonp)