---
title: node.js 和 mysql 结合使用
date: 2017-09-12 19:06:38
category: "NodeJS"
tags: ['node','mysql','数据库']
---
可视化工具：navicat for MySQL
###	node和mysql的配合
####	mysql和node.js的连接
	```
	var mysql      = require('mysql');
	var connection = mysql.createConnection({
	  host     : 'localhost', // 数据库的ip
	  port	   : 3306, // 数据库起服务的端口
	  user     : 'root', 
	  password : '123456',
	  database : 'test' // 数据库名称
	});
	// 链接数据库 
	connection.connect();
	// 推荐使用下面的链接方式
	connection.connect(function(err) {    
		if (err) {      
			console.error('error connecting: ' + err.stack);      
			return;    
		}    
		console.log('connected as id ' + connection.threadId);  // 线程id
	});
	// 测试数据库是否连接成功 
	connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
	  if (error) throw error;
	  console.log('The solution is: ', results[0].solution);
	});
	```
###	创建表
	```
	connection.query("CREATE TABLE person(id int,user varchar(255),password varchar(255))", function(err,result){
		if(err){throw err}
		console.log("创建表成功")
	})
	```
###	连接池
####	连接池连接
	```
	var pool  = mysql.createPool({    
		connectionLimit : 10,    
		host            : 'example.org',    
		user            : 'bob',    
		password        : 'secret',    
		database        : 'my_db'  
	});	
	// 测试数据库是否连接成功 
	pool.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
	  if (error) throw error;
	  console.log('The solution is: ', results[0].solution);
	});
	// 使用连接池连接可以更容易地共享某个连接，也可以管理多个连接。
	pool.getConnection(function(err, connection) { // 使用连接        
		connection.query( 'SELECT something FROM sometable', function(err, rows) { 
			// 使用连接执行查询       
			connection.release(); //连接不再使用，返回到连接池
			connection.destroy(); // 如果你想关闭连接并从连接池中删除它，在下次需要时连接池会再创建一个新的连接。			
		});  
	});
	```
####	连接池的事件
#####	连接
	```
	pool.on('connection', function (connection) {    
		connection.query('SET SESSION auto_increment_increment=1')  
	});
	```
#####	入列
	```
	pool.on('enqueue', function () {    
		console.log('Waiting for available connection slot');  
	});
	```
#####	在连接池中关闭所有连接
	如不再需要连接池时，你必须关闭所有连接。
	```
	pool.end(function (err) {    
		// all connections in the pool have ended  
	});
	```
#####	集群连接池
	集群连接池提供多主机连接.(分组&重试&选择器)
	```
	// create  
	var poolCluster = mysql.createPoolCluster();  
	// add configurations (the config is a pool config object)  
	poolCluster.add(config); 
	// add configuration with automatic name  
	poolCluster.add('MASTER', masterConfig); 
	// add a named configuration  
	poolCluster.add('SLAVE1', slave1Config);  
	poolCluster.add('SLAVE2', slave2Config);  
	// remove configurations  
	poolCluster.remove('SLAVE2'); 
	// By nodeId  poolCluster.remove('SLAVE*'); 
	// By target group : SLAVE1-2  
	// Target Group : ALL(anonymous, MASTER, SLAVE1-2), Selector : round-robin(default)  
	poolCluster.getConnection(function (err, connection) {});  
	// Target Group : MASTER, Selector : round-robin  
	poolCluster.getConnection('MASTER', function (err, connection) {});  
	// Target Group : SLAVE1-2, Selector : order  
	// If can't connect to SLAVE1, return SLAVE2. (remove SLAVE1 in the cluster)  
	poolCluster.on('remove', function (nodeId) {    console.log('REMOVED NODE : ' + nodeId); 
	// nodeId = SLAVE1   });  
	poolCluster.getConnection('SLAVE*', 'ORDER', function (err, connection) {});  
	// of namespace : of(pattern, selector)  
	poolCluster.of('*').getConnection(function (err, connection) {});  
	var pool = poolCluster.of('SLAVE*', 'RANDOM');  
	pool.getConnection(function (err, connection) {});  
	pool.getConnection(function (err, connection) {});  
	// close all connections  
	poolCluster.end(function (err) {    // all connections in the pool cluster have ended  });
	```
#####	更换用户并且改变连接状态
	该命令允许你在不关闭下列socket的情况下，改变当前用户和连接的其余部分.
	```
	connection.changeUser({user : 'john'}, function(err) {    
		if (err) throw err;  
	});
	```
###	数据库操作（CURD）
	代表创建（Create）、更新（Update）、读取（Retrieve）和删除（Delete）操作。
####	查询数据（Retrieve）
#####	query(sqlString, callback)
	第一个参数是一条SQL字符串，第二个参数是回调
	```
	connection.query('SELECT * FROM `books` WHERE `author` = "David"', function (error, results, fields) {    
		if (error) {
			console.log(error.message);
		}
		// 查询之后的操作
	});
	```
#####	query(sqlString, values, callback)
	带有值的占位符 (查看转义查询值)
	```
	connection.query('SELECT * FROM `books` WHERE `author` = ?', ['David'], function (error, results, fields) {    
		if (error) {
			console.log(error.message);
		}
		// 查询之后的操作
	});
	connection.end(); // 断开数据库连接
	```
#####	query(options, callback)
	在查询时带有大量的高级可选项
	```
	connection.query({
		sql: 'SELECT * FROM `books` WHERE `author` = ?',    
		timeout: 40000,   
		values: ['David']  
	}, function (error, results, fields) {    
		if (error) {
			console.log(error.message);
		}
		// 查询之后的操作
	});
	```
#####	查询值转义
	为了防止SQL注入，每当需要在SQL查询中使用用户数据时，你都应当提前对这些值进行转义。
	```
	// 第一种，转义可以通过 mysql.escape(), connection.escape() 或 pool.escape() 方法实现。
	var userId = 'some user provided value';  
	var sql    = 'SELECT * FROM users WHERE id = ' + connection.escape(userId);  
	connection.query(sql, function(err, results) {    
		// ...  
	});
	// 第二种，使用 ? 作为查询字符串中的占位符，替代你想要转义的值。
	connection.query('SELECT * FROM users WHERE id = ?', [userId], function(err, results) {    
		// ...  
	});
	```
#####	查询标识符转义
	如果用户提供了不可信的查询标识符（数据库名、表名、列名），
	你应该用 mysql.escapeId(identifier), connection.escapeId(identifier) 或 pool.escapeId(identifier) 方法对它进行转义。
	```
	var sorter = 'date';  
	var sql    = 'SELECT * FROM posts ORDER BY ' + connection.escapeId(sorter);  
	connection.query(sql, function(err, results) {    
		// ...  
	});
	var userId = 1;  
	var columns = ['username', 'email'];  
	var query = connection.query('SELECT ?? FROM ?? WHERE id = ?', [columns, 'users', userId], function(err, results) {    
		// SELECT `username`, `email` FROM `users` WHERE id = 1
	});
	```
#####	预查询
	你可以使用 mysql.format 来创建一个多插入点的查询语句，对id和值可以使用适当的转义处理 。
	这样你就获得了一个有效并且安全的查询语句，然后可以把它发送给 数据库。
	```
	var sql = "SELECT * FROM ?? WHERE ?? = ?";  
	var inserts = ['users', 'id', userId];  
	sql = mysql.format(sql, inserts);
	```
####	插入数据（Insert）
	```
	var addSql = 'INSERT INTO websites(Id, name, url, alexa, country) VALUES(0, ?, ?, ?, ?)';
	var addSqlParams = ["fanerge's Blogs", 'fanerge.githut.io', '333325', 'CN'];
	connection.query(addSql, addSqlParams, function (err, result) {
		if (err) {
			console.log('[SELECT ERROR] - ', err.message);
		}
		console.log('-------INSERT--------');
		console.log('INSERT ID:', result);
		console.log('-------END--------');
	});
	connection.end();
	```
####	更新数据（Update）
	```
	var modSql = 'UPDATE websites SET name = ?, url = ?, alexa = ? WHERE Id = ?';
	var modSqlParams = ["fanerge's Blogs", 'https://fanerge.github.io', 23, 6]
	connection.query(modSql, modSqlParams, function (err, result) {
		if (err) {
			console.log('[UPDATE ERROR] - ', err.message);
		}
		console.log('-------UPDATE--------');
		console.log('UPDATE affectedRows:', result.affectedRows);
		console.log('-------END--------');
	});
	connection.end();
	```
####	删除数据（Delete）
	```
	var delSql = 'DELETE FROM websites WHERE name = "fanerge's Blogs"';
	connection.query(delSql, function (err, result) {
		if (err) {
			console.log('[DELETE ERROR] - ', err.message);
		}
		console.log('-------DELETE--------');
		console.log('DELETE affectedRows:', result.affectedRows);
		console.log('-------END--------');
	});
	connection.end();
	```
###	常用技巧
####	获取插入行的id
	如果你把一行插入到一个有自增主键的表中，可以这样获得插入的ID
	```
	connection.query('INSERT INTO posts SET ?', {title: 'test'}, function(err, result) {    
		if (err) throw err;    console.log(result.insertId);  
	});
	```
####	取得受影响的行数
	你可以从 insert，update 或者 delete 子句中取得受影响的行数。
	```
	connection.query('DELETE FROM posts WHERE title = "wrong"', function (err, result) {    
		if (err) throw err;    
		console.log('deleted ' + result.affectedRows + ' rows');  
	})
	```
####	取得被改变的行数
	你可以从 update 子句取得被改变的行数。
	```
	connection.query('UPDATE posts SET ...', function (err, result) {    
		if (err) throw err;    
		console.log('changed ' + result.changedRows + ' rows');  
	})
	```
####	获取连接ID
	你可以取得MySQL的连接ID（“线程ID”），这是一个给定的连接，使用的是线程ID属性。
	```
	connection.connect(function(err) {    
		if (err) throw err;    
		console.log('connected as id ' + connection.threadId);  
	});
	```
####	并行执行查询
	MySQL协议是顺序的，这意味着你需要多次连接执行并行查询。
	你可以使用池来管理连接，一个简单的办法是每传入一个http请求，就创建一个连接。
####	流式查询行
	有的时候可能需要查询大量的数据行，然后在接收到这些数据行的时候一行行的处理它们。
	```
	var query = connection.query('SELECT * FROM posts');  
	query.on('error', function(err) {      
		// 处理错误，这之后会触发 'end' 事件    
	})    
	.on('fields', function(fields) {      
		// 字段信息    
	})
	.on('result', function(row) {      
		// 暂停连接。如果你的处理过程涉及到 I/O 操作，这会很有用。      
		connection.pause();     
		processRow(row, function() {        
			connection.resume();      
		});    
	})
	.on('end', function() {      
		// 所有数据行都已经接收完毕    
	});
	```
####	通过 Streams2 管道输出
	通过管道将查询结果输出到另一个流（最大缓冲 5 个对象）
	```
	connection.query('SELECT * FROM posts')    
	.stream({highWaterMark: 5})    
	.pipe(...);
	```
####	多语句查询
	```
	// 开启多语句查询
	var connection = mysql.createConnection({multipleStatements: true});
	connection.query('SELECT 1; SELECT 2', function(err, results) {    
		if (err) throw err;    
		// `results` is an array with one element for every statement in the query:     
	});
	```
###	总结：
1.	先使用var connection = mysql.createConnection方法一个数据库连接的配置项
2.	使用connection.connect方法连接数据库
3.	使用sql语句对数据库进行CURD操作
4.	操作完成后需使用connection.end方法断开数据库连接
	connection.end(function (err) { ... }); // 支持回调
	connection.destroy(); // 该方法会立即终止底层套接字（underlying socket）。另外，destroy()不会触发更多的事件和回调函数。
>参考文档：
	[Node.js 连接 MySQL](http://www.runoob.com/nodejs/nodejs-mysql.html)
	[如何在node.js里连接和使用mysql](http://www.techug.com/post/node-mysql-node-js.html)
	[mysqljs](https://github.com/mysqljs/mysql)
	[mysql](http://www.runoob.com/mysql/mysql-tutorial.html)




























