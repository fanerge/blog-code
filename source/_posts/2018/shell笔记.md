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
${name[@]} # 1 2 3 // 读取数组所有项目
```
##	获取数组的长度
```
${#name[@]} # 4
${#name[*]} # 4
${#name[n]} # n为下标，返回数组第n个项目的长度
```









