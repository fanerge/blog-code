---
title: dom接口
date: 2018-05-08 20:43:18
tags: 'js'
keywords: 'DOM'
categories: 'js'
copyright: true
---
#	CustomEvent
创建一个自定义事件。
```
// 添加一个适当的事件监听器
obj.addEventListener("cat", function(e) { process(e.detail) })

// 创建一个自定义事件
var event = new CustomEvent("cat", {"detail":{"hazcheeseburger":true}})
// 分发事件
obj.dispatchEvent(event)
```

#	DocumentFragment
DocumentFragment 接口表示一个没有父级文件的最小文档对象。
DocumentFragment不是真实DOM树的一部分，它的变化不会引起DOM树的重新渲染的操作(reflow) ，且不会导致性能等问题。
##	创建一个DocumentFragment
```
let ul = document.querySelector(`[data-uid="ul"]`);
let frag = document.createDocumentFragment();
const list = [
	'IE',
	'Chrome'
];
list.forEach(item => {
	let li = document.creteElement('li');
	li.textContent = item;
	frag.appendChild(li);
});
// 只进行一次dom操作，触发一次reflow
ul.appendChild(frag);
```	
##	其他方法（实例方法）
```
documentFragment.find() 
返回 DocumentFragment 树里第一个匹配的元素 Element 。
documentFragment.findAll() 
返回 DocumentFragment 树里所有匹配的元素  NodeList。
documentFragment.querySelector()
documentFragment.querySelectorAll()
documentFragment.getElementById()
```
#	MutationObserver
给开发者们提供了一种能在某个范围内的DOM树发生变化时作出适当反应的能力.该API设计用来替换掉在DOM3事件规范中引入的Mutation事件.
##	实例方法
observe()
给当前观察者对象注册需要观察的目标节点,在目标节点(还可以同时观察其后代节点)发生DOM变化时收到通知.
disconnect()
让该观察者对象停止观察指定目标的DOM变化.即使再次调用其observe()方法,该观察者对象包含的回调函数都不会再被调用.
takeRecords()
清空观察者对象的记录队列,并返回里面的内容.
##	示例
```
let target = document.querySelector('#some-id');

// 创建观察者对象
let observer = new MutationObserver(function(mutations) {
  mutations.forEach(function(mutation) {
    console.log(mutation.type);
  });    
});

// 配置观察选项:
let config = { attributes: true, childList: true, characterData: true }
 
// 传入目标节点和观察选项
observer.observe(target, config);
 
// 随后,你还可以停止观察
observer.disconnect();
```
























































