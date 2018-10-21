---
title: PWA-1
date: 2017-07-25 22:35:26
category: "PWA"
tags: '有趣的'
---
## Lavas -- 基于 Vue 的 PWA 解决方案，帮助开发者快速搭建 PWA 应用，解决接入 PWA 的各种问题
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### 基础教程
1.  Lavas 介绍
	Lavas 是什么？
		Lavas 是一个基于 Vue 的 PWA (Progressive Web Apps) 完整解决方案。
	Lavas 做什么？
		站点 PWA 化需要做什么，Lavas 就做什么。
		PWA (Progressive Web Apps) 是一种 Web App 新模型：
		1.站点可添加至主屏幕
		2.全屏方式运行
		3.支持离线缓存
		4.消息推送 ...
2.	探索PWA
	什么是PWA？
		PWA 工程的解决方案中借助了 service worker 的离线存储能力，消息推送能力以及系统的添加桌面能力，从而形成一个完善的 Web App 解决方案，帮助我们在 Web 端低成本的开发和维护一个逐步类 Native App 化的 Web App。
	什么站点适合改造成 PWA？
		除了对系统强依赖的 App, 以及游戏类的 App 等, 所有的 Native App 都可以改造成 PWA 应用。
	PWA 的效果？
		1.https 环境部署。
		2.响应式设计，一次部署，可以在移动设备和 PC 设备上运行。
		3.在不同浏览器下可正常访问。
		4.浏览器离线和弱网环境可极速访问。
		5.可以把 App Icon 入口添加到桌面。
		6.点击 Icon 入口有类似 Native App 的动画效果。
		7.灵活的热更新。
	离线缓存
		HTML5 新的 [service worker API](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API)，提供的离线缓存能力
	SPA (Single Page Apps)
	基于 Vue 架构的 PWA 工程
	App Shell
		App Shell 架构是构建 PWA 工程的一种方式，这种应用能可靠且即时地加载到您的用户屏幕上，与本地应用相似。
3.	快速开始 PWA 工程
	依赖工具
		$ npm install -g lavas
	初始化工程	
		$ lavas init	
	运行	
		cd projectName
		npm install
		npm run dev
4.	开发一个页面（以NotFound页面为例）
	添加路由	
		```
		const NotFound = () => import('@/pages/NotFound.vue');

		routes: [
			// 省略其他路由对象
			{
				path: '*',
				name: 'notFound',
				component: NotFound
			}
		]
		```
	页面组织结构	
		标准.vue单文件
	与app shell 的交互
		vuex	
	监听全局事件	
		```
		import EventBus from '@/event-bus';

		// 在 activated 钩子中注册
		EventBus.$on(`app-header:click-action`, ({actionIdx}) => {
			// 处理点击按钮事件
			// ...
		});
		```
	组件开发
		[vuetify](https://vuetifyjs.com/)
	异步请求数据	
		 [axios]()
5.	调试工程	
	webpack dev-server	
	chrome 调试	
6.	构建部署工程
	生产环境构建
	部署到服务器

###	进阶教程
1.	维护 service-worker.js 文件
	service-worker.js
		service-worker.js 文件作为缓存管理的重要文件	
	如何配置缓存内容
		通过 config/sw-precache.js 文件进行缓存配置，根据配置为用户缓存网站静态与动态资源，并截获用户的所有网络请求，决定是从缓存还是网络获取相应资源，限制缓存大小等。
2.	Service Worker 与页面通信
	如何使用 postMessage 方法发送信息
		在 sw.js 中向接管页面发信息，可以采用 client.postMessage() 方法
		在主页面给 Service Worker 发消息，可以采用 navigator.serviceWorker.controller.postMessage() 方法
	如何接收 postMessage 发送的信息
		在 sw.js 中接收主页面发来的信息，示例代码如下，通过 event.data 来读取数据
		在页面中接收 sw.js 发来的信息，示例代码如下，通过 event.data 来读取数据
3.	App Shell 调整及扩展
	App Shell 模型
	调整及扩展 App Shell
		状态管理
		与路由组件通信
		扩展 Shell
4.	App Skeleton 介绍
	Lavas 的 Skeleton 支持
	默认 Skeleton
5.	页面切换动画效果
	具体实现
6.	修改项目主题
	配置文件
		主题相关的配置文件在 config/theme.js 中
		使用预定义的颜色变量
		使用主题变量
7.	在项目中使用图标
	字体文件
	自定义 SVG
8.	使用 Material Design UI 开发
	muse-ui、vue-material、vuetify
9.	HTTPS 环境部署
10.	服务器端渲染

参考：
>	
[lavas-百度PWA解决方案](https://lavas.baidu.com/)
















