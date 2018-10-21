---
title: vue组件开发方法总结
date: 2017-10-18 19:29:37
category: "vue"
tags: ['js', 'vue']
copyright: true
---
*VUE 组件的三种开发方式*
**	开发组件大致分为3个步骤： 组件声明-组件注册（全局和局部）-组件使用**
#	使用 script 标签
	1.组件声明
	```
	<!-- 注意：使用<script>标签时，type指定为text/x-template，意在告诉浏览器这不是一段js脚本，浏览器在解析HTML文档时会忽略<script>标签内定义的内容。-->	
	<!-- type 和 id 必须要填写 -->
	<script type="text/x-template" id="myComponent">
		<!--只能有一个根节点，下面两种方式同样遵循-->
        <div class="root">
			<p>我是p1</p>
			<p>我是p2</p>
		</div>
    </script>
	```
	2.组件注册（分为全局注册和局部注册）
	```
	全局注册：需要确保在根实例初始化之前注册，这样才能使组件在任意实例中都可以使用。
		Vue.component('my-component',MyComponent);//此句一定要放在new Vue({...});之前
	局部注册：限定了组件只能在被注册的组件中使用，而无法在其他组件中使用。
	//全局注册组件
	Vue.component('my-component',{
		template: '#myComponent'
	});
	```
	3.组件使用
	```
	<div id="app">
        <my-component></my-component>
    </div>
	```
	本例全部代码
	```
	<!DOCTYPE html>
	<html>
		<body>
			<div id="app">
				<!--3.组件使用-->
				<my-component></my-component>
			</div>
			
			<!--1.组件声明-->
			<script type="text/x-template" id="myComponent">
				<div>This is a component!</div>
			</script>
		</body>
		<script src="js/vue.js"></script>
		<script>
			<!--2.组件注册-->
			Vue.component('my-component',{
				template: '#myComponent'
			});

			new Vue({
				el: '#app'
			});
		</script>
	</html>
	```
#	使用 template 标签
	1.组件声明
	```
	<template id="myComponent">
        <div>This is a component!</div>
    </template>
	```
	2.组件注册
	```
	Vue.component('my-component',{
		template: '#myComponent'
	});
	```
	3.使用组件
	```
	<div id="app">
        <my-component></my-component>
    </div>
	```
	本例全部代码
	```
	<!DOCTYPE html>
	<html>
		<body>
			<div id="app">
				<!--3.组件使用-->
				<my-component></my-component>
			</div>
			
			<!--1.组件声明-->
			<template id="myComponent">
				<div>This is a component!</div>
			</template>
		</body>
		<script src="js/vue.js"></script>
		<script>
			<!--2.组件注册-->
			Vue.component('my-component',{
				template: '#myComponent'
			});

			new Vue({
				el: '#app'
			});

		</script>
	</html>
	```
#	单文件组件
	注：这种方法常用在vue单页应用中。
	1.创建组件（hello.vue）
	```
	<template>
	  <div class="hello">
		<h1>{{ msg }}</h1>
	  </div>
	</template>

	<script>
	export default {
	  name: 'hello',
	  data () {
		return {
		  msg: '欢迎！'
		}
	  }
	}
	</script>
	```
	父组件代码（app.vue，这里app.vue为hello.vue的父组件）
	```
	<!-- 展示模板 -->
	<template>
	  <div id="app">
		<!-- 3.使用组件 -->
		<hello></hello>
	  </div>
	</template>

	<script>
	// 导入组件
	import Hello from './components/hello'

	export default {
	  name: 'app',
	  // 2.这里进行组件局部注册
	  components: {
		Hello
	  }
	}
	</script>
	```
	
>	参考文档：
	[vue官网](https://cn.vuejs.org/)
	[vue组件的3种书写形式](http://blog.csdn.net/u012123026/article/details/72460470)
	[vue.js中组件的创建和使用方法](http://blog.csdn.net/u013910340/article/details/72763418)







