---
title: JavaScript数据结构与算法
date: 2017-11-26 20:33:32
tags: '数据结构和算法'
categories: '数据结构和算法'
copyright: true
---
#	数组及矩阵（二维数组）和多维数组（a, b 均为数组）
最好使用数组存储一系列同一种数据类型的值（与其它语言保持一致）。
数组的优点：可以直接访问数组的某一项（相对于链表）。
数组的缺点：（在大多数语言中）数组的大小是固定的，从数组的起点或中间插入或移除项的成本很高，因为需要移动元素（尽管我们已经学过的JavaScript的 Array 类方法可以帮我们做这些事，但背后的情况同样是这样）。

##	改变原数组的方法
###	模拟栈数据结构
a.pop()  删除数组的最后一个元素并返回删除的元素。
a.push(item1, item2, ..., itemX)  向数组的末尾添加一个或更多元素，并返回新的长度。

###	模拟队列数据结构
a.shift()  删除并返回数组的第一个元素。
a.unshift(item1,item2, ..., itemX)  可向数组的开头添加一个或更多元素，并返回新的长度。

###	排序相关方法
a.sort(sortfunction)  方法用于对数组的元素进行排序。
a.reverse()  用于颠倒数组中元素的顺序。

###	其它
a.splice(index,howmany,item1,.....,itemX)  用于插入、删除或替换数组的元素。
	参数说明：1.规定从何处添加/删除元素。2.删除多少元素。3.插入的项目。
	这里返回类型为数组!!!
a.copyWithin(target, start, end)  从数组的指定位置拷贝元素到数组的另一个指定位置中。
	参数说明：1.复制到指定目标索引位置。2,3为复制的起始和结束位置。
a.fill(value, start, end)  将一个固定值替换数组的元素。
<p style="color: red;">PS：删除元素的方法，返回被删除的元素。
	添加元素的方法，返回数组长度。</p>
	
##	不操作原数组方法
###	返回Boolean值的方法
a.every(function(currentValue,index,arr), thisValue)  检测数值元素的每个元素是否都符合条件。
a.some(function(currentValue,index,arr),thisValue)  用于检测数组中的元素是否满足指定条件（函数提供）。
a.includes(searchElement, fromIndex) 判断该数组是否存在该数组中。

###	返回新数组的方法
a.concat(b)  连接两个或更多的数组，并返回连接后的数组。
a.filter(function(currentValue,index,arr), thisValue)  创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
a.slice(start, end)  可从已有的数组中返回选定的元素。
a.map(function(currentValue,index,arr), thisValue)   返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。

###	返回数组中的某项索引值
a.findIndex(function(currentValue, index, arr), thisValue)  返回符合传入测试（函数）条件的数组元素索引。
a.indexOf(item,start)  返回某个指定的字符串值在字符串中首次出现的位置。
a.lastIndexOf(item,start)   返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。

###	返回数组中的某项值
a.find(function(currentValue, index, arr),thisValue)  返回传入一个测试条件（函数）符合条件的数组第一个元素。

###	输出为字符串的方法
a.join(separator)  用于把数组中的所有元素通过分隔符转换一个字符串。
a.toString()  返回已逗号分割元素的字符串。

###	其它对数组进行操作
a.forEach(function(currentValue, index, arr), thisValue)  （not return a new array! & no return value!）对数组的每个元素执行一次提供的函数。
a.reduce(function(total, currentValue, currentIndex, arr), initialValue)  将数组元素计算为一个值（从左到右）。
a.reduceRight(function(total, currentValue, currentIndex, arr), initialValue)  将数组元素计算为一个值（从右到左）。
[Array方法参考手册](http://www.runoob.com/jsref/jsref-obj-array.html)

#	栈
栈，一种具有特殊行为的数组，具有后进先出（LIFO）原则的有序集合。
	（数组头部）栈底[]栈顶（数组尾部）
##	栈应该具有的方法
push(element(s)) ：添加一个（或几个）新元素到栈顶。
pop() ：移除栈顶的元素，同时返回被移除的元素。
peek() ：返回栈顶的元素，不对栈做任何修改（这个方法不会移除栈顶的元素，仅仅返回它）。
isEmpty() ：如果栈里没有任何元素就返回 true ，否则返回 false 。
clear() ：移除栈里的所有元素。
size() ：返回栈里的元素个数。这个方法和数组的 length 属性很类似。

##	构建栈及相应的方法
	```
	function Stack() {
	
		// 使用数组来模拟栈
		let items = [];
		
		// push方法
		this.push = function(element){
			items.push(element);
		};
		
		// pop方法
		this.pop = function(){
			return items.pop();
		};
		
		// peek方法
		this.peek = function(){
			return items[items.length-1];
			// return items.slice().pop();
		};
		
		// isEmpty方法
		this.isEmpty = function(){
			return items.length === 0;
		};	
		
		// size方法
		this.size = function(){
			return items.length;
		};
		
		// clear方法
		this.clear = function(){
			items = [];
		};
		
		// print调试方法
		this.print = function(){
			console.log(items.toString());
		};
	}
	
	// 测试代码
	let stack1 = new Stack();
	stack1.print(); // ''
	stack1.push(1);
	stack1.print(); // '1'
	stack1.clear();  
	stack1.print(); // ''
	```

##	栈的应用
###	10进制转2进制
	```
	function divideBy2(decNumber){
		let remStack = new Stack(),
			rem,
			binaryString = '';
			
		while (decNumber > 0){ //{1}
			rem = Math.floor(decNumber % 2); //{2}
			remStack.push(rem); //{3}
			decNumber = Math.floor(decNumber / 2); //{4}
		}
		
		while (!remStack.isEmpty()){ //{5}
			binaryString += remStack.pop().toString();
		}
		
		return binaryString;
	}
	```
	
#	队列
队列是遵循FIFO（First In First Out，先进先出，也称为先来先服务）原则的一组有序的项。

##	队列需要的方法
enqueue(element(s)) ：向队列尾部添加一个（或多个）新的项。
dequeue() ：移除队列的第一（即排在队列最前面的）项，并返回被移除的元素。
front() ：返回队列中第一个元素——最先被添加，也将是最先被移除的元素。队列不做任何变动（不移除元素，只返回元素信息——与 Stack 类的 peek 方法非常类似）。
isEmpty() ：如果队列中不包含任何元素，返回 true ，否则返回 false 。
size() ：返回队列包含的元素个数，与数组的 length 属性类似。

##	构建队列
	```
	function Queue() {
		
		// 保存数据
		let items = [];
		
		// enqueue方法
		this.enqueue = function(element){
			items.push(element);
		};
	
		// dequeue 方法
		this.dequeue = function(){
			return items.shift();
		};
	
		// front 方法
		this.front = function(){
			return items[0];
		};
	
		// isEmpty 方法
		this.isEmpty = function(){
			return items.length === 0;
		};
		
		// size方法
		this.size = function(){
			return items.length;
		};
		
		// print 调试方法
		this.print = function(){
			console.log(items.toString());
		};
	}
	```

##	优先队列
###	可以设置优先级的队列
	```
	function PriorityQueue() {
		let items = [];
		
		function QueueElement (element, priority){ // {1}
			this.element = element;
			this.priority = priority;
		}
		
		this.enqueue = function(element, priority){
			var queueElement = new QueueElement(element, priority);
			
			if (this.isEmpty()){
				items.push(queueElement); // {2}
			} else {
				var added = false;
				
				for (var i=0; i<items.length; i++){
					if (queueElement.priority < items[i].priority){
						items.splice(i,0,queueElement); // {3}
						added = true;
						break; // {4}
					}
				}
				
				if (!added){ //{5}
					items.push(queueElement);
				}
			}
		};
		//其他方法和默认的Queue实现相同
	}
	
	// 测试代码var priorityQueue = new PriorityQueue();
	priorityQueue.enqueue("John", 2);
	priorityQueue.enqueue("Jack", 1);
	priorityQueue.enqueue("Camila", 1);
	priorityQueue.print(); // Jack, Camila, John
	```

##	循环队列--击鼓传花
	```
	function hotPotato (nameList, num){
		var queue = new Queue(); // {1}
		
		for (var i=0; i<nameList.length; i++){
			queue.enqueue(nameList[i]); // {2}
		}
		
		var eliminated = '';
		
		while (queue.size() > 1){
			for (var i=0; i<num; i++){
				queue.enqueue(queue.dequeue()); // {3}
			}
		
			eliminated = queue.dequeue();// {4}
			console.log(eliminated + '在击鼓传花游戏中被淘汰。');
		}
		
		return queue.dequeue();// {5}
	}
	var names = ['John','Jack','Camila','Ingrid','Carl'];
	var winner = hotPotato(names, 7);
	console.log('胜利者：' + winner);
	```

#	链表
链表存储有序的元素集合，但不同于数组，链表中的元素在内存中并不是连续放置的。
每个元素由一个存储元素本身的节点和一个指向下一个元素的引用（也称指针或链接）组成。
然而，链表需要使用指针，因此实现链表时需要额外注意。
数组的另一个细节是可以直接访问任何位置的任何元素，而要想访问链表中间的一个元素，需要从起点（表头）开始迭代列表直到找到所需的元素。
链表的优点：相对于传统的数组，链表的一个好处在于，添加或移除元素的时候不需要移动其他元素。
链表的缺点：数组的另一个细节是可以直接访问任何位置的任何元素，而要想访问链表中间的一个元素，需要从起点（表头）开始迭代列表直到找到所需的元素。

##	创建一个链表
	```
	function LinkedList() {
		
		// 表示要加入列表的项
		let Node = function(element){ // {1}
			this.element = element; // 填加到列表的值
			this.next = null; // 列表中下一个节点项的引用
		};
		
		let length = 0; // {2}
		
		// 用于存在第一个节点的引用
		let head = null; 
		
		// 取得链表首元素
		this.getHead = function(){
			return head;
		};
		
		// 向列表尾部添加一个新的项
		this.append = function(element){
			let node = new Node(element);
			current;
			
			if (head === null) { // 列表中第一节点
				head = node;
			} else {
				current = head; 
				
				// 循环列表，直到找到最后一项
				wihle(current.next){
					current = current.next;
				}
				
				// 找到最后一项，将其next赋值为node，建立链接
				current.next = node;
			}
			
			length++; // 链表长度加1
		};
		
		// 向列表的特定位置插入一个新的项
		this.insert = function(position, element){
		
			//检查越界值
			if (position >= 0 && position <= length){ //{1}
				var node = new Node(element),
					current = head,
					previous,
					index = 0;
					
				if (position === 0){ //在第一个位置添加
					node.next = current; //{2}
					head = node;
				} else {
				
					while (index++ < position){ //{3}
						previous = current;
						current = current.next;
					}
					
					node.next = current; //{4}
					previous.next = node; //{5}
				}
				
				length++; //更新列表的长度
				return true;
			} else {
				return false; //{6}
			}
			
		};
		
		// 从列表的特定位置移除一项
		this.removeAt = function(position){
			//检查越界值
			if (position > -1 && position < length){ // {1}
				var current = head, // {2}
				previous, // {3}
				index = 0; // {4}
				
				//移除第一项
				if (position === 0){ // {5}
					head = current.next;
				} else {
					while (index++ < position){ // {6}
						previous = current; // {7}
						current = current.next; // {8}
					}
				
					//将previous与current的下一项链接起来：跳过current，从而移除它
					previous.next = current.next; // {9}
				}
				
				length--; // {10}
				return current.element;
			} else {
				return null; // {11}
			}
		};
		
		// 从列表中移除一项
		this.remove = function(element){
			var index = this.indexOf(element);
			return this.removeAt(index);
		};
		
		// 返回元素在列表中的索引
		this.indexOf = function(element){
			var current = head, //{1}
				index = -1;
			while (current) { //{2}
				if (element === current.element) {
					return index; //{3}
				}
				index++; //{4}
				current = current.next; //{5}
			}
			return -1;
		};
		
		// 如果链表中不包含任何元素，返回 true ，如果链表长度大于0则返回 false
		this.isEmpty = function() {
			return length === 0;
		};
		
		// 返回链表包含的元素个数。与数组的 length 属性类似
		this.size = function() {
			return length;
		};
		
		// 由于列表项使用了 Node 类，就需要重写继承自JavaScript对象默认的toString 方法，让其只输出元素的值
		this.toString = function(){
			var current = head, //{1}
			string = ''; //{2}
			
			while (current) { //{3}
				string = current.element; //{4}
				current = current.next; //{5}
			}
			
			return string; //{6}
		};
		
		// 调试方法
		this.print = function(){};
	}
	```

##	双向链表
双向链表的特点：双向链表和普通链表的区别在于，在链表中，一个节点只有链向下一个节点的链接，而在双向链表中，链接是双向的：一个链向下一个元素，另一个链向前一个元素。	
当我们访问链表项时，在单向链表中，如果迭代列表时错过了要找的元素，就需要回到列表起点，重新开始迭代。这是双向链表的一个优点。
	```
	function  DoublyLinkedList(){
		var Node = function(element){
			this.element = element;
			this.next = null;
			this.prev = null; //新增的
		};
		
		var length = 0;
		var head = null;
		var tail = null; //新增的
		
		this.insert = function(position, element){
			//检查越界值
			if (position >= 0 && position <= length){
				var node = new Node(element),
					current = head,
					previous,
					index = 0;
				if (position === 0){ //在第一个位置添加
					if (!head){ //新增的 {1}
						head = node;
						tail = node;
					} else {
						node.next = current;
						current.prev = node; //新增的 {2}
						head = node;
					}
				} else if (position === length) { //最后一项 //新增的
					current = tail; // {3}
					current.next = node;
					node.prev = current;
					tail = node;
				} else {
					while (index++ < position){ //{4}
					previous = current;
					current = current.next;
				}
				node.next = current; //{5}
				previous.next = node;
				current.prev = node; //新增的
				node.prev = previous; //新增的
				}
				length++; //更新列表的长度
				return true;
			} else {
				return false;
			}
		};
		
		this.removeAt = function(position){
			//检查越界值
			if (position > -1 && position < length){
				var current = head,
					previous,
					index = 0;
				//移除第一项
				if (position === 0){
					head = current.next; // {1}
					//如果只有一项，更新tail //新增的
					if (length === 1){ // {2}
						tail = null;
					} else {
						head.prev = null; // {3}
					}
				} else if (position === length-1){ //最后一项 //新增的
					current = tail; // {4}
					tail = current.prev;
					tail.next = null;
				} else {
					while (index++ < position){ // {5}
						previous = current;
						current = current.next;
					}
					//将previous与current的下一项链接起来——跳过current
					previous.next = current.next; // {6}
					current.next.prev = previous; //新增的
				}
				length--;
				return current.element;
			} else {
				return null;
			}
		};
	}
	```

##	循环链表
单向循环链表的特点：循环链表和链表之间唯一的区别在于，最后一个元素指向下一个元素的指针（ tail.next ）不是引用 null ，而是指向第一个元素（ head ）。
双向循环链表有指向 head 元素的 tail.next ，和指向 tail 元素的 head.prev 。

#	集合（es6的Set）
集合的定义：集合是由一组无序且唯一（即不能重复）的项组成的。
集合的操作：集合也有并集、交集、差集等基本操作。

##	创建一个集合
	```
	function Set(){
		let items = {}; // js对象不允许两个不同的属性，保证集合元素的唯一性
	
		this.has = function(value){
			// 区别 in可以查找原型链而 hasOwnProperty方法只会查找自身
			// return value in items;
			return items.hasOwnProperty(value);
		};
		
		this.add = function(value){
			if (!this.has(value)) {
				items[value] = value;
				return ture;
			}
			return false;
		};
	
		this.remove = function(value){
			if (this.has(value)) {
				delete items[value];
				return ture;
			}
			return false;
		};
		
		this.clear = function(){
			items = {};
		};
		
		this.size = function(){
			return Object.keys(items).length;
		};
	
		this.values = function(){
			return Object.keys(items);
		};
	}
	```
##	集合操作
并集：对于给定的两个集合，返回一个包含两个集合中所有元素的新集合。
交集：对于给定的两个集合，返回一个包含两个集合中共有元素的新集合。
差集：对于给定的两个集合，返回一个包含所有存在于第一个集合且不存在于第二个集合的元素的新集合。
子集：验证一个给定集合是否是另一集合的子集。
一下假设A、B为两个集合。

###	并集
运算公式：A∪B = { x | x ∈ A∨x ∈ B }
为Set类的union方法
	```
	// 思路先将原集合和新集合循环添加到并集合中
	this.union = function(otherSet){
		let unionSet = new Set();
		let values = this.values();
		for (let i = 0; i < values.length; i++) {
			unionSet.add(values[i]);
		}
		values = otherSet.values();
		for (let i = 0; i < values.length; i++) {
			unionSet.add(values[i]);
		}
		return unionSet;
	};
	```
	
###	交集
运算公式：A∩B = { x | x ∈ A∧x ∈ B }
	```
	this.intersection = function(otherSet){
		let intersectionSet = new Set();
		let values = this.values();
		
		for (let i = 0; i < values.length; i++) {
			if (otherSet.has(values[i])) {
				intersectionSet.add(values[i]);
			}
		}
		return intersectionSet;
	};
	```

###	差集
计算公式：AB = { x | x ∈ A ∧ x   B }
	```
	this.difference = function(otherSet){
		let differenceSet = new Set();
		let values = this.values();
		
		for (let i = 0; i < values.length; i++) {
			if (!otherSet.has(values[i])) {
				differenceSet.add(values[i]);
			}
		}
		return differenceSet;
	};
	```
	
###	子集
运算公式：∀x { x ∈ A → x ∈ B }
	```
	this.subset = function(otherSet){
		if (this.size() > otherSet.size()) {
			return false;
		} else {
			values = this.values();
			for (let i = 0; i < values.length; i++) {
				if (!otherSet.has(values[i])) {
					return false;
				}
			}
			return true;
		}
	};
	```

#	字典（映射es6的Map）
字典和散列表是用来存储唯一值（不重复的值）的数据结构。	
两者都是[键，值]的形式来存储数据	
	
##	创建一个字典
	```
	function  Dictionary(){
		let items = {};
		
		this.has = function(key){
			return key in items;
		};
		
		this.set = function(key, value){
			items[key] = value;
		};
		
		this.remove = function(key){
			if (this.has(key)) {
				delete items[key];
				return true;
			}
			return false;
		};
		
		this.get = function(key){
			return this.has(key) ? items[key] : undefined;
		};
		
		this.values = function(){
			let values = {};
			for (let k in items) {
				if (this.has(k)) {
					values.push(items[k]);
				}
			}
			return values;
		};
		
		this.clear = function(){
			items = {};
		};
		
		this.getItems = function(){
			return items;
		};
	}
	```

#	散列表
散列算法的作用是尽可能快地在数据结构中找到一个值。 
使用散列函数，就知道值的具体位置，因此能够快速检索到该值。
散列函数的作用是给定一个键值，然后返回值在表中的地址。

##	创建一个散列表
	```
	function HashTable(){
		var table = [];
		// 私有方法-散列函数
		let loseloseHashCode = function(key){
			let hash = 0;
			for (let i = 0; i < key.length; i++) {
				hash += key.charCodeAt(i);
			}
			return hash % 37;
		};
		
		this.put = function(key, value){
			let position = loseloseHashCode(key);
			console.log(position + ' - ' + key); //{6}
			table[position] = value;
		};
	
		this.get = function(key){
			return table[loseloseHashCode(key)];
		};
		
		this.remove = function(key){
			table[loseloseHashCode(key)] = undefined;
		};
	}
	```
##	处理散列表的冲突（同名的地址覆盖）
可以通过分离链接、线性探查和双散列法来解决冲突。

###	分离链接法
定义：包括为散列表的每一个位置创建一个链表并将元素存储在里面。
为了实现一个使用了分离链接的 HashTable 实例，我们需要一个新的辅助类来表示将要加入LinkedList 实例的元素。
	```
	let ValuePair = function(key, value){
		this.key = key;
		this.value = value;
		this.toString = function() {
			return '[' + this.key + ' - ' + this.value + ']';
		}
	};
	
	this.put = function(key, value){
		let position = loseloseHashCode(key);
		if (table[position] == undefined) {
			table[position] = new LinkedList();
		}
		table[position].append(new ValuePair(key, value));
	};
	
	this.get = function(key){
		let position = loseloseHashCode(key);
		
		if (table[position] !== undefined) {
			// 遍历链表来寻找键/值
			let current = table[position].getHead();
			while (current.next) {
				if (current.elment.key === key) {
					return current.element.value;
				}
				current = current.next;
			}
			
			// 检查元素在链表第一个或最后一个节点的情况
			if (current.element.key === key){ //{9}
				return current.element.value;
			}
		}
		return undefined;
	};
	
	this.remove = function(key){
		var position = loseloseHashCode(key);
		if (table[position] !== undefined){
			var current = table[position].getHead();
			while(current.next){
				if (current.element.key === key){ //{11}
					table[position].remove(current.element); //{12}
					if (table[position].isEmpty()){ //{13}
						table[position] = undefined; //{14}
					}
					return true; //{15}
				}
				current = current.next;
			}
			// 检查是否为第一个或最后一个元素
			if (current.element.key === key){ //{16}
				table[position].remove(current.element);
				if (table[position].isEmpty()){
					table[position] = undefined;
				}
				return true;
			}
		}
		return false; //{17}
		};
	```

###	线性探查
当想向表中某个位置加入一个新元素的时候，如果索引为index的位置已经被占据了，就尝试index+1的位置。如果index+1的位置也被占据了，就尝试index+2的位置，以此类推。
	```
	this.put = function(key, value){
		var position = loseloseHashCode(key); // {1}
		if (table[position] == undefined) { // {2}
			table[position] = new ValuePair(key, value); // {3}
		} else {
			var index = ++position; // {4}
			while (table[index] != undefined){ // {5}
				index++; // {6}
			}
			table[index] = new ValuePair(key, value); // {7}
		}
	};
	
	this.get = function(key) {
		var position = loseloseHashCode(key);
		if (table[position] !== undefined){ //{8}
			if (table[position].key === key) { //{9}
				return table[position].value; //{10}
			} else {
				var index = ++position;
				while (table[index] === undefined || table[index].key !== key){ //{11}
					index++;
				}
				if (table[index].key === key) { //{12}
					return table[index].value; //{13}
				}
			}
		}
		return undefined; //{14}
	};
	```
	
###	创建更好的散列函数
比较好的散列函数，不会产生太多的冲突。
	```
	var djb2HashCode = function (key) {
		var hash = 5381; //{1}
		for (var i = 0; i < key.length; i++) { //{2}
			hash = hash * 33 + key.charCodeAt(i); //{3}
		}
		return hash % 1013; //{4}
	};
	```
	
#	树	
树，它对于存储需要快速查找的数据非常有用。	
树是一种分层数据的抽象模型。

##	树的相关术语
位于树顶部的节点叫作根节点。
树中的每个元素都叫作节点，节点分为内部节点和外部节点。
至少有一个子节点的节点称为内部节点。
没有子元素的节点称为外部节点或叶节点。
有关树的另一个术语是子树。子树由节点和它的后代构成。
树的高度取决于所有节点深度的最大值。

##	二叉树和二叉搜索树（BST）
二叉树中的节点最多只能有两个子节点：一个是左侧子节点，另一个是右侧子节点。
这些定义有助于我们写出更高效的向树中插入、查找和删除节点的算法。
二叉搜索树（BST）是二叉树的一种，但是它只允许你在左侧节点存储（比父节点）小的值，在右侧节点存储（比父节点）大（或者等于）的值。
遍历一棵树是指访问树的每个节点并对它们进行某种操作的过程。
访问树的所有节点有三种方式：中序、先序和后序。
中序遍历 -- 是以从最小到最大的顺序访问所有节点（第二层到第一层左中右）。
先序遍历 -- 是以优先于后代节点的顺序访问每个节点的（第一层到第二层中左右）。
后序遍历 -- 是先访问节点的后代节点，再访问节点本身（第二层到第一层左右中）。
中序遍历的一种应用就是对树进行排序操作。
先序遍历的一种应用是打印一个结构化的文档。
后序遍历的一种应用是计算一个目录和它的子目录中所有文件所占空间的大小。
	```
	function BinarySearchTree() {
		let Node = function(key){
			this.key = key;
			this.left = null;
			this.right = null;
		};
		
		// 根节点
		let root = null;
		
		// 插入节点，小的节点插入left，大的节点插入right
		let insertNode = function(node, newNode){
			if (newNode.key < node.key) {
				if (node.left === null) {
					node.left = newNode;
				} else {
					insertNode(node.left, newNode);
				}
			} else {
				if (node.right === null) {
					node.right = newNode;
				} else {
					insertNode(node.right, newNode);
				}
			}
		};
		
		// 向树中插入一个新的键
		this.insert = function (key){
			var newNode = new Node(key); //{1}
			if (root === null){ //{2}
				root = newNode;
			} else {
				// 递归
				insertNode(root,newNode); //{3}
			}
		};
		
		// 私有方法-中序遍历
		let inOrderTraverseNode = function(node, callback){
			if (node !== null) {
				inOrderTraverseNode(node.left, callback);
				callback(node.key);
				inOrderTraverseNode(node.right, callback);
			}
		};
		
		// 中序遍历（左中右）小到大
		this.inOrderTraverse = function(callback){
			inOrderTraverseNode(root, callback);
		
		};
		
		// 私有方法-先序遍历
		var preOrderTraverseNode = function (node, callback) {
			if (node !== null) {
				callback(node.key); //{1}
				preOrderTraverseNode(node.left, callback); //{2}
				preOrderTraverseNode(node.right, callback); //{3}
			}
		};
		
		// 先序遍历（中左右）
		this.preOrderTraverse = function(callback){
			preOrderTraverseNode(root, callback);
		};
		
		// 私有方法-后序遍历
		var postOrderTraverseNode = function (node, callback) {
			if (node !== null) {
				postOrderTraverseNode(node.left, callback); //{1}
				postOrderTraverseNode(node.right, callback); //{2}
				callback(node.key); //{3}
			}
		};
		
		// 后序遍历（左右中）
		this.postOrderTraverse = function(callback){
			postOrderTraverseNode(root, callback);
		};

		// 私有方法-最小值
		let minNode = function(node){
			if (node) {
				while (node && node.left !== null) {
					node = node.left
				}
				return node.key;
			}
			return null;
		};
		
		// 搜索最小值
		this.min = function(){
			return minNode(node);
		};
		
		// 私有方法-最大值
		var maxNode = function (node) {
			if (node){
				while (node && node.right !== null) { //{5}
					node = node.right;
				}
				return node.key;
			}
			return null;
		}
		
		// 搜索最大值
		this.max = function() {
			return maxNode(root);
		};
		
		// 私有方法-搜索节点
		var searchNode = function(node, key){
			if (node === null){ //{2}
				return false;
			}
			if (key < node.key){ //{3}
				return searchNode(node.left, key); //{4}
			} else if (key > node.key){ //{5}
				return searchNode(node.right, key); //{6}
			} else {
				return true; //{7}
			}
		};
		
		// 搜索节点
		this.search = function(key){
			return searchNode(root, key); //{1}
		};

		// 私有方法-移除节点
		var removeNode = function(node, key){
			if (node === null){ //{2}
				return null;
			}
			if (key < node.key){ //{3}
				node.left = removeNode(node.left, key); //{4}
				return node; //{5}
			} else if (key > node.key){ //{6}
				node.right = removeNode(node.right, key); //{7}
				return node; //{8}
			} else { //键等于node.key
				//第一种情况——一个叶节点
				if (node.left === null && node.right === null){ //{9}
					node = null; //{10}
					return node; //{11}
				}
				//第二种情况——一个只有一个子节点的节点
				if (node.left === null){ //{12}
					node = node.right; //{13}
					return node; //{14}
				} else if (node.right === null){ //{15}
					node = node.left; //{16}
					return node; //{17}
				}
				//第三种情况——一个有两个子节点的节点
				var aux = findMinNode(node.right); //{18}
				node.key = aux.key; //{19}
				node.right = removeNode(node.right, aux.key); //{20}
				return node; //{21}
			}
		};
		
		// 移除节点
		this.remove = function(key){
			root = removeNode(root, key); //{1}
		};	
	}
	```
##	更多关于二叉树的知识
BST存在一个问题：取决于你添加的节点数，树的一条边可能会非常深
	有一种树叫作阿德尔森-维尔斯和兰迪斯树（AVL树）。AVL树是一种自平衡二叉搜索树，意思是任何一个节点左右两侧子树的高度之差最多为1。
相关知识：红黑树、堆积树

#	图
##	图的相关术语
图是网络结构的抽象模型。图是一组由边连接的节点（或顶点）。
任何二元关系都可以用图来表示。
一个图G = (V, E)由以下元素组成。
	V：一组顶点
	E：一组边，连接V中的顶点
相邻顶点：由一条边连接在一起的顶点称为相邻顶点。
顶点的度：一个顶点的度是其相邻顶点的数量。
路径：路径是顶点v 1 , v 2 ,…,v k 的一个连续序列，其中v i 和v i+1 是相邻的。
图是无环：如果图中不存在环，则称该图是无环的。
图是连通：如果图中每两个顶点间都存在路径，则该图是连通的。
有向图和无向图：图可以是无向的（边没有方向）或是有向的（有向图）。
图是强连通：如果图中每两个顶点间在双向上都存在路径，则该图是强连通的。
图是加权：图还可以是未加权的或是加权的。
##	图的表示
###	邻接矩阵
每个节点都和一个整数相关联，该整数将作为数组的索引。
我们用一个二维数组来表示顶点之间的连接。
如果索引为i的节点和索引为j的节点相邻，则array[i][j]=== 1，否则array[i][j] === 0
存在的问题：不是强连通的图（稀疏图）如果用邻接矩阵来表示，则矩阵中将会有很多0，这意味着我们浪费了计算机存储空间来表示根本不存在的边。
###	邻接表
邻接表由图中每个顶点的相邻顶点列表所组成。
存在好几种方式来表示这种数据结构。
我们可以用列表（数组）、链表，甚至是散列表或是字典来表示相邻顶点列表。
###	关联矩阵
在关联矩阵中，矩阵的行表示顶点，列表示边。
我们使用二维数组来表示两者之间的连通性，如果顶点v是边e的入射点，则array[v][e] === 1；否则，array[v][e] === 0。
	使用场景：关联矩阵通常用于边的数量比顶点多的情况下，以节省空间和内存。
##	创建图类
	```
	function Graph(){
		let vertices = [];
		let adjList = new Dictionary();
		
		// 添加顶点，参数为一个顶点
		this.addVertex = function(v){
			vertices.push(v);
			adjList.set(v, []);
		};
		
		// 添加顶点之间的边，参数为两个顶点
		this.addEdge = function(v, w){
			adjList.get(v).push(w);
			adjList.get(w).push(v);
		};
	}
	```
字典将会使用顶点的名字作为键，邻接顶点列表作为值。 
vertices数组和 adjList 字典两者都是我们 Graph 类的私有属性。
##	图的遍历
有两种算法可以对图进行遍历：广度优先搜索（Breadth-First Search，BFS）和深度优先搜索（Depth-First Search，DFS）。
图遍历可以用来寻找特定的顶点或寻找两个顶点之间的路径，检查图是否连通，检查图是否含有环等。
深度优先搜索  栈  通过将顶点存入栈中，顶点是沿着路径被探索的，存在新的相邻点就去访问
广度优先搜索  队列  通过将顶点存入队列中，最先入队列的顶点先被探索
###	广度优先搜索
广度优先搜索算法会从指定的第一个顶点开始遍历图，先访问其所有的相邻点，就像一次访问图的一层。先宽后深地访问顶点。
	```
	var initializeColor = function(){
		var color = [];
		for (var i=0; i<vertices.length; i++){
			color[vertices[i]] = 'white'; //{1}
		}
		return color;
	};
	this.bfs = function(v, callback){
		var color = initializeColor(), //{2}
			queue = new Queue(); //{3}
		queue.enqueue(v); //{4}
		while (!queue.isEmpty()){ //{5}
			var u = queue.dequeue(), //{6}
				neighbors = adjList.get(u); //{7}
			color[u] = 'grey'; // {8}
			for (var i=0; i<neighbors.length; i++){ // {9}
				var w = neighbors[i]; // {10}
				if (color[w] === 'white'){ // {11}
					color[w] = 'grey'; // {12}
					queue.enqueue(w); // {13}
				}
			}
			color[u] = 'black'; // {14}
			if (callback) { // {15}
				callback(u);
			}
		}
	};
	```
1.使用BFS寻找最短路径
	```
	this.BFS = function(v){
		var color = initializeColor(),
			queue = new Queue(),
			d = [], //{1}
			pred = []; //{2}
			queue.enqueue(v);
		for (var i=0; i<vertices.length; i++){ //{3}
			d[vertices[i]] = 0; //{4}
			pred[vertices[i]] = null; //{5}
		}
		while (!queue.isEmpty()){
			var u = queue.dequeue(),
				neighbors = adjList.get(u);
				color[u] = 'grey';
			for (i=0; i<neighbors.length; i++){
				var w = neighbors[i];
				if (color[w] === 'white'){
					color[w] = 'grey';
					d[w] = d[u] + 1; //{6}
					pred[w] = u; //{7}
					queue.enqueue(w);
				}
			}
			color[u] = 'black';
		}
		return { //{8}
		distances: d,
		predecessors: pred
		};
	};
	```
2.深入学习最短路径算法
	如果要计算加权图中的最短路径（例如，城市A和城市B之间的最短路径——GPS和Google Maps中用到的算法）	
###	深度优先搜索	
	```
	this.dfs = function(callback){
		var color = initializeColor(); //{1}
		for (var i=0; i<vertices.length; i++){ //{2}
			if (color[vertices[i]] === 'white'){ //{3}
				dfsVisit(vertices[i], color, callback); //{4}
			}
		}
	};
	var dfsVisit = function(u, color, callback){
		color[u] = 'grey'; //{5}
		if (callback) { //{6}
			callback(u);
		}
		var neighbors = adjList.get(u); //{7}
		for (var i=0; i<neighbors.length; i++){ //{8}
			var w = neighbors[i]; //{9}
			if (color[w] === 'white'){ //{10}
				dfsVisit(w, color, callback); //{11}
			}
		}
		color[u] = 'black'; //{12}
	};
	```
1.探索深度优先算法
	```
	var time = 0; //{1}
	this.DFS = function(){
		var color = initializeColor(), //{2}
			d = [],
			f = [],
			p = [];
			time = 0;
		for (var i=0; i<vertices.length; i++){ //{3}
			f[vertices[i]] = 0;
			d[vertices[i]] = 0;
			p[vertices[i]] = null;
		}
		for (i=0; i<vertices.length; i++){
			if (color[vertices[i]] === 'white'){
				DFSVisit(vertices[i], color, d, f, p);
			}
		}
		return { //{4}
				discovery: d,
				finished: f,
				predecessors: p
		};
	};
	var DFSVisit = function(u, color, d, f, p){
		console.log('discovered ' + u);
		color[u] = 'grey';
		d[u] = ++time; //{5}
		var neighbors = adjList.get(u);
		for (var i=0; i<neighbors.length; i++){
			var w = neighbors[i];
			if (color[w] === 'white'){
				p[w] = u; // {6}
				DFSVisit(w,color, d, f, p);
			}
		}
		color[u] = 'black';
		f[u] = ++time; //{7}
		console.log('explored ' + u);
	};
	```
2.拓扑排序——使用深度优先搜索

#	排序和搜索排序
##	排序算法
	冒泡排序
	选择排序
	插入排序
	归并排序
	快速排序
[算法我这里就不再多讲了，如果感兴趣可以查看我之前的系列算法文章]()
##	搜索排序	
###	顺序排序
	```
	this.sequentialSearch = function(item){
		for (var i=0; i<array.length; i++){ //{1}
			if (item === array[i]) //{2}
				return i; //{3}
			}
		}
		return -1; //{4}
	};
	```
###	二分搜索
(1) 选择数组的中间值。
(2) 如果选中值是待搜索值，那么算法执行完毕（值找到了）。
(3) 如果待搜索值比选中值要小，则返回步骤1并在选中值左边的子数组中寻找。
(4) 如果待搜索值比选中值要大，则返回步骤1并在选种值右边的子数组中寻找。
	```
	this.binarySearch = function(item){
		this.quickSort(); //{1}
		var low = 0, //{2}
			high = array.length - 1, //{3}
			mid, element;
		while (low <= high){ //{4}
			mid = Math.floor((low + high) / 2); //{5}
			element = array[mid]; //{6}
			if (element < item) { //{7}
				low = mid + 1; //{8}
			} else if (element > item) { //{9}
				high = mid - 1; //{10}
			} else {
				return mid; //{11}
			}
		}
		return -1; //{12}
	};
	```
	
#	算法补充知识	
##	递归
每款浏览器的javascript调用栈大小的限制。
	ECMAScript 6有尾调用优化（tail call optimization），使递归变快了。
	斐波那契数列
##	动态规划
动态规划（Dynamic Programming，DP）是一种将复杂问题分解成更小的子问题来解决的优化技术。
##	贪心算法	
贪心算法遵循一种近似解决问题的技术，期盼通过每个阶段的局部最优选择（当前最好的解），从而达到全局的最优（全局最优解）。它不像动态规划那样计算更大的格局。
##	大 O 表示法
时间复杂度O(n)的代码只有一层循环，而O(n 2 )的代码有双层嵌套循环。如果算法有三层遍历数组的嵌套循环，它的时间复杂度很可能就是O(n 3 )。

#	总结
##	序列数据结构
1.数组（列表）
2.栈
3.队列
&nbsp;&nbsp;普通队列
&nbsp;&nbsp;优先队列
&nbsp;&nbsp;循环队列
4.链表
&nbsp;&nbsp;单向链表
&nbsp;&nbsp;双向链表
&nbsp;&nbsp;循环单向链表
&nbsp;&nbsp;循环双向链表	
5.集合 Set

##	非序列数据结构
1.散列
2.字典 Map






