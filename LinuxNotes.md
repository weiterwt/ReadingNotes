##Shell知识

`source filename` 命令：

	Read and execute commands from filename in the current shell enviroment 
	and return the exit status of the last command executed from filename;
	
* 1 用户登录到Linux系统后，系统将启动一个用户shell。在这个shell中，可以使用shell命令或声明变量，也可以创建并运行shell脚本程序。

* 2 运行shell脚本程序时，系统将创建一个子shell。此时，系统中将有两个shell，一个是登录时系统启动的shell，另一个是系统为运行脚本程序创建的shell。

* 3 当一个脚本程序运行完毕，脚本shell将终止，返回到执行该脚本之前的shell。

export 功能说明：设置或显示环境变量。

	语　法：export [-fnp][变量名称]=[变量设置值]
	补充说明：在shell中执行程序时，shell会提供一组环境变量。export可新增，修改或删除环境变量，供后续执行的程序使用。export的效力仅限于该次登陆操作。
	参　　数：
	　-f 　代表[变量名称]中为函数名称。
	　-n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。
	　-p 　列出所有的shell赋予程序的环境变量。
***
##常用命令
`nl`命令，计算文件中的行号，将输出的文件内容自动加上行号。