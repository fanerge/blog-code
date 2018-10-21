---
title: vue admin
date: 2017-07-11 20:54:43
category: "vue"
tags: [vue,element-ui]
---
## 最近个人接触了一些后台管理系统的搭建，参考了很多资料，打算把自己的后台管理系统开发经验写成博客，可能会帮助到一些想做后台管理系统的朋友，更主要的记录整个开发思路，以及vue的一整套解决方案。
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### 后台管理系统主要功能(vue-cli搭建项目)
1. element-ui 饿了么出品的vue2.0 pc端 UI框架
2. axios vue官方推荐的请求库 支持Promise
3. html5的本地存储LocalStorage（用于记住密码等） 与 SessionStorage （用于存储个人信息和token等）
4. 浏览器样式兼容库 normalize.css 格式化css
5. 类似于YouTube的页面跳转精度条-nprogress 轻量的全局进度条控制
6. 官方提供的状态管理库 vuex
7. 官方提供路由 vue-router
8. 集成icon-font图标(symbol方式)
9. 添加全屏功能
（后续会继续添加）

### 先在项目中引入 [element-ui](http://element.eleme.io/#/zh-CN)
1. 安装 element-ui  npm i element-ui -S
2. 引入相关资源
	import ElementUI from 'element-ui'
	import 'element-ui/lib/theme-default/index.css'
	Vue.use(ElementUI)
	这样你就可以愉快的在你的项目中使用element的组件了。
3. 例如使用 Message 消息提示
	就可以在任何vue组件中 this.$message('这是一条消息提示'); // this指向vue实例

### 在项目中引入axios请求库[axios](https://www.axios.com/)
1. 安装 axios npm i axios -S   
2. 在使用axios的时候我们经常要进行一些封装和全局配置等	
    我一般开发时会在src目录下新建一个api的文件夹，里面存放对axios的封装（axios.js）和项目所有请求（http.js）
3. 这里重点讲一下axios.js这个文件夹，也就是对axios的封装以及拦截操作
	这里是全局配置
	```
	var service = axios.create({
	  baseURL:"https://some-domain.com/api/",
	  timeout:1000,
	  headers: {'X-Custom-Header':'foobar'}
	});
	```
	这里是request拦截操作
	```
	// request拦截器
	service.interceptors.request.use(config => {
	  if (store.getters.token) { // 判断全局状态中是否有token，也就是说判断你不是合法用户
		config.headers['token'] = store.getters.token; // 让每个请求携带自定义token，开心不用没个请求都带上token字段了
	  }
	  return config;
	}, error => {
	  Promise.reject(error);
	})
	```
	这里是response拦截操作
	```
	// respone拦截器
	service.interceptors.response.use(
	  response => {
		// code为非20000是抛错 可结合自己业务进行修改
		const res = response.data;
		if (res.code !== 20000) {
		  // 在该页面使用element组件，别忘了需要引入哦！！
		  Message({
			message: res.data,
			type: 'error',
			duration: 5 * 1000
		  });

		  // 50008:非法的token; 50012:其他客户端登录了;  50014:Token 过期了;
		  if (res.code === 50008 || res.code === 50012 || res.code === 50014) {
			// 这里需要你进行相应的操作处理
		  }
		  return Promise.reject(error);
		} else {
		  return response.data;
		}
	  },
	  error => {
		Message({
		  message: error.message,
		  type: 'error',
		  duration: 5 * 1000
		});
		return Promise.reject(error);
	  }
	)
	// 最后被忘了，要export 这个实例
	export default service;
	```
4. 阻止http.js请求文件
	里面的内容大致是这样的
	```
	import service from './axios'
	const ApiUrl = {
		login: 'login/', // 存放api路径
		...
	}
	export default {
		login () {
			// 这里后期我会发在vuex中的actions里面
			service.post(ApiUrl.login, {
			  username: 'fanerge',
			  password: 'XXXXXX'
			})
			.then((res) => {
				let resp = res.data
				console.log(resp)
			})
			.catch((err) => {
				console.error(err)
			})
		}
	}
	```
> 参考[vueAdmin-template](https://juejin.im/post/595b4d776fb9a06bbe7dba56)	
	
	
	
	
	
	
	
	
	
	
	
