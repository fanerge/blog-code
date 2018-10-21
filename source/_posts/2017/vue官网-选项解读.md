---
title: vue官网-选项解读
date: 2017-10-18 21:20:15
category: "vue"
tags: ['js', 'vue']
copyright: true
---

##	选项/数据
	```
	var app = new Vue({
		data() {            // 声明所有的根级响应式属性
			return {};
		},
		props: {},          // 父组件传递过来的属性
		propsData: {},      // 只能用于 new 创建的实例时传递 props。主要作用是方便测试
		methods: {},		// 组件相关的方法
		computed: {},		// 组件的计算属性
		watch: {},			// 监听组件的 data 中数据变化执行对应函数	
	});
	```
###	data	
	类型：Object | Function（对于组件只是使用这种方式，并且返回一个纯对象）
	作用：Vue 实例的数据对象。
	最佳实践：data中需要声明所有的根级响应式属性。
		有些时候想为 data 添加新数据。Vue.set( target, key, value )；target 可以是Object 和 Array(多个VUE实例对象)， key 为对应数据的键，value 为对应数据的值。
		对应还有 Vue.delete( target, key ) 移除VUE实例对象指定key的属性。
	数据驱动的原理：Vue 将会递归将 data 的属性转换为 getter/setter，从而让 data 的属性能够响应数据变化。
		属性通过 Object.defineProperty(obj, prop, descriptor) 来实现数据驱动。
###	props
	类型：Array<string> | Object
	作用：用于接收来自父组件的数据。对象允许配置高级选项，如是否必填（required）、
	类型检测（type）、自定义校验（validator函数）和设置默认值（default）。
	```
	// 对象语法，提供校验
	Vue.component('props-demo-advanced', {
	  props: {
		// 检测类型
		height: Number,
		// 检测类型 + 其他验证
		age: {
		  type: Number,
		  default: 0,
		  required: true,
		  validator: function (value) {
			return value >= 0
		  }
		}
	  }
	});
	```
###	propsData
	类型：{ [key: string]: any }
	作用：只能用于 new 创建的实例时传递 props。主要作用是方便测试。
	```
	var vm = new Vue({
	  propsData: {
		msg: 'hello'
	  }
	});
	```
###	computed
	类型：{ [key: string]: Function | { get: Function, set: Function } }
	作用：计算属性的结果会被缓存，除非依赖的响应式属性（data中的数据）变化才会重新计算。
	```
	var vm = new Vue({
	  data: { a: 1 },
	  computed: {
		// 仅读取
		aDouble: function () {
		  return this.a * 2
		},
	  },
	});
	
	// 组件类使用
	this.aDouble
	// 组件外使用
	vm.aDouble
	```
###	methods
	类型：{ [key: string]: Function }
	作用：为组件定义方法
	```
	var vm = new Vue({
	  data: { a: 1 },
	  methods: {
		plus: function () {
		  this.a++
		}
	  }
	});
	
	// 组件内使用
	this.plus()
	// 组件外使用
	vm.plus()
	```
###	watch
	类型：{ [key: string]: string | Function | Object }
	作用：键是需要观察的表达式data中的属性，值是对应回调函数。值也可以是方法名，或者包含选项的对象
	```
	var vm = new Vue({
	  data: {
		a: 1,
		b: 2,
		c: 3
	  },
	  watch: {
		a: function (val, oldVal) {
		  console.log('new: %s, old: %s', val, oldVal)
		},
		// 方法名
		b: 'someMethod',
		// 深度 watcher
		c: {
		  handler: function (val, oldVal) { /* ... */ },
		  deep: true
		}
	  }
	});
	```
	
##	选项/DOM
	```
	new Vue({
	  el: '#app',
	  template: '<div>我是模板</div>',
	  render (h) {
		throw new Error('oops')
	  },
	  renderError (h, err) {
		return h('pre', { style: { color: 'red' }}, err.stack)
	  }
	});
	```
###	el
	类型：string | HTMLElement
	作用：提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标（只在由 new 创建的实例中遵守）。
###	template
	类型：string	
	作用：一个字符串模板作为 Vue 实例的标识使用。
###	render
	类型：(createElement: () => VNode) => VNode
	作用：字符串模板的代替方案，允许你发挥 JavaScript 最大的编程能力。
###	renderError
	类型：(createElement: () => VNode, error: Error) => VNode
	作用：当 render 函数遭遇错误时，提供另外一种渲染输出。
	
##	选项/生命周期钩子（函数）
	在组件的具体某个过程触发相应的函数
	可以理解为：创建 -> 挂载 -> 更新 -> 激活 -> 销毁
	```
	new Vue({
		beforeCreate() {}, // 实例初始化之后
		created() {},      // 在实例创建完成后被立即调用
		beforeMount() {},  // 在挂载开始之前被调用
		mounted() {},      // 实例挂载到 DOM 节点
		beforeUpdate() {}, // 数据更新时调用
		updated() {},      // 数据更新完成后调用
		activated() {},    // keep-alive 组件激活时调用
		deactivated() {},  // keep-alive 组件停用时调用
		beforeDestory() {},// 实例销毁之前调用
		destoryed() {}     // 实例销毁后调用。
	});
	```
	详细信息请看官网-生命周期图示
![生命周期图示](https://cn.vuejs.org/images/lifecycle.png)
###	beforeCreate
	执行时机：在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。
###	created
	执行时机：在实例创建完成后被立即调用。完成以下任务：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调，但实例并没有挂载到真实节点。
###	beforeMount
	执行时机：在挂载开始之前被调用：相关的 render 函数首次被调用。
###	mounted
	执行时机：当实例挂载到文档 DOM 元素时触发。
	注意 mounted 不会保证所有的子组件也都一起被挂载，下面可以保证所有子组件都一起被挂起。
	```
	mounted: function () {
	  this.$nextTick(function () {
		// 整个视图页面全部渲染时触发，类似于jquery的ready方法。
	  });
	}
	```
###	beforeUpdate
	执行时机：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。
###	updated
	执行时机：由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
		此时可以进行 DOM 操作。
###	activated
	执行时机：keep-alive 组件激活时调用。
###	deactivated
	执行时机：keep-alive 组件停用时调用。	
###	beforeDestory
	执行时机：实例销毁之前调用。在这一步，实例仍然完全可用。
###	destoryed
	执行时机：Vue 实例销毁后调用。
	
##	选项/资源
	```
	var vm = new Vue({
		el: '#app',
		directives: {}, // 注册局部指令
		filters: {},    // 注册局部过滤器
		compoents: {}   // 注册局部组件 
	});
	```
###	directives
	类型：Object
	1.注册自定义指令（全局）
	```
	Vue.directive('focus', {
	  // 当绑定元素插入到 DOM 中。
	  inserted: function (el) {
	    // 聚焦元素
	    el.focus()
	  }
	});
	var vm = new Vue({
		el: '#app',
		// directives: {} 注册局部指令
	});
	```
	这里指令的钩子函数：bind、inserted、update、componentUpdated、unbind
	每个钩子函数的参数：(包括 el，binding，vnode，oldVnode)。
	2.使用自定义指令
	```
	<div id="app">
		<input v-focus>
	</div>
	```
###	filter
	类型：Object
	1.注册全局过滤器
	```
	Vue.filter('add4', function (value) {
	return value + 4;
	  // 返回处理后的值
	});
	var vm = new Vue({
		el: '#app',
		data() {
			return {
				num: 10
			};
		},
		// filters: {} 注册局部过滤器
	});
	```
	2.使用过滤器
	```
	<div id="app">
		<p>10 + 4 = {{num | add4}}</p> // 14
	</div>
	```
###	compoents（局部组件的注册）
	类型：Object
	1.注册全局组件
	```
	Vue.component('my-component', {
	  // 选项
	});
	var vm = new Vue({
		el: '#app',
		data() {
			return {
				num: 10
			};
		},
		// components: {} 注册局部组件
	});
	```
	2.使用组件
	```
	<div id="app">
		<my-component />
	</div>
	```
	
##	选项/组合
	```
	import parentComponent from './path';
	var vm = new Vue({
		el: '#app',
		parent: parentComponent, // 定义当前组件的父组件
		minxins: [mixin],        // 定义局部混合逻辑（引入的先触发）
		extends: CompA           // 定义局部继承（自身的先触发）
	});
	```
###	parent
	类型：Vue instance
	作用：指定已创建的实例的父实例，在两者之间建立父子关系。
		子实例可以用 this.$parent 访问父实例，子实例被推入父实例的 $children 数组中。
###	minxins
	类型：Array<Object>
	作用：混合 (mixins) 是一种分发 Vue 组件中可复用功能的非常灵活的方式。
	混合对象可以包含任意组件选项。以组件使用混合对象时，所有混合对象的选项将被
	混入该组件本身的选项。
	1.创建局部混合
	```
	var mixin = {
	  created: function () {
		console.log('混合对象的钩子被调用')
	  }
	};
	```
	2.使用混合
	```
	new Vue({
	  mixins: [mixin], // 这里使用
	  created: function () {
		console.log('组件钩子被调用')
	  }
	});
	```
	这里需要说明：混合对象的 钩子将在组件自身钩子 之前 调用。 
###	extends
	类型：Object | Function
	作用：允许声明扩展另一个组件(可以是一个简单的选项对象或构造函数)，而无需使用 Vue.extend。这主要是为了便于扩展单文件组件。
		这和 mixins 类似，区别在于，组件自身的选项会比要扩展的源组件具有更高的优先级。
	```
	var CompA = { ... }
	// 在没有调用 `Vue.extend` 时候继承 CompA
	var CompB = {
	  extends: CompA,
	  ...
	}
	```
###	provide/inject
	provide：Object | () => Object
	inject：Array<string> | { [key: string]: string | Symbol }
	说明：provide 和 inject 主要为高阶插件/组件库提供用例。并不推荐直接用于应用程序代码中。

##	选项/其它
###	name
	类型：string	
	作用：只有作为组件选项时起作用，并为组件命名。
	最佳实践：为每个组件命名，利于 vue-devtools 调试。
###	delimiters
	类型：Array<string>
	作用：改变纯文本插入分隔符。
	```
	new Vue({
	  delimiters: ['${', '}']
	});
	// 分隔符变成了 ES6 模板字符串的风格
	```
###	functional
	类型：boolean
	作用：使组件无状态 (没有 data ) 和无实例 (没有 this 上下文)。
###	model
	类型：{ prop?: string, event?: string }
	作用：允许一个自定义组件在使用 v-model 时定制 prop 和 event。
###	inheritAttrs
	类型：boolean
	作用：默认情况下父作用域的不被认作 props 的特性绑定 (attribute bindings) 将会“回退”且作为普通的 HTML 特性应用在子组件的根元素上。
###	comments
	类型：boolean
	作用：当设为 true 时，将会保留且渲染模板中的 HTML 注释。默认行为是舍弃它们。
	
>	参考文档：
	[官方api](https://cn.vuejs.org/v2/api)















