---
title: Geolocation（地理定位）
date: 2018-01-24 20:11:31
tags: 'html5'
categories: 'html5'
copyright: true
---
#	介绍
Geolocation 接口是一个用来获取设备地理位置的可编程的对象，它可以让Web内容访问到设备的地理位置，这将允许Web应用基于用户的地理位置提供定制的信息。
其实Geolocation 就是用来获取到当前设备的经纬度（位置）
带有此接口的对象可以用由 Navigator实现的属性NavigatorGeolocation.geolocation 来获得。
PS：鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。
#	详解
##	检测是否支持地理定位
```
if (navigator.geolocation) {
	// 做相应的操作 
} else {
	console.error('不支持地理')
}
```
##	获取当前定位
Geolocation.getCurrentPosition(success, error, options)
	确定设备的位置并返回一个携带位置信息的 Position 对象。
参数：
###	success
成功得到位置信息时的回调函数，使用Position 对象作为唯一的参数。 
####	Position.coords 只读（latitude、longitude、accuracy）
返回一个定义了当前位置的Coordinates 对象.
	coords.latitude	十进制数的纬度
	coords.longitude	十进制数的经度
	coords.accuracy	位置精度
	coords.altitude	海拔，海平面以上以米计
	coords.altitudeAccuracy	位置的海拔精度
	coords.heading	方向，从正北开始以度计
	coords.speed	速度，以米/每秒计
####	Position.timestamp 只读
返回一个时间戳DOMTimeStamp， 这个时间戳表示获取到的位置的时间。
###	error 可选
获取位置信息失败时的回调函数，使用 PositionError 对象作为唯一的参数，这是一个可选项。 
####	PositionError.code 只读
返回无符号的、简短的错误码。
	PERMISSION_DENIED--权限问题
	POSITION_UNAVAILABLE--内部错误
	TIMEOUT--超时
####	PositionError.message 只读
返回一个开发者可以理解的 DOMString 来描述错误的详细信息。
###	options 可选
#####	一个可选的PositionOptions 对象。
	enableHighAccuracy: false;--是否高精度，默认false 
	timeout: 5000;--超时事件ms 
	maximumAge: 0; 地理位置缓存时长ms
##	监视定位
Geolocation.watchPosition(success[, error[, options]])	  
	用于注册监听器，在设备的地理位置发生改变的时候自动被调用。也可以选择特定的错误处理函数。	  
	该方法会返回一个 ID，如要取消监听可以通过  Geolocation.clearWatch() 传入该 ID 实现取消的目的。
参数：
###	success
成功时候的回调函数， 同时传入一个 Position 对象当作参数。
###	error 可选
失败时候的回调函数，可选， 会传入一个 PositionError 对象当作参数。
###	options 可选
一个可选的 PositionOptions 对象。
PS：Position、PositionError、PositionOptions对象和上面一样。
##	清理监视定位	  
Geolocation.clearWatch(id)	  
	这个方法主要用于使用 Geolocation.watchPosition() 注册的 位置/错误 监听器。	  
参数：
###	id
希望移除的监听器所对应的 Geolocation.watchPosition() 返回的 ID 数字。
	Geolocation.watchPosition()注册一个位置改变监听器，每当设备位置改变时，返回一个 long 类型的该监听器的ID值。
Geolocation.clearWatch()
	取消由 watchPosition()注册的位置监听器。
>	参考文档：
	[Geolocation](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation)
	[HTML5 Geolocation（地理定位）](http://www.runoob.com/html/html5-geolocation.html)
	[PositionError](https://developer.mozilla.org/zh-CN/docs/Web/API/PositionError)
	[PositionOptions](https://developer.mozilla.org/zh-CN/docs/Web/API/PositionOptions)
	[watchPosition](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation/watchPosition)













