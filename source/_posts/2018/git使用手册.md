---
title: git使用手册
date: 2018-01-13 20:50:00
tags: 'git'
categories: '代码管理'
copyright: true
---
#	git基础知识
##	git工作流
你的本地仓库由 git 维护的三棵“树”组成。
第一个是你的 工作目录，它持有实际文件；
第二个是 暂存区（Index），它像个缓存区域，临时保存你的改动；
最后是 HEAD，它指向你最后一次提交的结果。
下图展示其关系
![git工作流](http://www.runoob.com/manual/git-guide/img/trees.png)
##	git配置用户信息
Git是分布式版本控制系统，SVN都是集中式的版本控制系统（需要中央服务器）
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
PS：注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
#	版本相关
##	创建版本库
版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
```
mkdir dirname 
cd dirname
pwd // pwd命令用于显示当前目录(绝对路径)
git init // 把这个目录变成Git可以管理的仓库
```
PS：目录名和文件名不可有中文
	.git 文件就是Git来跟踪管理版本库，千万别手动更改。
	如果没有.git 文件（系统隐藏关键文件），可以 ls -ah命令来查看
###	把文件添加到版本库
第一步，用命令git add告诉Git，把文件<span style="color: red">添加</span>到仓库：
实际上就是把文件修改添加到暂存区
```
git add fileName
git add *.js // 通配符
git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
git add -A  提交所有变化（是git add .和git add -u的结合，git add -all的简写）
```
git add . ：他会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。
git add -u ：（git add --update的缩写）他仅监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。
<strong>2018-03-16更新</strong>
PS：可以向git库多次添加文件，并一次提交
第二步，用命令git commit告诉Git，把文件<span style="color: red">提交</span>到仓库：
实际上就是把暂存区的所有内容提交到当前分支（默认是master分支）
`
git commit -m "说明文本"
`
PS：-m后面输入的是本次提交的说明，方便以后查看
更好的理解：需要提交的文件修改通过放到暂存区，然后，一次性提交暂存区的所有修改。
`
git commit -am '说明'
`
PS：git add 和 git commit的简写
清屏
`
reset + Enter
`
该命令可以让我们时刻掌握仓库当前的状态（当前是否有需要提交的修改）
`
git mv <oldName> <nemeName>
`
PS：git mv 命令用于移动或重命名一个文件、目录、软连接。
`
git status
`
PS：列出当前目录所有还没有被git管理的文件和 被git管理且被修改但还未提交（git commit）的文件。
查看具体修改了什么内容
```
git diff
git diff HEAD -- fileName // 比对某个文件
git diff <source_branch> <target_branch>
	比对两个分支
尚未缓存的改动：git diff
查看已缓存的改动： git diff --cached
查看已缓存的与未缓存的所有改动：git diff HEAD
显示摘要而非整个 diff：git diff --stat
```
PS：提交仓库前最好看一下，这是不是我们更改的。
经过对比后，就可以放心的添加和提交文件了
```
git add fileName
git commit -m 'note'
```
##	版本回退
该命令显示从最近到最远的提交日志
```
git log // 下面命令仅仅显示commit id（版本号），它是16进制数
git log --oneline // 历史记录的简洁的版本
git log --oneline --graph // 查看历史中什么时候出现了分支、合并
git log --reverse --oneline // 逆向显示所有日志
git log --author=fanerge // 查找指定用户的提交日志
git log --oneline --before={3.weeks.ago} --after={2010-04-18} --no-merges 
// --since 和 --before，但是你也可以用 --until 和 --after
```
回到上一个版本
```
// 当前版本是HEAD，上一个版本就是HEAD^，上上一个版本就是HEAD^^，前100的版本HEAD~100
git reset --hard HEAD^ // 回到上一个版本
```
回到未来的版本
`
git reset --hard 版本号 // 版本号可以只写几位，git帮我们完善
`
显示文件
`
cat fileName
`
如果忘记版本号想回到最新的版本怎么办？该命令可以查看到版本号
`
git reflog
`
PS：可以查看所有分支的所有操作记录（包括（包括commit和reset的操作），包括已经被删除的commit记录，git log则不能察看已经删除了的commit记录。
##	工作区和暂存区
###	工作区（Working Directory）
	就是之前mkdir生产的目录，存放git项目的目录。
###	版本库（Repository）
	工作区有一个隐藏目录.git，这个就是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
PS：其实git add fileName命令就是讲对应的文件添加到暂存区
	其实git commit 命令仅仅是将暂存区的东西提交到当前分支
##	管理修改
Git跟踪并管理的是修改，而非文件。
Git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中。
##	撤销修改
把 fileName 文件在工作区的修改全部撤销，这里有三种情况：
1.	一是 fileName 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
2.	二是 fileName 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
3.	三是 fileName 已经提交到本地版本库中，请使用 git reset --hard HEAD^
总之，就是让这个文件回到最近一次git commit或git add时的状态。

处理方式：
###	尚未存在暂存区
```
	git checkout -- fileName
```
PS：git checkout -\- fileName命令中的-\-很重要，没有-\-，就变成了“切换到另一个分支”的命令。
###	已存在暂存区
```
	git reset HEAD fileName	
	git checkout -- fileName // 必须要使用第一点的方式
```
PS：git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。
![工作区-暂存区-HEAD](http://marklodato.github.io/visual-git-guide/basic-usage.svg)
上面的四条命令在工作目录、暂存目录(也叫做索引)和仓库之间复制文件。
*	git add files 把当前文件放入暂存区域。
*	git commit 给暂存区域生成快照并提交。
*	git reset -- files 用来撤销最后一次git add files，你也可以用git reset 撤销所有暂存区域文件。
*	git checkout -- files 把文件从暂存区域复制到工作目录，用来丢弃本地修改。

##	删除文件
直接右键或命令删除文件
`
rm fileName
`
PS：此时，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了
这是有两种处理方式：
1. 真的想删除这个文件
```
	git rm fileName // git中删除对应文件
	git commit -m '说明' // 同步工作区和版本库
```
2.	删错了，你想从版本库中恢复（无论工作区是修改还是删除，都可以“一键还原”）
`
	git checkout -- fieName
`
PS：命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。	
#	远程仓库
##	设置ssh（以github举例）	
第1步：创建SSH Key。
`
	ssh-keygen -t rsa -C "youremail@example.com"
`
	如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa（私钥）和id_rsa.pub（公钥）两个文件
第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
	这里包括title和key字段
##	添加远程库
`
git remote add origin git@github.com:fanerge/repositoryName.git
`
	把本地库与远程库进行关联（在本地库目录下进行）
	远程库的名字就是origin，这是Git默认的叫法
`
git push -u origin master
`
	把本地库的所有内容推送到远程库上
	其实git push 是把当前分支如 master 推送到远程
PS：由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。	
	以后推送只需下面命令：
`
	git push origin master
`	
##	从远程库克隆	
假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。
```
git clone git@github.com:fanerge/仓库名.git	
or
git clone <repo> <directory> // 克隆到指定的目录
cd 仓库名
ls	
```
PS：git支持多种协议 ssh、https等
#	分支管理
你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。	
##	创建与合并分支	
当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上
```
git checkout -b branchName	
	git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：	
	git branch branchName // 创建分支
	git checkout branchName // 切换到分支
git branch
	查看分支
```
PS：git branch 命令会列出所有分支，当前分支前面会标一个*号。
```
git checkout master
	dev分支的工作完成，我们就可以切换回master分支
git merge branchName
	git merge命令用于合并指定分支到当前分支（这里是将dev合并到master）。
```
PS：这个操作只能在主分支master上进行。
	Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
##	删除分支
`
	git branch -d branchName
`
##	解决冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容。
	假如 readme.txt 存在冲突。
第一步：打开 readme.txt 文件手动处理冲突
第二步：添加到暂存区 git add readme.txt
第三步：提交到当前分支 git commit -m '说明'
```
git log --graph --pretty=oneline --abbrev-commit
	用带参数的git log也可以看到分支的合并情况
	用git log --graph命令可以看到分支合并图。
```
##	分支管理策略
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。	
```
git checkout -b dev	
git add readme.txt 
git commit -m "add merge"	
git checkout master	
git merge --no-ff -m "merge with no-ff" dev	
	准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward。
	因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
```
##	分支策略
首先，master 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
所有人都在 dev 分支上开发，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
##	Bug分支
在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。	
使用场景，在开发过程中新接收到一个bug需要紧急处理。
```
git stash
	Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
```
现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。	
```
git checkout master	
git checkout -b issue-101
```
###	创建bug分支
修复bug之后（提交修复bug相关的代码）
```	
	git add readme.txt 
	git commit -m "fix bug 101"
	git checkout master
	git merge --no-ff -m "merged bug fix 101" issue-101
	git branch -d issue-101
```
现在需要切换到原来分支继续开发
```
	git checkout dev
	git status
	git stash list
```
PS：工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
一种 git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除
一种 git stash pop，恢复的同时把stash内容也删了
PS：修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
	当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。
##	Feature分支
添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
`
git branch -D branchName
`
	强行删除一个没有合并的分支
PS：开发一个新feature，最好新建一个分支；
	如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。	
##	多人协作
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。
`
git remote
`
	查看远程库的信息（origin）
`
git remote -v
`
	显示更详细的信息，显示了可以抓取（fetch）和推送（push）的origin的地址。如果没有推送权限，就看不到push的地址。
###	推送分支
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上
`
git push origin master
`
	master 表示你要往远程那个分支推送
PS：一般 master、dev 分支需要推送到远程库，其它分支不需要。
###	抓取分支
`
git branch --set-upstream dev origin/dev
`
	把本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接
`
git fetch
`
PS：相当于是从远程获取最新版本到本地，不会自动merge
`
git pull
`
PS：相当于是从远程获取最新版本并merge到本地（git fetch + git merge）
	把最新的提交从origin/dev抓下来（提交前需要拉取分支的最新代码）
手动处理冲突
```
git commit -m "说明"
git push origin dev	
```
	推送到远程dev分支
多人协作的工作模式的步骤：	
1.	首先，可以试图用git push origin branch-name推送自己的修改；
2.	如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3.	如果合并有冲突，则解决冲突，并在本地提交；
4.	没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

PS：如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。	
#	标签管理
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。	
##	创建标签
```
git branch
git checkout master
	切换到需要打标签的分支
git tag tagName
	打标签
git tag -a tagName 
	-a 选项意为"创建一个带注解的标签"（谁打的，什么时候打的）
git tag	
	查看所有标签
git tag tagName commitId
	给特定版本号打标签
git show tagName
	查看标签信息
git tag -a tagName -m "说明" commitId
	创建带有说明的标签，用-a指定标签名，-m指定说明文字
git tag -s tagName -m "说明" commitId	
	通过-s用私钥签名（PGP签名标签）一个标签
```
##	操作标签
`
git tag -d tagName
`
	如果标签打错了，也可以删除
PS：因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。	
```
git push origin tagName	
	推送某一个标签到远端
git push origin --tags
	推送全部尚未推送到远端的本地标签
git tag -d tagName	
git push origin :refs/tags/tagName	
	如果标签已经推送到远程，先本地删除，再远程删除。
```
#	自定义Git
```
git config --global user.name "fanerge"
git config --global user.email fanerge@example.com
	配置用户信息
git config --global color.ui true
	让Git显示颜色，会让命令输出看起来更醒目。
git config --global core.editor notepad++
	配置文本编辑器
git config --global merge.tool vimdiff	
	配置差异分析工具
git config --list	
	查看全部配置信息
git config configName	
	查看单个配置信息
```
##	忽略特殊文件
创建一个特殊的.gitignore文件，把需要忽略的文件名填进去就可。	
	在这个目录的文件
	[所有配置文件可以直接在线浏览](https://github.com/github/gitignore)
忽略文件的原则是：
	忽略操作系统自动生成的文件，比如缩略图等；
	忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
	忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
有时需要向git添加文件，但又添加不上，需要检查.gitignore哪里写错了
`
	git check-ignore -v fileName
`	
暴力向git添加文件
`
	git add -f fileName // 不建议使用	
`
##	配置别名
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
```
git config --global alias.st status	
	为status 设置为 st 别名
	git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
##	配置文件	
每个仓库的Git配置文件都放在.git/config文件中。
cat .git/config 		
	查看本仓库的配置
##	搭建Git服务器
搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的apt命令就可以完成安装。
[具体步骤](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)
[管理公钥](https://github.com/res0nat0r/gitosis)	
[管理权限](https://github.com/sitaramc/gitolite)
#	速记手册
工作区：就是git版本管理的目录。
暂存区（stage或index）：git add添加到暂存区
当前分支：git commit将暂存区内容提交到当前分支
常用注释前缀：issue、bug、feature、fix、

把这个目录变成Git可以管理的仓库
	git init 
把文件添加到版本库（实际上就是把文件修改添加到暂存区）		
	git add fileName
	git add *.js // 通配符
	git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
	git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
	git add -A  提交所有变化（是git add .和git add -u的结合，git add -all的简写）
把文件提交到当前仓库（实际上就是把暂存区的所有内容提交到当前分支）	
	git commit -m "本次提交注释"
	
当前版本设置（跳转版本，HEAD为当前版本的指针）
	git reset --hard HEAD^ // 回到上一个版本	
	git reset --hard HEAD~100 // 回到前100个版本
	git reset --hard commitId // 回到commitId的版本

撤销操作
	还没git add放入暂存区
		git checkout -- readme.txt 
	文件已经git add添加到暂存区（依次执行）
		git reset HEAD fileName.txt
		git checkout -- readme.txt
	文件已经提交到当前版本（版本回退）
		git reset HEAD^
	文件已经提交到远程库
		无力回天
查看当前仓库的状态
	git status	
	
删除文件
	第一步：
	rm fileName.txt
	第二步：
	git rm fileName.txt // 告诉git确实要删除
	or
	git checkout -- fileName.txt // 删错了想恢复文件 

本地分支代码推送到远端
	git push -u origin master // 这里本地代码推送到了master主干上了
	git push origin master // 将当前分支推送到master分支
分支操作（新需求，主要是为了不影响其他人开发）
	git branch // 查看所有分支（*表示当前分支）
	git branch Name // 新建分支
	git checkout Name // 切换到分支
	or
	git checkout -b Name // 新建并切换到分支
	git branch -d Name // 删除Name分支
	git branch -D Name // 强行删除Name分支（危险）
	git checkout -b branch-name origin/branch-name // 新建并切换并建立本地分支和远程分支的联系分支
	git branch --set-upstream branch-name origin/branch-name // 设置本地的dev分支与远端的dev分支的链接
	<span style="color: red">git push origin dev:dev // 提交本地dev分支作为远程的dev分支</span>
远程分支操作
远程分支的新建和关联
	新建并切换至dev分支
	<span style="color: red">git checkout -b 'dev'</span>
	本地dev分支作用远程dev分支并推送至远端
	<span style="color: red">git push origin dev:dev</span>
	当前分支关联远程分支（dev->origin/dev）
	<span style="color: red">git branch --set-upstream-to=origin/dev</span>
合并分支
	git merge Name // 将Name分支合并到当前分支，直接把当前指向Name分支的当前提交（快进模式）	
	// Git就会在merge时生成一个新的commit（仅一个）
	 git merge --no-ff -m "部分注释" dev // 禁用Fast forward进行合并
处理冲突
	git pull // 拉最新代码，no tracking information表示本地分支和远程分支的链接关系没有创建
	<span style="color: red">git branch --set-upstream dev origin/dev // 设置本地的dev分支与远端的dev分支的链接</span>
	
储藏工作现场（用于临时处理bug但目前代码又不可提交的场景，等修复bug后再回来处理）
	git stash // 向储藏室添加工作现场，在其他分支上处理bug
	// 处理完bug，有回到之前的工作现场
	git stash list // 查看所有储藏室的工作现场
	// 恢复工作现场
	git stash apply // 只恢复
	git stash apply stash@{0} // 恢复某个工作现场
	git stash drop // 从储藏室删除
	or
	git stash pop
关联远程库
	git remote // 查看远程库的信息
	git remote -v // 更详细的信息
	git remote add origin git@server-name:path/repo-name.git
从远程clone	
	git clone git@github.com:fanerge/gitskills.git
	
标签管理
	git tag <name> // 为最新的commit打一个标签
	git tag // 查看所有标签
	git tag <name> commitId // 为某次commitId打标签
	git tag -a <tagname> -m "blablabla..." // 可以指定标签信息
	git tag -s <tagname> -m "blablabla..." // 可以用PGP签名标签
	git show <name> // 查看说明文字
	git tag -d <name> // 删除某个标签
	git push origin <tagname> // 推送某个标签到远程
	git push origin --tags // 一次性推送全部尚未推送到远程的本地标签
	// 如果标签在远程要删除
	git tag -d <name> // 删除某个标签
	git push origin :refs/tags/<name> // 删除一个远程标签
	
diff文件（difference）
	git diff fileName.txt
	git diff HEAD -- readme.txt // 查看工作区和版本库里面最新版本的区别
查看提交日志
	git log
	git log --pretty=oneline //
	git log --graph	// 查看分支合并图
	git log --graph --pretty=oneline --abbrev-commit // 查看分支合并情况
查看历史命令（用于回到未来版本）
	git relog

>	参考文档：
	[git 手册](https://shimo.im/docs/m2m3ZhdmJqgXY7JA)
	[git官网](https://git-scm.com/)
	[git - 简明指南](http://www.runoob.com/manual/git-guide/)
	[廖雪峰-git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013745374151782eb658c5a5ca454eaa451661275886c6000)
	[Git 教程](http://www.runoob.com/git/git-basic-operations.html)
	[Git fetch和git pull的区别](http://blog.csdn.net/hudashi/article/details/7664457)
	[git reflog](http://blog.csdn.net/ibingow/article/details/7541402)	
	[图解git](http://marklodato.github.io/visual-git-guide/index-zh-cn.html)