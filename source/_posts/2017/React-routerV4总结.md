---
title: React-routerV4总结
date: 2017-10-29 20:01:36
tags: '路由'
categories: 'React'
copyright: true
---

#	BrowserRouter
	作用：
		<Router> 使用 HTML5 提供的 history API (pushState, replaceState 和 popstate 事件) 来保持 UI 和 URL 的同步。
	属性：
		basename: string
			当前位置的基准 URL。如果你的页面部署在服务器的二级（子）目录，你需要将 basename 设置到此子目录。 正确的 URL 格式是前面有一个前导斜杠，但不能有尾部斜杠。
		getUserConfirmation: func
			当导航需要确认时执行的函数。默认使用 window.confirm。
		forceRefresh: bool
			当设置为 true 时，在导航的过程中整个页面将会刷新。 只有当浏览器不支持 HTML5 的 history API 时，才设置为 true。
		keyLength: number
			location.key 的长度。默认是 6。
		children: node
			渲染单一子组件（元素）。
	```
	import { BrowserRouter } from 'react-router-dom'
	
	<BrowserRouter
	  basename="/calendar"
	  forceRefresh="false"
	  getUserConfirmation={optionalFunc}
	  keyLength="10"
	>
	  <Link to="/today" /> // 渲染为 <a href="/calendar/today">
	</BrowserRouter>
	```

#	HashRouter	
	作用：
		HashRouter 是一种特定的 <Router>， HashRouter 使用 URL 的 hash (例如：window.location.hash) 来保持 UI 和 URL 的同步。
	属性：
		basename: string
			当前位置的基准 URL。正确的 URL 格式是前面有一个前导斜杠，但不能有尾部斜杠。
		getUserConfirmation: func
			当导航需要确认时执行的函数。默认使用 window.confirm。
		hashType: string
			window.location.hash 使用的 hash 类型。有如下几种：
			"slash" - 后面跟一个斜杠，例如 #/ 和 #/sunshine/lollipops
			"noslash" - 后面没有斜杠，例如 # 和 #sunshine/lollipops
			"hashbang" - Google 风格的 “ajax crawlable”，例如 #!/ 和 #!/sunshine/lollipops
			默认为 "slash"。
		children: node
			渲染单一子组件（元素）。
	```
	import { HashRouter } from 'react-router-dom'

	<HashRouter>
	  <App/>
	</HashRouter>
	```

#	Link			
	作用：
		为您的应用提供声明式的、无障碍导航。		
	属性：	
		to: string/object
			需要跳转到的路径(pathname)或地址（location）。
			需要跳转到的地址（location）。
		replace: bool
			当设置为 true 时，点击链接后将使用新地址替换掉访问历史记录里面的原地址。
			当设置为 false 时，点击链接后将在原有访问历史记录的基础上添加一个新的纪录。默认为 false。	
```
<Link to="/about">关于</Link>
<Link to={{
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: { fromDashboard: true }
}}/>
```	
			
#	NavLink		
	作用：	
		为当前 URL 添加 class 和 style。
	属性：
		activeClassName: string
			选中 URL 是添加的class。
		activeStyle：object
			选中 URL 是添加的style。
		exact: bool	
			如果为 true，path 为 '/one' 的路由将不能匹配 '/one/two'，反之，亦然。	
		strict: bool
			对路径末尾斜杠的匹配。如果为 true。path 为 '/one/' 将不能匹配 '/one' 但可以匹配 '/one/two'。
		isActive: func	
			为URL匹配添加更严谨的逻辑函数。	
			
#	Prompt
	作用：	
		当用户离开当前页面前做出一些提示。
	属性：
		message: string/func
			当用户离开当前页面时，设置的提示信息。
			当用户离开当前页面时，设置的回掉函数
		when: bool
			通过设置一定条件要决定是否启用 Prompt
			
#	MemoryRouter
	作用：
		无 DOM 的环境。
		
	属性：
		initialEntries: array
			一个 history 堆栈的数组。
		initialIndex: number
			在 initialEntries 数组中的初始index。
		getUserConfirmation: func
		keyLength: number
		children: node
		
#	Redirect	
	作用：
		重定向将替代当前的location 中的 history。
	属性：
		to: string/object
			重定向string
			重定向object
		push: bool
			当为 true 时，替换掉整个history，而不是当前这一条。
		from: string	
			重定向的路径名。
		
#	Route
	作用：
		它最基本的职责就是当页面的访问地址与 Route 上的 path 匹配时，就渲染出对应的 UI 界面。
	属性：
		component: func
			只有当访问地址和路由匹配时，一个 React component 才会被渲染，此时此组件接受 route props (match, location, history)。
		render: func	
			此方法适用于内联渲染，而且不会产生上文说的重复装载问题。
		children: func
			有时候你可能只想知道访问地址是否被匹配，然后改变下别的东西，而不仅仅是对应的页面。
		path: string
		exact: bool
		strict: bool
		
#	Router
	作用：底层路由接口组件，使用封装后的组件代替。	
		<BrowserRouter>
		<HashRouter>
		<MemoryRouter>
		<NativeRouter>
		<StaticRouter>
	属性：
		history：object
			导航的history对象
		children: node
				
#	StaticRouter
	定义：
		不会更改location的 <Router>，服务端渲染。
	属性：
		basename: string
		location: string	
		context: object	
		children: node	
			
#	Switch	
	定义：
		只渲染出第一个与当前访问地址匹配的 <Route> 或 <Redirect>。
		我们只想渲染出第一个匹配的路由就可以了，于是 <Switch> 应运而生！
	属性：
		children: node

#	对象和方法
##	history
###	路由分类
	1.browser history -- HTML5 history API
	2.hash history    -- 低版本浏览器
	3.memory history  -- 无DOM环境（RN 和 Node）
###	histoty对象详解
	length -- number 浏览历史堆栈中的条目数。		
	action -- string 路由跳转到当前执行页面的动作，分为 PUSH、REPLACE、POP。
	location -- object 当前访问地址信息组成的对象
		（pathname、search、hash、state）
	
	push(path, [state]) 在历史堆栈信息里加入一个新条目。
	replace(path, [state]) 在历史堆栈信息里替换掉当前的条目
	go(n) 将 history 堆栈中的指针向前移动 n。
	goBack() 等同于 go(-1)
	goForward 等同于 go(1)
	block(prompt) 阻止跳转
##	location	
	location 是指你当前的位置，将要去的位置，或是之前所在的位置
	在以下情境中可以获取 location 对象:
		1.在 Route component 中，以 this.props.location 获取
		2.在 Route render 中，以 ({location}) => () 方式获取
		3.在 Route children 中，以 ({location}) => () 方式获取
		4.在 withRouter 中，以 this.props.location 的方式获取
	可以在不同情境中使用 location：
		1.<Link to={location} />
		2.<NaviveLink to={location} />
		3.<Redirect to={location />
		4.history.push(location)
		5.history.replace(location)
##	match
	match 对象包含了 <Route path> 如何与 URL 匹配的信息，具有以下属性：
		1.params: object 路径参数，通过解析 URL 中的动态部分获得键值对
		2.isExact: bool 为 true 时，整个 URL 都需要匹配
		3.path: string 用来匹配的路径模式，用于创建嵌套的 <Route>
		4.url: string URL 匹配的部分，用于嵌套的 <Link>
	在以下情境中可以获取 match 对象
		1.在 Route component 中，以 this.props.match获取
		2.在 Route render 中，以 ({match}) => () 方式获取
		3.在 Route children 中，以 ({match}) => () 方式获取
		4.在 withRouter 中，以 this.props.match的方式获取
		5.matchPath 的返回值
	注：当一个 Route 没有 path 时，它会匹配一切路径。
##	matchPath
	pathname
	props
##	withRouter
		
>	参考文档：
	[MDN-history](https://developer.mozilla.org/en-US/docs/Web/API/History)
	[初探 React Router 4.0](http://blog.csdn.net/sinat_17775997/article/details/69218382)
