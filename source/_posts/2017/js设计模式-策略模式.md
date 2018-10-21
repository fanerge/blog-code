---
title: js设计模式-策略模式
date: 2017-10-15 19:11:08
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	策略模式
定义：定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。
举例：表单效验（是否为空、长度、手机号、邮箱等等）
	计算年终奖（工资、效绩）
下面以年终将做说明：
比如公司的年终奖是根据员工的工资和绩效来考核的，绩效为A的人，年终奖为工资的4倍，
绩效为B的人，年终奖为工资的3倍，绩效为C的人，年终奖为工资的2倍；
```
// 一组策略类封装具体的算法
const Bouns = {
	A (salary){
		return salary * 4;
	},
	B (salary){
		return salary * 3;
	},
	C (salary){
		return salary * 2;
	}
};
Object.freeze(Bouns);

/*
* 计算年终奖 环境类Context
* @param {String} A 效绩等级
* @param {Number} 10000 每月工资
* @returns {Number} 40000 年终奖
*/
const calculateBouns = function (type, salary){
	return Bouns[type](salary);
};

// 测试年终奖计算方式
const demo1 = calculateBouns('A', 10000);
const demo2 = calculateBouns('B', 80000);
console.log(demo1, demo2); // 40000, 240000
```
PS：
策略模式指的是定义一系列的算法，把它们一个个封装起来，将不变的部分和变化的部分隔开，
实际就是将算法的使用和实现分离出来；算法的使用方式是不变的，都是根据某个算法取得计算后的奖金数，
而算法的实现是根据绩效对应不同的绩效规则；
一个基于策略模式的程序至少由2部分组成，第一个部分是一组策略类，策略类封装了具体的算法，
并负责具体的计算过程。第二个部分是环境类Context，该Context接收客户端的请求，
随后把请求委托给某一个策略类。
复合开放-封闭原则，可变的部分为策略类（一组算法），不变的部分为执行具体算法的方式。
#	表单验证
```
// 这里我们实现一组策略类封装具体的验证规则
const strategy = {
	// 是否为空
	isNotEmpty (value, errorMsg){
		if (value === '') {
			return errorMsg;
		}
	},
	// 最小长度
	minLength (value, errorMsg, length){
		if (value.length < length) {
			return errorMsg;
		}
	},
	// 手机号码格式
	mobileFormat (value,errorMsg){
		if(!/(^1[3|5|8][0-9]{9}$)/.test(value)) {
			return errorMsg;
		}
	}
};
Object.freeze(strategy);

var Validator = function(){
	this.cache = [];  // 保存效验规则
};
Validator.prototype.add = function(dom,rules) {
	var self = this;
	for(var i = 0, rule; rule = rules[i++]; ){
		(function(rule){
			var strategyAry = rule.strategy.split(":");
			var errorMsg = rule.errorMsg;
			self.cache.push(function(){
				var strategy = strategyAry.shift();
				strategyAry.unshift(dom.value);
				strategyAry.push(errorMsg);
				return strategys[strategy].apply(dom,strategyAry);
			});
		})(rule);
	}
};
Validator.prototype.start = function(){
	for(var i = 0, validatorFunc; validatorFunc = this.cache[i++]; ) {
	var msg = validatorFunc(); // 开始效验 并取得效验后的返回信息
	if(msg) {
		return msg;
	}
	}
};
// 代码调用
var registerForm = document.getElementById("registerForm");
var validateFunc = function(){
	var validator = new Validator(); // 创建一个Validator对象
	/* 添加一些效验规则 */
	validator.add(registerForm.userName,[
		{strategy: 'isNotEmpty',errorMsg:'用户名不能为空'},
		{strategy: 'minLength:6',errorMsg:'用户名长度不能小于6位'}
	]);
	validator.add(registerForm.password,[
		{strategy: 'minLength:6',errorMsg:'密码长度不能小于6位'},
	]);
	validator.add(registerForm.phoneNumber,[
		{strategy: 'mobileFormat',errorMsg:'手机号格式不正确'},
	]);
	var errorMsg = validator.start(); // 获得效验结果
	return errorMsg; // 返回效验结果
};
// 点击确定提交
registerForm.onsubmit = function(){
	var errorMsg = validateFunc();
	if(errorMsg){
		alert(errorMsg);
		return false;
	}
}
```
PS：此处代码来源于--腾讯.曾探的《javascript设计模式》，这能很好的说明策略模式的用途。

>	参考文档：
	[理解javascript中的策略模式](http://www.cnblogs.com/tugenhua0707/p/4722696.html)