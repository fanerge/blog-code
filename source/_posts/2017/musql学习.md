---
title: mysql学习（上部分）
date: 2017-09-12 19:09:59
category: "sql"
tags: ['mysql','数据库']
---
基础构架：database --> table --> row 和 col（数据块）

###	RDBMS 术语
1.	数据库: 数据库是一些关联表的集合。.
2.	数据表: 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
3.	列: 一列(数据元素) 包含了相同的数据, 例如邮政编码的数据。
4.	行：一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
5.	冗余：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
6.	主键：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
7.	外键：外键用于关联两个表。
8.	复合键：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
9.	索引：使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
10.	参照完整性: 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

###	MySQL 创建数据库
####	使用 mysqladmin 创建数据库
	以下实例会创建一个 RUNOOB 数据库
	```
	[root@host]# mysqladmin -u root -p create RUNOOB
	Enter password:******
	```
####	SQL语句创建数据库 RUNOOB
	```
	 'CREATE DATABASE RUNOOB'
	```
	
###	MySQL 删除数据库
####	使用 mysqladmin 删除数据库
	```
	[root@host]# mysqladmin -u root -p drop RUNOOB
	Enter password:******
	```
####	SQL语句删除数据库 RUNOOB
	```
	'DROP DATABASE RUNOOB'
	```
	
###	MySQL 选择数据库
####	从命令提示窗口中选择MySQL数据库
	使用 RUNOOB 数据库
	```
	[root@host]# mysql -u root -p
	Enter password:******
	mysql> use RUNOOB;
	```
####	使用 NODE.js 选择数据库
	```
	var mysql = require('mysql');
	var connection = mysql.createConnetion({
		host: 'localhost',
		prot: 3306,
		user: 'root',
		password: '****',
		database: 'RUNOOB'
	});
	connection.connect(); // 连接数据库
	connection.query('SQL命令', function (err, result) { // 数据库相应的 CURD })	
	connection.end(); // 断开连接
	```

###	MySQL 数据类型
	MySQL支持多种类型，大致可以分为三类：数值、日期/时间和字符串(字符)类型。
####	数值类型
![mysql_int](/images/mysql_int.png)
####	日期和时间类型
	每个时间类型有一个有效值范围和一个"零"值，当指定不合法的MySQL不能表示的值时使用"零"值。
![mysql_time](/images/mysql_time.png)
####	字符串类型
	字符串类型指CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM和SET。
![mysql_string](/images/mysql_string.png)

###	MySQL 创建数据表
	创建数据表需要：表名、表字段名、定义每个表字段
####	SQL命令
	语法：
		CREATE TABLE table_name (column_name column_type)
		CREATE TABLE IF NOT EXISTS table_name (column_name column_type) // 判断是否存在
	实例：
	```
	CREATE TABLE IF NOT EXISTS `runoob_tbl`(
	   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
	   `runoob_title` VARCHAR(100) NOT NULL,
	   `runoob_author` VARCHAR(40) NOT NULL,
	   `submission_date` DATE,
	   PRIMARY KEY ( `runoob_id` )
	)ENGINE=InnoDB DEFAULT CHARSET=utf8;
	1.如果你不想字段为 NULL 可以设置字段的属性为 NOT NULL
	2.AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。
	3.PRIMARY KEY关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号分隔。
	```
####	在命令提示窗口中创建数据表
	```
	root@host# mysql -u root -p
	Enter password:*******
	mysql> use RUNOOB;
	mysql> CREATE TABLE runoob_tbl(
	   -> runoob_id INT NOT NULL AUTO_INCREMENT,
	   -> runoob_title VARCHAR(100) NOT NULL,
	   -> runoob_author VARCHAR(40) NOT NULL,
	   -> submission_date DATE,
	   -> PRIMARY KEY ( runoob_id )
	   -> )ENGINE=InnoDB DEFAULT CHARSET=utf8;
	```
####	node.js中创建表
	```
	connection.query("CREATE TABLE person(id int,user varchar(255),password varchar(255))", function(err,result){
		if(err){throw err}
		console.log("创建表成功")
	})
	```

###	MySQL 删除数据表
	语法：
		DROP TABLE table_name
####	在命令提示窗口中删除数据表
	```
	root@host# mysql -u root -p
	Enter password:*******
	mysql> use RUNOOB;
	mysql> DROP TABLE runoob_tbl
	```
####	node.js中删除表
	```
	connection.query('DROP TABLE runoob_tbl', function (err, result) {
		if (err) throw err;
		console.log(result);
	})
	```

###	MySQL 插入数据
	语法：	
		INSERT INTO table_name (field1, field2, ...filedN) VALUES (value1, value2, value3);
####	通过命令提示窗口插入数据
	```
	root@host# mysql -u root -p password;
	Enter password:*******
	mysql> use RUNOOB;
	mysql> INSERT INTO runoob_tbl 
		-> (runoob_title, runoob_author, submission_date)
		-> VALUES
		-> ("学习 PHP", "菜鸟教程", NOW());
	```
	读取某张表 -- select * from runoob_tbl;
####	node.js插入数据
	```
	connection.query('INSERT INTO websites(id, name, url, alexa, country) VALUES (0, "fanerge", "23213", "234", "成都")', function (err, result) {
		console.log(result);
	});
	// 占位符的形式
	connection.query('INSERT INTO websites(id, name, url, alexa, country) VALUES (0, ?, ?, ?)', ["fanerge12", "23213", "234", "成都"], function (err, result) {
		console.log('DELETE affectedRows:', result);
	});
	```

###	MySQL 查询数据
	语法：
		SELECT column_name, column_name 
		FROM table_name // 可以查询多张表以,分隔
		[WHERE clause] // 查询条件
		[OFFSET M][LIMIT N] // 偏移量 和 限制条数
####	通过命令提示符获取数据
	```
	select * from runoob_tbl
	```
####	node.js查询数据		
	```
	connection.query('SELECT name, url FROM websites', function (err, result) {
		console.log(result);
	});
	```
		
###	MySQL WHERE 子句
	可以用在：SELECT 和 DELETE 和 UPDATE 命令中。
	默认where语句不区分大小写，使用BINARY开启区分大小写。
	语法：
		SELECT field1, field2, ...filedN
		FROM table_name1, table_name2
		[WHERE confition1 [AND [OR]]] condition2
![mysql_where](/images/mysql_where.png)	
####	从命令提示符中读取数据
	```
	SELECT name, url FROM user WHERE name="fanerge" AND url="fanerge"
	```	
####	node.js读取数据
	```
	connection.query('SELECT name, url FROM user WHERE name="fanerge" AND url="fanerge"', function (err, result) {
		console.log(result);
	});
	```
###		MySQL UPDATE 查询	
	语法：	
		UPDATE table_name SET field1=new-value1, field1=new-value2
		[WHERE clause]
####	通过命令提示符更新数据	
	```
	UPDATE runoob_tbl SET runoob_title='学习 C++' WHERE runoob_id=3;
	```	
####	node.js更新数据	
	```
	connection.query('UPDATE user SET url="zhongguo" WHERE name="1name"', function (err, result) {
		console.log(result);
	});
	```
		
###		MySQL DELETE 语句	
	语法：	
		DELETE FROM table_name [WHERE clause]
####	从命令行中删除数据		
	```
	use RUNOOB;
	mysql> DELETE FROM runoob_tbl WHERE runoob_id=3;
	```	
####	node.js删除数据
	```
	connection.query('DELETE FROM user WHERE name="1name"', function (err, result) {
		console.log(result);
	});
	```
		
###	MySQL LIKE 子句
	我们需要获取 runoob_author 字段含有 "COM" 字符的所有记录，这时我们就需要在 WHERE 子句中使用 SQL LIKE 子句。	
	如果没有使用百分号 %, LIKE 子句与等号 = 的效果是一样的。	
	语法：
		SELECT field1, field2, ...filedN
		FROM table_name
		WHERE field1 LIKE confition1 [AND [OR]] field2 = 'somevalue'
####	在命令提示符中使用 LIKE 子句
	以下是我们将 runoob_tbl 表中获取 runoob_author 字段中以 COM 为结尾的的所有记录：
	```
	use RUNOOB
	SELECT * from runoob_tl WHERE runoob_author LIKE '%COM'
	```
####	node.js中使用 LIKE 子句
	```
	connection.query('SELECT * FROM websites WHERE BINARY name LIKE "f%"', function (err, result) {
		console.log(result);
	});
	```
		
###	MySQL UNION 操作符
	MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。
	多个 SELECT 语句会删除重复的数据。
	UNION [ALL | DISTINCT]	// DISTINCT: 可选，删除结果集中重复的数据。ALL: 可选，返回所有结果集，包含重复数据。
	语法：
		SELECT column1, column2, ... columnN
		FROM tables
		[WHERE conditions]
		UNION [ALL | DISTINCT]
		SELECT column1, column2, ... columnN
		FROM tables
		[WHERE conditions];
####	node.js 中使用 UNION
	```
	connection.query('SELECT country FROM websites UNION ALL SELECT country FROM apps ORDER BY country', function (err, result) {
		console.log(result);
	});
	```
####	带有 WHERE 的 SQL UNION ALL
	```
	connection.query('SELECT country, name FROM websites WHERE country LIKE "C%" UNION ALL SELECT country, app_name FROM apps WHERE country="CN" ORDER BY country', function (err, result) {
		console.log(result);
	});
	```	
		
###	MySQL 排序	
	语法：
		SELECT field1, field2, ...filedN table_name1, table_name2
		ORDER BY field1, [field2] [ASC [DESC]] 
####	在命令提示符中使用 ORDER BY 子句	
	```
	use RUNOOB;
	mysql> SELECT * from runoob_tbl ORDER BY submission_date ASC;
	```	
####	node.js 中 ORDER BY [ASC DESC]
	```
	connection.query('SELECT * FROM websites ORDER BY id DESC', function (err, result) {
		console.log(result);
	});
	```	
		
###	MySQL GROUP BY 语句
	语法：	
		SELECT column_name, function(column_name)
		FROM table_name
		WHERE column_name operator value
		GROUP BY column_name;
####	我们使用 GROUP BY 语句 将数据表按名字进行分组，并统计每个人有多少条记录
	```
	SELECT name, COUNT(*) FROM employee_tbl GROUP BY name;
	```
####	使用 WITH ROLLUP
	WITH ROLLUP 可以实现在分组统计数据基础上再进行相同的统计（SUM,AVG,COUNT…）。
	coalesce函数说明：name !== null ? name : '总数'。
	```
	SELECT coalesce(name, "总数"), SUM(singin) as singin_count FROM employee_tbl GROUP BY name WITH ROLLUP
	```

###	Mysql 连接的使用
	从多个数据表中读取数据。
	你可以在 SELECT, UPDATE 和 DELETE 语句中使用 Mysql 的 JOIN 来联合多表查询。
	JOIN 按照功能大致分为如下三类：
	1.	INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。(类似于交集)
	2.	LEFT JOIN（左连接）：获取左表所有记录，即使右表没有对应匹配的记录。
	3.	RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。	
####	在命令提示符中使用 INNER JOIN(相当于交集)
	```
	SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a INNER JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
	SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a, tcount_tbl b WHERE a.runoob_author = b.runoob_author;
	```
####	MySQL LEFT JOIN
	MySQL left join 与 join 有所不同。 MySQL LEFT JOIN 会读取左边数据表的全部数据，即便右边表无对应数据。
	```
	SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a LEFT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
	```
####	MySQL RIGHT JOIN
	MySQL RIGHT JOIN 会读取右边数据表的全部数据，即便左边边表无对应数据。
	```
	SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a RIGHT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
	```

###	MySQL NULL 值处理
	IS NULL: 当列的值是 NULL,此运算符返回 true。
	IS NOT NULL: 当列的值不为 NULL, 运算符返回 true。
	<=>: 比较操作符（不同于=运算符），当比较的的两个值为 NULL 时返回 true。
####	在命令提示符中使用 NULL 值
	SELECT * FROM runoob_test_tbl WHERE runoob_count IS NULL;
	SELECT * FROM runoob_test_tbl WHERE runoob_count IS NOT NULL;

###	MySQL 正则表达式
![mysql_regexp](/images/mysql_regexp1.png)
	```
	SELECT name FROM websites WHERE BINARY name REGEXP '^F'
	SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
	```
>	参考文档
	[MYSQL](http://www.runoob.com/mysql/mysql-tutorial.html)
	