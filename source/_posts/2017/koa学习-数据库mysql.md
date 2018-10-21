---
title: koa学习-数据库mysql
date: 2017-09-18 19:56:33
category: "NodeJS"
tags: ['js','web开发','服务端', 'koa']
---
###	模块介绍
	```
	const mysql = require('mysql');
	// 数据库的配置信息
	const connection = mysql.createConnection({
		host: 'localhost',
		port: 3306,
		user: 'root',
		password: 'root',
		database: 'sql'
	});
	// 连接数据库
	connection.connect();
	// 进行数据库操作
	connection.query('SELECT * FROM apps', (err, result) => {
		if (err)  throw err;
		console.log(result);
	});
	// 关闭连接
	connection.end();
	```
###	创建数据连接池
	一般情况下操作数据库是很复杂的读写过程，不只是一个会话，如果直接用会话操作，就需要每次会话都要配置连接参数。
	所以这时候就需要连接池管理会话。
	```
	const mysql = require('mysql');
	const pool = mysql.createPool({
		host: 'localhost',
		port: 3306,
		user: 'root',
		password: 'root',
		database: 'sql'
	});

	// 在数据池中进行会话操作
	pool.getConnection((err, connection) => {
		connection.query('SELECT * FROM apps', (err, result) => {
			console.log(result);
			// 连接完成，连接将返回连接池
			connection.release();
			if (err) throw err;
		})
	});
	```
###	使用promise和async-await来封装
	```
	// promise.js
	const mysql = require('mysql');
	const pool = mysql.createPool({
		host     :  '127.0.0.1',
		user     :  'root',
		password :  'root',
		database :  'sql'
	});

	let query = function (sql) {
		return new Promise((resolve, reject) => {
			pool.getConnection((err, connection) => {
				if (err) {
					reject(err);
				} else {
					connection.query(sql, (err, rows) => {
						if (err) {
							reject(err)
						} else {
							resolve(rows);
						}
					connection.release();   
					});
				}

			});
		});
	}

	module.exports = query;
	
	// async.js
	const query = require('./promise');

	async function getData (sql) {
		let dataList = await query(sql);
		console.log(dataList);
	}

	getData('SELECT * FROM sql');
	```
###	建表初始化
	+----------+  遍历sql  +---+ 解析所有sql +---+  执行sql  +------------>
		   |   |  目录下的  |   |  文件脚本  |   |   脚本     |   |
	+----------+  sql文件   +---+   内容    +---+           +------------>

	[详细代码见]()
>	参考文档：
	[koa-note](https://chenshenhai.github.io/koa2-note/note/request/post-use-middleware.html)
	[cookies](https://www.npmjs.com/package/cookies)