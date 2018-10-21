---
title: IndexDB探索之路
date: 2018-02-06 20:20:04
tags: 'html5'
keywords: 'IndexDB'
categories: 'html5'
copyright: true
---
[demo地址](https://fanerge.github.io/indexedDB/static/)
#	什么是 IndexDB？
IndexedDB 是一个用于在浏览器中储存较大数据结构的 Web API, 并提供索引功能以实现高性能查找. 像其他基于 SQL 的 关系型数据库管理系统 (RDBMS) 一样, IndexedDB 是一个事务型的数据库系统. 然而, 它是使用 JavaScript 对象而非列数固定的表格来储存数据的.
##	IndexDB 的特点
IndexDB 和大多数web存储解决方案相同，indexedDB也遵从同源协议(same-origin policy). 所以你只能访问同域中存储的数据，而不能访问其他域的。
IndexDB API包含异步(asynchronous) API 和同步(synchronous)API两种。  异步API适合大多数情况, 同步API必须同 WebWorkers一同使用.
##	为什么我们要使用 IndexDB？
WebStorage在浏览器中有大小限制，存放较大的数据就不能满足了。
IndexedDB 是 WebSQL 数据库的取代品, W3C组织在2010年11月18日废弃了webSql.  
IndexedDB 和WebSQL的不同点在于WebSQL 是关系型数据库（复杂）IndexedDB 是key-value型数据库（简单好使）.
#	基本概念
##	IndexedDB 数据库使用key-value键值对储存数据
key可以是二进制对象。
values 数据可以是结构非常复杂的对象，key可以是对象自身的属性。
你可以对对象的某个属性创建索引（index）以实现快速查询和列举排序。
##	IndexedDB 是事务模式的数据库
任何操作都发生在事务(transaction)中。  
IndexedDB API提供了索引(indexes), 表(tables), 指针(cursors)等等, 
但是所有这些必须是依赖于某种事务的。因此，你不能在事务外执行命令或者打开指针。
当用户在不同的标签页同时打开Web应用的两个实例时，这个事务模型就会非常有用。
如果没有事务操作的支持，这两个实例就会互相影响对方的修改。
##	IndexedDB API 基本上是异步的
IndexedDB的API不通过return语句返回数据，而是需要你提供一个回调函数来接受数据。
执行API时，你不以同步（synchronous）方式对数据库进行“存储”和“读取”操作，而是向数据库发送一个操作“请求”。
当操作完成时，数据库会以DOM事件的方式通知你，同时事件的类型会告诉你这个操作是否成功完成。
类似于XMLHttpRequest。
##	IndexedDB数据库“请求”无处不在
数据库“请求”负责接受成功或失败的DOM事件。
每一个“请求”都包含onsuccess和onerror事件属性，同时你还对“事件”调用addEventListener()和removeEventListener()。
“请求”还包括readyState，result和errorCode属性，用来表示“请求”的状态。
result属性尤其神奇，他可以根据“请求”生成的方式变成不同的东西，例如：IDBCursor实例、刚插入数据库的数值对应的键值（key）等。
##	IndexedDB在结果准备好之后通过DOM事件通知用户
DOM事件总是有一个类型（type）属性（在IndexedDB中，该属性通常设置为success或error）。
DOM事件还有一个目标（target）属性，用来告诉事件是被谁触发的。通常情况下，目标（target）属性是数据库操作生成的IDBRequest。
成功（success）事件不弹出提示并且不能撤销，错误（error）事件会弹出提示且可以撤销。
这一点是非常重要的，因为除非错误事件被撤销，否则他们会终止所在的任何事务。
##	IndexedDB是面向对象的
indexedDB不是用二维表来表示集合的关系型数据库。这一点非常重要，将影响你设计和建立你的应用程序。​​​​
传统的关系型数据库，你需要用到二维表来存储数据集合（每一行代表一个数据，每一列代表一个属性），indexedDB有所不同，它要求你为一种数据创建一个对象存储(object Store)，只要这种数据一个JavaScript对象即可。
每个对象存储都有一个索引(index)集合以方便查询和迭代遍历。
##	indexedDB不使用结构化查询语言（SQL）
它通过索引(index)所产生的指针(cursor)来完成查询操作，从而使你可以迭代遍历到结果集合。
##	IndexedDB遵循同源（same-origin）策略
“源”指脚本所在文档URL的域名、应用层协议和端口。每一个“源”都有与其相关联的数据库。
在同一个“源”内的所有数据库都有唯一、可区别的名称。
#	使用 IndexedDB
##	检测浏览器支持情况
```
if (window.indexedDB) {
	// todo
} else {
	alert('您的浏览器不支持indexdb')
}
```
下面我将会以demo来做常用的数据库操作说明，使用火狐浏览器做测试。
##	新建数据库/关闭数据库
indexedDB 有一个open(indexDbName[, version])，这个方法会打开某个数据库，若不存在则新建。
第一个参数为数据库名称 'demo'，第二个参数为 版本号。
db为打开数据库成功回掉 event.target.result 戴白哦数据库实例，有 close() 为关闭该数据库
```
function createDatabase(indexDbName) {
	
	// 不存在则新建，存在则打开
	let openRequest = indexedDB.open(indexDbName);
	
	openRequest.onerror = function(event) {
        console.log("Database error: " + event.target.errorCode);
    };
 
	openRequest.onsuccess = function(event) {
		console.log("Database created");
		let db = event.target.result;
		// db.close();
		console.log("this is :"+db);
	};
	
	//更改数据库，或者存储对象时候在这里处理
	openRequest.onupgradeneeded = function (e) {
		console.log(e);
	};
}
```
![新建数据库](http://p26lefllv.bkt.clouddn.com/indexedDBIns.png)
##	确定数据结构并添加数据
onupgradeneeded 唯一可以修改数据库结构的地方。
在 indexedDB 中一个数据库中可以包含多个objectStore，objectStore是一个灵活的数据结构，可以存放多种类型数据。
创建 objectStore 的方法为数据库实例的 createObjectStore(name[, options]);
删除 objectStore 的方法为数据库实例的 deleteObjectStore(name);
其中options 有两个可选key，分别是 keyPath（选择objectStore中某个指定字段作为键值）、autoIncrement（若为true，objectStore有一个key generator）
我们创建好的 objectStore 也有一些方法：
createIndex(indexName, keyPath[, objectParameters]) 该方法作用为创建一个索引来通过 indexName 搜索 objectStore 里的数据。
objectStore.add(value[, key]) 该方法作用为将数据添加到 objectStore 中。
介绍了相关的方法，我们就通过循环来向 objectStore 添加数据。
下面是具体实现代码：
```
function insertData(indexDbName){
	// 带写入的数据
	const customerData = [
	  { ssn: "444-44-4444", name: "Bill", age: 35, email: "bill@company.com" },
	  { ssn: "555-55-5555", name: "Donna", age: 32, email: "donna@home.org" }
	];
	
	// 如果在没有新建数据库时写入数据，这里只能带高版本的数据库版本才能出发 onupgradeneeded 事件
	let openRequest = indexedDB.open(indexDbName, '3.0');
	
	openRequest.onerror = function(e) {
		console.log("Database error: " + e.target.errorCode);
	};
	
	openRequest.onsuccess = function(event) {
		console.log("Database created");
	};
	
	openRequest.onupgradeneeded = function(event) {
		
		console.log("开始写入数据");
		let db = event.target.result;
		// keyPath、autoIncrement
		let objectStore = db.createObjectStore("customers", { keyPath: "ssn" });
		objectStore.createIndex("name", "name", { unique: false });
		objectStore.createIndex("email", "email", { unique: true });
		
		for (let item of customerData) {
			objectStore.add(item);
		}
		
		// 删除 objectStore
		// db.deleteObjectStore("customers");
	};
}
```
![新建数据库](http://p26lefllv.bkt.clouddn.com/indexed%20DBIns1.png)
PS： 这里有个坑，需要说明一下。
onupgradeneeded事件在下列情况下被触发：
1.数据库第一次被打开时即新建
2.打开数据库时指定的版本号高于当前被持久化的数据库版本号
##	添加数据
添加数据在 onsuccess 钩子中进行。
IndexedDB 添加数据通过事务来添加数据。
下面重点介绍下 transaction(storeNames[, mode]); 
第一个参数是事务希望跨越的对象存储空间的列表。
第二个参数事务中可以执行的访问类型。
返回一个事务对象。
事务可以接收三种不同类型的 DOM 事件： error，abort，以及 complete。
```
function addData(storeName) {
	const datas = [
	  { ssn: "666-66-6666", name: "fanerge", age: 15, email: "fanerge@company.com" },
	  { ssn: "777-77-7777", name: "sdsd", age: 22, email: "sdsd@home.org" }
	];
	
	let openRequest = indexedDB.open('demo', '3.0');
	
	openRequest.onerror = function(e) {
		console.log("Database error: " + e.target.errorCode);
	};
	
	openRequest.onsuccess = function(event) {
		let db = event.target.result;
		let transaction=db.transaction(storeName,'readwrite');
        let store=transaction.objectStore(storeName); 
		
		for(let i=0;i<datas.length;i++){
            store.add(datas[i]);
        }
	};
	
}
```
![添加数据](http://p26lefllv.bkt.clouddn.com/indexedDBAdd.png)
##	删除数据
同样删除数据也使用 transaction。
唯一区别是使用了 objectStore的 delete(key)，该方法为删除指定key的数据项。
objectStore 还有一个方法 clear()清空该 store 中的数据。
```
function del66(key){
	let openRequest = indexedDB.open('demo', '3.0');
	
	openRequest.onerror = function(e) {
		console.log("Database error: " + e.target.errorCode);
	};
	
	openRequest.onsuccess = function(event) {
		let db = event.target.result;
		let transaction = db.transaction('customers','readwrite');
		let store=transaction.objectStore('customers'); 
		let request = store.delete(key);
		
		request.onsuccess = function(event) {
			console.log('删除成功');
		};
		
		request.onerror = function(event) {
			console.log('删除失败');
		};
	};
}
```
##	查找数据
同样查找数据也使用 transaction。
唯一区别是使用了 objectStore的 get(key)，该方法为删除指定key的数据项。
```
function getDataByKey(key){
	let openRequest = indexedDB.open('demo', '3.0');
	
	openRequest.onerror = function(e) {
		console.log("Database error: " + e.target.errorCode);
	};
	
	openRequest.onsuccess = function(event) {
		let db = event.target.result;
		let transaction = db.transaction('customers','readwrite');
		let store=transaction.objectStore('customers'); 
		let request = store.get(key);
		
		request.onsuccess = function(event) {
			let item =event.target.result; 
			console.log(item); // 获得的该数据项
			console.log('查找成功');
		};
		
		request.onerror = function(event) {
			console.log('查找失败');
		};
	};
}
```
##	更新数据
可以调用object store的put方法更新数据，会自动替换键值相同的记录，达到更新目的，没有相同的则添加，以使用keyPath做键为例
```
function updateDataByKey(key){
	let openRequest = indexedDB.open('demo', '3.0');
	
	openRequest.onerror = function(e) {
		console.log("Database error: " + e.target.errorCode);
	};
	
	openRequest.onsuccess = function(event) {
		let db = event.target.result;
		let transaction = db.transaction('customers','readwrite');
		let store=transaction.objectStore('customers'); 
		let request = store.get(key);
		
		request.onsuccess = function(event) {
			let item =event.target.result; 
			 // { ssn: "666-66-6666", name: "fanerge", age: 15, email: "fanerge@company.com" }
			item.ssn = "666-66-6666"
			item.name = "yuzhenfan"
			item.age = 18
			item.email = "yzf@alipay.com"
            store.put(item); 
			console.log('更新成功');
		};
		
		request.onerror = function(event) {
			console.log('更新失败');
		};
	};
}
```
![更新数据](http://p26lefllv.bkt.clouddn.com/indexedDBupdate.png)

>	参考文档
[数据库写入时机](https://www.cnblogs.com/lovelgx/articles/6026957.html)
[html5使用indexdb的代码实例分享](http://www.php.cn/html5-tutorial-359628.html)
[wiki-数据库事务](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1)
[MDN-IndexedDB](https://developer.mozilla.org/zh-CN/docs/Glossary/IndexedDB)
[w3c-IndexDB-API](http://w3c.github.io/IndexedDB/)
[IndexDB-Guides](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API/Basic_Concepts_Behind_IndexedDB)
[HTML5本地存储——IndexedDB（一：基本使用）](https://www.cnblogs.com/dolphinX/p/3415761.html)