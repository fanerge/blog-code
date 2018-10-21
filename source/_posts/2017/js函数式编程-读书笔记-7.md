---
title: js函数式编程-读书笔记
date: 2017-08-11 20:59:55
category: "函数式编程"
tags: ['读书笔记','函数式编程']
---
最近，工作不是很忙，赶紧为自己充电。准备很长一段事件撸**函数式编程**。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

###	基于流的编程 or 无类编程
	
####	连接
	```
	function createPerson () {
		let firstName = '';
		let lastName = '';
		let age = 0;
		return {
			setFirstName: function (fn) {
				firstName = fn;
				return this;	
			},
			setLastName: function (fn) {
				lastName = fn;
				return this;	
			},
			setAge: function (fn) {
				age = fn;
				return this;
			},
			toString: function () {
				return [firstName, lastName, age].join(' ');
			}
		};
	}
	let dd = createPerson()
			.setFirstName('yu')
			.setLastName('fan')
			.setAge(11)
			.toString();
	console.log(dd) // yu fan 11
	```

####	管道
	pipeline(a, _.compact, _.initial, _.rest, rev) // 对数据a进行一系列的操作

####	数据流与控制流
	
###	无类编程

####	

#### 数据导向
	
#### Mixins
	```
	类层次结构
	function ContainerClass () {}
	function ObservedContainerClass () {}
	function HoleClass () {}
	function CASClass () {}
	function TableBaseClass () {}
	
	ObservedContainerClass.prototype = new ContainerClass();
	HoleClass.prototype = new ObservedContainerClass();
	CASClass.prototype = new HoleClass();
	TableBaseClass.prototype = new HoleClass();
	
	// 用Mixin扁平化层级结构
	function Container (val) {
		this._value = val;
		this.init(val);
	}
	Container.prototype.init = _.identity;
	let Hole = function (val) {
		Container.call(this, val);
	}
	let HoleMixin = {
		setValue: function (newValue) {
			let oldVal = this._value;
			this.validate(newValue);
			this._value = newValue;
			this.notify(oldVal, newValue);
			return this._value;
		},
	};
	let ObserverMixin = (function () {
		let _watchers = [];
		return {
			watch: function (fun) {
				_watchers.push(fun);
				return _.size(_watchers);
			},
			notify: function (oldVal, newVal) {
				_.each(_._watchers, function (watcher) {
					watcher.call(this, oldVal, newVal);
				});
				return _.size(_watchers);
			}
		};
	});
	let ValidateMixin = {
		addValidator: function (fun) {
			this._validator = fun;
		},
		init: function (val) {
			this.validate(val);
		},
		validate: function (val) {
			if (existy(this._validator) &&
				!this._validator(val)) {
				fail('Attrmpted to set invalid value' + polyToString(val));
			}

		}
	};
	_.extend(Hole.prototype, HoleMixin, ValidateMixin, ObserverMixin); // 同级继承
	let h = new Hole(43);
	h.addValidator(always(false));
	console.log(h); // 43
	
	let SwapMixin = {
		swap: function (fun) {
			let args = _.rest(arguemnts);
			let newValue = fun.apply(this, _.identity);
			return this.setValue(newValue);
		},
	};
	let o = {_value: 0, setValue: _.identity}
	_.extend(o, SwapMixin);
	o.swap(construct, [1, 2, 3]); //[0, 1, 2, 3]
	```
>	源代码仓库
	[函数式编程-读书笔记](https://github.com/fanerge/Functional-reading-notes)




































































	```
