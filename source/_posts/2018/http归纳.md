---
title: HTTP知识（前端相关）
date: 2018-07-05 20:21:36
tags: 'http'
categories: 'http'
copyright: true
---
#  安全（Safe）
说一个HTTP方法是安全的，是说这是个不会修改服务器的数据的方法。也就是说，这是一个对服务器只读操作的方法。这些方法是安全的：GET，HEAD和OPTIONS。
所有安全的方法都是idempotent（幂等），有些不安全的方法如PUT和DELETE则不是。
PS：网络爬虫也是依赖于安全的HTTP方法，安全方法: GET, HEAD, OPTIONS；非安全方法: PUT, DELETE, POST。
[MDN-Safe](https://developer.mozilla.org/zh-CN/docs/Glossary/safe)

#  幂等（Idempotent）
一个HTTP方法是幂等的，指的是同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态（不是返回的状态码而是数据）也是一样的。换句话说就是，幂等方法不应该具有副作用（统计用途除外）。在正确实现的条件下，GET，HEAD，PUT和DELETE 等方法都是幂等的，而 POST 方法不是。所有的 safe 方法也都是幂等的。

#  HTTP方法比较
##  GET 
GET方法请求一个指定资源的表示形式，使用GET的请求应该只被用于获取数据。
##  HEAD
HEAD方法请求一个与GET请求的响应相同的响应，但没有响应体（也就是说只存在响应头无响应体）。
PS：该请求方法的一个使用场景是在下载一个大文件前先获取其大小再决定是否要下载, 以此可以节约带宽资源。
##  POST
POST方法用于将实体提交到指定的资源，通常导致状态或服务器上的副作用的更改。 
##  PUT
请求方法 PUT  用于新增资源或者使用请求中的有效负载替换目标资源的表现形式。
PS：PUT 与 POST 方法的区别在于，PUT方法是幂等的。
##  DELETE
DELETE 请求方法用于删除指定的资源。
##  CONNECT
CONNECT 方法可以开启一个客户端与所请求资源之间的双向沟通的通道。它可以用来创建隧道（tunnel）。
##  OPTIONS
OPTIONS 方法 用于获取目的资源所支持的通信选项。在 CORS 作为预检请求。
##  TRACE
TRACE 方法 实行了向目标资源的沿路径的消息环回(loop-back)测试 ，提供了一种实用的debug机制。
##  PATCH
请求方法 PATCH  用于对资源进行部分修改。
PS：在HTTP协议中， PUT 方法已经被用来表示对资源进行整体覆盖， 而 POST 方法则没有对标准的补丁格式的提供支持。不同于  PUT 方法，而与 POST 方法类似，PATCH  方法是非幂等的，这就意味着连续多个的相同请求会产生不同的效果。

#  浏览器缓存
##  强缓存
###  Cache-Control（请求头和响应头）
通用消息头被用于在http 请求和响应中通过指定指令来实现缓存机制。
max-age：指定设置缓存最大的有效时间（单位为s）。
s-maxage：覆盖max-age 或者 Expires 头，但是仅适用于共享缓存(比如各个代理CDN)，并且私有缓存中它被忽略。
public：表明响应可以被接收的客户端、代理服务器等缓存。
private：表明响应只能被客户端缓存，不能作为共享缓存（即代理服务器不能缓存它）。
no-cache：在释放缓存副本之前，强制高速缓存将请求提交给原始服务器进行验证。
no-store：缓存不应存储有关客户端请求或服务器响应的任何内容。
###  Expires（响应头）
响应头包含日期/时间， 即在此时候之后，响应过期。
PS：如果在Cache-Control响应头设置了 "max-age" 或者 "s-max-age" 指令，那么 Expires 头会被忽略。
##  协商缓存
###  Last-modified（响应头）
它包含源头服务器认定的资源做出修改的日期及时间。 
工作原理：浏览器首次请求资源是响应头中包含 Last-modified 字段，当再次请求时将上次 Last-modified 的值赋值给 If-Modified-Since 请求头，服务器根据对应资源的修改时间于 If-Modified-Since 的值进行比较，从而断定该资源是否被修改再响应304或200。
###  ETag（响应头）
工作原理：和 Last-modified（If-Modified-Since）工作原理基本一致，只不过服务器判断资源是否被更改的条件不是文件修改时间而是为资源生产的hash值是否发生变化。
PS：可以弥补 max-age 和 Last-modified 只能精确到秒的缺陷。
[alloyteam-浅谈Web缓存](http://www.alloyteam.com/2016/03/discussion-on-web-caching/)


