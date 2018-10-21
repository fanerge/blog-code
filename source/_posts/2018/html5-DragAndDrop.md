---
title: 拖放（Drag 和 Drop）
date: 2018-01-23 20:20:03
tags: 'html5'
categories: 'html5'
copyright: true
---
#	理论介绍
拖放（Drag 和 drop）是 HTML5 标准的组成部分。
DataTransfer 对象：拖拽对象用来传递的媒介，使用一般为Event.dataTransfer。
draggable 属性：为需要拖拽的元素设置该属性。
Event.effectAllowed 属性：就是拖拽的效果。
Event.preventDefault() 方法：阻止默认的些事件方法等执行。
在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。
##	相关事件
###	拖动目标上触发事件
ondragstart 事件：当拖拽元素开始被拖拽的时候触发的事件。
ondrag 事件：当元素或者选择的文本被拖动时触发 drag 事件，大约每几百毫秒的触发一次。
ondragend 事件：当拖拽完成后触发的事件（比如松开鼠标按键或敲“Esc”键）。
###	释放目标时触发的事件
ondragenter 事件：当拖曳元素进入目标元素的时候触发的事件。
ondragover 事件：拖拽元素在目标元素上移动的时候触发的事件。
ondragleave 事件：当被鼠标拖动的对象离开其容器范围内时触发此事件。
ondrop 事件：被拖拽的元素在目标元素上同时鼠标放开触发的事件。
ondragexit 事件：当一个元素不再拖动立即选择目标元素触发。
#	DataTransfer 对象详解
在进行拖放操作时，DataTransfer 对象用来保存被拖动的数据。它可以保存一项或多项数据、一种或者多种数据类型。
##	属性
dropEffect	String
	设置实际的放置效果，它应该始终设置成 effectAllowed  的可能值之一 。
effectAllowed	String
	用来指定拖动时被允许的效果。
files 	FileList
	包含一个在数据传输上所有可用的本地文件列表。如果拖动操作不涉及拖动文件，此属性是一个空列表。
types	DOMStringList
	保存一个被存储数据的类型列表作为第一项，顺序与被添加数据的顺序一致。
##	方法
addElement(element)
	设置拖动源。通常你不需要改变这项，如果修改这项将会影响拖动的哪个节点和dragend事件的触发。默认目标是被拖动的节点。
setData(type,data)
	为一个给定的类型设置数据。
getData()
	根据指定的类型检索数据，如果指定类型的数据不存在或者该 DataTransfer 对象中没有数据，方法将返回一个空字符串。
clearData(type)
	删除与给定类型关联的数据。类型参数是可选的。
setDragImage(imgElement,offsetX,offsetY)
	自定义一个期望的拖动时的图片。大多数情况下，这项不用设置，因为被拖动的节点被创建成默认图片。
#	实现拖拽
##	确定什么是可拖动的
让一个元素被拖动需要添加 draggable 属性，再加上全局事件处理函数ondragstart。
```
function dragstart_handler(ev) {
	console.log("dragStart");
	ev.dataTransfer.setData("text/plain", ev.target.id);
}

<body>
	<p id="p1" draggable="true" ondragstart="dragstart_handler(event);">This element is draggable.</p>
</body>
```
##	定义拖动数据
应用程序可以在拖动操作中包含任意数量的数据项。每个数据项都是一个  string 类型，典型的MIME类型，如：text/html。
```
function dragstart_handler(ev) {
	// 添加拖拽数据
	ev.dataTransfer.setData("text/plain", ev.target.id);
	ev.dataTransfer.setData("text/html", "<p>Example paragraph</p>");
	ev.dataTransfer.setData("text/uri-list", "http://developer.mozilla.org");
}
```
##	定义拖动图像
拖动过程中，浏览器会在鼠标旁显示一张默认图片。当然，应用程序也可以通过setDragImage() 方法自定义一张图片.
```
function dragstart_handler(ev) { 
	var img = new Image(); 
	img.src = 'example.gif'; 
	ev.dataTransfer.setDragImage(img, 10, 10);
}
```
PS：img 的 src 属性路径是以使用该 js 页面为基准。
##	定义拖动效果
```
function dragstart_handler(ev) {
  // Set the drag effect to copy
  ev.dataTransfer.dropEffect = "copy";
}
```
##	定义一个放置区
```
function dragover_handler(ev) {
	// 这里必须阻止默认行为，否则没有效果
	ev.preventDefault();
	ev.dataTransfer.dropEffect = "move"
}
function drop_handler(ev) {
	ev.preventDefault();
	// Get the id of the target and add the moved element to the target's DOM
	var data = ev.dataTransfer.getData("text");
	ev.target.appendChild(document.getElementById(data));
}
<body>
	<div id="target" ondrop="drop_handler(event);" ondragover="dragover_handler(event);">Drop Zone</div>
</body>
```
##	处理放置效果
```
function dragstart_handler(ev) {
	ev.dataTransfer.setData("text/plain", ev.target.id);
	ev.dropEffect = "move";
}
function dragover_handler(ev) {
	ev.preventDefault();
	// Set the dropEffect to move
	ev.dataTransfer.dropEffect = "move"
}
function drop_handler(ev) {
	ev.preventDefault();
	var data = ev.dataTransfer.getData("text");
	ev.target.appendChild(document.getElementById(data));
}
<body>
	<p id="p1" draggable="true" ondragstart="dragstart_handler(event);">This element is draggable.</p>
	<div id="target" ondrop="drop_handler(event);" ondragover="dragover_handler(event);">Drop Zone</div>
</body>
```
##	拖动结束
在拖动目标元素上监听 dragend 事件，此时你可以做一起其他事情。
>	参考文档：
	[HTML5 drag & drop 拖拽与拖放简介](http://www.zhangxinxu.com/wordpress/2011/02/html5-drag-drop-%E6%8B%96%E6%8B%BD%E4%B8%8E%E6%8B%96%E6%94%BE%E7%AE%80%E4%BB%8B/)
	[HTML5 拖放（Drag 和 Drop）详解与实例](https://www.cnblogs.com/moqiutao/p/6365113.html)
	[DataTransfer](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)
	[Drag and Drop API](https://developer.mozilla.org/zh-CN/docs/Web/API/HTML_Drag_and_Drop_API)











