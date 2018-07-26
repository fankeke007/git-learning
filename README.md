# git-learning
深入git常用操作

参考：[git 教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 术语

要想理解git的一整套运行原理得先了解相关术语，理解相关术语后能更好的促进我们理解git的工作流。

![工作区、暂存区、版本库三者之间关系](https://raw.githubusercontent.com/fankeke007/git-learning/master/imgs/0.jpg)

### 工作区（Working Directory）

在电脑能看到的目录，在这里做的修改若不通过以下2步，是不会对版本库有影响的。
- git add（将修改添加到暂存区）；
- git commit（将修改添加到版本库）。

### 暂存区（Stage）

git版本库中的称为stage（或index）的区域。当变动通过git add 后，变动的内容就存放到了暂存区（此时还未到正式版本库，通过git commit之后才正式到版本库形成一个新的版本）。

### 版本库（Repository）

工作区有一个隐藏的目录.git，这个不算工作区，而是git的版本库。

git版本库里存了很多东西，其中最重要的就是称为stage的暂存区和git为我们自动创建的第一个master分支，以及指向master分支的一个指针HEAD。

## 1.命令行操作

> 情景案例：先在本地创建git仓库，后期在线上github（远程）创建同名git仓库，然后将本地git仓库与远程关联。

### 1.1创建本地git仓库

只需如下3步，即可创建一个本地仓库

	#1.创建项目文件夹
	mkdir git-learning
	#2.进入项目文件夹（工作目录）
	cd mkdir
	#3.创建本地仓库
	git init

### 1.2修改文件（添加，修改，删除）

添加文件，修改文件，删除文件中均需要2步操作：

	#可以一次add多个文件（夹），只需空格分开它们
	git add <filename>
	git commit -m "operation notice"

	#其中删除文件可以由 git rm <filename>代替。



> 在仓库中添加一个abc.txt文件，其内容为：
>> git is a distributed version control system.

>> git is free software distributed under the GPL.

>> git has a mutable index called stage.

>> git tracks changes of files.


	#创建文件abc.txt
	type nul>abx.txt
	#添加文件到暂存区
	git add abc.txt
	#添加文件到版本库
	git commit -m "add abc.txt"

### 撤销修改

假设在abc.txt 最后添加了如下修改：fankeke is a hansome man.

现在要撤销以上修改。

撤销修改的几种情况：
1. 修改的文件还在工作区，并未提交的暂存区：git checkout -- abc.txt ,结果是使得文件回到最近一次commit或add状态。
2. 修改已通过git add提交到暂存区:
3. 修改已通过git commit提交到版本库

