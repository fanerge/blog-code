---
title: ramda运用--官方文档解读
date: 2017-08-25 20:37:40
category: "函数式编程"
tags: ['js','函数式编程','ramda']
---
从8月25到8月29共5个晚上和小部分周末时间撸完了Ramda官方文档，继续在事件中继续学习。
Rmda中共分为List、Relation、Object、Function、Math、Type、Logic、String共8大类。

####	String

#####	test
	定义：检测字符串是否匹配给定的正则表达式。
	```   
	R.test(/^x/, 'xyz'); // true
	```   

#####	match
	定义：正则匹配字符串。注意，如果没有匹配项，则返回空数组。和 String.prototype.match 不同，后者在没有匹配项时会返回 null。
	```   
	R.match(/([a-z]a)/g, 'bananas'); //=> ['ba', 'na', 'na']
	```   

#####	replace
	定义：替换字符串的子串或正则匹配到的值。
	```   
	R.replace('foo', 'bar', 'foo foo foo'); // bar foo foo
	```   

#####	split
	定义：根据指定的分隔符将字符串拆分为字符串类型的数组。
	```   
	R.split('.', 'a.b.c.xyz.d'); // ['a', 'b', 'c', 'xyz', 'd']
	```   


#####	toLower
	定义：将字符串转换成小写。
	```   
	R.toLower('XYZ'); // xyz
	```   

#####	toUpper
	定义：将字符串转换为大写。
	```   
	R.toUpper('abc'); // ABC
	```   
	
#####	toString
	定义：返回代表输入元素的字符串。求得的输出结果应该等价于输入的值。许多内建的 toString 方法都不满足这一条件。
	如果输入值是 [object Object] 对象，且自身含有 toString 方法（不是 Object.prototype.toString 方法），那么直接调用这个方法求返回值。
	这意味着，通过用户自定义的构造函数可以提供合适的 toString 方法。
	```   
	R.toString(42); // 42
	```   
	
#####	trim
	定义：删除字符串首、尾两端的空白字符。
	```   
	R.trim('   xyz  '); //=> 'xyz'
	```   

####	List(Array)	

#####	adjust
	定义：将数组中指定索引处的值替换为经函数变换的值。
	```   
	R.adjust(R.add(10))(1)([1, 2, 3]); // [1, 12, 3]
	```   
#####	all
	定义：如果列表中的所有元素都满足 predicate，则返回 true；否则，返回 false。
	```   
	var equals3 = R.equals(3);
	R.all(equals3)([3, 3, 3]); // true
	R.all(equals3)([1, 3, 3]); // false
	```   
#####	any
	定义：只要列表中有一个元素满足 predicate，就返回 true，否则返回 false。
	```   
	var lessThan0 = R.flip(R.lt)(0);
	var lessThan2 = R.flip(R.lt)(2);
	R.any(lessThan0)([1, 2]); // false
	R.any(lessThan2)([1, 2]); // true
	```   
	
#####	aperture
	定义：返回一个新列表，列表中的元素为由原列表相邻元素组成的 n 元组。如果 n 大于列表的长度，则返回空列表。
	```   
	R.aperture(2, [1, 2, 3, 4, 5]); // [[1, 2], [2, 3], [3, 4], [4, 5]]
	```   
	
#####	append
	定义：在列表末尾拼接一个元素。
	```   
	R.append(5)([1, 2, 3]); // [1, 2, 3, 5]
	```   
	
#####	chain
	定义：chain 将函数映射到列表中每个元素，并将结果连接起来。 chain 在一些库中也称为 flatMap（先 map 再 flatten ）。
	```   
	var duplicate = n => [n, n];
	R.chain(duplicate, [1, 2, 3]); // [1, 1, 2, 2, 3, 3]
	```    	
	
#####	concat
	定义：连接列表或字符串。
	```   
	R.concat('ABC', 'DEF'); // ABCDEF
	R.concat([1, 2, 3])([4, 5, 6]); // [1, 2, 3, 4, 5, 6]
	```    		
	
#####	contains
	定义：只要列表中有一个元素等于指定值，则返回 true；否则返回 false。通过 R.equals 函数进行相等性判断。
	```   
	R.contains(2, [1, 2, 3]); // true
	```   	
	
#####	drop
	定义：删除给定 list，string 或者 transducer/transformer（或者具有 drop 方法的对象）的前 n 个元素。
	```   
	R.drop(2, ['fan', 'yu', 'zhen']); // ['zhen']
	R.drop(2, 'fanerge'); // nerge
	```   	
	
#####	dropLast
	定义：删除 "list" 末尾的 n 个元素。
	```   
	R.dropLast(2, ['fan', 'yu', 'zhen']); // ['fan']
	R.dropLast(2, 'fanerge'); // faner
	```   	
	
#####	dropLastWhile
	定义：对 list 从后向前一直删除满足 predicate 的尾部元素，直到遇到第一个 falsy 值，此时停止删除操作。
	predicate 需要作为第一个参数传入。
	```   
	var lteThree = x => x <= 3;
	R.dropLastWhile(lteThree, [1, 2, 3, 4, 3, 2, 1]); // [1, 2, 3, 4]
	```   	
	
#####	dropRepeats
	定义：返回一个没有连续重复元素的 list。通过 R.equals 函数进行相等性判断。
	```   
	R.dropRepeats([1, 1, 2, 2, 3, 3, 2, 2]); // [1, 2, 3, 2] 
	```   	
	
#####	dropRepeatsWith
	定义：返回一个没有连续重复元素的 list。首个参数提供的 predicate 用于检测 list 中相邻的两个元素是否相等。
	一系列相等元素中的首个元素会被保留。
	```   
	var list = [1, -1, 1, 3, 4, -4];
	R.dropRepeatsWith(R.eqBy(Math.abs), list); // [1, 3, 4]
	```   	

#####	dropWhile
	定义：对 list 从前向后删除满足 predicate 的头部元素，直到遇到第一个 falsy 值。
	predicate 需要作为第一个参数传入。
	```   
	var lteTwo = x => x <= 2;
	R.dropWile(lteTwo, [1, 2, 3, 4, 3, 2, 1]); // [3, 4, 3, 2, 1]
	```   	

#####	endsWith
	定义：检查列表或字符串是否以给定的值结尾。
	```   
	R.endsWith('c', 'abc'); // true
	R.endsWith(['c'], ['a', 'b', 'c']); // true
	```   	

#####	filter
	定义：使用 predicate 遍历传入的 Filterable，返回满足 predicate 的所有元素的新的 Filterable。新 Filterable 与原先的类型相同。Filterable 类型包括 plain object 或者任何带有 filter 方法的类型，如 Array 。
	```   
	var isEven = n => n % 2 === 0;
	R.filter(isEven, [1, 2, 3, 4]); // [2, 4]
	```   	

#####	find
	定义：查找并返回 list 中首个满足 predicate 的元素；如果未找到满足条件的元素，则返回 undefined 。
	```   
	var xs = [{a: 1}, {a: 2}, {a: 3}];
	R.find(R.propEq('a', 2))(xs); // {a: 2}
	```   
	
#####	findIndex
	定义：查找并返回 list 中首个满足 predicate 的元素的索引；如果未找到满足条件的元素，则返回 -1 。
	```   
	var xs = [{a: 1}, {a: 2}, {a: 3}];
	R.findIndex(R.propEq('a', 2))(xs); // 1
	```   
	
#####	findLast
	定义：查找并返回 list 中最后一个满足 predicate 的元素；如果未找到满足条件的元素，则返回 undefined 。
	```   
	var xs = [{a: 1, b: 0}, {a:1, b: 1}];
	R.findLast(R.propEq('a', 1))(xs); // {a: 1, b: 1}
	```   
	
#####	findLastIndex
	定义：查找并返回 list 中最后一个满足 predicate 的元素的索引；如果未找到满足条件的元素，则返回 -1 。
	```   
	var xs = [{a: 1, b: 0}, {a:1, b: 1}];
	R.findLastIndex(R.propEq('a', 1))(xs); // 1
	```   

#####	flatten
	定义：获取 list 的所有元素（包含所有子数组中的元素），然后由这些元素组成一个新的数组。深度优先。
	```   
	R.flatten([1, [2, [3]]]); // [1, 2, 3]
	```   

#####	forEach
	定义：遍历 list，对 list 中的每个元素执行方法 fn。
	Ramda 的 forEach 会将原数组返回。
	```   
	var printPlusFive = x => console.log(x + 5);
	R.forEach(printPlusFive, [1, 2, 3]); // [1, 2, 3]
	```   

#####	fromPairs
	定义：由一系列 “键值对” 创建一个 object。如果某个键出现多次，选取最右侧的键值对。
	```   
	var R.fromPairs([['a', 1], ['b', 2], ['c', 3]]); // {a: 1, b: 2, c: 3}
	```   

#####	groupBy
	定义：将列表根据一定规则拆分成多组子列表，并存储在一个对象中。
	对列表中的每个元素调用函数，根据函数返回结果进行分组。
	函数返回字符串作为相等性判断，返回的字符串作为存储对象的键，具有相同返回字符串的元素聚合为数组，作为该键的值。
	```   
	var byGrade = R.groupBy(function(student) {
	  var score = student.score;
	  return score < 65 ? 'F' :
			 score < 70 ? 'D' :
			 score < 80 ? 'C' :
			 score < 90 ? 'B' : 'A';
	});
	var students = [{name: 'Abby', score: 84},
					{name: 'Eddy', score: 58},
					// ...
					{name: 'Jack', score: 69}];
	byGrade(students);
	// {
	//   'A': [{name: 'Dianne', score: 99}],
	//   'B': [{name: 'Abby', score: 84}]
	//   // ...,
	//   'F': [{name: 'Eddy', score: 58}]
	// }
	```   

#####	groupWith
	定义：通过给定的对比函数，将列表按顺序分割成多组子列表。
	对比函数只比较相邻元素。
	```   
	var R.groupWith(R.equals, [0, 1, 1, 2, 3, 5, 13, 21]); 
	// [[0], [1, 1], [2], [3], [5], [13], [21]]
	```   

#####	head
	定义：求列表或字符串的首个元素。在某些库中，该函数也被称作 first。
	```   
	R.head([1, 2, 3]); // 1
	R.head('fan'); // f
	```   

#####	indexBy
	定义：通过生成键的函数，将元素为对象的 list 转换为以生成的键为索引的新对象。注意，如果 list 中多个对象元素生成相同的键，以最后一个对象元素作为该键的值。
	```   
	var list = [{id: 'xyz', title: 'A'}, {id: 'abc', title: 'B'}];
	R.indexBy(R.prop('id'), list);
	//=> {abc: {id: 'abc', title: 'B'}, xyz: {id: 'xyz', title: 'A'}}
	```   
	
#####	indexOf
	定义：返回给定元素在数组中首次出现时的索引值，如果数组中没有该元素，则返回 -1。通过 R.equals 函数进行相等性判断。
	```   
	R.indexOf(3, [1, 2, 3, 4]); // 2
	```   	
	
#####	init
	定义：返回 list 或 string 删除最后一个元素后的部分。
	```   
	R.init([1, 2, 3]);  //=> [1, 2]
	R.init('abc');  //=> 'ab'
	```   	

#####	insert
	定义：将元素插入到 list 指定索引处。注意，该函数是破坏性的：返回处理后列表的拷贝。函数运行过程中不会破坏任何列表。
	```   
	R.insert(2, 'x', [1, 2, 3, 4]); // [1,2,'x',3,4]
	```   	

#####	insertAll
	定义：将子 list 插入到 list 指定索引处。注意，该函数是破坏性的：返回处理后列表的拷贝。函数运行过程中不会破坏任何列表。
	```   
	R.insertAll(2, ['x', 'y'], [1, 2, 3, 4]); // [1, 2, 'x', 'y', 3, 4]
	```   	
	
#####	intersperse
	定义：在列表的元素之间插入分割元素。
	```   
	R.intersperse('n', ['ba', 'a', 'a']); //=> ['ba', 'n', 'a', 'n', 'a']
	```   	
	
#####	into
	定义：使用 transducer 对 list 中的元素进行转换，然后使用基于 accumulator 的类型的迭代器函数将转换后的元素依次添加到 accumulator 上。
	```   
	var numbers = [1, 2, 3, 4];
	var transducer = R.compose(R.map(R.add(1)), R.take(2));
	R.into([], transducer, numbers); //=> [2, 3]
	```   	
	
#####	join
	定义：将列表中所有元素通过 分隔符 串连为一个字符串。
	```   
	var spacer = R.join('#');
	spacer([1, 2, 3]); // 1#2#3
	```   	
	
#####	last
	定义：返回列表或字符串的最后一个元素。
	```   
	R.last(['fan', 'zhen', 'yu']); // yu
	R.last('abc'); // c
	```   	
	
#####	lastIndexOf
	定义：返回数组中某元素最后一次出现的位置，如果数组中不包含该项则返回 -1 。通过 R.equals 函数进行相等性判断。
	```   
	R.lastIndexOf(3, [-1,3,3,0,1,2,3,4]); // 6
	```   
	
#####	length
	定义：通过 list.length，返回数组的大小（数组中元素的数量）。
	```   
	R.length([]); // 0
	```   
	
#####	map
	定义：接收一个函数和一个 functor, 将该函数应用到 functor 的每个值上，返回一个具有相同形态的 functor。
	Ramda 为 Array 和 Object 提供了合适的 map 实现，因此 R.map 适用于 [1, 2, 3] 或 {x: 1, y: 2, z: 3}。
	若第二个参数自身存在 map 方法，则调用自身的 map 方法。
	```   
	var double = x => x * 2;
	R.map(double, [1, 2, 3]); // [2, 4, 6]
	```   

#####	mapAccum
	定义：mapAccum 的行为类似于 map 和 reduce 的组合；
	它将迭代函数作用于列表中的每个元素，从左往右传递经迭代函数计算的累积值，并将最后的累积值和由所有中间的累积值组成的列表一起返回。 
	迭代函数接收两个参数，acc 和 value， 返回一个元组 [acc, value]。
	```   
	var digits = ['1', '2', '3', '4'];
	var appender = (a, b) => [a + b, a + b];
	R.mapAccum(appender, 0, digits); // ['01234', ['01', '012', '0123', '01234']]
	```   
	
#####	mapAccumRight
	定义：mapAccumRight 的行为类似于 map 和 reduce 的组合；
	它将迭代函数作用于列表中的每个元素，从右往左传递经迭代函数计算的累积值，并将最后的累积值和由所有中间的累积值组成的列表一起返回。
	和 mapAccum 类似，除了列表遍历顺序是从右往左的。
	迭代函数接收两个参数，value 和 acc ，返回一个元组 [value, acc]。
	```   
	var digits = ['1', '2', '3', '4'];
	var append = (a, b) => [a + b, a + b];
	R.mapAccumRight(append, 5, digits); // [['12345', '2345', '345', '45'], '12345']
	```   
	
#####	mapObjIndexed
	定义：Object 版本的 map。mapping function 接受三个参数： (value, key, obj) 。如果仅用到参数 value，则用 map 即可。
	```   
	var values = {x: 1, y: 2, z: 3};
	var prependKeyAndDouble = (val, key, obj) => key + (val * 2);
	R.mapIndexed(prependKeyAndDouble, values); // { x: 'x2', y: 'y4', z: 'z6' }
	```   
	
#####	mergaAll
	定义：将对象类型列表合并为一个对象。
	```   
	R.mergeAll([{foo: 1}, {bar: 2}, {baz: 3}]); // {foo: 1, bar: 2, baz: 3}
	```   
	
#####	none
	定义：如果列表中的元素都不满足 predicate，返回 true；否则返回 false。
	```   
	var isEven = n => n % 2 === 0;
	R.none(isEven, [1, 2, 3]); // true
	```   
	
#####	nth
	定义：返回列表或字符串的第 n 个元素。如果 n 为负数，则返回索引为 length + n 的元素。
	```   
	var list = ['foo', 'bar', 'baz', 'quux'];
	R.nth(1, list); // 'bar'
	```   
	
#####	pair
	定义：接收两个参数，fst 和 snd，返回数组 [fst, snd]。
	```   
	R.pair('foo', 'bar'); // ['foo', 'bar']
	```   
	
#####	partition
	定义：接收两个参数，fst 和 snd，返回数组 [fst, snd]。
	```   
	R.partition(R.contains('s'), ['sss', 'ttt', 'foo', 'bars']);
	// [ [ 'sss', 'bars' ],  [ 'ttt', 'foo' ] ]
	```   
	
#####	pluck
	定义：从列表内的每个对象元素中取出特定名称的属性，组成一个新的列表。
	pluck 可以作用于任何 functor ，包括 Array，因为它等价于 R.map(R.prop(k), f)。
	```   
	R.pluck('a')([{a: 1}, {a: 2}]); // [1, 2]
	```   
	
#####	prepend
	定义：在列表头部之前拼接一个元素。
	```   
	R.prepend('fee', ['fi', 'fo', 'fum']); // ['fee', 'fi', 'fo', 'fum']
	```   
	
#####	range
	定义：返回从 from 到 to 之间的所有数的升序列表。左闭右开（包含 from，不包含 to）。
	```   
	R.range(1, 5); // [1, 2, 3, 4]
	```   
	
#####	reduce
	定义：左折叠操作。
	遍历列表，相继调用二元迭代函数（参数为累积值和从数组中取出的当前元素），将本次迭代结果作为下次迭代的累积值。返回最终累积值。
	可以用 R.reduced 提前终止遍历操作。
	```   
	R.reduce(R.subtract, 0, [1, 2, 3, 4]) // => ((((0 - 1) - 2) - 3) - 4) = -10
	```   
	
#####	reduceBy
	定义：首先对列表中的每个元素调用函数 keyFn ，根据 keyFn 返回的字符串对列表元素进行分组。
	然后调用 reducer 函数 valueFn，对组内的元素进行折叠操作。
	该函数相当于更通用的 groupBy 函数。
	```   
	var reduceToNamesBy = R.reduceBy((acc, student) => acc.concat(student.name), []);
	var namesByGrade = reduceToNamesBy(function(student) {
	  var score = student.score;
	  return score < 65 ? 'F' :
			 score < 70 ? 'D' :
			 score < 80 ? 'C' :
			 score < 90 ? 'B' : 'A';
	});
	var students = [{name: 'Lucy', score: 92},
					{name: 'Drew', score: 85},
					{name: 'Bart', score: 62}];
	namesByGrade(students);
	// {
	//   'A': ['Lucy'],
	//   'B': ['Drew']
	//   'F': ['Bart']
	// }
	```   
	
#####	reduced
	定义：返回一个封装的值，该值代表 reduce 或 transduce 操作的最终结果。
	返回值是一个黑盒：不保证其内部结构的稳定性。
	```   
	R.reduce(
		(acc, item) => item > 3 ? R.reduced(acc) : acc.concat(item),
		[],
		[1, 2, 3, 4, 5]
	); // [1, 2, 3]
	```   
	
#####	reduceRight
	定义：右折叠操作。
	遍历列表，相继调用二元迭代函数（参数为累积值和从数组中取出的当前元素），将本次迭代结果作为下次迭代的累积值。返回最终累积值。
	类似于 reduce，除了遍历列表的顺序是从右向左的。
	```   
	R.reduceRight(R.subtract, 0, [1, 2, 3, 4]); // (1 - (2 - (3 - (4 - 0)))) = -2
	```   
	
#####	reduceWhile
	定义：与 reduce 类似， reduceWhile 会遍历列表，相继调用二元迭代函数，并返回最终累积值。
	reduceWhile 在每次调用迭代函数前，先使用 predicate 进行判断，如果 predicate 返回 false ，则提前终止遍历操作，并返回当前累积值。
	```   
	var isOdd = (acc, x) => x % 2 === 1;
	var xs = [1, 3, 5, 60, 777, 800];
	R.reduceWhile(isOdd, R.add, 0, xs); // 9
	```   
	
#####	reject
	定义：filter 的补操作。返回结果为 R.filter 操作结果的补集。
	```   
	var isOdd = (acc, x) => x % 2 === 1;
	R.reject(isOdd, [1, 2, 3, 4]); // [2, 4]
	```   
	
#####	remove
	定义：删除列表中从 start 开始的 count 个元素。_ 注意，该操作是非破坏性的：不改变原列表，返回处理后列表的拷贝。
	```   
	R.remove(2, 3, [1, 2, 3, 4, 5, 6, 7, 8]); // [1, 2, 6, 7, 8]
	```   
	
#####	repeat
	定义：生成包含 n 个同一元素的数组。
	```   
	R.repeat('h', 5); // ['h', 'h', 'h', 'h', 'h']
	```   
	
#####	reverse
	定义：对列表或字符串的排列顺序取反。
	```   
	R.reverse([1, 2, 3]); // [3, 2, 1]
	```   
	
#####	scan
	定义：Scan 与 reduce 类似，但会将每次迭代计算的累积值记录下来，组成一个列表返回。
	```   
	var numbers = [1, 2, 3, 4];
	var factorials = R.scan(R.multiply, 1, numbers); // [1, 1, 2, 6, 24]
	```   
	
#####	sequence
	定义：将一个 Applicative 的 Traversable 转换成一个 Traversable 类型的 Applicative。
	```   
	R.sequence(Maybe.of, [Just(1), Just(2), Just(3)]);   //=> Just([1, 2, 3])
	```   
	
#####	slice
	定义：取出给定的列表或字符串（或带有 slice 方法的对象）中，从 fromIndex（包括）到 toIndex（不包括）的元素。
	```   
	R.slice(1, 3, ['a', 'b', 'c', 'd']); // ['b', 'c']
	```   
	
#####	sort
	定义：使用比较函数对列表进行排序。比较函数每次接受两个参数，如果第一个值较小，则返回负数；如果第一个值较大，则返回正数；如果两值相等，返回零。注意，返回的是列表的 拷贝 ，不会修改原列表。
	```   
	var diff = function (a, b) { return a - b };
	R.sort(diff, [4, 2, 7, 5]); // [2, 4, 5, 7]
	```   
	
#####	splitAt
	定义：在指定的索引处拆分列表或者字符串。
	```   
	R.splitAt(1, [1, 2, 3]); // [[1], [2, 3]]
	R.splitAt(5, 'hello world'); // ['hello', 'world']
	```   
	
#####	splitEvery
	定义：将列表拆分成指定长度的子列表集。
	```   
	R.splitEvery(3, [1, 2, 3, 4, 5, 6, 7]); // [[1, 2, 3], [4, 5, 6], 7]
	R.splitEvery(3, 'foobarbaz'); //=> ['foo', 'bar', 'baz']
	```   
	
#####	splitWhen
	定义：查找列表中首个满足 predicate 的元素，在该处将列表拆分为两部分。首个满足 predicate 的元素包含在后一部分。
	```   
	R.splitWhen(R.equals(2), [1, 2, 3, 1, 2, 3]);   //=> [[1], [2, 3, 1, 2, 3]]
	```   
	
#####	startsWith
	定义：检查列表是否以给定的值开头。
	```   
	R.startsWith('a', 'abc'); // true
	R.startsWith(['a'], ['a', 'b', 'c']); // true
	```   
	
#####	tail
	定义：删除列表中的首个元素（或者调用对象的 tail 方法）。
	```   
	R.tail([1, 2, 3]); // [2, 3]
	```   
	
#####	take
	定义：返回列表的前 n 个元素、字符串的前n个字符或者用作 transducer/transform（或者调用对象的 take 方法）。
	```   
	R.take(1, ['foo', 'bar', 'baz']); //=> ['foo']
	```   
	
#####	takeLast
	定义：返回列表的后 n 个元素。如果 n > list.length，则返回 list.length 个元素。
	```   
	R.takeLast(1, ['foo', 'bar', 'baz']); //=> ['baz']
	```   
	
#####	takeLastWhile
	定义：从后往前取出列表元素，直到遇到首个不满足 predicate 的元素为止。取出的元素中不包含首个不满足 predicate 的元素。
	```   
	var isNotOne = x => x !== 1;
	R.takeLastWhile(isNotOne, [1, 2, 3, 4]); // [2, 3, 4]
	```   
	
#####	takeWhile
	定义：从前往后取出列表元素，直到遇到首个不满足 predicate 的元素为止。取出的元素中不包含首个不满足 predicate 的元素。
	```   
	var isNotFour = x => x !== 4;
	R.takeLastWhile(isNotFour, [1, 2, 3, 4, 3, 2, 1]); // [1, 2, 3]
	```   
	
#####	times
	定义：执行输入的函数 n 次，返回由函数执行结果组成的数组。
	fn 为一元函数，n 次调用接收的参数为：从 0 递增到 n-1 。
	```   
	R.times(R.identity, 5); // [0, 1, 2, 3, 4]
	```   
	
#####	transduce
	定义：用 iterator function 初始化 transducer ，生成一个 transformed iterator function。
	然后顺次遍历列表，对每个列表元素先进行转换，然后与累积值进行归约，返回值作为下一轮迭代的累积值。最终返回与初始累积值类型相同的一个累积值。
	```   
	var numbers = [1, 2, 3, 4];
	var transducer = R.compose(R.map(R.add(1)), R.take(2));
	R.transduce(transducer, R.flip(R.append), [], numbers); //=> [2, 3]
	```   
	
#####	transpose
	定义：二维数组行列转置。输入 n 个长度为 x 的数组，输出 x 个长度为 n 的数组。
	```   
	R.transpose([[1, 'a'], [2, 'b'], [3, 'c']]); // [[1, 2, 3], ['a', 'b', 'c']]
	```   	
	
#####	traverse
	定义：将返回值为 Applicative 类型的函数映射到一个 Traversable 上。
	然后使用 sequence 将结果由 Traversable of Applicative 转换为 Applicative of Traversable。
	```   
	var safeDiv = n => d => d === 0 ? Nothing() : Just(n / d);
	R.traverse(Maybe.of, safeDiv(10), [2, 4, 5]); //=> Just([5, 2.5, 2])
	```   	
	
#####	unfold
	定义：通过一个种子值（ seed ）创建一个列表。unfold 接受一个迭代函数：
	该函数或者返回 false 停止迭代，或者返回一个长度为 2 的数组：数组首个元素添加到结果列表，第二个元素作为种子值传给下一轮迭代使用。
	```   
	var f = n => n > 50 ? false : [-n, n + 10];
	R.unfold(f, 10); //=> [-10, -20, -30, -40, -50]
	```   	
	
#####	uniq
	定义：列表去重操作。返回无重复元素的列表。通过 R.equals 函数进行相等性判断。
	```   
	R.uniq([1, 1, 2, 1]); // [1, 2]
	```   
	
#####	uniqBy
	定义：返回无重复元素的列表。元素通过给定的函数的返回值以及 R.equals 进行相同性判断。如果给定的函数返回值相同，保留第一个元素。
	```   
	R.uniqBy(Math.abs, [1, -1, 2, 1]); // [1, 2]
	```   
	
#####	uniqWith
	定义：返回无重复元素的列表。元素通过 predicate 进行相同性判断。如果通过 predicate 判断两元素相同，保留第一个元素。
	```   
	var strEq = R.eqBy(String);
	R.uniqWith(strEq)([1, '1', 2, 1]); //=> [1, 2]
	```   
	
#####	unnest
	定义：R.chain(R.identity) 的简写, 对 Chain 类型的数据消除一层嵌套。
	```   
	R.unnest([1, [2], [[3]]]); //=> [1, 2, [3]]
	```   

#####	update
	定义：替换数组中指定索引处的值。
	```   
	R.update(1, 11, [0, 1, 2]); // [0, 11, 2]
	```   

#####	without
	定义：求第二个列表中，未包含在第一个列表中的任一元素的集合。通过 R.equals 函数进行相等性判断。
	```   
	R.without([1, 2], [1, 2, 1, 3, 4]); // [3, 4]
	```   

#####	xprod
	定义：将两个列表的元素两两组合，生成一个新的元素对列表。
	```   
	R.xprod([1, 2], ['a', 'b']); // [[1, 'a'], [1, 'b'], [2, 'a'], [2, 'b']]
	```   

#####	zip
	定义：将两个列表对应位置的元素组合，生成一个新的元素对列表。生成的列表长度取决于较短的输入列表的长度。
	注意，zip 等价于 zipWith(function(a, b) { return [a, b] }) 。
	```   
	R.zip([1, 2, 3], ['a', 'b', 'c']); // [[1, 'a'], [2, 'b'], [3, 'c']]
	```   

#####	zipObj
	定义：将两个列表对应位置的元素作为键值对组合，生成一个新的键值对的列表。生成的列表长度取决于较短的输入列表的长度。
	注意，zip 等价于 pipe(zipWith(pair), fromPairs) 。
	```   
	R.zipObj(['a', 'b', 'c'], [1, 2, 3]); // {a: 1, b: 2, c: 3}
	```   

#####	zipWith
	定义：将两个列表对应位置的元素通过一个函数处理，生成一个新的元素的列表。生成的列表长度取决于较短的输入列表的长度。
	```   
	var f = (x, y) => {
	  // ...
	};
	R.zipWith(f, [1, 2, 3], ['a', 'b', 'c']);
	//=> [f(1, 'a'), f(2, 'b'), f(3, 'c')]
	```   

####	Object	
	
#####	assoc
	定义：浅复制对象，然后设置或覆盖对象的指定属性。
	```   
	R.assoc('c', 3, {a: 1, b: 2}); {a: 1, b: 2, c: 3}
	```   	
	
#####	assocPath
	定义：浅复制对象，设置或覆盖即将创建的给定路径所需的节点，并将特定值放在该路径的末端。
	```   
	R.assocPath(['a', 'b', 'c'], 42, {a: {b: {c: 0}}}); // {a: {b: {c: 42}}}
	```   	
	
#####	clone
	定义：深复制。其值可能（嵌套）包含 Array、Object、Number、String、Boolean、Date 类型的数据。Function 通过引用复制。
	```   
	var objects = [{}, {}, {}];
	var objectsClone = R.clone(objects);
	objects === objectsClone; // true
	```   		
	
#####	dissoc
	定义：删除对象中指定 prop 属性。
	```   
	R.dissoc('b', {a: 1, b: 2, c: 3}); // {a: 1, c: 3}
	```   		
	
#####	dissocPath
	定义：浅复制对象，删除返回对象中指定路径上的属性。
	```   
	R.dissocPath(['a', 'b', 'c'], {a: {b: {c: 42}}}); // {a: {b: {}}}
	```   	
	
#####	eqProps
	定义：判断两个对象指定的属性值是否相等。通过 R.equals 函数进行相等性判断。可用作柯里化的 predicate 。
	```   
	var o1 = {a: 1, b: 2, c: 3, d: 4};
	var o2 = {a: 10, b: 20, c: 3, d: 40};
	R.eqProps('a', o1, o2); // false
	```   	
	
#####	evolve
	定义：递归地对 object 的属性进行变换，变换方式由 transformation 函数定义。所有非原始类型属性都通过引用来复制。
	如果某个 transformation 函数对应的键在被变换的 object 中不存在，那么该方法将不会执行。
	```   
	var tomato  = {firstName: '  Tomato ', data: {elapsed: 100, remaining: 1400}, id:123};
	var transformations = {
		firstName: R.trim,
		lastName: R.trim, // Will not get invoked
		data: {elapsed: R.add(1), remaining: R.add(-1)}
	};
	R.evolve(transformations, tomato); // {firstName: 'Tomato', data: {elapsed: 101, remaining: 1399}, id:123}
	```   	
	
#####	forEachObjIndexed
	定义：遍历 object，对 object 中的每对 key 和 value 执行方法 fn。
	fn 接收三个参数: (value, key, obj).
	```   
	var printKeyConcatValue = (value, key) => console.log(key + ':' + value);
	R.forEachObjIndexed(printKeyConcatValue, {x: 1, y: 2}); //=> {x: 1, y: 2}
	```   	
	
#####	has
	定义：如果对象自身含有指定的属性，则返回 true；否则返回 false。
	```   
	R.has('name')({name: 'fanerge'}); // true
	```   	
	
#####	hasIn
	定义：如果对象自身或原型链上含有指定的属性，则返回 true；否则返回 false。
	```   
	var Rect = function (w, h) {
		this.width = w;
		this.height = h;
	};
	Rect.prototype.area = function (){
		return this.width * this.heigth;
	};
	var rect1 = new Rect(100, 200);
	R.hasIn('area')(rect1); // true
	```   	
	
#####	invert
	定义：与 R.invertObj 类似，但会将值放入数组中，来处理一个键对应多个值的情况。
	```   
	var raceResultsByFirstName = {
	  first: 'alice',
	  second: 'jake',
	  third: 'alice',
	};
	R.invert(raceResultsByFirstName);
	//=> { 'alice': ['first', 'third'], 'jake':['second'] }
	```   	
	
#####	invertObj
	定义：将对象的键、值交换位置：值作为键，对应的键作为值。交换后的键会被强制转换为字符串。注意，如果原对象同一值对应多个键，采用最后遍历到的键。
	```   
	var raceResults = {
	  first: 'alice',
	  second: 'jake'
	};
	R.invertObj(raceResults);
	//=> { 'alice': 'first', 'jake':'second' }
	```   	
	
#####	keys
	定义：返回给定对象所有可枚举的、自身属性的属性名组成的列表。注意，不同 JS 运行环境输出数组的顺序可能不一致。
	```   
	R.keys({a: 1, b: 2, c: 3}); // ['a', 'b', 'c']
	```   	
	
#####	keysIn
	定义：返回给定对象所有属性（包括 prototype 属性）的属性名组成的列表。注意，不同 JS 运行环境输出数组的顺序可能不一致。
	```   
	var F = function () { this.x = 'X' };
	F.prototype.y = 'Y';
	var f = new F();
	R.keysIn(f); // ['x', 'y']
	```   	
	
#####	lens
	定义：返回封装了给定 getter 和 setter 方法的 lens 。 getter 和 setter 分别用于 “获取” 和 “设置” 焦点（lens 聚焦的值）。
	setter 不会改变原数据。
	```   
	var xLens = R.lens(R.prop('x'), R.assoc('x'));
	R.view(xLens, {x: 1, y: 2}); // 1
	R.set(xLens, 4, {x: 1, y: 2}); // {x: 4, y: 2}
	R.over(xLens, R.negate, {x: 1, y: 2}); // {x: -1, y: 2}
	```   	
	
#####	lensIndex
	定义：返回聚焦到指定索引的 lens。
	```   
	var headLens = R.lensIndex(0);
	R.view(headLens, ['a', 'b', 'c']); // a
	R.set(headLens, 'x', ['a', 'b', 'c']); // ['x', 'b', 'c']
	R.over(headLens, R.toUpper, ['a', 'b', 'c']); // ['A', 'B', 'C']
	```   	
	
#####	lensPath
	定义：返回聚焦到指定路径的 lens。
	```   
	var xHeadYLens = R.lensPath(['x', 0, 'y']);
	R.view(xHeadYLens, {x: [{y: 2, z: 3}, {y: 4, z: 5}]}); // 2
	R.set(xHeadYLens, 1, {x: [{y: 2, z: 3}, {y: 4, z: 5}]}); // {x: [{y: 1, z: 3}, {y: 4, z: 5}]}
	R.over(xHeadYLens, R.negate, {x: [{y: 2, z: 3}, {y: 4, z: 5}]}); // {x: [{y: -2, z: 3}, {y: 4, z: 5}]}
	```   	
	
#####	lensProp
	定义：返回聚焦到指定属性的 lens。
	```   
	var xLens = R.lensProp('x');
	R.view(xLens, {x: 1, y: 2});            //=> 1
	R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
	R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
	```   	
	
#####	merge
	定义：合并两个对象的自身属性（不包括 prototype 属性）。如果某个 key 在两个对象中都存在，使用后一个对象对应的属性值。
	```   
	R.merge({'name': 'fanerge', 'age': 10}, {age: 30}); // {'name': 'fanerge', age: 30}
	```   
	
#####	mergaDeepLeft
	定义：合并两个对象的自身属性（不包括 prototype 属性）。如果某个 key 在两个对象中都存在：
	并且两个值都是对象，则继续递归合并这两个值。
	否则，采用第一个对象的值。
	```   
	R.mergeDeepLeft({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                { age: 40, contact: { email: 'baa@example.com' }});
	// { name: 'fred', age: 10, contact: { email: 'moo@example.com' }}
	```   
	
#####	mergaDeepRight
	定义：合并两个对象的自身属性（不包括 prototype 属性）。如果某个 key 在两个对象中都存在：
	并且两个值都是对象，则继续递归合并这两个值。
	否则，采用第二个对象的值。
	```   
	R.mergeDeepRight({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                 { age: 40, contact: { email: 'baa@example.com' }});
	// { name: 'fred', age: 40, contact: { email: 'baa@example.com' }}
	```   
	
#####	mergaDeepWith
	定义：合并两个对象的自身属性（不包括 prototype 属性）。如果某个 key 在两个对象中都存在：
	并且两个关联的值都是对象，则继续递归合并这两个值。
	否则，使用给定函数对两个值进行处理，并将返回值作为该 key 的新值。
	如果某 key 只存在于一个对象中，该键值对将作为结果对象的键值对。
	```   
	R.mergeDeepWith(R.concat,
                { a: true, c: { values: [10, 20] }},
                { b: true, c: { values: [15, 35] }});
	// { a: true, b: true, c: { values: [10, 20, 15, 35] }}
	```   
	
#####	mergeDeepWithKey
	定义：合并两个对象的自身属性（不包括 prototype 属性）。如果某个 key 在两个对象中都存在：
	并且两个关联的值都是对象，则继续递归合并这两个值。
	否则，使用给定函数对该 key 和对应的两个值进行处理，并将返回值作为该 key 的新值。
	如果某 key 只存在于一个对象中，该键值对将作为结果对象的键值对。
	```   
	let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
	R.mergeDeepWithKey(concatValues,
					   { a: true, c: { thing: 'foo', values: [10, 20] }},
					   { b: true, c: { thing: 'bar', values: [15, 35] }});
	// { a: true, b: true, c: { thing: 'bar', values: [10, 20, 15, 35] }}
	```   	
	
#####	mergeWith
	定义：使用给定的两个对象自身属性（不包括 prototype 属性）来创建一个新对象。
	如果某个 key 在两个对象中都存在，则使用给定的函数对每个对象该 key 对应的 value 进行处理，处理结果作为新对象该 key 对应的值。
	```   
	R.mergeWith(R.concat, 
					{a: true, values: [10, 20]}, 
					{b: true, values: [15, 35]});
	// {a: true, b: true, values: [10, 20, 15, 35]}
	```   	
	
#####	mergeWithKey
	定义：使用给定的两个对象自身属性（不包括 prototype 属性）来创建一个新对象。
	如果某个 key 在两个对象中都存在，则使用给定的函数对该 key 和每个对象该 key 对应的 value 进行处理，处理结果作为新对象该 key 对应的值。
	```   
	let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
	R.mergeWithKey(concatValues,
				   { a: true, thing: 'foo', values: [10, 20] },
				   { b: true, thing: 'bar', values: [15, 35] });
	// { a: true, b: true, thing: 'bar', values: [10, 20, 15, 35] }
	```   	
	
#####	objOf
	定义：创建一个包含单个键值对的对象。
	```   
	var matchPhrases = R.compose(
		R.objOf('must'),
		R.map(R.objOf('match_phrase'))
	);
	matchPhrases(['foo', 'bar', 'baz']); //=> {must: [{match_phrase: 'foo'}, {match_phrase: 'bar'}, {match_phrase: 'baz'}]}
	```   
	
#####	omit
	定义：删除对象中给定的 keys 对应的属性。
	```   
	R.omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); // {b: 2, c: 3}
	```   	
	
#####	over
	定义：对数据结构中被 lens 聚焦的部分进行函数变换。
	```   
	var headLens = R.lensIndex(0);
	R.over(headLens, R.toUpper, ['foo', 'bar', 'baz']); // ['FOO', 'bar', 'baz']
	```   	
	
#####	path
	定义：取出给定路径上的值。
	```   
	R.path(['a', 'b'], {a: {b: 2}}); // 2
	```   	
	
#####	pathOr
	定义：如果非空对象在给定路径上存在值，则将该值返回；否则返回给定的默认值。
	```   
	R.pathOr('N/A', ['a', 'b'], {a: {b: 2}}); // 2
	```   	
	
#####	pick
	定义：返回对象的部分拷贝，其中仅包含指定键对应的属性。如果某个键不存在，则忽略该属性。
	```   
	R.pick(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); // {a: 1, d: 4}
	```   	
	
#####	pickAll
	定义：与 pick 类似，但 pickAll 会将不存在的属性以 key: undefined 键值对的形式返回。
	```   
	R.pickAll(['a', 'd', 'e'], {a: 1, b: 2, c: 3, d: 4}); // {a: 1, d: 4, e: undefined}
	```   	
	
#####	pickBy
	定义：返回对象的部分拷贝，其中仅包含 key 满足 predicate 的属性。
	```   
	var isUpperCase = (val, key) => key.toUpperCase() === key;
	R.pickBy(isUpperCase, {a: 1,b: 2, A: 3, B: 4}); // {A: 3, B: 4}
	```   	
	
#####	project
	定义：模拟 SQL 中的 select 语句。
	```   
	var abby = {name: 'Abby', age: 7, hair: 'blond', grade: 2};
	var fred = {name: 'Fred', age: 12, hair: 'brown', grade: 7};
	var kids = [abby, fred];
	R.project(['name', 'grade'], kids); // [{name: 'Abby', grade: 2}, {name: 'Fred', grade: 7}]
	```   
	
#####	prop
	定义：取出对象中指定属性的值。如果不存在，则返回 undefined。
	```   
	R.prop('x', {x: 100}); // 100
	```   
	
#####	propOr
	定义：对于给定的非空对象，如果指定属性存在，则返回该属性值；否则返回给定的默认值。
	```   
	var alice = {
	  name: 'ALICE',
	  age: 101
	};
	var favorite = R.prop('favoriteLibrary');
	var favoriteWithDefault = R.propOr('Ramda', 'favoriteLibrary');

	favorite(alice);  // undefined
	favoriteWithDefault(alice);  // 'Ramda'
	```   
	
#####	props
	定义：返回 prop 的数组：输入为 keys 数组，输出为对应的 values 数组。values 数组的顺序与 keys 的相同。
	```   
	R.props(['x', 'y'], {x: 1, y: 2}); // [1, 2]
	```   
	
#####	set
	定义：通过 lens 对数据结构聚焦的部分进行设置。
	```   
	var xLens = R.lensProp('x');
	R.set(xLens, 4, {x: 1, y: 2}); // {x: 4, y: 2}
	```   
	
#####	toPairs
	定义：将一个对象的属性转换成键、值二元组类型的数组，只处理对象自身的属性。注意：不同 JS 运行环境输出数组的顺序可能不一致。
	```   
	R.toPairs({a: 1, b: 2, c: 3}); // [['a', 1], ['b', 2], ['c', 3]]
	```   	
	
#####	toPairsIn
	定义：将一个对象的属性转换成键、值二元组类型的数组，包括原型链上的属性。注意，不同 JS 运行环境输出数组的顺序可能不一致。
	```   
	var F = function () { this.x = 'X'; };
	F.prototype.y = 'Y';
	var f = new F();
	R.toPairsIn(f); // [['x','X'], ['y','Y']]
	```   	
	
#####	values
	定义：返回对象所有自身可枚举的属性的值。注意：不同 JS 运行环境输出数组的顺序可能不一致。
	```   
	R.values({a: 1, b: 2, c: 3}); //=> [1, 2, 3]
	```   	
	
#####	valuesIn
	定义：返回对象所有属性的值，包括原型链上的属性。注意：不同 JS 运行环境输出数组的顺序可能不一致。
	```   
	var F = function() { this.x = 'X'; };
	F.prototype.y = 'Y';
	var f = new F();
	R.valuesIn(f); //=> ['X', 'Y']
	```   	
	
#####	view
	定义：返回数据结构中，lens 聚焦的部分。lens 的焦点决定了数据结构中的哪部分是可见的。
	```   
	var xLens = R.lensProp('x');
	R.view(xLens, {x: 1, y: 2});  //=> 1
	```   	
	
#####	where
	定义：接受一个测试规范对象和一个待检测对象，如果测试满足规范，则返回 true，否则返回 false。测试规范对象的每个属性值都必须是 predicate 。
	每个 predicate 作用于待检测对象对应的属性值，如果所有 predicate 都返回 true，则 where 返回 true，否则返回 false 。
	where 非常适合于需要声明式表示约束的函数，比如 filter 和 find 。
	```   
	var pred = R.where({
	  a: R.equals('foo'),
	  b: R.complement(R.equals('bar')),
	  x: R.gt(R.__, 10),
	  y: R.lt(R.__, 20)
	});

	pred({a: 'foo', b: 'xxx', x: 11, y: 19}); //=> true
	pred({a: 'xxx', b: 'xxx', x: 11, y: 19}); //=> false
	```   	
	
#####	whereEq
	定义：接受一个测试规范对象和一个待检测对象，如果测试满足规范，则返回 true，否则返回 false。
	如果对于每一个测试规范对象的属性值，待检测对象中都有一个对应的相同属性值，则 where 返回 true，否则返回 false 。
	whereEq 是 where 的一种特殊形式。
	```   
	var pred = R.whereEq({a: 1, b: 2});
	pred({a: 1});              //=> false
	pred({a: 1, b: 2});        //=> true
	```   	
	
####	Logic

#####	allPass
	定义：传入包含多个 predicate 的列表，返回一个 predicate：如果给定的参数满足列表中的所有 predicate ，则返回 true。
	该函数返回一个柯里化的函数，参数个数由列表中参数最多的 predicate 决定。
	```   
	var isQueen = R.propEq('rank', 'Q');
	var isSpade = R.propEq('suit', '??');
	var isQueenOfSpades = R.allPass([isQueen, isSpade]);
	isQueenOfSpades({rank: 'Q', suit: '??'}); // true
	```   

#####	anyPass
	定义：传入包含多个 predicate 的列表，返回一个 predicate：只要给定的参数满足列表中的一个 predicate ，就返回 true。
	该函数返回一个柯里化的函数，参数个数由列表中参数最多的 predicate 决定。
	```   
	var isQueen = R.propEq('rank', 'Q');
	var isSpade = R.propEq('suit', '??');
	var isQueenOfSpades = R.anyPass([isQueen, isSpade]);
	isQueenOfSpades({rank: 'K', suit: '??'}); // true
	```   

#####	and（针对于值）
	定义：如果两个参数都是 true，则返回 true；否则返回 false。相当于且（&&）
	```   
	R.and(true, true); // true
	```   		
	
#####	or（针对于值）
	定义：逻辑或运算，只要有一个参数为 truth-y，就返回 true；否则返回 false。
	```   
	R.or(true, true); // true
	```   

#####	not（针对于值）
	定义：逻辑非运算。 当传入参数为 false-y 值时，返回 true；truth-y 值时，返回 false。
	```   
	R.not(true); // false
	```   

#####	both（针对于函数）
	定义：该函数调用两个函数，并对两函数返回值进行与操作。若第一个函数结果为 false-y 值 (false, null, 0 等)，则返回该结果，否则返回第二个函数的结果。注意，both 为短路操作，即如果第一个函数返回 false-y 值，则不会调用第二个函数。
	```   
	var gt10 = R.gt(R.__, 10);
	var lt20 = R.lt(R.__, 20);
	var f = R.both(gt10, lt20);
	f(15); // true
	```   		
#####	either（针对于函数）
	定义：返回由 || 运算符连接的两个函数的包装函数。如果两个函数中任一函数的执行结果为 truth-y，则返回其执行结果。 注意，这个是短路表达式，意味着如果第一个函数返回 truth-y 值的话，第二个函数将不会执行。
	```   
	var gt10 = x => x > 10;
	var even = x => x % 2 === 0;
	var f = R.either(gt10, even);
	f(101); // true
	```   
	
#####	complement（针对于函数）
	定义：对函数的返回值取反。接受一个函数 f，返回一个新函数 g：在输入参数相同的情况下，若 f 返回 'true-y' ，则 g 返回 false-y ，反之亦然。
	```   
	var isNotNil = R.complement(R.isNil);
	isNotNil(null); // true
	```   	
	
#####	cond
	定义：返回一个封装了 if / else，if / else if/ else逻辑的函数 fn。 R.cond 接受列表元素为 [predicate，transformer] 的列表。 
	fn 的所有参数顺次作用于每个 predicate，直到有一个返回 "truthy" 值，此时相应 transformer 对参数处理，并作为 fn 的结果返回。
	如果没有 predicate 匹配，则 fn 返回 undefined。
	```   
	var fn = R.cond([
		[R.equals(0), R.always('water freezes at 0')],
		[R.equals(100), R.always('water freezes at 100')],
		[R.T, temp => `nothing special happens at ${temp}`],
	]);
	fn(3); // nothing special happens at 3
	```   	
	
#####	defaultTo
	定义：如果第二个参数不是 null、undefined 或 NaN，则返回第二个参数，否则返回第一个参数（默认值）。
	```   
	var defaultTo42 = R.defaultTo(42);
	defaultTo42(undefined); // 42 
	defaultTo42(13); // 13 
	```   	
	
#####	ifElse
	定义：根据 condition predicate 的返回值调用 onTrue 或 onFalse 函数。
	```   
	var inCount = R.ifElse(
		R.has('count'),
		R.over(R.lensProp('count'), R.inc),
		R.assoc('count', 1)
	);
	incCount({});           //=> { count: 1 }
	incCount({ count: 1 }); //=> { count: 2 }
	```   	
	
#####	isEmpty
	定义：检测给定值是否为其所属类型的空值，若是则返回 true ；否则返回 false 。
	```   
	R.isEmpty([1, 2, 3]); // false
	R.isEmpty(''); // true
	```   

#####	pathSatisfies
	定义：如果对象的给定路径上的属性满足 predicate，返回 ture；否则返回 false。
	```   
	R.pathSatisfies(y => y > 0, ['x', 'y'], {x: {y: 2}}); // true
	```   

#####	propSatisfies
	定义：如果指定的对象属性满足 predicate，返回 true；否则返回 false。
	```   
	R.propSatisfies(x => x > 0, 'x', {x: 1, y: 2}); // true
	```   

#####	unless
	定义：判断输入值是否满足 predicate，若不符合，则将输入值传给 whenFalseFn 处理，并将处理结果作为返回；若符合，则将输入值原样返回。
	```   
	let safeInc = R.unless(R.isNil, R.inc);
	safeInc(null); //=> null
	safeInc(1); //=> 2
	```   

#####	until
	定义：接受一个 predicate ，transform function 和 初始值，返回一个与初始值相同类型的值。
	对输入值进行 transform ，直到 transform 的结果满足 predicate，此时返回这个满足 predicate 的值。
	```   
	R.until(R.gt(R.__, 100), R.multiply(2))(1); // 128
	```   

#####	when
	定义：判断输入值是否满足 predicate，若符合，则将输入值传给 whenTrueFn 处理，并将处理结果作为返回；若不符合，则将输入值原样返回。
	```   
	var truncate = R.when(
	  R.propSatisfies(R.gt(R.__, 10), 'length'),
	  R.pipe(R.take(10), R.append('…'), R.join(''))
	);
	truncate('12345');         // '12345'
	truncate('0123456789ABC'); // '0123456789…'
	```   
	
####	Function

#####	__
	定义：柯里化函数的参数占位符。允许部分应用于任何位置的参数。
	```   
	假设 g 代表柯里化的三元函数
	g(1, 2, 3)
	g(R.__, 2, 3)(1)
	g(R.__, R__, 3)(1)(2)
	g(R.__, 2, R.__)(1)(3)
	g(R.__, 2)(R.__, 3)(1)
	// 这些函数都是等价的。
	var greet = R.replace('{name}', R.__, 'Hello, {name}!');
	greet('Fanerge'); // Hello, Fanerge
	```   	
	
#####	addIndex
	定义：通过向列表迭代函数的回调函数添加两个新的参数：当前索引、整个列表，创建新的列表迭代函数。
	```   
	var mapIndexed = R.addIndex(R.map);
	mapIndexed((val, idx) => {idx + '-' + val}, ['f', 'a', 'n']); // ['0-f', '1-a', '2-n']
	```   
	
#####	always
	定义：返回一个返回恒定值的函数。注意，对于非原始值，返回的值是对原始值的引用。
	```   
	var t = R.always('Tee');
	t(); // Tee
	```   
	
#####	ap
	定义：将函数列表作用于值列表上。
	```   
	R.ap([R.multiply(2), R.add(3)], [1, 2, 3]); // [2, 4, 6, 4, 5, 6]
	```   

#####	apply
	定义：将函数 fn 作用于参数列表 args。apply 可以将变参函数转换为为定参函数。如果上下文很重要，则 fn 应该绑定其上下文。
	```   
	var nums = [1, 2, 3, -99, 42, 6, 7];
	R.apply(Math.max, nums); // 42
	```   

#####	applySpec
	定义：接受一个属性值为函数的对象，返回一个能生成相同结构对象的函数。返回的函数使用传入的参数调用对象的每个属性位对应的函数，来生成相应属性的值。
	```   
	var getMetrics = R.applySpec({	
		sum: R.add,
		nested: { mul: R.multiply }
	});
	getMetrics(2, 4); // {sum: 6, nested: { mul: 8 }}
	```   
	
#####	ascend
	定义：由返回值可与 < 和 > 比较的函数，创建一个升序比较函数。
	```   
	var byAge = R.ascend(R.prop('age'));
	var people = [{name: 'yzf', age: 11}, {name: 'wkm', age: 10}];
	var peopleByYoungestFirst = R.sort(byAge, people); // [{"name":"wkm","age":10},{"name":"yzf","age":11}]
	```   	
	
#####	binary
	定义：将任意元函数封装为二元函数（只接受2个参数）中。任何额外的参数都不会传递给被封装的函数。
	```   
	var takesThreeArgs = function (a, b, c){
		return [a, b, c];
	};
	vat takeTwoArgs = R.binary(takesThreeArgs);
	// takeTwoArgs.length; // 2
	takeTwoArgs(1, 2, 3); // [1, 2, undefined]
	```   		
	
#####	bind
	定义：创建一个绑定了上下文的函数。
	注意：与 Function.prototype.bind 不同，R.bind 不会绑定额外参数。
	```   
	var log = R.bind(console.log, console);
	log(1); // 1
	```   
	
#####	call
	定义：提取第一个参数作为函数，其余参数作为刚提取的函数的参数，调用该函数并将结果返回。
	```   
	R.call(R.add, 1, 2); // 3
	```   	
	
#####	comparator
	定义：由首个参数是否小于第二个参数的判断函数，生成一个比较函数。
	```   
	var byAge = R.comparator((a, b) => a.age < b.age);
	var people = [{name: 'yzf', age: 11}, {name: 'wkm', age: 10}];
	var peopleByIncreasingAge = R.sort(byAge, people); // [{"name":"wkm","age":10},{"name":"yzf","age":11}]
	```   	
	
#####	compose
	定义：从右往左执行函数组合（右侧函数的输出作为左侧函数的输入）。最右侧函数可以是任意元函数（参数个数不限），其余函数必须是一元函数。
	```   
	R.compose(R.add(1), R.multiply(2))(3); // 7
	```   		
	
#####	composeK
	定义：接受一系列函数，返回从右向左的 Kleisli 组合，每个函数必须返回支持 chain 操作的值。
	```   
	R.composeK(h, g, f) 等同于 R.compose(R.chain(h)，R.chain(g)，R.chain(f))。
	```   	
	
#####	composeP
	定义： 从右向左执行返回 Promise 的函数的组合。最右边的函数可以是任意元函数（参数个数不限）; 其余函数必须是一元函数。
	```   
	var db = {
		  users: {
				JOE: {
				  name: 'Joe',
				  followers: ['STEVE', 'SUZY']
				}
		  }
	};
	var lookupUser = (userId) => Promise.resolve(db.users[userId]);
	var lookupFollowers = (user) => Promise.resolve(user.followers);
	lookupUser('JOE').then(lookupFollowers);
	var followersForUser = R.composeP(lookupFollowers, lookupUser);
	followersForUser('JOE').then(followers => console.log('Followers:', followers)) // ["STEVE","SUZY"]
	```   
	
#####	construct
	定义：将构造函数封装进柯里化函数，新函数与原构造函数的传入参数类型及返回值类型相同。
	```   
	// constructor function
	function Animal (kind) {
		this.kind = kind;
	}
	Animal.prototype.sighting = function () {
		return `it's a ${this.kind}!`;
	};
	var AnimalConstructor = R.construct(Animal);
	console.log(AnimalConstructor('PIG')); // {"kind":"PIG"}
	```   
	
#####	constructN
	定义：将构造函数封装进柯里化函数，新函数与原构造函数的传入参数类型及返回值类型相同。为了能够使用变参的构造函数，返回函数的元数需要明确指定。
	```   
	function Salad() {
	  this.ingredients = arguments;
	};
	Salad.prototype.recipe = function() {
	  var instructions = R.map((ingredient) => (
		'Add a whollop of ' + ingredient, this.ingredients)
	  )
	  return R.join('\n', instructions)
	}

	var ThreeLayerSalad = R.constructN(3, Salad)

	// Notice we no longer need the 'new' keyword, and the constructor is curried for 3 arguments.
	var salad = ThreeLayerSalad('Mayonnaise')('Potato Chips')('Ketchup')
	console.log(salad.recipe());
	// Add a whollop of Mayonnaise
	// Add a whollop of Potato Chips
	// Add a whollop of Potato Ketchup
	```   
	
#####	converge
	定义：接受一个 converging 函数和一个分支函数列表，返回一个新函数。
	当被调用时，新函数接受参数，并将这些参数转发给每个分支函数；然后将每个分支函数的计算结果作为参数传递给 converging 函数，converging 函数的计算结果即新函数的返回值。
	```   
	var average = R.converge(R.divide, [R.sum, R.length]);
	average([1, 2, 3, 4, 5, 6, 7]); // 4
	```   
	
#####	curry
	定义：对函数进行柯里化。柯里化函数与其他语言中的柯里化函数相比，有两个非常好的特性：
	1.参数不需要一次只传入一个。 
		g(1)(2)(3) === g(1, 2, 3)
	2.占位符值 R.__ 可用于标记暂未传入参数的位置。允许部分应用于任何参数组合，而无需关心它们的位置和顺序。
		F(1, 2, 3) === F(R.__, 2, 3)(1) === F(R.__, 3)(1)(2)
	```   
	var addFourNumbers = (a, b, c, d) => a + b + c + d;
	var curriedAddFourNumbers = R.curry(addFourNumbers);
	var f = curriedAddFourNumbers(1); // 返回剩余三个参数的函数
	f(2, 3, 4); // 10(当参数全部传入，才返回结果)
	```   
	
#####	curryN
	定义：对函数进行柯里化，并限制柯里化函数的元数。
	```   
	var sumArgs = (...args) => R.sum(args);
	var curriedFourNumbers = R.curryN(5, sumArgs);
	var f = curriedAddFourNumbers(1, 2);
	var g = f(3, 4, 5); // 15
	```   	
	
#####	descend
	定义：由返回值可与 < 和 > 比较的函数，创建一个降序比较函数。
	```   
	var byAge = R.descend(R.prop('age'));
	var people = [
		
	];
	var peopleByOldFirst = R.sort(byAge, people);
	```   	
	
#####	empty
	定义：根据传入参数的类型返回其对应的空值。
	Ramda 定义了各类型的空值如下：Array ([])，Object ({})，String ('')，和 Arguments。empty 还支持其它定义了 <Type>.empty 和/或 <Type>.prototype.empty 的类型。
	```   
	R.empty(Just(42)); // Noting()
	R.empty([1, 2]); // []
	```   	
	
#####	F
	定义：恒定返回 false 的函数。忽略所有的输入参数。
	```   
	R.F(); // false
	```   	
	
#####	flip
	定义：交换函数前两个参数的位置。
	```   
	var mergeThree = (a, b, c) => [].concat(a, b, c);
	mergeThree(1, 2, 3); // [1, 2, 3]
	R.flip(mergeThree)(1, 2, 3); // [2, 1, 3]
	```   	
	
#####	identity
	定义：将输入值原样返回。适合用作默认或占位函数。
	```   
	R.identity(1); // 1
	var obj = {};
	R.identity(obj) === obj; // true
	```   	
	
#####	invoker
	定义：将具有指定元数（参数个数）的具名方法，转换为可以被给定参数和目标对象直接调用的函数。
	返回的函数是柯里化的，它接收 arity + 1 个参数，其中最后一个参数是目标对象。
	```   
	var sliceFrom = R.invoker(1, 'slice');
	sliceFrom(6, 'abcdefghijklm'); //=> 'ghijklm'
	```   	
	
#####	juxt
	定义：juxt 将函数列表作用于值列表。
	```   
	var getRange = R.juxt([Math.min, Math.max]);
	getRange(3, 4, 9, -3); // [-3, 9]
	```   	
	
#####	lift
	定义：提升一个多元函数，使之能映射到列表、函数或其他符合 FantasyLand Apply spec 规范的对象上。
	```   
	var madd3 = R.lift((a, b, c) => a + b + c);
	madd3([1,2,3], [1,2,3], [1]); //=> [3, 4, 5, 4, 5, 6, 5, 6, 7]
	```   	
	
#####	liftN
	定义：将一个函数提升为指定元数的函数，使之能映射到多个列表、函数或其他符合 FantasyLand Apply spec 规范的对象上。
	```   
	var madd3 = R.liftN(3, (...args) => R.sum(args));
	madd3([1,2,3], [1,2,3], [1]); //=> [3, 4, 5, 4, 5, 6, 5, 6, 7]
	```   	
	
#####	memoize
	定义：memoize 方法可以缓存函数的计算结果。
	创建一个新函数，被调用时，缓存特定参数对应的经 fn 计算的结果，并将结果返回。
	此后如果用相同的参数调用缓存的 fn 时，直接返回该参数对应的缓存结果，不必再调用 fn。
	```   
	var count = 0;
	const factorial = R.memoize(n => {
		count += 1;
		return R.product(R.range(1, n + 1));
	});
	factorial(5); // 120
	factorial(5); // 120
	count; // 1(只进行了一次运算)
	```   	
	
#####	memoizeWith
	定义：R.memoize 的可定制版本。memoizeWith 需要一个额外的函数，该函数接受一个参数集，用于创建缓存的键值，在该缓存中会存储被缓存函数的结果。
	注意，生成缓存键值时，要避免可能会错误地覆盖之前已缓存键值对的冲突。
	```   
	var count = 0;
	const factorial = R.memoizeWith(R.identity, n => {
		count += 1;
		return R.product(R.range(1, n + 1));
	});
	factorial(5); // 120
	factorial(5); // 120
	count; // 1(只进行了一次运算)
	```   		

#####	nAry
	定义：将一个任意元（包括零元）的函数，封装成一个确定元数（参数个数）的函数。任何多余的参数都不会传入被封装的函数。
	```   
	var takesTwoArgs = (a, b) => [a, b];
	takesTwoArgs.length; // 2
	takesTwoArgs(1, 2); // [1, 2]
	var takesOneArg = R.nAry(1, takeTwoArgs);
	takesOneArg.length; // 1
	takesOneArg(1, 2); // [1, undefined]
	```   	
	
#####	nthArg
	定义：返回一个函数，该函数返回它的第 n 个参数。
	```   
	R.nthArg(1)('a', 'b', 'c'); // b
	```   	
	
#####	o
	定义：o 是一个柯里化组合函数，返回一元函数。
	类似于 compose，o 从右到左执行函数组合。但与 compose 不同的是，传递给 o 的最右边的函数为一元函数。
	```   
	R.o(R.multiply(10), R.add(10))(-4); // 60
	```   	
	
#####	of
	定义：将给定值作为元素，封装成单元素数组。
	```   
	R.of(42); // [42]
	```   	
	
#####	once
	定义：创建一个只能调用一次的函数。
	将给定函数 fn 封装到新函数fn'中，fn' 确保 fn 只能调用一次。重复调用fn' ，只会返回第一次执行时的结果。
	```   
	var addOneOnce = R.once(x => x + 1);
	addOneOnce(10); // 11
	addOneOnce(51); // 11
	```   	
	
#####	partial
	定义：部分应用。
	接收两个参数：函数 f 和 参数列表，返回函数 g。当调用 g 时，将初始参数和 g 的参数顺次传给 f，并返回 f 的执行结果。
	```   
	var multiply2 = (a, b) => a * b;
	var double = R.partial(multiply2, [2]);
	double(3); // 2*3=6
	```   	
	
#####	partialRight
	定义：部分应用。
	接收两个参数：函数 f 和 参数列表，返回函数 g。当调用 g 时，将 g 的参数和初始参数顺序传给 f，并返回 f 的执行结果。
	```   
	var multiply2 = (a, b) => a * b;
	var double = R.partial(multiply2, [2]);
	double(3); // 3*2=6
	```   	
	
#####	pipe
	定义：从左往右执行函数组合。最左边的函数可以是任意元函数（参数个数不限），其余函数必须是一元函数。
	```   
	var f = R.pipe(Math.pow, R.negate, R.inc);
	f(3, 4); // -(3^4) + 1
	```   	
	
#####	pipeK
	定义：将一系列函数，转换成从左到右的 Kleisli 组合，每个函数必须返回支持chain操作的值。
	R.pipeK(f, g, h) 等价于 R.pipe(R.chain(f), R.chain(g), R.chain(h))。
	```   
	var getStateCode = R.pipeK(
	  parseJson,
	  get('user'),
	  get('address'),
	  get('state'),
	  R.compose(Maybe.of, R.toUpper)
	);
	getStateCode('{"user":{"address":{"state":"ny"}}}');
	// Just('NY')
	```   	
	
#####	pipeP
	定义：从左往右执行返回 Promise 的函数的组合。最左边的函数可以是任意元函数（参数个数不限）；其余函数必须是一元函数。
	```   
	var followersForUser = R.pipeP(db.getUserById, db.getFollowers);
	```   	
	
#####	T
	定义：恒定返回 true 的函数。忽略所有的输入参数。
	```   
	R.T(); // true
	```   	
	
#####	tap
	定义：对输入的值执行给定的函数，然后返回输入的值。
	```   
	var sayX = x => console.log('x is ' + x);
	R.tap(sayX, 100); // 100
	```   	
	
#####	tryCatch
	定义：tryCatch 接受两个函数：tryer 和 catcher，生成的函数执行 tryer，若未抛出异常，则返回执行结果。
	若抛出异常，则执行 catcher，返回 catcher 的执行结果。注意，为了有效的组合该函数，tryer 和 catcher 应返回相同类型的值。
	```   
	R.tryCatch(R.prop('x'), R.f)({x: true}); // true
	```   	
	
#####	unapply
	定义：输入一个只接收单个数组作为参数的函数，返回一个新函数：
	接收任意个参数；
	将参数组成数组传递给 fn ；
	返回执行结果。
	换言之，R.unapply 将一个使用数组作为参数的函数，变为一个不定参函数。 R.unapply 是 R.apply 的逆函数
	```   
	R.unapply(JSON.stringify, 1, 2, 3); // '[1, 2, 3]'
	```   
	
#####	unary
	定义：将任意元（包括零元）函数封装成一元函数。任何额外的参数都不会传递给被封装的函数。
	```   
	var takesTwoArgs = function(a, b) {
	  return [a, b];
	};
	takesTwoArgs.length; //=> 2
	takesTwoArgs(1, 2); //=> [1, 2]

	var takesOneArg = R.unary(takesTwoArgs);
	takesOneArg.length; //=> 1
	// 只有一个参数能被传递到函数当中
	takesOneArg(1, 2); //=> [1, undefined]
	```   
	
#####	uncurryN
	定义：将一个柯里化的函数转换为一个 n 元函数。
	```   
	var addFour = a => b => c => d => a + b + c + d;
	var uncurriedAddFour = R.uncurryN(4, addFour);
	uncurriedAddFour(1, 2, 3, 4); // 10
	```   
	
#####	useWith
	定义：接受一个函数 fn 和一个 transformer 函数的列表，返回一个柯里化的新函数。
	当被调用时，新函数将每个参数转发给对应位置的 transformer 函数，然后将每个 transformer 函数的计算结果作为参数传递给 fn，fn 的计算结果即新函数的返回值。
	如果新函数传传入参数的数量比 transformer 函数的数量多，多出的参数会作为附加参数直接传给 fn 。
	如果不需要处理多出的那部分参数，除了忽略之外，也可以用 identity 函数来作为 transformer ，以保证新函数的参数数量是确定的。
	```   
	R.useWith(Math.pow, [R.identity, R.identity])(3, 4); //=> 81
	```   

####	Math
	
#####	add
	定义：两数相加。
	```   
	R.add(2)(3); // 5
	```   
	
#####	divide
	定义：两数相除。等价于a / b。
	```   
	R.divide(71, 100); // 0.71
	```   

#####	dec（相当于自减）
	定义：减一。与i--的区别为会生产一个新数据（不改变原有值）。
	```   
	R.dec(42); // 41
	```   
	
#####	inc（相当于自加）
	定义：加1。
	```   
	R.inc(42); // 43
	```   	
	
#####	mathMod
	定义：mathMod 和算术取模操作类似，而不像 % 操作符 （或 R.modulo）。所以 -17 % 5 等于 -2，而 mathMod(-17, 5) 等于 3 。
	mathMod 要求参数为整型，并且当模数等于 0 或者负数时返回 NaN 。
	```   
	R.mathMod(-17, 5); // 3
	R.mathMod(17, 5); // 2
	```   	
	
#####	mean
	定义：返回给定数字列表的平均值。
	```   
	R.mean([2, 7, 9]); // 6
	```   	

#####	median
	定义：返回给定数字列表的中位数。
	```   
	R.median([2, 9, 7]); // 7
	```   	

#####	modulo
	定义：用第一个参数除以第二个参数，并返回余数。注意，该函数是 JavaScript-style 的求模操作。数学求模另见 mathMod。
	```   
	R.modulo(17, 3); // 2
	R.modulo(-17, 3); // -2
	```   	

#####	multiply
	定义：两数相乘，等价于柯里化的 a * b 。
	```   
	R.multiply(2, 3); // 6
	```   

#####	negate
	定义：取反操作。
	```   
	R.negate(42); // -42
	```   
	
#####	product
	定义：列表中的所有元素相乘。
	```   
	R.product([2, 3, 4]); // 24
	```   
	
#####	subtract
	定义：首个参数减去第二个参数。
	```   
	R.subtract(1, 2); // -1
	```   
	
#####	sum
	定义：对数组中所有元素求和。
	```   
	R.sum([1, 2, 3]); // 6
	```   

####	Relation

#####	clamp
	定义：将数字限制在指定的范围内。
	clamp 也可用于其他有序类型，如字符串和日期。
	```   
	R.clamp(1, 10, -5); // 1
	R.clamp(1, 10, 4); // 4
	```   

#####	countBy
	定义：根据给定函数提供的统计规则对列表中的元素进行分类计数。
	返回一个对象，其键值对为：fn 根据列表元素生成键，列表中通过 fn 映射为对应键的元素的个数作为值。
	注意，由于 JavaScript 对象的实现方式，所有键都被强制转换为字符串。
	```   
	var numbers = [1.0, 1.1, 1.2, 2.0, 3.0, 2.2];
	R.countBy(Math.floor, numbers); // {'1': 3, '2': 2, '3': 1 }
	```   

#####	difference
	定义：求差集。求第一个列表中，未包含在第二个列表中的任一元素的集合。对象和数组比较数值相等，而非引用相等。
	```   
	R.difference([1, 2, 3, 4], [7, 6, 5, 4, 3]); // [1, 2]
	```   

#####	differenceWith
	定义：求第一个列表中未包含在第二个列表中的所有元素的集合（集合中没有重复元素）。
	两列表中的元素通过 predicate 判断相应元素是否同时 “包含在” 两列表中。
	```   
	var cmp = (x, y) => x.a === y.a;
	var l1 = [{a: 1}, {a: 2}, {a: 3}];
	var l2 = [{a: 3}, {a: 4}];
	R.differenceWith(cmp, l1, l2); // [{a: 1}, {a: 2}]
	```   

#####	eqBy
	定义：接受一个函数和两个值，通过传入函数对两个值进行相等性判断。如果两个值的计算结果相等，则返回 true ；否则返回 false 。
	```   
	R.eqBy(Math.abs, 5, -5); // true
	```   

#####	equals
	定义：如果传入的参数相等，返回 true；否则返回 false。可以处理几乎所有 JavaScript 支持的数据结构。
	```   
	R.equals(1, 1); // true
	R.equals(['1'], ['1']); // true
	```   

#####	gt
	定义：如果首个参数大于第二个参数，返回 true；否则返回 false。
	```   
	R.gt(2, 1); // true
	```   

#####	gte
	定义：如果首个参数大于等于第二个参数，返回 true；否则返回 false。
	```   
	R.gte(2, 1); // true
	```   

#####	identical
	定义：如果两个参数是完全相同，则返回 true，否则返回 false。
	如果它们引用相同的内存，也认为是完全相同的。NaN 和 NaN 是完全相同的；0 和 -0 不是完全相同的。
	```   
	var o = {};
	R.identical(o, o); //=> true
	R.identical(1, 1); //=> true
	```   

#####	innerJoin
	定义：接受一个 predicate pred 、列表 xs 和 ys ，返回列表 xs'。
	依次取出 xs 中的元素，若通过 pred 判断等于 ys 中的一个或多个元素，则放入 xs' 。
	pred 必须为二元函数，两个参数分别来自于对应两个列表中的元素。
	```   
	R.innerJoin(
	  (record, id) => record.id === id,
	  [{id: 824, name: 'Richie Furay'},
	   {id: 956, name: 'Dewey Martin'},
	   {id: 313, name: 'Bruce Palmer'},
	   {id: 456, name: 'Stephen Stills'},
	   {id: 177, name: 'Neil Young'}],
	  [177, 456, 999]
	);
	// [{id: 456, name: 'Stephen Stills'}, {id: 177, name: 'Neil Young'}]
	```   

#####	intersection
	定义：取出两个 list 中相同的元素组成的 set （集合：没有重复元素）。
	```   
	R.intersection([1, 2, 3, 4], [7, 6, 5, 4, 3]); // [4, 3]
	```   
	
#####	intersectionWith
	定义：取出两个 list 中相同的元素组成的 set （集合：没有重复元素）。由给定的 predicate 进行相同性判断。
	```   
	var buffaloSpringfield = [
	  {id: 824, name: 'Richie Furay'},
	  {id: 956, name: 'Dewey Martin'},
	  {id: 313, name: 'Bruce Palmer'},
	  {id: 456, name: 'Stephen Stills'},
	  {id: 177, name: 'Neil Young'}
	];
	var csny = [
	  {id: 204, name: 'David Crosby'},
	  {id: 456, name: 'Stephen Stills'},
	  {id: 539, name: 'Graham Nash'},
	  {id: 177, name: 'Neil Young'}
	];

	var list = R.intersectionWith(R.eqBy(R.prop('id')), buffaloSpringfield, csny);
	//=> [{id: 456, name: 'Stephen Stills'}, {id: 177, name: 'Neil Young'}]
	```   
	
#####	lt
	定义：如果首个参数小于第二个参数，返回 true；否则返回 false。
	```   
	R.lt(2, 1); // false
	R.lt('a', 'b'); // true
	```   
	
#####	lte
	定义：如果首个参数小于或等于第二个参数，返回 true；否则返回 false。
	```   
	R.lt(2, 2); // false
	R.lt('a', 'b'); // true
	```   
	
#####	max
	定义：返回两个参数中的较大值。
	```   
	R.max(789, 123); // 789
	```   
	
#####	maxBy
	定义：接收一个函数和两个值，返回使给定函数执行结果较大的值。
	```   
	var square = n => n * n;
	R.maxBy(square, -3, 2); // -3
	R.reduce(R.maxBy(square), 0, [3, -5, 4, 1, -2]); // -5
	```   
	
#####	min
	定义：返回两个参数中的较小值。
	```   
	R.min(789, 123); // 123
	R.min('a', 'c'); // a
	```   
	
#####	minBy
	定义：接收一个函数和两个值，返回使给定函数执行结果较小的值。
	```   
	var square = n => n * n;
	R.minBy(square, -3, 2); // 2
	```   
	
#####	pathEq
	定义：判断对象的嵌套路径上是否为给定的值，通过 R.equals 函数进行相等性判断。常用于列表过滤。
	```   
	var user1 = { address: { zipCode: 90210 } };
	var user2 = { address: { zipCode: 55555 } };
	var user3 = { name: 'Bob' };
	var users = [ user1, user2, user3 ];
	var isFamous = R.pathEq(['address', 'zipCode'], 90210);
	var list = R.filter(isFamous, users); // [{"address":{"zipCode":90210}}]
	```   
	
#####	propEq
	定义：如果指定对象属性与给定的值相等，则返回 true ；否则返回 false 。通过 R.equals 函数进行相等性判断。
	```   
	var abby = {name: 'Abby', age: 7, hair: 'blond'};
	var fred = {name: 'Fred', age: 12, hair: 'brown'};
	var rusty = {name: 'Rusty', age: 10, hair: 'brown'};
	var alois = {name: 'Alois', age: 15, disposition: 'surly'};
	var kids = [abby, fred, rusty, alois];
	var hasBrownHair = R.propEq('hair', 'brown');
	R.filter(hasBrownHair, kids); // [fred, rusty]
	```   
	
#####	sortBy
	定义：根据给定的函数对列表进行排序。
	```   
	var sortByNameCaseInsensitive = R.sortBy(R.compose(R.toLower, R.prop('name')));
	var alice = {
	  name: 'ALICE',
	  age: 101
	};
	var bob = {
	  name: 'Bob',
	  age: -10
	};
	var clara = {
	  name: 'clara',
	  age: 314.159
	};
	var people = [clara, bob, alice];
	sortByNameCaseInsensitive(people); //=> [alice, bob, clara]
	```   
	
#####	sortWith
	定义：依据比较函数列表对输入列表进行排序。
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
	  R.descend(R.prop('age')),
	  R.ascend(R.prop('name'))
	]);
	ageNameSort(people); //=> [alice, clara, bob]
	```   
	
#####	symmetricDifference
	定义：求对称差集。所有不属于两列表交集元素的集合，其元素在且仅在给定列表中的一个里面出现。
	```   
	R.symmetricDifference([1,2,3,4], [7,6,5,4,3]); //=> [1,2,7,6,5]
	```   
	
#####	symmetricDifferenceWith
	定义：求对称差集。所有不属于两列表交集元素的集合。交集的元素由条件函数的返回值决定。
	```   
	var eqA = R.eqBy(R.prop('a'));
	var l1 = [{a: 1}, {a: 2}, {a: 3}, {a: 4}];
	var l2 = [{a: 3}, {a: 4}, {a: 5}, {a: 6}];
	R.symmetricDifferenceWith(eqA, l1, l2); //=> [{a: 1}, {a: 2}, {a: 5}, {a: 6}]
	```   	
	
#####	union
	定义：集合并运算，合并两个列表为新列表（新列表中无重复元素）。
	```   
	R.union([1, 2, 3], [2, 3, 4]); //=> [1, 2, 3, 4]
	```   	
	
#####	unionWith
	定义：集合并运算，合并两个列表为新列表（新列表中无重复元素）。由 predicate 的返回值决定两元素是否重复。
	```   
	var l1 = [{a: 1}, {a: 2}];
	var l2 = [{a: 1}, {a: 4}];
	R.unionWith(R.eqBy(R.prop('a')), l1, l2); //=> [{a: 1}, {a: 2}, {a: 4}]
	```   	
	
####	Type
	
#####	is
	定义：检测一个对象（val）是否是给定构造函数的实例。该函数会依次检测其原型链，如果存在的话。
	```   
	R.is(Object, {}); // true
	```   
	
#####	isNil
	定义：检测输入值是否为 null 或 undefined 。
	```   
	R.isNil(null); // true
	```   
	
#####	propIs
	定义：判断指定对象的属性是否为给定的数据类型，是则返回 true ；否则返回 false 。
	```   
	R.propIs(Number, 'x', {x: 1, y: 2}); // true
	```   
	
#####	type
	定义：用一个单词来描述输入值的（原生）类型，返回诸如 'Object'、'Number'、'Array'、'Null' 之类的结果。不区分用户自定义的类型，统一返回 'Object'。
	```   
	R.type([]); // Array
	```   
	
	
	










