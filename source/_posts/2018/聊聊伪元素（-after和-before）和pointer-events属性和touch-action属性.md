---
title: '聊聊伪元素（::after和::before）、pointer-events属性、touch-action属性'
date: 2018-07-06 20:00:32
tags: 'css'
categories: 'css'
copyright: true
---
#  伪元素（以::after举例）
##  属性基础介绍
定义：::after用来创建一个伪元素，做为已选中元素的最后一个子元素。通常会配合content属性来为该元素添加装饰内容。这个虚拟元素默认是行内元素。
使用：CSS2.0为`element:after{content: '';...}`；CSS3.0为`element::after{content: '';...}`
这些并不是我今天想讨论的，我们应该来看看伪元素的一些其他性质。
##  补充知识
本文的重点：我们都知道我们不能为伪元素添加事件（目前是这样，不知道以后W3C会不会考虑），但是伪元素是可以触发元素本体上的事件。我认为伪元素的出现使我们在某种程度上可以精简DOM结构（伪元素可以达到补充、完善UI的目的），但是由于会触发元素本体上的事件，可能有些地方使用需要考虑一下了（在DOM简化的微小性能优化和是否适用于该场景中权衡一下）。
PS：元素本体：为了文章的好理解，我自己起的名字，意思为某个元素除了虚拟元素（::after等）以外的部分。
比如为了为了实现下图UI，用元素的 after伪类 实现登录和注册分割中的‘点.’，但这个 after伪类保留了 span标签 登录事件感觉有点不符合UI语义。
![after伪类的使用](http://pau044s3z.bkt.clouddn.com/after%E4%BC%AA%E7%B1%BB%E4%BD%BF%E7%94%A8.png)

#  CSS之pointer-events属性
我这里只讨论 pointer-events 的auto、none、inherit、initial、unset（不讨论SVG相关的属性值）。
##  属性基础介绍
定义：指定在什么情况下 (如果有) 某个特定的图形元素可以成为鼠标事件的 target，也就是说通过设置该属性可以控制该元素是否为事件的target。
对上面提到的属性值做下简单的介绍：
auto：  与 pointer-events 属性未指定时的表现效果相同
none：  元素永远不会成为鼠标事件的 target（不一定意味着元素上的事件侦听器永远不会触发，后面在做介绍）。
inherit：  该关键字使得元素获取其父元素的计算值(computed value )，由于该属性为非继承属性 inherit 在这里指定的行为通常没有多大意义，一般使用使用 initial 或 unset 作为替代。 
initial：  相关标准都设置了每个元素的的初始值，pointer-events 属性的初始值为 auto 。 
unset：  关键字 unset 是 关键字 initial 和 inherit的组合（如果有继承父级样式，则将该属性重新设置为继承的值，如果没有继承父级样式，则将该属性重新设置为初始值）。
##  补充知识
还记得我们刚刚讲到 pointer-events 的属性值为 none 时提到“不一定意味着元素上的事件侦听器永远不会触发，后面在做介绍”，这里开始做说明。
>  使用pointer-events来阻止元素成为鼠标事件目标不一定意味着元素上的事件侦听器永远不会触发。如果元素后代明确指定了pointer-events属性并允许其成为鼠标事件的目标，那么指向该元素的任何事件在事件传播过程中都将通过父元素，并以适当的方式触发其上的事件侦听器。当然，位于父元素但不在后代元素上的鼠标活动都不会被父元素和后代元素捕获（鼠标活动将会穿过父元素而指向位于其下面的元素）。

###  该属性提高页面滚动时候的绘制性能是不准确的
为什么这么说呢？已经有前辈写过相应的文章，以及详细的测试数据[pointer-events:none提高页面滚动时候的绘制性能？](https://www.zhangxinxu.com/wordpress/2014/01/pointer-events-none-avoiding-unnecessary-paints/)
###  只能失效鼠标事件，并不能让键盘事件失效
pointer-events 只能失效鼠标事件，并不能让键盘事件失效，所以用该属性对按钮的禁用需要小心[大家可以看看张鑫旭前辈的文章](https://www.zhangxinxu.com/wordpress/2011/12/css3-pointer-events-none-javascript/)。

# CSS之touch-action属性 
##  属性基础介绍
定义：用于指定某个给定的区域是否允许用户操作，以及如何响应用户操作（比如浏览器自带的划动、缩放等）。
属性值介绍：
auto：  当触控事件发生在元素上时，由浏览器来决定进行哪些操作，比如对viewport进行平滑、缩放等。
none：  当触控事件发生在元素上时，不进行任何操作。
pan-x：  启用单指水平平移手势。可以与 pan-y 、pan-up、pan-down、pinch-zoom 组合使用。
pan-y：  启用单指垂直平移手势。可以与 pan-x 、pan-left 、pan-right、pinch-zoom 组合使用。
manipulation：  浏览器只允许进行滚动和持续缩放操作。任何其它被auto值支持的行为不被支持。启用平移和缩小缩放手势，但禁用其他非标准手势，例如双击以进行缩放。 禁用双击可缩放功能可减少浏览器在用户点击屏幕时延迟生成点击事件的需要。
pinch-zoom：  启用多手指平移和缩放页面。 这可以与任何平移值组合。
##  补充知识
移动端300ms延迟，就可以使用 touch-action: manipulation;因为 manipulation 会禁用双击事件（浏览器就不需要等待300ms之后来判断了）。
处理移动端点击延迟代码
```
html {
  touch-action: manipulation;
}
```
###  实践应用
1.  在地图或游戏开发中，最常见的用法是禁用元素（及其不可滚动的后代）上的所有手势，以使用自己提供的拖放和缩放行为。
```
#map {
  touch-action: none;
}
```
2.  水平图像轮播开发中，只想通过水平滑动但不想干扰网页的垂直滚动或缩放，可用以下代码。
```
.image-carousel {
  width: 100%;
  height: 150px;
  touch-action: pan-y pinch-zoom;
}
```
