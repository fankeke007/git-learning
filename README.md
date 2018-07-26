# git-learning
深入git常用操作

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

- git add <filename>
- git commit -m "operation notice"

其中删除文件可以由==git rm <filename> ==代替。

> 在仓库中添加一个abc.txt文件，其内容为：
>>




