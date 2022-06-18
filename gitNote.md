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

(2)如果想知道具体有什么变化，可以用diff命令

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

