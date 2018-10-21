---
title: grid布局
date: 2017-09-11 20:09:01
category: "css"
tags: ['css','布局']
---
#### 设置在网格容器上的属性
	display: grid | inline-grid | subgrid;
	grid-template-columns
	grid-template-rows
	grid-template-areas
	grid-column-gap
	grid-row-gap
	grid-gap
	justify-items
	align-items
	align-content
	grid-auto-columns
	grid-auto-rows
	grid-auto-flow
	grid
####	设置在网格项上的属性
	grid-column-start
	grid-column-end
	grid-row-start
	grid-row-end
	grid-column
	grid-row
	grid-area
	justify-self
	align-self
####	显式的网格
#####	html
	```
	<section class="grid">
		<div class="item">item1</div>
		<div class="item">item1</div>
		<div class="item">item1</div>
		<div class="item">item1</div>
		<div class="item">item1</div>
		<div class="item">item1</div>
		<div class="item">item1</div>
	</section>
	```
#####	css
	```
	.grid {
		display: grid; 
		grid-template-columns: 1fr 2fr 3fr; // 指定3个列，分别指定每个列的宽度（1/6, 2/6, 3/6）。
		grid-template-rows: 100px 100px; // 指定2行，分别指定每个行的宽度
		grid-gap: 20px 10px; // 指定行间距和列间距（若只有一个参数是行和类间距取相同值）
	}
	```
####	重复轨道
#####	css
	```
	.grid {
		display: grid;
		grid-template-columns: repeat(3, 1fr 2fr); // 第一个参数指定的是重复次数，第二个参数是每次重复的轨道列表（共3*2=6列）。
		grid-template-rows: 100px 100px;
		grid-gap: 20px 1px;
	}
	```
####	自动重复轨道
#####	css
	```
	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fill, 100px); // auto-fill关键词创建了许多与网格容器相匹配的轨道，而不会导致网格溢出。
		grid-template-rows: 100px 100px;
		grid-gap: 20px 10px;
	}
	.grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, 100px); // auto-fit关键词与auto-fill有点类似，只是在网格项放置之后，它只会在需要时创建尽可能多的轨道，而重复的空轨道会堆叠在一起（合并）。
		grid-template-rows: 100px 100px;
		grid-gap: 20px 10px;
	}
	```
####	隐式网格
	如果网格中有更多的网格项，或者网格项被放置在显式网格之外，网格容器就会通过向网格中添加网格线来自动生成网格轨道。
	显式网格和这些额外的隐式轨道和网格线构成了所谓的隐式网格。
#####	css
	对于子项目
	```
	.item:first-child {
		grid-column-start: -1;
	}
	.item:nth-child(2) {
		grid-row-start: 4;
	}
	```
####	隐式轨道尺寸
#####	css
	```
	.grid { 
		display: grid; 
		grid-template-columns: repeat(4, 1fr); 
		grid-template-rows: 100px 100px; 
		grid-gap: 20px; 
		grid-auto-columns: 200px; 
		grid-auto-rows: 60px; 
		grid-auto-columns: minmax(200px, auto); // 隐藏轨道现在最小宽度是200px
		grid-auto-rows: minmax(auto, 300px); // 隐藏轨道现在最大高度度是300px
	}
	```
####	将网格扩展到开始
#####	css
	```
	.item:first-child {
	  grid-row-end: 1;
	  grid-row-start: span 3; // 开始跨越3个单元格
	}
	.item:nth-child(2) {
	  grid-column-end: 2;
	  grid-column-start: span 2; // 开始跨越2个单元格
	}
	```
####	自动放置
#####	css
我们可以通过使用grid-auto-flow属性来指定如何把网格项目自动放置到网格容器。
	```
	.grid2 {
		grid-auto-flow: column;
	}
	```
####	未定义显式网格
使用grid-auto-rows和grid-auto-columns可以自动地调整单元格大小，因此不需要定义显式网格。

>	参考文档
1.	[显式网格和隐式网格的区别 ](http://www.w3cplus.com/css3/difference-explicit-implicit-grids.html)	
2.	[Grid布局指南](http://www.jianshu.com/p/d183265a8dad)















