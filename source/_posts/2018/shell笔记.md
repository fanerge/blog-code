---
title: shell笔记
date: 2018-12-12 21:39:01
tags: 'shell'
categories: '服务端'
copyright: true
---
最近公司需要做gitlab分支重置、部署、构建的可视化工具，涉及到Shell脚本相关的知识，先来补充一波。
#	运行Shell脚本&Shell 注释
##	作为可执行程序
```
chmod +x ./demo.sh #使脚本具有执行权限
./demo.sh #执行脚本
```
##	作为解释器参数
```
/bin/sh ./demo.sh
```
##	单行注释
以 # 开头的行就是注释，会被解释器忽略。
##	多行注释
```
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```
PS：EOF 也可以使用其他符号(' !)。
#	Shell 变量
##	声明变量
```
name=fanerge 
name='fanerge'
name="fanerge"
```
##	使用变量
```
$name
${name}
```
##	只读变量
```
name=fanerge
readonly name #表明该变量只读
```
##	删除变量
```
unset nmae
```
##	获取字符串长度
```
name="fanerge"
${#string} # 7
```
##	提取子字符串	
```
name="fanerge"
${name:1} # anerge
${name:1:4} # aner
```
PS：分别代表开始位置和结束位置，没有结束位置则到字符串末尾
##	查找子字符串
```
name="fanerge"
`expr index "${name}" e` # 4 // 从1开始计数
```
#	Shell 数组
##	声明数组
```
name=(1 2 3)
```
##	读取数组
```
${name[2]} # 3 // 读取指定下标项目
${name[*]} # 1 2 3 // 读取数组所有项目
${name[@]} # 1 2 3 // 读取数组所有项目
```
##	获取数组的长度
```
${#name[*]} # 4
${#name[@]} # 4
${#name[n]} # n为下标，返回数组第n个项目的长度
```
##	Shell 传递参数
```
sh test.sh f a n
echo $0 # test.sh
echo $1 # f
```
PS：其他特殊符号。
参数处理	说明
$#	传递到脚本的参数个数
$*	以一个单字符串显示所有向脚本传递的参数。
如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
$$	脚本运行的当前进程ID号
$!	后台运行的最后一个进程的ID号
$@	与$*相同，但是使用时加引号，并在引号中返回每个参数。
如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
#	Shell 基本运算符
原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
##	算术运算符
```
val=`expr 2 + 3` # 5
```
PS：表达式和运算符之间要有空格，加（+）、减（-）、乘（\*）、除（/）、取余（%）、赋值（=）、条件表达式的等于（==）和不等于（!=）
##	关系运算符
关系运算符只支持数字，不支持字符串，除非字符串的值是数字。
运算符	说明	
-eq	检测两个数是否相等，相等返回 true。	
-ne	检测两个数是否不相等，不相等返回 true。	
-gt	检测左边的数是否大于右边的，如果是，则返回 true。	
-lt	检测左边的数是否小于右边的，如果是，则返回 true。
-ge	检测左边的数是否大于等于右边的，如果是，则返回 true。
-le	检测左边的数是否小于等于右边的，如果是，则返回 true。
##	布尔运算符
运算符	说明
!	非运算，表达式为 true 则返回 false，否则返回 true。	
-o	或运算，有一个表达式为 true 则返回 true。
-a	与运算，两个表达式都为 true 才返回 true。
##	逻辑运算符
运算符	说明
&&	逻辑的 AND
||	逻辑的 OR
##	字符串运算符
运算符	说明	
=	检测两个字符串是否相等，相等返回 true。
!=	检测两个字符串是否相等，不相等返回 true。	
-z	检测字符串长度是否为0，为0返回 true。	
-n	检测字符串长度是否为0，不为0返回 true。	
str	检测字符串是否为空，不为空返回 true。
##	文件测试运算符
文件测试运算符用于检测 Unix 文件的各种属性。
操作符	说明	
-b file	检测文件是否是块设备文件，如果是，则返回 true。
-c file	检测文件是否是字符设备文件，如果是，则返回 true。	
-d file	检测文件是否是目录，如果是，则返回 true。
-f file	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。
-g file	检测文件是否设置了 SGID 位，如果是，则返回 true。
-k file	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。
-p file	检测文件是否是有名管道，如果是，则返回 true。	
-u file	检测文件是否设置了 SUID 位，如果是，则返回 true。
-r file	检测文件是否可读，如果是，则返回 true。	
-w file	检测文件是否可写，如果是，则返回 true。	
-x file	检测文件是否可执行，如果是，则返回 true。
-s file	检测文件是否为空（文件大小是否大于0），不为空返回 true。
-e file	检测文件（包括目录）是否存在，如果是，则返回 true。
#	Shell echo命令
```
echo I am fanerge
echo 'I am fanerge' # 原样输出
echo "I am fanerge"
echo "\"I am fanerge\"" # 带有转义字符
echo -e "OK! \n" # -e 开启转义 \n为换行 \c为不换行
echo `date` # 显示命令执行结果
```
#	Shell printf 命令
printf  format-string  [arguments...]
参数说明：
	format-string: 为格式控制字符串
	arguments: 为参数列表。
##	format-string部分参数
%d,用来输出十进制整数。
%f,用来输出实数（包括单，双精度），以小数形式输出，默认情况下保留小数点6位。
%c,用来输出一个字符。
%s,用来输出一个字符串。
%-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。
%-4.2f 指格式化为小数，其中.2指保留2位小数。
##	printf的转义序列
序列	说明
\a	警告字符，通常为ASCII的BEL字符
\b	后退
\c	抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略
\f	换页（formfeed）
\n	换行
\r	回车（Carriage return）
\t	水平制表符
\v	垂直制表符
\\	一个字面上的反斜杠字符
\ddd	表示1到3位数八进制值的字符。仅在格式字符串中有效
\0ddd	表示1到3位的八进制值字符
#	Shell test 命令
Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。
数值测试、字符串测试、文件测试类似
```
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```
#	Shell 流程控制
##	if else
```
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```
##	for 循环
```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```
##	while 语句
```
while condition
do
    command
done
```
PS：let 命令，它用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量。
```
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站URL: '
while read url
do
    echo "是的！${url} 是一个好网站"
done
```
PS：while循环可用于读取键盘信息。
##	无限循环
```
while :
do
    command
done
---------
while true
do
    command
done
---------
for (( ; ; ))
```
##	until 循环
```
until condition
do
    command
done
```
PS：until 循环执行一系列命令直至条件为 true 时停止，这恰好与 while 相反。
##	case
```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
*)
	command1
    command2
    ...
    commandN
    ;;
esac
```
PS：case以esac结尾。
##	跳出循环
在循环过程中，有时候需要在未达到循环结束条件时强制跳出循环，Shell使用两个命令来实现该功能：break和continue。
break命令
break命令允许跳出所有循环（终止执行后面的所有循环）。
continue
continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。
#	Shell 函数
```
[ function ] funname [()]

{

    action;

    [return int;]

}
-------

```
PS：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)。
$1、$2、${10}分别代表第一个、第二个、第十个参数。
函数返回值在调用该函数后通过 $? 来获得。
参数处理	说明
$#	传递到脚本的参数个数
$*	以一个单字符串显示所有向脚本传递的参数
$$	脚本运行的当前进程ID号
$!	后台运行的最后一个进程的ID号
$@	与$*相同，但是使用时加引号，并在引号中返回每个参数。
$-	显示Shell使用的当前选项，与set命令功能相同。
$?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。
#	Shell 输入/输出重定向
命令	说明
command > file	将输出重定向到 file。
PS：注意任何file1内的已经存在的内容将被新内容替代。
command < file	将输入重定向到 file。
command >> file	将输出以追加的方式重定向到 file。
PS：追加。
n > file	将文件描述符为 n 的文件重定向到 file。
n >> file	将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m	将输出文件 m 和 n 合并。
n <& m	将输入文件 m 和 n 合并。
<< tag	将开始标记 tag 和结束标记 tag 之间的内容作为输入。
#	Shell 文件包含
```
. filename   # 注意点号(.)和文件名中间有一空格
```




















