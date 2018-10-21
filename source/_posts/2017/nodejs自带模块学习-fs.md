---
title: fs模块-文件
date: 2017-09-16 10:18:38
category: "NodeJS"
tags: ['js','web开发','服务端']
---
###	fs模块-文件系统
	以下方法均有同步和异步版本
	说明：文件 I/O 是对标准 POSIX 函数的简单封装。 通过 require('fs') 使用该模块。 所有的方法都有异步和同步的形式。
	异步方法的最后一个参数都是一个回调函数。 传给回调函数的参数取决于具体方法，但回调函数的第一个参数都会保留给异常。 如果操作成功完成，则第一个参数会是 null 或 undefined。
	步方法时，任何异常都会被立即抛出。 可以使用 try/catch 来处理异常，或让异常向上冒泡。

###	异步的删除文件
	```
	fs.unlink('tmp/hello', function (err) {
		if (err) throw err;
		console.log('删除成功');
	});
	```
###	同步的删除文件
	```
	try {
		fs.unlinkSync('tmp/hello');
	}catch (e) {
		console.error(e);
	}
	console.log('删除成功');
	```
###	fs.contants
	作用：返回一个包含常用文件系统操作的常量的对象。FRWX（可见、读、写、执行）
###	fs.FSWatcher 类
	作用：从 fs.watch() 返回的对象是该类型。
	方法：change、error、close分别代表监视目录或文件的改变和出错和关闭监视。
		fs.watch('./tmp', (eventType, filename) => {
			if (filename) { console.log(filename); }
		});
###	fs.ReadStream 类
	作用：ReadStream 是一个可读流。
	属性：
		readStream.bytesRead - 已读取的字节数。
		readStream.path - 流正在读取的文件的路径，指定在 fs.createReadStream() 的第一个参数
	方法：open、close分别对应ReadStream文件fs.open()被打开和fs.close()被关闭时触发。
###	fs.Stats 类
	从 fs.stat()、fs.lstat() 和 fs.fstat() 及其同步版本返回的对象都是该类型。
	stats.isFile()
	stats.isDirectory()
	stats.isBlockDevice()
	stats.isCharacterDevice()
	stats.isSymbolicLink() (仅对 fs.lstat() 有效)
	stats.isFIFO()
	stats.isSocket()
###	Stat 时间值
	atime "访问时间" - 文件数据最近被访问的时间。
	mtime "修改时间" - 文件数据最近被修改的时间。
	ctime "变化时间" - 文件状态最近更改的时间。
	birthtime "创建时间" - 文件创建的时间。 
###	fs.WriteStream类
	作用：WriteStream 一个可写流。
	属性：
		writeStream.bytesWritten - 已写入的字节数。 不包括仍在排队等待写入的数据。
		writeStream.path - 流正在写入的文件的路径，指定在 fs.createWriteStream() 的第一个参数。
	方法：
		open() - 当 WriteStream 文件被打开时触发。
		close() - 当 WriteStream 底层的文件描述符已被使用 fs.close() 方法关闭时触发。
###	fs.access(path[, mode], callback);
	作用：判断用户是否有权限操作给定的目录或者是文件。
		如果要检查一个文件是否存在且不操作它。
	mode参数的可选项：
		fs.constants.F_OK - path 文件对调用进程可见（默认）。
		fs.constants.R_OK - path 文件可被调用进程读取。
		fs.constants.W_OK - path 文件可被调用进程写入。
		fs.constants.X_OK - path 文件可被调用进程执行。
###	fs.accessSync(path[, mode]);
	fs.access() 的同步版本。
###	fs.appendFile(file, data[, options], callback);
	作用：异步地追加数据到一个文件，如果文件不存在则创建文件。
	fs.appendFile('filename.txt', 'data to append' , (err) => {
		if (err) throw err;
		console.log('添加成功');
	});
###	fs.chmod(path, mode, callback);
	作用： 更改文件属性（存取模式）(mode)。
###	fs.chown(path, uid, gid, callback);
	作用：修改文件目录属主。用户ID，群体身份（共享资源系统使用者的身份）
###	fs.close(fd, callback);
	作用：关闭已打开的文件。
###	fs.createReadStream(path[, options]);
	作用：返回一个新建的ReadStream对象。
###	fs.createWriteStream(path[, options]);
	作用：返回一个新建的WriteStream对象。
###	fs.existsSync(path);
	作用： 判断文件是否存在的同步版，异步已经废弃了。
###	fs.fchmod(fd, mode, callback);
	作用：更改文件权限（文件描述符）。
###	fs.fchown(fd, uid, gid, callback);
	作用：更改文件所有权(文件描述符)。
###	fs.fdatasync(fd, callback);
	作用：刷新数据到磁盘。
###	fs.fstat(fd, callback);
	作用：返回文件的详细信息。
###	fs.fsync(fd, callback);
	作用：同步缓存数据到磁盘。
###	fs.ftruncate(fd, len, callback);
	作用：截取文件内容。
###	fs.futimes(fd, atime, mtime, callback);
	作用：更改一个文件所提供的文件描述符引用的文件的时间戳。
###	fs.lchmod(path, mode, callback);
	作用：更改文件权限(不解析符号链接)。
###	fs.lchown(path, uid, gid, callback)
	作用：更改文件所有权（不解析符号链接）。
###	fs.link(existingPath, newPath, callback);
	作用：创建硬链接(只能在本券中)。
###	fs.lstat(path, callback);
	作用：获取文件信息(不解析符号链接)。
###	fs.mkdir(path[, mode], callback);
	作用：创建文件目录，如果目录已存在，将抛出异常。
###	fs.mkdtemp(prefix[, options], callback);
	作用：创建临时目录。
###	fs.open(path, flags[, mode], callback);
	作用：打开文件。
###	fs.read(fd, buffer, offset, length, position, callback);
	作用：读取文件内容。
###	fs.readdir(path[, options], callback);
	作用：读取文件目录。
###	fs.readFile(path[, options], callback);
	作用：读取文件。
###	fs.readlink(path[, options], callback);
	作用：读取软连接信息。
###	fs.readSync(fd, buffer, offset, length, position);
	作用：读取文件内容，返回字节数。
###	fs.realpath(path[, options], callback);
	作用：获取真实路径。
###	fs.rename(oldPath, newPath, callback);
	作用：重命名路径。
###	fs.rmdir(path, callback);
	作用：删除文件目录。
###	fs.stat(path, callback);
	作用：获取文件信息。
###	fs.symlink(target, path[, type], callback);
	作用：创建符号链接。
###	fs.truncate(path[, len], callback);
	作用：文件内容截取操作。
###	fs.unlink(path, callback);
	作用：删除文件操作。
###	fs.unwatchFile(filename[, listener]);
	作用：解除文件监听。
###	fs.utimes(path, atime, mtime, callback);
	作用：修改文件时间戳。
###	fs.watch(filename[, options][, listener]);
	作用：监控文件。
###	fs.watchFile(filename[, options], listener);
	作用：监控文件。
###	fs.write(fd, buffer[, offset[, length[, position]]], callback);
	作用：向文件写数据。
###	fs.writeFile(file, data[, options], callback);
	作用：向文件写数据。		
>	参考文档：
	[fs参考-api](http://nodejs.cn/api/fs.html)
	[fs模块](http://blog.csdn.net/zza000000/article/details/54341943)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	












