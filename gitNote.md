# 1 git简介

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

# 2.版本控制

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
