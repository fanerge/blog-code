---
title: web常见的安全问题及解决方法
date: 2017-10-26 19:48:33
tags: ['安全']
categories: '安全'
copyright: true
---
#	前端安全问题
##	XSS 漏洞 	
	定义：
		跨站脚本攻击(Cross Site Scripting)，为了不和层叠样式表(Cascading Style Sheets, CSS)的缩写混淆，故将跨站脚本攻击缩写为XSS。
		恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的目的。
	举例：
	1.HTML DOM
		视图（textContent）
			<a href="/user/1">{{ user_name }}</a>
		数据
			<script>alert(1)</script>
		结果
			<a href="/user/1"><script>alert(1)</script></a>
		最基本的例子，如果此处不对 user_name 中的特殊符号进行 escape，就会造成 XSS。
	2.HTML Attribute
		视图（attribute）
			<img src="{{ image_url }}">
		数据
			onerror="alert(1)"
		结果
			<img src="" onerror="alert(1)">
		这个例子表明，如果只对尖括号进行 escape 是不够的，很多时候引号也需要被 escape。
		简单来说，对不同输出场景，需要使用不同的 escape 规则。
	3.Javascript
			<script>var user_data = {{ user_data|json_encode }};</script>
		数据
			{"exploit": "</script><script>alert(1);//"}
		结果
			<script>var user_data = {"exploit": "</script><script>alert(1);//"};</script>
		这是一个特别的例子，大多数人觉得，对于输出在 <script> 中的内容，json_encode 一下就安全了，其实不然。在这个例子中，XSS 仍然发生了。
	解决方案：
		1.在不同上下文中，使用合适的 escape 方式
		2.不要相信任何来自用户的输入，（不仅限于 POST Body，还包括 QueryString，甚至是 Headers）
##	 CSRF 漏洞
	定义：	
		CSRF（Cross-site request forgery）跨站请求伪造通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。
		它与XSS非常不同，XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。
		与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。
	理解CSRF攻击：攻击者盗用了你的身份，以你的名义发送恶意请求。
		CSRF能够做的事情包括：以你名义发送邮件，发消息，盗取你的账号，甚至于购买商品，虚拟货币转账......
		造成的问题包括：个人隐私泄露以及财产安全。	
	CSRF攻击攻击原理及过程如下：
       1. 用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；
       2. 在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；
       3. 用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；
       4. 网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；
       5. 浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。
	   网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的恶意代码被执行。 
	举例：	
	1.跨站转账
		银行网站A，它以GET请求来完成银行转账的操作，如：http://www.mybank.com/Transfer.php?toBankId=11&money=1000
	　　危险网站B，它里面有一段HTML的代码如下：
	　　<img src=http://www.mybank.com/Transfer.php?toBankId=11&money=1000>
	　　首先，你登录了银行网站A保存了cookie，然后访问危险网站B，噢，这时你会发现你的银行账户少了1000块......
	　　为什么会这样呢？原因是银行网站A违反了HTTP规范，使用GET请求更新资源。
		在访问危险网站B的之前，你已经登录了银行网站A，而B中的<img>以GET的方式请求第三方资源（这里的第三方就是指银行网站了，
		原本这是一个合法的请求，但这里被不法分子利用了），所以你的浏览器会带上你的银行网站A的Cookie发出Get请求，
		去获取资源“http://www.mybank.com/Transfer.php?toBankId=11&money=1000”，结果银行网站服务器收到请求后，
		认为这是一个更新资源操作（转账操作），所以就立刻进行转账操作......	
	解决方案：（让服务器分辨出是真实用户还是攻击者）	
		1.为请求带上 token
		2.验证HTTP Referer字段
			根据HTTP协议，在HTTP头中有一个字段叫Referer，它记录了该HTTP请求的来源地址。
		3.在HTTP头中自定义属性并验证
##  a链接的安全问题
阐述问题：当a链接有target="_blank"属性时，必须添加rel="noreferrer noopener"，不然新产生的页面可以通过window.opener来获取到父窗口的window对象。
```
<a href="www.baidu.com" target="_blank" rel="nofollow me noopener noreferrer" >
```
[聊聊 rel=noopener](https://juejin.im/post/5950f387f265da6c44072d6c)
[关于a标签target=“_blank"使用rel=noopener](https://juejin.im/post/5addbf3bf265da0b78682131)
	
#	后端安全问题
##	SQL 注入漏洞
	问题展示：
		<?php $user = mysql_query('SELECT * FROM USERS WHERE UserName="'.$_GET['user'].'"'); ?>
		那么当请求中 user 参数为 ";DROP TABLE USERS;-- 时，合成的 SQL 语句是：
		SELECT * FROM USERS WHERE UserName="";DROP TABLE USERS;--"
		// 这样将删除表 users
	解决方案：
		所有 SQL 语句都使用参数化查询（推荐）或对参数进行 escape（不推荐）
##	权限控制漏洞
	问题展示：
		未经授权可以进行的操作都是权限控制漏洞。
		例如，某些网站的后台操作就仗着「以为用户不知道入口地址」不进行任何权限检查，
		又例如，某些操作可能出现不允许更改的字段被用户递交更改（往往是那些网页上标记为 disabled 或者 hidden 的字段），
		再例如，允许通过 ../ 访问到不应该被访问的文件等（一般存在于 include 中）。
	解决方案：
		所有地方都要进行权限检查（如是否已登录、当前用户是否有足够权限、该项是否可修改等），
		总之，不要相信任何来自用户的数据，URL 当然也是。
##	SESSION 与 COOKIE
	问题展示：
		Session 和 Cookie 是两种用于存储用户当前状态的工具。
		某些开发者不了解 Session 与 Cookie 的区别，误用或者混用，导致敏感信息泄露或者信息篡改。
		Cookie 存储在浏览器上，用户可以查看和修改 Cookie。
	解决方案：
		Session 是存储在服务端的数据，一般来说安全可靠；大多数 Session 都是基于 Cookie 实现的
		（在 Cookie 中存储一串 SESSION_ID，在服务器上存储该 SESSION_ID 对应的内容）。
##	IP 地址
	问题展示：
		首先，用户的 IP 地址一般存储在 REMOTE_ADDR 中，这是唯一的可信的 IP 地址数据（视不同语言而定）。
		然后某些代理服务器，会将用户的真实 IP 地址附加在 header 的 VIA 或 X_FORWARDED_FOR 中（因为REMOTE_ADDR 是代理服务器自身的 IP）。
		所以，要获取用户 IP 地址，一般做法是，判断是否存在 VIA 或者 X_FORWARDED_FOR 头，
		如果存在，则使用它们，如果不存在则使用 REMOTE_ADDR。
		这就产生问题了，X_FORWARDED_FOR 或 VIA 是 HTTP Header，换句话说，它们是可以被伪造的。
		例如，在投票中，如果采信了 X_FORWARDED_FOR，往往意味着被刷票。
	解决方案：
		只使用 REMOTE_ADDR 作为获取 IP 的手段。
##	验证码
	问题展示：
		验证码里常见的问题有：非一次性、容易被识别。
	解决方案：
		非一次性指的是，同一个验证码可以一直被用下去。一般来说，每进行一次验证码校对（无论正确与否），
		都应该强制更换或清除 Session 中的验证码。
		关于识别问题，在当前科技水平下，不加噪点不加扭曲的验证码几乎是 100% 可识别的。

#	其他
##	nodeJs项目重新安装依赖包
rm -rf node_modules
rm package-lock.json
npm cache clear --force
npm install
		
>	参考文档：
	[Web 开发常见安全问题](http://blog.csdn.net/fengyinchao/article/details/50775121)
	[浅谈CSRF攻击方式](http://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html)

















