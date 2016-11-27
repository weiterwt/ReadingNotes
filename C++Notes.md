#C++笔记汇总
2016年5月22号
***

`sizeof` 操作符： 对指针做`sizeof`操作将返回存放指针所需的内存大小，若想获取该指针所指向对象的大小，则必须对该指针进行解引用`sizeof(*p)`，但即使p为NULL也可以。

当操作符的优先级相同时，由其结合性决定求解次序。算术操作具有左结合性，说明他们从左向右结合。

`int  *pi  =  new int;      //没有初始化`

`int  *pi  =  new int();   //初始化为0`

`delete  p`；删除指针后，该指针变成悬垂指针。悬垂指针指向曾经存放对象的内存，但该对象已经不再存在了。一旦删除了指针所指向的对象，立即将指针置为0。

`const  int  *pci  =  new  const  int(1024);   //必须初始化`

`C++`自动将枚举类型的对象或枚举成员转换为整型，其转换结果可用于任何要求使用整数值得地方。

通过`运行时类型识别(RTTI)`，程序能够使用基类的指针或引用来检索这些指针或引用所指对象的实际派生类型。

	dynamic_cast 操作符，将基类类型的指针或引用安全地转换为派生类的指针或引用。
	const_cast 将表达式的const性质转换掉
	static_cast 编译器隐式执行的任何类型转换都可以由它显示完成。
异常机制提供程序中错误检测与错误处理部分之间的通信。C++的异常处理中包括：

    1、throw表达式(throw expression)，错误检测部分使用这种表达式来说明遇到了不可处理的错误。可以说throw引发(raise)了异常条件。
    2、try块(try block)，错误处理部分使用它来处理异常。
    3、由标准库定义的一组异常类(exception class)，用来在throw和相应的catch之间传递有关的错误信息。

容器定义的类型别名：其中`value_type`(元素类型)、`reference`(元素的左值类型，是`value_type&`的同义词)、`const_reference`(等效于`const  value_type&`)使用这些，无须知道容器元素的真正类型，就能使用它，泛型编程时非常有用。

	c.erase(p)   删除迭代器p所指向的元素，返回一个指向被删除元素后面元素的迭代器，在删除之前保证p不是end迭代器
	c.clear()      删除容器c内的所有元素

####关联容器

    关联容器(associative  container)支持通过键来高效地查找和读取元素。
    map		关联数组：元素通过键来存储和读取
    set		大小可变的集合，支持通过键实现的快速读取
    multimap	支持同一个键多次出现的map类型
    multiset	支持同一个键多次出现的set类型
    
    pair类型在utility头文件中定义
    make_pair(v1, v2) 以v1和v2值创建一个新的pair对象，其元素类型分别是v1和v2的类型
    map<K, V>::key_type		在map容器中，用做索引的键的类型
    map<K, V>::mapped_type	在map容器中，键所关联的值得类型
    map<K, V>::value_type	一个pair类型，它的first元素具有const  map<K, V>::key_type类型，				而second元素则为map<K, V>::mapped_type类型。

map的带有一个键-值pair形参的insert版本返回一个值：包含一个迭代器和一个bool值的pair对象，其中迭代器指向map中具有相应键的元素，而bool值则表示是否插入了该元素。未插入为false

查找map中的元素：count和find，用于检查某个键是否存在而不会插入该键。

删除map中的元素：erase

set类型，set不支持下标操作符，而且没有定义`mapped_type`类型，`value_type`不是pair类型，而是与`key_type`相同的类型。

正如不能修改map中元素的键部分一样，set中的键也为const。在获得指向set中某元素的迭代器后，只能对其做读操作，而不能做写操作。

####泛型算法

	int  iSearchValue = 42;
	vector<int>::const_iterator  result  =  find(vec.begin(), vec.end(), iSearchValue);
	只要找到与给定值相等的元素，find就返回指向该元素的迭代器，否则，返回它的第二个迭代器。
	
	泛型算法   #include  <algorithm>  
	算术算法    #include <numeric>
	int  sum = accumulate(vec.begin(), vec.end(), 42);  sum为vec的元素之和再加上42

`find_first_of`函数：带有两对参数来标记两段元素范围，在第一段范围内查找与第二段范围中任意元素匹配的元素，然后返回一个迭代器，指向第一个匹配的元素。如果找不到匹配元素，则返回第一个范围的end迭代器。

	fill函数：fill(vec.begin(),  vec.end(),  0);  //reset  each  element  to 0
为了允许把程序拆分成多个逻辑部分来编写，C++语言支持分离式编译（`separate  compilation`）机制，该机制允许将程序分割为若干个文件，每个文件可被独立编译。

***

### C++ Primer 5

	Linux命令行，访问main函数返回值：echo $?
	Windows：echo %ERRORLEVEL%
	
	g++ 编译时加上：-std=c++0x 参数来打开对C++11的支持
```c++
while(std::cin >> value)	//当使用istream对象作为条件时，其效果是检测流的状态。如果流是有效的，即流未遇到错误，则成功。当遇到文件结束符(end-of-file)，或遇到一个无效输入时，istream对象的状态会变为无效。
文件结束符：windows中输入Ctrl+Z; Linux中输入Ctrl+D
```

```
long long 类型是在C++ 11中新定义的，最小占64bit
大多数计算机将内存中的每个字节与一个数字(被称为"地址address")关联起来。
字面值常量的形式和值决定了它的数据类型。
short类型没有对应的字面值
浮点型字面值是一个double类型
true和false是布尔类型的字面值，nullptr是指针字面值(c++11引入)
引用不是对象，没有实际的地址，所以不能定义指向引用的指针
NULL值也是为0，在头文件cstdlib中定义
```

```
int *p;
int *&r = p;    //r是一个对指针p的引用

默认情况下，const对象的被设定为仅在文件内有效，当多个文件中出现了同名的const变量时，其实等同于在不同文件中分别定义了独立的变量。 想在多个文件之间共享const对象，必须在变量的定义之前添加extern关键字。

用名词顶层const(top-level const)表示指针本身是个常量，而底层const(low-level const)表示指针所指的对象是一个常量。
const int ci = 3;
const int &r = ci;	//用于声明引用的const都是底层const

常量表达式(const expression)是指值不会改变并且在编译过程就能得到计算结果的表达式。

c++11新标准规定，允许将变量声明为constexpr类型以便编译器来验证变量的值是否是一个常量表达式。
constexpr int mf = 10;	//20是常量表达式
constexpr int sz = size();	//只有当size是一个constexpr函数时，才是一个正确的声明语句

const int *p = nullptr;
constexpr int *q = nullptr;
p是一个指向常量的指针，q是一个常量指针，说明：constexpr把它所定义的对象置为了顶层const
```

c++11新标准规定了一种新的方法：使用别名声明(alias declaration)来定义类型的别名：

```
using SI = Sales_item;	//SI是Sales_item的同义词
```

```c++
typedef char *pstring;
const pstring cstr = 0;	//cstr是指向char的常量指针
const pstring *ps;	//ps是一个指针，它的对象是指向char的常量指针
const是对给定类型的修饰。pstring实际上是指向char的指针，因此，const pstring就是指向char的常量指针，而非指向常量字符的指针。
```

```
使用auto类型说明符，让编译器替我们去分析表达式所属的类型。
auto i = 0, *p = &i;	//i是整型，p是整型指针
auto sz = 0, pi = 3.14;	//错误，sz和pi的类型不一致

auto一般会忽略掉顶层const
	const int ci = i, &cr = ci;
	auto b = ci;	//b是一个整数(ci的顶层const特性被忽略掉了)
	auto c = cr;	//c是一个整数(cr是ci的别名)
	auto d = &i;	//d是一个整型指针
	auto e = &ci;	//e是一个指向整数常量的指针
	auto &g = ci;	//g是一个整型常量引用
```

```
类型说明符decltype，它的作用是选择并返回操作数的数据类型
	decltype(f()) sum = x;	//不调用函数f，sum的类型就是函数f的返回类型
如果decltype使用的表达式是一个变量，则decltype返回该变量的类型(包括顶层const和引用在内)
	const int ci = 0, &cj = ci;
	decltype(ci) x = 0;		//类型为const int
	decltype)(cj) y = x;	//类型为const int&
	
	int i = 0, *p = &i;
	decltype(*p) c = i;		//c的类型是int&
	decltype((i)) d;	//错误，d是int&，必须初始化，给i加上了一个括号，得到引用类型
```

getline()函数的参数是一个输入流和一个string对象，函数从给定的输入流中读入内容，直到遇到换行符为止(注意换行符也被读进来了)，然后把所读的内容存入到那个string对象中去(注意不存换行符)。

```
string对象的size函数返回的是一个string::size_type类型的值
	string line;
	auto len = line.size();	//len的类型是string::size_type
cctype头文件中一些函数:
	ispunct(c)	当c时标点符号时为真
```

```c++
C++11：范围for语句
	string str("hello world!!");
	decltype(str.size()) punct_cnt = 0;
	for (auto c : str)
	{
    	if (ispunct(c))
          {
              ++punct_cnt;
          }
	}
```

通过下标访问不存在的元素所产生的错误就是`缓冲区溢出(buffer overflow)`

```
为了便于专门得到const_iterator类型的返回值，c++11引入了两个新函数：cbegin和cend
	vector<int> vec;
	auto itr = vec.cbegin();	//itr的类型是vector<int>::const_iterator
```

```
使用size_t类型操作数组的下标，size_t是一种机器相关的无符号类型，它被设计得足够大以便能表示内存中任意对象的大小。在cstddef头文件中定义

int ia[] = {0, 1, 2, 3, 4, 5};
auto ia2(ia);	//ia2是一个整型指针，指向ia的第一个元素
decltype(ia)返回的类型是由6个整数构成的数组
C++11新引入begin和end函数，定义在iterator头文件中
	int *beg = begin(ia);	//指向ia首元素的指针
	int *last = end(ia);	//指向数组尾元素的下一位置的指针
```

```
C风格字符串不是一种类型，而是为了表达和使用字符串而形成的一种约定俗成的写法。按此习惯写的字符串放在字符数组中并以空字符结束('\0')
不能用string对象直接初始化指向字符的指针，要使用c_str函数转换
	string s("hello");
	char *str = s;	//错误
	const char *str = s.c_str();	//正确
	c_str函数的返回值是一个c风格的字符串，也就是，这个函数返回结果是一个指针，该指针指向一个以空字符结束的字符数组。
```

```
int ia[2][3] = {{1,2,3}, {4,5,6}};
int (&row)[3] = ia[1];	//把row定义成一个含有3个整数的数组的引用
要使用范围for语句处理多维数组，除了最内层的循环外，其他所有循环的控制变量都应该是引用类型
```

```
当一个对象被用作右值的时候，用的是对象的值(内容)；当对象被用作左值的时候，用的是对象的身份(在内存中的位置)

如果表达式的求值结果是左值，decltype作用于该表达式(不是变量)得到一个引用类型。

逻辑运算符和关系运算符的返回值都是布尔类型。
```

```
后置递增运算符的优先级高于解引用运算符，因此*p++等价于*(p++)
```

```
sizeof运算符返回一个一条表达式或一个类型名字所占的字节数，所得值是一个size_t类型。sizeof运算不会把数组转换成指针来处理。

逗号运算符(comma operator)含有两个运算对象，按照从左向右的顺序依次求值，结果是右侧表达式的值。
```

```
强制类型转换
	static_cast  任何具有明确定义的类型转换，只要不包含底层const，都可以使用，当需要把一个较大的算术类型赋值给一个较小的类型时，static_cast非常有用。使用static_cast找回存在于void*指针中的值：
		void *p = &d;	//d的真正类型是double
		double *dp = static_cast<double*>(p);
		
	dynamic_cast  支持运行时类型识别
	
	const_cast  只能该表运算对象的底层const
		const char *pc;
		char *p = const_cast<char*>(pc);
	对于将常量对象转换成非常量对象的行为，称为"去掉const性质(cast away the const)"
	
	reinterpret_cast  通常为运算对象的位模式提供较低层次上的重新解释
		int *ip;
		char *pc = reinterpret_cast<char*>(ip);
	必须牢记pc所指的真实对象是一个int而非字符，如果把pc当成普通的字符指针使用就可能在运行时发生错误。
```

```
一个表达式，比如ival + 5, 末尾加上分号就变成了表达式语句。
复合语句(compound statement)是指用花括号括起来的语句和声明的序列，复合语句也被称作块(block)，一个块就是一个作用域。 
```

```
异常是指存在于运行时的反常行为，这些行为超出了函数正常功能的范围。
	throw表达式(throw expression),引发(raise)了异常
	try语句块(try block)，异常处理部分使用try语句块处理异常，catch子句结束。

那些在异常发生期间正确执行了"清理"工作的程序被称作异常安全(exception safe)的代码。
```

```
执行函数的第一步是(隐式地)定义并初始化它的形参。
函数声明也称作函数原型(function prototype).

void fcn(const int i){}
void fcn(int i){}	//错误，属于重复定义，因为顶层const会被忽略掉

void fcn(const int *p){}
void fcn(int *p){}   //正确，因为这个const是底层const
```

```
int main(int argc, char *argv[]);当使用argv中的实参时，一定要记住可选的实参从argv[1]开始；argv[0]保存程序的名字，而非用户输入。
```

```c++
为了编写能处理不同数量实参的函数，c++11新标准提供了两种主要的方法：如果所有的实参类型相同，可以传递一个名为initializer_list的标准库类型；如果实参的类型不同，可以编写一种特殊的函数，也就是所谓的可变参数模板。

还有一个特殊的形参类型(即省略符)，可以用它传递可变数量的实参，一般只用于与C函数交互的接口程序。

如果想向initializer_list形参中传递一个值的序列，则必须把序列放在一对花括号内:
	void ErrorMsg(initializer_list<string> ilst);
	使用：ErrorMsg({"hello", "okay"});
	
C++11新标准规定，函数可以返回花括号包围的列表：
	vector<string> process()
	{
        if (str.empty())
          return {};
  		else
          return {"Hello", str};
	}
```

```
int (*func(int i))[10];
	1、func(int i)表示调用func函数时需要一个int类型的实参
	2、(*func(int i))意味着我们可以对函数调用的结果执行解引用操作
	3、(*func(int i))[10]表示解引用func的调用将得到一个大小是10的数组
	4、int (*func(int i))[10]表示数组中的元素是int类型
	
在C++11新标准中还有一种可以简化上述func声明的方法，就是使用尾置返回类型(trailing return type)
	auto func(int i) -> int(*)[10]; //func接受一个int类型的实参，返回一个指针，该指针指向含有10个整数的数组
	
	int odd[] = {1,2,3,4,5};
	decltype(odd); //结果是一个数组，而不是指针
```

```
constexpr函数(constexpr function)是指能用于常量表达式的函数，返回类型及所有形参的类型都得是字面值类型。
	constexpr int new_sz()
	{
       return 30;
	}
constexpr函数不一定返回常量表达式。
```

```
assert是一种预处理宏(preprocessor marco)：
	assert(expr);
	首先对expr求值，如果表达式为假，assert输出信息并终止程序的执行。
assert宏定义在cassert头文件中。

assert的行为依赖于一个名为NDEBUG的预处理变量的状态，如果定义了NDEBUG，则assert什么也不做。默认状态下没有定义NDEBUG。
变量: __func__输出调试的函数的名字。编译器为每个函数都定义了__func__，它是const char的一个静态数组，用于存放函数的名字。
	__FILE__  存放文件名
	__LINE__  存放当前行号
	__TIME__  存放文件编译时间
	__DATE__  存放文件编译日期
```

```
函数指针指向的是函数而非对象，和其他指针一样，函数指针指向某种特定类型。函数的类型由它的返回类型和形参类型共同决定，与函数名无关。
	bool LengthCompare(const string &, const string &);
	该函数的类型是: bool(const string &, const string &)。
	
	bool (*pf)(const string &, const string &);  //pf就是一个指向函数的指针。
	
	当把函数名作为一个值使用时，该函数自动地转换成指针。
	pf = LengthCompare;
	pf = &LengthCompare;	//等价的
```

### 类

```
类的基本思想是数据抽象(data abstraction)和封装(encapsulation)
封装实现了类的接口和实现的分离

当调用一个成员函数时，用请求该函数的对象地址初始化this指针。

常量对象，以及常量对象的引用或指针都只能调用常量成员函数。

友元：如果类想把一个函数作为它的友元，只需要增加一条以friend关键字开始的函数声明语句即可。
```

































