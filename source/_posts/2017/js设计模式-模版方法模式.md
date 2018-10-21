---
title: js设计模式-模版方法模式
date: 2017-11-01 20:48:34
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	模版方法模式
定义：模板方法模式由二部分组成，第一部分是抽象父类，第二部分是具体实现的子类，一般的情况下是抽象父类封装了子类的算法框架，包括实现一些公共方法及封装子类中所有方法的执行顺序，子类可以继承这个父类，并且可以在子类中重写父类的方法，从而实现自己的业务逻辑。
使用场景：（主要用于步骤相似的事情）
1.	泡饮品（茶 和 coffee）
2.	公司面试（百度面试 和 阿里面试）

#	IT公司面试
下面就以IT公司面试作为父类，百度面试作为子类来实现面试流程模版方法。
（1.笔试 >> 2.技术面试 >> 3.领导面试 >> 4.等offer）
##	定义父类
`let ITInterview = function(){};`
###	笔试
```
// 笔试
ITInterview.prototype.writtenTest = function(){
	console.log("某公司笔试测试");
};
```
###	技术面试
```
// 技术面试
ITInterview.prototype.technicalInterview = function(){
	console.log("某公司技术面试测试");
}; 
```
###	领导面试
```
// 领导面试
ITInterview.prototype.leader = function(){
	console.log("某公司leader来面试了");
};
```
###	等offer
```
// 等通知
ITInterview.prototype.waitNotice = function(){
	console.log("某公司的offer到了");
};
```
###	定义模版方法
作用：封装了子类的算法框架，包括实现一些公共方法及封装子类中所有方法的执行顺序。
```
// 当然，这是一切顺利的流程......
ITInterview.prototype.init = function(){
	this.writtenTest();
	this.technicalInterview();
	this.leader();
	this.waitNotice();
};
```
##	定义子类（百度面试）
首先子类需要重父类哪里继承所有的方法
```
let BaiDuInterview = function(){};
BaiDuInterview.prototype = new ITInterview();
```
###	百度笔试
```
// 笔试
BaiDuInterview.prototype.writtenTest = function(){
	console.log("百度公司笔试测试"); // 无非就是面试题不一样呗
};
```
###	百度技术面试
```
// 技术面试
BaiDuInterview.prototype.technicalInterview = function(){
	console.log("百度公司技术面试测试");
}; 
```
###	百度领导面试
```
// 领导面试
BaiDuInterview.prototype.leader = function(){
	console.log("百度公司leader来面试了");
};
```
###	等offer
```
// 等通知
BaiDuInterview.prototype.waitNotice = function(){
	console.log("百度公司的offer到了");
};
```
###	测试执行
let baiduInterview = new BaiDuInterview();
baiduInterview.init(); // 子类还可以重写父类的init方法，这样各个公司面试的流程就不一样了。
依次输出：
百度公司笔试测试
百度公司技术面试测试
百度公司leader来面试了
百度公司的offer到了

>	参考文档：
[JS设计模式之模板方法](http://blog.csdn.net/xu_ya_fei/article/details/51628310)
[JavaScript：设计模式之模板方法](https://www.2cto.com/kf/201507/420128.html)
[javascript模板方法模式](http://www.cnblogs.com/tugenhua0707/p/4780227.html)
















