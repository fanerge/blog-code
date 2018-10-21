---
title: vue官网-全局与实例解读
date: 2017-10-18 22:21:49
category: "vue"
tags: ['js', 'vue']
copyright: true
---
#	全局API
##	Vue.extend( options )
	用法：使用基础 Vue 构造器，创建一个“子类”。	
	```
	// 创建构造器
	var Profile = Vue.extend({
	  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
	  data: function () {
		return {
		  firstName: 'Walter',
		  lastName: 'White',
		  alias: 'Heisenberg'
		}
	  }
	});
	// 创建 Profile 实例，并挂载到一个元素上。
	new Profile().$mount('#mount-point')
	```
##	Vue.nextTick( [callback, context] )
	用法：在下次 DOM 更新循环结束之后执行延迟回调。
	```
	// 修改数据
	vm.msg = 'Hello'
	// DOM 还没有更新
	Vue.nextTick(function () {
	  // DOM 更新之后进行相应的操作
	});
	```
##	Vue.set( target, key, value )
	用法：设置对象的属性。
##	Vue.delete( target, key )
	用法：删除对象的属性。
##	Vue.directive( id, [definition] )
	用法：注册或获取全局指令。
	如自定义指令，v-focus
##	Vue.filter( id, [definition] )
	用法：注册或获取全局过滤器。
##	Vue.component( id, [definition] )
	用法：注册或获取全局组件。
##	Vue.use( plugin )
	用法：安装 Vue.js 插件。
##	Vue.mixin( mixin )
	用法：全局注册一个混合，影响注册之后所有创建的每个 Vue 实例。
##	Vue.compile( template )
	用法：在 render 函数中编译模板字符串。
##	Vue.version
	用法：提供字符串形式的 Vue 安装版本号。
		
#	实例
	以下实例均为 vm。
	```
	var vm = new ({
		el: '#app'
	});
	```
##	实例属性
###	vm.$data 
	作用：Vue 实例代理了对其 data 对象属性的访问。
###	vm.$props
	作用：Vue 实例代理了对其 props 对象属性的访问。
###	vm.$el（只读）
	作用：Vue 实例使用的根 DOM 元素。
###	vm.$options（只读）
	作用：用于当前 Vue 实例的初始化选项。需要在选项中包含自定义属性时会有用处。
###	vm.$parent（只读）
	作用：父实例，如果当前实例有的话。
###	vm.$root（只读）
	作用：当前组件树的根 Vue 实例。如果当前实例没有父实例，此实例将会是其自己。
###	vm.$children（只读）
	作用：当前实例的直接子组件。
###	vm.$slots（只读）
	作用：用来访问被插槽分发的内容。每个具名插槽 有其相应的属性 (例如：slot="foo" 中的内容将会在 vm.$slots.foo 中被找到)。
###	vm.$scopedSlots（只读）
	作用：用来访问作用域插槽。
###	vm.$refs（只读）
	作用：一个对象，持有已注册过 ref 的所有子组件。
###	vm.$isServer（只读）
	作用：当前 Vue 实例是否运行于服务器。
###	vm.$attrs（只读）
	作用：包含了父作用域中不被认为 (且不预期为) props 的特性绑定 (class 和 style 除外)。
###	vm.$listeners
	作用：包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。
##	实例方法/数据
###	vm.$watch(expOrFn, callback, [options]);
	作用：观察 Vue 实例变化的一个表达式或计算属性函数。
	返回值：{Function} unwatch
		vm.$watch 返回一个取消观察函数，用来停止触发回调：
###	vm.$set( target, key, value )
	作用：这是全局 Vue.set 的别名。
###	vm.$delete( target, key )
	作用：这是全局 Vue.delete 的别名。
##	实例方法/事件
###	vm.$on(event, callback);	
	作用：监听当前实例上的自定义事件。
###	vm.$once(event, callback);
	作用：监听一个自定义事件，但是只触发一次，在第一次触发之后移除监听器。
###	vm.$off( [event, callback] )
	作用：移除自定义事件监听器。
###	vm.$emit(event, [...args]);	
	作用：触发当前实例上的事件。附加参数都会传给监听器回调。
##	实例方法/生命周期
###	vm.$mount( [elementOrSelector] )
	作用：如果 Vue 实例在实例化时没有收到 el 选项，则它处于“未挂载”状态，没有关联的 DOM 元素。
###	vm.$forceUpdate()
	作用：迫使 Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。
###	vm.$nextTick( [callback] )
	作用：将回调延迟到下次 DOM 更新循环之后执行。
###	vm.$destroy()
	作用：完全销毁一个实例。
#	指令（内置）
##	v-text
	作用：更新元素的 textContent。
		还可以使用 {{ Mustache }}
##	v-html
	作用：更新元素的 innerHTML 。
##	v-show
	作用：根据表达式之真假值，切换元素的 display CSS 属性。
		当条件变化时该指令触发过渡效果。
##	v-if
	作用：根据表达式的值的真假条件渲染元素。
		在切换时元素及它的数据绑定 / 组件被销毁并重建。
##	v-else
	作用：否则，前一兄弟元素必须有 v-if 或 v-else-if。
##	v-else-if
	作用：表示 v-if 的 “else if 块”。
##	v-for
	作用：基于源数据多次渲染元素或模板块。
	```
	<!--数组-->
	<div v-for="(item, index) in items" :key="item.id"></div>
	<!--对象-->
	<div v-for="(val, key, index) in object" :key="item.id"></div>
	```
##	v-on（@）
	作用：绑定事件监听器。
	事件修饰符：.stop, .prevent, .capture, .self, {keyCode | keyAlias}
		.native, .once, .left, .right, .middle, .passive	
##	v-bind（:）
	作用：绑定属性。
	属性修饰符：.prop, .camel, .sync（会扩展成一个更新父组件绑定值的 v-on 侦听器）	
##	v-model
	作用：表单和数据的双向绑定。
	表单修饰符：.lazy, .number, .trim
##	v-pre
	作用：跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。
##	v-cloak
	作用：这个指令保持在元素上直到关联实例结束编译。	
		需配合 CSS 规则 [v-cloak] { display: none } 一起使用。
##	v-once	
	作用：只渲染元素和组件一次。
#	特殊属性
##	key
	作用：性能考虑。
	使用场景：v-for, transition-group 过渡组件。
##	refs
	ref 被用来给元素或子组件注册引用信息。
##	slot
	作用：用于标记往哪个具名插槽中插入子组件内容。
##	slot-scoped
	作用：用于将元素或组件表示为作用域插槽。
##	is
	作用：用于动态组件且基于 DOM 内模板的限制来工作。
	```
	<!-- component changes when currentView changes -->
	<component v-bind:is="currentView"></component>
	<!-- necessary because `<my-row>` would be invalid inside -->
	<!-- a `<table>` element and so would be hoisted out      -->
	<table>
	  <tr is="my-row"></tr>
	</table>
	```
#	内置的组件
##	component
	用法：渲染一个“元组件”为动态组件。依 is 的值，来决定哪个组件被渲染。
	props: is, inline-template
	```
	<!-- 动态组件由 vm 实例的属性值 `componentId` 控制 -->
	<component :is="componentId"></component>
	<!-- 也能够渲染注册过的组件或 prop 传入的组件 -->
	<component :is="$options.components.child"></component>
	```
##	transition
	用法：<transition> 元素作为单个元素/组件的过渡效果。
	props：...
	事件：...
##	transition-group
	用法：<transition-group> 元素作为多个元素/组件的过渡效果。
	props: tag, move-class, mode
	事件：...
##	keep-alive
	props：include, exclude	
	用法：<keep-alive> 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。
##	slot
	用法：<slot> 元素作为组件模板之中的内容分发插槽。<slot> 元素自身将被替换。
	props：name
	
>	参考文档：
	[官方api](https://cn.vuejs.org/v2/api)















