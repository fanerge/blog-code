---
title: path模块-路径
date: 2017-09-16 10:16:31
category: "NodeJS"
tags: ['js','web开发','服务端']
---
###	path模块-路径
	在不同系统中，路径分隔符显示不同。POSIX 上的 / 与 Windows 上的 \
	说明：path 模块提供了一些工具函数，用于处理文件与目录的路径。
###	path.sep
	作用：提供了平台特定的路径片段分隔符。
		windows上是 \
		POSIX上是 /
###	path.win32 和 path.posix	
	要想在任何操作系统上处理 Windows 文件路径时获得一致的结果，可以使用 path.win32
		path.win32.basename('c:\\temp\\myfile.html'); // myfile.html
	要想在任何操作系统上处理 POSIX 文件路径时获得一致的结果，可以使用 path.posix
		path.posix.basename('/temp/myfile.html'); // myfile.html
###	path.delimiter
	作用：提供平台特定的路径分隔符。
		windows是;
		POSIX是:
		process.env.PATH; // '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin'
		process.env.PATH.split(path.delimiter); // ['/usr/bin', '/bin', '/usr/sbin', '/sbin', '/usr/local/bin']
###	path.basename(path[, ext]);
	作用：返回一个 path 的最后一部分。
		path.basename('/foo/bar/myfile.html'); // 'myfile.html'
		patn.basename('/foo/bar/myfile.html', '.html'); // 'myfile'
###	path.dirname(path);
	作用：返回一个path的目录名。
		path.dirname('/foo/bar/myfile'); // foo/bar
###	path.extname(path)
	作用：返回 path 的扩展名，即从 path 的最后一部分中的最后一个 .（句号）字符到字符串结束。
		path.extname('index.coffee.md'); // '.md'
###	path.format(pathObject);
	作用：会从一个对象返回一个路径字符串。 与 path.parse() 相反。
	参数：Object
		{
			dir: '',
			root: '',
			base: '',
			name: '',
			ext: ''
		}
		在windows中
		path.format({
			root: 'ignored',
			dir: '\home\user',
			base: 'file.txt'
		});
		// '\home\user\file.txt'
###	path.parse(path);
	作用：返回一个对象，对象的属性表示 path 的元素。
		path.parse('/home/user/file.txt');
		// {
			root: '/',
			base: '/home/user',
			ext: '.txt'.
			name: 'file'
		}
###	path.isAbsolute(path);
	作用：会判定 path 是否为一个绝对路径。
		path.isAbsolute('bar\baz'); // true
###	path.join([...path]);
	作用：使用平台特定的分隔符把全部给定的 path 片段连接到一起，并规范化生成的路径。
		path.join('/foo', 'bar', 'baz/asdf', 'quux', '..'); // 返回: '/foo/bar/baz/asdf'
###	path.normalize(path);
	作用：规范化给定的 path，并解析 '..' 和 '.' 片段。
		path.normalize('/foo/bar/baz/asdf/quux/..'); // '/foo/bar/baz/asdf'
###	path.relative(from, to);
	作用：返回从 from 到 to 的相对路径（基于当前工作目录）。
		path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
		// '../../impl/bbb'
###	path.resolve([...paths]);
	作用：把一个路径或路径片段的序列解析为一个绝对路径。
		path.resolve('/foo/bar', './baz'); // '/foo/bar/baz'
>	参考文档：
	[path参考-api](http://nodejs.cn/api/path.html)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	












