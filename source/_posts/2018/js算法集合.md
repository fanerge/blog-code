---
title: js算法集合
date: 2018-04-08 22:20:17
tags: '数据结构和算法'
categories: '数据结构和算法'
copyright: true
---
#	判断文本是否为回文
定义：如果将一个文本翻转过来，能和原文本完全相等，那么就可以称之为“回文”。
##	方法一（字符串、数组内置方法）
```
/*
* 判断文字是否为回文
* @param {string|number} val 需要判断的文字
* @return {boolean} bool 是否为回文 
*/
function isPalindrome1(val){
	// 允许输入字符串和数字和布尔值
	if (typeof val !== 'string') val = val.toString();
	let newVal = val.split('').reverse().join('');
	
	return val === newVal;
}

isPalindrome1(121) // true
isPalindrome1('yuzuy') // true
```
// PS：方法简单，但效率不高，会产生一个新的变量
##	方法二（循环）
```
/*
* 判断文字是否为回文
* @param {string|number} val 需要判断的文字
* @return {boolean} bool 是否为回文 
*/
function isPalindrome2(val){
	val = val + ''; // 非字符串转化为字符串
	
	// 这里为什么 i <= j 呢？如果中间只有一个字符，是不需要比较的，它肯定等于它本身！！！
	for(let i = 0, j = val.length - 1; i < j; i++, j--){
		if(val.charAt(i) !== val.charAt(j)){
			return false;
		}
	}
	
	return true;
}

isPalindrome2(121) // true
isPalindrome2('yuzuy') // true
```
PS：网上还有其他解法，大多为以上两种的变形。

#	反转字符串
##	方法一（字符串、数组内置方法））
借用反转字符串的方法
```
/*
* 反转字符串
* @param {string} val 需要反转的字符串
* @return {string} str 反转后的字符串
*/
function reverseVal1(val){
	if (typeof val !== 'string') return;
	
	return val.split('').reverse().join('');
}
```
##	方法二（循环）
循环系列
```
/*
* 反转字符串
* @param {string} val 需要反转的字符串
* @return {string} str 反转后的字符串
*/
function reverseVal2(val){
	if (typeof val !== 'string') return;
	
	let str = '',
		i = 0,
		len = val.length;
	while(i < len){
		str += val.charAt(len - 1 - i);
		i++; 
	}
	
	return str;
}

/*
* 反转字符串
* @param {string} val 需要反转的字符串
* @return {string} str 反转后的字符串
*/
function reverseVal3(val){
	if (typeof val !== 'string') return;
	
	let str = '',
		len = val.length;
	for(let i = len - 1; i >= 0; i--){
		str += val.charAt(i)
	}
	
	return str;
}
```
测试：reverseVal('abc') // 'cba'

#	阶乘
##	方法一（递归）
```
/*
* 阶乘
* @param {number} n 需要求的阶乘
* @return {number} 阶乘值
*/
function factorialize1(n){
	if(typeof n !== 'number') throw new Error('参数必须为整整')
	if(n === 1) return 1;

	// 建议不要使用 arguments.callee，目前已经废弃了。
	return n * factorialize1(n - 1);
}
```
PS：上面代码是一个阶乘函数，计算n的阶乘，最多需要保存n个调用记录，复杂度 O(n) 。
递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。
##	方法二（ES6尾调用优化）
（递归优化版）
```
/*
* 阶乘
* @param {number} n 需要求的阶乘
* @return {number} 阶乘值
*/
function factorialize2(n, total = 1){
	if(typeof n !== 'number' || typeof total !== 'number') throw new Error('参数必须为整整')
	if(n === 1) return total;

	return factorialize2(n - 1, n * total)
	// f(3) => f(2, 3 * 2) => f(1, 6) => 6
}
```
PS：[ES6尾调用优化](http://es6.ruanyifeng.com/#docs/function#尾调用优化)但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。
尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。
##	方法三（循环）
```
/*
* 阶乘
* @param {number} n 需要求的阶乘
* @return {number} 阶乘值
*/
function factorialize3(n){
	if(typeof n !== 'number') throw new Error('参数必须为整整')
	if(n === 1) return 1;	
	let total = 1;

	while(n>1){
		total = n * total;
		n--;
	}

	return total;
}
```
测试：factorialize1(3) // 6

#	随机生成长度为n字符串
##	方法一
```
/*
* 生成指定长度的随机字符串
* @param {number} n 生成字符串个数
* @return {string} str 反转后的字符串
*/
function randomString1(n){
	let str = 'abcdefghijklmnopqrstuvwxyz0123456789';
	let tem = '',
		i = 0;
	
	// Math.random 函数产生值的范围[0,1)
	while(i<n){
		tem += str.charAt(Math.floor(Math.random() * str.length))
		i++;
	}
	
	return tem;
}
```
PS：Math.round(Math.random() * (str.length - 1))
Math.ceil(Math.random() * (str.length - 1))
Math.floor(Math.random() * str.length)
这三种方式等价，都能生成[0, str.length-1]随机数
##	方法二（进制转化）
```
/*
* 生成指定长度的随机字符串
* @param {number} n 生成字符串个数
* @return {string} 反转后的字符串
*/
function randomString2(n){
	return Math.random().toString(36).substr(2).slice(0, n)
}
```
PS：该方法原理为随机产生的数转换为指定进制字符串
toString(n)，n为[2,36]，n<=10时只产生0-9也就是10进制数字
该方法有个缺点，产生字符串的长度有一定的限制。
##	方法三（随机码点）
```
/*
* 生成指定长度的随机字符串
* @param {number} n 生成字符串个数
* @return {string} str 反转后的字符串
*/
function randomString3(n){
	let str = '';
	
	function randomChar(){
		let l = Math.floor(Math.random() * 62);
		if(l < 10) return l; // 数字部分 0-9
		if(l < 36) return String.fromCharCode(l + 55); // 大写字母
		
		return String.fromCharCode(l + 61); // 小写字母
	}
	
	while(str.length < n) str += randomChar();
	
	return str;
}
```
PS：可以参考对于的[ASCII码表](https://baike.baidu.com/item/ASCII/309296?fr=aladdin)。
测试：randomString1(3) // '1sd'

#	数组去重
##	方法一（ES6的Set数据结构）
```
/*
* 数组去重
* @param {array} ary 需要去重的数组
* @return {array} 去重后的数组
*/
function unique1(ary){
	return [...new Set(ary)];
}
```
##	方法二（对象的key唯一性）
```
/*
* 数组去重
* @param {array} ary 需要去重的数组
* @return {array} 去重后的数组
*/
function unique2(ary){
	let obj = {},
		i = 0,
		len = ary.length;
	
	while(i < len){
		if(!obj[ary[i]]){
			obj[ary[i]] = true; // 如果不存在
		}
		i++;
	}
	return Object.keys(obj);
}
```
PS：该方法存在一定问题，数组的元素全部被转化为字符串，因为ES6之前对象的key只能是字符串。
会把数字1和字符串'1'，会被视为同一个值。
##	方法三（临时数组判断插入）
```
/*
* 数组去重
* @param {array} ary 需要去重的数组
* @return {array} 去重后的数组
*/
function unique3(ary){
	let tem = [],
		i = 0,
		len = ary.length;
	
	while(i < len){
		// tem.indexOf() === -1 同理
		!tem.includes(ary[i]) ? tem.push(ary[i]) : '';
		i++;
	}
	
	return tem;
}
```
##	方法四（判断首次出现的位置）
如果当前数组的第i项在当前数组中第一次出现的位置不是i，那么表示第i项是重复的，忽略掉。否则存入结果数组。
```
/*
* 数组去重
* @param {array} ary 需要去重的数组
* @return {array} 去重后的数组
*/
function unique4(ary){
	let tem = [ary[0]],
		len = ary.length;
	
	for(let i = 1; i < len; i++ ){
		// 核心，首次的索引出现是否为当前的索引
		if(ary.indexOf(ary[i]) === i) tem.push(ary[i]);
	}
	
	return tem;
}
```
##	方法五（排序后逐个比较插入）
给传入数组排序，排序后相同值相邻，然后遍历时新数组只加入不与前一值重复的值。
```
/*
* 数组去重
* @param {array} array 需要去重的数组
* @return {array} 去重后的数组
*/
function unique5(array){
	let ary = array.slice();
	ary.sort();
	let tem = [ary[0]];
	for(let i = 0, len = ary.length; i < len; i++){
		ary[i] !== tem[tem.length - 1] ? tem.push(ary[i]) : '';
	}
	
	return tem;
}
```
PS：返回的数组顺序发生了改变。
##	方法六
获取没有重复的最右一值放入新数组（检测到有重复值时终止当前循环同时进入顶层循环的下一轮判断）。
```
/*
* 数组去重
* @param {array} ary 需要去重的数组
* @return {array} 去重后的数组
*/
function unique6(ary){
	let tem = [];
	for(let i = 0, len = ary.length; i < len; i++){
		for(let j = i + 1; j < len; j++){
			if(ary[i] === ary[j]) j = ++i;
		}
		tem.push(ary[i])
	}
	
	return tem;
}
```
测试：unique1([1, 2, 3, 2]) // [1, 2, 3]

#  出现次数最多的字符
##	方法一（对象key的唯一性进行累加）
```
function maxNum1(str){
	if(typeof(str) !== 'string') str = str.toString();
	let obj = {},
		maxChar = []; // 使用数组保存出现最多次的某些字符
	str.split('').forEach( (val) => {
		if(!obj[val]){
			let demo = obj[val] = 1;

		}else{
			obj[val]++;
		}
	})

	let maxCount =  Math.max.apply(null, Object.values(obj))

	// forEach方法总是返回 undefined 且 没有办法中止或者跳出 forEach 循环。
	Object.entries(obj).forEach( item => {
		if(item[1] == maxCount){
			maxChar.push(item[0])
		}
	})

	return maxChar;
}		
```
测试：maxNum1('11223333') // '3'

#	数组扁平化
实现方法：Array.prototype.flatten(depth)，参数depth表示需要扁平化的层数，返回一个新的数组。 
##	方法一（递归遍历数组拼接）
```
function flatten1(ary){
	let tem = [],
		i = 0,
		len = ary.length;

	while(i < len){
		if(Array.isArray(ary[i])){
			// 递归进行上面步骤
			// [].concat(...ary)，它的参数可以为数组或值，作用为将数组或值连接成新数组。
			tem = tem.concat(flatten1(ary[i]))
		}else{
			tem.push(ary[i]);
		}
		i++;
	}

	return tem;
}
```
PS：可以处理多层数组。
##	方法二（reduce结合concat）
```
function flatten2(ary){

	return ary.reduce((pre, cur) => {
		return pre.concat(Array.isArray(cur) ? flatten2(cur) : cur)
	}, [])
	
}
```
PS：可以处理多层数组。
##	方法三（转化为字符串）
```
function flatten2(ary){

	return ary.toString().split(',')

}
```
PS：返回的数组项将为字符串。
##	方法四（解构数组）
```
function flatten4(ary){

	let tem = []
	ary.forEach(item => {
		if(Array.isArray(item)){
			tem = tem.concat(...item);
		}else{
			tem = tem.concat(item);
		}
	})

	return tem;
}
```
PS：只能处理2维数组。
测试：getMaxProfit1([1, 2, 3, [4, 5, 6]]) // [1, 2, 3, 4, 5, 6]

#	数组中最大差值
##	方法一
```
function getMaxProfit1(ary){
	return Math.max.apply(null, ary) - Math.min.apply(null, ary);
}
```
测试：getMaxProfit1([1, 2, 3, 4]) // 3 

#	斐波那契数列
这里我们只实现通项公式
##	方法一
```
function fib1(n){
	if(n === 1 || n === 2){
		return 1;
	}

	return fib1(n - 1) + fib1(n - 2);
}
```
PS：时间复杂度为O(2^n)，空间复杂度为O(n)

##	方法二
```
function fib2(n){
	let tem = [1, 1];
	if(n === 1 || n === 2){
		return 1;
	}

	// 数组索引从0开始，数列索引从1开始
	for(let i = 2; i < n; i++){
		tem[i] = tem[i-1] + tem[i-2];
	}

	return tem[n-1];
}
```
PS：时间复杂度为O(n)，空间复杂度为O(n)
##	方法三
```
function fib2(n){
	let prev = 1, 
		next = 1,
		res;
	for(let i = 2; i < n; i++){
		res = prev + next;  
		prev = next; 
		next = res;
	}	

	return res;
}
```
PS：时间复杂度为O(n)，空间复杂度为O(1)
测试：fib2(3) // 2 

#	判断是否为质数（prime number）素数
质数：只能被1和自己整除且大于1的数。
合数：数大于1且因数多余2个（大于1的数质数的补集）。
##	方法一（循环）
```
function isPrimeNumber1(n){
	if(n < 2) return false;
	if(n === 2) return true; // 最小的质数

	for(let i = 2; i < n; i++){
		if(n % i === 0){
			return false;
		}
	}

	return true;
}
```
测试：isPrimeNumber1(2) // true
##	方法二（正则）
```
function isPrimeNumber1(n){
  return n<2?false:!/^(11+?)\1+$/.test(Array(n+1).join('1'))
}
```
PS：该方法很巧妙，于2018-04-25在掘金上发现。
[方法详解](https://juejin.im/post/5adeb462f265da0b9c104358)

#	最小公约数
```
function greatestCommonDivisor1(a, b){
	if(a < 0 || b < 0) throw new Error('参数只能为正整数');
	if(a < 2 || b < 2) return 1;
	let min = a,
		max = b,
		arymin = [];

	if(a > b) {
		min = b;
		max = a;
	}

	for(let i = 1; i <= min; i++){
		if(min % i === 0){
			arymin.push(i);
			console.log(1)
		}
	}

	arymin.reverse();

	for(let j = 0, len = arymin.length; j < len; j++){
		if(max % arymin[j] === 0){
			return arymin[j];
		}
	}

}
```
测试：greatestCommonDivisor1(5, 10) // 5

#	金额转大写
```
function money2Chinese(num) {
  if(typeof num) throw new Error('参数为数字')
  let strOutput = ""
  let strUnit = '仟佰拾亿仟佰拾万仟佰拾元角分'
  num += "00"
  const intPos = num.indexOf('.')
  if (intPos >= 0) {
    num = num.substring(0, intPos) + num.substr(intPos + 1, 2)
  }
  strUnit = strUnit.substr(strUnit.length - num.length)
  for (let i = 0; i < num.length; i++) {
    strOutput += '零壹贰叁肆伍陆柒捌玖'.substr(num.substr(i, 1), 1) + strUnit.substr(i, 1)
  }
  
  return strOutput.replace(/零角零分$/, '整').replace(/零[仟佰拾]/g, '零').replace(/零{2,}/g, '零').replace(/零([亿|万])/g, '$1').replace(/零+元/, '元').replace(/亿零{0,3}万/, '亿').replace(/^元/, "零元");
}
```
测试：money2Chinese(1234) // 壹仟贰佰叁拾肆元整














