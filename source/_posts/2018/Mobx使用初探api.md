---
title: Mobx使用初探api
date: 2018-11-21 21:17:30
tags: 'Mobx'
categories: '前端'
copyright: true
---
#	Mobx数据流
简单总结下Mobx常用api的使用
![mobx数据流](https://cn.mobx.js.org/flow.png)
#	可观察数据
##	Array、Object、Map、
Observable // 将一个数据变成可观察数据（数组不是真正的数组）
extendObservable() // 将动态添加的数据变为可观察（对象）
##	String、Number、Boolean
如果是方法的还需要observable.box来修饰
调用get和set方法可以访问和修改原始类型值
#	对可观察数据做出的反应
##	computed 
可以根据多个可观察数据产生一个新的可观察数据
##	autorun
自动追踪可观察数据，当在可观察数据发生变化时执行（会初始化执行一次）
##	when
当第一个参数为true，执行第二个参数方法
##	reaction
它接收两个函数参数，第一个(数据函数)是用来追踪并返回数据作为第二个函数(效果 函数)的输入
#	修改可观察数据
action
action.bound // 多一个功能绑定this
runInAction
将多次可观察数据的改变合并到一次触发（优化性能）
#	mobx-react
import {PropTypes as mobxPropTypes} from ‘mobs-react’;
给UI组件使用 @observer // 把react组件的render方法包装成autorun
#	工具函数
##	import {intercept, observe} from ‘mobx’;
observe 和 intercept 可以用来监测单个 observable(它们不追踪嵌套的 observable) 的变化。
intercept 可以在变化作用于 observable 之前监测和修改变化。 observe 允许你在 observable 变化之后拦截改变。
##	toJS
递归地将一个(observable)对象转换为 javascript 结构。
支持 observable 数组、对象、映射和原始类型。 
##	spy
spy(listener). 注册一个全局间谍监听器，用来监听所有 MobX 中的事件。
##	trace
trace 是一个小工具，它能帮助你查找为什么计算值、 reactions 或组件会重新计算。

#	mobx提升性能法则
细粒度拆分视图组件
使用专用组件处理列表
尽可能晚地解构可观察数据