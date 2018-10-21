---
title: java基础知识学习
date: 2017-12-26 20:25:18
tags: ['java', '服务端']
categories: 'java'
copyright: true
---
#	常用指令介绍
javac HelloWorld.java -- 该命令用于将 java 源文件编译为 class 字节码文件，如： javac HelloWorld.java。如果成功编译没有错误的话，会出现一个 HelloWorld.class 的文件。
java HelloWorld -- java 后面跟着的是java文件中的类名,例如 HelloWorld 就是类名，如: java HelloWorld。
	
#	配置环境变量	
JAVA_HOME：JDK安装在C:\jdk1.6.0目录里，则设置JAVA_HOME为该目录路径, 那么以后要使用这个路径的时候, 只需输入%JAVA_HOME%即可, 避免每次引用都输入很长的路径串	
path 变量：path 变量使得我们能够在系统中的任何地方运行java应用程序，比如 javac、java、javah 等等,这就要找到我们安装 JDK 的目录，假设我们的JDK安装在 C:\jdk1.6.0 目录下,那么在 C:\jdk1.6.0\bin 目录下就是我们常用的 java 应用程序,我们就需要把 C:\jdk1.6.0\bin 这个目录加到 path 环境变量里面。	
classpath 变量：classpath 环境变量，是当我们在开发java程序时需要引用别人写好的类时，要让 java 解释器知道到哪里去找这个类。通常，sun 为我们提供了一些额外的丰富的类包，一个是 dt.jar，一个是 tools.jar，这两个 jar 包都位于 C:\jdk1.6.0\lib 目录下，所以通常我们都会把这两个 jar 包加到我们的 classpath 环境变量中 set classpath=.;C:\jdk1.6.0\lib\tools.jar;C:\jdk1.6.0\lib\dt.jar。	
	
#	Java 基础语法	
一个Java程序可以认为是一系列对象的集合，而这些对象通过调用彼此的方法来协同工作。
##	基本概念
类：类是一个模板，它描述一类对象的行为和状态。
对象：对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
方法：方法就是行为，一个类可以有很多方法。逻辑运算、数据修改以及所有动作都是在方法中完成的。	
实例变量：每个对象都有独特的实例变量，对象的状态由这些实例变量的值决定。
##	基本语法
大小写敏感：Java是大小写敏感的，这就意味着标识符Hello与hello是不同的。
类名：对于所有的类来说，都使用大驼峰。
方法名：所有的方法名都应该使用小驼峰。
源文件名：源文件名必须和类名相同。当保存文件的时候，你应该使用类名作为文件名保存（切记Java是大小写敏感的），文件名的后缀为.java。（如果文件名和类名不相同则会导致编译错误）。
主方法入口：所有的Java 程序由public static void main(String []args)方法开始执行。
##	Java标识符
Java所有的组成部分都需要名字。类名、变量名以及方法名都被称为标识符。
跟其他语言类似。
##	Java修饰符
Java可以使用修饰符来修饰类中方法和属性。
访问控制修饰符 : default, public , protected, private
非访问控制修饰符 : final, abstract, strictfp
##	Java变量
局部变量
类变量（静态变量）
成员变量（非静态变量）
##	Java数组
数组是储存在堆上的对象，可以保存多个同类型变量。
这里有别于js的数组，同类型限制。
##	Java枚举
枚举限制变量只能是预先设定好的值。
例如，我们为果汁店设计一个程序，它将限制果汁为小杯、中杯、大杯。这就意味着它不允许顾客点除了这三种尺寸外的果汁。
##	Java关键字
这些保留字不能用于常量、变量、和任何标识符的名称。
跟其他语言类似。
##	Java注释
单行 //
多行 /**/
##	继承
如果你要创建一个类，而且已经存在一个类具有你所需要的属性或方法，那么你可以将新创建的类继承该类。
被继承的类称为超类（super class），派生类称为子类（subclass）。
##	接口
在Java中，接口可理解为对象间相互通信的协议。
接口只定义派生要用到的方法，但是方法的具体实现完全取决于派生类。

#	Java 对象和类
对象：对象是类的一个实例，有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。
类：类是一个模板，它描述一类对象的行为和状态。
##	Java中的类
	```
	public class Dog{
	  String breed; // 成员变量
	  int age;
	  String color;
	  
	  // 构造方法
	  public Dog(String name){
        // 这个构造器仅有一个参数：name
	  }
	  
	  // 入口函数
	  public static void main(String []args){
		// 下面的语句将创建一个Dog对象
		Dog myDog = new Dog( "tommy" );
	  }
	  
	  Static demo; // 类变量
	  void barking(){
	  }
	 
	  void hungry(){
		String dd; // 局部变量
	  }
	 
	  void sleeping(){
	  }
	}
	```
局部变量：在方法、构造方法或者语句块中定义的变量被称为局部变量。
成员变量：成员变量是定义在类中，方法体之外的变量。
类变量：类变量也声明在类中，方法体之外，但必须声明为static类型。
##	构造方法
构造方法的名称必须与类同名，一个类可以有多个构造方法。
	```
	  public Dog(String name){
        // 这个构造器仅有一个参数：name
	  }
	```
##	访问实例变量和方法
	```
	muDog = new Dog();
	/* 访问类中的变量 */
	muDog.breed;
	/* 访问类中的方法 */
	muDog.hungry();
	```
##	源文件声明规则
当在一个源文件中定义多个类，并且还有import语句和package语句时，要特别注意这些规则。
	一个源文件中只能有一个public类
	一个源文件可以有多个非public类
	源文件的名称应该和public类的类名保持一致。例如：源文件中public类的类名是Employee，那么源文件应该命名为Employee.java。
	如果一个类定义在某个包中，那么package语句应该在源文件的首行。
	如果源文件包含import语句，那么应该放在package语句和类定义之间。如果没有package语句，那么import语句应该在源文件中最前面。
	import语句和package语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。
##	Java包
包主要用来对类和接口进行分类。当开发Java程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。
##	Import语句
Import语句用来提供一个合理的路径，使得编译器可以找到某个类。
例如，下面的命令行将会命令编译器载入java_installation/java/io路径下的所有类 import java.io.*;

#	Java 基本数据类型
变量就是申请内存来存储值。也就是说，当创建变量的时候，需要在内存中申请空间。
内存管理系统根据变量的类型为变量分配存储空间，分配的空间只能用来储存该类型数据。
实际上，JAVA中还存在另外一种基本类型void，它也有对应的包装类 java.lang.Void，不过我们无法直接对它们进行操作。
Java 的两大数据类型:
	内置数据类型
	引用数据类型
##	内置数据类型
Java语言提供了八种基本类型。六种数字类型（四个整数型，两个浮点型），一种字符类型，还有一种布尔型。
###	byte
byte 数据类型是8位、有符号的，以二进制补码表示的整数；
	范围：-128（-2^7） 到 127（2^7-1）；
	byte 类型用在大型数组中节约空间，主要代替整数，因为 byte 变量占用的空间只有 int 类型的四分之一；
	例子：byte a = 100，byte b = -50。
###	short
short 数据类型是 16 位、有符号的以二进制补码表示的整数
	范围：-32768（-2^15） 到 32767（2^15 - 1）；
	Short 数据类型也可以像 byte 那样节省空间。一个short变量是int型变量所占空间的二分之一；
	例子：short s = 1000，short r = -20000。
###	int
int 数据类型是32位、有符号的以二进制补码表示的整数；
	范围：-2,147,483,648（-2^31） 到 2,147,483,647（2^31 - 1）；
	一般地整型变量默认为 int 类型；
	例子：int a = 100000, int b = -200000。
###	long
long 数据类型是 64 位、有符号的以二进制补码表示的整数；
	范围：-9,223,372,036,854,775,808（-2^63） 到 9,223,372,036,854,775,807（2^63 -1）；
	这种类型主要使用在需要比较大整数的系统上；
	例子： long a = 100000L，Long b = -200000L。
###	float
float 数据类型是单精度、32位、符合IEEE 754标准的浮点数；
	float 在储存大型浮点数组的时候可节省内存空间；
	浮点数不能用来表示精确的值，如货币；
	例子：float f1 = 234.5f。
###	double
double 数据类型是双精度、64 位、符合IEEE 754标准的浮点数；
	浮点数的默认类型为double类型；
	double类型同样不能表示精确的值，如货币；
	例子：double d1 = 123.4d 或者 123.4。
###	boolean
boolean数据类型表示一位的信息；
	只有两个取值：true 和 false；
	默认值是 false；
	例子：boolean one = true。
###	char
char类型是一个单一的 16 位 Unicode 字符；
	范围：\u0000（即为0） 到 \uffff（即为65,535）；
	char 数据类型可以储存任何字符；
	例子：char letter = 'A';。
##	引用类型
	对象、数组都是引用数据类型。
	所有引用类型的默认值都是null。
	一个引用变量可以用来引用任何与之兼容的类型。
	例子：Site site = new Site("Runoob")。
##	Java 常量
常量在程序运行时是不能被修改的。
	在 Java 中使用 final 关键字来修饰常量。
	final double PI = 3.1415927;
##	自动类型转换	
整型、实型（常量）、字符型数据可以混合运算。运算中，不同类型的数据先转化为同一类型，然后进行运算。
转换从低级到高级。
低  ------------------------------------>  高
byte,short,char—> int —> long—> float —> double 	
##	自动类型转换	
必须满足转换前的数据类型的位数要低于转换后的数据类型，例如: short数据类型的位数为16位，就可以自动转换位数为32的int类型，同样float数据类型的位数为32，可以自动转换为64位的double类型。	
强制类型转换
	1. 条件是转换的数据类型必须是兼容的。
	2. 格式：(type)value type是要强制类型转换后的数据类型。		
隐含强制类型转换	
	1. 整数的默认类型是 int。
	2. 浮点型不存在这种情况，因为在定义 float 类型时必须在数字后面跟上 F 或者 f。
#	Java 变量类型
在Java语言中，所有的变量在使用前必须声明。
	声明格式：type identifier [ = value][, identifier [= value] ...] ;
Java语言支持的变量类型有：	
	类变量：独立于方法之外的变量，用 static 修饰。
	实例变量：独立于方法之外的变量，不过没有 static 修饰。
	局部变量：类的方法中的变量。
##	Java 局部变量	
	局部变量声明在方法、构造方法或者语句块中；
	局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁；
	访问修饰符不能用于局部变量；
	局部变量只在声明它的方法、构造方法或者语句块中可见；
	局部变量是在栈上分配的。
	局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。
##	实例变量
实例变量声明在一个类中，但在方法、构造方法和语句块之外；
当一个对象被实例化之后，每个实例变量的值就跟着确定；
实例变量在对象创建的时候创建，在对象被销毁的时候销毁；
实例变量的值应该至少被一个方法、构造方法或者语句块引用，使得外部能够通过这些方式获取实例变量信息；
实例变量可以声明在使用前或者使用后；
访问修饰符可以修饰实例变量；
实例变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见；
实例变量具有默认值。数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；
实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：ObejectReference.VariableName。
```
// 这个实例变量对子类可见
public String name;
// 私有变量，仅在该类可见
private double salary;
```
##	类变量（静态变量）
类变量也称为静态变量，在类中以static关键字声明，但必须在方法构造方法和语句块之外。
无论一个类创建了多少个对象，类只拥有类变量的一份拷贝。
静态变量除了被声明为常量外很少使用。常量是指声明为public/private，final和static类型的变量。常量初始化后不可改变。
静态变量储存在静态存储区。经常被声明为常量，很少单独使用static声明变量。
静态变量在程序开始时创建，在程序结束时销毁。
与实例变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量声明为public类型。
默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量还可以在静态语句块中初始化。
静态变量可以通过：ClassName.VariableName的方式访问。
类变量被声明为public static final类型时，类变量名称一般建议使用大写字母。如果静态变量不是public和final类型，其命名方式与实例变量以及局部变量的命名方式一致。
#	Java 修饰符	
Java语言提供了很多修饰符，主要分为以下两类：
	访问修饰符
	非访问修饰符
##	访问控制修饰符
Java中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Javav支持 4 种不同的访问权限。
	权限有4中：当前类、同一包内、子孙类、其他包
	public : 对所有类可见。使用对象：类、接口、变量、方法。
	protected : 对同一包内的类和所有子类可见。使用对象：变量、方法。 注意：不能修饰类（外部类）。
	default (即缺省，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。
	private : 在同一类内可见。使用对象：变量、方法。 注意：不能修饰类（外部类）
访问控制和继承
	父类中声明为 public 的方法在子类中也必须为 public。
	父类中声明为 protected 的方法在子类中要么声明为 protected，要么声明为 public，不能声明为 private。
	父类中声明为 private 的方法，不能够被继承。
##	非访问修饰符	
static 修饰符，用来修饰类方法和类变量。
final final 变量能被显式地初始化并且只能初始化一次。被声明为 final 的对象的引用不能指向不同的对象。但是 final 对象里的数据可以被改变。也就是说 final 对象的引用不能改变，但是里面的值可以改变。
	类中的 final 方法可以被子类继承，但是不能被子类修改。
abstract 抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充。
	抽象方法是一种没有任何实现的方法，该方法的的具体实现由子类提供。
synchronized 关键字声明的方法同一时间只能被一个线程访问。
transient 序列化的对象包含被 transient 修饰的实例变量时，java 虚拟机(JVM)跳过该特定的变量。
volatile volatile 修饰的成员变量在每次被线程访问时，都强制从共享内存中重新读取该成员变量的值。而且，当成员变量发生变化时，会强制线程将变化值回写到共享内存。这样在任何时刻，两个不同的线程总是看到某个成员变量的同一个值。
#	Java 运算符
	算术运算符
	关系运算符
	位运算符
	逻辑运算符
	赋值运算符
	其他运算符
	instanceof 运算符
		该运算符用于操作对象实例，检查该对象是否是一个特定类型（类类型或接口类型）。
		( Object reference variable ) instanceof  (class/interface type)
	类别	操作符	关联性
	后缀	() [] . (点操作符)	左到右
	一元	++ -- ！~	从右到左
	乘性 	* /％	左到右
	加性 	+ -	左到右
	移位 	>> >>>  << 	左到右
	关系 	>> = << = 	左到右
	相等 	==  !=	左到右
	按位与	＆	左到右
	按位异或	^	左到右
	按位或	|	左到右
	逻辑与	&&	左到右
	逻辑或	| |	左到右
	条件	？：	从右到左
	赋值	= + = - = * = / =％= >> = << =＆= ^ = | =	从右到左
	逗号	，	左到右
注：只有一元、条件（三目）、赋值运算符关联性为从右向左，其余全是从左向右	
#	Java 循环结构	
	while 循环
	do…while 循环
		对于 while 语句而言，如果不满足条件，则不能进入循环。但有时候我们需要即使不满足条件，也至少执行一次。
		do…while 循环和 while 循环相似，不同的是，do…while 循环至少会执行一次。
	for 循环
	Java 增强 for 循环（对于数组）
		```
		for(声明语句 : 表达式)
		{
		   //代码句子
		}
		```
	break 关键字
		break 主要用在循环语句或者 switch 语句中。
		break 在循环语句中作用是跳出本层（并非本次）的循环。
		break 在switch语句中作用是跳出该switch语句体。
	continue 关键字
		continue 适用于任何循环语句中。作用是让程序立刻跳转到下一次循环的迭代。
		在 for 循环中，continue 语句使程序立即跳转到更新语句i++。
		在 while 或者 do…while 循环中，程序立即跳转到布尔表达式的判断语句。
	return 关键字
		返回数据给函数的调用者。
		函数一旦执行到了return关键字，那么该函数马上结束。 (能结束一个函数)
#	Java 分支结构
	if 语句
	switch 语句
#	Java Number & Math 类	
所有的包装类（Integer、Long、Byte、Double、Float、Short）都是抽象类 Number 的子类。
这种由编译器特别支持的包装称为装箱，所以当内置数据类型被当作对象使用的时候，编译器会把内置类型装箱为包装类。相似的，编译器也可以把一个对象拆箱为内置类型。Number 类属于 java.lang 包。	
##	Java Math 类	
Java 的 Math 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。
Math 的方法都被定义为 static 形式，通过 Math 类可以在主函数中直接调用。	
	[Number & Math 类方法](http://www.runoob.com/java/java-number.html)
#	Java Character 类
Character 类用于对单个字符进行操作。
Character 类在对象中包装一个基本类型 char 的值	
##	转义序列
前面有反斜杠（\）的字符代表转义字符，它对编译器来说是有特殊含义的。
	[Character 类方法](http://www.runoob.com/java/java-character.html)
#	Java String 类
字符串广泛应用 在Java 编程中，在 Java 中字符串属于对象，Java 提供了 String 类来创建和操作字符串。
注意:String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了。
如果需要对字符串做很多修改，那么应该选择使用 StringBuffer & StringBuilder 类。
##	创建字符串	
	String greeting = "fanerge";
##	字符串长度	
	int len = greeting.length();
##	连接字符串
string1.concat(string2);	
	[String 类方法](http://www.runoob.com/java/java-string.html)
#	Java StringBuffer 和 StringBuilder 类
当对字符串进行修改的时候，需要使用 StringBuffer 和 StringBuilder 类。
和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。
它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。	
	[StringBuffer 和 StringBuilder 类方法](http://www.runoob.com/java/java-stringbuffer.html)
#	Java 数组
Java 语言中提供的数组是用来存储固定大小的同类型元素。
##	声明数组变量	
格式：dataType[] arrayRefVar;   // 首选的方法	
	dataType arrayRefVar[];  // 效果相同，但不是首选方法
实例：double[] myList; // 声明一个为double类型的数组	
##	创建数组
	arrayRefVar = new dataType[arraySize];
数组变量的声明，和创建数组可以用一条语句完成，如下所示：
	dataType[] arrayRefVar = new dataType[arraySize];
##	处理数组
循环处理 for
foreach 循环
	[Array类方法](http://www.runoob.com/java/java-array.html)	
#	Java 日期时间	
Date 类
	[Date类方法](http://www.runoob.com/java/java-date-time.html)
	Calendar类
Calendar类是一个抽象类，在实际使用时实现特定的子类的对象，创建对象的过程对程序员来说是透明的，只需要使用getInstance方法创建即可。
	GregorianCalendar类	
	Calendar类实现了公历日历，GregorianCalendar是Calendar类的一个具体实现。
#	Java 正则表达式
java正则在java.util.regex包中
Pattern 类：
	pattern 对象是一个正则表达式的编译表示。Pattern 类没有公共构造方法。要创建一个 Pattern 对象，你必须首先调用其公共静态编译方法，它返回一个 Pattern 对象。该方法接受一个正则表达式作为它的第一个参数。
Matcher 类：
	Matcher 对象是对输入字符串进行解释和匹配操作的引擎。与Pattern 类一样，Matcher 也没有公共构造方法。你需要调用 Pattern 对象的 matcher 方法来获得一个 Matcher 对象。
PatternSyntaxException：
	PatternSyntaxException 是一个非强制异常类，它表示一个正则表达式模式中的语法错误。	
##	捕获组	
	捕获组是把多个字符当一个单独单元进行处理的方法，它通过对括号内的字符分组来创建。	
##	正则表达式语法
[正则方法](http://www.runoob.com/java/java-regular-expressions.html)
#	Java 方法
##	方法的定义
	修饰符 返回值类型 方法名(参数类型 参数名){
		...
		方法体
		...
		return 返回值;
	}
##	方法调用
	注：main 方法是被 JVM 调用的，除此之外，main 方法和其它方法没什么区别。	
void 关键字
方法的重载	
	创建另一个有相同名字但参数不同的方法 -- 方法的重载
	就是说一个类的两个方法拥有相同的名字，但是有不同的参数列表。
变量作用域
	变量的范围是程序中该变量可以被引用的部分。
	方法的参数范围涵盖整个方法。参数实际上是一个局部变量。
	for循环的初始化部分声明的变量，其作用范围在整个循环。
	但循环体内声明的变量其适用范围是从它声明到循环体结束。
命令行参数的使用	
构造方法
	构造方法和它所在类的名字相同，但构造方法没有返回值。（可不写）
可变参数	
	语法：typeName... parameterName
finalize() 方法
	Java 允许定义这样的方法，它在对象被垃圾收集器析构(回收)之前调用，这个方法叫做 finalize( )，它用来清除回收对象。
#	Java 流(Stream)、文件(File)和IO
Java.io 包几乎包含了所有操作输入、输出需要的类。所有这些流类代表了输入源和输出目标。
Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等等。
##	读取控制台输入	
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	int read( ) throws IOException
##	从控制台读取字符串
	String readLine( ) throws IOException
##	控制台输出
控制台的输出由 print( ) 和 println() 完成。这些方法都由类 PrintStream 定义，System.out 是该类对象的一个引用。
PrintStream 继承了 OutputStream类，并且实现了方法 write()。这样，write() 也可以用来往控制台写操作。
##	读写文件
一个流被定义为一个数据序列。输入流用于从源读取数据，输出流用于向目标写数据。	
FileInputStream
	该流用于从文件读取数据，它的对象可以用关键字 new 来创建。	
FileOutputStream
	该类用来创建一个文件并向文件中写数据。	
##	文件和I/O
	File Class(类)
	FileReader Class(类)
	FileWriter Class(类)
##	Java中的目录
创建目录：
	mkdir( )方法创建一个文件夹，成功则返回true，失败则返回false。失败表明File对象指定的路径已经存在，或者由于整个路径还不存在，该文件夹不能被创建。
	mkdirs()方法创建一个文件夹和它的所有父文件夹。
读取目录：
	一个目录其实就是一个 File 对象，它包含其他文件和文件夹。
删除目录或文件
	删除文件可以使用 java.io.File.delete() 方法。
#	Java Scanner 类
java.util.Scanner 是 Java5 的新特征，我们可以通过 Scanner 类来获取用户的输入。	
	通过 Scanner 类的 next() 与 nextLine() 方法获取输入的字符串，在读取前我们一般需要 使用 hasNext 与 hasNextLine 判断是否还有输入的数据	
next() 与 nextLine() 区别
next():
	1、一定要读取到有效字符后才可以结束输入。
	2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。
	3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
next() 不能得到带有空格的字符串。
nextLine()：
	1、以Enter为结束符,也就是说 nextLine()方法返回的是输入回车之前的所有字符。
	2、可以获得空白。
#	Java 异常处理
异常发生的原因有很多，通常包含以下几大类：
	用户输入了非法数据。
	要打开的文件不存在。
	网络通信时连接中断，或者JVM内存溢出。
要理解Java异常处理是如何工作的，你需要掌握以下三种类型的异常：
	检查性异常：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
	运行时异常： 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
	错误： 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。
##	Exception 类的层次
	异常类有两个主要的子类：IOException 类和 RuntimeException 类。
##	Java 内置异常类
##	异常方法
##	捕获异常
##	多重捕获块
##	throws/throw 关键字
##	finally关键字	
##	声明自定义异常
#	Java 继承
##	类的继承格式
	```
	class 父类 {
	}
	 
	class 子类 extends 父类 {
	}
	```
##	继承的特性
子类拥有父类非private的属性，方法。
子类可以拥有自己的属性和方法，即子类可以对父类进行扩展。
子类可以用自己的方式实现父类的方法。
Java的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如A类继承B类，B类继承C类，所以按照关系就是C类是B类的父类，B类是A类的父类，这是java继承区别于C++继承的一个特性。
提高了类之间的耦合性（继承的缺点，耦合度高就会造成代码之间的联系）。
##	继承关键字
继承可以使用 extends 和 implements 这两个关键字来实现继承，而且所有的类都是继承于 java.lang.Object
###	extends关键字
在 Java 中，类的继承是单一继承，也就是说，一个子类只能拥有一个父类，所以 extends 只能继承一个类。
###	implements关键字
使用 implements 关键字可以变相的使java具有多继承的特性，使用范围为类继承接口的情况，可以同时继承多个接口（接口跟接口之间采用逗号分隔）。
	```
	public class C implements A,B {
	}
	```
###	super 与 this 关键字
	super关键字：我们可以通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。
	this关键字：指向自己的引用。
###	final关键字
	final 关键字声明类可以把类定义为不能继承的，即最终类；或者用于修饰方法，该方法不能被子类重写：
	声明类：
		final class 类名 {//类体}
	声明方法：
		修饰符(public/private/default/protected) final 返回值类型 方法名(){//方法体}
##	构造器
子类不能继承父类的构造器（构造方法或者构造函数），但是父类的构造器带有参数的，则必须在子类的构造器中显式地通过super关键字调用父类的构造器并配以适当的参数列表。
#	Java 重写(Override)与重载(Overload)
##	重写(Override)
	重写是子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变。
###	方法的重写规则
参数列表必须完全与被重写方法的相同；
返回类型必须完全与被重写方法的返回类型相同；
访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为public，那么在子类中重写该方法就不能声明为protected。
父类的成员方法只能被它的子类重写。
声明为final的方法不能被重写。
声明为static的方法不能被重写，但是能够被再次声明。
子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为private和final的方法。
子类和父类不在同一个包中，那么子类只能够重写父类的声明为public和protected的非final方法。
重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。
构造方法不能被重写。
如果不能继承一个方法，则不能重写这个方法。
###	Super关键字的使用
	当需要在子类中调用父类的被重写方法时，要使用super关键字。
##	重载(Overload)
重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。
方法的重写(Overriding)和重载(Overloading)是java多态性的不同表现，重写是父类与子类之间多态性的一种表现，重载可以理解成多态的具体表现形式。
#	Java 多态
多态是同一个行为具有多个不同表现形式或形态的能力。	
##	多态存在的三个必要条件
	继承
	重写
	父类引用指向子类对象
		Parent p = new Child();
##	虚方法
	当子类对象调用重写的方法时，调用的是子类的方法，而不是父类中被重写的方法。
	要想调用父类中被重写的方法，则必须使用关键字super。
##	多态的实现方式
方式一：重写：
	这个内容已经在上一章节详细讲过，就不再阐述，详细可访问：Java 重写(Override)与重载(Overload)。
方式二：接口
1. 生活中的接口最具代表性的就是插座，例如一个三接头的插头都能接在三孔插座中，因为这个是每个国家都有各自规定的接口规则，有可能到国外就不行，那是因为国外自己定义的接口类型。
2. java中的接口类似于生活中的接口，就是一些方法特征的集合，但没有方法的实现。具体可以看 java接口 这一章节的内容。
方式三：抽象类和抽象方法
	详情请看 Java抽象类 章节。
#	Java 抽象类
在面向对象的概念中，所有的对象都是通过类来描绘的，但是反过来，并不是所有的类都是用来描绘对象的，如果一个类中没有包含足够的信息来描绘一个具体的对象，这样的类就是抽象类。
##	抽象类
	在Java语言中使用abstract class来定义抽象类。
##	继承抽象类
	尽管我们不能实例化一个Employee类的对象，但是如果我们实例化一个Salary类对象，该对象将从 Employee 类继承7个成员方法，且通过该方法可以设置或获取三个成员变量。
##	抽象方法	
	如果你想设计这样一个类，该类包含一个特别的成员方法，该方法的具体实现由它的子类确定，那么你可以在父类中声明该方法为抽象方法。
	Abstract关键字同样可以用来声明抽象方法，抽象方法只包含一个方法名，而没有方法体。
	声明抽象方法会造成以下两个结果：
		如果一个类包含抽象方法，那么该类必须是抽象类。
		任何子类必须重写父类的抽象方法，或者声明自身为抽象类。
#	Java 封装
在面向对象程式设计方法中，封装（英语：Encapsulation）是指一种将抽象性函式接口的实现细节部份包装、隐藏起来的方法。
封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问。
要访问该类的代码和数据，必须通过严格的接口控制。
封装最主要的功能在于我们能修改自己的实现代码，而不用修改那些调用我们代码的程序片段。
适当的封装可以让程式码更容易理解与维护，也加强了程式码的安全性。
##	实现Java封装的步骤	
. 	修改属性的可见性来限制对属性的访问（一般限制为private）
. 	对每个值属性提供对外的公共方法访问，也就是创建一对赋取值方法，用于对私有属性的访问getter和setter方法
.	采用 this 关键字是为了解决实例变量（private String name）和局部变量（setName(String name)中的name变量）之间发生的同名的冲突。
#	Java 接口
接口（英文：Interface），在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。
##	接口的声明	
[可见度] interface 接口名称 [extends 其他的类名] {
        // 声明变量
        // 抽象方法
	}
##	接口的实现
	class Cat implements 接口名称[, 其他接口, 其他接口..., ...] ...
##	接口的继承
	一个接口能继承另一个接口，和类之间的继承方式比较相似。接口的继承使用extends关键字，子接口继承父接口的方法。
##	接口的多继承
	public interface Hockey extends Sports, Event
##	标记接口
	最常用的继承接口是没有包含任何方法的接口。
	标识接口是没有任何方法和属性的接口.它仅仅表明它的类属于一个特定的类型,供其他代码来测试允许做一些事情。
	标识接口作用：简单形象的说就是给某个对象打个标（盖个戳），使对象拥有某个或某些特权。
#	Java 包(package)
为了更好地组织类，Java 提供了包机制，用于区别类名的命名空间。
Java 使用包（package）这种机制是为了防止命名冲突，访问控制，提供搜索和定位类（class）、接口、枚举（enumerations）和注释（annotation）等。	
语法：package pkg1[．pkg2[．pkg3…]];
	例如,一个Something.java 文件它的内容
	package net.java.util
	public class Something{
	   ...
	}
	那么它的路径应该是 net/java/util/Something.java 这样保存的。
##	创建包
	创建包的时候，你需要为这个包取一个合适的名字。之后，如果其他的一个源文件包含了这个包提供的类、接口、枚举或者注释类型的时候，都必须将这个包的声明放在这个源文件的开头。
	包声明应该在源文件的第一行，每个源文件只能有一个包声明，这个文件中的每个类型都应用于它。
	如果一个源文件中没有使用包声明，那么其中的类，函数，枚举，注释等将被放在一个无名的包（unnamed package）中。
##	import 关键字
	为了能够使用某一个包的成员，我们需要在 Java 程序中明确导入该包。使用 "import" 语句可完成此功能。
	在 java 源文件中 import 语句应位于 package 语句之后，所有类的定义之前，可以没有，也可以有多条
语法：import package1[.package2…].(classname|*);
	用 import 关键字引入，使用通配符 "*"  import payroll.*;
##	package 的目录结构
	1.创建 vehicle 目录
	2.在目录中新建 Car.java
		// 文件名 :  Car.java
		package vehicle;
		 
		public class Car {
		   // 类实现  
		}
##	设置 CLASSPATH 系统变量
	类目录的绝对路径叫做 class path。设置在系统变量 CLASSPATH 中。编译器和 java 虚拟机通过将 package 名字加到 class path 后来构造 .class 文件的路径。
用下面的命令显示当前的CLASSPATH变量：
	Windows 平台（DOS 命令行下）：C:\> set CLASSPATH
	UNIX 平台（Bourne shell 下）：# echo $CLASSPATH
删除当前CLASSPATH变量内容：
	Windows 平台（DOS 命令行下）：C:\> set CLASSPATH=
	UNIX 平台（Bourne shell 下）：# unset CLASSPATH; export CLASSPATH
设置CLASSPATH变量:
	Windows 平台（DOS 命令行下）： C:\> set CLASSPATH=C:\users\jack\java\classes
	UNIX 平台（Bourne shell 下）：# CLASSPATH=/home/jack/java/classes; export CLASSPATH
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	





