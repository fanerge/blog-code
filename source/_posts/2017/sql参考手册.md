---
title: sql参考手册
date: 2017-09-14 22:21:26
category: "sql"
tags: '数据库'
---
####	AND / OR
	AND & OR 运算符用于基于一个以上的条件对记录进行过滤。
	语法：SELECT column_name(s) FROM table_name WHERE condition AND|OR condition;
	选择count大于10 且 count小于230
		SELECT * FROM access_log WHERE count > 10 AND count < 230
	选择count小于10 或 count大于230	
		SELECT * FROM access_log WHERE count < 10 OR count > 230
####	ALTER TABLE	
	ALTER TABLE 语句用于在已有的表中添加、删除或修改列。
	语法：
		// 添加一列
		ALTER TABLE table_name  
		ADD column_name datatype 
		// 删除一列
		ALTER TABLE table_name   
		DROP COLUMN column_name
		// 更改一列字段类型	
		ALTER TABLE table_name   
		MODIFY COLUMN column_name VARCHAR(10)
		// 更改一列
		ALTER TABLE table_name   
		CHANGE COLUMN column_name re_name
####	AS (alias)
	SELECT column_name AS column_alias 
	FROM table_name
	OR
	SELECT column_name
	FROM table_name AS table_alias
	对url改名为ii	
		SELECT url AS ii FROM websites
####	BETWEEN
	语法：
		SELECT column_name(s)
		FROM table_name
		WHERE column_name
		BETWEEN value1 AND value2
	选择alexa1到10的所有记录
		SELECT * FROM websites WHERE alexa BETWEEN 1 AND 10	
####	CREATE DATABASE
	语法：CREATE DATABASE database_name
	创建IIII数据库	
		CREATE DATABASE IIII	
####	DROP DATABASE
	语法：DROP DATABASE database_name
	删除IIII数据库
		DROP DATABASE IIII
####	CREATE TABLE 	
	语法：
		CREATE TABLE table_name (
			column_name data_type,
			column_name data_type,
			...
		)
	创建一个名为 sds 的表，且有字段int	
		CREATE TABLE sds (id int)
####	DROP TABLE
	语法：DROP TABLE IF EXISTS tabale_name
	如果表sds存在就删除
		DROP TABLE IF EXISTS sds
####	CREATE INDEX 
	创建索引
	语法：
		CREATE INDEX index_name
		ON tabale_name (column_name)
		OR
		CREATE UNIQUE INDEX index_name
		ON tabale_name (column_name)
####	CREATE VIEW
	创建视图
	视图是基于 SQL 语句的结果集的可视化的表。
	语法：
		CREATE VIEW view_name AS
		SELECT column_name(s)
		FROM tabale_name
####	DELETE
	DELETE FROM tabale_name
	WHERE some_column = some_value
	OR
	DELETE FROM tabale_name // 删除整个表
	DELETE * FROM tabale_name
####	DROP DATABASE
	删除数据库
	DROP DATABASE data_name
####	DROP INDEX 
	删除索引
	DROP INDEX index_name
####	DROP TABLE	
		删除表
		DROP TABLE table_name
####	GROUP BY
	合计函数 (比如 SUM) 常常需要添加 GROUP BY 语句。
	SELECT column_name, aggregate_function(column_name)
	FROM table_name
	WHERE column_name operator value1
	GROUP BY column_name
####	HAVING
	在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用。	
	SELECT column_name, aggregate_function(column_name)
	FROM tabale_name
	WHERE column_name operator value
	GROUP BY column_name
	HAVING aggregate_function(column_name) operator value
####	IN
	IN 操作符允许您在 WHERE 子句中规定多个值。
	SELECT column_name(s)
	FROM tabale_name
	WHERE column_name
	IN(value1, value2)
####	INSERT INTO 
	插入数据
	INSERT INTO tabale_name
	VALUES(value1, value2, ...value3)
	OR	
	INSERT INTO tabale_name
	(column1, column2, ...column3)
	VALUES(value1, value2, ...value3)
####	INNER JOIN
	在表中存在至少一个匹配时，INNER JOIN 关键字返回行。	
	SELECT column_name(s)
	FROM tabale_name1
	INNER JOIN tabale_name2
	ON tabale_name1.column_name = tabale_name2.column_name
####	LEFT JOIN	
	返回左表
	SELECT column_name(s)
	FROM tabale_name1
	LEFT JOIN tabale_name2
	ON tabale_name1.column_name = tabale_name2.column_name
####	RIGHT JOIN	
	返回左表
	SELECT column_name(s)
	FROM tabale_name1
	RIGHT JOIN tabale_name2
	ON tabale_name1.column_name = tabale_name2.column_name		
####	FULL JOIN
	只要其中某个表存在匹配，FULL JOIN 关键字就会返回行。	
	SELECT column_name(s)
	FROM tabale_name1
	FULL JOIN tabale_name2
	ON tabale_name1.column_name = tabale_name2.column_name
####	LIKE	
	LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。	
	SELECT column_name(s)
	FROM tabale_name
	WHERE column_name LIKE pattern
####	ORDER BY
	ORDER BY 语句用于对结果集进行排序。
	SELECT column_name 
	FROM table_name
	ORDER BY column_name[ASC|DESC]
####	SELECT
	查询
	SELECT column_name(s)
	FROM tabale_name
####	SELECT *
	SELECT *
	FROM tabale_name
####	SELECT DISTINCT
	关键词 DISTINCT 用于返回唯一不同的值。	
	SELECT DISTINCT column_name(s)
	FROM tabale_name
####	SELECT INTO 
	SELECT *
	INTO new_table_name [IN externaldatabase]
	FROM old_table_name
	OR
	SELECT column_name(s)
	INTO new_table_name [IN externaldatabase]
	FROM old_table_name
####	SELECT TOP
	SELECT column_name(s)
	FROM table_name
	LIMIT number
####	TRUNCATE TABLE	
	TRUNCATE TABLE table_name	
####	UNION
	SQL UNION 操作符合并两个或多个 SELECT 语句的结果。
	SELECT column_name(s) FROM tabale_name1
	UNION
	SELECT column_name(s) FROM tabale_name2
####	UNION ALL
		SELECT column_name(s) FROM tabale_name1
		UNION ALL
		SELECT column_name(s) FROM tabale_name2
####	UPDATE
	更新
	UPDATE tabale_name
	SET column1=value, column2=value
	WHERE some_column=some_value
####	WHERE
	条件
	SELECT column_name(s)
	FROM tabale_name
	WHERE column_name operator value
>	参考文档
	[SQL文档](http://www.runoob.com/sql/sql-quickref.html)
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		