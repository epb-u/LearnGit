# 1 git简介

什么是git？一个用于管理软件版本的软件

##  1.1 安装git

下载网站：https://git-scm.com/downloads

安装方法：选项全部选择默认

创建用户：

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

--global:在这台机器上的所有Git仓库都会使用这个配置，即无论这台电脑(操作系统)中的任何用户使用git命令，对外都表现为同一个用户名和邮箱

## 1.2 创建版本库

（1）创建方法：选择一个文件夹(最好是空的),然后使用init命令

```shell
git init
Initialized empty Git repository in 当前目录
#该命令会在当前目录下创建一个.git的隐藏目录，千万别自己手动改里面的东西
```

注意：需要在git bash或cmd执行该命令，powershell不行

（2）将文件添加到版本库

A）现在在工作区（.git存在的目录）创建一个txt文件，并写入两行文字，注意别用windows默认文本编辑器写。

B）git add 将文件存入暂存区，即文件不会直接存入版本库

```shell
git add fileName
#如果成功就不会有任何提示
```

note：可以通过tab键对文件名进行补全，使用通配符*可以将目录下的所有文件添加到暂存区

C）git commit 将所有暂存区的文件提交到版本库

```shell
git commit -m "提交说明"
```

# 2 版本控制

（1）当改变文件内容后，可以通过status命令查看当前状态，可以看到readme.txt文件被修改过了，但没有提交(commit)

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

（2）如果想知道具体有什么变化，可以用diff命令

```shell
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

## 2.1 版本回退

​	每一次commit都会保存一个快照，如果不小心弄乱了文件，或误删了文件，都可以从**最近的一次**commit恢复

（1）查看修改历史

```shell
$ git log
commit e42d004c72e26f3b45b98ffb37fb53a2dcc02bf7 (HEAD -> master)
Author:用户名<邮箱>
Date: 修改日期
	这次的提交说明
commit 0bc8ec8be27e8e0d94a94850c9a97e62c9f86ab6
以下类似
```

note1：--pretty=oneline参数可以简化信息，e42d004...这一大坨数字是由SHA-1算法计算出的版本号(commit ID)

note2:HEAD代表当前版本，HEAD^表示上一个版本，HEAD^^表示上上一个版本，同时可以用HEAD~2简写

note3:log内容查看结束后在键盘敲入q即可退出。

（2）回退版本

```shell
git reset --hard HEAD^
```

​	此时当前的版本会回到上一个版本0bc8e，同时在log中删除e42..这个版本，如果希望版本再次变成e42d,可以使用如下命令

```shell
git rest --hard e42
```

​	也就是说可以直接通过版本号变更当前版本，同时不需要把版本号写全，git会自动寻找，如果忘记了e42d..这个版本号，可以通过reflog命令查询

```shell
$ git reflog
```

​	reflog会输出所有你执行过的命令，可以在哪里看到commit时产生的版本号

## 2.2工作区和暂存区

（1）工作区

在文件系统中可以看到的目录即为一个工作区

（2）暂存区

​	在某个工作区中git init后产生的.git隐藏目录即为一个版本库，在.git目录中的index二进制文件就是暂存区(stage)，git add后文件暂存到这个地方。Git会自动创建一个**分支**master，git commit后文件全部提交到这个分支

​	.git隐藏目录中的HEAD文件中记载了master文件的位置，如refs/heads/master，而master文件记录了当前版本的版本号

（3）tracked和untracked

​	所有在工作区的文件都会被git管理，如果某个文件没有被add，那么文件状态为untracked，反之为tracked

## 2.3修改管理

​	git管理的是”修改“而不是文件，当文件被修改后，使用git add,再对文件进行更改，然后直接git commit，git就会提示用户，文件有变化但没有添加到暂存区(stage)。

## 2.4撤销修改

​	在word或其他文本编辑器中，ctrl+z就可以进行撤销操作，在git中可以使用两种方式进行撤销操作。

（1）手动撤销

​	手动把不想要的内容删掉，或者把不小心删除的内容重新写上去

（2）使用 git checkout

```shell
$ git checkout --fileName
```

​	当文件没有被添加到暂存区：文件回到最开始的状态

​	当文件已经被添加到暂存区：文件回到暂存区中的文件状态

（当git add readme.txt后，这一时刻的版本即为A。那么checkout就会让readme.txt回到A版本。如果没有git add,那么之前 git commit会生成一个版本B,checkout就会让readme.txt回到B状态）

## 2.5删除文件

​	在工作区删除没用的文件，可以鼠标右键点击删除，或在命令行使用rm指令，此时在git bash中使用rm命令即可将文件从版本库中删除

```shell
$ git rm fileName
```

​	然后正常进行git commit即可，如果希望撤销删除，就使用checkout，需要注意的是，checkout本质上是从.git隐藏目录中找之前的“副本”，如果一开始就没有把文件add+commit，那么checkout就不可能成功。

# 3 远程仓库

## 3.1 SSH设置

​	什么是ssh? 一种网络安全协议，在github仓库和本地仓库直接使用该协议加密并传输数据。SSH采用公钥加密技术，即主机和服务器同时存有公钥，主机携带私钥，每次进行通信时，使用私钥验证主机身份。

（1）创建SSH key

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
```

​	使用该命令后，在用户主目录下会产生.ssh隐藏文件夹，其中包含id_rsa和id_rsa.pub两个文件。前者是私钥，不能泄露给其他人，后者是公钥。

（2）在github中添加公钥

​	在设置中找到ssh keys界面，点击add SSH key,然后将id_rsa.pub中的内容复制进去。

​	如果有多台电脑，可以生成多个key,然后将各个key的公钥保存到github个人账户中。

## 3.2 添加远程库

​	在github创建一个新的仓库后，可以将该仓库克隆到本地，或者将本地的一个项目与之关联

```shell
git remote add origin git@github.com:epb-u/LearnGit.git
```

​	origin代表远程库，由于主机和服务器中保存的都是LearnGit这个项目，为了区分主机端和服务器端的仓库，所以会给远程端起名叫origin，这是默认叫法，也可以改成自己想要的

```shell
git branch -M main
git push -u origin main
```

分支概念在第四章详细介绍。

## 3.3 从远程库克隆

​	在github创建远程库之后，随便找个目录使用clone命令

```shell
git clone git@github.com:michaelliao/LearnGit.git
```

​	最终会在当前目录下生成LearnGit文件夹，里面会包含一个README.md，以及一个.git隐藏文件
