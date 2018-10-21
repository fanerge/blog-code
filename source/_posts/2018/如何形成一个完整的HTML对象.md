---
title: 如何形成一个完整的HTML对象
date: 2018-07-28 19:22:25
tags: 'html5'
categories: 'html5'
copyright: true
---
写在前面，本文将同步发布于Blog、掘金、segmentfault、知乎等处，如果本文对你有帮助，记得为我得到我的[个人技术博客项目](https://github.com/fanerge/fanerge.github.io)给个star哦。
#	为何写这篇文章？
你可能做Web开发已经有一段时间，你是否有想过下列问题呢？
为什么div元素甚至是所有的html元素都可以使用addEventListener来添加事件呢？
为什么每个DOM节点都有parentNode、firstChild、nodeType等属性呢？
为什么每个DOM元素都有className、classList、innerHTML等属性呢？
为什么有些DOM元素有accessKey、contentEditable、isContentEditable等属性呢？
为什么每个DOM元素都有onclick、ondblclick、ondrag等属性？
本文就是来解答这些简单而又不“简单”的问题。
#	Object
你可以在浏览器中选择一个节点，然后在控制台中输入
```
$0.hasOwnProperty 
// ƒ hasOwnProperty() { [native code] }
```
PS：$0表示当前选择的DOM元素。
为什么我的一个元素会有对象的方法，追究其原因在设计HTML时，通过从各个接口继承不同的属性和方法。
例如span元素继承关系：span -> HTMLSpanElement -> HTMLElement -> Element -> Node -> EventTarget-> Object
![property](http://oxpnrlb4j.bkt.clouddn.com/html%E5%B1%9E%E6%80%A7.png)
[2018-08-16]
#	EventTarget
##	定义
EventTarget 是一个由可以接收事件的对象实现的接口，并且可以为它们创建侦听器。
##	作用
Element，document 和 window 是最常见的事件目标，但是其他对象也可以是事件目标，比如XMLHttpRequest，AudioNode，AudioContext 等等。
许多事件目标（包括元素，文档和 window）还支持通过 onXXX（如onclick） 属性和属性设置事件处理程序。
##	该接口的方法
###	EventTarget.addEventListener()
在EventTarget上注册特定事件类型的事件处理程序。
###	EventTarget.removeEventListener()
EventTarget中删除事件侦听器。
###	EventTarget.dispatchEvent()
将事件分派到此EventTarget。
##	我们自己实现EventTarget
```
var EventTarget = function() {
  this.listeners = {};
};

EventTarget.prototype.listeners = null;
EventTarget.prototype.addEventListener = function(type, callback) {
  if (!(type in this.listeners)) {
    this.listeners[type] = [];
  }
  this.listeners[type].push(callback);
};

EventTarget.prototype.removeEventListener = function(type, callback) {
  if (!(type in this.listeners)) {
    return;
  }
  var stack = this.listeners[type];
  for (var i = 0, l = stack.length; i < l; i++) {
    if (stack[i] === callback){
      stack.splice(i, 1);
      return;
    }
  }
};

EventTarget.prototype.dispatchEvent = function(event) {
  if (!(event.type in this.listeners)) {
    return true;
  }
  var stack = this.listeners[event.type].slice();

  for (var i = 0, l = stack.length; i < l; i++) {
    stack[i].call(this, event);
  }
  return !event.defaultPrevented;
};
```
#	Node
##	定义
Node是一个接口，许多DOM类型从这个接口继承，并允许类似地处理（或测试）这些各种类型。Node是一个接口，许多DOM类型从这个接口继承，并允许类似地处理（或测试）这些各种类型。
##	有那些接口重Node继承其方法和属性？
Document, Element, CharacterData (which Text, Comment, and CDATASection inherit), ProcessingInstruction, DocumentFragment, DocumentType, Notation, Entity, EntityReference
PS：在方法和属性不相关的特定情况下，这些接口可能返回null。它们可能会抛出异常 - 例如，当将子节点添加到不允许子节点存在的节点时。
##	接口相关的属性和方法
###	属性
####	Node.baseURI
返回一个表示base URL的DOMString。不同语言中的base URL的概念都不一样。 在HTML中，base URL表示协议和域名，以及一直到最后一个'/'之前的文件目录。
####	Node.childNodes
返回一个包含了该节点所有子节点的实时的NodeList。NodeList 是“实时的”意思是，如果该节点的子节点发生了变化，NodeList对象就会自动更新。 
####	Node.firstChild
返回该节点的第一个子节点，如果该节点没有子节点则返回null。
####	Node.lastChild
返回该节点的最后一个子节点，如果该节点没有子节点则返回null。
此处省略若干Node接口属性，[更多属性查看这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Node#属性)。
###	方法
--------------------重点分割线-------------------
重点：从其父类EventTarget继承了addEventListener、removeEventListener、dispatchEvent等方法。
####	Node.appendChild()
将一个节点添加到指定父节点的子节点列表末尾。
####	Node.contains()
返回的是一个布尔值，来表示传入的节点是否为该节点的后代节点。
####	Node.cloneNode()
返回调用该方法的节点的一个副本。
此处省略若干Node接口方法，[更多方法查看这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Node#方法)。
#	Element
##	说明
Element是非常通用的基类，所有 Document对象下的对象都继承它。这个接口描述了所有相同种类的元素所普遍具有的方法和属性。 这些继承自Element并且增加了一些额外功能的接口描述了具体的行为。
PS：HTMLElement 接口是所有HTML元素的基础接口， 而 SVGElement 接口是所有SVG元素的基本接口。
在web以外的语言，像 XUL 可以通过 XULElement 的API，也能实现它。
##	属性
所有属性继承至它的祖先接口 Node, 和它所扩展的接口 EventTarget, 并且从以下部分继承了属性ParentNode, ChildNode, NonDocumentTypeChildNode, 和Animatable.
###	Element.assignedSlot
返回元素对应的 HTMLSlotElement 接口
###	Element.attributes
返回一个与该元素相关的所有属性集合NamedNodeMap 
###	Element.classList
返回该元素包含的class属性是一个DOMTokenList.
###	Element.className
它是一个 DOMString 表示这个元素的class.
此处省略若干Element接口属性，[更多方法查看这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Element#Properties)。
##	方法
--------------------重点分割线-------------------
从它的父类（Node）和它父类的父类（EventTarget）继承方法，并实现parentNode、ChildNode、NonDocumentTypeChildNode、Animatable。
此处省略若干Element接口方法，[更多方法查看这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Element#Properties)。
###	Element.closest()
方法用来获取匹配特定选择器且离当前元素最近的祖先元素（也可以是当前元素本身）。如果匹配不到，则返回 null。
###	Element.getAttribute()
返回元素上一个指定的属性值。如果指定的属性不存在，则返回  null 或 "" （空字符串）。
###	Element.getElementsByClassName()
参数中给出类的列表，返回一个动态的 HTMLCollection ，这里面包含了所有持有这些类的后代元素。
此处省略若干Element接口方法，[更多方法查看这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Element#Methods)。
#	HTMLElement
##	作用
HTMLElement 接口表示所有的 HTML 元素。一些HTML元素直接实现了HTMLElement接口，其它的间接实现HTMLElement接口。
##	属性
--------------------重点分割线-------------------
继承自父接口Element和 GlobalEventHandlers的属性。  
HTMLElement.accessKey	DOMString	获取/设置元素访问的快捷键
HTMLElement.accessKeyLabel	DOMString	返回一个包含元素访问的快捷键的字符串（只读）
HTMLElement.contentEditable	DOMString	获取/设置元素的可编辑状态
HTMLElement.isContentEditable Boolean	表明元素的内容是否可编辑（只读）
此处省略若干HTMLElement接口属性，[更多方法查看这里](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement#属性)。
##	Event handlers
HTMLElement.onTouchStart
HTMLElement.onTouchEnd 
HTMLElement.onTouchMove 
HTMLElement.onTouchEnter 
HTMLElement.onTouchLeave 
HTMLElement.onTouchCancel 
##	方法
HTMLElement.blur()	void	元素失去焦点
HTMLElement.click()	void	触发元素的点击事件
HTMLElement.focus()	void	元素获得焦点
HTMLElement.forceSpellCheck() 	void	 
#	GlobalEventHandlers
##	定义
GlobalEventHandlers接口描述了事件处理程序像HTMLElement常见的几个接口,文件,窗口,或WorkerGlobalScope Web Workers。这些接口可以实现更多的事件处理程序。
##	属性
###	GlobalEventHandlers.onabort
中断事件。
###	GlobalEventHandlers.onblur
失去焦点事件。
###	GlobalEventHandlers.onfocus
获取焦点事件。
此处省略若干GlobalEventHandlers接口属性，[更多方法查看这里](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers#Properties)。
#	元素接口
该接口用于创建对应的元素。
如：
HTMLDivElement 接口提供了一些特殊属性（它也继承了通常的 HTMLElement 接口）来操作div元素。
HTMLFormElement接口可以创建或者修改<form>对象;它继承了HTMLElement接口的方法和属性。
HTMLAnchorElement 接口表示超链接元素，并提供一些特别的属性和方法（除了那些继承自普通 HTMLElement对象接口的之外）以用于操作这些元素的布局和显示。
......
#	回答前面问题
通过上面的知识，我们了解到：
HTMLDivElement（其他元素接口） 继承 HTMLElement 和 GlobalEventHandlers 接口。
HTMLElement 继承 Element 接口。
Element 继承 Node 接口。
Node 继承 EventTarget 接口。
![html由来](http://oxpnrlb4j.bkt.clouddn.com/html%E5%85%83%E7%B4%A0%E7%94%B1%E6%9D%A5%281%29.svg)
为什么div元素甚至是所有的html元素都可以使用addEventListener来添加事件呢？
回答：从 EventTarget 接口中继承而来。
为什么每个DOM节点都有parentNode、firstChild、nodeType等属性呢？
回答：从 Node 接口中继承而来。
为什么每个DOM元素都有className、classList、innerHTML等属性呢？
回答：从 Element 接口中继承而来。
为什么有些DOM元素有accessKey、contentEditable、isContentEditable等属性呢？
回答：从 HTMLElement 接口中继承而来。
为什么每个DOM元素都有onclick、ondblclick、ondrag等属性？
回答：从 GlobalEventHandlers 接口中继承而来。
--------------------重点分割线-------------------
##	只有通过上面的继承关系，我们得到的 DOM 元素才是一个完整的 HTML 对象，我们才能为它设置/获取属性、绑定事件、添加样式类等操作。
参考文档：
>	
[EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)
[Node](https://developer.mozilla.org/en-US/docs/Web/API/Node)
[Element](https://developer.mozilla.org/en-US/docs/Glossary/Element)
[HTMLElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement)
[GlobalEventHandlers](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalEventHandlers)


