---
title: React-API
date: 2017-10-26 20:28:33
tags: ['React']
categories: 'React'
copyright: true
---
#	React 高阶 API
	Creating React Elements
		```
		推荐使用 JSX
		createElement() -- 根据给定的类型创建并返回新的 React element。
		createFactory() -- 根据给定的类型返回一个创建React元素的函数。
		```
	Transforming Elements
		```
		cloneElement() -- 以 element 作为起点，克隆并返回一个新的 React Element。
		isValidElement() -- 验证对象是否是一个React元素。返回 true 或 false 。
		React.Children() -- React.Children 提供了处理 this.props.children 这个不透明数据结构的工具。
			React.Children.map(children, function[(thisArg)]) -- 返回数组
			React.Children.forEach(children, function[(thisArg)]) -- 对数组每项进行操作
			React.Children.count(children) -- 返回组件总数
			React.Children.only(children) -- 返回仅有的子级
			React.Children.toArray(children) -- 返回扁平数组
		```
		
#	Components
	React 组件可以让你把UI分割为独立、可复用的片段，并将每一片段视为相互独立的部分。
	React组件可以通过继承 React.Component 或 React.PureComponent 来定义。
##	React.Component
		```
		class Greeting extends React.Component {
		  render() {
			return <h1>Hello, {this.props.name}</h1>;
		  }
		}
		```
		组件生命周期
			装配 -- 这些方法会在组件实例被创建和插入DOM中时被调用
				```
				constructor()
				componentWillMount()
				render()
				componentDidMount()
				```
			更新 -- 属性或状态的改变会触发一次更新。当一个组件在被重渲时，这些方法将会被调用
				```
				componentWillReceiveProps()
				shouldComponentUpdate()
				componentWillUpdate()
				render()
				componentDidUpdate()
				```
			卸载 -- 当一个组件被从DOM中移除时，该方法被调用
				```
				componentWillUnmount()
				```
		其他API
			```
			setState() 参数为对象或函数
			forceUpdate()
			```
		类属性
			```
			defaultProps
				defaultProps可以被定义为组件类的一个属性，用以为类设置默认的属性。
			displayName
				displayName被用在调试信息中。
			```
		实例属性
			```
			props
				this.props包含了组件该调用者定义的属性。
			state
				状态是该组件的特定数据，其可能改变多次。
			```
		参考
			```
			render()
				可以返回：React元素、字符串和数字、Portals、null、布尔值（null和布尔值什么都不渲染）
			constructor(props)
				super(props) 有两个作用：
					this 指向（子类的实例）
					在构造函数中使用 this.props  
			componentWillMount()		
				在装配发生前被立刻调用。
				其在render()之前被调用。
			componentDidMount()	
				在组件被装配后立即调用。	
				初始化使得DOM节点应该进行到这里。ajax 和 定时器
			componentWillReceiveProps(nextProps)	
				在装配了的组件接收到新属性前调用。
			shouldComponentUpdate(nextProps, nextState)	
				使用shouldComponentUpdate()以让React知道当前状态或属性的改变是否不影响组件的输出。
			componentWillUpdate(nextProps, nextState)	
				当接收到新属性或状态时，componentWillUpdate()为在渲染前被立即调用。
			componentDidUpdate(prevProps, prevState)	
				在更新发生后立即被调用。该方法并不会在初始化渲染时调用。
			componentWillUnmount()	
				在组件被卸载和销毁之前立刻调用。	
				可以在该方法里处理任何必要的清理工作，例如解绑定时器，取消网络请求，清理任何在componentDidMount环节创建的DOM元素。
			setState(updater, [callback])
				参数为对象 {name: 'fanerge'}
				参数为函数(prevState, props) => stateChange
			component.forceUpdate(callback)	
				调用forceUpdate()将会导致组件的 render()方法被调用，并忽略shouldComponentUpdate()。
			```
##	React.PureComponent
		浅对比继承该类来对 prop 和 state 进行比较，并调用 shouldComponentUpate()。 
		深对比使用 forceUpdate() 和 不可变对象 来促进嵌套数据的快速比较。		
	
#	ReactDOM
	```
	react-dom这个软件包提供了针对DOM的方法，可以在你应用的顶级域中调用，也可以在有需要的情况下用作跳出React模型的出口。
	render()
		渲染一个React元素，添加到位于提供的container里的DOM元素中，并返回这个组件的一个引用 (或者对于无状态组件返回null).
		```
		ReactDOM.render(
		  element,
		  container,
		  [callback]
		)
		```
	ReactDOM.unmountComponentAtNode(container)
		从DOM元素中移除已挂载的React组件，清除它的事件处理器和state。
		有组件被卸载的时候返回true，没有组件可供卸载时返回 false。
	ReactDOM.findDOMNode(component)
		如果这个组件已经被挂载到DOM中，函数会返回对应的浏览器中生成的DOM元素 。
		大多数情况下，你可以添加一个指向DOM节点的引用，从而完全避免使用 findDOMNode 这个函数.		
	```
	
#	ReactDOMServer
	ReactDOMServer 类可以让你在服务端渲染你的组件。
##	ReactDOMServer.renderToString(element)
		把一个React元素渲染为原始的HTML。
		你可以用这个方法在服务端生成HTML，并根据初始请求发送标记来加快页面的加载速度，
		同时让搜索引擎可以抓取你的页面来达到优化SEO的目的。	
##	ReactDOMServer.renderToStaticMarkup(element)
		类似 renderToString，但是不会创建额外的DOM属性，例如 data-reactid 这些仅在React内部使用的属性。
		如果你希望把React当作一个简单的静态页面生成器来使用，这很有用，因为去掉
		
#	DOM Elements
##	React实现了一套与浏览器无关的DOM系统，兼顾了性能和跨浏览器的兼容性。
		```
		class -- className
		for -- htmlFor
		tabindex -- tabIndex
		aria-* （对于残障人士更易使用的各种机制）小写字母命名 
		data-* （自定义属性）小写字母命名
		```
##	React和HTML DOM属性的区别
	```
	checked属性
		受控组件 -- <input>标签type属性值为checkbox或radio时，支持checked属性。
		非受控组件 -- defaultChecked这是非受控组件的属性
	类名属性
		class -- className
	dangerouslySetInnerHTML函数
		处理Cross-site scripting (XSS) 
		dangerouslySetInnerHTML是React提供的替换浏览器DOM中的innerHTML接口的一个函数。
	htmlFor
		for -- htmlFor
	绑定事件
		onchange -- onChange
	selected
		受控组件 -- 使用selected属性，设定组件是否选中的状态。
	style属性
		<Hello style={{color: 'blue'}} />
		浏览器后缀除了ms以外，都应该以大写字母开头。
		这就是为什么WebkitTransition有一个大写字母W。
	suppressContentEditableWarning
		contentEditable 该属性少使用。
	value
		受控组件 -- <input> 和 <textarea> 组件都支持value属性。
		非受控组件 -- defaultValue属性对应的是非受控组件的属性，用来设置组件第一次加载时的值。
	支持所有的HTMl属性		
	```
	
#	SyntheticEvent
##	您的事件处理函数将会接收SyntheticEvent的实例，一个基于浏览器原生事件的跨浏览器实现。
		```
		boolean bubbles
		boolean cancelable
		DOMEventTarget currentTarget
		boolean defaultPrevented
		number eventPhase
		boolean isTrusted
		DOMEvent nativeEvent
		void preventDefault()
		boolean isDefaultPrevented()
		void stopPropagation()
		boolean isPropagationStopped()
		DOMEventTarget target
		number timeStamp
		string type
		```
##	事件池	
		SyntheticEvent是共享的。那就意味着在调用事件回调之后，SyntheticEvent对象将会被重用，并且所有属性会被置空。
##	支持的事件
	在事件名后面加Capture就能在事件捕获阶段注册事件处理函数。
	你可以使用onClickCapture代替onClick在事件捕获阶段来处理点击事件。
	[查看所有支持的事件](https://doc.react-china.org/docs/events.html)

#	Test Utilities
	导入 -- import ReactTestUtils from 'react-dom/test-utils';
	[单元测试](https://doc.react-china.org/docs/test-utils.html)

#	浅层渲染
	[浅层渲染](https://doc.react-china.org/docs/shallow-renderer.html)

#	Test Renderer	
	[Test Renderer](https://doc.react-china.org/docs/test-renderer.html)		
			
>	参考文档：
	[React官方API](https://doc.react-china.org/docs/react-api.html)
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			