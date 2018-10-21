---
title: url模块-网址
date: 2017-09-16 15:04:28
category: "NodeJS"
tags: ['js','web开发','服务端']
---
###	url 模块提供了一些实用函数，用于 URL 处理与解析。
###	URL 字符串与 URL 对象
	一个 URL 字符串是一个结构化的字符串，它包含多个有意义的组成部分。 当被解析时，会返回一个 URL 对象，它包含每个组成部分作为属性。
	┌─────────────────────────────────────────────────────────────────────────────────────────────┐
	"  https:   //    user   :   pass   @ sub.host.com : 8080   /p/a/t/h  ?  query=string   #hash "
	│          │  │          │          │   hostname   │ port │          │                │       │
	│          │  │          │          ├──────── ──────┴──────┤          │                │       │
	│ protocol │  │ username │ password │        host         │          │                │       │
	├──────────┴──┼──────────┴──────────┼─────────────────────┤          │                │       │
	│   origin    │                     │       origin        │ pathname │     search     │ hash  │
	├─────────────┴─────────────────────┴─────────────────────┴──────────┴────────────────┴───────┤
	│                                            href                                             │
	└─────────────────────────────────────────────────────────────────────────────────────────────┘
###	Constructor: new URL(input[, base])
	```
	const { URL } = require('url');
	const myURL = new URL('/foo', 'https://example.org/');
	// https://example.org/foo
	```
一下实例均以：var myURL = new URL('https://user:pass@sub.host.com:8080/p/a/t/h?query=string#hash');
###	url.hash
	作用：获取及设置URL的分段(hash)部分。
		myURL.hash; // #hash
###	url.host
	作用：获取及设置URL的主机(host)部分。
		myURL.host; // user:pass@sub.host.com:8080
###	url.hostname
	作用：获取及设置URL的主机名(hostname)部分。 url.host和url.hostname之间的区别是url.hostname不 包含端口。
		myURL.hostname; // user:pass@sub.host.com
###	url.href
	作用：获取及设置序列化的URL。
		myURL.href; // https://user:pass@sub.host.com:8080/p/a/t/h?query=string#hash
###	url.origin
	作用：获取只读序列化的URL orgin部分。
		myURL.origin; // https://user:pass@sub.host.com:8080
###	url.password
	作用：获取及设置URL的密码(password)部分。
		myURL.password; // pass
###	url.pathname
	作用：获取及设置URL的路径(path)部分。
		myURL.pathname; // /p/a/t/h
###	url.port
	作用：获取及设置URL的端口(port)部分。
		myURL.port; 8080
###	url.protocol
	作用：获取及设置URL的协议(protocol)部分。
		myURL.protocol; // https
###	url.search
	作用：获取及设置URL的序列化查询(query)部分部分。
		myURL.search; // ?query=string
###	url.searchParams
	作用：获取表示URL查询参数的URLSearchParams对象。该属性是只读的；
		myURL.searchParams; // 
###	url.username	
	作用：获取及设置URL的用户名(username)部分。
		myURL.username; // user
###	url.toString()
	作用：在URL对象上调用toString()方法将返回序列化的URL。返回值与url.href和url.toJSON()的相同。
		如果需要更大灵活性，require('url').format()可能更合适。
###	url.toJSON()
	在URL对象上调用toJSON()方法将返回序列化的URL。
###	Constructor: new URLSearchParams(obj)
	通过使用查询哈希映射实例化一个新的URLSearchParams对象，obj的每一个属性的键和值将被强制转换为字符串。
	urlSearchParams.append(name, value)
		在查询字符串中附加一个新的键值对。
	urlSearchParams.entries()
		返回: <Iterator> 在查询中的每个键值对上返回一个ES6迭代器。
	urlSearchParams.forEach(fn[, thisArg])
		在查询字符串中迭代每个键值对，并调用给定的函数。
	urlSearchParams.get(name)
		返回键是name的第一个键值对的值。如果没有对应的键值对，则返回null。
	urlSearchParams.getAll(name)
		返回键是name的所有键值对的值，如果没有满足条件的键值对，则返回一个空的数组。
	urlSearchParams.has(name)
		如果存在至少一对键是name的键值对则返回 true。
	urlSearchParams.keys()
		在每一个键值对上返回一个键的ES6迭代器。
	urlSearchParams.set(name, value)
		将URLSearchParams对象中与name相对应的值设置为value。
	urlSearchParams.sort()	
		按现有名称就地排列所有的名称-值对。	
	urlSearchParams.toString()	
		返回查询参数序列化后的字符串，必要时存在百分号编码字符。
	urlSearchParams.values()
		在每一个键值对上返回一个值的ES6迭代器。
	urlSearchParams[@@iterator]()	
		返回在查询字符串中每一个键值对的ES6迭代器。
	url.domainToASCII(domain)	
		返回Punycode ASCII序列化的domain. 如果domain是无效域名，将返回空字符串。
	url.domainToUnicode(domain)	
		返回Unicode序列化的domain. 如果domain是无效域名，将返回空字符串。
	url.format(URL[, options])	
		返回一个WHATWG URL对象的可自定义序列化的URL字符串表达。
>	参考文档：
	[url参考-api](http://nodejs.cn/api/url.html)