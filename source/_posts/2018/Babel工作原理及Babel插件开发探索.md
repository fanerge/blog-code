---
title: Babel工作原理及Babel插件开发探索
date: 2018-03-04 20:46:18
tags: 'Babel'
categories: 'js'
copyright: true
---
在掘金上看见了[面试官: 你了解过Babel吗？写过Babel插件吗? 答: 没有。卒](https://juejin.im/post/5a9315e46fb9a0633a711f25)，正巧自己对Babel工作原理和Babel插件开发也不够了解，赶紧来补一波吧。
#	基础概念
首先我们这里需要了解一些基本的概念，[这篇文章介绍的很详细](https://www.hazyzh.com/b/180211145458)，我这边只提一下。
##	Babel
Babel 是 JavaScript 编译器，更确切地说是源码到源码的编译器，通常也叫做“转换编译器（transpiler）”。 
意思是说你为 Babel 提供一些 JavaScript 代码，Babel 更改这些代码，然后返回给你新生成的代码。
##	AST
抽象语法树（abstract syntax tree或者缩写为AST），或者语法树（syntax tree），是源代码的抽象语法结构的树状表现形式，这里特指编程语言的源代码。
和抽象语法树相对的是具体语法树（concrete syntaxtree），通常称作分析树（parse tree）。
一般的，在源代码的翻译和编译过程中，语法分析器创建出分析树。一旦AST被创建出来，在后续的处理过程中，比如语义分析阶段，会添加一些信息。
##	静态分析
>	静态分析是在不需要执行代码的前提下对代码进行分析的处理过程 （执行代码的同时进行代码分析即是动态分析）。 
静态分析的目的是多种多样的， 它可用于语法检查，编译，代码高亮，代码转换，优化，压缩等等场景。

#	Babel 的三个主要处理步骤分别是： 解析（parse），转换（transform），生成（generate）
##	解析
接收代码并输出AST。这个步骤又分为两个阶段：词法分析（Lexical Analysis）和 语法分析（Syntactic Analysis）。
###	词法分析
词法分析阶段把字符串形式的代码转换成令牌（tokens）流。
你可以把令牌看作是一个扁平化的语法片段数组。
如：n*n代码经过词法分析转换成令牌
```
// n*n
[
  { type: { ... }, value: "n", start: 0, end: 1, loc: { ... } },
  { type: { ... }, value: "*", start: 2, end: 3, loc: { ... } },
  { type: { ... }, value: "n", start: 4, end: 5, loc: { ... } },
  ...
]
```
每一个type有一组属性来描述该令牌：
```
{
  type: {
    label: 'name',
    keyword: undefined,
    beforeExpr: false,
    startsExpr: true,
    rightAssociative: false,
    isLoop: false,
    isAssign: false,
    prefix: false,
    postfix: false,
    binop: null,
    updateContext: null
  },
  ...
}
```
###	语法分析
语法分析阶段会把一个令牌(tokens)流转换成 AST 的形式。 这个阶段会使用令牌中的信息把它们转换成一个 AST 的表述结构，这样更易于后续的操作。
这个过程我们可以通过[astexplorer](http://astexplorer.net/#/gist/5579c7f7b371a6f7781f974c9aa1bb6a/238c6f5a96217c0e3b403e33e9de2483da50b0fe)来查看我们代码生成的AST。
这个时候我们的AST就产生了，如下图。
![源代码到AST](http://p52glq5m1.bkt.clouddn.com/ASTdemo1.png)
PS：上图左边为我们的源代码，右边为对应生成的抽象语法树AST。
##	转换
转换步骤接收 AST 并对其进行遍历，在此过程中对节点进行添加、更新及移除等操作。 这是 Babel 或是其他编译器中最复杂的过程 同时也是插件将要介入工作的部分。
##	生成
代码生成步骤把最终（经过一系列转换之后）的 AST 转换成字符串形式的代码，同时还会创建源码映射（source maps）。
代码生成其实很简单：深度优先遍历整个 AST，然后构建可以表示转换后代码的字符串。
Babel工作原理见下图表示。
![Babel工作原理](http://p52glq5m1.bkt.clouddn.com/Babel%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.webp)
[图片来源，探索 babel 和 babel 插件是怎么工作的](https://www.hazyzh.com/b/180211145458)

#	开发一个Babel插件
##	Visitors（访问者）
当我们谈及“进入”一个节点，实际上是说我们在访问它们， 之所以使用这样的术语是因为有一个访问者模式（visitor）的概念。
访问者是一个用于 AST 遍历的跨语言的模式。 简单的说它们就是一个对象，定义了用于在一个树状结构中获取具体节点的方法。

```
const MyVisitor = {
  Identifier: {
	// 当进入Identifier节点的时候执行
	enter() { 
	  console.log("Entered");
	},
	// 当退出Identifier节点的时候执行
	exit() {
      console.log("Exited!");
    }
    
  }
};
```
PS： 许多时候我们只需要关心进入节点，就可以使用简写 Identifier() { ... } 或者 Identifier: { enter() { ... } } 。
这是一个简单的访问者，把它用于遍历中时，每当在树中遇见一个 Identifier 的时候会调用 Identifier里面的enter方法和exit方法。
##	Paths（路径）
>	我们通过 visitor可以在遍历到对应节点执行对应的函数，当需要修改对应节点的信息，我们还需要拿到对应节点的信息以及节点和所在的位置 （即和其他节点间的关系）, visitor在遍历到对应节点执行对应函数时候会给我们传入 path参数，辅助我们完成上面这些操作。注意 Path 是表示两个节点之间连接的对象,而不是当前节点，我们上面访问到了 Identifier节点，它传入的 path参数看起来是这样的：

```
{
  "parent": {
    "type": "VariableDeclarator",
    "id": {
      ...
    },
    ....
  },
  "node": {
    "type": "Identifier",
    "name": "..."
  }
}
```
这里就可以通过：path.node.name 获得当前节点的name；path.parent.id 获得父节点的id
另外path对象上还包含添加、更新、移动和删除节点有关的其他很多方法，我们可以通过文档去了解。
##	开始动手写插件了
输入的源代码为：
`yuzhenfan === wangkemei`
生成的AST
```
{
  type: "BinaryExpression",
  operator: "===",
  left: {
    type: "Identifier",
    name: "yuzhenfan"
  },
  right: {
    type: "Identifier",
    name: "yuzhenfan"
  }
}
```
省略部分属性，可以通过[http://astexplorer.net](http://astexplorer.net/#/gist/5579c7f7b371a6f7781f974c9aa1bb6a/238c6f5a96217c0e3b403e33e9de2483da50b0fe)查看全部属性。
```
const babel = require('babel-core')

// 我们的babel插件
let MyVisitor = function({ types: t }) {
  return {
    visitor: {
		
      BinaryExpression(path) {
		  
        if (path.node.operator !== "===") {
			return;
		}
		
		// 改变当前节点的left、right（插件的核心代码）
		path.node.left = t.identifier("fanerge1");
		path.node.right = t.identifier("fanerge2");
      }
	  
    }
  };
}

const code = `yuzhenfan === wangkemei;`;

let demo = babel.transform(code, {
  // 使用我们的插件
  plugins: [MyVisitor]
})

console.log(demo); // fanerge1===fanerge2
```
输出的代码为（经过我们的插件处理）：
`fanerge1===fanerge2`
下图为node打印出Balbel输出的代码：
![babel转换的代码](http://p52glq5m1.bkt.clouddn.com/result.png)
[项目地址，非常简单的Babel插件，后续再继续学习](https://github.com/fanerge/babel_plugin)
>	参考文档
[Babel 插件手册](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-introduction)
[ESTree](https://github.com/estree/estree)
[AST Explorer](http://astexplorer.net/)
[探索 babel 和 babel 插件是怎么工作的](https://www.hazyzh.com/b/180211145458)
[掘金-babel插件](https://juejin.im/post/5a9315e46fb9a0633a711f25)









































