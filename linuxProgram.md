#Linux 编程
###gcc
	gcc 常用选项
	-E	预处理，生成 .i文件
	-S	编译，生成 .s汇编文件
	-c	汇编，编译源码生成 .o目标文件
	-o	生成目标文件
	-Wall	使gcc对源代码有问题的地方发出警告
	-I[dir]	将dir加入头文件的搜索路径
	-L[dir]	将dir加入库文件的搜索路径
	-l[lib]	链接lib库
	-g	在目标文件中嵌入调试信息，以便gdb调试程序
	-O	优化编译后的代码
	-w	关闭所有警告信息
静态库(.a)：程序在编译链接的时候把代码连接到可执行文件中。程序运行的时候将不再需要静态库。
共享库(.so/.sa)：程序在运行的时候才去链接共享库的代码，多个程序可共享使用库的代码。

	生成共享库
	gcc -shared -fPIC file.o -o file.so   shared表示生成共享库，-fPIC表示生成位置无关码
	使用共享库
	gcc main.o -o main -L. -lfile    链接共享库，只要库名即可
###gdb
	启动gdb： gdb [-q] [executable-file] [core-file]
	break(b) n  #在第n行设置断点  b search.c:4   b file_name:fun_name
	run(r) 		#启动程序，在断点处暂停 run arg1 arg2 ...运行程序并加上参数
	step(s)		#单步跟踪，进入函数内部
	until		#跳出循环
	continue(c)	#继续运行，直到下一个断点
	watch expr 一旦expr值发生改变，程序就停止【设置观察点】
	ptype - 查看变量类型	#ptype 变量/字符串/数组/函数
	print *array@len  查看动态内存
	print x=10    动态改变运行时数据
	where命令/bt命令： 栈回溯，显示导致段错误的执行函数树
	wh命令	#查看程序代码窗口
	info functions	#列出函数的名字
	call func()   #直接调用函数执行，同print func()
	b (anonymous namespace)::func()		#给匿名空间的函数设置断点
	tbreak(tb)  file.c:20   #给程序设置一个临时断点，只会生效一次
	break ... if cond	#设置条件断点  b func:20 if i==10
	ignore bnum count	#接下来count次编号为bnum的断点都不会让程序中断，只有count+1次断点触发才会让程序中断。
	set print array-indexes on  #设置后，再print array 就可以打印出索引下标了
	set scheduler-locking on	#调试一个线程时，让其他线程暂停执行
	启动GDB时指定“-tui”参数，或者运行gdb过程中使用Ctrl+X+A组合键，都可以进入图形化调试界面

	GDB 多进程调试
	set args   设置进程参数
	set follow-fork-mode child ：to follow child processes
	set follow-fork-mode parent：to return to the default behavior
core文件调试：
	
	在程序崩溃时，一般会生成一个文件叫core文件。core文件记录的是程序崩溃时的内存映像，并加入调试信息。core文件生成的过程叫做core dump
	设置生成core文件
	ulimit -c    #查看core-dump状态
	ulimit	-c 数字  #ulimit -c 1024
	ulimit -a		#用于查看当前所有状态信息
	gdb	程序名	core-file   #调试core文件
###makefile
	makefile基本规则
	Target ...: Dependencies ...
			Command ...
	说明：
		目标(Target)：即最终想要产生的文件，如：可执行文件，目标文件或中间文件等；目标也可以是要执行的动作，如clean，也称为伪目标。
		依赖(Dependencies)：为了产生目标文件而依赖的文件列表，一个目标通常依赖于多个文件
		命令(Command)：是make执行的动作(shell命令或是可在shell下执行的程序，如echo)。每个命令行的起始字符必须为TAB字符！！！
		