---
title: MVC学习
date: 2017-09-21 20:22:26
category: "设计模式"
tags: ['MVC']
---
###	名词解释
	MVC的全名是Model View Controller，是模型（model）--视图（view）--控制器（controller）的缩写，是一种软件设计典范。
	M（数据模型）：比如你设计一个User对象，包含username和password属性，它就是一个简单的M。
	M是指业务模型，V是指用户界面，C则是控制器。
###	在web开发中
	V--即View视图是指用户看到并与之交互的界面。比如由html元素组成的网页界面，或者软件的客户端界面。
		MVC的好处之一在于它能为应用程序处理很多不同的视图。
		在视图中其实没有真正的处理发生，它只是作为一种输出数据并允许用户操纵的方式。
	M--即Model模型是指模型表示业务规则。在MVC的三个部件中，模型拥有最多的处理任务。
		被模型返回的数据是中立的，模型与数据格式无关，这样一个模型能为多个视图提供数据，
		由于应用于模型的代码只需写一次就可以被多个视图重用，所以减少了代码的重复性。
	C--即Controller控制器是指控制器接受用户的输入并调用模型和视图去完成用户的需求，
		控制器本身不输出任何东西和做任何处理。它只是接收请求并决定调用哪个模型构件去处理请求，
		然后再确定用哪个视图来显示返回的数据。
	它们三者的关系
![mvc关系图](http://images2015.cnblogs.com/blog/811883/201704/811883-20170423150019101-1710764799.jpg)
	用户首先在界面中进行人机交互，然后请求发送到控制器，控制器根据请求类型和请求的指令发送到相应的模型，
	模型可以与数据库进行交互，进行增删改查操作，完成之后，根据业务的逻辑选择相应的视图进行显示，此时用户获得此次交互的反馈信息，
	用户可以进行下一步交互，如此循环。
###	每层主要的功能
	视图（View）：用户界面
	控制器（Controller）：业务逻辑
	模型（Model）：数据保存
![各部分之间的通信方式如下](http://image.beekka.com/blog/2015/bg2015020105.png)
###	wiki
	控制器（Controller）- 负责转发请求，对请求进行处理。
	视图（View） - 界面设计人员进行图形界面设计。
	模型（Model） - 程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。
![图解MVC](/images/ModelViewControllerDiagramZh.png)

	将应用程序划分为三种组件，模型 - 视图 - 控制器（MVC）设计定义它们之间的相互作用。
	模型（Model） 用于封装与应用程序的业务逻辑相关的数据以及对数据的处理方法。“ Model ”有对数据直接访问的权力，例如对数据库的访问。
	“Model”不依赖“View”和“Controller”，也就是说， Model 不关心它会被如何显示或是如何被操作。
	但是 Model 中数据的变化一般会通过一种刷新机制被公布。
	为了实现这种机制，那些用于监视此 Model 的 View 必须事先在此 Model 上注册，从而，View 可以了解在数据 Model 上发生的改变。（比较：观察者模式（软件设计模式））
	视图（View）能够实现数据有目的的显示（理论上，这不是必需的）。在 View 中一般没有程序上的逻辑。
	为了实现 View 上的刷新功能，View 需要访问它监视的数据模型（Model），因此应该事先在被它监视的数据那里注册。
	控制器（Controller）起到不同层面间的组织作用，用于控制应用程序的流程。它处理事件并作出响应。
	“事件”包括用户的行为和数据 Model 上的改变。
###	javscript的一个MVC示例
	```
	/** 模擬 Model, View, Controller */
	var M = {}, V = {}, C = {};

	/** Model 負責存放資料 */
	M.data = "hello world";

	/** View 負責將資料輸出到螢幕上 */
	V.render = (M) => { alert(M.data); }

	/** Controller 作為一個 M 和 V 的橋樑 */
	C.handleOnload = () => { V.render(M); }

	/** 在網頁讀取的時候呼叫 Controller */
	window.onload = C.handleOnload;
	```
>	参考文档：
	[MVC简介](http://www.cnblogs.com/diyunfei/p/6752618.html)	
	[阮一峰](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)
	[wiki-MVC](https://zh.wikipedia.org/wiki/MVC)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	