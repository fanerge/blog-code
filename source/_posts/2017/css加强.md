---
title: css-加强（白光划过效果、3D立方体）
date: 2017-08-29 20:53:23
category: "css"
tags: ['css','web开发','读书笔记']
---
在接下来的一段时间学习看云上购买的一本[Web开发实战](https://www.kancloud.cn/dennis/javascriptmethod/261472)书籍，来提升自己的软实力。
本部分为css实战部分。

####	白光划过效果

#####	创建模板
我们要放置一张图片，用一个div包裹起来：
	```
	<div class="highlight-box">
		<img src="./img/demo.png" width="296" height="374" alt="s">
	</div>
	```
	
#####	设置CSS样式
定义初始样式:
	```
	.highlight-box {
        /**这里用于before、after伪类定位**/
        position: relative; 
        width: 296px;
        height: 374px;
        overflow: hidden;
    }
	```
接着让我们来制作白光，我们不需多余的元素，只需使用:before选择器.position为伪类定位：
	```
	.highlight-box:before {
        position: absolute;
        /* 注意这里top和left，让白光移动到图片左上角，后续的划过动画也是依靠这两个属性 */
        top: -200%;
        left: -100%;
        z-index: 2;
        display: block;
        content: '';
        /* 定义白光的高宽，hight为300%是为了防止在移动过程中白光不够用 */
        width: 50%;
        height: 300%;
        /* 使用线性渐变来实现白光 */
        background: linear-gradient(to left top, rgba(255, 255, 255, .05) 20%, rgba(255, 255, 255, .6) 65% , rgba(255, 255, 255, .05) 100%);
        /* background: rgba(255, 255, 255, .5); */
        transform: rotate(45deg);
    }
	```
我们使用渐变（linear-gradient）来实现白光效果，同时为了斜向划过，使用transform: rotate(45deg)将其旋转45度。
上面的height、width、top和left，你也可以使用具体的像素值，不过建议采用百分比，这样可以重复使用，而不需手动改变太多值。
触发白光(hover伪类和css3动画)：
	```
	.highlight-box:hover:before {
        /* 这里省略了私有前缀代码 */
        animation: crossed .5s linear;
    }
    @keyframes crossed {
        0% {
            top: -200%;
            left: -100%;
        }
        100% {
            top: -50px;
            left: 100%;
        }
    }
	```
截图效果(鼠标划入时的效果)
		![白光划过](/images/css_white_light.jpg)
	
####	3D立方体
随着CSS3的出现，实现3D效果已经不是难事，这一节就来看看3D立方体是如何实现的。
#####	创建模板	
首先来放置一个父div.cude，然后在其里面放置6个div，分别表示立方体的6个面。
	```
	<div class="cude">
		<div class="front surface">
			正面
		</div>
		<div class="surface left">
			左面
		</div>
		<div class="surface right">
			右面
		</div>
		<div class="surface bottom">
			底面
		</div>
		<div class="surface top">
			顶面
		</div>
		<div class="surface back">
			背面
		</div>
	</div>
	```

#####	设置CSS样式
	```
	.cude {
		width:300px;
		height:300px;
		position:relative;
		margin:100px auto;
		transform-style:preserve-3d;
		-webkit-transform-style:preserve-3d;
	}
	.surface {
		position:absolute;
		top:0;
		left:0;
		width:300px;
		height:300px;
		background:#666;
		opacity:0.8;
		font-size:60px;
		text-align:center;
		line-height:300px;
		font-weight:bold;
		color:#fff;
		border:1px solid #fff;
		-webkit-transition:all .3s;
		transition:all .3s;
	}
	.surface img {
		width:100%;
	}
	.front {
		transform:rotateY(0) translateZ(150px);
	}
	.back {
		transform:translateZ(-150px) rotateY(180deg);
	}
	.left {
		transform:rotateY(-90deg) translateZ(150px);
	}
	.right {
		transform:rotateY(90deg) translateZ(150px);
	}
	.top {
		transform:rotateX(90deg) translateZ(150px);
	}
	.bottom {
		transform:rotateX(90deg) translateZ(-150px);
	}
	@-webkit-keyframes rotate {
		from {
			transform: rotateX(0deg) rotateY(0deg);
		}
		to {
			transform: rotateX(360deg) rotateY(360deg);
		}
	}
	```
	
总结：
1.	使用了before和after伪类
2.	linear-gradient-api的使用
3.	animation
4.	transform(分为2D转换和3D转换)
5.	2d转换：translate(x,y)、translateX(x)、translateY(y)、scale(x,y)、scaleX(x)、scaleY(y)、rotate(angle)、skew(x-angle,y-angle)、skewX(angle)
6.	3d转换：translate3d(x,y,z)、translateZ(z)、scale3d(x,y,z)、scaleZ(z)、rotate3d(x,y,z,angle)、rotateX(angle)、rotateY(angle)、rotateZ(angle)

>	参考文档
1.	[web开发实战](https://www.kancloud.cn/dennis/javascriptmethod/261473)
2.	[linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient)
3.	[transform](http://www.runoob.com/cssref/css3-pr-transform.html)
	[代码仓库](https://github.com/fanerge/web-/tree/master)































