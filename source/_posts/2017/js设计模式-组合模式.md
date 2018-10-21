---
title: js设计模式-组合模式
date: 2017-11-02 19:57:34
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	组合模式的基础
定义：组合模式（Composite）将对象组合成树形结构以表示“部分-整体”的层次结构，组合模式使得用户对单个对象和组合对象的使用具有一致性。
作用：组合模式让你可以优化处理递归或分级数据结构。
使用场景：系统目录结构、网站导航结构、文件扫描、DOM的机制，一个DOM节点可以包含子节点，不管是父节点还是子节点都有添加、删除、遍历子节点的通用功能。
该模式由两部分构成：
1.子对象（Leaf）：组成组合对象的最基本对象。
2.组合对象（Composite）：由子对象组合起来的复杂对象。

#	组合模式的例子
文件扫描
```
// 定义组合对象（文件夹）
let Folder = function( name ){
	this.name = name;
	this.files = [];
};
Folder.prototype.add = function( file ){
	this.files.push( file );
};
Folder.prototype.scan = function(){
	console.log('开始文件扫描:' + this.name);
	for( let i = 0, file, files = this.files; file = files[i++]; ){
		file.scan();
	}
};

//定义叶子对象（文件）
let File = function( name ){
	this.name = name;
};
File.prototype.add = function(){
	throw new Error('文件下面不能再添加文件');
};
File.prototype.scan = function(){
	console.log('开始扫瞄：' + this.name);
};

let folder = new Folder('前端学习');
let folder1 = new Folder('JS学习');
let folder2 = new Folder('JQ学习');

let file1 = new File('JS设计模式');
let file2 = new File('JQ实战');
let file3 = new File('前端性能');

folder1.add(file1);
folder2.add(file2);
folder.add(folder1);
folder.add(folder2);
folder.add(file3);
folder.scan();

// 输出
开始文件扫描：前端学习
开始文件扫描：JS学习
开始扫瞄：JS设计模式
开始文件扫描：JQ学习
开始扫瞄：JQ实战
开始扫瞄：前端性能
```
PS：父类和子类必须具有相同的接口（方法），只不过它们相同的方法具有的功能不相同，例如父类的实例具有 add 方法作用是，可以添加文件夹 或者 文件。子类的实例具有 add 方法则不能添加文件夹 或者 文件，却抛出一个错误（子类重写父类的方法）。 

>	参考文档：
[0521组合模式](https://github.com/fanerge/Study-Notes/blob/master/2017%E5%B9%B4/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E9%9B%86%E5%90%88/0521%E7%BB%84%E5%90%88%E6%A8%A1%E5%BC%8F.txt)
[深入理解JavaScript系列（40）：设计模式之组合模式](http://www.cnblogs.com/TomXu/archive/2012/04/12/2435530.html)