---
title: js设计模式-享元模式
date: 2017-11-07 20:59:00
tags: ['设计模式']
categories: '设计模式'
copyright: true
---
#	享元模式的基础
定义：享元模式是一种用于性能优化的模式，如果系统中因为创建了大量类似的对象而导致内存不足或占用过高这种模式就非常有用了。
使用场景：第一种是应用在数据层上，主要是应用在内存里大量相似的对象上；第二种是应用在DOM层上，享元可以用在中央事件管理器上用来避免给父容器里的每个子元素都附加事件句柄。
作用：Flyweight中有两个重要概念--内部状态intrinsic和外部状态extrinsic之分，内部状态就是在对象里通过内部方法管理，而外部信息可以在通过外部删除或者保存。

#	内衣厂展示许多商品
需求：假设有个内衣工厂，目前的产品有50种男士内衣与50种女士内衣，为了推销产品，工厂决定生产一些塑料模特来穿上它们的内衣拍成内衣广告。这里很显然适合使用 <span style="color: red;">享元模式</span> 来组织代码。
```
// 定义塑料模特的构造函数
var Model = function (sex) {
	this.sex = sex;
}
// 为模特拍照
Model.prototype.takePhoto = function () {
	console.log('sex=' + this.sex + 'underwear=' + this.underwear )
}

// 实例化一个男模特 和 一个女模特
var maleModel = new Model('male'),
	female    = new Model('female');    
for (var i = 1; i <=50; i++){
	// 分别为模特换上 50 件内衣 以及 照相
	maleModel.underwear = 'underwear' + i;
	maleModel.takePhoto();
}
for (var i = 1; i <=50; i++){
	// 分别为模特换上 50 件内衣 以及 照相
	female.underwear = 'underwear' + i;
	femaleModel.takePhoto();
}
```
PS：很显然，这里只需要两个对象便完成这个需求。

#	地图应用（对象池）
说明：对象池维护一个装载空闲对象的池子，如果需要对象的时候，不是直接new，而是转从对象池里获取对象。如果对象池里没有空闲对象则创建一个新的对象，当获取的对象完成他的职责之后，再进入池子等待被下次获取。
需求：假设我们在开发一个地图应用，地图上经常会出现一些标志地名的小气泡，我们叫它toolTip 。当第一次搜索A地点时假设出现了2个小气泡，第二次搜索A附近的B地点的时候页面出现了6个小气泡，按照对象池的思想，在第二次搜索开始之前，并不会把第一次创建的2个小气泡删除掉，而是把它们放进对象池。这样第二次搜索结果页面里，我们只需要再创建4个小气泡而不是6个。
##	定义个气泡工厂
```
var toolTipFactory = (function () {
	var toolTipPool  = [];   // toolTip 对象池
	return {
		create : function () {
			if(toolTipPool.length === 0){
			   var div = document.createElement('div');
			   document.body.appendChild(div);
			   return div;
			} else{
			   return toolTipPool.shift(); //shift a dom
			}
		},
		recover : function (tooltipDom) {
			return toolTipPool.push(tooltipDom);  
		}
	};
})();
```
PS：气泡工厂的对象池为数组是私有属性被包含在工厂闭包里，这个工厂又两个暴露对外的方法，create表示获取一个div节点，recover表示回收一个div节点。
##	第一次搜索（2个气泡）
```
var ary = [];
for(var i = 0, str; str = ['A','B'][i++]){  // get ary elem
	var toolTip = toolTipFactory.create();
	toolTip.innerHTML = str;
	ary.push(toolTip)
};
```
PS：现在重新回到第一次搜索的时刻，目前需要创建2个小气泡节点，为了方便回收，用一个数组ary来记录它们。
##	现在开始回收节点
```
for(var i = 0 , toolTip ; toolTip = ary[i++]){
	toolTip.recover(toolTip)
}
```
##	第二次搜索（6个气泡）
```
for(var i =0 , str; str = ['A','B','C','D','E','F'][i++]; ){
	var toolTip = toolTipFactory.create();
	toolTip.innerHTML(str);
}
```
PS：现在页面中已经出现了6个节点，上一次创建好的节点被共享给了下一次操作。

>	参考文档：
[三分钟教会你JS设计模式之享元模式](http://www.jianshu.com/p/a158478c95b5)
[深入理解JavaScript系列（37）：设计模式之享元模式](http://www.cnblogs.com/TomXu/archive/2012/04/09/2379774.html)