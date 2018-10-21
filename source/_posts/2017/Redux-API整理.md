---
title: Redux-API整理
date: 2017-10-31 17:29:04
tags: 'Redux'
categories: 'React'
copyright: true
---
Redux 定义了一系列的约定（contract）来让你来实现（例如 reducers），同时提供少量辅助函数来把这些约定整合到一起。
Redux 只关心如何管理 state。在实际的项目中，你还需要使用 UI 绑定库如 react-redux。

#	redux相关API
##	创建 store
createStore(reducers, [preloadedState], enhancer)
##	合并 reducers 函数
该函数返回结果将作为 createStore 的第一个参数。
随着应用变得越来越复杂，可以考虑将 reducer 函数 拆分成多个单独的函数，拆分后的每个函数负责独立管理 state 的一部分。
combineReducers(reducers)
##	添加中间件
该函数返回结果将作为 createStore 的第三个参数。
我们可以在 action - reducer 中间加入，中间件如 redux-saga 等。
applyMiddleware(...middlewares)
##	other
bindActionCreators(actionCreators, dispatch)
compose(...functions)
##	Store API
getState() // 返回应用当前的 state 树。
dispatch(action) // 分发 action。这是触发 state 变化的惟一途径。
subscribe(listener) // 添加一个变化监听器。每当 dispatch action 的时候就会执行，state 树中的一部分可能已经变化时触发监听函数。
replaceReducer(nextReducer) // 替换 store 当前用来计算 state 的 reducer。
#	React-redux
作用：Redux 官方提供的 React 绑定库。
本库深受分离容器组件和展示组件思想启发。
在应用中，只有最顶层组件是对 Redux 可知（例如路由处理）这是很好的。
所有它们的子组件都应该是“笨拙”的，并且是通过 props 获取数据。
对比 容器组件 和 展示组件

| 组件类型      | 容器组件       | 展示组件  |
| ------------- |:-------------:|:---------:|
| 位置    | 最顶层，路由处理      | 中间和子组件   |
| 使用 Redux    | 是       | 否   |
| 读取数据    | 从 Redux 获取 state     | 从 props 获取数据   |
| 修改数据      | 向 Redux 发起 actions      | 从 props 调用回调函数   |  
	    
##	Provider 组件
作用：
Provider 使组件层级中的 connect() 方法都能够获得 Redux store。
正常情况下，你的根组件应该嵌套在 Provider 中才能使用 connect() 方法。
属性：
store (Redux Store): 应用程序中唯一的 Redux store 对象
children (ReactElement) 组件层级的根组件。
```
ReactDOM.render(
  <Provider store={store}>
	<Router history={history}>...</Router>
  </Provider>,
  targetEl
);
```
###	connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
作用：
	连接 React 组件与 Redux store。
	连接操作不会改变原来的组件类，反而返回一个新的已与 Redux store 连接的组件类。
参数：
1.[mapStateToProps(state, [ownProps]): stateProps] (Function): 
	如果定义该参数，组件将会监听 Redux store 的变化。
2.[mapDispatchToProps(dispatch, [ownProps]): dispatchProps] (Object or Function): 
	如果传递的是一个对象，那么每个定义在该对象的函数都将被当作 Redux action creator，
	而且这个对象会与 Redux store 绑定在一起，其中所定义的方法名将作为属性名，合并到组件的 props 中。
3.[mergeProps(stateProps, dispatchProps, ownProps): props] (Function):
	如果指定了这个参数，mapStateToProps() 与 mapDispatchToProps() 的执行结果和组件自身的
	props 将传入到这个回调函数中。该回调函数返回的对象将作为 props 传递到被包装的组件中。
4.[options] (Object) 如果指定这个参数，可以定制 connector 的行为。

返回值：
	根据配置信息，返回一个注入了 state 和 action creator 的 React 组件。
静态属性：
	WrappedComponent (Component): 传递到 connect() 函数的原始组件类。
静态方法：
	组件原来的静态方法都被提升到被包装的 React 组件。
实例方法：
	getWrappedInstance(): ReactComponent
	仅当 connect() 函数的第四个参数 options 设置了 { withRef: true } 才返回被包装的组件实例。

>	参考文档：
[redux 官方文档](https://github.com/react-guide/redux-tutorial-cn)
[redux](http://www.redux.org.cn/docs/api/createStore.html)
	