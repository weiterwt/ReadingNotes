#Git知识
###集中式vs分布式
集中式：集中式版本控制系统，版本库是集中放在中央服务器的。每次需要从中央服务器取得最新的版本，然后才开始干活，干完活了，再把自己的活推送给中央服务器。

分布式：分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库。只需要将自己的修改推送给别人，就可以看到相应的修改了。

安装Git

在Linux上安装

	ubuntu ：sudo apt-get install git 
在Mac OS上安装

	通过homebrew安装Git
在Windows上安装

	msysgit是Windows版的Git，从https://git-for-windows.github.io下载
配置：

	git config —-global user.name “Your Name”
	git config —-global user.email “email@com”
—-global参数，表示在这台机器上所有的Git仓库都会使用这个配置。

版本库又名仓库（repository），可以简单理解成一个目录

创建一个空目录：

	mkdir learngit
	cd learngit
	git init
Notepad++的默认编码设置为UTF-8 without BOM。

把文件添加到仓库：

	git add filename
	git commit -m “说明语句”
