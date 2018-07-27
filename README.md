# git-learning
深入git常用操作

参考：
[git 教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[git](https://git-scm.com/book/zh/v1/)
[git](https://www.yiibai.com/git/git_pull.html)


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

## 常用命令工具

### 查看状态（git status）

可以查看工作区，暂存区当前状态，会给出相应的提示操作。(是否有修改需要提交到暂存区，是否有暂存区需要提交到版本库)

	git status

### 查看历史版本（git log / git reflog）

	# 查看历史版本，但是回退后无法看到后面的版本
	git log
	#查看历史版本（所有，不管是否有回退）
	git reflog

### 版本回退

	git reset --hard[/soft/mixed] HEAD~xx[/HEAD@{xx}/commitId]

reset命令 hard、soft、mixed 3个参数的区别：
- mixed：是reset 命令的默认参数，会将版本库、stage回退到指定的commit版本，但是**这期间的修改保留在工作区**
- hard：将版本库，stage，工作区全都回退到指定版本，这会丢失stage、工作区中的修改
- soft：将版本库回退到指定版本，这期间（回退的两个版本之间）的变更都在stage中

版本参数的3中形式：

- HEAD~2：上两个版本
- HEAD@{3}：git reflog中显示的版本信息
- commitId : log 中显示的具体版本id （不需要写全，只需前几位，git会自动去匹配）

三者的区别见下图（参考：[git reset soft,hard,mixed之区别深解](https://www.cnblogs.com/kidsitcn/p/4513297.html)）

![reset mixed /hard /soft 三个参数的区别](https://raw.githubusercontent.com/fankeke007/git-learning/master/imgs/reset-params.jpg)


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
>> git is a distributed version control system.<br/>
>> git is free software distributed under the GPL.<br/>
>> git has a mutable index called stage.<br/>
>> git tracks changes of files.


	#创建文件abc.txt
	type nul>abx.txt
	#添加文件到暂存区
	git add abc.txt
	#添加文件到版本库
	git commit -m "add abc.txt"

### 1.3撤销修改（删除）

假设在abc.txt 最后添加了如下修改：fankeke is a hansome man.现在要撤销以上修改。

**撤销修改的3种情况：**
1. 修改的文件还在工作区，并未提交的暂存区：git checkout -- abc.txt ，结果是放弃工作区当前修改，使得文件回到最近一次commit或add状态。
2. 修改已通过git add提交到暂存区: git reset HEAD abc.txt，丢弃提交到暂存区的修改，该操作会**把修改回退到工作区**，然后执行第一种情况的操作，即可以彻底撤销修改。
3. 修改已通过git commit提交到版本库：（若还没提交到远程仓库）回退版本即可（若已提价到远程仓库，即使回退版本也会留下记录）。

**撤销删除**：
1. 在工作区删除，未将修改提交到暂存区：git checkout -- abc.txt
2. 删除（修改）已(git add/git rm)提交到暂存区:
	- git reset HEAD abc.txt
	- git checkout -- abc.txt
3. 删除已提交到版本库：回滚版本库即可

### 1.4建立远程仓库

在github,码云，gitlab或其他类似平台，按步骤操作即可(一般最好与本地仓库保持名称一致)。

### 1.5联结本地仓库与远程仓库

语法：git remote add [shorname] [url]
- shortname:可以指定一个远程仓库的名字，以便将来使用
- url ：远程仓库的地址

	#和远程仓库建立联结
	git remote add origin git@github.com:fankeke007/git-learning.git


### 1.6推送数据到远程仓库（git push）
语法：git push **\<远程主机名\> \<本地分支\>:\<远程分支\>**

	#将本地master分支推送到origin服务器的master分支，若master不存在会新建
	git push origin master

	#若省略本地分支名，则表示删除指定的远程分支(删除远程master分支)
	git push origin :master
	#等同于
	git push origin --delete master

	#若当前分支与远程分支间存在追踪关系，则本地分支与远程分支都可以省略
	# 将当前分支推送到远程对应分支
	git push origin

	#若当前分支只有一个追踪分支，那么主机名也可以省略
	git push

	#若当前分支与多个主机分支存在追踪关系，则可以使用-u参数指定默认主机，这样以后就可以不加任何参数使用git push
	git push -u origin master

	#不管是否存在对应分支，将本地所有分支都推送到远程origin主机上
	git push --all origin

	#强行推送
	git push --force origin

	#推送标签（tag）
	git push origin --tags



不带任何参数的git push ，默认只推送当前分支，这叫做**simple**方式。此外还有一种**matching**方式，会推送所有对应的远程分支的本地分支。git2.0之前默认matching，现在改为默认simple。若需修改这个设置，可以采用git config 命令：

	git config --global push.default matching




若在推送数据前已经有其他人推送了若干更新，则push会失败，会给出相应的操作提示。根据提示进行相应的操作即可。

### 1.7获取远程仓库数据并与本地指定分支合并 (git pull)

语法:git pull **\<远程主机名\> \<远程分支\>:\<本地分支\>**

	#取回origin主机的next分支，与本地master分支合并
	git pull origin next：master

	#取回origin主机的next分支，并与当前分支合并，则可以省略冒号及后面内容
	git pull origin next

	#若当前分支只有一个追踪分支（且当前分支与远程分支存在追踪关系），则远程主机名也可省略
	git pull

>**git pull = git fetch + git merge**<br/>
>**git pull --rebase = git fetch + git rebase**


>在某些场合，Git会自动在本地分支与远程分支之间，建立一种追踪关系(tracking)。比如，在git clone的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的master分支自动”追踪”origin/master分支。若当前分支与远程分支存在追踪关系，git pull 就可以省略远程分支名。 git 允许手动建立追踪关系：

	#指定master分支追踪origin/next 分支
	git branch --set-upstream master origin/next

### 1.8分支管理



#### 1.8.1创建分支

	#创建名为branch-1的分支
	git branch branch-1

#### 1.8.2切换分支

	#切换到branch-1分支
	git checkout branch-1

>创建和切换分支可以有一行命令搞定:<br/>git checkout -b branch-1

#### 1.8.3查看本地和远程分支

	#本地+远程
	git branch -a
	#远程
	git branch -r
	#本地
	git branch [--list]


#### 1.8.4修改分支名称

	git branch -m branch-1 branch-11

#### 1.8.5删除远程分支

	git push origin --delete branch-1
	#or
	git push origin :branch-1

#### 1.8.6合并某个分支到当前分支（master）

	git checkout master
	git merge branch-11

#### 1.8.7删除分支

	# 若 branch-11 还未merge，则会失败，需用-D强行删除
	git branch -d branch-11

#### 1.8.8建立本地与远程分支映射关系

	#完整写法
	git branch --set-upstream-to=origin/branchName  localBranchName
	#若是当前分支与远程分支映射，可简写
	git branch --set-upstream-to origin/branchName

	#撤销本地分支与远程分支的联系
	git branch --unset-upstream

#### 1.8.9查看本地与远程分支建的映射关系

	git branch -vv

### merge

参考：
[git-merge 完全解析](https://www.jianshu.com/p/58a166f24c81)
[一个成功的git分支模型](https://www.jianshu.com/p/b357df6794e3)












rebase:https://www.cnblogs.com/pinefantasy/articles/6287147.html
rebase:https://www.jianshu.com/p/4a8f4af4e803
