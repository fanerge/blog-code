---
title: React-基础知识整理
date: 2018-07-17 20:17:11
tags: ['React']
categories: 'React'
copyright: true
---
#	setState 一定是异步的吗？
setState 只在合成事件和钩子函数中是“异步”的，在原生事件和 setTimeout 中都是“同步”的。
[你真的理解setState吗？](https://juejin.im/entry/5b45c6955188251abd7d14be)
#	Fiber
Fiber 本质上是一个虚拟的堆栈帧，新的调度器会按照优先级自由调度这些帧，从而将之前的同步渲染改成了异步渲染，在不影响体验的情况下去分段计算更新。
对于如何区别优先级，React 有自己的一套逻辑。对于动画这种实时性很高的东西，也就是 16 ms 必须渲染一次保证不卡顿的情况下，React 会每 16 ms（以内） 暂停一下更新，返回来继续渲染动画。
对于异步渲染，现在渲染有两个阶段：reconciliation 和 commit 。前者过程是可以打断的，后者不能暂停，会一直更新界面直到完成。
##	Reconciliation 阶段
componentWillMount
componentWillReceiveProps（使用 getDerivedStateFromProps 代替）
shouldComponentUpdate
componentWillUpdate
##	Commit 阶段
componentDidMount
componentDidUpdate
componentWillUnmount
[react](https://yuchengkai.cn/docs/zh/frontend/react.html)
#	生命周期函数
```
class ExampleComponent extends React.Component {
  // 用于初始化 state
  constructor() {}
  // 用于替换 `componentWillReceiveProps` ，该函数会在初始化和 `update` 时被调用
  // 因为该函数是静态函数，所以取不到 `this`
  // 如果需要对比 `prevProps` 需要单独在 `state` 中维护
  static getDerivedStateFromProps(nextProps, prevState) {}
  // 判断是否需要更新组件，多用于组件性能优化
  shouldComponentUpdate(nextProps, nextState) {}
  // 组件挂载后调用
  // 可以在该函数中进行请求或者订阅
  componentDidMount() {}
  // 用于获得最新的 DOM 数据
  getSnapshotBeforeUpdate() {}
  // 组件即将销毁
  // 可以在此处移除订阅，定时器等等
  componentWillUnmount() {}
  // 组件销毁后调用
  componentDidUnMount() {}
  // 组件更新后调用
  componentDidUpdate() {}
  // 渲染组件函数
  render() {}
  // 以下函数不建议使用
  componentWillMount() {}
  componentWillUpdate(nextProps, nextState) {}
  componentWillReceiveProps(nextProps) {}
}
```	
##	首次渲染触发的生命周期
constructor()
UNSAFE_componentWillMount()
render()
componentDidMount()
##	非初次渲染触发的生命周期
UNSAFE_componentWillReceiveProps(nextProps) 
shouldComponentUpdate(nextProps, nextState)
UNSAFE_componentWillUpdate()
render()
componentDidUpdate()
##	其他什么周期函数
componentWillUnmount()
static getDerivedStateFromProps(nextProps, prevState) // 代替 componentWillReceiveProps 在初始化和 update 时触发，由于时静态方法无 this
getSnapshotBeforeUpdate(prevProps, prevState) // 此生命周期的返回值将作为第三个参数传递给componentDidUpdate。 （这个生命周期不是经常需要的，但可以用于在恢复期间手动保存滚动位置的情况。）与componentDidUpdate一起，这个新的生命周期将覆盖旧版componentWillUpdate的所有用例。

#	React16.0中的portal
作用：将子节点插入到父节点之外的dom（render到一个组件里面去，实际改变的是网页上另一处的DOM结构。）。
使用场景：Modal、Dialog、Message等全局提示组件
比如写一个Dialog通用组件
```
import React from 'react';
import {createPortal} from 'react-dom';

class Dialog extends React.Component {
  constructor() {
    super(...arguments);

    const doc = window.document;
    this.node = doc.createElement('div');
    doc.body.appendChild(this.node);
  }

  render() {
    return createPortal(
      <div class="dialog">
        {this.props.children}
      </div>, //塞进传送门的JSX
      this.node //传送门的另一端DOM node
    );
  }

  componentWillUnmount() {
    window.document.body.removeChild(this.node);
  }
}
```
[React-DOM Portal](https://zhuanlan.zhihu.com/p/29880992?utm_source=wechat_session&utm_medium=social&from=singlemessage)
#	React16新特性
##	Error Boundary（错误边界）
之前，一旦某个组件发生错误，整个组件树将会从根节点被unmount下来。
Error Boundary可以看作是一种特殊的React组件，新增了componentDidCatch这个生命周期函数，它可以捕获自身及子树上的错误并对错误做优雅处理，包括上报错误日志、展示出错提示，而不是卸载整个组件树。（注：它并不能捕获runtime所有的错误，比如组件回调事件里的错误，可以把它想象成传统的try-catch语句）
PS：最佳实践封装通用错误组件，用起包裹可能出错的组件，来捕获子组件可能发生的错误。
##	render方法新增返回类型
render方法支持直接返回string，number，boolean，null，portal，以及fragments(带有key属性的数组)，这可以在一定程度上减少页面的DOM层级。
##	支持自定义DOM属性
现在React可以将属性直接传递给DOM，不过有些写法任然无效。
##	setState传入null时不会再触发更新
比如在一个选择城市的函数中，当点击某个城市时，newValue的值可能发生改变，也可能是点击了原来的城市，值没有变化，返回null则可以直接避免触发更新，不会引起重复渲染，不需要在shouldComponentUpdate函数里面去判断。
[十分钟快速了解React16新特性](https://www.jianshu.com/p/af0ae26eac18)

#	React编码优化
1.	使用 key 属性为列表项组件做身份标记
2.	shouldComponentUpdate 决定组件是否render
3.	PureComponent 来定义组件
4.	Stateless components 函数式组件
5.	慎用bind，它会返回一个新的函数











