Redis数据库里面的每个键值对(key-value pair)都是有对象(object)组成的。

	* 1 数据库键总是一个字符串对象(string object)
	* 2 数据库键的值则可以是字符串对象、列表对象(list object)、哈希对象(hash object)、集合对象(set object)、有序集合对象(sorted set object)这五种对象中的其中一种
Redis自己构建了一种名为简单动态字符串(simple dynamic string, SDS)的抽象字符串类型。

	SDS的结构
	