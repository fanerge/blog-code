---
title: vue vuex
date: 2017-07-12 21:57:30
category: "vue"
tags: [vue,element-ui]
---
## 今天来总结下，在项目中使用vuex，项目采用[easy-mock](https://easy-mock.com)模拟数据
*如有不正确的地方，请大家联系我，谢谢。*

### 首先在项目中添加vuex
1. 在store文件夹下面建立index.js,内容如下
	```
   import Vue from 'vue'
   import Vuex from 'vuex'
   import { login } from '@/api/http' // 用于登录的
   Vue.use(Vuex)
	const store = new Vuex.Store({
	  state: { // 保存全局state --- this.$store.state[key]-- 放在computed
		userInfo: null,
		token: ''
	  },
	  getters: { // 对state进行不修改操作，相当于数据库的查操作 --- this.$store.getter[key]-- 放在computed

	  },
	  mutations: { // 同步操作state --- this.$store.commit('func',payload)-- 放在methods
		login (state, payload) {
		  state.userInfo = payload
		},
		setToken (state, payload) {
		  state.token = payload
		}
	  },
	  actions: { // 异步操作state --- this.$store.dispatch('func',payload)-- 放在methods
		login ({ commit }, userInfo) {
		  return new Promise((resolve, reject) => {
			login(userInfo.username, userInfo.password)
			.then((res) => {
			  commit('login', res.userInfo) // 全局state中保存用户信息
			  commit('setToken', res.token) // 全局state中保存token
			  resolve(res) // 这里很关键，必须要传递res参数，登录之后的其他操作
			})
			.catch((err) => {
			  reject(err)
			})
		  })
		}
	  }
	})

	export default store
	```
2. 在main.js中添加
	```
	import store from './store'
	new Vue({
	  el: '#app',
	  router,
	  store,
	  template: '<App/>',
	  components: {App}
	})
	```
3. 在api文件夹中http.js文件中
	```
	import http from './axios'
	export function login (username, password) {
	  return http.post('user/login', {
		username,
		password
	  })
	}
	```
4. 在登录组件中login.vue	
	```
	methods: {
      login (data) {
        this.$store.dispatch('login', data)
        .then((res) => {
          console.dir(res)
        })
        .catch((err) => {
          console.error(err)
        })
      }
    }
	```










