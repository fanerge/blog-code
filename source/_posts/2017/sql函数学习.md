---
title: sql函数学习
date: 2017-09-14 20:07:14
category: "sql"
tags: '数据库'
---
###	SQL 函数
	SQL 拥有很多可用于计数和计算的内建函数。
####	SQL Aggregate 函数
	SQL Aggregate 函数计算从列中取得的值，返回一个单一的值。
	AVG() - 返回平均值
	COUNT() - 返回行数
	FIRST() - 返回第一个记录的值
	LAST() - 返回最后一个记录的值
	MAX() - 返回最大值
	MIN() - 返回最小值
	SUM() - 返回总和
####	SQL Scalar 函数
	SQL Scalar 函数基于输入值，返回一个单一的值。
	UCASE() - 将某个字段转换为大写
	LCASE() - 将某个字段转换为小写
	MID() - 从某个文本字段提取字符，MySql 中使用
	SubString(字段，1，end) - 从某个文本字段提取字符
	LEN() - 返回某个文本字段的长度
	ROUND() - 对某个数值字段进行指定小数位数的四舍五入
	NOW() - 返回当前的系统日期和时间
	FORMAT() - 格式化某个字段的显示方式
####	AVG()
	一下实例中均使用数据库sql，表名为access_log
	AVG() 函数返回数值列的平均值。
	语法：SELECT AVG(column_name) FROM table_name;
	1.返回count的平均值
		SELECT AVG(count) FROM access_log
	2.返回大于count平均数的元组
		SELECT site_id, count FROM access_log
		WHERE count > (SELECT AVG(count) FROM access_log);
####	COUNT()
	COUNT() 函数返回匹配指定条件的行数（NULL 不计入）。
	语法：SELECT COUNT(column_name) FROM table_name; // COUNT(column_name) 函数返回指定列的值的数目（NULL 不计入）
		SELECT COUNT(DISTINCT column_name) FROM table_name; // COUNT(DISTINCT column_name) 函数返回指定列的不同值的数目
	1.返回表中记录数
		SELECT COUNT(*) FROM access_log
	2.返回site_id = 3的记录拥有count的数量，并改名为nums
		SELECT COUNT(count) AS nums FROM access_log WHERE site_id = 3
	3.计算 "access_log" 表中不同 site_id 的记录数
		SELECT COUNT(DISTINCT site_id) AS nums FROM access_log
####	SQL FIRST() 函数
	FIRST() 函数返回指定的列中第一个记录的值。
	注释：只有 MS Access 支持 FIRST() 函数。
	语法：SELECT FIRST(column_name) FROM table_name;
	MySQL 语法：
		SELECT column_name FROM table_name
		ORDER BY column_name ASC
		LIMIT 1;
	1.mysql中获取第一条记录
		SELECT count FROM access_log ORDER BY count ASC LIMIT 1;
####	SQL LAST() 函数
	LAST() 函数返回指定的列中最后一个记录的值。
	注释：只有 MS Access 支持 LAST() 函数。
	语法：SELECT LAST(column_name) FROM table_name;
	MySQL 语法：
		SELECT * FROM table_name 
		ORDER BY column_name ASC
		LIMIT 1;
	1.mysql中获取最后一条记录
	SELECT * FROM access_log ORDER BY count DESC LIMIT 1;
####	SQL MAX() 函数
	MAX() 函数返回指定列的最大值。
	语法：SELECT MAX(column_name) FROM table_name;
	1.返回count最大的一条记录
		SELECT * FROM access_log WHERE count = (SELECT MAX(count) FROM access_log)
####	SQL MIN() 函数
	MIN() 函数返回指定列的最小值。
	语法：SELECT column_name FROM table_name;
	1.返回count最小的一条记录中的aid 和 count字段
		SELECT aid, count FROM access_log WHERE count = (SELECT MIN(count) FROM access_log)
####	SQL SUM() 函数
	SUM() 函数返回数值列的总数。
	语法：SELECT column_name FROM table_name;
	1.返回count 的总和
		SELECT SUM(count) FROM access_log	
####	SQL GROUP BY 语句
	GROUP BY 语句用于结合聚合函数，根据一个或多个列对结果集进行分组。
	语法：
		SELECT column_name, aggregate_function(column_name)
		FROM table_name
		WHERE column_name operator value
		GROUP BY column_name;
	1.返回通过site_id分组对count求和
		SELECT site_id, SUM(access_log.count) AS nums 
		FROM access_log GROUP BY site_id
	2.统计所有网站的访问的记录数（LEFT JOIN 关键字从左表（table1）返回所有的行，即使右表（table2）中没有匹配。）
	SELECT websites.name, COUNT(access_log.aid) AS nums 
	FROM access_log LEFT JOIN websites ON access_log.site_id = websites.id 
	GROUP BY websites.name
####	SQL HAVING 子句
	在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。
	HAVING 子句可以让我们筛选分组后的各组数据。
	1.现在我们想要查找总访问量大于 200 的网站
	SELECT Websites.name, Websites.url, SUM(access_log.count) AS nums FROM (access_log
	INNER JOIN Websites
	ON access_log.site_id=Websites.id)
	GROUP BY Websites.name
	HAVING SUM(access_log.count) > 200;
####	SQL UCASE() 函数
	UCASE() 函数把字段的值转换为大写。
	语法：SELECT UCASE(column_name) FROM table_name;
	1.将某个列转化为大写
		SELECT UCASE(websites.url) AS url3 FROM websites
####	SQL LCASE() 函数
	LCASE() 函数把字段的值转换为大写。
	语法：SELECT LCASE(column_name) FROM table_name;
	1.将某个列转化为小写
		SELECT LCASE(country) FROM websites
####	SQL MID() 函数
	MID() 函数用于从文本字段中提取字符。
	语法：SELECT MID(column_name, start [,length]) FROM table_name;
	1.获取网站地址
		SELECT MID(websites.url, 4) FROM websites
####	SQL LEN() 函数
	LEN() 函数返回文本字段中值的长度。
	语法：SELECT LEN(column_name) FROM table_name;
	MySQL中函数为 LENGTH()
		SELECT LENGTH(column_name) FROM table_name;
	1.获取网址长度
		SELECT LENGTH(websites.url) FROM websites;
####	SQL ROUND() 函数
	ROUND() 函数用于把数值字段舍入为指定的小数位数。
	语法：SELECT ROUND(column_name, decimals) FROM table_name;
	1.对url四舍五入处理
		SELECT ROUND(url) FROM websites WHERE id=7;
####	SQL NOW() 函数	
	NOW() 函数返回当前系统的日期和时间。
	语法：SELECT NOW() FROM websites
####	SQL FORMAT() 函数
	FORMAT() 函数用于对字段的显示进行格式化。
	语法：SELECT FORMAT(column_name, format) FROM table_name;
	1.格式化日期
		SELECT name, url, DATE_FORMAT(Now(), '%Y-%m-%d') AS date FROM websites
####	参考手册
	[SQL参考手册](http://www.runoob.com/sql/sql-quickref.html)
	
>	参考文档
	[SQL](http://www.runoob.com/sql/sql-hosting.html)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	

















