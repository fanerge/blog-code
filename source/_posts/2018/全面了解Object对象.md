---
title: 全面了解Object对象
date: 2018-03-20 20:36:46
tags: 'js'
categories: 'js'
copyright: true
---
#	为什么一切皆为对象
‘一切皆为对象’，这可是javascript中‘圣经’，可是为什么这样说呢，我们来一探究竟吧？
为了解决这个问题，我们的从javascript的原型链说起。
##	原型链
在js几乎任何对象有一个 [[prototype]] 属性，在标准中，这是一个隐藏属性。
虽然说 [[prototype]] 是一个隐藏属性，但很多浏览器都给每一个对象提供 \_\_proto\_\_ 这一属性，这个属性就是该对象的[[prototype]]。
>	Object.prototype 的 \_\_proto\_\_  属性是一个访问器属性（一个getter函数和一个setter函数）, 暴露了通过它访问的对象的内部 [[Prototype]] (一个对象或 null)。
使用 \_\_proto\_\_ 是有争议的，也不鼓励使用它。因为它从来没有被包括在EcmaScript语言规范中，但是现代浏览器都实现了它。\_\_proto\_\_ 属性已在ECMAScript 6语言规范中标准化，用于确保Web浏览器的兼容性，因此它未来将被支持。它已被不推荐使用, 赞成Object.getPrototypeOf/Reflect.getPrototypeOf 和Object.setPrototypeOf/Reflect.setPrototypeOf 来获取或设置对象的原型链。

下面我们用标准的方法来获取对象的原型
```
// 基本数据类型（Number,String,Boolean,Symbol）
let str = 'strstr';
// 1
Object.getPrototypeOf(str) // String对象（这里包括字符串原型链上的所有方法如slice、indexOf等）
// 2
Object.getPrototypeOf(Object.getPrototypeOf(str)) // Object对象
// 3
Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(str))) // null
console.dir(Object.getPrototypeOf(str))

// 其他类型（Object,Function,Array,Error,Math,Date,Map,Set,WeakMap,WeakSet,JSON）
function demo() {}
// 4
Object.getPrototypeOf(demo) // anonymous函数
// 5
Object.getPrototypeOf(Object.getPrototypeOf(demo)) // Object对象
// 6
Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(demo))) // null
```
PS：博主分别以基本数据类型Number,String,Boolean,Symbol（这里不考虑undefined和null，因为它们是特殊的两个值，木有原型）做了测试，上面代码注释1,2,3最终到达原型链的顶端null。
对于其他类型也是同样的，通过4,5,6到达原型链顶端Object.prototype === null。
从上面可以整个javascript语法系统都是基于这个原型链来实现方法和属性的继承的，并且可以看到原型链是有终点的值为 null。

搞清楚了原型链，在回到‘一切皆为对象’。
明白了javascript的原型链，明白了所有的继承终点都到了Object上，就不难理解‘一切皆为对象’了吧。

#	对象创建的方式
##	字面量
```
let obj = {
	name: 'yzf',
	age: 27,
	city: 'chengdu'
}
```
这是大家最熟悉的创建方式。
##	Object实例化
```
let obj = new Object({
	// name: 'yzf',
	// age: 27,
	// city: 'chengdu'
});
obj.name = 'yzf';
obj.age = 27;
obj.city = 'chengdu';
```
这里两种方式，为obj赋值，一种在实例化是进行，一种在实例化之后添加。
##	构造函数
```
function Person(name, age, city){
	this.name = name;
	this.age = age;
	this.city = city;
}

let fanerge = new Person('yzf', 27, 'chengdu')
```
#  Object对象的方法
Object对象的方法分为Object静态方法和Object的实例方法，静态方法定义在Object自身上，而实例方法定义在Object.prototype上；
在使用方式上也有区别，静态方法使用'如Object.keys(obj)'而实例方法使用'如obj.hasOwnProperty(prop)'。
我认为我们学习API是需要重点了解一个API的使用方式、定义、参数说明、返回值，下面我给出Object标准库中的方法，其中有部分是ES6+的方法，存在一定的兼容性问题。
##  Object的静态方法
###  Object.assign(target, ...sources)
定义：用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。
参数：target为目标对象，sources为源对象。
返回：目标对象。
###  Object.create(proto, [propertiesObject])
定义：创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 
参数：proto为新创建对象的原型对象，propertiesObject为相关属性的描述符。
返回：新对象。
###  Object.defineProperty(obj, prop, descriptor)
定义：会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
参数：obj为要在其上定义属性的对象，prop为要定义或修改的属性的名称，descriptor为将被定义或修改的属性描述符。
返回：obj。
###  Object.defineProperties(obj, props)
定义：直接在一个对象上定义新的属性或修改现有属性，并返回该对象。
参数：obj为要在其上定义属性的对象，props为要定义或修改的一个或多个属性的描述符。
返回：obj。
###  Object.keys(obj)
定义：返回一个由一个给定对象的自身可枚举属性组成的数组。
参数：obj为要返回其枚举自身属性的对象。
返回：一个表示给定对象的所有可枚举属性的字符串数组。
###  Object.values(obj)
定义：返回一个给定对象自己的所有可枚举属性值的数组。
参数：obj为要返回其枚举自身属性的对象。
返回：一个包含对象自身的所有可枚举属性值的数组。
###  Object.values(obj)
定义：返回一个给定对象自己的所有可枚举属性值的数组。
参数：obj为要返回其枚举自身属性的对象。
返回：给定对象自身可枚举属性的键值（键值对也为数组）对数组。
###  Object.preventExtensions(obj)
定义：让一个对象变的不可扩展，也就是永远不能再添加新的属性。
参数：obj为将要变得不可扩展的对象。
返回：已经不可扩展的对象。
PS：该对象的属性可能仍然可删除，且对象的原型仍然可以添加属性。
###  Object.isExtensible(obj)
定义：判断一个对象是否是可扩展的（是否可以在它上面添加新的属性）。
参数：obj为需要检测的对象。
返回：表示给定对象是否可扩展的一个Boolean 。
###  Object.seal(obj)
定义：封闭一个对象，阻止添加新属性并将所有现有属性标记为不可配置。
参数：obj为将要被密封的对象。
返回：被密封的对象。
PS：属性不可配置的效果就是属性变的不可删除，以及一个数据属性不能被重新定义成为访问器属性（get）。
###  Object.isSealed(obj)
定义：判断一个对象是否被密封。
参数：obj为需要检测的对象。
返回：表示给定对象是否被密封的一个Boolean。
###  Object.freeze(obj)
定义：可以冻结一个对象，冻结指的是不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、可配置性、可写性。
参数：obj为要被冻结的对象。
返回：要被冻结的对象。
###  Object.isFrozen(obj)
定义：判断一个对象是否被冻结。
参数：obj为需要检测的对象。
返回：表示给定对象是否被冻结的Boolean。
###  Object.getOwnPropertyDescriptor(obj, prop)
定义：返回指定对象上一个自有属性对应的属性描述符。
参数：obj为需要查找的目标对象，prop为目标对象内属性名称（String类型）。
返回：如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。
###  Object.getOwnPropertyDescriptors(obj)
定义：用来获取一个对象的所有自身属性的描述符。
参数：obj为需要查找的目标对象。
返回：所指定对象的所有自身属性的描述符，如果没有任何自身属性，则返回空对象。
###  Object.getOwnPropertyNames(obj)
定义：返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。
参数：obj为返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。
返回：在给定对象上找到的属性对应的字符串数组。
###  Object.getOwnPropertySymbols(obj)
定义：返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。
参数：obj为要返回 Symbol 属性的对象。
返回：在给定对象自身上找到的所有 Symbol 属性的数组。
###  Object.setPrototypeOf(obj, prototype)
定义：设置一个指定的对象的原型 ( 即, 内部[[Prototype]]属性）到另一个对象或null。
参数：obj为要设置其原型的对象，prototype为该对象的新原型(一个对象 或 null)。
返回：返回obj对象。
###  Object.getPrototypeOf(object)
定义：返回指定对象的原型（内部[[Prototype]]属性的值）。
参数：要返回其原型的对象。
返回：给定对象的原型。如果没有继承属性，则返回null。
###  Object.is(value1, value2);
定义：判断两个值是否是相同的值。
参数：value1为需要比较的第一个值，value2为需要比较的第二个值。
返回：表示两个参数是否相同的Boolean 。
##  Object的实例方法
下面obj为Object的实例。
###  obj.hasOwnProperty(prop)
定义：返回一个布尔值，指示对象自身属性中是否具有指定的属性。
参数：prop为要检测的属性String或者Symbol。
返回：用来判断某个对象是否含有指定的属性的Boolean。
###  prototypeObj.isPrototypeOf(obj)
定义：返回一个布尔值，指示对象自身属性中是否具有指定的属性。
参数：obj为在该对象的原型链上搜寻。
返回：表示调用对象是否在另一个对象的原型链上的Boolean。
###  obj.propertyIsEnumerable(prop)
定义：返回一个布尔值，表示指定的属性是否可枚举。
参数：prop为需要测试的属性名。
返回：用来表示指定的属性名是否可枚举的Boolean。
###  obj.toLocaleString()
定义：方法返回一个该对象的字符串表示。
返回：表示对象的字符串。
###  obj.propertyIsEnumerable(prop)
定义：返回一个表示该对象的字符串。
返回：表示该对象的字符串。
PS：ES6之前经常这样用Object.prototype.toString.call(obj) === ‘[object Object]’ 来区别Object和Array。
###  ob.valueOf()
定义：返回指定对象的原始值。
返回：返回值为该对象的原始值。
###  object instanceof constructor
定义：instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。
参数：object为要检测的对象，constructor为某个构造函数。
返回：boolean值。[2018-03-21]

>	参考文档：
[MDN\_\_proto\_\_](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)
[\_\_proto\_\_和prototype的区别](https://www.zhihu.com/question/34183746/answer/58068402)
[Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)








