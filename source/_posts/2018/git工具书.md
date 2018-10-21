---
title: git工具书
date: 2018-04-23 22:41:30
tags: 'git'
categories: '代码管理'
copyright: true
---
#	工作原理
![git原理](http://oxpnrlb4j.bkt.clouddn.com/git%E5%8E%9F%E7%90%86.png)
[图片来源](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
[Git 常用操作总结](https://juejin.im/entry/5ac04d195188255c946bcd4a)
PS：
Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库

#	基本操作	
##  git托管项目（新建 or clone一个git项目）
mkdir gitdemo // 在当前目录创建gitdemo目录
cd gitdemo 
pwd // 显示当前目录
git init // 将当前目录设置为git管理的仓库
git init [projectName] // 新建一个目录，将其初始化为Git代码库
or
git clone [ssh/https]

##  工作区>>暂存区（add为添加文件rm为移除文件）
git add [fileName] // 单个或多个文件
git add *.js // 通配符
git add -u  // 提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
git add .  // 提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
git add -A  // 提交所有变化（是git add .和git add -u的结合，git add -all的简写

git rm [file1] [file2] // 删除工作区文件，并且将这次删除放入暂存区
git rm -\-cached [fileName] // 停止追踪指定文件，但该文件会保留在工作区
git mv [fileNameOld] [fileNameNew] // 改名文件，并且将这个改名放入暂存区

##  暂存区>>本地仓库
git commit -m '说明信息' // 提交
git commit -am '说明' // git add . 和 git commit的简写（一次完成两个动作）
常用commit type
- feat: 新功能
- fix: 修复问题
- docs: 修改文档
- style: 修改代码格式，不影响代码逻辑
- refactor: 重构代码，理论上不影响现有功能
- perf: 提升性能
- test: 增加修改测试用例
- chore: 修改工具相关（包括但不限于文档、代码生成等）

##  本地仓库>>远程仓库
git push ssh://git@dev.fanerge.com/gitdemo.git master // 把本地仓库提交到远程仓库的master分支中 
git push -u origin master // 当前分支提交到远程 master 分支
git push // 如果当前分支与远程分支存在追踪关系

##	远程仓库>>本地仓库
git fetch [远程主机名] [分支名] // 远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地
git fetch origin master
git clone [ssh/https]// 从远程主机克隆一个版本库
[更详细的资料](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
##	远程仓库>>工作区
git pull [远程主机名] [远程分支名]:[本地分支名] // 远程主机某个分支的更新，再与本地的指定分支合并
git pull // 当前分支只有一个追踪分支时可省略部分参数

#	分支操作
##	查看分支
git branch // 查看本地所有分支（*表示当前分支）
git branch -v // 查看本地所有分支及最近一次提交信息
git branch -vv // 查看本地所有分支及最近一次提交信息和远程追踪分支
git branch -r // 查看远程所有分支
git branch -a // 查看本地/远程所有分支
##  切换分支
git checkout [branchName] // 切换分支
git checkout - // 切换到上一个分支
##	新建分支
git branch [branchName] // 新建一个分支（但仍然留在当前分支）
git checkout -b [branchName] // 新建一个分支（并切换到该分支）
git branch [branchName] [commitId] // 新建一个分支（并指向指定的commitId）
git branch -\-track [branchName] [remoteBranchName] //新建一个分支（并指定与远程分支的追踪关系）
如：新建 test 分支，并指向远程的 orign/dev 分支 
git branch -\-track test origin/dev
## 新建远程分支（新建本地分支，推送的远程作为远程分支）
git branch [branchName]
git push origin [branchName]:[remoteBranchName]
还需要为新建本地分支和远程分支建立追踪关系
如：本地的 test 分支作为远程的 dev 分支
git push origin test:dev
##	建立追踪关系（当前分支和远程分支）
作用：用来描述当前和远程分支的位置关系，在使用git pull 和 git push 的时候可以省略远程参数
git branch -\-track [branchName] [remoteBranchName] //新建一个分支（并指定与远程分支的追踪关系）
git branch –\-set-upstream-to=origin/[branchName]
如：当前分支和远程 dev 分支建立追踪关系
git branch –\-set-upstream-to=origin/dev
##	合并分支（branchName合并到当前分支）
git merge [branchName]
git rebase [branchName]
PS：
git merge dev（当前分支为master） 
会在 master 分支产生一个新的commit。
git rebase dev（当前分支为master）
会将master上的提交推至顶端。
##	删除分支（本地）
git branch -d [branchName] // 删除分支（本地）	
git push origin -\-delete [branchName] // 删除分支（远程）
git branch -dr [remoteBranchName] // 删除分支（远程）

#	标签管理
tag作用类似于里程碑，可以快速找到里程碑的代码。
##	查看所有tag
git tag
##	新建tag在当前commit
git tag [tagName]
##	新建一个tag在指定commit
git tag [tagName] [commitId]
##	删除本地tag
git tag -d [tagName]
##	删除远程tag
git push origin :refs/tags/[tagName]
##	查看tag信息
git show [tagName]
##	向远程提交指定tag
git push [remoteBranchName] [tagName]
##	新建一个分支，指向某个tag
git checkout -b [branchName] [tagName]

#	查看信息
##	显示有变更的文件
git status
##	显示当前分支的版本历史
git log
##	显示当前分支的版本历史（包括commit发生变更的文件）
git log --stat
##	显示当前分支的版本历史（通过关键词）
git log -S [keyword]
##	显示某个commit之后的所有变动，每个commit占据一行
git log [tagName] HEAD --pretty=format:%s
##	显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
git log [tagName] HEAD --grep feature
##	显示某个文件的版本历史，包括文件改名
git log --follow [fileName]
git whatchanged [fileName]
##	显示指定文件相关的每一次diff
git log -p [fileName]
##	显示过去5次提交
git log -5 --pretty --oneline
##	显示所有提交过的用户，按提交次数排序
git shortlog -sn
##	显示指定文件是什么人在什么时间修改过
git blame [file]
##	显示暂存区和工作区的差异
git biff
##	显示暂存区和上一个commit的差异
git diff --cached [fileName]
##	显示工作区与当前分支最新commit之间的差异
git diff HEAD
##	显示两次提交之间的差异
git diff [first-branch]...[second-branch]
##	显示今天你写了多少行代码
git diff --shortstat "@{0 day ago}"
##	显示某次提交的元数据和内容变化
git show [commitId]
##	显示某次提交发生变化的文件
git show --name-only [commitId]
##	显示某次提交时，某个文件的内容
git show [commitId]:[fileName]
##	显示当前分支的最近几次提交
git reflog

#	远程同步
##	下载远程仓库的所有变动
git fetch [ssh/https]
##	显示所有远程仓库（包括fetch和push地址）
git remote -v
##	显示某一远程仓库的信息
git remote show [remoteRepositoryName]
##	增加一个新的远程仓库，并命名
git remote add [shortname] [url]
##	取回远程仓库的变化，并与本地分支合并（merge）
git pull [remote] [remoteBranchName]
PS：remote 一般为 origin ，当建立追踪关系，可以git pull 省略两个参数
##	上传本地指定分支到远程仓库
git push [remote] [branchName]
##	强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force
##	推送所有分支到远程仓库
git push [remote] --all

#	撤销/恢复
##	撤销工作区修改（指定文件）
git chekout -- [fileName]
##	撤销工作区（所有文件）
git checkout -- .
##	暂存区-->工作区（指定文件）
git reset HEAD [fileName] // 撤销暂存区的修改
git checkout [fileName] // 撤销工作区的修改
##	重置暂存区到上次提交commit（工作区不变）
git reset [fileName]
##	重置暂存区、工作区到上次提交commit
git reset --hard 
git reset HEAD^ --hard // 回到上一次commit
git reset HEAD^^ --hard // 回到上一次的上一次commit
##	重置暂存区、当前分支指定commitId（工作区不变）
git reset [commitId]
##	重置暂存区、工作区、当前分支为指定commitId
git reset --hard [commitId]
##	重置当前HEAD为指定commitId（暂存区、工作区不表）
git reset --keep [commitId]
##	新建一个commitId，用来撤销指定commitId（后者的所有变化都将被前者抵消，并且应用到当前分支）
git revert [commitId]
##	暂时将未提交的变化存于stash，后续再从stash中取出
git stash list // 查看 stash 列表
git stash // 保存当前暂未提交的修改 
git stash apply // 从stash中恢复，但不清除该stash
git stash drop  // 清除该stash
git stash pop // 从stash中恢复并清除该stash
PS：一般用于停下手中活，处理更加紧急的任务，处理完任务后在回过头继续。
git stash pop = git stash apply + git stash drop

#	开发分支合并到master分支
更新与2018-07-17
git rebase的作用用于增删改当前 commits ，并将其合并到对应分支。
适用于：dev分支有开发，master分支其他人有合并，合并时需要以最新的master分支作为基础合并。
假如在dev分支开发完毕需要合并时：
1.	git checkout dev // 确保在你所在的开发分支如dev
2.	git rebase master 
3.	如果有冲突，解决冲突
4.	git rebase \-\-continue
5.	git checkout master // 回到master分支，进行合并
6.	git merge dev

[git：rebase（变基）的使用和理解](https://juejin.im/post/5b46c784f265da0f540513cd)

#	如何将fork别人的库与原项目同步呢
更新于2018-09-12
1.	在fork库中添加上游代码库的 remote 源
git remote add {upstream} git@github.com:xxx/xxx.git 
2.	将本地的修改提交commit
3.	在每次PR前，做如下操作即可与上游{upstream}同步
git remote update upstream
git rebase upstream/{branch name} 如：git rebase upstream/dev
4.	Push 代码
git push origin dev

#	vim
##	Vim共有3种模式
编辑模式(命令模式)-默认模式
输入模式
末行模式
##	各种模式的切换
###	编辑-->输入
 i: 在当前光标所在字符的前面，转为输入模式；
 a: 在当前光标所在字符的后面，转为输入模式；
 o: 在当前光标所在行的下方，新建一行，并转为输入模式；
 I：在当前光标所在行的行首，转换为输入模式
 A：在当前光标所在行的行尾，转换为输入模式
 O：在当前光标所在行的上方，新建一行，并转为输入模式；
###	输入-->编辑
ESC
###	编辑-->末行
:
###	末行-->编辑
ESC, ESC
##	关闭文件
1、末行模式关闭文件
:q  退出
:wq 保存并退出
:q! 不保存并退出
:w 保存
:w! 强行保存
:wq --> :x
2、编辑模式下退出
ZZ: 保存并退出

>	参考文档：
[git 官网](https://git-scm.com/)
[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000)









