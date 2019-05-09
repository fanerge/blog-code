---
title: linux备忘录
date: 2019-05-09 17:38:41
tags: 'linux'
categories: '服务端'
copyright: true
---
# 常用操作
##  public key
也就是你本地的 id_rsa.pub 文件，将文件内容添加到远程下列目录中
/root/.ssh/authorized_keys
##  yum
yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令。
yum [options] [command] [package ...]
options：可选，选项包括-h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
command：要进行的操作。
package操作的对象。
1.列出所有可更新的软件清单命令：yum check-update
2.更新所有软件命令：yum update
3.仅安装指定的软件命令：yum install <package_name>
4.仅更新指定的软件命令：yum update <package_name>
5.列出所有可安裝的软件清单命令：yum list
6.删除软件包命令：yum remove <package_name>
7.查找软件包 命令：yum search <keyword>
8.清除缓存命令:
yum clean packages: 清除缓存目录下的软件包
yum clean headers: 清除缓存目录下的 headers
yum clean oldheaders: 清除缓存目录下旧的 headers
yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的headers

# 文件
##  ls
显示目录
ls 目录名称
ls 无参数时，显示当前目录下的文件
ls / 显示根目录下的文件
### 显示一个文件的属性以及文件所属的用户和组
ls -l
前0～9位分别代表
0代表文件类型，d为目录，-为文件，l为链接文档等
1～3代表属主权限user（rwx分别代表read、write、execute，没有该权限则-表示）
4～6代表属组权限group
7～9代表其他用户权限others
### 查看全部文件
ls -a
### 仅列出目录本身，而不是列出目录内的文件数据(常用)
ls -d
##  cd
Change Directory
切换目录
cd 相对路径或绝对路径
##  pwd
Print Working Directory
显示目前所在的目录
### pwd [-P]
-P ：显示出确实的路径，而非使用连结 (link) 路径。
## mkdir
make directory
创建新目录
### mkdir [-mp] 目录名称
-m ：配置文件的权限
mkdir -m 711 test2 
-p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！
mkdir -p test1/test2
##  rmdir
删除空的目录
-p ：连同上一级『空的』目录也一起删除
##  cp [-adfilprsu] 来源档 目标档
拷贝文件和目录
##  rm
rm [-fir] 文件或目录
-f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
-i ：互动模式，在删除前会询问使用者是否动作
-r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！
##  mv
移动文件与目录，或修改名称
mv [-fiu] source destination
-f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖
-i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
-u ：若目标文件已经存在，且 source 比较新，才会升级 (update)
##  chgrp
chgrp [-R] 属组名 文件名
更改文件属组，-R代表递归操作目录中的所有文件和目录
##  chown
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
更改文件属主，也可以同时更改文件属组
##  chmod
更改文件9个属性
Linux文件属性有两种设置方法，一种是数字，一种是符号。
chmod [-R] xyz 文件或目录 // xyz分别代表权限值，x为user，y为grounp，z为others，r(4)w(2)x(1)
chmod u=rwx,g=rx,o=r  文件或目录
[chmod](https://www.runoob.com/linux/linux-file-attr-permission.html)

#  Linux 文件内容查看
##  cat
由第一行开始显示文件内容
cat [-AbEnTv]
##  tac
文件内容从最后一行开始显示，tac与cat命令刚好相反。
tac 文件路径
##  nl
查看文件显示行号
nl [-bnw] 文件路径
##  more
一页一页翻动查看文件
more 文件路径
##  less
一页一页翻动查看文件（可以往前翻）
less 文件路径
less运行时可以进行下列操作
/字串     ：向下搜寻『字串』的功能；
?字串     ：向上搜寻『字串』的功能；
n         ：重复前一个搜寻 (与 / 或 ? 有关！)
##  head
取出文件前面几行
head [-n number] 文件 
显示前number行
##  tail
取出文件后面几行
tail [-n number] 文件 
-n ：后面接数字，代表显示几行的意思
-f ：表示持续侦测后面所接的档名，要等到按下[ctrl]-c才会结束tail的侦测
看服务端日志用的比较多： tail -nf 100 文件

# 用户和用户组管理
##  useradd
添加新的用户账号
useradd 选项 用户名
-c comment 指定一段注释性描述。
-d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
-g 用户组 指定用户所属的用户组。
-G 用户组，用户组 指定用户所属的附加组。
-s Shell文件 指定用户的登录Shell。
-u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。
如：useradd -d /usr/sam -m sam
##  userdel
删除帐号，将/etc/passwd等系统文件中的该用户记录删除，必要时还删除用户的主目录。
userdel 选项 用户名
-r 它的作用是把用户的主目录一起删除。
##  usermod
修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。
usermod 选项 用户名
##  passwd
指定和修改用户口令
passwd 选项 用户名
-l 锁定口令，即禁用账号。
-u 口令解锁。
-d 使账号无口令。
-f 强迫用户下次登录时修改口令。
##  groupadd
增加一个新的用户组
groupadd 选项 用户组
如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。
-g GID 指定新用户组的组标识号（GID）。
-o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。
##  groupdel
删除一个已有的用户组
groupdel 用户组
##  groupmod
修改用户组的属性
roupmod 选项 用户组
-g GID 为用户组指定新的组标识号。
-o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。
-n 新用户组 将用户组的名字改为新名字。
##  newgrp
如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限。
newgrp groupName
##  其他
/etc/passwd文件是用户管理工作涉及的最重要的一个文件。
/etc/group文件记录的是用户所属的用户组。
/etc/shadow
主目录：一般是用户登录后的目录，各用户的主目录都被组织在同一个特定的目录下，而用户主目录的名称就是该用户的登录名。
[添加批量用户](https://www.runoob.com/linux/linux-user-manage.html)

# 磁盘管理
##  df
检查文件系统的磁盘空间占用情况。
df [-ahikHTm] 目录或文件名
-a ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
-k ：以 KBytes 的容量显示各文件系统；
-m ：以 MBytes 的容量显示各文件系统；
-h ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
-H ：以 M=1000K 取代 M=1024K 的进位方式；
-T ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
-i ：不用硬盘容量，而以 inode 的数量来显示
##  du
对文件和目录磁盘使用的空间的查看。
du [-ahskm] 文件或目录名称
-a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
-h ：以人们较易读的容量格式 (G/M) 显示；
-s ：列出总量而已，而不列出每个各别的目录占用容量；
-S ：不包括子目录下的总计，与 -s 有点差别。
-k ：以 KBytes 列出容量显示；
-m ：以 MBytes 列出容量显示；
##  fdisk
Linux 的磁盘分区表操作工具
fdisk [-l] 装置名称
-l ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。
##  mkfs
磁盘分割完毕后自然就是要进行文件系统的格式化，格式化的命令非常的简单，使用 mkfs（make filesystem） 命令。
mkfs [-t 文件系统格式] 装置文件名
##  fsck
file system check
用来检查和维护不一致的文件系统
fsck [-t 文件系统] [-ACay] 装置名称
##  mount
磁盘挂载与卸除
mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n]  装置文件名  挂载点

> 参考文档
[linux启动流程](http://www.ruanyifeng.com/blog/2013/08/linux_boot_process.html)
[runoob/linux](https://www.runoob.com/linux/linux-system-boot.html)





































