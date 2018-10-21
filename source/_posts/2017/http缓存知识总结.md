---
title: http缓存相关的知识
date: 2017-09-01 18:56:40
category: "通信协议"
tags: ['http','缓存']
copyright: true
---
####	http缓存整理
#####	缓存过程
	当一个用户发送一个静态资源请求通过下面几步获取资源：
1.	当第一次发送请求时，http返回200的状态码。
2.	在没有关闭缓存请求的时候，则返回header中返回包含last-Modified以及Etag和Expires的字段，然后将文件保存在Cache目录下。
3.	后续请求该文件时，先本地查找该资源。如果本地缓存存在该资源，但不知道是否过期，则发送一个http请求到服务器，然后服务器来判断。
	如果该文件没有改动，则返回304，继续使用本地资源。
	如果该文件发生改动，服务器返回该资源并返回200。
	如果服务器没有这个资源，则返回404。
#####	http头部缓存相关key
######	request header缓存相关
	cache-control：
		其缓存指令对于前端常用的有如下no-cache、no-store、max-age这几个值；
	if-none-match：
		该字段与响应中的eTag一起使用，表示检查实体是否有更新改变;客户端第一次发送请求时候响应报文会包含字段Etag，表示资源状态，当资源改变后该值也会改变（客户端不必关心该值怎么生成）
		然后缓存保存下该字段，第二次已经有该缓存时候在浏览本地缓存时候会将该值赋给if-none-match字段发送给服务器，服务器将发送的值与当前的状态进行对比，
		如果值一样的话则答复304去使用缓存数据，如果值改变了则发送最新数据给客户端替代现有缓存数据，并且返回状态200;
	if-modified-since:
		该字段与last-modified配合使用，跟上述原理差不多，都是响应端先返回一个last-modified时间字段，再次请求时候 request头部会将缓存中的last-modified字段拿出来赋给if-modified-since，
		发送给服务器，服务器去判断时间是否过期，如未过期则返回304，告诉客户使用缓存数据，如果过期则重新返回一个last-modified并且返回200；
######	response header缓存相关：	
	Etag：
		刚才也说过 是跟if-none-match配合去使用，它根据实体内容生成的一段hash字符串（类似于MD5或者SHA1之后的结果），可以标识资源的状态。 当资源发送改变时，ETag也随之发生变化。
		使用Etag主要是为了解决根据时间无法解决的问题：比如文件修改频繁（秒之内修改），导致根据时间无法判断是否更新；以及修改时间变了，但是内容没变（我们应该认为该文件是没变的）
	expires：
		表示缓存过期时间例如:expires:Mon Dec 30 2011 11:01:19 GMT，跟cache-control中的max-age作用一样，不过在碰见max-age之后，该值会被覆盖从而被max-age替代;
	last-modified:
		表示文件最后修改时间；

#####	实现有关前端对于缓存的操作
######	使用缓存
	方法一：
	在服务器进行配置其max-age或者expires使其设置一个过期值为当前一年之后。这样每次进行检验时候都会使用缓存中文件。
	例如在.htaccess中
	<IfModule mod_headers.c>
	 <FilesMatch ".(gif|jpg|jpeg|png|ico)$">
	Header set Cache-Control "max-age=604800"
	 </FilesMatch>
	方法二：
	前端设置if-modified-since去设置一个上次修改时间大于当前日期，
	方法三：
	服务器端根据etag去判断是否匹配来根据实际业务来使用缓存；
	后面两个方法属于弱缓存数据头，需要浪费http连接，所以建议使用第一种方式;
######	禁用缓存
	方法一：
	可以在meta标签标明
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0"> 
	方法二：
	也可以动态去setRequestHeader，强制不用缓存设置组合如下：
	cache-control='no-cache,no-store'
	pragma='no-cache'
	if-modified-since=0;
	方法三：
	请求端设置if-modified-since为已经过期的某个时间，可以是几年前或者几十年前。
	方法四：
	服务端设置Expires为过期某个时间，例如PHP中header("Expires: Mon, 26 Jul 1997 05:00:00 GMT");
	实际开发中如果需要一致性检测则尽量去配合Etag以及last-Modified去进行比较然后返回使用缓存还是新数据;这个有点偏服务器端了，不再赘述
	方法五：
	url后面加随机数或者时间戳url += “&random=” + Math.random()这个方法js以及PHP经常用，原理就是每个请求的url都不一样这样一来缓存中找不到对应数据，就自动去服务器寻找最新资源;






































