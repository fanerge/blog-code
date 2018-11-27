---
title: Web性能优化
date: 2018-07-13 22:33:31
tags: '性能'
categories: '性能'
copyright: true
---
#	页面渲染的4个关键指示
##	First Paint（FP）
仅有一个需要挂载的根节点。
##	First Contentful Paint（FCP）
包含页面的基本框架，但没有数据内容。
##	First Meaningful（FMP）
包含页面所有元素及数据。
##	Time to Interactive（TTI）
允许交互时间。
#  预加载技术
##  DNS prefetch
DNS prefetching通过指定具体的URL来告知客户端未来会用到相关的资源，这样浏览器可以尽早的解析DNS。
`<link rel="dns-prefetch" href="//example.com">`
##  Preconnect 
和DNS prefetch类似，preconnect不光会解析DNS，还会建立TCP握手连接和TLS协议（如果需要）。
`<link rel="preconnect" href="http://css-tricks.com">`
##  Prefetch
当能确定网页在未来一定会使用到某个资源时，开发者可以让浏览器提前请求并且缓存好以供后续使用，prefetch支持预拉取图片、脚本或者任何可以被浏览器缓存的资源。
`<link rel="prefetch" href="image.png">`
PS：可以解决字体文件必须等DOM和CSSOM创建好后才能下载的性能瓶颈。
##  Subresource
可以用来指定资源是最高优先级的。
`<link rel="subresource" href="styles.css">`
PS：rel=prefetch指定了下载后续页面用到资源的低优先级，而rel=subresource则是指定当前页面资源的提前加载。
##  Prerender
prerender是一个重量级的选项，它可以让浏览器提前加载指定页面的所有资源。
一些适用场景用户搜索后的结果页面、登录成功后的页面、多页文章（预先加载下一页的资源）
`<link rel="prerender"  href="/thenextpage.html" />`
PS：该属性会下载所有的资源、创建DOM、渲染页面、执行JS[这篇文章可以防止页面还没有展示给用户就出发JS的执行](https://www.w3.org/TR/page-visibility/)等等。
##  Preload
允许始终预加载某些资源，不像prefetch有可能被浏览器忽略，浏览器必须请求preload标记的资源。
`<link rel="preload" href="image.png">`
[AlloyTeam-一箩筐的预加载技术](http://www.alloyteam.com/2015/10/prefetching-preloading-prebrowsing/)

#	微任务（jobs）和宏任务（task）
微任务包括：process.nextTick，原生Promise(有些实现的promise将then方法放到了宏任务中)，Object.observe(已废弃)，MutationObserver
宏任务包括：script，setTimeout，setInterval，setImmediate，I/O ，UI rendering，MessageChannel 

#	如何生成新图层（避免重排影响其他图层）
通过以下几个常用属性可以生成新图层
3D 变换：translate3d、translateZ
will-change
video、iframe 标签
通过动画实现的 opacity 动画转换
position: fixed

#	React 16 加载性能优化
[React 16 加载性能优化指南](https://juejin.im/entry/5b506b315188251b24382faa)

#	CSS性能优化技巧
##	内联首屏关键CSS（Critical CSS）
即为只将渲染首屏内容所需的关键CSS内联到HTML中。
[github-首屏的关键样式提取](https://github.com/filamentgroup/criticalCSS)
##	异步加载CSS（四种方式）
第一种方式是使用JavaScript动态创建样式表link元素，并插入到DOM中。
第二种方式是将link元素的media属性设置为用户浏览器不匹配的媒体类型甚至不存在的类型都可以，对浏览器来说，如果样式表不适用于当前媒体类型，其优先级会被放低，会在不阻塞页面渲染的情况下再进行下载。
`
<link rel="stylesheet" href="mystyles.css" media="noexist" onload="this.media='all'">
`
第三种方式是通过rel属性将link元素标记为alternate可选样式表，也能实现浏览器异步加载。
`
<link rel="alternate stylesheet" href="mystyles.css" onload="this.rel='stylesheet'">
`
第四中方式是rel=”preload”5这一Web标准指出了如何异步加载资源，包括CSS类资源。
`
<link rel="preload" href="mystyles.css" as="style" onload="this.rel='stylesheet'">
`
PS：第四中为标准方案，as 属性是必须的。
##	去除无用CSS
[github-去除无用CSS](https://github.com/uncss/uncss)
##	有选择地使用选择器
CSS选择器的匹配是从右向左进行的。

#	如何启用GPU硬件加速
##	什么情况形成新的层 layer？（作用为重绘时只影响该层，不影响层外的元素。）
1.	3D 或者 CSS的transform属性
2.	video 和 canvas 元素
3.	CSS的filter属性
4.	覆盖在其它元素之上的元素，比如通过z-index提升层级

##	哪些属性直接在GPU处理？
transform
opacity
filter
transform: translateZ(0); // 可以强制GPU渲染
##	使用硬件加速需要注意的地方？
###	Memory
GPU处理过多的内容会导致内存问题。这在移动端和移动端浏览器会导致崩溃。因此，通常不会对所有的元素使用硬件加速。
###	Font rendering
在GPU渲染字体会导致抗锯齿无效。这是因为GPU和CPU的算法不同。因此如果你不在动画结束的时候关闭硬件加速，会产生字体模糊。
###	浏览器的优化
通常配合will-change属性使用
```
.example {
	transform: rotate(1turn);
	will-change: transform;
}
```
PS：这个功能允许你告诉浏览器这个属性会发生变化，因此浏览器会在开始之前对其进行优化。
[在 CSS 动画中使用硬件加速](https://juejin.im/post/5b6143996fb9a04fd343ae28)













