#Python 2.7
Windows中设置PATH环境变量：`set path=%path%;C:\python27`

交互模式中，最近一个表达式的值赋给变量`_`

Python支持的数值类型：`int`,`float`,`Decimal`,`Fraction`,复数：使用后缀j或J表示虚数部分`(2+3j)`，从复数z中提取实部和虚部，使用z.real和z.imag。

字符串可使用单引号(`'...'`)或双引号(`"..."`)标识。`\`可以用来转义引号。

原始字符串：在第一个引号前面加上一个`r`,例如：r'C:\program\name'

字符串文本能够分成多行，一种 方法是使用三引号：`"""..."""`或`'''...'''`，这样行尾换行符会被自动包含到字符串中，可以在行尾加上`\`。

字符串可以由`+`操作符连接，可以由`*`表示重复：`3 * ‘i’ + ‘y’ ----> 'iiiy'`

Python没有单独的字符类型；一个字符就是一个简单地长度为1的字符串。

字符串的第一个字符索引为0，索引可以是负数，这将导致从右开始计算。注意：-0实际上就是0，不会导致从右开始计算。

切片可以获得一个子字符串：

	word = 'Python'
	word[0] = 'P'
	word[0:2] = 'Py'   #索引从0开始，但是不包括下标为2的字符
	word[:3] = 'Pyt'   #省略第一个索引默认为0，省略第二个索引默认为切片的字符串的大小
Python能够优雅地处理那些没有意义的切片索引：一个过大的索引值将被字符串实际长度所代替，当上边界比下边界大时就返回空字符串。

Python字符串不可以被更改，是不可变的。所以这样是错的：`word[0] = 'K'`

内置函数`len()`返回字符串的长度：`len(word)`

Unicode字符串：`u'Hello World!'`

列表(list)，其元素不必是同一类型：`mylist = ['Hello', 'World', 10, 5]`，列表也支持切片操作，所有的切片操作都会返回一个包含请求的元素的新列表，同时，列表也支持字符串一样的`+`操作。 `mylist[:] = []`清空列表。`len(mylist)`计算列表的长度。

列表允许修改：`mylist[0] = 5` ， `mylist.append(10)`在列表的末尾添加新的元素。

嵌套列表：

	a = ['h','l','o']
	b = [1, 2]
	x = [a, b]
生成斐波拉契子序列：

	a, b = 0, 1		#多重赋值
	while b < 10:
		print b,	#用逗号结尾可以禁止输出换行
		a, b = b, a+b
if语句

	if condition:
		...
	elif condition:
		...
	else:
		...
如果要在迭代过程中修改迭代序列，则可以使用迭代的复本进行：

	for value in words[:]:
		if len(value) > 5:
			words.insert(0, value)
`range()`函数，生成一个等差级数链表

	range(10):		[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
	range(5, 10):	[5, 6, 7, 8, 9]
	range(0, 10, 3):[0, 3, 6, 9]
循环可以有一个`else`子句；它在循环迭代完整个列表(对于for)后或执行条件为false(对于while)时执行，但循环被break中止的情况下不会执行。

pass语句什么也不做，用于那些语法上必须要有什么语句，但程序什么也不做的场合。

定义函数：`def func(parameter):` 下一行必须缩进

函数的实参总是传值调用(这里的值总是一个对象引用，而不是该对象的值)。

可以这样做：

	f = func
	f(value)   #调用func函数
没有`return`语句的函数会返回一个None值

关键字参数：函数可以通过关键字参数的形式来调用

	def parrot(value, state='a siff', action='voom', type='Blue'):
		...
	parrot(10)
	parrot(value=5, action='Voom')
	parrot(state='off', value=10)
引入一个形如`**name`的参数时，它接收一个字典，该字典包含了所有未出现在形式参数列表中的关键字参数。使用一个形如`*name`的形式参数，它接收一个元组，包含了所有没有出现在形式参数列表中的参数值。`*name必须在**name之前出现`。

Lambda形式：

	lambda a, b: a+b   #一个匿名函数，返回两个参数的和。
列表：

	list.append(x)	#把一个元素添加到链表的结尾，相当于list[len(list):]=[x]
	list.extend(L)	#将一个给定列表L中的所有元素添加到另一个列表中
	list.insert(i, x) #在指定位置插入一个元素
	list.remove(x)	#删除链表中值为x的第一个元素，如果没有，这返回一个错误
	list.pop([i])	#从链表的指定位置删除元素，并将其返回。参数[i]表示可选，而不是需要输入[]
	list.index(x)	#返回链表中第一个值为x的元素的索引，没有则返回一个错误
	list.count(x)	#返回x在链表中出现的次数
	list.sort(cmp=None, key=None, reverse=False) #对链表中的元素进行排序
	list.reverse()	#倒排链表中的元素
`filter(function, sequence) 返回一个sequence(序列)，包括了给定序列中所有调用function(item)后返回值为true的元素`
	
	def f(x): return x %3 == 0 or x % 5 == 0
	filter(f, range(2, 25))
`map(function, sequence) 为每一个元素依次调用function(item)并将返回值组成一个链表返回。`

`reduce(function, sequence) 返回一个单值，它是这样构造的：首先以序列的前两个元素调用函数function，再以返回值和第三个参数调用，依次执行下去`

字典以关键字为索引，关键字可以是任意不可变类型，通常用字符串或数值。一对大括号创建一个空的字典：`{}`
字典的主要操作是依据键来存储和取值，也可以用del来删除键：值对。

对一个字典执行`keys()`将返回一个字典中所有关键字组成的无序列表。

`dict()`函数可以直接创建字典：`dict([('a', 1), ('b', 2), ('c', 3)])`

在序列中循环时，索引位置和对应值可以使用`enumerate()`函数同时得到：

	for i, v in enumerate(['a', 'b', 'c']):
		print(i, v)
	0 a
	1 b
	2 c
同时循环两个或更多的序列，可以使用zip()整体打包：

	for value in zip(list1, list2):
		...
遍历字典时，使用`iteritems()`方法可以同时得到键和对应的值：

	dic = {'a':123, 'b':45}
	for k, v in dic.iteritems():
		print k, v
	a 123
	b 45
###模块
`sys`模块内置于所有的Python解释器

`dir()`函数用于按模块名搜索模块定义，返回一个排好序的字符串类型的存储列表，无参数调用时，返回当前定义的命名列表。

包通常是使用用“圆点模块名”的结构化模块命名空间。[参考](http://www.pythondoc.com/pythontutorial27/modules.html)

模块中除了包含函数定义外，也可以包含可执行语句，这些语句一般用来初始化模块，它们仅在第一次被导入的地方执行一次。

被导入的模块名会放入当前模块的全局符号表中。

###格式化输出：

	1、自己处理整个字符串，通过使用字符串切割和连接操作可以创建任何需要的输出形式
	2、使用str.format()方法
将值转化为字符串：`repr()或str()`函数，函数`str()`用于将值转化为适于人阅读的形式，而`repr()`转化为供解释器读取的形式。

`str.rjust()、str.ljust()、str.center()`对输出的字符串进行对齐操作。

`str.format()`的基本用法：`print 'We are the {} who say "{}!" '.format('knights', 'Ni')`

可以通过指明大括号中的数值来传入对象：`print '{0} and {1}'.format('spam', 'eggs')`或`print '{1} and {0}'.format('spam', 'eggs')`

也可以使用关键字参数：`print 'This {food} is {adjective}.'.format(food='spam', adjective='absolutely horrible')`

`print('The value of PI is approximately {0:.3f}.'.format(math.pi))`

###文件读写
函数`open()`返回文件对象：`open(filename, mode)`，可选的模式有：`'r'`只读，`'w'`只写，`'a'`追加，`'r+'`读写方式打开文件。

文件对象的方法：

	f = open('filename', 'r+')
	f.read(size)	#size可选，指定要读取的字符串长度， 文件末尾时，返回一个空字符串""
	f.readline()	#从文件中读取单独一行，结尾自动加上一个换行符'\n'
	f.readlines()	#返回一个列表，其中包含了文件中所有的数据行
	f.write(string)	#将string的内容写入文件，并返回写入字符的长度
	f.tell()	#返回一个整数，代表文件对象在文件中的指针位置，该数值计量了自文件开头到指针处的比特数
	f.seek(offset, from_what) #改变文件对象指针，from_what值为0表示自文件起始处开始，1表示自当前文件指针位置开始，2表示自文件末尾开始
	f.close()	#关闭文件并释放其占用的所有系统资源
用关键字`with`处理文件对象：`with open('filename', 'r') as f: read_data = f.read()`，这样文件用完后会自动关闭

标准模块`json`可以接受Python数据结构，并将它们转换为字符串表示形式，此过程称为序列化。

异常处理：`try: ... except ...:`
	
	import sys
	
	try:
	    f = open('myfile.txt')
	    s = f.readline()
	    i = int(s.strip())
	except IOError as e:
	    print "I/O error({0}): {1}".format(e.errno, e.strerror)
	except ValueError:
	    print "Could not convert data to an integer."
	except:
	    print "Unexpected error:", sys.exc_info()[0]
	    raise
`raise`语句允许程序员强制抛出一个指定的异常：比如`raise NameError('ThisErrors')`，要抛出的异常由raise的唯一参数标识，它必需是一个异常实例或异常类(继承自`Exception`的类)。

`try:... finally:...`不管有没有发生异常，finally子句都会执行。try语句经由break、continue或return语句退出也一样会执行finally子句。