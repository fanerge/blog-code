---
title: canvas开发参考手册
date: 2018-01-03 21:20:06
tags: 'html5'
categories: 'html5'
copyright: true
---
<iframe src="https://fanerge.github.io/canvas_solar_system/" width="700" height="350" frameborder="0">
#	介绍
[学完canvas的相关知识可以做一些小动画了](https://fanerge.github.io/canvas_solar_system/)
最早由Apple引入WebKit，用于Mac OS X 的 Dashboard。
canvas 是 HTML5 新增的元素，可用于通过使用JavaScript中的脚本来绘制图形。例如，它可以用于绘制图形，制作照片，创建动画，交互式游戏，甚至可以进行实时视频处理或渲染。
#	基本用法
canvas 它是一个元素，当然具有元素通用的属性，如id、class等。
##	渲染上下文（The rendering context）
canvas 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容。
```
// 获取渲染上下文
let canvas = document.querySelector('#canvas')
let ctx = canvas.getContext('2d')
```
##	检查支持性
```
if (canvas.getContext) {
	// 支持
} else {
	// 不支持
}
```
#	绘制形状
##	矩形
fillRect(x, y, width, height)
	绘制一个填充的矩形
strokeRect(x, y, width, height)
	绘制一个矩形的边框
clearRect(x, y, width, height)
	清除指定矩形区域，让清除部分完全透明。
	常用于清理画布。
##	路径
图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。
1.	首先，你需要创建路径起始点。
2.	然后你使用画图命令去画出路径。
3.	之后你把路径封闭。
4.	一旦路径生成，你就能通过描边或填充路径区域来渲染图形。
beginPath() 
	新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
closePath()
	闭合路径之后图形绘制命令又重新指向到上下文中。
stroke() -- 轮廓
	通过线条来绘制图形轮廓。
fill() -- 整体
	通过填充路径的内容区域生成实心的图形。
PS：fill会自动闭合儿stroke不会。
moveTo(x, y) -- 移动笔触
	将笔触移动到指定的坐标x以及y上。
	当canvas初始化或者beginPath()调用后，你通常会使用moveTo()函数设置起点。我们也能够使用moveTo()绘制一些不连续的路径
lineTo(x, y) -- 直线
	绘制一条从当前位置到指定x以及y位置的直线。
arc(x, y, radius, startAngle, endAngle, anticlockwise) -- 圆弧
	画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
arcTo(x1, y1, x2, y2, radius)
	根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。
PS：角度是以x轴为基准且为弧度，转化公式：radians=(Math.PI/180)*degrees
quadraticCurveTo(cp1x, cp1y, x, y) -- 二次贝塞尔曲线
	绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y) -- 三次贝塞尔曲线
	绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。
PS：贝塞尔曲线都会以开始路径作为起点，实际上二次贝塞尔曲线由3个点控制，N次贝塞尔曲线由n+1个点控制。
[wiki-贝塞尔曲线](https://zh.wikipedia.org/wiki/%E8%B2%9D%E8%8C%B2%E6%9B%B2%E7%B7%9A)
绘制矩形的额外方法
rect(x, y, width, height)
	绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。	
###	Path2D 对象	
为了简化代码和提高性能，Path2D对象已可以在较新版本的浏览器中使用，用来缓存或记录绘画命令，这样你将能快速地回顾路径。	
Path2D()
Path2D()会返回一个新初始化的Path2D对象（可能将某一个路径作为变量——创建一个它的副本，或者将一个包含SVG path数据的字符串作为变量）。	
Path2D.addPath(path [, transform])​
	添加了一条路径到当前路径（可能添加了一个变换矩阵）。
```
// 使用 SVG paths
var p = new Path2D("M10 10 h 80 v 80 h -80 Z");
```
#	使用样式和颜色	
##	色彩 Colors	
fillStyle = color
	设置图形的填充颜色。	
strokeStyle = color
	设置图形轮廓的颜色。	
PS：orange、#ffa500、rgb(255, 165, 0)、rgba(255, 165, 0, 1)、hsl(360, 50%, 50%)、hsla(360, 50%, 50%, 1)
##	透明度 Transparency	
globalAlpha = transparencyValue
	这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0。
ctx.strokeStyle = "rgba(255,0,0,0.5)";
ctx.fillStyle = "rgba(255,0,0,0.5)";	
##	线型 Line styles
可以通过一系列属性来设置线的样式。	
lineWidth = value
	设置线条宽度。
	线宽是指给定路径的中心到两边距离之和的粗细。换句话说就是在路径的两边各绘制线宽的一半。	
lineCap = type
	设置线条末端样式。
	butt，round 和 square。
	默认是 butt。
lineJoin = type
	设定线条与线条间接合处的样式。
	round, bevel 和 miter。
	默认是 miter。
miterLimit = value
	限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度。	
getLineDash()
	返回一个包含当前虚线样式，长度为非负偶数的数组。
	[a, b] a表示实线，b表示空白，这样交替出现。
setLineDash(segments)
	设置当前虚线样式。
lineDashOffset = value
	设置虚线样式的起始偏移量。
##	渐变 Gradients（新建的渐变对象）
###	线性渐变
let lineargradient = createLinearGradient(x1, y1, x2, y2)	
	createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)。
###	径向渐变	
let radialgradient = createRadialGradient(x1, y1, r1, x2, y2, r2)
	createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。
gradient.addColorStop(position, color)	
	addColorStop 方法接受 2 个参数，position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间。color 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）。
##	图案样式 Patterns
createPattern(image, type)
	该方法接受两个参数。Image 可以是一个 Image 对象的引用，或者另一个 canvas 对象。Type 必须是下面的字符串值之一：repeat，repeat-x，repeat-y 和 no-repeat。
	你需要确认 image 对象已经装载(onload)完毕，否则图案可能效果不对的。
##	阴影 Shadows
shadowOffsetX = float
	shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
shadowOffsetY = float
	shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
shadowBlur = float
	shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。
shadowColor = color
	shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。
##	Canvas 填充规则
当我们用到 fill（或者 clip和isPointinPath ）你可以选择一个填充规则，该填充规则根据某处在路径的外面或者里面来决定该处是否被填充，这对于自己与自己路径相交或者路径被嵌套的时候是有用的。
"nonzero": 默认值.
"evenodd"
#	绘制文本
canvas 提供了两种方法来渲染文本:
fillText(text, x, y [, maxWidth])
	在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.
strokeText(text, x, y [, maxWidth])
	在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.
##	有样式的文本
font = value
	当前我们用来绘制文本的样式. 这个字符串使用和 CSS font 属性相同的语法. 默认的字体是 10px sans-serif。
textAlign = value
	文本对齐选项. 可选的值包括：start, end, left, right or center. 默认值是 start。
textBaseline = value
	基线对齐选项. 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。默认值是 alphabetic。
direction = value
	文本方向。可能的值包括：ltr, rtl, inherit。默认值是 inherit。
##	先进的文本测量
当你需要获得更多的文本细节时，下面的方法可以给你测量文本的方法。
measureText()
	将返回一个 TextMetrics对象的宽度、所在像素，这些体现文本特性的属性。
`
var text = ctx.measureText("foo"); // TextMetrics object
text.width; // 16;
`	
#	使用图像 Using images	
canvas更有意思的一项特性就是图像操作能力。可以用于动态的图像合成或者作为图形的背景，以及游戏界面（Sprites）等等。
引入图像到canvas里需要以下两步基本操作：	
1.	获得一个指向HTMLImageElement的对象或者另一个canvas元素的引用作为源，也可以通过提供一个URL的方式来使用图片（参见例子）
2.	使用drawImage()函数将图片绘制到画布上
##	获得需要绘制的图片	
HTMLImageElement
	这些图片是由Image()函数构造出来的，或者任何的<img>元素
HTMLVideoElement
	用一个HTML的 <video>元素作为你的图片源，可以从视频中抓取当前帧作为一个图像
HTMLCanvasElement
	可以使用另一个 <canvas> 元素作为你的图片源。
ImageBitmap
	这是一个高性能的位图，可以低延迟地绘制，它可以从上述的所有源以及其它几种源中生成。
PS：这些源统一由 CanvasImageSource类型来引用。	
1.	使用相同页面内的图片	
document.images集合
document.getElementsByTagName()方法
document.getElementById()获得这个图片	
2.	使用其它域名下的图片
在 HTMLImageElement上使用crossOrigin属性，你可以请求加载其它域名上的图片。	
3.	由零开始创建图像（需要onload保证图片加载完毕）
`
var img = new Image();   // 创建一个<img>元素
img.src = 'myImage.png'; // 设置图片源地址
`	
4.	通过 data: url 方式嵌入图像	
`
img.src = 'data:image/gif;base64,...'
`
5.	使用视频帧	
`
return document.getElementById('myvideo');
`	
##	绘制图片	
drawImage(image, x, y)
	其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标。	
##	缩放 Scaling
drawImage(image, x, y, width, height)
	这个方法多了2个参数：width 和 height，这两个参数用来控制 当向canvas画入时应该缩放的大小
##	切片 Slicing
drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
	第一个参数和其它的是相同的，都是一个图像或者另一个 canvas 的引用。其它8个参数最好是参照右边的图解，前4个是定义图像源的切片位置和大小，后4个则是定义切片的目标显示位置和大小。	
##	控制图像的缩放行为 Controlling image scaling behavior
Gecko 1.9.2 引入了 mozImageSmoothingEnabled 属性，值为 false 时，图像不会平滑地缩放。默认是 true 。
`
cx.mozImageSmoothingEnabled = false;
`
#	变形 Transformations
##	状态的保存和恢复 Saving and restoring state
save()restore()
	save 和 restore 方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。
PS：Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存。	
##	绘画状态包括：
当前应用的变形（即移动，旋转和缩放，见下）
strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation 的值
当前的裁切路径（clipping path）
##	移动 Translating
我们先介绍 translate 方法，它用来移动 canvas 和它的原点到一个不同的位置。
translate(x, y)
	translate 方法接受两个参数。x 是左右偏移量，y 是上下偏移量，如右图所示。
PS：在做变形之前先保存状态是一个良好的习惯。
	在绘制螺旋（spirograph）图案，如果不使用 translate 方法，那么只能看见其中的四分之一。
##	旋转 Rotating
它用于以原点为中心旋转 canvas。
rotate(angle)
	这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。
	旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 translate 方法。
##	缩放 Scaling
我们用它来增减图形在 canvas 中的像素数目，对形状，位图进行缩小或者放大。
scale(x, y)
	scale 方法接受两个参数。x,y 分别是横轴和纵轴的缩放因子，它们都必须是正值。值比 1.0 小表示缩小，比 1.0 大则表示放大，值为 1.0 时什么效果都没有。
PS：默认情况下，canvas 的 1 单位就是 1 个像素。举例说，如果我们设置缩放因子是 0.5，1 个单位就变成对应 0.5 个像素，这样绘制出来的形状就会是原先的一半。
##	变形 Transforms
允许对变形矩阵直接修改。
transform(m11, m12, m21, m22, dx, dy)
	这个方法是将当前的变形矩阵乘上一个基于自身参数的矩阵，在这里我们用下面的矩阵：	
`
m11 m21 dx
m12 m22 dy
0 	0 	1
`	
m11：水平方向的缩放
m12：水平方向的偏移
m21：竖直方向的偏移
m22：竖直方向的缩放
dx：水平方向的移动
dy：竖直方向的移动	
setTransform(m11, m12, m21, m22, dx, dy)
	这个方法会将当前的变形矩阵重置为单位矩阵，然后用相同的参数调用 transform 方法。	
	从根本上来说，该方法是取消了当前变形,然后设置为指定的变形,一步完成。
resetTransform()	
	重置当前变形为单位矩阵，它和调用以下语句是一样的：	
	ctx.setTransform(1, 0, 0, 1, 0, 0);
#	组合 Compositing
对合成的图形来说，绘制顺序会有限制。不过，我们可以利用 globalCompositeOperation 属性来改变这种状况。此外, clip属性允许我们隐藏不想看到的部分图形.	
##	globalCompositeOperation
globalCompositeOperation = type
	这个属性设定了在画新图形时采用的遮盖策略，其值是一个标识12种遮盖方式的字符串。
具体type值
```
source-over	默认。在目标图像上显示源图像。
source-atop	在目标图像顶部显示源图像。源图像位于目标图像之外的部分是不可见的。
source-in	在目标图像中显示源图像。只有目标图像内的源图像部分会显示，目标图像是透明的。
source-out	在目标图像之外显示源图像。只会显示目标图像之外源图像部分，目标图像是透明的。
destination-over	在源图像上方显示目标图像。
destination-atop	在源图像顶部显示目标图像。源图像之外的目标图像部分不会被显示。
destination-in	在源图像中显示目标图像。只有源图像内的目标图像部分会被显示，源图像是透明的。
destination-out	在源图像外显示目标图像。只有源图像外的目标图像部分会被显示，源图像是透明的。
lighter	显示源图像 + 目标图像。
copy	显示源图像。忽略目标图像。
xor	使用异或操作对源图像与目标图像进行组合。
```
##	裁切路径 Clipping paths
裁切路径和普通的 canvas 图形差不多，不同的是它的作用是遮罩，用来隐藏不需要的部分。	
clip()	
	来创建一个新的裁切路径。
	默认情况下，canvas 有一个与它自身一样大的裁切路径（也就是没有裁切效果）。
#	基本的动画	
如果需要移动Canvas中的shape，我们不得不对所有东西（包括之前的）进行重绘。重绘是相当费时的，而且性能很依赖于电脑的速度。	
##	动画的基本步骤	
1.清空 canvas
	除非接下来要画的内容会完全充满 canvas （例如背景图），否则你需要清空所有。最简单的做法就是用 clearRect 方法。
2.保存 canvas 状态
	如果你要改变一些会改变 canvas 状态的设置（样式，变形之类的），又要在每画一帧之时都是原始状态的话，你需要先保存一下。
3.绘制动画图形（animated shapes）
	这一步才是重绘动画帧。
4.恢复 canvas 状态
	如果已经保存了 canvas 的状态，可以先恢复它，然后重绘下一帧。	
##	操控动画 Controlling an animation
为了实现动画，我们需要一些可以定时执行重绘的方法。
###	有安排的更新画布 Scheduled updates	
window.setInterval(), window.setTimeout(),和window.requestAnimationFrame()来设定定期执行一个指定函数。	
requestAnimationFrame(callback)
	方法告诉浏览器您希望执行动画并请求浏览器调用指定的函数在下一次重绘之前更新动画。
	该方法使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用。
window.cancelAnimationFrame(ID) 
	以取消回调函数。	
##	高级动画
###	添加速率	
window.requestAnimationFrame(callback) 该方式帮助我们控制动画。
ctx.clearRect(0,0, canvas.width, canvas.height) 在callback前面调用清除画布。	
###	设置边界
物体碰到边缘，反弹回来，将速度更换方向即可。	
`
if (ball.y + ball.vy > canvas.height || ball.y + ball.vy < 0 ) {
	ball.vy = -ball.vy;
}
`	
PS：这里对垂直方向上分析，水平方向类似。
小球的y坐标+小球y的速度大于canvas的高度（下边缘）或者小球的y坐标+小球y的速度小于0（上边缘）反向
###	添加加速度
`
ball.vy *= .99;
ball.vy += .25;
`	
###	长尾效果	
用一个半透明的 fillRect 函数取代之，就可轻松制作长尾效果。	
`
ctx.fillStyle = 'rgba(255,255,255,0.3)';
ctx.fillRect(0,0,canvas.width,canvas.height);
`	
###	添加鼠标控制
为canvas元素添加 mousemove、 mouseout、click等事件，在通过 canvas 的 shape 来进行精确控制
##	像素操作	
可以直接通过ImageData对象操纵像素数据，直接读取或将数据数组写入该对象中。	
###	ImageData 对象	
ImageData对象中存储着canvas对象真实的像素数据，它包含以下几个只读属性：	
width
	图片宽度，单位是像素
height
	图片高度，单位是像素
data
	Uint8ClampedArray类型的一维数组，包含着RGBA格式的整型数据，范围在0至255之间（包括255）。
	每一个像素又数组4为组成，分别对应r、g、b、a
###	创建一个ImageData对象
去创建一个新的，空白的ImageData对象，你应该会使用createImageData() 方法。有2个版本的createImageData()方法。	
var myImageData = ctx.createImageData(width, height);
	上面代码创建了一个新的具体特定尺寸的ImageData对象。所有像素被预设为透明黑。
var myImageData = ctx.createImageData(anotherImageData);
	你也可以创建一个被anotherImageData对象指定的相同像素的ImageData对象。这个新的对象像素全部被预设为透明黑。这个并非复制了图片数据。
###	得到场景像素数据
为了获得一个包含画布场景像素数据的ImageData对像，你可以用getImageData()方法：	
var myImageData = ctx.getImageData(left, top, width, height);
	这个方法会返回一个ImageData对象，它代表了画布区域的对象数据，此画布的四个角落分别表示为(left, top), (left + width, top), (left, top + height), 以及(left + width, top + height)四个点。这些坐标点被设定为画布坐标空间元素。
###	在场景中写入像素数据
你可以用putImageData()方法去对场景进行像素数据的写入。	
ctx.putImageData(myImageData, dx, dy);
	dx和dy参数表示你希望在场景内左上角绘制的像素数据所得到的设备坐标。
##	缩放和反锯齿
在drawImage() 方法， 第二个画布和imageSmoothingEnabled 属性的帮助下，我们可以放大显示我们的图片及看到详情内容。
因为反锯齿默认是启用的，我们可能想要关闭它以看到清楚的像素。	
禁用反锯齿
`
ctx.imageSmoothingEnabled = false;
ctx.mozImageSmoothingEnabled = true;
ctx.webkitImageSmoothingEnabled = true;
ctx.msImageSmoothingEnabled = true;
`		
###	保存图片
HTMLCanvasElement  提供一个toDataURL()方法，此方法在保存图片的时候非常有用。	
它返回一个包含被类型参数规定的图像表现格式的数据链接。返回的图片分辨率是96dpi。	
canvas.toDataURL('image/png')
	默认设定。创建一个PNG图片。
canvas.toDataURL('image/jpeg', quality)
	创建一个JPG图片。你可以有选择地提供从0到1的品质量，1表示最好品质，0基本不被辨析但有比较小的文件大小。
canvas.toBlob(callback, type, encoderOptions)
	这个创建了一个在画布中的代表图片的Blob对像。
PS：这里是canvas对象上的方法，而不是画布上下文对象的方法。	
##	点击区域和无障碍访问	
canvas 标签只是一个位图，它并不提供任何已经绘制在上面的对象的信息。	
###	内容兼容
```
<canvas> ... </canvas>标签里的内容被可以对一些不支持canvas的浏览器提供兼容。	
```
###	ARIA 规则	
Accessible Rich Internet Applications (ARIA) 定义了让Web内容和Web应用更容易被有身体缺陷的人获取的办法。你可以用ARIA属性来描述canvas元素的行为和存在目的	
###	点击区域（hit region）	
判断鼠标坐标是否在canvas上一个特定区域里一直是个有待解决的问题。hit region API让你可以在canvas上定义一个区域，这让无障碍工具获取canvas上的交互内容成为可能。它能让你更容易地进行点击点击检测并把事件转发到DOM元素去。这个API有以下三个方法（都是实验性特性，请先在浏览器兼容表上确认再使用）。	
CanvasRenderingContext2D.addHitRegion() 
	在canvas上添加一个点击区域。
	ctx.addHitRegion({control: element});
	addHitRegion()方法也可以带一个control选项来指定把事件转发到哪个元素上（canvas里的元素）。
ctx.removeHitRegion() 
	从canvas上移除指定id的点击区域。
ctx.clearHitRegions() 
	移除canvas上的所有点击区域。
PS：实验性方法，浏览器暂不支持	
##	焦点圈
当用键盘控制时，焦点圈是一个能帮我们在页面上快速导航的标记。要在canvas上绘制焦点圈，可以使用drawFocusIfNeeded 属性。	
ctx.drawFocusIfNeeded() 
	如果给定的元素获得了焦点，这个方法会沿着在当前的路径画个焦点圈。	
ctx.scrollPathIntoView() 
	把当前的路径或者一个给定的路径滚动到显示区域内。
	scrollPathIntoView()方法可以让一个元素获得焦点的时候在屏幕上可见(滚动到元素所在的区域)。	
##	canvas的优化
###	性能贴士	
下面是一些改善性能的建议	
1.	在离屏canvas上预渲染相似的图形或重复的对象
```
myEntity.offscreenCanvas = document.createElement("canvas");
myEntity.offscreenCanvas.width = myEntity.width;
myEntity.offscreenCanvas.height = myEntity.height;
myEntity.offscreenContext = myEntity.offscreenCanvas.getContext("2d");
myEntity.render(myEntity.offscreenContext);
```
2.	避免浮点数的坐标点，用整数取而代之	
```
当你画一个没有整数坐标点的对象时会发生子像素渲染。
ctx.drawImage(myImage, 0.3, 0.5);
浏览器为了达到抗锯齿的效果会做额外的运算。为了避免这种情况，请保证在你调用drawImage()函数时，用Math.floor()函数对所有的坐标点取整。
```
3.	不要在用drawImage时缩放图像（多个 canvas 元素）	
在离屏canvas中缓存图片的不同尺寸，而不要用drawImage()去缩放它们。		
4.	使用多层画布去画一个复杂的场景	
你可能会发现，你有些元素不断地改变或者移动，而其它的元素，例如外观，永远不变。这种情况的一种优化是去用多个画布元素去创建不同层次。	
###	用CSS设置大的背景图	
如果像大多数游戏那样，你有一张静态的背景图，用一个静态的<div>元素，结合background 特性，以及将它置于画布元素之后。这么做可以避免在每一帧在画布上绘制大图。	
5.	用CSS transforms特性缩放画布	
CSS transforms 特性由于调用GPU，因此更快捷。最好的情况是，不要将小画布放大，而是去将大画布缩小。	
6.	使用moz-opaque属性(仅限Gecko)	
如果你的游戏使用画布而且不需要透明，请在画布上设置moz-opaque属性。这能够用于内部渲染优化。
```
<canvas id="mycanvas" moz-opaque></canvas>
```
###	其他优化
将画布的函数调用集合到一起（例如，画一条折线，而不要画多条分开的直线）
避免不必要的画布状态改变
渲染画布中的不同点，而非整个新状态
尽可能避免 shadowBlur特性
尽可能避免text rendering
使用不同的办法去清除画布(clearRect() vs. fillRect() vs. 调整canvas大小)
有动画，请使用window.requestAnimationFrame() 而非window.setInterval()
请谨慎使用大型物理库
用JSPerf测试性能
[demo地址](https://fanerge.github.io/canvas_solar_system/)
[这里向大家推荐一下阿里开源的数据可视化库antV（g2、g6、f2）](https://antv.alipay.com/zh-cn/index.html)
>	参考文档：
	[MDN-canvas教程](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)	
	[廖雪峰老师的Canvas](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/00143449990549914b596ac1da54a228a6fa9643e88bc0c000)
	[MDN-canvas标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/canvas)
	[Canvas 的基本原理](https://segmentfault.com/a/1190000004469449)
	[HTML5画布(CANVAS)速查简表](http://www.webhek.com/post/html5-canvas-cheat-sheet.html)























