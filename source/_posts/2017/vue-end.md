---
title: vue  end
category: "vue"
tags: [vue,element-ui]
---
## 今天完成后台界面中引入[iconfont](http://www.iconfont.cn)图标和添加全屏功能[screenfull]()
*如有不正确的地方，请大家提出来，我会更正，共同进步，谢谢。*

### 引入iconfont图标，这是阿里的开源产品，个人感觉不错，其中symbol方式应该会成为未来的主流方式（不支持ie8）
1. 进入iconfont后，创建项目，把自己喜欢的图标都放到项目中去，点击下载这里我们只需要iconfont.js文件
2. 在main.js中引入 
	```
	import '@/assets/icon-font/iconfont' // 引入iconfont图库
	```
3. 这里我把它单独封装一个组件，以便其他地方使用(直接贴上源码了，都能看懂)
	```
	<template>
		<svg class="icon" :style="{width: width + 'em', height: height + 'em', verticalAlign: verticalAlign + 'em'}" aria-hidden="true">
		  <use :xlink:href="iconName"></use>
		</svg>
	</template>

	<script>
	  export default {
		name: 'iconfont',
		props: {
			icon: {
				type: String,
				required: true 
			},
			width: {
				type: Number,
				default: 1
			},
			height: {
				type: Number,
				default: 1
			},
			verticalAlign: {
				type: Number,
				default: -0.15
			}
		},
		data () {
		  return {
		  }
		},
		computed: {
			iconName () {
				return `#${this.icon}`
			}
		}
	  }
	</script>

	<style scoped>
	  .icon {
		   fill: currentColor;
		   overflow: hidden;
		}
	</style>
	```
4. 使用的时候
	```
	<iconfont icon="icon-daxiang" :width="2" :height="2" :verticalAlign="-0.5"></iconfont>
	```
### 添加全屏功能
1. 安装依赖 npm i screenfull -S
2. 我还是封装成组件，一遍其他项目使用(贴原代码)
	```
	<template>
		<iconfont @click.native="toggleFullScreen" :icon="iconType"></iconfont>
	</template>

	<script>
	import screenfull from 'screenfull'
	import iconfont from '@/components/iconfont'
	  export default {
		name: 'screenfull',
		data () {
		  return {
			isFullscreen: false,
			iconType: 'icon-quanping'
		  }
		},
		methods: {
			toggleFullScreen () {
				if (!screenfull.enabled) {
					this.$message({
					  message: 'you browser can not work',
					  type: 'warning'
					});
					return false;
				}
				this.iconType = this.isFullscreen ? 'icon-quanping' : 'icon-tuichuquanping'
				screenfull.toggle()
				this.isFullscreen = !this.isFullscreen
			}
		},
		components: {
			iconfont
		}
	  }
	</script>

	<style scoped>
	</style>
	```
后台系统，先做的这里，以后在添加。打算接着撸一段事件的函数式编程










