
周末看了CSS2019开发者大会的视频，受益匪浅。本文记录大部分视频内容以及自己的理解，如有理解不当的地方，还望指出。
### 为什么块格式自动 margin 不垂直居中元素？
如果 margin-left 和 margin-right 都设置为 auto，则他们两个值相等，所以水平能够居中。
如果 margin-top 和 margin-bottom 都设置为 auto，则他们实际等于0，所以就不能垂直居中。
[CSS5th分享原文](https://www.chenhuijing.com/slides/53-cssconfcn-2019/#/18)
### 移动端设置 viewport 的两种写法
meta 标签
```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
CSS(暂不可用)
```
@viewport {
    width: extend-to-zoom 100%;
    height: auto;
    zoon: 1.0;
}
```
### 滚动捕捉（实验性）
scroll-snap-type // 定义在滚动容器中的一个snap点如何被执行。
scroll-snap-align
scroll-margin // 定义滚动捕捉区域的开始，该区域用于将此框捕捉到snapport。
scroll-padding
### 自定义属性（非CSS变量）
声明属性(--开头)
```
// 任何元素都可以使用
:root {
    --gutter: 1em;
}
// 局部可使用
#side div {
    --gutter: 1em;
}
```
使用($开头)
```
.container {
    padding: $gutter;
}
```

### CSS Houdini（Js in CSS）
CSS Houdini 是各大厂商的工程师所组成的工作小组，志在建立一系列的 API，让开发者能够介入浏览器的 CSS engine 操作，帶给开发者更多的解决方案。

### 以内容为主的尺寸计算方式
外部尺寸： 根据元素的上下文确定大小，而不考虑其内容。
如：width: 400px;
内部尺寸： 根据元素的内容确定大小，而不考虑其上下文。
如：widht: min-content;
[w3c#intrinsic-sizes](https://www.w3.org/TR/css-sizing-3/#intrinsic-sizes)
利用@supports，本地CSS功能检测
```
.selector {
  /* Styles that are supported in old browsers */
}

@supports (property:value) {
  .selector {
    /* Styles for browsers that support the specified property */
  }
}
```

### 混合模式和滤镜
mix-blend-mode // 描述了元素的内容应该与元素的直系父元素的内容和元素的背景如何混合。
background-blend-mode // 定义该元素的背景图片和背景色如何混合。
isolation // 定义该元素是否必须创建一个新的stacking context。
filter // 将滤镜效果应用于元素。
isolation 属性的主要作用是当和background-blend-mode属性一起使用时，可以只混合一个指定元素栈的背景：它允许使一组元素从它们后面的背景中独立出来，只混合这组元素的背景。

### 镂空
矩形镂空
outline: 999px solid rgba(0, 0, 0, .5);
椭圆镂空
box-shadow: 0 0 0 9999px rgba(0, 0, 0, .5);
不规则镂空
mask-composite


### mask
允许使用者通过部分或者完全隐藏一个元素的可见区域。这种效果可以通过遮罩或者裁切特定区域的图片。
具体有下列属性
mask-image
mask-mode
mask-repeat
mask-position
mask-clip
mask-origin
mask-size
mask-type
mask-composite
[CSS遮罩CSS3 mask/masks详细介绍](https://www.zhangxinxu.com/wordpress/2017/11/css-css3-mask-masks/)

### transition && animation
要创建一个 transition,浏览器需要看到样式的变化
1.  使用 requestAnimationFrame 来让浏览器看到样式变化
```
button.onclick = () => {
  // Make the panel
  const panel = document.createElement('div');
  ...

  // Stretch it in
  panel.style.transform = 'scale(0)';
  panel.style.transition = 'transform .5s';
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      panel.style.transform = 'scale(1)';
    });
  });
};
```
2.  使用 getComputedStyle(elem).property 来让浏览器看到样式变化
```
button.onclick = () => {
  // Make the panel
  const panel = document.createElement('div');
  ...

  // Stretch it in
  panel.style.transform = 'scale(0)';
  getComputedStyle(panel).transform;
  panel.style.transition = 'transform .5s';
  panel.style.transform = 'scale(1)';
};
```
虽然 getComputedStyle(elem) 不会更新样式，但是 getComputedStyle(elem).property 会更新样式。
使用 getComputedStyle 的代价很高
transitionend
transitionend 事件会在 CSS transition 结束后触发。
但下列情况不会出发
当 transition 完成前移除 transition 时，比如移除css的transition-property 属性，事件将不会被触发。
当 transition 完成前设置 display 为"none"，事件同样不会被触发。
当 transition 完成前元素被移除不会触发。
为了防止以上 transition 被取消
setTimeout 将其添加到异步任务队列中。
transitioncancel 事件。
还有 transitionrun 事件（需确保一开始是有 transition 的）。

animation
animation-timing-function只在关键帧之间 适用
WEB ANIMATIONS
```
ele.animate(
  {
    transform: ['scale(1)', 'scale(2)'],
    opacity: [1, 0],
  },
  {
    duration: 3000,
    iterations: Infinity,
    easing: 'cubic-bezier(0.7, 0.0, 1.0, 1.0)',
  }
);
```
特性检查（html.animate）
```
if ('animate' in elem) {
  // ... Animate away...
}
```
[polyfill (web-animations-js)](https://github.com/web-animations/web-animations-js)
[Animating like you just don’t care with Element.animate](https://hacks.mozilla.org/2016/08/animating-like-you-just-dont-care-with-element-animate/)
尽量只对 transform 和 opacity 使用动画。
根据用户的偏好，禁用复杂动画，媒体查询 prefers-reduced-motion 可偏向于用户。
移动端一般动画和过渡时间把握在300ms，根据屏幕大小适当调整。

##  其他属性
text-fill-color
设置字体的填充颜色，如果未设置此属性，则字体颜色为 color 属性的值。
text-fill-color 与 color 的区别
text-fill-color 只设置字体的颜色，color 设置所有的前景色。相当于 text-fill-color 为 color 的子集。
background-clip 
置元素的背景（背景图片或颜色）是否延伸的范围（border-box/padding-box/content-box/text等）。
box-decoration-break
指定一个元素的片段时、多行、多列或页面断应该呈现形式（ 默认slice，clone独立呈现）。
clip-path
该属性可以创建一个只有元素的部分区域可以显示的剪切区域（circle、ellipse、polygon等）。
object-fit
指定可替换元素的内容应该如何适应到其使用的高度和宽度确定的框。
可替换元素有哪些？iframe、video、embed、img等
writing-mode
设置文本行如何被布置（水平或垂直），以及其中块前进方向。
shape-outside
shape-outside的CSS 属性定义了一个可以是非矩形的形状，相邻的内联内容应围绕该形状进行排列。
CSS Regions && CSS Exclusions


