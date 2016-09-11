#Git知识
###集中式vs分布式
集中式：集中式版本控制系统，版本库是集中放在中央服务器的。每次需要从中央服务器取得最新的版本，然后才开始干活，干完活了，再把自己的活推送给中央服务器。

分布式：分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库。只需要将自己的修改推送给别人，就可以看到相应的修改了。
***
###安装Git

在Linux上安装

	ubuntu ：sudo apt-get install git 
在Mac OS上安装

	通过homebrew安装Git
在Windows上安装

	msysgit是Windows版的Git，从https://git-for-windows.github.io下载
###配置：

	git config —-global user.name “Your Name”
	git config —-global user.email “email@com”
—-global参数，表示在这台机器上所有的Git仓库都会使用这个配置。

版本库又名仓库（repository），可以简单理解成一个目录
###使用
创建一个空目录：

	mkdir learngit
	cd learngit
	git init
Notepad++的默认编码设置为UTF-8 without BOM。

把文件添加到仓库：

	git add filename
	git commit -m “说明语句”
时光机穿梭

	git status 命令显示仓库当前的状态
	git diff filename 查看具体修改的内容
版本回退

	git log 命令查看提交历史记录  --prettu=oneline参数：只输出每次提交的一行信息：版本号+说明
Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上个版本就是HEAD^^，上100个版本写成HEAD~100
	
	git reset --hard HEAD^	回退到上一个版本
会退到上一个版本后，又想退回去，那么这个时候只能根据版本号来了，不然就没办法了  `git reset --hard 版本号`
可以使用`git reflog`命令查看每一条使用过的命令，可找回提交的版本号(commit id)

####工作区和暂存区
工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。Git的版本库里面存了很多东西，其中最重要的就是称为stage(或者加index)的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`

`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。


