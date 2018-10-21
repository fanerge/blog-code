---
title: React-组件书写方式
date: 2017-10-23 20:57:29
tags: ['React']
categories: 'React'
copyright: true
---
#	createClass
	ES5 定义组件只能使用 createClass	
	```
	const React = require(react);
	const Greeting = React.createClass({
	
		// 属性校验 
		propTypes: {
			name: React.PropTypes.string 
		},
		
		// 默认属性
		getDefaultProps: function() {
			return {
				name: 'fanerge' 
			};
		},
		
		// 初始化state
		getInitialState: function() {
			return {
				count: this.props.initialCount
			};
		}, 
		
		// 事件函数
		handleClick: function() {  },
		
		render: function() {
			return <h1 onClick={this.handleClick}>{{this.props.name}}</h1>
		};
	});
	
	module.exports = Greeting;
	```
	PS：使用createClass，React对属性中的所有函数都进行了this绑定，也就是如上面的hanleClick其实相当于handleClick.bind(this) 。

#	component
	ES6 的类（语法糖）对原型继承机制进行了封装
	```
	import React from 'react';
	class Greeting extends React.Component {
	
		constructor(props) {
			super(props);
			// 设置初始值
			this.state = {count: props.initialCount};
			// 为函数绑定 this
			this.handleClick = this.handleClick.bind(this);
		}
		
		// 定义props方式1
		static defaultProps = {
			name: 'Mary' 
		}
		
		// props验证方式1
		static propTypes = {
			name: React.PropTypes.string
		}
		
		// 事件函数
		handleClick() {  }
		
		render() {
			return <h1>Hello, {this.props.name}</h1>
		};
	};
	
	// props验证方式2
	Greeting.propTypes = {
		name: React.PropTypes.string
	};
	
	// 定义props方式2
	Greeting.defaultProps = {
		name: 'Mary' 
	};
	
	export default Greeting;
	```
	PS：可以看到Greeting继承自React.component,在构造函数中，通过super()来调用父类的构造函数。

#	PureComponet
	作用：当组件更新时，如果组件的 props 和 state 都没发生改变， render 方法就不会触发，省去 Virtual DOM 的生成和比对过程，达到提升性能的目的。
	```
	class CounterButton extends React.PureComponent {
		
		constructor(props) {
			super(props);
			this.state = {count: 1};
		}
		
		render() {
			return(
				<button
					color={this.props.color}
					onClick={() => this.setState(state => ({count: state.count + 1}))}>
					Count: {this.state.count}
				</button>
			);
		}
	
	}
	```
	PS：这种情况下，PureComponent只会对this.props.words进行一次浅比较，虽然数组里面新增了元素，
		但是this.props.words与nextProps.words指向的仍是同一个数组，因此this.props.words !== nextProps.words 
		返回的便是flase，从而导致ListOfWords组件没有重新渲染。
	最简单避免上述情况的方式，就是避免使用可变对象作为props和state，取而代之的是每次返回一个全新的对象,如下通过concat来返回新的数组：
	```
	handleClick() {
	  this.setState(prevState => ({
		words: prevState.words.concat(['marklar'])
	  }));
	}
	```
	你还可以考虑使用Immutable.js来创建不可变对象，通过它来简化对象比较，提高性能。

#	Stateless Functional Component
	作用：数据都是通过props传入的时候，我们便可以使用Stateless Functional Component来快速创建组件。
	```
	import React from 'react';
	const Button = ({
		day, 
		increment
	}) => {
		return (
			<div>
				<button onClick={increment}>Today is {day}</button>
			</div>
		);
	};
	
	Button.propTypes = {
		day: PropTypes.string.isRequired,
		increment: PropTypes.func.isRequired
	};
	```
#	ES7
```
class IndexPage extends PureComponent {
  state = {
    stripeObj : {
      number: 6,
      color: 'rgba(16, 142, 233, 1)',
      show: true
    },
    ellipsisObj: {
      fontSize : '40px',
      color: 'red',
      show: true
    }
  }

  // 事件方法，自动绑定 this
  stripeColorChange = (e) => {
    alert('sds');
  }

  render (){
    return (<div onClick={this.stripeColorChange}></div>);
  }
}

IndexPage.propTypes = {
};
```	
#	如何选择（优先级降低）
1.	Stateless Functional Component（无状态、无生命周期）
2.	PureComponent （不可变对象，最好配合Immutable.js使用）
3.	Component
4.	createClass

>	参考文档：
	[谈一谈创建React Component的几种方式](https://segmentfault.com/a/1190000008402834)
	[React创建组件的三种方式及其区别](http://www.cnblogs.com/wonyun/p/5930333.html)
	
	