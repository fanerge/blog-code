---
title: CSS笔记
date: 2018-06-06 20:45:51
tags: 'css'
categories: 'css'
copyright: true
---
本文主要目的唤起你的记忆，如果你很熟悉该属性就当做复习，如果不熟悉，你就应该好好去查查文档了，毕竟本文只是点到即止的。
#  CSS方法
##  attr()
用来获取选择到的元素的某一HTML属性值，并用于其样式。
##  calc()
可以通过计算来决定一个CSS属性的值了。
PS：运算符前后都需要保留一个空格，例如：`width: calc(100% - 10px);`
##  counter
`counter-reset`属性创建或重置一个或多个计数器。
`counter-increment`属性递增一个或多个计数器值。
`counter(name)`方法用于获取计数器的值。
PS：`counter-reset`属性通常是和`counter-increment`属性，`content`属性一起使用。
##  cubic-bezier()
它主要作用于动画和过渡的运动曲线函数 animation-timing-function 和 transition-timing-function 。
[cubic-bezier转换网站](http://cubic-bezier.com/)
##  gradient
###  linear-gradient
线性渐变
`background-image: linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet);`
`repeating-linear-gradient` -- 重复线性渐变
`background: repeating-linear-gradient(to top left, lightpink, lightpink 5px, white 5px, white 10px);`
###  radial-gradient
径向渐变
`background: radial-gradient(red, yellow, rgb(30, 144, 255));`
`repeating-radial-gradient` -- 重复径向渐变
`background: repeating-radial-gradient(powderblue, powderblue 8px, white 8px, white 16px);`
##  image-set()
可以根据用户设备的分辨率匹配合适的图像。
`background-image: image-set( "test.png" 1x, "test-2x.png" 2x, "test-print.png" 600dpi );`
###  img 的 srcset属性
以逗号分隔的一个或多个字符串列表表明一系列用户代理使用的可能的图像。
`<img src="mm-width-128px.jpg" srcset="mm-width-128px.jpg 1x, mm-width-256px.jpg 2x">`
PS：img的srcset属性方便的解决了页面图片适应不同屏幕密度的情况。
##  matrix()
matrix() 指定了一个由指定的 6 个值组成的 2D 变换矩阵。这种矩阵的常量值是隐含的，而不是由参数传递的；其他的参数是以列优先的顺序描述的。
matrix3d() 参数为 9 个值，对应 3D 变换。
PS：所有的 transform 参数值都可以用矩阵来表示。
##  var()
var()函数可以代替元素中任何属性中的值的任何部分。
语法：`var( <custom-property-name> [, <declaration-value> ]? )`
ps：带有前缀--的属性名，比如--example--name，表示的是带有值的自定义属性，其可以通过 var 函数在全文档范围内复用的。

#  通用属性
##  backface-visibility
指定当元素背面朝向观察者时是否可见。元素的背面总是透明的，当其朝向观察者时，显示正面的镜像。
##  cross-fade
作用：CSS3背景图片透明叠加属性。
方法：`background-image: -webkit-cross-fade(url(1.jpg), url(2.jpg), 50%);`
PS：透明度是作用在第二张图片上的。
[Cross-fade](http://www.zhangxinxu.com/wordpress/2012/09/css3-background-image-cross-fade/)
##  caret-color
用来定义插入光标（caret）的颜色，这里说的插入光标，就是那个在网页的可编辑器区域内，用来指示用户的输入具体会插入到哪里的那个一闪一闪的形似竖杠 | 的东西。
##  clip-path
可以创建一个只有元素的部分区域可以显示的剪切区域（也就是说可以让一个元素显示出不同的形式，如圆形、椭圆、多边形等等）。
[CSS3 clip-path 用法详解](http://yunkus.com/css-clip-path/)
##  shape-outside
属性指定使用下面列表的值来定义浮动元素的浮动区域。这个浮动区域决定了行内内容（浮动元素）所包裹的形状。
其中还包括basic-shape有inset()、 circle()、ellipse()、polygon()
##  resolution
###  dpi
每英寸包含点的数量（dots per inch）
普通屏幕通常包含96dpi，一般将2倍于此的屏幕称之为高分屏，即大于等于192dpi的屏幕，比如Mac视网膜屏就达到了192dpi（即2dppx）。
###  dpcm
每厘米包含点的数量（dots per centimeter）
###  dppx
每像素包含点的数量（dots per pixel）
基本的换算单位
```
1dppx = 96dpi
1dpi ≈ 0.39dpcm
1dpcm ≈ 2.54dpi
1in = 2.54cm = 25.4 mm = 101.6q = 72pt = 6pc = 96px
```
##  empty-cells
渲染表格 table 中没有可见内容的单元格的边框和背景，取值为show 和 hide。
##  width
width属性又多了几个关键字成员，fill-available, max-content, min-content, 以及fit-content，兼容性还有很大问题，暂不深究。
###  max-content
固有的首选宽度.
###  min-content
这个关键字将解析为这个容器内部最大的不可断行元素的宽度（即最宽的单词、图片或具有固定宽度的盒元素）。
###  available
包含块的宽度减去水平 margin, border 和 padding.
###  fit-content 
以下两种情况下的较大值:固有的最小宽度 或 固有首选宽度（max-content）和可用宽度（available）的较小值  
###  border-box
之前的 length 或 percentage 应用到元素的边框盒子.
###  content-box
之前的 length 或 percentage 应用到元素的内容盒子.
##  hanging-punctuation 
指定了标点符号应该放在文本句子的开头还是结尾。悬挂标点符号可能被放在线框外。
##  hyphens
告知浏览器在换行时如何使用连字符连接单词。可以完全阻止使用连字符，也可以控制浏览器什么时候使用，或者让浏览器决定什么时候使用。
##  image-rendering
决定浏览器对缩放图像采取的缩放算法.它适用于元素本身和有其他属性的图像.它对非缩放图像没有影响。
##  image-orientation
用来修正某些图片的预设方向。
PS：该属性不是用来对图片进行任意角度旋转的, 它是用来修正那些带有不正确的预设方向的图片的. 因此该属性值会被四舍五入到 90 度的整数倍.
##  通用关键字
`inherit` -- 关键字使得元素获取其父元素的计算值(computed value )，当然肯定只针对可继承属性。
`initial` -- 是将属性的初始值( initial value)赋给元素，至于那些为[不同属性的初始值，请参见W3C](https://www.w3.org/TR/CSS2/propidx.html)。
`unset` -- CSS 关键字 unset 是 关键字 initial 和 inherit的组合（换句话说这个unset关键字会优先用inherit的样式，其次会应该用initial的样式）。
`all` -- CSS all简写属性重设除了unicode-bidi 和 direction 之外的所有属性至它们的初始值或继承值。
##  inline-size
定义元素的块的水平或垂直大小，这取决于它的写入模式。它对应于 width 或 height，取决于 writing-mode 属性。
##  isolation
定义该元素是否必须创建一个新的[stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)。
PS：该属性的主要作用是当和background-blend-mode属性一起使用时，可以只混合一个指定元素栈的背景：它允许使一组元素从它们后面的背景中独立出来，只混合这组元素的背景。
##  支持欠佳的属性，暂不深究
margin-block-start 和 margin-block-end
定义元素的逻辑块结束余量，该元素根据元素的writing-mode、方向性和文本方向映射到物理量度。
margin-inline-start 和 margin-inline-end
min-block-size 和 min-inline-size
offset-block-start 和 offset-block-end
offset-inline-start 和 offset-inline-end
padding-block-start 和 padding-block-end
padding-inline-end 和 padding-inline-start
##  mask
允许使用者通过部分或者完全隐藏一个元素的可见区域。这种效果可以通过遮罩或者裁切特定区域的图片。
遮罩mask是一个复合属性，包括mask-image、mask-mode、mask-repeat、mask-position、mask-clip、mask-origin、mask-size、mask-composite这8个属性
###  mask-image
默认值为none，值为透明图片，或透明渐变
###  mask-repeat
默认值为repeat，可选值与background-repeat相同
###  mask-position
默认值为0 0，可选值与background-position相同
###  mask-clip
默认值为border-box，可选值与background-clip相同
###  mask-origin
默认值为border-box，可选值与background-origin相同
###  mask-size
默认值为auto，可选值与background-size相同
###  mask-mode
默认值为match-source，可选值为alpha、luminance、match-source，或者它们的组合
###  mask-composite
默认值为add，可选值为add、subtract、intersect、exclude
##  mix-blend-mode
描述了元素的内容应该与元素的直系父元素的内容和元素的背景如何混合。
##  作用于替换元素，如img
###	object-fit
指定替换元素的内容应该如何适应到其使用的高度和宽度确定的框。
###  object-position
指定元素的替换内容在其盒子内的对齐方式。
##  overflow-wrap
是用来说明当一个不能被分开的字符串太长而不能填充其包裹盒时，为防止其溢出，浏览器是否允许这样的单词中断换行。
PS：word-wrap 属性原本属于微软的一个私有属性，在 CSS3 现在的文本规范草案中已经被重名为 overflow-wrap 。
##  pointer-events
指定在什么情况下 (如果有) 某个特定的图形元素可以成为鼠标事件的 target。
##  quotes
设置嵌套引用的引号类型。
PS：当4个参数时，前两个值规定第一级引用嵌套，后两个值规定下一级引号嵌套。前两个值规定第一级引用嵌套，后两个值规定下一级引号嵌套。
##  @supports
CSS 规则关联了一组嵌套的CSS语句,这些语句被放置在一个CSS区块中,该区块以大括号分割, 还有一个由多个CSS声明检测组成的条件,它是一个键值组合, 由逻辑与,逻辑或,逻辑非组合而成. 这样的条件语句称为支持条件.
语法：
```
@supports <supports_condition> {
  /* specific rules */
}
```
PS：在 supports_condition 中还支持 not、or、and 逻辑。
##  tab-size
用于自定义制表符 (U+0009) 的宽度。
##  table-layout
用于布局表格单元格，行和列的算法。
##  text-align-last
描述的是一段文本中最后一行在被强制换行之前的对齐规则。
##  text-combine-upright
文本结合 writing-mode（为vertical-rl 或 vertical-lr） 指定多个字符的组合到单个字符的空间中。
##  text-emphasis
主要效果为文本强调。
text-emphasis-color
text-emphasis-position
text-emphasis-style
##  text-orientation
设置文本的方向。
##  text-rendering 
CSS 属性定义浏览器渲染引擎如何渲染字体。浏览器会在速度、清晰度、几何精度之间进行权衡。
##  text-transform
CSS属性指定如何将元素的文本大小写。
##  text-underline-position
当 text-decoration属性的值设置为 underline 之后，可以用 text-underline-position 属性为其设置下划线的位置。
##  touch-action
用于指定某个给定的区域是否允许用户操作，以及如何响应用户操作（比如浏览器自带的划动、缩放等）。
##  transform-box
defines the layout box, to which the transform and transform-origin properties relate to.
##  transform-style
确定元素的子元素是否位于3D空间中，还是在该元素所在的平面内被扁平化。
##  unicode-bidi
CSS 的 unicode-bidi 属性和 direction 属性一起决定了如何处理文档中的双向文本（bidirectional text）。
## unicode-range 
属性值可以是单个字符编码、字符编码区间、通配符区间、多个值等，如小写字母：[0x61,0x7a]（或十进制[97, 122]）
CSS unicode-range 特定字符使用 font-face 自定义字体。
##  vertical-align
CSS 的属性 vertical-align 用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。
`baseline`
元素基线与父元素的基线对齐。
对于一些 可替换元素，比如 textarea ， HTML标准没有说明它的基线，这意味着对其使用这个关键字，各浏览器表现可能不一样。
`sub`
元素基线与父元素的下标基线对齐。
`super`
元素基线与父元素的上标基线对齐。
`text-top`
元素顶端与父元素字体的顶端对齐。
`text-bottom`
元素底端与父元素字体的底端对齐。
`middle`
元素中垂线与父元素的基线加上小写x一半的高度值对齐。
`length`
元素基线超过父元素的基线指定高度。可以取负值。
`percentage`
同 length , 百分比相对于 line-height 。
以下两个值是相对于整行来说的：
`top`
 元素及其后代的顶端与整行的顶端对齐。
`bottom`
元素及其后代的底端与整行的底端对齐。
##  will-change
CSS 属性 will-change 为web开发者提供了一种告知浏览器该元素会有哪些变化的方法，这样浏览器可以在元素属性真正发生变化之前提前做好对应的优化准备工作。
PS：这种优化可以将一部分复杂的计算工作提前准备好，使页面的反应更为快速灵敏。
##  writing-mode
定义了文本水平或垂直排布以及在块级元素中文本的行进方向。
##	增加热区的范围
1.	border 可以增加热区（与用户交互的区域），outline 和 box-shadow 是不能办到的。
2.	伪元素同样可以代表其宿主元素来响应鼠标交互。

通常的做法
```
border: 10px solid transparent;
```

#  filter(滤镜效果)
CSS滤镜（filter）属提供的图形特效，像模糊，锐化或元素变色。过滤器通常被用于调整图片，背景和边界的渲染。
filter 可以开启浏览器的硬件加速GPU，优化性能。
##  blur()
给图像设置高斯模糊。filter: blur(number);
##  brightness()
给图片应用一种线性乘法，使其看起来更亮或更暗。如果值是0%，图像会全黑。值是100%，则图像无变化。其他的值对应线性乘数效果。值超过100%也是可以的，图像会比原来更亮。如果没有设定值，默认是1。
##  drop-shadow()
给图像设置一个阴影效果。阴影是合成在图像下面，可以有模糊度的，可以以特定颜色画出的遮罩图的偏移版本。 
##  contrast()
调整图像的对比度。值是0%的话，图像会全黑。值是100%，图像不变。值可以超过100%，意味着会运用更低的对比。若没有设置值，默认是1。
##  grayscale()
将图像转换为灰度图像。值定义转换的比例。值为100%则完全转为灰度图像，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。若未设置，值默认是0。
##  hue-rotate()
给图像应用色相旋转。“angle”一值设定图像会被调整的色环角度值。
##  invert()
反转输入图像。值定义转换的比例。100%的价值是完全反转。值为0%则图像无变化。值在0%和100%之间，则是效果的线性乘子。 若值未设置，值默认是0。
##  opacity()
转化图像的透明程度。
##  saturate()
转换图像饱和度。值定义转换的比例。值为0%则是完全不饱和，值为100%则图像无变化。
##  sepia()
将图像转换为深褐色。值定义转换的比例。值为100%则完全是深褐色的，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。若未设置，值默认是0。
PS：你可以组合任意数量的函数来控制渲染。下面的例子可以增强图像的对比度和亮度。
`filter: contrast(175%) brightness(3%)`

#  背景
##  background
background 是CSS简写属性，用来集中设置各种背景属性。background 可以用来设置一个或多个属性:background-color, background-image, background-position, background-repeat,background-size, background-attachment。
##  background-attachment
如果指定了 background-image ，那么 background-attachment 决定背景是在视口中固定的还是随包含它的区块滚动的。
##  background-blend-mode
定义该元素的背景图片，以及背景色如何混合。
##  background-clip
置元素的背景（背景图片或颜色）是否延伸到边框下面。
PS：简写形式background:bg-color bg-image bg-position/bg-size bg-repeat bg-origin bg-clip bg-attachment initial|inherit;

#  伪类
##  :active
:active CSS伪类匹配被用户激活的元素。
PS：当多伪类同时使用时，需要注意顺序，否则就会发生被覆盖，如链接:link — :visited — :hover — :active。
##  :target
代表一个唯一的页面元素(目标元素)，其ID与当前URL片段匹配.
##  :default
表示一组相关元素中的默认表单元素。
`该选择器可以在 <button>, <input type="checkbox">, <input type="radio">, 以及 <option> 上使用。`
##  :dir
伪类匹配特定文字书写方向的元素。在HTML中, 文字方向由dir属性决定。
##  :enabled
表示任何启用的（enabled）元素。如果一个元素能够被激活（如选择、点击或接受文本输入）或获取焦点，则该元素是启用的。
##  :disabled  
表示任何被禁用的元素。如果一个元素不能被激活（如选择、点击或接受文本输入）或获取焦点，则该元素处于被禁用状态。
##  :read-only 
表示元素不可被用户编辑的状态（如锁定的文本输入框）。
##  :read-write
代表一个元素（例如可输入文本的 input元素）可以被用户编辑。
PS：这个选择器不仅仅选择 input 元素，它也会选择所有可以被用户编辑的元素，例如设置了 contenteditable 属性的 p 元素。
##  :empty 
代表没有子元素的元素。子元素只可以是元素节点或文本（包括空格），无论一个元素是否为 (empty 或 not), 注释或处理指令都不会产生影响。
##  :not()
是以一个简单的以选择器X为参数的功能性标记函数。它匹配不符合参数选择器X描述的元素。
PS：:not伪类不像其它伪类，它不会增加选择器的优先级。
##  :focus
表示获得焦点的元素（如表单输入）。当用户点击或触摸元素或通过键盘的 “tab” 键选择它时会被触发。
##  :fullscreen
应用于当前处于全屏显示模式的元素。 它不仅仅选择顶级元素，还包括所有已显示的栈内元素。
##  :in-range
代表一个 input 元素，其当前值处于属性min 和max 限定的范围之内.
##  :out-of-range
代表一个 input 元素，其当前值不在属性min 和max 限定的范围之内.
##  :indeterminate
表示状态不确定的表单元素.
##  :invalid
表示任意内容未通过验证的 input 或其他 form 元素 .
##  :valid
表示任意内容通过验证的 input 或其他 form 元素 .
##  :required
表示 任意 input 元素表示任意拥有required属性的 input 或 textarea 元素使用它. 它允许表单在提交之前容易的展示必填字段并且渲染其外观. 
##  :lang()
基于元素语言来匹配页面元素。
##  :optional
表示任意没有required属性的 input，select 或  textarea 元素使用它。
##  :only-child
代表了属于某个父元素的唯一一个子元素.
PS：等价的方法还有:first-child:last-child或者:nth-child(1):nth-last-child(1)
##  :root
这个 CSS 伪类匹配文档树的根元素。对于 HTML 来说，:root 表示 html 元素，除了优先级更高之外，与 html 选择器相同。
##  :scope
它将会匹配作为选择符匹配元素的参考点(css的作用域或作用点)。在HTML中，可以使用 style 的scoped属性来重新定义新的参考点。如果HTML中没有使用这个属性，那么默认的参考点(css的作用域或作用点)是 html。
##  scroll-behavior
当由于导航或者CSSOM滚动api产生滚动时，CSS属性 scroll-behavior 为一个滚动框指定滚动行为，其他任何的滚动，例如那些由于用户行为而产生的滚动，不受这个属性的影响。在根元素中指定这个属性时，它反而适用于视窗。
PS：可以配合 a 链接来实现平滑滚动到对应锚点位置。
[可以在Chrome浏览器中测试下](https://codepen.io/fanerge/pen/ERPELJ)
##  scroll-snap-type
CSS属性定义在滚动容器中的一个snap点如何被严格的执行。
PS：此属性不能用来指定任何精确的动画或者物理运动效果来执行snap点，而是交给用户代理来处理。
##  shape-outside
CSS 属性定义了一个行内内容应该包裹的形状。默认表现是行内元素包裹该形状的margin box。
##  shape-image-threshold
CSS property defines the alpha channel threshold used to extract the shape using an image as the value for shape-outside.
##  shape-margin
CSS property specifies a margin for a CSS shape created using shape-outside.

#  @page
@page 规则用于在打印文档时修改某些CSS属性。
你不能用@page规则来修改所有的CSS属性，而是只能修改margin,orphans,widow 和 page breaks of the document。对其他属性的修改是无效的。
##  size
指定页面盒模型所在的容器的大小和方向。一般情况下，因为一个页面盒模型被渲染到一面纸张上，所以这个属性也指示了目标纸张的大小。
##  marks
向文档添加剪切标记和/或注册标记。
##  bleed
指定一个超出页面盒模型的区域，在这个区域的页面内容将被裁剪。
##  :first
需要和 @page 配套使用，打印文档的时候，第一页的样式。
##  :left
需要和 @page 配套使用, 对打印文档的左侧页设置CSS样式.
##  :right
需要和 @page 配套使用, 对打印文档的右侧页设置CSS样式.
##  :blank
与 :empty 关系类似，浏览器支持不佳。
##  两个实验性
:recto 和 :verso 
##  page-break-after
CSS 属性调整当前元素之后的分页符
##  page-break-before
CSS属性调整当前元素之前的分页符。
##  page-break-inside
CSS 属性调整当前元素内的分页符。

#  伪元素
##  ::after
::after用来创建一个伪元素，做为已选中元素的最后一个子元素。通常会配合content属性来为该元素添加装饰内容。
PS：这个虚拟元素默认是行内元素。
##  ::fitst-letter
会选中某 block-level element（块级元素）第一行的第一个字母，并且文字所处的行之前没有其他内容（如图片和内联的表格） 。
PS：你可能还不知道，::before 伪元素 和 content 属性结合起来有可能会在元素前面注入一些文本。如此，::first-letter 将会匹配到content文本的首字母。
首行只在 block-container box内部才有意义, 因此 ::first-letter 伪元素 只在display属性值为block, inline-block, table-cell, list-item 或者 table-caption的元素上才起作用. 其他情况下, ::first-letter 毫无意义.
##  ::backdrop
是在任何处于全屏模式的元素下的即刻渲染的盒子（并且在所有其他在堆中的层级更低的元素之上）。
##  ::placeholder
可以选择一个表单元素的占位文本，它允许开发者和设计师自定义占位文本的样式。
##  ::selection 
应用于文档中被用户高亮的部分（比如使用鼠标或其他选择设备选中的部分）。
##  ::slotted()
CSS pseudo-element represents any element that has been placed into a slot inside an HTML template (see Using templates and slots for more information).

#  css3 布局
##  Flex 布局
place-content
[Study-Notes](https://github.com/fanerge/Study-Notes/blob/master/2017%E5%B9%B4/0320%E6%80%BB%E7%BB%93flex%E5%B8%83%E5%B1%80.txt)
##  grid 布局
###  Grid
通过设置 display: grid;  可以定义一个 CSS 网格。然后使用 grid-template-rows 和 grid-template-columns 属性来定义网格的 columns 和 rows。
PS：grid-template-rows 和 grid-template-columns 有较多种参数。
minmax(min, max) 
可以设置最小值和最大值，当某个值为 auto 时不限制。
repeat( [ <positive-integer> | auto-fill | auto-fit ] , <track-list> )
重复的多个 track，第一个参数指定了 repeat 的次数。
auto-fit
倾向于使用最少列数占满当前行空间，浏览器先是和 auto-fill 一样，暗中创建一些列来填充多出来的行空间，然后坍缩（collapse）这些列以便腾出空间让其余列扩张。
auto-fill
倾向于容纳更多的列，所以如果在满足宽度限制的前提下还有空间能容纳新列，那么它会暗中创建一些列来填充当前行。
###  Grid Areas
网格区域是网格中由一个或者多个网格单元格组成的一个矩形区域。当你使用基于网格线位置放置一个项目或者使用命名的网格区域定义区域时，网格区域被创建。
通常用 grid-area 属性命名它们（为子网格命名），然后用 grid-template-areas 把它们放在网格上。
[Grid Areas](https://developer.mozilla.org/zh-CN/docs/Glossary/Grid_areas)
###  Grid Lines
使用Grid布局在显式网格中定义轨道的同时会创建网格线。
网格线可以用它们的编号来寻址，线编号遵循文档的写入模式，因此在从右到左的语言中，列线1行将位于网格的右侧。
PS：主要给下列属性使用grid-column-start、grid-column-end、grid-row-start、grid-row-end。
[Grid Lines](https://developer.mozilla.org/zh-CN/docs/Glossary/Grid_lines)
###  Gutters
网格间距是网格轨道之间的间距，可以通过 grid-column-gap 或者 grid-row-gap 在Grid布局中创建。

#  动画和过渡和转换
##  animation
animation属性是如下属性的一个简写属性形式: animation-name, animation-duration, animation-timing-function, animation-delay, animation-iteration-count, animation-direction 和 animation-fill-mode.
##  animation-name
指定应用的一系列动画，每个名称代表一个由@keyframes定义的动画序列。
##  animation-delay
CSS属性定义动画于何时开始，即从动画应用在元素上到动画开始的这段时间的长度。
##  animation-direction
CSS 属性指示动画是否反向播放，它通常在简写属性animation中设定。
##  animation-fill-mode
用来指定在动画执行之前和之后如何给动画的目标应用样式。
##  animation-iteration-count
定义动画在结束前运行的次数 可以是1次 无限循环。
##  animation-play-state
定义一个动画是否运行或者暂停。可以通过查询它来确定动画是否正在运行。另外，它的值可以被设置为暂停和恢复的动画的重放。
##  animation-timing-function
定义CSS动画在每一动画周期中执行的节奏。
##  perspective 或 perspective()
指定了观察者与z=0平面的距离，使具有三维位置变换的元素产生透视效果。z>0的三维元素比正常大，而z<0时则比正常小，大小程度由该属性的值决定。
##  perspective-origin 
指定了观察者的位置，在属性perspective中被用作消失点。

#  百分比属性参照对象
##  参照包含块宽高
1.  参照包含块的width（margin、padding、width、left、right、font-size、text-index）
2.  参照包含块的height（height、top、bottom）

##  参照自身盒子宽高
1.  盒子模型中的border-radius
2.  背景中的background-size
3.  在transform变换中，translate()、transform-origin、scale()
[你知道我们平时在CSS中写的%都是相对于谁吗？](https://juejin.im/post/5b0bc994f265da092918d421)

#  长度单位
##  相对长度单位
###  em
相对长度单位，这个单位表示元素的font-size的计算值。如果用在font-size 属性本身，它会继承父元素的font-size。
###  ex
这个单位表示元素font的 x-height 。在含有“x”字母的字体中，它是该字体的小写字母的高度；对于很多字体， 1ex ≈ 0.5em。
###  ch
这一单位代表元素所用字体 font中“0”这一字形的宽度（“0”，Unicode字符U+0030），或更准确地说是“0”这一字形的预测尺寸（advance measure）。
###  rem
这个单位代表根元素的 font-size 大小（html 元素的font-size）。
###  lh
等于元素行高line-height的计算值。
###  rlh
等于根元素行高line-height的计算值。
##  视口比例的长度
###  vh
视口高度的 1/100。
###  vw
视口宽度的 1/100。
###  vi
等于初始包含块的大小的1%，在根元素的内联轴的方向上。
###  vb
等于初始包含块的大小的1%，在根元素的块轴的方向上。
###  vmin
视口高度和宽度之间的最小值的 1/100。
###  vmax
视口高度和宽度之间的最大值的 1/100。
##  绝对长度单位
###  px
对于屏幕显示，通常是一个设备像素（点）的显示。
###  mm
毫米。
###  cm
厘米（10毫米）。
###  in
英寸（2.54厘米）。
###  pt
磅（1/72 英寸）。
###  pc
12 点活字 (1 pc 等于 12 点)。


#  color
##  颜色关键字
black（黑） 、silver（银）、gray[*]（灰）、white（白）等等。
##  其他关键字
transparent 关键字，是 rgba(0,0,0,0) 的简写。
currentColor 关键字，取当前 color 的值。
## 颜色表达式
十六进制符号 #RRGGBB 
rgb(r, g, b)
rgba(r, g, b, a)
hsl(h, s, l) // 分别代表：色相、饱和度、亮度
hsla(h, s, l, a)

#  angle
用于表示角的大小，单位为度（degrees）、 百分度（gradians）、弧度（radians）或圈数（turns）。在 gradient 和 transform 的某些方法等场景中有所应用。
##  deg
度。一个完整的圆是 360deg。例：0deg，90deg，14.23deg。
##  grad
百分度。一个完整的圆是 400grad。例：0grad，100grad，38.8grad。
##  rad
弧度。一个完整的圆是 2π 弧度，约等于 6.2832rad。1rad 是 180/π 度。例：0rad，1.0708rad，6.2832rad。
##  turn
圈数。一个完整的圆是 1turn。例：0turn，0.25turn，1.2turn。

#	cusor（各属性使用的场景）
##	not-allowed
提示禁用状态，如按钮禁用、禁止拖动
```
:disabled, [disabled], [aria-disabled="true"] {
	cursor: not-allowed;
}
```
##	 none
隐藏鼠标光标，如播放视频等
```
video {
	cursor: url(transparent.gif); // 兼容
	cursor: none;
}
```














  