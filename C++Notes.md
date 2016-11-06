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
true和false是布尔类型的字面值，nullptr是指针字面值
```

