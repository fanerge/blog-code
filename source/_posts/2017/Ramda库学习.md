---
title: Ramda库学习
date: 2017-08-13 19:28:31
category: "函数式编程"
tags: ['js','函数式编程','ramda']
---
[学习源代码](https://github.com/fanerge/Ramda-study.git)
经过4个晚上的学习，了解了ramda库120多个api的用法，下一步继续深入学习函数式编程。
继续学习js函数式编程，这里贴出阮老师总结的Ramda的优点：
	1.	Ramda 的数据一律放在最后一个参数，理念是"function first，data last"。
	2.	所有方法都支持柯里化。

####	一、比较运算（gt、gte、lt、lte、eauals、eqBy）
	gt：判断第一个参数是否大于第二个参数。
	```
	let gt1 = R.gt(2)(1);
	// console.log(gt1); true
	let gt2 = R.gt('a')('c');
	// console.log(gt2); false
	```
	gte：判断第一个参数是否大于等于第二个参数。
	```
	let gte1 = R.gte(2)(2);
	// console.log(gte1); true
	let gte2 = R.gte('a')('c');
	// console.log(gte2); false
	```
	lt：判断第一个参数是否小于第二个参数。
	```
	let lt1 = R.lt(2)(1);
	// console.log(lt1); false
	let lt2 = R.lt('a')('z');
	// console.log(lt2); true	
	```
	lte：判断第一个参数是否小于等于第二个参数。
	```
	let lte1 = R.lte(1)(2);
	// console.log(lte1); true
	let lte2 = R.lte('a')('v');
	// console.log(lte2); true	
	```
	eauals：比较两个值是否相等（支持对象的比较）。
	```
	let equals1 = R.equals(1)(1);
	// console.log(equals1); true
	let equals2 = R.equals(1)('1');
	// console.log(equals2); false
	let equals3 = R.equals([1, 2])([1,2]);
	// console.log(equals3); // true
	let equals4 = R.equals({a:1})({a:2});
	// console.log(equals4); // false
	```
	eqBy：比较两个值传入指定函数的运算结果是否相等。
	```
	let eqBy1 = R.eqBy(Math.abs, 5, -5);
	console.log(eqBy1);
	```
	
####	二、数学运算（add、subtract、multiply、divide）
	add：返回两个值的和。
	```
	let add1 = R.add(1)(10);
	// console.log(add1); // 11	
	```
	subtract：返回第一个参数减去第二个参数的差。
	```
	let subtract1 = R.subtract(10)(2);
	//console.log(subtract1); // 8
	```
	multiply：返回两个值的积。
	```
	let multiply1 = R.multiply(5)(6);
	// console.log(multiply1); // 30
	```
	divide：返回第一个参数除以第二个参数的商。
	```
	let divide1 = R.divide(5)(10);
	// console.log(divide1); // 0.5
	```
	
####	三、逻辑运算（either、both、allPass）
	either：接收两个参数，相当于 || 运算（或）。	
	```
	let gt10 = x => x > 10;
	let even = x => x % 2 === 0;
	let either1 = R.either(gt10)(even);
	let either2 = either1(18);
	let either3 = either1(3);
	// console.log(either2); // true
	//console.log(either3); // false
	```
	both：接收两个参数，相当于 && 运算（且）。	
	```
	var gt10 = x => x > 10;
	var even = x => x % 2 === 0;
	let both1 = R.both(gt10)(even);
	let both2 = both1(18);
	let both3 = both1(4);
	// console.log(both2); // true
	// console.log(both3); // false
	```
	allPass：接受一个函数数组作为参数，只有它们都返回true，才返回true，否则返回false。
	```
	var gt10 = x => x > 10;
	var lt20 = x => x < 20;
	var even = x => x % 2 === 0;
	let allPass1 = R.allPass([gt10, even, lt20]);
	let allPass2 = allPass1(16);
	let allPass3 = both1(13);
	console.log(allPass2); // true
	console.log(allPass3); // false	
	```
	
####	四、字符串（split、test、match）
	split：按照指定分隔符将字符串拆成一个数组。	
	```
	let split1 = R.split('.')('a.b.c.fanerge');
	// console.log(split1); // ["a", "b", "c", "fanerge"]	
	```
	test：判断一个字符串是否匹配给定的正则表达式。
	```
	let test1 = R.test(/^f/)('fanerge');
	// console.log(test1); // true
	```
	match：返回一个字符串的匹配结果。
	```
	let match1 = R.match(/([a-z]a)/g)('bananas')
	// console.log(match1); // ["ba", "na", "na"]
	let match2 = R.match(/a/)('n');
	// console.log(match2); // []
	```
	
####	五、函数
#####	5.1 函数的合成（compose、pipe、converge）
	compose：将多个函数合并成一个函数，从右到左执行
	```
	let compose1 = R.compose(Math.abs, R.add(1), R.multiply(2))(4)
	console.log(compose1); // 9	
	```
	pipe：将多个函数合并成一个函数，从左到右执行。
	```
	let pipe1 = R.pipe(Math.abs, R.add(1), R.multiply(2))(4);
	console.log(pipe1); // 10
	```
	converge：接受两个参数，第一个参数是函数，第二个参数是函数数组。传入的值先使用第二个参数包含的函数分别处理以后，再用第一个参数处理前一步生成的结果。
	```
	var sumOfArr = arr => {
		let sum = 0;
		arr.forEach(i => sum += i);
		return sum;
	};
	var lengthArr = arr => arr.length;
	var converge1 = R.converge(R.divide, [sumOfArr, lengthArr]);
	var converge2 = converge1([1, 2, 3, 4, 5]);
	// console.log(converge2); // 3
	```
####	5.2 柯里化（curry、partial、partialRight、useWith、memoize、complement）
	curry：将多个参数的函数，转化成单参数的形式。
	```
	var addFourNumbers = (a, b, c, d) => a + b + c + d;
	var curriedAddFourNumbers = R.curry(addFourNumbers);
	var f = curriedAddFourNumbers(1)(2)(3)(4);
	// console.log(f); // 10
	```
	partial：允许多参数的函数接受一个数组，指定最左边的部分参数。
	```
	var multiply2 = (a, b) => a * b;
	var double = R.partial(multiply2, [3]);
	// console.log(double(2)); // 6
	```
	partialRight：与partial类似，但数组指定的参数为最右边的参数。
	```
	var greet = (salutation, title, firstName, lastName) =>
	  salutation + ', ' + title + ' ' + firstName + ' ' + lastName + '!';
	var greetMsJaneJones = R.partialRight(greet, ['Ms.', 'Jane', 'Jones']);
	var dd = greetMsJaneJones('Hello'); 
	console.log(dd); // 'Hello, Ms. Jane Jones!'
	```
	useWith：接受一个函数fn和一个函数数组fnList作为参数，返回fn的柯里化版本。该新函数的参数，先分别经过对应的fnList成员处理，再传入fn执行。
	```
	var decreaseOne = x => x - 1;
	var increaseOne = x => x + 1;
	var useWith1 = R.useWith(Math.pow, [decreaseOne, increaseOne])(3)(4);
	console.log(useWith1) // 2^5 32
	```
	memoize：返回一个函数，会缓存每一次的运行结果。
	```
	var productOfArr = arr => {
	  var product = 1;
	  arr.forEach(i => product *= i);
	  return product;
	};
	var count = 0;
	var factorial = R.memoize(n => {
	  count += 1;
	  return productOfArr(R.range(1, n + 1));
	});
	var memoize1 = factorial(5);
	console.log(memoize1, count); // 120, 1
	```
	complement：返回一个新函数，如果原函数返回true，该函数返回false；如果原函数返回false，该函数返回true。
	```
	var gt10 = x => x > 10;
	var lte10 = R.complement(gt10);
	var complement1 = lte10(11);
	// console.log(complement1); // false	
	```
####	5.3 函数的执行（binary、tap、zipWith、apply、applySpec、ascend、descend）
	binary：参数函数执行时，只传入最前面两个参数。
	```
	var takesThreeArgs = function (a, b, c) {
		return [a, b, c];
	};
	var takesTwoArgs = R.binary(takesThreeArgs);
	var binary1 = takesTwoArgs(1, 2, 3);
	// console.log(binary1); // [1, 2, undefined]
	```
	tap：将一个值传入指定函数，并返回该值。
	```
	var sayX = x => console.log('x is' + x);
	var tap1 = R.tap(sayX)(100);
	// console.log(tap1); // x is100 100
	```
	zipWith：将两个数组对应位置的值，一起作为参数传入某个函数。
	```
	var f = (x, y) => {
		return x + y;
	};
	var zipWith1 = R.zipWith(f)([1, 2, 3])([4, 5, 6]);
	// console.log(zipWith1); // [5, 7, 9]
	```
	apply：将数组转成参数序列，传入指定函数。
	```
	var nums = [1, 2, 3, -99, 42, 6, 7];
	var apply1 = R.apply(Math.max)(nums);
	// console.log(apply1); // 42
	```
	applySpec：返回一个模板函数，该函数会将参数传入模板内的函数执行，然后将执行结果填充到模板。
	```
	var getMetrics = R.applySpec({
		sum: R.add,
		nested: {mul: R.multiply}
	});
	var applySpec1 = getMetrics(2, 4);
	//console.log(applySpec1); // { sum: 6, nested: { mul: 8 } }
	```
	ascend：返回一个升序排列的比较函数，主要用于排序。
	```
	var byAge = R.ascend(R.prop('age'));
	var people = [
		{name: 'fan', age: 11},
		{name: 'yu', age: 8},
		{name: 'zhen', age: 9}
	];
	var ascend1 = R.sort(byAge)(people);
	// console.log(ascend1); // [ {name: 'yu', age: 8}, {name: 'zhen', age: 9}, name: 'fan', age: 11}];
	```
	ascend：返回一个升序排列的比较函数，主要用于排序。
	```
	var byAge = R.descend(R.prop('age'));
	var people = [
		{name: 'fan', age: 11},
		{name: 'yu', age: 8},
		{name: 'zhen', age: 9}
	];
	var descend1 = R.sort(byAge)(people);
	// console.log(descend1); // [ {name: 'fan', age: 11}, {name: 'zhen', age: 9}, {name: 'yu', age: 8}];
	```
	
####	六、数组
#####	6.1 数组的特性判断（contains、all、any、none）
	contains：如果包含某个成员，返回true。		
	```
	var contains1 = R.contains(3)([1,2]);
	// console.log(contains1); // false	
	```
	contains：如果包含某个成员，返回true。		
	```
	var contains1 = R.contains(3)([1,2]);
	var contains2 = R.contains({name: 'fan'})([{name: 'fan'},2]);
	// console.log(contains1); // false
	// console.log(contains2); // true
	```
	all：所有成员都满足指定函数时，返回true，否则返回false		
	```
	var equals3 = R.equals(3);
	var all1 = R.all(equals3)([3, 3]);
	// console.log(all1); // true
	```
	any：只要有一个成员满足条件，就返回true。	
	```
	var lessThan0 = R.flip(R.lt)(0);
	var lessThan2 = R.flip(R.lt)(2);
	var any1 = R.any(lessThan0)([1, 2]);
	// console.log(any1); // false
	var any2 = R.any(lessThan2)([1, 2]);
	// console.log(any2); // true
	```
	none：没有成员满足条件时，返回true。	
	```
	var isEven = n => n % 2 === 0;
	var none1 = R.none(isEven)([1, 3, 5, 7, 9, 11]) // true
	var none2 = R.none(isEven)([1, 3, 5, 7, 8, 11]) // false
	console.log(none2); // true
	```
#####	6.2 数值的截取和添加（head、last、tail、init、nth、take、slice、remove、insert、insertAll、prepend、append、intersperse、join）
	head：返回数组的第一个成员。	
	```
	var head1 = R.head([1, 2, 3]);
	// console.log(head1); // 1
	```
	last：返回数组的最后一个成员。	
	```
	var last1 = R.last('fan');
	console.log(last1); // n
	```
	tail：返回第一个成员以外的所有成员组成的新数组。	
	```
	var tail1 = R.tail([1, 2, 3]);
	// console.log(tail1);  // [2, 3]
	```
	init：返回最后一个成员以外的所有成员组成的新数组。	
	```
	var init1 = R.init([1, 2, 3]);
	// console.log(init1);  // [1, 2]
	```
	nth：取出指定位置的成员。
	```
	var list = [1, 2, 3, 4];
	var nth1 = R.nth(0)(list);
	// console.log(nth1); // 1
	```
	take：取出前 n 个成员。
	```
	var take1 = R.take(3)([1, 2, 3, 4]);
	// console.log(take1); // [1, 2, 3]
	```
	takeLast：取出后 n 个成员。
	```
	var takeLast1 = R.takeLast(3)([1, 2, 3, 4]);
	// console.log(takeLast1); // [2, 3, 4]
	```
	slice：从起始位置（包括）开始，到结束位置（不包括）为止，从原数组截取出一个新数组。
	```
	var slice1 = R.slice(1, 3)([1, 2, 3, 4]);
	// console.log(slice1); // [2, 3]
	```
	remove：移除开始位置后的n个成员。
	```
	var remove1 = R.remove(2, 3)([1, 2, 3, 4, 5, 6, 7, 8]);
	// console.log(remove1); // [1, 2, 6, 7, 8]
	```
	insert：在指定位置插入给定值。
	```
	var insert1 = R.insert(2, 'x')([1, 2, 3]);
	// console.log(insert1); // [1, 2, "x", 3]
	```
	insertAll：在指定位置，插入另一个数组的所有成员。
	```
	var insertAll1 = R.insertAll(2, ['x', 'y', 'z'])([1, 2, 3, 4]);
	// console.log(insertAll1); // [1, 2, "x", "y", "z", 3, 4]
	```
	prepend：在数组头部插入一个成员。
	```
	var prepend1 = R.prepend('fee')(['ss', 'ee']);
	// console.log(prepend1); // ["fee", "ss", "ee"]
	```
	append：在数组尾部追加新的成员。
	```
	var append1 = R.append('test')(['ss']);
	// console.log(append1); // ["ss", "test"]
	```
	intersperse：在数组成员之间插入表示分隔的成员。
	```
	var intersperse1 = R.intersperse('/')(['aa', 'bb', 'cc']);
	// console.log(intersperse1); // ["aa", "/", "bb", "/", "cc"]
	```
	join：将数组合并成一个字符串，并在成员之间插入分隔符。
	```
	var join1 = R.join('|')([1, 2, 3]);
	// console.log(join1); // 1|2|3
	```
#####	6.3 数组的过滤（filter、reject、takeWhile、dropWhile、without）
	filter：过滤出符合条件的成员。
	```
	var isEven = n => n % 2 === 0;
	var filter1 = R.filter(isEven)([1, 2, 3]);
	// console.log(filter1); // [2]
	```
	reject：过滤出所有不满足条件的成员。
	```
	var isEven = n => n % 2 === 0;
	var reject1 = R.reject(isEven)([1, 2, 3]);
	// console.log(reject1); // [1, 3]
	```
	takeWhile：一旦满足条件，取出前面的所有成员。
	```
	var isNotFour = x => x !== 4;
	var takeWhile1 = R.takeWhile(isNotFour)([1, 2, 3, 4, 3])
	// console.log(takeWhile1); // [1, 2, 3]
	```
	dropWhile：一旦满足条件，取出剩余的所有成员。
	```
	var lteTwo = x => x <= 2;
	var dropWhile1 = R.dropWhile(lteTwo)([1, 2, 3, 4, 3, 2, 1]);
	// console.log(dropWhile1); // [3, 4, 3, 2, 1]
	```
	without：返回指定值以外的成员。
	```
	var without1 = R.without([1, 2])([1, 2, 1, 3, 4]);
	// console.log(without1); // [3, 4]
	```
#####	6.4 单数组运算（countBy、splitAt、splitEvery、splitWhen、aperture、partition、indexOf、lastIndexOf、map、mapIndexed、forEach、reduce、reduceRight、reduceWhile、sort、sortWith、adjust、ap、flatten、groupWith）
	countBy：对每个成员执行指定函数以后，返回一个对象，表示各种执行结果分别包含多少成员。	
	```
	var numbers = [1.0, 1.1, 1.2, 2.0, 3.0, 2.2];
	var countBy1 = R.countBy(Math.floor)(numbers);
	// console.log(countBy1); // {1: 3, 2: 2, 3: 1}
	```
	splitAt：在给定位置，将原数组分成两个部分。
	```
	var splitAt1 = R.splitAt(1)([1, 2, 3]);
	// console.log(splitAt1); // [[1],[2, 3]]
	```
	splitEvery：按照指定的个数，将原数组分成多个部分。
	```
	var splitEvery1 = R.splitEvery(3)([1, 2, 3, 4, 5, 6, 7]);
	// console.log(splitEvery1); // [[1, 2, 3], [4, 5, 6], [7]]
	```
	splitWhen：以第一个满足指定函数的成员为界，将数组分成两个部分。
	```
	var splitEvery1 = R.splitEvery(3)([1, 2, 3, 4, 5, 6, 7]);
	// console.log(splitEvery1); // [[1, 2, 3], [4, 5, 6], [7]]
	```
	aperture：每个成员与其后给定数量的成员分成一组，这些组构成一个新的数组。
	```
	var aperture1 = R.aperture(3)([1, 2, 3, 4, 5, 6, 7]);
	// console.log(aperture1); // [[1, 2, 3], [2, 3, 4], [3, 4, 5], [4, 5, 6], [5, 6, 7]]
	```
	partition：根据是否满足指定函数，将成员分区。
	```
	var partition1 = R.partition(R.contains('s'))(['aaa', 'bbb', 'sss']);
	// console.log(partition1); // [['sss'], ["aaa", "bbb"]]
	```
	indexOf：某个值在数组中第一次出现的位置。
	```
	var indexOf1 = R.indexOf(3)([1, 2, 3, 4]);
	// console.log(indexOf1); // 2
	```
	lastIndexOf：某个值在数组中最后一次出现的位置。
	```
	var lastIndexOf1 = R.lastIndexOf(3)([-1, 3, 3, 0, 1, 2, 3, 4]);
	// console.log(lastIndexOf1); // 6
	```
	map：数组的每个成员依次执行某个函数。
	```
	var double = x => x * 2;
	var map1 = R.map(double)([1, 2, 3]);
	// console.log(map1); // [2, 4, 6]
	```
	mapIndexed：与map类似，区别是遍历函数可以额外获得两个参数：索引位置和原数组。
	```
	var mapIndexed = R.addIndex(R.map);
	var mapIndex1 = mapIndexed((val, idx) => idx + '-' + val, ['f', 'o', 'o', 'b', 'a', 'r']);
	// console.log(mapIndex1); // ["0-f", "1-o", "2-o", "3-b", "4-a", "5-r"]
	```
	forEach：数组的每个成员依次执行某个函数，总是返回原数组。
	```
	var printXPlusFive = x => console.log(x + 5);
	var forEach1 = R.forEach(printXPlusFive, [1, 2, 3]); 
	// console.log(forEach1); // [1, 2, 3]
	```
	reduce：数组成员依次执行指定函数，每一次的运算结果都会进入一个累积变量。
	```
	var mySubtract = function (a, b) {
		return a - b;
	};
	var reduce1 = R.reduce(mySubtract, 0)([1, 2, 3, 4]);
	// console.log(reduce1); // -10
	```
	reduceRight：与reduce类似，区别是数组成员从左到右执行。
	```
	var reduceRight1 = R.reduceRight(R.subtract, 0)([1, 2, 3, 4]);
	// console.log(reduceRight1); // -2
	```
	reduceWhile：与reduce类似，区别是有一个判断函数，一旦数组成员不符合条件，就停止累积。
	```
	var isOdd = (acc, x) => x % 2 === 1;
	var ys = [2, 4, 6];
	var reduceWhile1 = R.reduceWhile(isOdd, R.add, 111)(ys);
	// console.log(reduceWhile1); // 111
	```
	sort：按照给定函数，对数组进行排序。
	```
	var diff = function (a, b) { return a -b; };
	var sort1 = R.sort(diff)([4, 2, 7, 5]);
	// console.log(sort1); // [2, 4, 5, 7]
	```
	sortWith：按照给定的一组函数，进行多重排序。
	```
	var alice = {
		name: 'alice',
		age: 40
	};
	var bob = {
		name: 'bob',
		age: 30
	};
	var clara = {
		name: 'clara',
		age: 40
	};
	var people = [clara, bob, alice];
	var ageNameSort = R.sortWith([
			// R.descend(R.prop('age')),
			R.ascend(R.prop('name'))
		]);
	var sortWith1 = ageNameSort(people);
	console.log(sortWith1); // [{name: "alice", age: 40}, {name: "bob", age: 30}, {name: "clara", age: 40}]
	```
	adjust：对指定位置的成员执行给定的函数。
	```
	var adjust1 = R.adjust(R.add(10) ,1)([1, 2, 3]);
	// console.log(adjust1); // [1, 12, 3]
	```
	ap：数组成员分别执行一组函数，将结果合成为一个新数组。
	```
	var ap1 = R.ap([R.multiply(2), R.add(3)])([1, 2, 3]);
	// console.log(ap1); // [2, 4, 6, 4, 5, 6]
	```
	flatten：将嵌套数组铺平。
	```
	var flatten1 = R.flatten([1, 2, [ 3, 4, 5, [6]]]);
	// console.log(flatten1); // [1, 2, 3, 4, 5, 6]
	```
	groupWith：将数组成员依次按照指定条件两两比较，并按照结果将所有成员放入子数组。
	```
	var groupWith1 = R.groupWith(R.equals)([0, 1, 1, 2, 3, 5, 8, 13, 21]);
	// console.log(groupWith1); // [[0], [1, 1], [2], [3], [5], [8], [13], [21]]
	```
#####	6.5 双数组运算（concat、zip、zipObj、xprod、intersection、intersectionWith、diffrence、diffrenceWith、symmetricDifference、symmetricDifferenceWith）
	concat：将两个数组合并成一个数组。
	```
	var concat1 = R.concat([1, 2])(['a', 'b']);
	console.log(concat1); // [1, 2, "a", "b"]
	```
	zip：将两个数组指定位置的成员放在一起，生成一个新数组。
	```
	var zip1 = R.zip([1, 2, 3])(['a', 'b', 'c']);
	// console.log(zip1); // [[1, "a"], [2, "b"], [3, "c"]]
	```
	zipObj：将两个数组指定位置的成员分别作为键名和键值，生成一个新对象。
	```
	var zip1 = R.zip([1, 2, 3])(['a', 'b', 'c']);
	// console.log(zip1); // [[1, "a"], [2, "b"], [3, "c"]]
	```
	xprod：将两个数组的成员两两混合，生成一个新数组。
	```
	var xprod1 = R.xprod([1, 2])(['a', 'b']);
	// console.log(xprod1); // [[1, "a"], [1, "b"], [2, "a"], [2, "b"]]
	```
	intersection：返回两个数组相同的成员组成的新数组。
	```
	var intersection1 = R.intersection([1, 2, 3, 4])([4, 3, 8]);
	// console.log(intersection1); // [4, 3]
	```
	intersectionWith：返回经过某种运算，有相同结果的两个成员。
	```
	var buffaloSpringfield = [
	  {id: 824, name: 'Richie Furay'},
	  {id: 177, name: 'Neil Young'}
	];
	var csny = [
	  {id: 204, name: 'David Crosby'},
	  {id: 177, name: 'Neil Young'}
	];
	var intersectionWith1 = R.intersectionWith(R.eqBy(R.prop('id')), buffaloSpringfield)(csny);
	// console.log(intersectionWith1); // [{id: 177, name: "Neil Young"}]
	```
	difference：返回第一个数组不包含在第二个数组里面的成员。
	```
	var difference1 = R.difference([1, 2, 3, 4])([7, 6, 5, 4, 3]);
	// console.log(difference1); // [1, 2]
	```
	differenceWith：返回执行指定函数后，第一个数组里面不符合条件的所有成员。
	```
	var cmp = (x, y) => x.a === y.a;
	var l1 = [{a: 1}, {a: 2}, {a: 3}];
	var l2 = [{a: 3}, {a: 4}];
	var differenceWith1 = R.differenceWith(cmp, l1)(l2);
	console.log(differenceWith1); // [{a: 1}, {a: 2}]
	```
	symmetricDifference：返回两个数组的非共有成员所组成的一个新数组。
	```
	var symmetricDifference1 = R.symmetricDifference([1, 2, 3, 4])([7, 6, 5, 4, 3]);
	// console.log(symmetricDifference1); // [1, 2, 7, 6, 5]
	```
	symmetricDifferenceWith：根据指定条件，返回两个数组所有运算结果不相等的成员所组成的新数组。
	```
	var eqA = R.eqBy(R.prop('a'));
	var l1 = [{a: 1}, {a: 2}, {a: 3}, {a: 4}];
	var l2 = [{a: 3}, {a: 4}, {a: 5}, {a: 6}];
	var symmetricDifferenceWith1 = R.symmetricDifferenceWith(eqA, l1, l2);
	// console.log(symmetricDifferenceWith1); // [{a: 1}, {a: 2}, {a: 5}, {a: 6}]
	```
#####	6.6 复合数组（find、dindIndex、findLast、findLastIndex、pluck、project、transpose、mergeAll、fromPairs、groupBy、sortBy）
	find：返回符合指定条件的成员。
	```
	var xs = [{a: 1}, {a: 2}, {a: 3}];
	var find1 = R.find(R.propEq('a', 2))(xs);
	// console.log(find1); // [{a: 2}]
	```
	findIndex：返回符合指定条件的成员的位置。
	```
	var xs = [{a: 1}, {a: 2}, {a: 3}];
	var findIndex1 = R.findIndex(R.propEq('a', 2))(xs);
	// console.log(findIndex1); // 1
	```
	findLast：返回最后一个符合指定条件的成员。
	```
	var xs = [{a: 1, b: 0}, {a:1, b: 1}];
	var findLast1 = R.findLast(R.propEq('a', 1))(xs);
	// console.log(findLast1); // {a: 1, b: 1}
	```
	findLastIndex：返回最后一个符合指定条件的成员的位置。
	```
	var xs = [{a: 1, b: 0}, {a:1, b: 1}];
	var findLastIndex1 = R.findLastIndex(R.propEq('a', 1))(xs);
	// console.log(findLastIndex1); // 1
	```
	pluck：取出数组成员的某个属性，组成一个新数组。
	```
	var pluck1 = R.pluck('a')([{a: 1}, {a: 2}]);
	// console.log(pluck1); // [1, 2]
	```
	project：取出数组成员的多个属性，组成一个新数组。
	```
	var abby = {name: 'Abby', age: 7, hair: 'blond', grade: 2};
	var fred = {name: 'Fred', age: 12, hair: 'brown', grade: 7};
	var kids = [abby, fred];
	var project1 = R.project(['name', 'grade'])(kids);
	// console.log(project1); // [{name: 'Abby', grade: 2}, {name: 'Fred', grade: 7}]
	```
	transpose：将每个成员相同位置的值，组成一个新数组。
	```
	var transpose1 = R.transpose([[1, 'a'], [2, 'b'], [3, 'c']]);
	// console.log(transpose1); // [[1, 2, 3], ['a', 'b', 'c']]
	```
	mergeAll：将数组的成员合并成一个对象。
	```
	var mergeAll1 = R.mergeAll([{foo: 1}, {bar: 2}, {baz: 3}]);
	// console.log(mergeAll1); // {foo:1,bar:2,baz:3}
	```
	fromPairs：将嵌套数组转为一个对象。
	```
	var fromPairs1 = R.fromPairs([['a', 1], ['b', 2], ['c', 3]])
	// console.log(fromPairs1); // {a: 1, b: 2, c: 3}
	```
	groupBy：将数组成员按照指定条件分组。
	```
	var byGrade = R.groupBy(function (student) {
		let score = student.score;
		return score < 60 ? 'F' :
				score < 70 ? 'D' :
				score < 80 ? 'C' :
				score < 90 ? 'B' : 'A';
	});
	var students = [{name: 'Abby', score: 84},
					{name: 'Eddy', score: 58},
					{name: 'Jack', score: 90}];
	var groupBy1 = byGrade(students);
	// console.log(groupBy1); 
	// {
	//   'A': [{name: 'Jack', score: 90}],
	//   'B': [{name: 'Abby', score: 84}]
	//   'F': [{name: 'Eddy', score: 58}]
	// }
	```
	sortBy：根据成员的某个属性排序。
	```
	var sortByFirstItem = R.sortBy(R.prop(0));
	var sortBy1 = sortByFirstItem([[-1, 1], [-2, 2], [-3, 3]])
	// console.log(sortBy1); // // [[-3, 3], [-2, 2], [-1, 1]]
	```
####	七、对象
#####	7.1 对象的特征判断（has、hasIn、propEq、whereEq、where）
	has: 返回一个布尔值，表示对象自身是否具有该属性。
	```
	var hasName = R.has('name');
	var has1 = hasName({name: 'fan'});
	// console.log(has1); // true
	```
	hasIn：返回一个布尔值，表示对象自身或原型链上是否具有某个属性。
	```
	function Rectangle (width, height) {
		this.width = width;
		this.height = height;
	}
	Rectangle.prototype.area = function () {
		return this.width * this.height;
	};
	var square = new Rectangle(2, 2);
	var hasIn1 = R.hasIn('width')(square); // 自身的
	var hasIn2 = R.hasIn('area')(square); // 原型的
	// console.log(hasIn1, hasIn2); // true true
	```
	propEq：如果单个属性等于给定值，返回true。
	```
	var abby = {name: 'Abby', age: 7, hair: 'blond'};
	var fred = {name: 'Fred', age: 12, hair: 'brown'};
	var rusty = {name: 'Rusty', age: 10, hair: 'brown'};
	var alois = {name: 'Alois', age: 15, disposition: 'surly'};
	var kids = [abby, fred, rusty, alois];
	var hasBrownHair = R.propEq('hair', 'brown');
	var propEq1 = R.filter(hasBrownHair)(kids) // [fred, rusty]
	// console.log(propEq1); // // [fred, rusty]
	```
	whereEq：如果多个属性等于给定值，返回true。
	```
	var pred = R.whereEq({a: 1, b: 2});
	var whereEq1 = pred({a: 1, b: 2});
	// console.log(whereEq1); // true
	```
	where：如果各个属性都符合指定条件，返回true。
	```
	var pred = R.where({
		a: R.equals('foo'),
		b: R.equals('bzr'),
		x: R.gt(10),
		y: R.lt(20)
	});
	var where1 = pred({a: 'foo', b: 'bzr', x: 12, y: 15});
	console.log(where1); // true
	```
#####	7.2 对象的过滤（omit、filter、reject）
	omit：过滤指定属性。
	```
	var omit1 = R.omit(['a', 'b'])({a: 1, b: 2, c: 3, d: 4});
	// console.log(omit1); // {b: 2, c: 3}
	```
	filter：返回所有满足条件的属性。
	```
	var isEven = n => n % 2 === 0;
	var filter1 = R.filter(isEven)({a: 1, b: 2, c: 3, d: 4});
	// console.log(filter1); // {b: 2, d: 4}
	```
	reject：返回所有不满足条件的属性。
	```
	var isOdd = n => n % 2 === 1;
	var reject1 = R.reject(isOdd)({a: 1, b: 2, c: 3, d: 4});
	// console.log(reject1); // {b: 2, d: 4}
	```
	reject：返回所有不满足条件的属性。
#####	7.3 对象的截取（dissoc、assoc、partition、pick、pickAll、pickBy、keys、keysIn、values、valuesIn、invertObj、invert）
	dissoc：过滤指定属性。
	```
	var dissoc1 = R.dissoc('b')({a: 1, b: 2, c: 3});
	// console.log(dissoc1); // {a: 1, c: 3}
	```
	assoc：添加或改写某个属性。
	```
	var assoc1 = R.assoc('c', 3)({a: 1, b: 2});
	// console.log(assoc1); // {a: 1, b: 2, c: 3}
	```
	partition：根据属性值是否满足给定条件，将属性分区。
	```
	var partition1 = R.partition(R.contains('s'))({a: 'ssss', b: 'ttt', foo: 'bars'});
	// console.log(partition1); // [ { a: 'sss', foo: 'bars' }, { b: 'ttt' }  ]
	```
	pick：返回指定属性组成的新对象。
	```
	var pick1 = R.pick(['a', 'd'])({a: 1, b: 2, c: 3, d: 4});
	// console.log(pick1); // {a: 1, d: 4}
	```
	pickAll：与pick类似，但会包括不存在的属性。
	```
	var pickAll1 = R.pickAll(['a', 'd', 'f'])({a: 1, b: 2, c: 3, d: 4});
	// console.log(pickAll1); // {a: 1, d: 4, f: undefined}
	```
	pickBy：返回符合条件的属性。
	```
	var isUpperCase = (val, key) => key.toUpperCase() === key;
	var pickBy1 = R.pickBy(isUpperCase)({a: 1, b: 2, A: 3, B: 4});
	// console.log(pickBy1); // {A: 3, B: 4}
	```
	keys：返回对象自身属性的属性名组成的新数组。
	```
	var keys1 = R.keys({a: 1, b: 2});
	// console.log(keys1); // ['a', 'b']
	```
	keysIn：返回对象自身的和继承的属性的属性名组成的新数组。
	```
	var F = function () {this.x = 'X'};
	F.prototype.y = 'Y';
	var f = new F();
	var keysIn1 = R.keysIn(f);
	// console.log(keysIn1); // ['x', 'y']
	```
	values：返回对象自身的属性的属性值组成的数组。
	```
	var values1 = R.values({a: 1, b: 2, c: 3});
	// console.log(values1); // [1, 2, 3]
	```
	valuesIn：返回对象自身的和继承的属性的属性值组成的数组。
	```
	var F = function() { this.x = 'X'; };
	F.prototype.y = 'Y';
	var f = new F();
	var valuesIn1 = R.valuesIn(f);
	// console.log(valuesIn1); // ["X", "Y"]
	```
	invertObj：将属性值和属性名互换。如果多个属性的属性值相同，只返回最后一个属性。
	```
	var raceResultsByFirstName = {
	  first: 'alice',
	  second: 'jake',
	  third: 'alice',
	};
	var invertObj1 = R.invertObj(raceResultsByFirstName);
	// console.log(invertObj1); // {alice: "third", jake: "second"}
	```
	invert：将属性值和属性名互换，每个属性值对应一个数组。
	```
	var raceResultsByFirstName = {
	  first: 'alice',
	  second: 'jake',
	  third: 'alice',
	};
	var invert1 = R.invert(raceResultsByFirstName);
	console.log(invert1); // // { 'alice': ['first', 'third'], 'jake':['second'] }
	```
#####	7.4 对象的运算（prop、map、mapObjIndexed、forEachObjIndexed、merge、mergeWith、eqProps、evolve）
	prop：返回对象的指定属性。
	```
	var prop1 = R.prop('x')({x: 100});
	// console.log(prop1); // 100
	```
	map：对象的所有属性依次执行某个函数。
	```
	var double = x => x * 2;
	var map1 = R.map(double)({x: 1, y: 2, z: 3});
	// console.log(map1); // {x: 2, y: 4, z: 6}
	```
	mapObjIndexed：与map类似，但是会额外传入属性名和整个对象。
	```
	var values = {x: 1, y: 2, z: 3};
	var prependkeyAndDouble = (num, key, obj) => key + (num * 2);
	var mapObjIndexed1 = R.mapObjIndexed(prependkeyAndDouble)(values);
	// console.log(mapObjIndexed1); // {x: "x2", y: "y4", z: "z6"}
	```
	forEachObjIndexed：每个属性依次执行给定函数，给定函数的参数分别是属性值和属性名，返回原对象。
	```
	var printkeyConcatValue = (value, key) => console.log(key + ':' + value);
	var forEachObjIndexed1 = R.forEachObjIndexed(printkeyConcatValue)({x: 1,y: 2});
	// console.log(forEachObjIndexed1); // {x: 1,y: 2}
	```
	merge：合并两个对象，如果有同名属性，后面的值会覆盖掉前面的值。
	```
	var merge1 = R.merge({'name': 'fred', 'age': 10})({'age': 40});
	// console.log(merge1); // {name: "fred", age: 40}
	```
	mergeWith：合并两个对象，如果有同名属性，会使用指定的函数处理。
	```
	var mergeWith1 = R.mergeWith(
			R.concat,
			{a: true, values: [10, 20]},
			{b: true, values: [15, 35]}
		);
	// console.log(mergeWith1); // { a: true, b: true, values: [10, 20, 15, 35] }
	```
	eqProps：比较两个对象的指定属性是否相等。
	```
	var o1 = {a: 1, b: 2, c: 3, d: 4};
	var o2 = {a: 10, b: 20, c: 3, d: 40};
	var eqProps1 = R.eqProps('c', o1)(o2);
	// console.log(eqProps1); //true
	```
	evolve：对象的属性分别经过一组函数的处理，返回一个新对象。
	```
	var tomato = {
		firstName: 'Tomato',
		data: {elapsed: 100, remaining: 1400},
		id: 123
	};
	var transformations = {
		firstName: R.trim,
		lastName: R.trim,
		data: {elapsed: R.add(1), remaining: R.add(-1)}
	};
	var evolve1 = R.evolve(transformations)(tomato);
	// console.log(evolve1);
	// {
	//   firstName: 'Tomato',
	//   data: {elapsed: 101, remaining: 1399},
	//   id: 123
	// }
	```
#####	7.5 复合对象（path、pathEq、assocPath）
	path：取出数组中指定路径的值。
	```
	var path1 = R.path(['a', 'b'], {a: {b: 2}});
	// console.log(path1); // 2
	```
	pathEq：返回指定路径的值符合条件的成员。
	```
	var user1 = {address: {zipCode: 11111}};
	var user2 = {address: {zipCode: 22222}};
	var user3 = {address: {zipCode: 33333}};
	var users = [user1, user2, user3];
	var isFamous = R.pathEq(['address', 'zipCode'], 11111);
	var pathEq1 = R.filter(isFamous)(users);
	// console.log(pathEq1); // [user1]
	```
	assocPath：添加或改写指定路径的属性的值。
	```
	var assocPath1 = R.assocPath(['a', 'b', 'c'], 42)({a: {b: {c: 0}}});
	// console.log(assocPath1); // {a: {b: {c: 42}}}
	```
>	参考文档：
[阮老师--Ramda 函数库参考教程](http://www.ruanyifeng.com/blog/2017/03/ramda.html)	
[Ramda 中文](http://ramda.cn/)		
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	












