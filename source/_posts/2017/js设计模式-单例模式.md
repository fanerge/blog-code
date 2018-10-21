---
title: js设计模式-单例模式
date: 2017-10-15 16:12:31
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	单例模式
定义：单例就是保证一个类只有一个实例，实现的方法一般是先判断实例存在与否，如果存在直接返回，如果不存在就创建了再返回，这就确保了一个类只有一个实例对象。
	在JavaScript里，单例作为一个命名空间提供者，从全局命名空间里提供一个唯一的访问点来访问该对象。
举例：模态框、登录控件、注销控件
下面均以登录模态框做说明
#	引入代理实现单例模式
```
var CreateDiv = function(html) {
	this.html = html;
	this.init();
};

CreateDiv.prototype.init = function() {
	var div = document.createElement('div');
	div.innerHTML = this.html;
	document.body.appendChild(div);
}

var ProxySingletonCreateDiv = (function() {
	var instance;
	return function(html) {
		if (!instance) {
			instance = new CreateDiv(html);
		}
		return instance;
	}
})();

var a = new ProxySingletonCreateDiv('seven1');
var b = new ProxySingletonCreateDiv('seven2');

console.log(a === b); // true
```
PS：我们负责管理单例的逻辑移到了代理类ProxySingletonCreateDiv中。
这样一来，CreateDiv就变成了一个普通的类，他跟ProxySingletonCreateDiv组合起来可以达到单例模式的效果。

#	通用的单例模式
```
// 通用的单例验证方法
const getSingle = function (fn){
	let result;
	return function (){
		return result || (result = fn.apply(this, arguments));
	};
};

// 创建登录模态框
const createLoginLayer = function (){
	const div = document.createElement('div');
	div.innerHTML = '我是登录模态框';
	document.body.appendChild(div);
	return div;
};

// 为登录模态框使用单例模式
const createSingleLoginLoyer = getSingle(createLoginLayer);
const loginLayer1 = createSingleLoginLoyer(); // 第一个登录模态框
const loginLayer2 = createSingleLoginLoyer(); // 还是第一个登录模态框

console.log(loginLayer1 === loginLayer2); // true
```
这时不管你执行多少次 createSingleLoginLoyer() 方法，都只会生产一个 div 节点。
我们的通用单例模式就完成了。	
	
#	惰性单例
定义：惰性单例指的是在需要的时候才创建对象的实例。
以创建登录模态框为例
```
const createLoginLayer = (function (){
	let div;
	return function (){
		if (!div) {
			div = document.createElement('div');
			div.innerHTML = '我是登录模态框';
		}
		return div;
	};
})();

// 在点击按钮时才创建节点（惰性）
document.getElementById('login-btn').onclick = function (){
	var loginLayer = createLoginLayer();
	loginLayer.style.display = 'block';
};
```
这里的对惰性单例的实现主要是只有单例了网页上的登录按钮，才会去创建，登录框的dom节点，并且只是创建一次。
	
>	参考文档：
	[JavaScript设计模式----单例模式](https://segmentfault.com/a/1190000006049548)
	[设计模式之单例模式](http://www.cnblogs.com/TomXu/archive/2012/02/20/2352817.html)	
		