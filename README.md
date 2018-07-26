# git-learning
深入git常用操作

参考：[git 教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 术语

要想理解git的一整套运行原理得先了解相关术语，理解相关术语后能更好的促进我们理解git的工作流。

![工作区、暂存区、版本库三者之间关系](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

### 工作区


### 暂存区

### 版本库

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

### 1.2添加文件，修改文件，删除文件

添加文件，修改文件，删除文件中均需要2步操作：

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


