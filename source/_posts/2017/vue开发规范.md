---
title: vue开发规范
date: 2017-10-20 21:44:22
tags: ['vue']
categories: 'vue'
copyright: true
---
##	组件名为多个单词
	组件名应该始终是多个单词的，根组件 App 除外。
	```
	Vue.component('todo-item', { //  自动将组件的 name 设置为 todo-item
	  // ...
	})
	export default {
	  name: 'TodoItem', // 最佳实践为组件带上 name 调试有好处 
	  // ...
	}
	```
##	组件数据
	组件的 data 必须是一个函数。
	每次返回一个新的纯对象。
##	Prop定义
	Prop 定义应该尽量详细（至少为其指定 type ）。
	```
	props: {
	  status: {
		type: String,
		required: true,
		validator: function (value) {
		  return [
			'syncing',
			'synced',
			'version-conflict',
			'error'
		  ].indexOf(value) !== -1
		}
	  }
	}
	```
##	为 v-for 设置键值
	总是用 key 配合 v-for。
##	为组件样式设置作用域
	下面对于单文件组件。
	对于应用来说，顶级 App 组件和布局组件中的样式可以是全局的，
	但是其它所有组件都应该是有作用域的（3种方式）。
1.	scoped 
	<style scoped></style>	
2.	css Modules
	```
	<button :class="[$style.button]">X</button>
	<style module>
	.button {
	  border: none;
	  border-radius: 2px;
	}
	.buttonClose {
	  background-color: red;
	}
	</style>
	```
3.	BEM 约定
	```
	<button class="c-Button c-Button--close">X</button>
	<style>
	.c-Button {
	  border: none;
	  border-radius: 2px;
	}
	</style>	
	```
4.	私有属性名 必要
	在插件、混入等扩展中始终为自定义的私有属性使用 $_ 前缀。
	并附带一个命名空间以回避和其它作者的冲突 (比如 $_yourPluginName_)。
	```
	var myGreatMixin = {
	  // ...
	  methods: {
		$_myGreatMixin_update: function () {
		  // ...
		}
	  }
	}
	```
##	组件文件 
	只要有能够拼接文件的构建系统，就把每个组件单独分成文件。
	components/
	|- TodoList.vue
	|- TodoItem.vue
##	单文件组件文件的大小写（大驼峰）
	单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)。
##	基础组件名
	应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，
	比如 Base、App 或 V。
	components/
	|- BaseButton.vue
	|- BaseTable.vue
	|- BaseIcon.vue
##	单例组件名 
	只应该拥有单个活跃实例的组件应该以 The 前缀命名，以示其唯一性。
	components/
	|- TheHeading.vue
	|- TheSidebar.vue
##	紧密耦合的组件名 
	和父组件紧密耦合的子组件应该以父组件名作为前缀命名。
	components/
	|- TodoList.vue
	|- TodoListItem.vue
	|- TodoListItemButton.vue
##	组件名中的单词顺序 
	组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。
	components/
	|- SearchButtonClear.vue
	|- SearchButtonRun.vue
	|- SearchInputQuery.vue
	|- SearchInputExcludeGlob.vue
	|- SettingsCheckboxTerms.vue
	|- SettingsCheckboxLaunchOnStartup.vue
##	自闭合组件
	在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的——但在 DOM 模板里永远不要这样做。	
	```
	<!-- 在单文件组件、字符串模板和 JSX 中 -->
	<MyComponent/>
	<!-- 在 DOM 模板中 -->
	<my-component></my-component>
	```
##	模板中的组件名大小写
	对于绝大多数项目来说，在单文件组件和字符串模板中组件名应该总是 PascalCase 的——但是在 DOM 模板中总是 kebab-case 的。
	<!-- 在所有地方 -->
	<my-component></my-component>
##	JS/JSX 中的组件名大小写
	JS/JSX 中的组件名应该始终是 PascalCase 的，尽管在较为简单的应用中只使用 Vue.component 进行全局组件注册时，
	可以使用 kebab-case 字符串。
	```
	Vue.component('MyComponent', {
	  // ...
	})
	Vue.component('my-component', {
	  // ...
	})
	import MyComponent from './MyComponent.vue'
	export default {
	  name: 'MyComponent',
	  // ...
	}
	```
##	完整单词的组件名
	组件名应该倾向于完整单词而不是缩写。
	components/
	|- StudentDashboardSettings.vue 学生面板设置
	|- UserProfileOptions.vue
##	Prop 名大小写
	在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。
	```
	props: {
	  greetingText: String
	}
	// 在 html 中
	<WelcomeMessage greeting-text="hi"/>
	```
##	多个特性的元素
	多个特性的元素应该分多行撰写，每个特性一行。
	```
	<img
	  src="https://vuejs.org/images/logo.png"
	  alt="Vue Logo"
	>
	```
##	模板中简单的表达式
	组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。
##	简单的计算属性
	应该把复杂计算属性分割为尽可能多的更简单的属性。
##	带引号的特性值（双引号）
	非空 HTML 特性值应该始终带引号 (单引号或双引号，选你 JS 里不用的那个)。
##	指令缩写 
	指令缩写 (用 : 表示 v-bind: 和用 @ 表示 v-on:) 应该要么都用要么都不用。
##	组件/实例的选项的顺序
	组件/实例的选项应该有统一的顺序。
	副作用 (触发组件外的影响)
		el
	全局感知 (要求组件以外的知识)
		name
		parent
	组件类型 (更改组件的类型)
		functional
	模板修改器 (改变模板的编译方式)
		delimiters
		comments
	模板依赖 (模板内使用的资源)
		components
		directives
		filters
	组合 (向选项里合并属性)
		extends
		mixins
	接口 (组件的接口)
		inheritAttrs
		model
		props/propsData
	本地状态 (本地的响应式属性)
		data
		computed
	事件 (通过响应式事件触发的回调)
		watch
	生命周期钩子 (按照它们被调用的顺序)
	非响应式的属性 (不依赖响应系统的实例属性)
		methods
	渲染 (组件输出的声明式描述)
		template/render
		renderError
##	元素特性的顺序
	元素 (包括组件) 的特性应该有统一的顺序。	
	定义 (提供组件的选项)
		is
	列表渲染 (创建多个变化的相同元素)
		v-for
	条件渲染 (元素是否渲染/显示)
		v-if
		v-else-if
		v-else
		v-show
		v-cloak
	渲染方式 (改变元素的渲染方式)
		v-pre
		v-once
	全局感知 (需要超越组件的知识)
		id
	唯一的特性 (需要唯一值的特性)
		ref
		key
		slot
	双向绑定 (把绑定和事件结合起来)
		v-model
	其它特性 (所有普通的绑定或未绑定的特性)
	事件 (组件事件监听器)
		v-on
	内容 (复写元素的内容)
		v-html
		v-text
##	组件/实例选项中的空行 
	你可能想在多个属性之间增加一个空行，特别是在这些选项一屏放不下，
	需要滚动才能都看到的时候。
##	单文件组件的顶级元素的顺序
	单文件组件应该总是让 template、script 和 style 标签的顺序保持一致。
	且 <style> 要放在最后，因为另外两个标签至少要有一个。
	<template>...</template>
	<script>/* ... */</script>
	<style>/* ... */</style>
##	没有在 v-if/v-if-else/v-else 中使用 key 谨慎使用
	如果一组 v-if + v-else 的元素类型相同，最好使用 key (比如两个 <div> 元素)。
	```
	<div v-if="error" key="search-status">
	  错误：{{ error }}
	</div>
	<div v-else key="search-results">
	  {{ results }}
	</div>
	```
##	scoped 中的元素选择器 谨慎使用
	元素选择器应该避免在 scoped 中出现。
##	隐性的父子组件通信 谨慎使用
	应该优先通过 prop 和事件进行父子组件之间的通信，而不是 this.$parent 或改变 prop。
	```
	Vue.component('TodoItem', {
	  props: {
		todo: {
		  type: Object,
		  required: true
		}
	  },
	  template: `
		<input
		  :value="todo.text"
		  @input="$emit('input', $event.target.value)"
		>
	  `
	})
	```
##	非 Flux 的全局状态管理 谨慎使用
	应该优先通过 Vuex 管理全局状态，而不是通过 this.$root 或一个全局事件总线。
	
>	参考文档：
	[vue代码指南](https://cn.vuejs.org/v2/style-guide)
	
	
	
	
	
	
	
	
	






