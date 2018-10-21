---
title: Chrome调试
date: 2018-05-24 22:13:30
tags: '调试'
categories: '调试'
copyright: true
---
记录Chrome DevTools比较少用但又很重要的技巧。
#  检查动画
使用Chrome DevTools Animations(动画)检查器检查和修改动画。
功能：
通过打开Animation Inspector(动画检查器)捕获动画。它会自动检测动画并将它们分组。
通过减慢动画，重播动画，或查看源代码，来检查动画。
通过更改动画的时间，延迟，持续时间或关键帧偏移来修改动画。

#  设置DOM断点
设置DOM断点可以用来调试复杂的JavaScript应用程序。例如，如果你的JavaScript改变了DOM元素的样式，设置一个DOM断点当元素的属性被修改时触发。
在以下DOM更改都会触发断点：子树的变化，属性改变，节点删除。
设置DOM断点
Elements --> Break on --> Subtree Modifications/Attributes Modifications/Node Removal
查看DOM断点（包含断点类型）
Elements --> DOM Breakpoints

#  查看元素事件监听器
在Event Listeners(事件侦听器)窗格中查看与DOM节点相关联的JavaScript事件。
查看事件
Elements --> Event Listeners
当取消勾选Framework listeners(框架侦听器)复选框时，事件侦听器代码可能会解析框架或库代码中的某处。

#  模拟传感器
Main menu --> More Tools --> Sensors
模拟地理位置坐标以测试地理位置覆盖。
模拟设备方向来测试加速计数据。

#  在XHR上中断
有两种方法可以触发XHR上的断点：当任何XHR到达XHR生命周期的某个阶段时（readystatechange，load等），或者当XHR的URL与某个字符串匹配时。

#  调试杂项
命名函数可以提高调用堆栈的可读性，不限于回掉函数。
##  把第三方代码放入Blackbox(黑箱)例如第三方库：jQuery、React等
1.打开 DevTools Settings(设置)。
2.在左侧的导航菜单中，单击Blackboxing(黑箱)。
3.点击Add pattern...(添加模式)按钮。
4.在Pattern(模式)文本框输入您希望从调用堆栈中排除的文件名模式。DevTools 会排除该模式匹配的任何脚本。
5.在文本字段右侧的下拉菜单中，选择Blackbox(黑箱)以执行脚本文件但是排除来自调用堆栈的调用，或选择6.Disabled(禁用)来阻止文件执行。
7.点击 Add(添加) 保存。
下次运行页面并触发断点时，DevTools 将在Call Stack(调用堆栈)中隐藏任何来自放入黑盒脚本函数的调用。
[把第三方代码放入Blackbox](http://www.css88.com/doc/chrome-devtools/javascript/step-code/)
管理线程执行
使用Sources(源文件)面板上的Threads(线程)窗格暂停，step into(步入)，并检查其他线程，例如service worker 或 web worker 线程。
##  启动 JavaScript CPU 状态分析
启动一个JavaScript CPU 状态分析，可以添加一个可选标签名。要停止分析，请调用console.profileEnd()。每个分析结果都将添加到Profiles(分析)面板。
console.profile([label])
console.profileEnd();
在调用该方法的地方打印堆栈跟踪。
console.trace(object)

#  监听事件
monitorEvents()方法指示DevTools记录指定目标事件的信息。
monitorEvents(document.body, "click");
要停止监听事件，请调用unmonitorEvents()方法,传递一个停止监视对象的参数。
unmonitorEvents(document.body);
查看在对象上注册事件监听器
getEventListeners() API返回在指定对象上注册事件的监听器。
返回值是一个对象，其中包含每个已注册事件类型的数组（例如，click 或 keydown）。 每个数组的成员都是对象，描述每中类型的已注册监听器。

#	# Chrome 滚动截屏
##  Chrome 截全屏（PC）
1.  打开控制台  Alt + Command+ I (Mac) 或 Ctrl + Shift + I (Windows)
2.  功能搜索  Command + Shift + P(Mac) 或 Ctrl + Shift + P (Windows)
3.  输入“Screen” 并选择 Capture full size screenshot 功能即可

##  Chrome 截全屏（移动端）
只需在 toggle device toolbar 选择移动端
进行上面2的操作即可
##  部分截屏
只需在选择对应的DOM基础上
进行上面2的操作即可
[如何利用 Chrome 浏览器实现滚动截屏](https://weibo.com/ttarticle/p/show?id=2309404241869646237445)

#	Chrome 模拟移动端降低CPU及网速
Performance -> setting -> CPU/Network


>	参考文档：
[参考](http://www.css88.com/doc/chrome-devtools/)








