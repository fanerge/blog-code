---
title: js设计模式-职责链模式
date: 2017-11-06 21:11:22
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	职责链模式的基础
定义：职责链模式（Chain of responsibility）是使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系。将这个对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理他为止。职责链模式的名字非常形象，一系列可能会处理请求的对象被该连接成一条链，请求在这些对象之间依次传递，直到遇到一个可以处理它的对象，我们把这些对象成为链中的节点。
优点：请求发送者只需要知道链中的第一个节点，从而弱化了发送者和一组接收者之间的强联系。
使用场景：挤公交车递钱（只有售票员可以收钱）、交押金预定手机
	
#	交押金预定手机
>	假设一个电视购物网站对于某部新上市的手机经过了2轮缴纳500元定金与200元定金的预定，现在已经到了正式购买的阶段。
支付了500元定金的用户在购买阶段可以使用100元优惠券，200元定金可以使用50元优惠券，普通用户没有优惠且当库存不足不一定能买到。
约定：
orderType: 表示订单类型(定金或者普通用户),code的值为1时候是500元定金用户，为2是200元定金用户，为3则是普通用户。
pay : 表示用户是否支付定金，用户虽然下过500元定金的订单但是如果他一直没有支付定金，那么只能降级为普通用户。
stock : 仅用户普通用户的库存数量，定金用户不受限制。
首先定义三种预定的客户的订单并且让每种客户订单有满足当前预定条件和不满足当前预定条件（需后面继续处理）
```
// 500 元客户订单
var order500 = function (orderType,pay,stock) {
	if(orderType === 1 && pay){
		 console.log('500 rmb deposit, get 100 coupon ')
	} else {
		return 'nextSuccessor'  // unknow the next node but always pass to next.
	}
};

// 200 元客户订单
var order200 = function (orderType,pay,stock) {
	if(orderType === 2 && pay){
		console.log('200 rmb deposit , get 50 coupon')
	} else{
		 return 'nextSuccessor'; 
	}
};

// 无预约客户订单
var orderNormal = function (orderType,pay,stock) {
	if(stock > 0){
		 console.log('normal buy no coupon')
	} else{
		 console.log('the stock lack')
	}
};

// 定制职责链对象（作用是形成订单职责链）
var Chain = function (fn) {
	this.fn = fn;
	this.successor = null;
};
// 设置职责链
Chain.prototype.setNextSuccessor = function (successor) {
	return this.successor = successor;
};
// 设置每个节点的调用方式
Chain.prototype.passRequest = function () {
	var ret = this.fn.apply(this.arguments);
	if(ret === 'nextSuccessor'){
		return this.successor && this.successor.passRequest.apply(this.successor,arguments)
	}
	return ret;
};

// 现在我们把3个订单函数分别包装成职责链的节点
var chainOrder500 = new Chain(order500);
var chainOrder200 = new Chain(order200);
var chainOrderNormal = new Chain(orderNormal);

// 这里我们把上面封装的节点连成一条线，依次判断执行
chainOrder500.setNextSuccessor(chainOrder200)
chainOrder200.setNextSuccessor(chainOrderNormal)

// 测试代码
chainOrder500.passRequest(1,true,6); // 500 rmb deposit, get 100 coupon
chainOrder500.passRequest(2,true,4); // 200 rmb deposit , get 50 coupon
```
假如我们又想支持，300元定金购买，那我们就在改链中增加一个节点即可
```
var order300 = function () {
// todo
};
chainOrder300 = new Chain(chainOrder300)
chainOrder500.setNextSuccessor(chainOrder300)
chainOrder300.setNextSuccessor(chainOrder200)
```
我们可以自由灵活的增加移除和链中的节点顺序，这样就很简单能满足瞬息万变的需求了。

>	参考文档：
	[三分钟教会你JS设计模式之职责链模式](http://www.jianshu.com/p/19b0033423be)
	[深入理解JavaScript系列（38）：设计模式之职责链模式](http://www.cnblogs.com/TomXu/archive/2012/04/10/2435381.html)

