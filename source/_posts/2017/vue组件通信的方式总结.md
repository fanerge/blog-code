---
title: vue组件通信的方式总结
date: 2017-10-17 21:43:29
category: "vue"
tags: ['js', 'vue']
copyright: true
---
#	父子组件通信
	父组件通过 props 向子组件传递数据，子组件通过执行父组件的方法，通知父组件子组件所发生的变化。
	```
	// 父组件
	<one-address :addressitems="addressitems" @edit-address="editAddress"></one-address>
	
	// 子组件
	<div>{{ addressitems.partment }}{{ addressitems.address }}</div>
	export default {
	  props: {
		addressitems: Object
	  },
	  methods: {
		 editAddress () {
		  this.$emit('edit-address', false)
		 }
	  }
	}
	```
#	非父子组件通信
	非父子组件通信同样也可以用Vue.$emit自定义事件来解决
	```
	var bus = new Vue();
	// 组件A
	bus.$emit('id-selected', 1);
	// 组件B
	bus.$on('id-selected', function (id) {
	 console.log(id)
	});
	```
#	vue跨组件跨模块通信
	使用 vuex
	vuex有四个核心概念，其中state和getters主要是用于数据的存储与输出，
	而mutations和actions是用于提交事件并修改state中的数据。
	这里盗取vuex官网图，需要详细了解请访问[vuex](https://vuex.vuejs.org)
![vuex原理图](https://vuex.vuejs.org/zh-cn/images/vuex.png)
>	参考文档：
	[vue组件之间的多种通信方法](http://www.jianshu.com/p/a78277be91d0)
	[vue组件之间的通信（一）](http://whutzkj.space/2017/08/05/vue%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E7%9A%84%E9%80%9A%E4%BF%A1%EF%BC%88%E4%B8%80%EF%BC%89/#more)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	