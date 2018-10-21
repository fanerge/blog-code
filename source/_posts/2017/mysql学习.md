---
title: mysql学习（下部分）
date: 2017-09-14 19:54:37
category: "sql"
tags: ['mysql','数据库']
---

###	MySQL 事务
	MySQL 事务主要用于处理操作量大，复杂度高的数据。
	在人员管理系统中，你删除一个人员，你即需要删除人员的基本资料，也要删除和该人员相关的信息。
	事务用来管理 insert,update,delete 语句。
	事务是必须满足4个条件（ACID）： Atomicity（原子性）、Consistency（稳定性）、Isolation（隔离性）、Durability（可靠性）。
####	事物控制语句
	BEGIN或START TRANSACTION；显式地开启一个事务；
	COMMIT；也可以使用COMMIT WORK，不过二者是等价的。COMMIT会提交事务，并使已对数据库进行的所有修改称为永久性的；
	ROLLBACK；有可以使用ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；
	SAVEPOINT identifier；SAVEPOINT允许在事务中创建一个保存点，一个事务中可以有多个SAVEPOINT；
	RELEASE SAVEPOINT identifier；删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常；
	ROLLBACK TO identifier；把事务回滚到标记点；
	SET TRANSACTION；用来设置事务的隔离级别。InnoDB存储引擎提供事务的隔离级别有READ UNCOMMITTED、READ COMMITTED、REPEATABLE READ和SERIALIZABLE。
####	MYSQL 事务处理主要有两种方法
	1、用 BEGIN, ROLLBACK, COMMIT来实现
		BEGIN 开始一个事务
		ROLLBACK 事务回滚
		COMMIT 事务确认
	2、直接用 SET 来改变 MySQL 的自动提交模式:
		SET AUTOCOMMIT=0 禁止自动提交
		SET AUTOCOMMIT=1 开启自动提交
####	事务测试
	CREATE TABLE runoob_transaction_test( id int(5)) engine=innodb;  # 创建数据表
	mysql> select * from runoob_transaction_test; 
	mysql> begin;  # 开始事务
	mysql> insert into runoob_transaction_test value(5); 
	mysql> insert into runoob_transaction_test value(6); 
	mysql> commit; # 提交事务 
	mysql>  select * from runoob_transaction_test;
	+------+
	| id   |
	+------+
	| 5    |
	| 6    |
	+------+ 
	mysql> begin;    # 开始事务
	mysql>  insert into runoob_transaction_test values(7);
	mysql> rollback;   # 回滚
	mysql>   select * from runoob_transaction_test;   # 因为回滚所以数据没有插入

###	MySQL ALTER命令
	当我们需要修改数据表名或者修改数据表字段时，就需要使用到MySQL ALTER命令。
####	删除，添加或修改表字段
	// 删除一列（不能是最后一列）
	ALTER TABLE table_name DROP column_name;
	// 添加一列
	ALTER TABLE table_name ADD column_name INT;
	// 查看列数据类型
	SHOW COLUMNS FROM table_name;
	// MySQL提供的关键字 FIRST (设定位第一列)
	ALTER TABLE table_name ADD column_name INT FIRST;
	// AFTER 字段名（设定位于某个字段之后）
	ALTER TABLE table_name ADD column_name INT AFTER c;
####	修改字段类型及名称
	如果需要修改字段类型及名称, 你可以在ALTER命令中使用 MODIFY 或 CHANGE 子句 。	
	// 更改列的类型
	ALTER TABLE table_name MODIFY column_name CHAR(10);
	ALTER TABLE table_name CHANGE i j INT;
####	ALTER TABLE 对 Null 值和默认值的影响
	// 指定字段 column_name 为 NOT NULL 且默认值为100 。
	ALTER TABLE table_name MODIFY column_name BIGINT NOT NULL DEFAULT 100;	
####	修改字段默认值
	// ALTER 来修改字段的默认值
	ALTER TABLE table_name ALTER column_name SET DEFAULT 1000;
	// ALTER 命令及 DROP子句来删除字段的默认值
	ALTER TABLE table_name ALTER column_name DROP DEFAULT;
####	修改表名
	ALTER TABLE table_name RENAME TO table_name1;
	
###	MySQL 索引
	MySQL索引的建立对于MySQL的高效运行是很重要的，索引可以大大提高MySQL的检索速度。
	索引分单列索引和组合索引。
	单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。
	组合索引，即一个索引包含多个列。
	实际上，索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录。
####	普通索引
#####	创建索引
	CREATE INDEX indexName ON mytable(username(length)); 
	// 如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length。
#####	修改表结构(添加索引)
	ALTER table tableName ADD INDEX indexName(columnName)
#####	创建表的时候直接指定
	CREATE TABLE mytable(  
		ID INT NOT NULL,   
		username VARCHAR(16) NOT NULL,  
		INDEX [indexName] (username(length))  
	);  
#####	删除索引的语法
	DROP INDEX [indexName] ON mytable;
####	唯一索引
	它与前面的普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。
#####	创建索引
	CREATE UNIQUE INDEX indexName ON mytable(username(length)) 
#####	修改表结构
	ALTER table mytable ADD UNIQUE [indexName] (username(length))
#####	创建表的时候直接指定
	CREATE TABLE mytable(  
		ID INT NOT NULL,   
		username VARCHAR(16) NOT NULL,  
		UNIQUE [indexName] (username(length))  
	); 
####	使用ALTER 命令添加和删除索引
	ALTER TABLE tbl_name ADD PRIMARY KEY (column_list): 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
	ALTER TABLE tbl_name ADD UNIQUE index_name (column_list): 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
	ALTER TABLE tbl_name ADD INDEX index_name (column_list): 添加普通索引，索引值可出现多次。
	ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list):该语句指定了索引为 FULLTEXT ，用于全文索引。
	// 为在表中添加索引。
	ALTER TABLE testalter_tbl ADD INDEX (c);
	// 可以在 ALTER 命令中使用 DROP 子句来删除索引。
	ALTER TABLE testalter_tbl DROP INDEX c;
####	使用 ALTER 命令添加和删除主键
	// 主键只能作用于一个列上，添加主键索引时，你需要确保该主键默认不为空（NOT NULL）。
	ALTER TABLE testalter_tbl MODIFY i INT NOT NULL;
	ALTER TABLE testalter_tbl ADD PRIMARY KEY (i);
	// 使用 ALTER 命令删除主键，删除主键时只需指定PRIMARY KEY，但在删除索引时，你必须知道索引名。
	ALTER TABLE testalter_tbl DROP PRIMARY KEY;
####	显示索引信息
	// 使用 SHOW INDEX 命令来列出表中的相关的索引信息。可以通过添加 \G 来格式化输出信息。
	SHOW INDEX FROM table_name; \G
	
###	MySQL 临时表
	MySQL 临时表在我们需要保存一些临时数据时是非常有用的。临时表只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。
	// 创建临时表
	CREATE TEMPORARY TABLE SalesSummary (
		product_name VARCHAR(50) NOT NULL, 
		total_sales DECIMAL(12,2) NOT NULL DEFAULT 0.00, 
		avg_unit_price DECIMAL(7,2) NOT NULL DEFAULT 0.00, 
		total_units_sold INT UNSIGNED NOT NULL DEFAULT 0
	);
	// 临时表插入数据
	INSERT INTO SalesSummary
    (product_name, total_sales, avg_unit_price, total_units_sold)
    VALUES
    ('cucumber', 100.25, 90, 2);
####	删除MySQL 临时表
	DROP TABLE SalesSummary;
	
###	MySQL 复制表
	使用 SHOW CREATE TABLE 命令获取创建数据表(CREATE TABLE) 语句，该语句包含了原数据表的结构，索引等。
	复制以下命令显示的SQL语句，修改数据表名，并执行SQL语句，通过以上命令 将完全的复制数据表结构。
	如果你想复制表的内容，你就可以使用 INSERT INTO ... SELECT 语句来实现。
###	MySQL 元数据
	查询结果信息： SELECT, UPDATE 或 DELETE语句影响的记录数。
	数据库和数据表的信息： 包含了数据库及数据表的结构信息。
	MySQL服务器信息： 包含了数据库服务器的当前状态，版本号等。
####	获取服务器元数据
	SELECT VERSION( )	服务器版本信息
	SELECT DATABASE( )	当前数据库名 (或者返回空)
	SELECT USER( )	当前用户名
	SHOW STATUS	服务器状态
	SHOW VARIABLES	服务器配置变量
	
###	MySQL 序列使用
	MySQL序列是一组整数：1, 2, 3, ...，由于一张数据表只能有一个字段自增主键， 
	如果你想实现其他字段也实现自动增加，就可以使用MySQL序列来实现。
####	使用AUTO_INCREMENT
	CREATE TABLE insect
		(
		id INT UNSIGNED NOT NULL AUTO_INCREMENT,
		PRIMARY KEY (id),
		name VARCHAR(30) NOT NULL, # type of insect
		date DATE NOT NULL, # date collected
		origin VARCHAR(30) NOT NULL # where collected
		);
	INSERT INTO insect (id,name,date,origin) VALUES
		(NULL,'housefly','2001-09-10','kitchen'),
		(NULL,'millipede','2001-09-10','driveway'),
		(NULL,'grasshopper','2001-09-10','front yard');
####	获取AUTO_INCREMENT值
	在MySQL的客户端中你可以使用 SQL中的LAST_INSERT_ID( ) 函数来获取最后的插入表中的自增列的值。
####	重置序列
	如果你删除了数据表中的多条记录，并希望对剩下数据的AUTO_INCREMENT列进行重新排列，那么你可以通过删除自增的列，然后重新添加来实现。
	ALTER TABLE insect DROP id;
	ALTER TABLE insect
    ADD id INT UNSIGNED NOT NULL AUTO_INCREMENT FIRST,
    ADD PRIMARY KEY (id);
####	设置序列的开始值
	一般情况下序列的开始值为1，但如果你需要指定一个开始值100，那我们可以通过以下语句来实现
	CREATE TABLE insect
		(
		id INT UNSIGNED NOT NULL AUTO_INCREMENT,
		PRIMARY KEY (id),
		name VARCHAR(30) NOT NULL, 
		date DATE NOT NULL,
		origin VARCHAR(30) NOT NULL
	)engine=innodb auto_increment=100 charset=utf8;

###	MySQL 处理重复数据
####	防止表中出现重复数据
	你可以在MySQL数据表中设置指定的字段为 PRIMARY KEY（主键） 或者 UNIQUE（唯一） 索引来保证数据的唯一性。
	1.如果你想设置表中字段first_name，last_name数据不能重复，你可以设置双主键模式来设置数据的唯一性。
	```
	CREATE TABLE person_tbl
	(
	   first_name CHAR(20) NOT NULL,
	   last_name CHAR(20) NOT NULL,
	   sex CHAR(10),
	   PRIMARY KEY (last_name, first_name)
	);
	```
	2.INSERT IGNORE INTO会忽略数据库中已经存在的数据，如果数据库没有数据，就插入新的数据，如果有数据的话就跳过这条数据。
	```
	INSERT IGNORE INTO person_tbl (last_name, first_name)
    VALUES( 'Jay', 'Thomas');
	```
	3.另一种设置数据的唯一性方法是添加一个UNIQUE索引。
	```
	CREATE TABLE person_tbl
	(
	   first_name CHAR(20) NOT NULL,
	   last_name CHAR(20) NOT NULL,
	   sex CHAR(10)
	   UNIQUE (last_name, first_name)
	);
	```
####	统计重复数据
	以下我们将统计表中 first_name 和 last_name的重复记录数
	SELECT COUNT(*) as repetitions, last_name, first_name
    FROM person_tbl
    GROUP BY last_name, first_name
    HAVING repetitions > 1;
	确定哪一列包含的值可能会重复。
	在列选择列表使用COUNT(*)列出的那些列。
	在GROUP BY子句中列出的列。
	HAVING子句设置重复数大于1。
####	过滤重复数据
	1.你需要读取不重复的数据可以在 SELECT 语句中使用 DISTINCT 关键字来过滤重复数据。
		SELECT DISTINCT last_name, first_name FROM person_tbl;
	2.使用 GROUP BY 来读取数据表中不重复的数据。
		SELECT last_name, first_name
		FROM person_tbl
		GROUP BY (last_name, first_name);
####	删除重复数据
	1.删除person_tbl表中的重复数据
	CREATE TABLE tmp SELECT last_name, first_name, sex
	FROM person_tbl;
	GROUP BY (last_name, first_name, sex);
	DROP TABLE person_tbl;
	ALTER TABLE tmp RENAME TO person_tbl;
	2.可以在数据表中添加 INDEX（索引） 和 PRIMAY KEY（主键）这种简单的方法来删除表中的重复记录。
	ALTER IGNORE TABLE person_tbl
    ADD PRIMARY KEY (last_name, first_name);
	
###	MySQL 及 SQL 注入
	如果您通过网页获取用户输入的数据并将其插入一个MySQL数据库，那么就有可能发生SQL注入安全的问题。	
	我们永远不要信任用户的输入，我们必须认定用户输入的数据都是不安全的，我们都需要对用户输入的数据进行过滤处理。
	// 过滤用户的输入
	```
	if (preg_match("/^\w{8,20}$/", $_GET['username'], $matches))
	{
	   $result = mysqli_query($conn, "SELECT * FROM users 
							  WHERE username=$matches[0]");
	}
	 else 
	{
	   echo "username 输入异常";
	}
	```
	防止SQL注入，我们需要注意以下几个要点：
		1.永远不要信任用户的输入。对用户的输入进行校验，可以通过正则表达式，或限制长度；对单引号和 双"-"进行转换等。
		2.永远不要使用动态拼装sql，可以使用参数化的sql或者直接使用存储过程进行数据查询存取。
		3.永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。
		4.不要把机密信息直接存放，加密或者hash掉密码和敏感的信息。
		5.应用的异常信息应该给出尽可能少的提示，最好使用自定义的错误信息对原始错误信息进行包装。
		6.sql注入的检测方法一般采取辅助软件或网站平台来检测，软件一般采用sql注入检测工具jsky，网站平台就有亿思网站安全平台检测工具。MDCSOFT SCAN等。采用MDCSOFT-IPS可以有效的防御SQL注入，XSS攻击等。
####	防止SQL注入
	在脚本语言，如Perl和PHP你可以对用户输入的数据进行转义从而来防止SQL注入。
	如：mysqli_real_escape_string()
	```
	if (get_magic_quotes_gpc()) 
	{
	  $name = stripslashes($name);
	}
	$name = mysqli_real_escape_string($conn, $name);
	 mysqli_query($conn, "SELECT * FROM users WHERE name='{$name}'");
	```
####	Like语句中的注入
	```
	$sub = addcslashes(mysqli_real_escape_string($conn, "%something_"), "%_");
	// $sub == \%something\_
	 mysqli_query($conn, "SELECT * FROM messages WHERE subject LIKE '{$sub}%'");
	addcslashes() 函数在指定的字符前添加反斜杠。
	```
	
###	MySQL 导出数据
	MySQL中你可以使用SELECT...INTO OUTFILE语句来简单的导出数据到文本文件上。
	1.将数据表 runoob_tbl 数据导出到 /tmp/tutorials.txt 文件中
	```
	SELECT * FROM runoob_tbl 
    INTO OUTFILE '/tmp/tutorials.txt';
	```
	2.可以通过命令选项来设置数据输出的指定格式，以下实例为导出 CSV 格式
	```
	SELECT * FROM passwd INTO OUTFILE '/tmp/tutorials.txt'
    FIELDS TERMINATED BY ',' ENCLOSED BY '"'
    LINES TERMINATED BY '\r\n';
	```
	3.生成一个文件，各值用逗号隔开。
	```
	SELECT a,b,a+b INTO OUTFILE '/tmp/result.text'
	FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
	LINES TERMINATED BY '\n'
	FROM test_table;
	```
####	导出表作为原始数据
	mysqldump是mysql用于转存储数据库的实用程序。
	使用mysqldump导出数据需要使用 --tab 选项来指定导出文件指定的目录，该目标必须是可写的。	
	以下实例将数据表 runoob_tbl 导出到 /tmp 目录中。
	```	
	mysqldump -u root -p --no-create-info \
            --tab=/tmp database_name table_name
	```
####	导出SQL格式的数据
	1.导出SQL格式的数据到指定文件
	```
	mysqldump -u root -p database_name table_name > dump.txt
	```
	2.导出整个数据库的数据
	```
	mysqldump -u root -p database_name > database_dump.txt
	```
	3.备份所有数据库
	```
	mysqldump -u root -p --all-databases > database_dump.txt
	```
####	将数据表及数据库拷贝至其他主机
	将数据备份至 dump.txt 文件中
		mysqldump -u root -p database_name table_name > dump.txt
	将备份的数据库导入到MySQL服务器中，可以使用以下命令，使用以下命令你需要确认数据库已经创建
		mysql -u root -p database_name < dump.txt
	使用以下命令将导出的数据直接导入到远程的服务器上，但请确保两台服务器是相通的，是可以相互访问的
		mysqldump -u root -p database_name \
			| mysql -h other-host.com database_name
	
###	MySQL 导入数据
####	使用 LOAD DATA 导入数据
	将从当前目录中读取文件 dump.txt ，将该文件中的数据插入到当前数据库的 mytbl 表中。
		LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl;
		如果指定LOCAL关键词，则表明从客户主机上按路径读取文件。如果没有指定，则文件在服务器上按路径读取文件。
####	使用 mysqlimport 导入数据
	从文件 dump.txt 中将数据导入到 mytbl 数据表中
		mysqlimport -u root -p --local database_name dump.txt
####	mysqlimport的常用选项介绍
![mysql_import](/images/mysql_import.png)
	
>	参考文档
	[MYSQL](http://www.runoob.com/mysql/mysql-tutorial.html)
	
	
	
	
	
	
	



