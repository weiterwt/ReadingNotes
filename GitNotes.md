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

	git log 命令查看提交历史记录  --pretty=oneline参数：只输出每次提交的一行信息：版本号+说明
Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上个版本就是HEAD^^，上100个版本写成HEAD~100
	
	git reset --hard HEAD^	回退到上一个版本
会退到上一个版本后，又想退回去，那么这个时候只能根据版本号来了，不然就没办法了  `git reset --hard 版本号`
可以使用`git reflog`命令查看每一条使用过的命令，可找回提交的版本号(commit id)

####工作区和暂存区
工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。Git的版本库里面存了很多东西，其中最重要的就是称为stage(或者加index)的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`

`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

`git dirr HEAD -- filename`命令可以查看工作区和版本库里面最新版本的区别

####撤销修改

`git checkout -- filename`可以丢弃工作区的修改，有两种情况：

* 1 修改后还没有add到暂存区，撤销修改回到和版本库一模一样的状态；
* 2 已经添加到暂存区后，又作了修改，撤销修改就回到添加到暂存区后的状态。

`git checkout -- filename`命令中的`--`很重要，没有`--`就变成了“切换到另一个分支”的命令了。

想要把提交到了暂存区的修改撤销掉，可以使用命令`git reset HEAD filename`，修改将重新放回工作区。
####删除文件
先将工作区里面的文件删除掉，再使用`git rm`和`git commit`命令

##远程仓库
创建SSH Key： `ssh-keygen -t rsa -C "youremail"` 这样在.ssh目录下，有id_rsa和id_rsa.pub两个文件，id_rsa是秘钥，id_rsa.pub是公钥。

本地仓库执行：`git remote add origin git@github.com:Github_name/repository_name` 添加后，远程仓库的名字就是origin

本地库内容推送到远程库上：`git push -u origin master` 把当前的分支master推送到远程，第一次推送master分支时，加上了`-u`参数，Git不但会把本地的master分支内容推送给远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令了。`git push origin master`

删除本地仓库关联的远程仓库：`git remote rm origin`

从远程仓库克隆

从远程仓库克隆到本地：`git clone git@github.com:GithubName/repositoryName`

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

###分支
创建分支

	git checkout -b branch_name   创建了一个名为branch_name的分支，-b参数表示创建并切换
	上面一条可以拆分为：git branch branch_naem   git checkout branch_name
	使用git branch 命令查看当前分支
当要合并分支时，需要先切换到合入的分支，如：将`test`分支的修改内容合并到`master`分支，则做如下操作：

	git checkout master
	git merge test
`git branch -d branch_name` 删除分支

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。用`git log --graph`命令可以看到合并图。

合并分支时，加上`--no-ff`参数（屏蔽掉了`fast forward`模式）（`git merge --no-ff -m "说明" branch_name`）就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并。

Bug分支：有需要快速解决的Bug，但是当前工作不能提交，这时可以使用`git stash`功能，把当前工作现场“储藏”起来，因此可以放心地创建分支来修复BUG。

`git stash list`命令查看工作现场存储情况。恢复工作现场：`git stash apply`恢复后，stash内容并不删除，需要使用`git stash drop`来删除；使用`git stash pop`，恢复的同时把stash内容也删了。

丢弃一个没有被合并过的分支，使用`git branch -D branch_name`强行删除。

`git remote -v`查看远程仓库的信息

`git pull`从远程抓取分支

`git branch --set-upstream branch-name origin/branch-name` 建立本机分支和远程分支的关联。

`git checkout -b branch-name origin/branch-name` 在本地创建和远程分支对应的分支。

##配置别名
git config --global alias.st status   使用st代替了status

当前用户的配置文件是~/.gitconfig

本地有修改提交了，远程仓库有修改，这时`git pull`时会有冲突，可使用`git merge origin/master`合并，再手动解决冲突

参考[廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)