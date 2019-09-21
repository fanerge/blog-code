---
title: git常用语法汇总
date: 2018-10-31 21:42:02
tags: 'git'
categories: '代码管理'
copyright: true
---
本文不会介绍git的[工作原理](https://fanerge.github.io/2018/git%E4%BD%BF%E7%94%A8%E6%89%8B%E5%86%8C.html)，需要你对git有一定的了解。本文几乎罗列出了所有常用的git命令，可作为日常开发的查询手册。

#	git clone
git clone [remoteUrl] // clone远程库到当前目录（项目名为远程项目名，会自动设置origin为remoteUrl的引用）
git clone [remoteUrl] [本地项目名] // clone项目并会重命名为[本地项目名]
PS：自动设置本地 master 分支跟踪clone的远程仓库的 master 分支

#	git fetch
用于从另一个存储库下载对象和引用
git fetch // 取回远程主机所有分支的更新
git fetch [remoteUrl] // 取回远程主机[remoteUrl]的更新到本地仓库（不自动合并到工作区）
git fetch [upstream] // 取回远程主机的引用[upstream]的更新到本地仓库
git fetch [remoteUrl] [branchName] // 取回远程主机[remoteUrl]的分支[branchName]的更新
git fetch [remoteUrl|upstream] [远程分支名]:[本地分支名] // 将[远程分支]fetch到本地并命名为[本地分支名]

#	git checkout
用于切换分支或恢复工作区文件
git checkout - // 切到最近的一次分支
git checkout [branchName] // 切换到[branchName]分支
git checkout -b [branchName] // 创建并切换到[branchName]分支
git checkout -b [localBranch] [remoteUrl]/[remoteBranch] // 创建并切换到[localBranch]并追踪[remoteUrl]/[remoteBranch]
git checkout -m [branchName] // 如果你在错误分支中开发，但又不允许直接切换分支（因为本地有修改），git会帮我们将错误分支到代码合并到branchNane
git checkout \-\- [fileName] // 撤销工作区的指定文件的操作（没有通过git add添加到暂存区）
git checkout . // 撤销工作区的所有文件的操作
git checkout head \-\- [fileName] // 撤销文件到上次commit的时候（head指向上次commit）
git checkout -b [localBranchName] [upstream]/[remoteBranchName] // 以远程[remoteBranchName]为模板新建[localBranchName]并切换到该分支并追踪到[upstream]/[remoteBranchName]【2018-12-19】

#	git pull
用于从另一个存储库或本地分支获取并集成（git fetch + git merge FETCH_HEAD）
git pull [远程主机名] [远程分支名]:[本地分支名] 
git pull origin next:master // 取回origin主机的next分支，与本地的master分支合并
git pull origin next // 若想取回origin主机的next分支并与当前分支合并，可省略当前分支名
一旦当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名
git pull origin
如果当前分支只有一个追踪分支，连远程主机名都可以省略
git pull

#	git add
将文件内容添加到索引/暂存区
git add [path] // [path]可以是文件也可以是目录
git add . // 将所有修改添加到暂存区
git add -u [path] // 把[path]中所有跟踪文件中被修改过或已删除文件的信息添加到暂存区（省略<path>表示 . ,即当前目录）
git add -A [path] // 所有跟踪文件中被修改过或已删除文件和所有未跟踪的文件信息添加到暂存区（省略<path>表示 . ,即当前目录）
git add -i [path] // 查看已跟踪的文件是否有更改、是否有添加到暂存区

#	git status 
用于显示工作目录和暂存区的状态
git status -uno // 只列出所有已经被git管理的且被修改但没提交的文件
git status -s // \-\-short 格式化输出git status 

#	git commit
将暂存区当前内容与描述更改的用户和日志消息一起存储在新的提交中
git commit -a // 会对以已追踪的文件自动执行git add并commit（只会对已追踪的文件有效果）
git commit -m '注释' // 带注释的提交
git commit \-\-amend -m '注释'// 尝试重写提交（修改上次commit，如果提交内容没有更改，将使用本次提交注释覆盖上次提交注释）

#	git push
用于将本地分支的更新
git push [远程主机名] [本地分支名]:[远程分支名] // 将[本地分支名]推送到[远程主机名]的[远程分支名]（远程分支名不存在时则新建）
git push [远程主机名] :[远程分支名] // 省略本地分支，表示删除[远程主机名]的[远程分支名]
git push [远程主机名] -d [远程分支名] // 与上等价（\-\-delete）
git push [upstream] [branchName] // 推送[branchName]分支到远端
git push -a [远程主机名] // 将本地的所有分支都推送到远程主机（\-\-all）
git push [远程主机名] HEAD // 将当前分支推送到远程的同名分支
git push [远程主机名] \-\-tags // 推送所有标签
git push [远程主机名] [tagName] // 推送单个标签
git push [远程主机名] :[tagName] // 删除远程标签
git push [远程主机名] tag [tagName] // 将本地标签[tagName]推送到远端[remoteUrl]

#	git branch
查看（当前分支前会有星号）、创建、删除分支
git branch // 查看本地分支
git branch -r // 查看远端分支
git branch -a // 本地+远程分支列表（\-\-all）
git branch [branchName] // 新建[branchName]分支
git branch -v // 查看分支的最近commit及注释
git branch -vv // 查看本地
git branch -D [branchName] // 删除[branchName]分支（需要切换到要删除分支以外的分支）
git branch -m [oldBranchName] [newBranchName] // 重命名分支
git branch \-\-set-upstream-to [remoteUrl]/[branchName] // 为当前分支建立追踪关系，追踪为[remoteUrl]/[branchName]
git branch \-\-unset-upstream // 撤销本地分支与远程分支的追踪关系

#	git merge
用于将两个或两个以上的开发历史加入一起
git merge [branchName] // 合并branchName分支到当前分支的顶部
git merge -s ours [branchName] // 合并branchName分支到当前分支,并使用ours合并策略（该参数将强迫冲突发生时，自动使用当前分支的版本）
git merge -s theies [branchName] // 同上，但该参数将强迫冲突发生时，自动使用被合并分支的版本

#	git rebase
如在 dev 分支上执行：git rebase master
作用：该命令会把你的”dev”分支里的每个提交(commit)取消掉，并且把它们临时保存为补丁(patch)(这些补丁放到".git/rebase"目录中),然后把最新的“master”代码合并到“dev”分支，最后把之前临时保存的这些补丁应用到”dev”分支上。
git describe // 显示离当前提交最近的标签
##	需求（开发分支 dev 远程分支 remoteDev）
git checkout dev
Git rebase remoteDev
// 如果有冲突，解决冲突—循环
git add .
git rebase \-\-continue
git rebase \-\-abort // 在过程中可以终止rebase，恢复到rebase开始前的状态。
[git rebase 与 git merge](http://gitbook.liuhui998.com/4_2.html)

#	git log
用于显示提交日志信息
git log -1 // 查看最近一条commit记录
git log -n // 最近n次提交
git log \-\-oneline // 单行显示日志
git log \-\-pretty=oneline // 查看以前提交记录
git log \-\-no-merges // 显示整个提交历史记录，但跳过合并
git log [dirName]/[fileName] // 查看当前分支dirName目录下fileName文件的提交日志
git log \-\-since="2 weeks ago" \-\- [fileName] // 显示最近两周fileName文件的提交日志
git log \-\-name-status [branchName1]..[branchName2] // 显示branchName2分支尚未在branchName1分支中的提交
git log \-\-follow [fileName] // 显示fileName文件的更改信息，包括更名之前的提交
git log \-\-branches // 显示所有分支的提交
git log \-\-branches \-\-not \-\-remotes=origin // 显示所有分支的提交（但不包括本地追踪远程分支origin）
git log [remoteUrl]/[branchName] // 查看远端某分支的日志
git log [commitId] // 查看对应commitId的提交
git log \-\-author=[userName] // 查看属于userName提交的记录
git log -p // 查看提交历史并显示每次提交的内容差异
git log -p -2 // 最近两条
git log \-\-stat // 每次提交的简略的统计
git log \-\-pretty=[args] // 参数为 oneline：一行显示，还有short，full ，fuller

#	git shortlog
用于汇总git日志输出（commit次数+提交注释）
git shortlog -s // 汇总每位开发者commit次数
git shortlog -n // commit次数排名

#	git diff 
工作目录(Working tree)和暂存区域快照(index)之间的差异
git diff [fileName] // 比较当前文件和暂存区文件差异
git diff [commitId1] [commitId2] // 比较两次提交之间的差异
git diff [branch1] [branch2] // 比较两个分支之间的差异
git diff \-\-staged // 比较暂存区和版本库差异
git diff \-\-cached // 比较暂存区和版本库差异（git add后尚未git commit）
git diff \-\-stat // 仅仅比较统计信息
git diff HEAD // 自上次提交以来工作树中的更改
git diff [branchName] // 查看工作目录和某分支的差异
git diff HEAD^ HEAD // 比较上次提交和上上次提交
git diff // 查看未暂存的修改
git diff \-\-cached // \-\-staged 查看已暂存的修改

#	git rm && git mv
用于从工作区和索引中删除文件（git rm 删除文件可以被git记录下来，rm只是物理删除）
git rm \-\-cached [fileName] // 只是从暂存区中删除文件索引
git rm [fileName] // 工作区和暂存区同时删除文件
PS：其他参数-f为\-\-force -r为递归处理该目录下的所有文件
用于移动或重命名文件
git mv [fileName] [dirName] // 将文件[fileName]移动到目录[dirName]中去
git mv [oldFileName] [newFileName] // 重命名

#	git reset
作用是修改HEAD的位置，即将HEAD指向的位置改变为之前存在的某个版本（这个版本之后的commit都将消失）【2018-12-18更新】
git reset HEAD [fileName] // 撤销已经git add到暂存区的指定文件的操作（原理是重新取最后一次commit的内容）
git reset HEAD // 撤销已经git add到暂存区的操作
git reset HEAD~1 // 重置到上次commit
git reset [commitId] // 重置到commitId
git reset \-\-soft [commitId] // HEAD回退到commitId，暂存区和工作区不变
git reset \-\-mixed [commitId] // HEAD回退到commitId，暂存区改变，工作区不变（默认方式）
git reset \-\-hard [commitId] // HEAD回退到commitId，暂存区和工作区都将改变（非常危险）
[git reset soft,hard,mixed之区别深解](https://www.cnblogs.com/kidsitcn/p/4513297.html)

#	git revert
作用通过反做创建一个新的版本，这个版本的内容与我们要回退到的目标版本一样，但是HEAD指针是指向这个新生成的版本，而不是目标版本。
适用场景： 如果我们想恢复之前的某一版本（该版本不是merge类型），但是又想保留该目标版本后面的版本，记录下这整个版本变动流程，就可以用这种方法。
[Git恢复之前版本的两种方法reset、revert（图文详解）](https://blog.csdn.net/yxlshk/article/details/79944535)【2018-12-18更新】

#	git remote
git remote rename [shortOldName] [shortNewName]// 修改一个远程仓库的简写名（引用）
git remote rm [shortname] // 移除一个源
git remote show [remoteName] // 查看某一个远程仓库的更多信息
git remote // 查看远程仓库（origin代表你本地clone的远程地址）
git remote -v // 查看远程仓库（带fetch、push地址）
git remote add [shortName] [url] // 添加远程仓库（shortname为以后的引用名）
git remote set-url [shortOldName] [url] // 修改源[shortOldName]的地址
git remote // 管理一组跟踪的存储库
git remote // 查询当前库的远程库
git remote -v // （\-\-verbose）查看库的远程fetch和push地址（前提是有对应权限）
git remote add [shortName] [remoteUrl] // 添加远程仓库[shortName]为其简短引用

#	git stash
适用于在处理需要较长时间的任务task1时又有紧急任务task2需要处理，可以通过git stash来保存本次的修改并将工作目录恢复到HEAD提交，等完成紧急任务task2后又继续之前的任务task1
git stash // 将当前任务存储起来（保存本地修改，并恢复工作目录以匹配HEAD提交）
git stash list // 查看已存储的任务列表
git stash apply stash@{2} // 应用已存储的任务列表的第2+1条
git stash drop stash@{2} // 从已存储的任务列表移除第2+1条
git stash pop // 取出已保存的最近任务（git stash apply + git stash drop）
PS：适用于在处理需要较长时间的任务时，有紧急任务需要处理在其他分支处理

#	git tag
用于创建，列出，删除或验证使用GPG签名的标签对象
git tag // 列出所有标签
git tag -a [tagName] -m [options] HEAD // 为当前HEAD创建标签
git tag -a [tagName] -m [options] [commitID] // 为某个commitID创建标签
git tag -l // 查看所有标签
git tag -d [tagName] // 删除某个标签
git tag -l '关键字' // 列出满足关键字的标签
git tag -v [tagName] // 查询tagName是否已经使用
git tag [tagName] // 创建轻量级标签（轻量级标签实际上就是一个保存着对应提交对象的校验和信息的文件，其实就是不带-a，-s 或 -m）
git tag -a [tagName] -m [说明] // 创建一个带说明的标签名为tagName的标签
git tag -a [tagName] [commitId] // 为某个commitId创建tagName
PS：实质tag保存在.git/refs/tags中

#	git submodule
用于初始化或更新或检查子模块
场景：基于公司的项目会越来越多，常常需要提取一个公共的类库提供给多个项目使用，下面介绍下基本使用步骤
克隆含有子模块的项目
git clone [remoteUrl] // clone 主项目
git submodule init // 初始化本地配置文件
git submodule update // clone相关的子模块
或者
git clone \-\-recursive [remoteUrl] // 先clone主项目，再递归clone子模块
为项目添加子项目
git submodule add [remoteUrl] // 添加子模块，[remoteUrl]为子模块远程地址
删除某个子项目
git rm \-\-cached [subModuleName]
rm -rf [subModuleName]
rm .gitmodules
vim .git/config
git commit -a -m 'remove [subModuleName] submodule'
[git submodule命令](https://www.yiibai.com/git/git_submodule.html#article-start)

#	git show
用于显示各种类型的对象
git show [tagName] // 查看相应标签的版本信息
git show [tagName]^{tree} // 显示标签[tagName]指向的树
git show -s \-\-format=%s [tagName]^{commit} // 显示标签[tagName]指向的提交主题
git show next~10:Documentation/README // 
git show [commitId] // 查看某次提交的内容

#	patch
git format-patch // 创建最新提交的修补程序
git format-patch [commitId] // 为指定[commitId]创建补丁
git apply [补丁标记id] // 使用补丁修改本地文件而不创建提交
git am [补丁标记id] // 使用补丁修改本地文件并创建提交 

#	git config
用于获取并设置存储库或全局选项
git config \-\-list // 查看所有的git配置
git config [key] // 查看特定项配置

#	重置分支
git branch -D [branchName] // 删除本地分支
git push [远程主机名] -d [远程分支名] // 删除远程分支
git fetch [远程主机名] master:[本地分支名] // 以远程的master作为本地的[本地分支名]
git push [远程主机名] [本地分支名] // 将本地分支推送到远端
git branch \-\-set-upstream-to [远程主机名]/[远程分支名]
git checkout -b [localBranchName] [upstream]/[remoteBranchName]

#	查端口所用PID，并kill【2019-02-01更新】
##	mac
lsof -i:port号 // 查端口所用PID
kill PID // 杀掉进程
##	window
netstat -aon | findstr port号 // 查端口所用PID
tasklist | findstr PID // 根据PID查进程
taskkill /pid  PID -t -f // 杀掉进程

#	其他
git init // 创建一个空的Git仓库或重新初始化一个现有仓库
git reflog // 查看所有分支的所有操作记录（包括commit和reset的操作）
git cherry-pick [commitHash] // 把某个分支的commit作为一个新的commit引入到你当前分支上 
git help // 查看帮助列表
git help [key] // 查看特定[key]相关帮助
git mergetool // 用于运行合并冲突解决工具来解决合并冲突
git blame [file] // 用来定位每一行代码的最后一次修改者
ifconfig // 查看ip地址等信息
ipconfig // 查看ip地址（window）

#	终端操作技巧
##	光标
Ctrl+a  光标移动到开始位置
Ctrl+e  光标移动到最末尾
##	删除
Ctrl+k  删除此处至末尾的所有内容
Ctrl+u  删除此处至开始的所有内容


>	参考文档：
[Git撤销&回滚操作](https://blog.csdn.net/ligang2585116/article/details/71094887)
[Git Cheat Sheet](https://shfshanyue.github.io/cheat-sheets/git)
[git-tips](https://github.com/git-tips/tips)


























































































