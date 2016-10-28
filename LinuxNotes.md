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
`nl [选项] [文件]`命令，计算文件中的行号，将输出的文件内容自动加上行号。

	参数：-b :指定行号指定的方式，主要有：-b a :表示不论是否为空行，也同样列出行号（类似cat -n）
	-b t :如果有空行，空的那一行不要列出行号
	-n ln :行号在荧幕的最左方显示
	-n rn :行号在自己栏位的最右方显示，且不加0；
	-n rz :行号在自己栏位的最右方显示，且加0；
	-w num:行号栏位的占用位数。
`more`命令，一页一页的显示文件内容，空格键往下翻页，按b键往回一页显示。

	`less [参数] filename` less是对文件或其他输出进行分页显示的工具。
	参数：
	-b <缓冲区大小> ：设置缓冲区的大小
	-e 	:当文件显示结束后，自动离开
	-f	:强迫打开特殊文件
	-g	:只标志最后搜索的关键词
	-i	:忽略搜索时的大小写
	-o <文件名>	：将less 输出的内容在指定文件中保存起来
查找命令

	which 	查看可执行文件的位置
	whereis 查看文件的位置
	locate	配合数据库查看文件位置
	find	实际搜寻硬盘查询文件名称
`find pathname -options [-print -exec -ok ...]` 用于在文件树中查找文件，并作出相应的处理。
	
	命令选项：
	-name	按照文件名查找文件
	-perm	按照文件权限来查找文件
	-user	按照 文件属主来查找文件
	-group	按照文件所属的组来查找文件
	-mtime -n +n 按照文件的更改时间来查找文件，-n表示文件更改时间距现在n天以内，+n是n天以前
	-size n:[c] 查找文件长度为n块的文件，带有c时表示文件长度以字节计。
tar命令和gzip命令

	tar [必要参数][选择参数][包名][文件名]
	-A	新增压缩文件到已存在的压缩
	-B	设置区块大小
	-c	建立新的压缩文件
	-d	记录文件的差别
	-r	添加文件到已经压缩的文件
	-u	添加改变了和现有的文件到已经存在的压缩文件
	-x	从压缩的文件中提取文件
	-t	显示压缩文件的内容
	-z	支持gzip解压文件
	-j	支持bzip2解压文件
	-Z	支持compress解压文件
	-v	显示操作过程
	可选参数：
	-f	指定压缩文件
	-C	切换到指定目录
	
	gzip [参数] [文件或目录]
	-a	使用ASCII文文字模式
	-c	把压缩后的文件输出到标准输出设备，不去变动原始文件
	-d	解开压缩文件
	-f	强行压缩文件
	-l	列出压缩文件的相关信息
	-v	显示指令执行过程
df命令

	df [选项] [文件] df命令的功能是用来检查Linux服务器文件系统的磁盘空间占用情况。
	-a	全部文件系统列表
	-h	方便阅读方式显示
	-i	显示inode信息
	-k	区块为1024字节
	-l	只显示本地文件系统
du命令

	du [选项] [文件] du命令查看文件和目录磁盘使用的空间
	-a	显示目录中所有文件的大小

`echo -e "包含转义序列的字符串"`  #双引号中使用

`echo -e "\e[1;31m this is red text \e[0m"`

env命令查看所有与终端相关的环境变量

查看进程的环境变量：cat /proc/$PID/environ

	var=value不同于var = value，前者是赋值操作，后者是相等操作
	length=${#var}  获取变量的长度
判断当前用户是不是root用户可以看UID这个环境变量是不是0，是0则表示是root用户

	let命令可以直接执行基本的算术操作，当使用let时，变量名之前不需要再添加$： let result=num1+num2
	操作符[]使用：result=$[num1+num2]
	使用(()): result=$((num1+num2))
	expr: result=`expr num1+num2`
bc是一个用于数学运算的高级工具，这个精度计算器包含了大量的选项

	echo "5 * 1.34" | bc
	
	no=54;
	result=`echo "$no * 1.5" | bc`
	echo $result
	
	设定小数精度：echo "scale=2;3/8" | bc
	进制转换：
	no=100
	echo "obase=2;$no" | bc   #输出1100100
	
	no=1100100
	echo "obase=10;ibase=2;$no" | bc  #输出100
使用tee实现既将数据重定向到文件，又提供一份重定向数据的副本作为后继命令的stdin，例子：要在终端中打印stdout，同时将它重定向到一个文件中，则使用`command | tee -a FILE1 FILE2` -a追加到文件

	打印数组的值：echo ${array[*]}  或者  echo ${array[@]}
	数组的长度：  echo ${#array[*]}
	
	声明关联数组：declare -A arr_name
	获取索引列表：echo ${!arr_name[*]}
alias可以创建别名

	tput cols    #获取终端的列数
	tput lines 	 #获取终端的行数
	tput longname	#打印中终端名
	
	date +%s   #显示当前时间的时间戳
	date "+%d %B %Y"   #以“日 月 年”的形式显示
	date -s "时间格式"   #设置时间

	调试：
	使用-x选项启用shell脚本的跟踪调试功能：bash/sh -x scriptname.sh