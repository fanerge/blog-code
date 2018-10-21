---
title: Redux总结
date: 2017-10-29 19:53:24
tags: 'Redux'
categories: 'React'
copyright: true
---

#	Redux 三大原则
##	单一数据源
	整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。
##	State 是只读的
	惟一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。
##	使用纯函数来执行修改
	为了描述 action 如何改变 state tree ，你需要编写 reducers。
Immutable 是一个可实现持久数据结构的 JavaScript 库。

#	Redux 基本概念
##	Action
###	Action
	```
	const ADD_TODO = 'ADD_TODO'
	{
	  type: ADD_TODO,
	  text: 'Build my first Redux app'
	}
	```
	Action 是把数据从应用传到 store 的有效载荷。它是 store 数据的唯一来源。
	一般来说你会通过 store.dispatch() 将 action 传到 store。	
	Action 本质上是 JavaScript 普通对象。
	我们约定，action 内必须使用一个字符串类型的 type 字段来表示将要执行的动作。
###	Action 创建函数
	```
	function addTodo(text) {
	  return {
		type: ADD_TODO,
		text
	  }
	}
	```
	Action 创建函数 就是生成 action 的方法。“action” 和 “action 创建函数” 这两个概念很容易混在一起，使用时最好注意区分。
###	分发 Action（此时就新建了一条 todo）
	Redux 中只需把 action 创建函数的结果传给 dispatch() 方法即可发起一次 dispatch 过程。
	```
	dispatch(addTodo('新建的todo'))
	```
##	Reducer
	Action 只是描述了有事情发生了这一事实，并没有指明应用如何更新 state。
	而这正是 reducer 要做的事情。
###	设计 State 结构
	在 Redux 应用中，所有的 state 都被保存在一个单一对象中。
	```
	{
	  visibilityFilter: 'SHOW_ALL',
	  todos: [
		{
		  text: 'Consider using Redux',
		  completed: true,
		},
		{
		  text: 'Keep all state in a single tree',
		  completed: false
		}
	  ]
	}
	```
###	Action 处理（reducer 纯函数）
	reducer 模版（不再reducer中执行有副作用的操作，如 API 请求和路由跳转）
	```
	// 模版(previousState, action) => newState
	
	function todoApp(state = initialState, action) {
	  // 这里暂不处理任何 action，
	  // 仅返回传入的 state。
	  return state
	}
	```
###	处理多个 action	
	```
	function todoApp(state = initialState, action) {
	  switch (action.type) {
		case SET_VISIBILITY_FILTER:
		  return Object.assign({}, state, {
			visibilityFilter: action.filter
		  })
		case ADD_TODO:
		  return Object.assign({}, state, {
			todos: [
			  ...state.todos,
			  {
				text: action.text,
				completed: false
			  }
			]
		  })
		default:
		  return state
	  }
	}
	```
###	拆分 Reducer
	注意每个 reducer 只负责管理全局 state 中它负责的一部分。
	每个 reducer 的 state 参数都不同，分别对应它管理的那部分 state 数据。
##	Store
	总结，action 和 reducers。
	action 来描述“发生了什么”，和使用 reducers 来根据 action 更新 state 的用法。
	Store的作用：
	1.维持应用的 state
	2.提供 getState() 方法获取 state	
	3.提供 dispatch(action) 方法更新 state
	4.通过 subscribe(listener) 注册监听器
	5.通过 subscribe(listener) 返回的函数注销监听器
###	发起 Actions
	```
	// 订阅 state 更改
	// 注意 subscribe() 返回一个函数用来注销监听器
	let unsubscribe = store.subscribe(() =>
	  console.log(store.getState())
	)
	// 发起 action
	store.dispatch(reducers)
	```
##	数据流动
	Redux 应用中数据的生命周期
	1.调用 store.dispatch(action)
		你可以在任何地方调用 store.dispatch(action)，包括组件中、XHR 回调中、甚至定时器中。
	2.Redux store 调用传入的 reducer 函数
		Store 会把两个参数传入 reducer： 当前的 state 树和 action。
		let nextState = todoApp(previousState, action);
	3.根 reducer 应该把多个子 reducer 输出合并成一个单一的 state 树。	
		combineReducers() 来把根 reducer 拆分成多个函数，用于分别处理 state 树的一个分支。
	4.Redux store 保存了根 reducer 返回的完整 state 树。	

#	Redux 高级
##	异步 Action
	Action
		每个 API 请求都需要 dispatch 至少三种 action：	
		1.一种通知 reducer 请求开始的 action。	
		2.一种通知 reducer 请求成功结束的 action。	
		3.一种通知 reducer 请求失败的 action。	
	同步 Action Creator	
		```
		export const SELECT_SUBREDDIT = 'SELECT_SUBREDDIT'

		export function selectSubreddit(subreddit) {
		  return {
			type: SELECT_SUBREDDIT,
			subreddit
		  }
		}
		```
###	设计 state 结构
###	处理 Action（reducer）
		处理异步 action：redux-thunk、redux-promise、redux-promise-middleware
###	异步 Action Creator	
##	Middleware
	你可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等。		
	问题: 记录日志	
	问题: 崩溃报告	
##	搭配 React Router	
	Redux 和 React Router 将分别成为你数据和 URL 的事实来源。	
			
>	参考文档：
	[Redux 中文文档](http://www.redux.org.cn/docs/introduction/Motivation.html)