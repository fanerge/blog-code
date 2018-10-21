---
title: util模块-工具
date: 2017-09-16 10:18:15
category: "NodeJS"
tags: ['js','web开发','服务端']
---
###	util模块
	说明：util 模块主要用于支持 Node.js 内部 API 的需求。 大部分实用工具也可用于应用程序与模块开发者。
###	util.callbackify(original)
###	util.debuglog(section)
	作用：util.debuglog() 方法用于创建一个函数，基于 NODE_DEBUG 环境变量的存在与否有条件地写入调试信息到 stderr。
		const debuglog = util.debuglog('foo');
		debuglog('hello from foo [%d]', 123);
		若程序的环境运行是带上NODE_DEBUG=foo,则FOO 3245: hello from foo [123]
		3234是进程id
###	util.deprecate(function, string)
	作用：util.deprecate() 方法会包装给定的 function 或类，并标记为废弃的。
		// 在util模块中
		exports.puts = util.deprecate(function() {
		  for (let i = 0, len = arguments.length; i < len; ++i) {
			process.stdout.write(arguments[i] + '\n');
		  }
		}, 'util.puts: 使用 console.log 代替');
###	util.format(format[, ...args])
	作用：返回一个格式化后的字符串，使用第一个参数作为一个类似 printf 的格式。
	对应的占位符：
		%s - 字符串
		%d - 数值
		%i - Integer
		%f - Float
		%j - JSON
		%o - Object
		%% - 单个百分号（'%'）。不消耗参数。
		util.format('%s:%d', 'fan', 100); // 'fan:100'
###	util.inherits(constructor, superConstructor)
	注意，不建议使用 util.inherits()。 请使用 ES6 的 class 和 extends 关键词获得语言层面的继承支持。
	作用：从一个构造函数中继承原型方法到另一个。 constructor 的原型会被设置到一个从 superConstructor 创建的新对象上。
		util.inherits(MyStream, EventEmitter);
###	util.inspect(object[, options])
	作用：返回 object 的字符串表示，主要用于调试。 附加的 options 可用于改变格式化字符串的某些方面。
###	自定义 util.inspect 颜色
	作用：可以通过 util.inspect.styles 和 util.inspect.colors 属性全局地自定义 util.inspect 的颜色输出（如果已启用）。
###	util.inspect.custom
	作用：可被用于声明自定义的查看函数。
###	util.inspect.defaultOptions
	作用：defaultOptions 值允许对被 util.inspect 使用的默认选项进行自定义。
###	util.promisify(original)
	作用：让一个遵循通常的让一个遵循通常的 Node.js 回调风格的函数， 即 (err, value) => ... 回调函数是最后一个参数, 返回一个返回值是一个 promise 版本的函数。	
>	参考文档：
	[util参考-api](http://nodejs.cn/api/util.html)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	












