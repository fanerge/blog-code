---
title: javascript标准库总结
date: 2018-03-19 21:56:38
tags: 'js'
categories: 'js'
copyright: true
---
#	值属性
这部分属性只是简单的值，它们没有自己的属性和方法。
###	Infinity
	全局属性 Infinity 是一个数值，表示无穷大。
###	NaN
	全局属性 NaN 的值表示不是一个数字（Not-A-Number）。
###	undefined
	全局属性undefined表示原始值undefined。它是一个JavaScript的 原始数据类型 。
###	null
	值 null 特指对象的值未设置。它是 JavaScript 基本类型 之一。

#	函数属性
全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。	
###	eval(str)
	eval() 函数会将传入的字符串当做 JavaScript 代码进行执行。
PS：eval会造成安全和性能方面的问题，具体参见[避免在不必要的情况下使用 eval](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval#Don.27t_use_eval.21)。
###	isFinite(arg)
	判断被传入的值（非number类型将转换为number类型）是否为有限值。
###	isNaN()
	判断被传入的值（非number类型将转换为number类型）是否为NaN。
PS：使用Number.isNaN()来代替更有语义性。
###	parseFloat(str)
	parseFloat() 函数解析一个字符串参数并返回一个浮点数。
PS：如果在解析过程中遇到了正负号(+或-),数字(0-9),小数点,或者科学记数法中的指数(e或E)以外的字符,则它会忽略该字符以及之后的所有字符,返回当前已经解析到的浮点数.同时参数字符串首位的空白符会被忽略.
	如果第一个字符不能解析，直接返回NaN。
###	parseInt(str, radix);
	parseInt() 函数解析一个字符串参数，并返回一个指定基数的整数 (数学系统的基础)。
PS：radix一个介于2和36之间的整数，表示上述字符串的基数（默认为10）。
###	encodeURI(URI)
函数通过将特定字符的每个实例替换为一个、两个、三或四转义序列来对统一资源标识符 (URI) 进行编码 (该字符的 UTF-8 编码仅为四转义序列)由两个 "代理" 字符组成)。
PS：encodeURI 字母、数字、;、,、/、?、:、@、&、=、+、$、-、_、.、!、~、*、'、(、)、#、之外的所有字符。
###	decodeURI(encodeURI)
	decodeURI() 函数解码一个由encodeURI 先前创建的统一资源标识符（URI）或类似的例程。
###	encodeURIComponent(str)
	encodeURIComponent()是对统一资源标识符（URI）的组成部分进行编码的方法。
PS：encodeURIComponent 转义除了字母、数字、(、)、.、!、~、*、'、-和_之外的所有字符。
	为了避免服务器收到不可预知的请求，对任何用户输入的作为URI部分的内容你都需要用encodeURIComponent进行转义。
###	decodeURIComponent(encodedURI)
decodeURIComponent() 方法用于解码由 encodeURIComponent 方法或者其它类似方法编码的部分统一资源标识符（URI）。
###	encodeURI和encodeURIComponent的区别与使用场景
	区别在于编码的字符范围不同。
encodeURI使用于编码整个URI而encodeURIComponent主要query部分（当你需要编码URL中的参数）。
[简单明了区分escape、encodeURI和encodeURIComponent](https://www.cnblogs.com/season-huang/p/3439277.html)

#  Function
全局的Function对象没有自己的属性和方法, 但是, 因为它本身也是函数，所以它也会通过原型链从Function.prototype上继承部分属性和方法。
##  原型属性
###  length
定义：指明函数的形参个数（确定多少个必须要传入的参数）区别于arguments.length实参个数（确定函数被调用时的实际传参个数）。
###  constructor
定义：返回创建实例对象的 Object 构造函数的引用。
##  原型方法
###  func.apply(thisArg, [argsArray])
定义：调用一个函数, 其具有一个指定的this值，以及作为一个数组（或类似数组的对象）提供的参数。
参数：thisArg为func函数执行时this的指向，argsArray为类数组参数数组。
返回：调用有指定this值和参数的函数的结果。
###  fun.call(thisArg, arg1, arg2, ...)
定义：调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)。
参数：thisArg为func函数执行时this的指向，arg1, arg2, ...为指定的参数列表。
返回：返回值是你调用的方法的返回值，若该方法没有返回值，则返回undefined。
运用：1.使用call方法调用父构造函数（在一个子构造函数中，你可以通过调用父构造函数的call方法来实现继承）
  2.使用call方法调用匿名函数3.使用call方法调用函数并且指定上下文的'this'
[运用举例](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call#示例)
###  fun.bind(thisArg[, arg1[, arg2[, ...]]])
定义：调用一个函数, 其具有一个指定的this值，以及作为一个数组（或类似数组的对象）提供的参数。
参数：thisArg为当绑定函数被调用时，该参数会作为原函数运行时的 this 指向，arg1、arg2...为当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。
返回：由指定的this值和初始化参数改造的原函数拷贝（返回一个函数）。
运用：1.创建绑定函数（显式绑定this）2.偏函数（使一个函数拥有预设的初始参数）。
[运用举例](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#示例)
###  Function.prototype.toString()
定义：返回一个表示当前函数源代码的字符串。
参数：null。
返回：表示函数源代码的一个字符串。

#	Number
JavaScript 的 Number 对象是经过封装的能让你处理数字值的对象。
Number()，如果参数无法被转换为数字，则返回 NaN。
##	属性
###	Number.EPSILON
两个可表示(representable)数之间的最小间隔，在进行计算时误差在这个范围内被认为是合理的。
###	Number.MAX_SAFE_INTEGER
JavaScript 中最大的安全整数 (2^53 - 1)。
###	Number.MIN_SAFE_INTEGER
JavaScript 中最小的安全整数 (-(2^53 - 1)).
###	Number.MAX_VALUE
能表示的最大正数。最小的负数是 -MAX_VALUE。
###	Number.MIN_VALUE
能表示的最小正数即最接近 0 的正数 (实际上不会变成 0)。最大的负数是 -MIN_VALUE。
###	Number.NaN
Not A Number.
###	Number.NEGATIVE_INFINITY
特殊的负无穷大值，在溢出时返回该值。
###	Number.POSITIVE_INFINITY
特殊的正无穷大值，在溢出时返回改值。
##	方法
下列方法均不会发生将String转化为Number的过程。
###	Number.isNaN(value)
定义：确定传递的值是否为 NaN和其类型是 Number。它是用于代替原始的全局isNaN()。
参数：要被检测是否是 NaN 的值。
返回：一个布尔值，表示给定的值是否是 NaN。
PS：该方法不同于全局的isNaN()，不会将字符串转换为数字。
###	Number.isFinite(value)
定义：用来检测传入的参数是否是一个有穷数（finite number）。
参数：value要被检测有穷性的值。
返回：一个布尔值表示给定的值是否是一个有穷数。
PS：和全局的 isFinite() 函数相比，这个方法不会强制将一个非数值的参数转换成数值。
###	Number.isInteger(value)
定义：用来判断给定的参数是否为整数。
参数：value要判断此参数是否为整数。
返回：判断给定值是否是整数的 Boolean 值。
###	Number.isSafeInteger(testValue)
定义：用来判断传入的参数值是否是一个“安全整数”（safe integer）。
参数：testValue需要检测的参数。
返回：一个布尔值 表示给定的值是否是一个安全整数（safe integer）。
###	Number.parseFloat(string)
定义：可以把一个字符串解析成浮点数。
参数：string被解析的字符串。
返回：对应的浮点数。
PS：与全局函数 parseFloat()一样。
###	Number.parseInt(string[, radix])
定义：可以根据给定的进制数的一个字符串数解析成整数。
参数：string要解析的值，radix一个介于2和36之间的整数(数学系统的基础)，表示上述字符串的基数。
##	实例方法
下列方法均返回为字符串。
###	numObj.toExponential([fractionDigits])
定义：以指数表示法返回该数值字符串表示形式。
参数：fractionDigits一个整数，用来指定小数点后有几位数字。
返回：一个用幂的形式 (科学记数法) 来表示Number 对象的字符串。
###	numObj.toFixed(digits)
定义：使用定点表示法来格式化一个数。
参数：digits小数点后数字的个数。
返回：所给数值的定点数表示法的字符串形式。
###	numObj.toPrecision(precision)
定义：以指定的精度返回该数值对象的字符串表示。
参数：precision一个用来指定有效数个数的整数。
返回：以定点表示法或指数表示法表示的一个数值对象的字符串表示。
###	numObj.toLocaleString([locales [, options]])
定义：返回这个数字在特定语言环境下的表示字符串。
参数：locales为指定本地要使用的编号系统，options为有下列属性（localeMatcher、style、currency等等但存在一定的兼容性）
返回：返回一个语言环境下的表示字符串。
PS：通常用于格式化为某种货币形式。
###	numObj.toString([radix])
定义：返回指定 Number 对象的字符串表示形式。
参数：radix指定要用于数字到字符串的转换的基数(从2到36)。
返回：转换后的字符串。
###	numObj.valueOf()
定义：返回一个被 Number 对象包装的原始值。
返回：表示指定 Number 对象的原始值的数字。

#	String
##	静态方法
###	String.fromCharCode(num1, ..., numN) 
定义：返回使用指定的Unicode值序列创建的字符串。
###	String.fromCodePoint(num1[, ...[, numN]])
定义：返回使用指定的代码点序列创建的字符串，但是这个方法不能识别 32 位的 UTF-16 字符（Unicode 编号大于0xFFFF）。
###	String.raw(callSite, ...substitutions) || String.raw`templateString`
是用来获取一个模板字符串的原始字面量值的。
##	实例属性
###	length
返回：字符串的长度。
###	N
返回：第N个字符串，但不能更改。
##	实例方法
###	str.charAt(index)
定义：从一个字符串中返回指定index的字符，缺省参数为0。
###	str.charCodeAt(index)
定义：返回给定索引处字符的 UTF-16 代码单元值的数字；如果索引超出范围，则返回 NaN。
###	str.codePointAt(index)
定义：返回 一个 Unicode 编码点值的非负整数。
###	str.concat(string2, string3[, ..., stringN])
定义：将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回。
###	str.includes(searchString[, index])
定义：判断一个字符串是否包含在另一个字符串中，根据情况返回true或false。
###  str.startsWith(searchString [, index])
定义：用来判断当前字符串是否是以另外一个给定的子字符串“开头”的，根据判断结果返回 true 或 false。
###	str.endsWith(searchString [, index]);
定义：判断当前字符串是否是以另外一个给定的子字符串“结尾”的，根据判断结果返回 true 或 false。
###	str.indexOf(searchValue[, index])
定义：第一次出现的指定值的索引，开始在Index进行搜索，否则返回-1。
###	str.lastIndexOf(searchValue[, index])
定义：返回指定值在调用该方法的字符串中最后出现的位置，如果没找到则返回 -1。
###	str.localeCompare(compareString[, locales[, options]])
定义：localeCompare() 方法返回一个数字来指示一个参考字符串是否在排序顺序前面或之后或与给定字符串相同。
###	str.match(regexp);
定义：当一个字符串与一个正则表达式匹配时， match()方法检索匹配项。
###	str.normalize([form]);
定义：会按照指定的一种 Unicode 正规形式将当前字符串正规化。
###	str.padStart(targetLength [, padString])
定义：会用一个字符串填充在当前字符串之前（如果需要的话则重复填充），返回填充后达到指定长度的字符串。
###	str.padEnd(targetLength [, padString])
定义：会用一个字符串填充在当前字符串之后（如果需要的话则重复填充），返回填充后达到指定长度的字符串。
###	str.repeat(count);
定义：返回一个新字符串，该字符串包含被连接在一起的指定数量的字符串的副本。
PS：参数从零开始。
###	str.replace(regexp|substr, newSubStr|function)
定义：返回一个由替换值替换一些或所有匹配的模式后的新字符串。
如果第一个参数为regexp第二个参数为function时，该函数参数说明
参数1：匹配模式的字符串。
参数2--：子表达是匹配的子字符串（就是捕获分组）。
倒数参数2：声明匹配在string中出现的位置。
倒数参数1：进行匹配的sting本身。
###	str.search(regexp)
定义：行正则表达式和 String对象之间的一个搜索匹配。
返回：如果匹配成功，则 search() 返回正则表达式在字符串中首次匹配项的索引。否则，返回 -1。
###	str.slice(beginSlice[, endSlice])
定义：提取一个字符串的一部分，并返回一新的字符串。
参数：beginSlice从该索引（以 0 为基数）处开始提取原字符串中的字符。如果值为负数，会被当做 sourceLength + beginSlice 看待，这里的sourceLength 是字符串的长度。
endSlice在该索引（以 0 为基数）处结束提取字符串，同样可为负数。
###	str.split([separator[, limit]])
定义：使用指定的分隔符字符串将一个String对象分割成字符串数组，以将字符串分隔为子字符串，以确定每个拆分的位置。 
###  str.substr(start[, length])
定义：返回一个字符串中从指定位置开始到指定字符数的字符。
PS：start >=str.length 或 length <= 0 返回空字符串;start < 0 则转换为start + str.length。
###  str.substring(indexStart[, indexEnd])
定义：返回一个字符串在开始索引到结束索引之间的一个子集, 或从开始索引直到字符串的末尾的一个子集。
PS：一些特殊情况。
如果 indexStart 等于 indexEnd，substring 返回一个空字符串。
如果省略 indexEnd，substring 提取字符一直到字符串末尾。
如果任一参数小于 0 或为 NaN，则被当作 0。
如果任一参数大于 stringName.length，则被当作 stringName.length。
如果 indexStart 大于 indexEnd，则 substring 的执行效果就像两个参数调换了一样。
###  str.toLowerCase()
定义：将调用该方法的字符串值转为小写形式，并返回新字符串。
###  str.toUpperCase()
定义：将调用该方法的字符串值转换为大写形式，并返回新字符串。
###  str.toLocaleLowerCase()
定义：根据任何特定于语言环境的案例映射，返回调用字符串值转换为小写的值。
PS：在大多数情况下，该方法产生的结果和调用toLowerCase()的结果相同（除土耳其等）。
###  str.toLocaleUpperCase()
定义：使用本地化（locale-specific）的大小写映射规则将输入的字符串转化成大写形式并返回结果字符串。
###  str.toString()
定义：返回指定对象的字符串形式。
###  str.trim()
定义：会从一个字符串的两端删除空白字符，返回一个新的字符串。
PS：str.trimLeft() 和 str.trimRight() 不是标准方法。
###  string[Symbol.iterator]
返回一个新的Iterator对象，它遍历字符串的代码点，返回每一个代码点的字符串值。
PS：下列内置类型拥有默认迭代器行为Array、String、Set、Map等，而Object没有。

#  Array
##  静态方法
在 ES2015 中， Class 语法允许我们为内置类型（比如 Array）和自定义类新建子类（比如叫 SubArray）。这些子类也会继承父类的静态方法，比如 SubArray.from()，调用该方法后会返回子类 SubArray 的一个实例，而不是 Array 的实例。
###  Array.from(arrayLike[, mapFn[, thisArg]])
定义：从一个类似数组或可迭代对象中创建一个新的数组实例。
参数：
  arrayLike想要转换成数组的伪数组对象或可迭代对象。
  mapFn (可选参数)如果指定了该参数，新数组中的每个元素会执行该回调函数。
  thisArg (可选参数)可选参数，执行回调函数 mapFn 时 this 对象。
返回：一个新的数组。
PS：Array.from(obj, mapFn, thisArg) 就相当于 Array.from(obj).map(mapFn, thisArg)，ES6之前的做法：Array.prototype.slice.call(arrayLike)。
###  Array.isArray(obj)
定义：确定传递的值是否是一个 Array。
返回：boolean。
PS：ES6之前的做法Object.prototype.toString.call(arg) === '[object Array]'。
###  Array.of(element0[, element1[, ...[, elementN]]])
定义：创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型。
参数：任意个参数，将按顺序成为返回数组中的元素。
返回：参数列表组成的数组。
PS：Array.of() 和 Array 构造函数之间的区别在于处理整数参数：Array.of(7) 创建一个具有单个元素 7 的数组，而 Array(7) 创建一个包含 7 个 undefined 元素的数组。
##  实例方法及属性
###  ary.length
返回：读写数组的长度。
###  修改器方法（改变原数组）
####   arr.copyWithin(target[, start[, end]]) 
定义：浅复制数组的一部分到同一数组中的另一个位置，并返回它，而不修改其大小。
参数：
target 0 为基底的索引，复制序列到该位置。如果是负数，target 将从末尾开始计算。如果 target 大于等于 arr.length，将会不发生拷贝。如果 target 在 start 之后，复制的序列将被修改以符合 arr.length。
start 0 为基底的索引，开始复制元素的起始位置。如果是负数，start 将从末尾开始计算。如果 start 被忽略，copyWithin 将会从0开始复制。
end 0 为基底的索引，开始复制元素的结束位置。copyWithin 将会拷贝到该位置，但不包括 end 这个位置的元素。如果是负数， end 将从末尾开始计算。如果 end 被忽略，copyWithin 将会复制到 arr.length。
返回值：操作原数组。
####  arr.fill(value[, start[, end]])  
定义：用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。
参数：
  value 用来填充数组元素的值。
  start 开始索引，默认为0。
  end 结束索引，默认为arr.length（不包括）。
返回：修改后的数组。
####  arr.push(element1, ..., elementN)
定义：将一个或多个元素添加到数组的末尾，并返回新数组的长度。
参数：
  elementN 被添加到数组末尾的元素。
返回：操作后的数组的长度。
####  arr.pop()
定义：从数组中删除最后一个元素，并返回该元素的值。此方法更改数组的长度。
返回：从数组中删除的元素(当数组为空时返回undefined)。
####  arr.reverse()
定义：将数组中元素的位置颠倒。
返回：颠倒数组中元素的位置，并返回该数组的引用。
####  arr.sort(compareFunction)
定义：可以根据指定方法对数组进行排序。
compareFunction 可选。用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的各个字符的Unicode位点进行排序。
返回：返回排序后的数组。原数组已经被排序后的数组代替。
####  arr.shift()
定义：从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。
返回：从数组中删除的元素; 如果数组为空则返回undefined。 
####  arr.unshift(element1, ..., elementN)
定义：将一个或多个元素添加到数组的开头，并返回新数组的长度。
参数：element1, ..., elementN 要添加到数组开头的元素。
返回：当一个对象调用该方法时，返回其 length 属性值。
####  array.splice(start, [deleteCount], [item1], [item2], ...)
定义：通过删除现有元素和/或添加新元素来更改一个数组的内容。
参数：
  start 开始修改的位置。
  deleteCount 移除数组元素的个数。
  item1、item2...为添加的元素。
返回：由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。
###  访问方法（不直接操作原理的数组）
####  old_array.concat(value1[, value2[, ...[, valueN]]])
定义：用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。
参数：valueN 将数组和/或值连接成新数组。
返回：新数组。
####  arr.includes(searchElement, [fromIndex])
定义：用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。
参数：
  searchElement 需要查找的元素值。
  fromIndex 从该索引处开始查找 searchElement。
返回：boolean。
####  arr.join([separator])
定义：将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。
参数：
  searchElement 需要查找的元素值。
  fromIndex 从该索引处开始查找 searchElement。
返回：string。
####  arr.slice([begin], [end])
定义：返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对象。
返回：一个含有提取元素的新数组。
####  arr.indexOf(searchElement[, fromIndex = 0])
定义：返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
返回：首个被找到的元素在数组中的索引位置; 若没有找到则返回 -1。
####  arr.lastIndexOf(searchElement[, fromIndex = arr.length - 1])
定义：返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
返回：数组中最后一个元素的索引，如未找到返回-1。
####  arr.toString()
定义：返回一个字符串，表示指定的数组及其元素。
返回：逗号分隔的字符串。
###  迭代方法
####  array.forEach(callback(currentValue, index, array){ //do something}, this)
定义：对数组的每个元素执行一次提供的函数。
返回：undefined。
PS：没有办法中止或者跳出 forEach 循环，需要跳出请使用循环代替。
已删除（使用delete方法等情况）或者未初始化的项将被跳过（但不会跳过那些值为 undefined、null 的项）。
####  array.map(callback(currentValue, index, array){ //do something}, this)
定义：创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。
返回：一个新数组，每个元素都是回调函数的结果。
####  arr.keys()
定义：返回一个新的Array迭代器，它包含数组中每个索引的键。
返回：一个新的 Array 迭代器对象。
####  arr.values()
定义：返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值。
返回：一个新的 Array 迭代器对象。
####  arr.entries()
定义：返回一个新的Array Iterator对象，该对象包含数组中每个索引的键/值对。
返回：一个新的 Array 迭代器对象。
####  arr.every(callback[, thisArg])
定义：测试数组的所有元素是否都通过了指定函数的测试。
返回：boolean。
####  arr.some(callback[, thisArg])
定义：测试数组中的某些元素是否通过由提供的函数实现的测试。
返回：boolean。
####  arr.filter(callback[, thisArg])
定义：创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 
返回：新数组。
####  arr.findIndex(callback[, thisArg])
定义：返回数组中满足提供的测试函数的第一个元素的索引。否则返回-1。
返回：当某个元素通过 callback 的测试时，返回数组中的一个值的索引，否则返回 -1。
####  arr.find(callback[, thisArg])
定义：返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。
返回：当某个元素通过 callback 的测试时，返回数组中的一个值，否则返回 undefined。
####  arr.reduce(callback[, initialValue])
定义：对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。
参数：
  callback 执行数组中每个值的函数，包含四个参数：
    accumulato 累加器累加回调的返回值; 它是上一次调用回调时返回的累积值，或initialValue（如下所示）。
    currentValue 数组中正在处理的元素。
    currentIndex可选 数组中正在处理的当前元素的索引。 如果提供了initialValue，则索引号为0，否则为索引为1。
    array可选 调用reduce的数组。
  initialValue 可选用作第一个调用 callback的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。  
返回：函数累计处理的结果。
####  arr.reduceRight(callback[, initialValue])
定义：接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值。
####  arr[Symbol.iterator]()  
定义：默认为数组不说了迭代器，@@iterator 属性和 values() 属性的初始值均为同一个函数对象。
返回：数组的 iterator 方法，默认情况下与 values() 返回值相同。
####  arr.flatten(depth)
定义：会递归到指定深度将所有子数组连接，并返回一个新数组。
参数：depth 可选指定嵌套数组中的结构深度，默认值为1。
返回：一个将子数组连接的新数组。
####  arr.flatMap(function callback(currentValue[, index[, array]]) { // 返回新数组的元素}[, thisArg])  
定义：首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 和 深度值1的 flatten 几乎相同，但flatMap通常在合并成一种方法的效率稍微高一些。
返回：一个新的数组，其中每个元素都是回调函数的结果，并且结构深度 depth 值为1。

#  Proxy && Reflect
Proxy是一个构造函数（对对象的访问进行拦截），Reflect（操作对象提供的API）。
Reflect它与Proxy对象的方法是一一对应的，这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。
##  Proxy
Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。
Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。
```
Proxy方法：target参数表示所要拦截的目标对象，handler参数也是一个对象，用来定制拦截行为
var proxy = new Proxy(target, handler);

var person = {
  name: "张三"
};

var proxy = new Proxy(person, {
  get: function(target, property) {
    if (property in target) {
      return target[property];
    } else {
      throw new ReferenceError("Property \"" + property + "\" does not exist.");
    }
  }
});

proxy.name // "张三"
proxy.age // 抛出一个错误
```
[Proxy使用详解](http://pinggod.com/2016/%E5%AE%9E%E4%BE%8B%E8%A7%A3%E6%9E%90-ES6-Proxy-%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF/)
#  Reflect
将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，从Reflect对象上可以拿到语言内部的方法。
// 下列方法说明：target为目标对象，name为某个属性，receiver为如果name属性设置了赋值函数，则为函数的this指向
Reflect.apply(target, thisArg, args)
  Reflect.apply方法等同于Function.prototype.apply.call(func, thisArg, args)，用于绑定this对象后执行给定函数。
Reflect.construct(target, args)
  Reflect.construct方法等同于new target(...args)，这提供了一种不使用new，来调用构造函数的方法。
Reflect.get(target, name, receiver)
  Reflect.get方法查找并返回target对象的name属性，如果没有该属性，则返回undefined。
Reflect.set(target, name, value, receiver)
  Reflect.set方法设置target对象的name属性等于value。
Reflect.defineProperty(target, name, desc)
  Reflect.defineProperty方法基本等同于Object.defineProperty，用来为对象定义属性。未来，后者会被逐渐废除，请从现在开始就使用Reflect.defineProperty代替它。
Reflect.deleteProperty(target, name)
  Reflect.deleteProperty方法等同于delete obj[name]，用于删除对象的属性。
Reflect.has(target, name)
  Reflect.has方法对应name in obj里面的in运算符。
Reflect.ownKeys(target)
  Reflect.ownKeys方法用于返回对象的所有属性，基本等同于Object.getOwnPropertyNames与Object.getOwnPropertySymbols之和。
Reflect.isExtensible(target)
  Reflect.isExtensible方法对应Object.isExtensible，返回一个布尔值，表示当前对象是否可扩展。
Reflect.preventExtensions(target)
  Reflect.preventExtensions对应Object.preventExtensions方法，用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功。
Reflect.getOwnPropertyDescriptor(target, name)
  Reflect.getOwnPropertyDescriptor基本等同于Object.getOwnPropertyDescriptor，用于得到指定属性的描述对象，将来会替代掉后者。
Reflect.getPrototypeOf(target)
  Reflect.getPrototypeOf方法用于读取对象的__proto__属性，对应Object.getPrototypeOf(obj)。
Reflect.setPrototypeOf(target, prototype)
  Reflect.setPrototypeOf方法用于设置对象的__proto__属性，返回第一个参数对象，对应Object.setPrototypeOf(obj, newProto)。

#  Event
Event接口表示在DOM中发生的任何事件; 一些是用户生成的（例如鼠标或键盘事件），而其他由API生成(例如指示动画已经完成运行的事件，视频已被暂停等等)。有许多类型的事件，其中一些使用基于主要事件接口的其他接口。事件本身包含所有事件通用的属性和方法。
##  属性
###  bubbles（只读）
定义：用来表示该事件是否在DOM中冒泡的boolean值。
###  cancelBubble（废弃）
定义：获取或设置当前事件是否要取消冒泡（使用e.stopPropagation()代替）。
###  cancelable（只读）
定义：表示这个事件是否可以取消默认行为（阻止默认行为e.preventDefault()）。
###  composed（只读）
定义：表示该事件是否可以Shadow DOM 传递到一般的 DOM。
###  currentTarget（只读）
定义：当前注册事件的对象的引用，这个值会在传递途中发生变化。
###  deepPath
定义：返回事件冒泡过程所有经过的节点所构成的Array数组。
###  defaultPrevented（只读）
定义：返回是否已经调用了e.preventDefault()来阻止默认行为。
###  eventPhase（只读）
定义：返回事件流正在哪个阶段。
###  returnValue（废弃）
定义：获取或设置事件的默认操作是否已被阻止。
###  target（只读）
定义：返回一个触发事件的对象的引用（ie的srcElement）。
###  timeStamp（只读）
定义：事件创建时的时间戳，毫秒级别。
###  type（只读）
定义：返回一个字符串（不区分大小写）, 表示该事件对象的事件类型。
###  isTrusted（只读）
定义：指明事件是否是由浏览器（当用户点击实例后）或者由脚本（使用事件的创建方法，例如event.initEvent）启动。
###  target与currentTarget的区别
event.target返回触发事件的元素
event.currentTarget返回绑定事件的元素
[event对象中 target和currentTarget 属性的区别](https://www.cnblogs.com/yewenxiang/p/6171411.html)
##  方法
###  document.createEvent("UIEvents")
创建一个新的事件（Event），随之必须调用自身的 init 方法进行初始化。
###  event.initEvent(type, bubbles, cancelable)
定义：Event.initEvent() 方法可以用来初始化由Document.createEvent() 创建的 event 实例，且在触发之前event.dispatchEvent()。
###  event.preventDefault()
定义：如果此事件没有需要显式处理，那么它默认的动作也不要做（因为默认是要做的）。
###  event.stopPropagation()
定义：阻止捕获和冒泡阶段中当前事件的进一步传播（只阻止当前侦听器）。
###  event.stopImmediatePropagation()
定义：阻止调用相同事件的其他侦听器。














































