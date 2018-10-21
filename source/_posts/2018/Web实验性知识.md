---
title: Web实验性知识
date: 2018-08-04 13:10:44
tags: 'html,css,js'
categories: '杂项'
copyright: true
---
#	伪类:placeholder-shown
##	定义
CSS 伪类在 input 或 textarea 元素显示 placeholder text 时生效。
PS：可以配合 :not() 伪类等配合，优化表单。

#	伪:focus-within
##	定义
CSS 伪类，表示一个元素获得焦点或该元素的后代元素获得焦点。换句话说，元素自身或者它的某个后代匹配:focus伪类。
[神奇的选择器 :focus-within](https://github.com/chokcoco/iCSS/issues/36)

#	display:contents
##	定义
元素本身不产生任何边界框，而元素的子元素与伪元素仍然生成边界框，元素文字照常显示。为了同时照顾边界框与布局，处理这个元素时，要想象这个元素不在元素树型结构里，而只有内容留下。这包括元素在原文档中的子元素与伪元素，比如::before和::after这两个伪元素，如平常一样，前者仍然在元素子元素之前生成，后者在之后生成。
[MDN-placeholder-shown](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:placeholder-shown)
[五个最新的CSS特性以及如何使用它们](https://zhuanlan.zhihu.com/p/40736286)

#	contain
##	定义
contain 属性允许开发者声明当前元素和它的内容尽可能的独立于 DOM 树的其他部分。这使得浏览器在重新计算布局、样式、绘图或它们的组合的时候，只会影响到有限的 DOM 区域，而不是整个页面。
[MDN-contain](https://developer.mozilla.org/zh-CN/docs/Web/CSS/contain)


























