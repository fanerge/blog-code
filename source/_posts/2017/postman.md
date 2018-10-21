---
title: postman
date: 2017-09-26 19:32:30
category: "调试"
tags: ['http','调试']
---
	虽然一直使用postman进行http相关的调试，但一直都停留在简单使用，并没有对其的深入学习。
	现在比较全面的学习下post相关功能。
###	postman仪表盘布局
	分为左右两栏（sidebar + main）
		siderbar：Search + History + Collections
		main：
			第一行：请求方式 + URL + Params + Send(Send and Download) + Save(Save as)
			第二行(request)：Authorization + Headers + Body + Pre-request Script(请求发送前运行的脚本) + Tests + Code(转化为相应的语言请求代码)
			第三行(response)：Body + Cookies + Headers + Tests + Status(响应状态) + Time(响应等待时间) 	
###	请求	
	HTTP请求的4部分:URL，method，headers，body。
	对于body说明：
		1.mutipart/form-data：是网页表单用来传输数据的默认格式。（提交表单上传文件）
		2.x-www-form-urlencoded：同前面一样，注意,你不能上传文件通过这个编码模式。
		3.raw：raw request可以包含任何东西。所有填写的text都会随着请求发送。
		4.binary：image, audio or video files.text files 。 也不能保存历史，每次选择文件，提交。
	身份验证Authentication：
		1.Basic Auth：填写用户名和密码，点击Refresh headers
		2.Digest Auth：要比Basic Auth复杂的多。使用当前填写的值生成authorization header。
			所以在生成header之前要确保设置的正确性。
			如果当前的header已经存在，postman会移除之前的header。
		3.OAuth 1.0a：postman的OAuth helper让你签署支持OAuth 1.0基于身份验证的请求。
			OAuth不用获取access token,你需要去API提供者获取的。
			OAuth 1.0可以在header或者查询参数中设置value。
		4.OAuth 2.0：postman支持获得OAuth 2.0 token并添加到requests中。	
###	响应	
	对于body说明：
		1.Pretty：格式化了JSON和XML，方便查看。
		2.Raw：是text文本。
		3.Preview：返回html页面。
	cookies：可以显示browser cookies，需要开启Interceptor。
###	Writting Test
	暂不涉及！！！
	Postman的Tests标签可以用来写测试
###	运行Collections
	Postman测试管理的单位是测试集（Collections），测试集内可以创建文件夹(Folder)和具体的请求(Requests)。
	postman允许你运行collection，你可以运行任意的次数。 
	最后会给出一个整体运行的结果。
	会保存每一次运行的结果，提供给你比较每一次运行结果的不同。
###	Postman开发者模式
	使用环境变量可以方便的在开发和测试环境进行切换。
	1.点击环境管理按钮添加环境。
	2.通常会有开发和测试两个环境，分别添加相应的环境变量。
		环境一：ev_develop
			url：127.0.0.1.8081
		环境二：ev_test
			url：10.1.1.133.8080
	3.使用环境变量
		{{url}}/api/v1/currentUser.json
		No Environment选择对应的环境即可
	4.查看和编辑环境变量
		下拉select和眼睛图标
###	Postman接口测试
	1.对于同一接口的多个测试请求，可统一使用后缀命名
		在Collections中
		如测试登录的不同情况：
		login_normal
		login_bad_username
		login_bad_password
		login_miss_username
		login_miss_password
	2.对于关键字搜索测试，可使用关键字作为后缀，以保持与开发关键字一致
		如测试评分资料内容列表展示
		评分资料内容列表_pageNum
		评分资料内容列表_numPerPage
		评分资料内容列表_docName
	3.对于需要额外说明的测试请求，需要在描述里增加注释
		点击edit添加Description
###	Postman接口测试进阶	
	1.全局变量的创建方式与环境变量相同	
	2.使用方式也是相同的，不同的是全局变量可以在不同环境下共用	
>	参考文档：
	[postman 使用详解](http://blog.csdn.net/flowerspring/article/details/52774399)
	[postmen 使用入门](https://jingyan.baidu.com/article/0f5fb09907e3046d8334ea2f.html)
	
	
	
	
	