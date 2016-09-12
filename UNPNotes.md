#UNP
`SCTP`(流控制传输协议)：面向消息，能在所连接的端点之间提供多个流，每个流各自可靠地按序递送消息。一个流上某个消息的丢失不会阻塞同一关联其他流上消息的投递。

TCP选项：MSS选项。发送SYN的TCP一端使用这个选项通告对端它的最大分节大小(`maximum segment size`)，也就是它在本连接的每个TCP分节中愿意接受的最大数据量。

**`TIME_WAIT`** 状态，持续时间2MSL，MSL(`maximum  segment  lifetime`)最长分节生命期

套接字对：本地IP地址、本地TCP端口号、外地IP地址、外地TCP端口号

标识每个端点的两个值（IP地址和端口号）通常称为一个套接字。

IPv4套接字地址结构：`sockaddr_in   头文件<netinet/in.h>`

<pre>
struct  in_addr
{
	in_addr_t  s_addr;
}

struct  sockaddr_in
{
	uint8_t  			sin_len;
	sa_family_t  		sin_family;
	in_port_t    		sin_port;
	struct in_addr   	sin_addr;
	char  			sin_zero[8];
}
</pre>

主机字节序：某个给定系统所用的字节序称为主机字节序(`host  byte  order`)

网络字节序：网络协议指定一个网络字节序(`network  byte  order`)

通用套接字地址结构：
<pre>
struct  sockaddr
{
	uint8_t  		sa_len;
	sa_family_t  	sa_family;
	char   		sa_data[14];
}
</pre>
IPv6套接字地址结构
<pre>
struct  in6_addr
{
	uint8_t  s6_addr[16];     //128位的IPv6地址
}

#define    SIN6_LEN
struct  sockaddr_in6
{
	uint8_t  			sin6_len;
	sa_family_t 		sin6_family;   //地址族
	in_port_t			sin6_port;
	uint32_t			sin6_flowinfo;
	struct in6_addr	sin6_addr;
	uint32_t 			sin6_scope_id;
}
</pre>
IPv4的地址族是`AF_INET`，IPv6的地址族是`AF_INET6`。

新的通用套接字地址结构
<pre>
struct  sockaddr_storage
{
	uint8_t		ss_len;
	sa_family_t	ss_family;
	/*其他字段透明*/
}
</pre>

	uint16_t  htons(uint16_t  host16bitvalue);
	uint32_t  htonl(uint32_t  host32bitvalue);   返回：网络字节序的值
	uint16_t  ntohs(uint16_t  net16bitvalue);
	uint32_t  ntohl(uin32_t  net32bitvalue);   返回：主机字节序的值

`<string.h>`中`void  bzero(void *dest, size_t nbytes);`  把目标字节串中指定数目的字节置为0

<arpa/inet.h>中

函数名中p和n分别代表表达（presentation）和数值（numeric）

	int inet_pton(int family, const char* strptr, void* addrptr);
		返回：若成功则为1，若输入不是有效的表达格式则为0，若出错则为-1
	const char *inet_ntop(int family, const void *addrptr, char *strptr, size_t len);
		返回：若成功则为指向结果的指针，若出错则为NULL

为了执行网络I/O，一个进程必须做的第一件事情就是调用`socket`函数，指定期望的通信协议类型。

	<sys/socket.h>
	int socket(int family, int type, int protocol);
		返回：若成功则为非负描述符，若出错则为-1
<pre>	
family		说明
AF_INET		IPv4协议
AF_INET6	IPv6协议
AF_LOCAL	Unix域协议
AF_ROUTE	路由套接字
AF_KEY		密钥套接字

type		  	 说明
SOCK_STREAM	  	 字节流套接字
SOCK_DGRAM	  	 数据报套接字
SOCK_SEQPACKET	 有序分组套接字
SOCK_RAW		 原始套接字

protocol		说明
IPPROTO_CP		TCP传输协议
IPPROTO_UDP		UDP传输协议
IPPROTO_SCTP	SCTP传输协议
</pre>

TCP客户用connect函数来建立与TCP服务器的连接

	<sys/socket.h>
	int connect(int sockfd, const struct sockaddr *servaddr, socklen_t addrlen);   返回：若成功则为0，若出错则为-1
	sockfd是有socket函数返回的套接字描述符，第二个、第三个参数分别是一个指向套接字地址结构的指针和该结构的大小。

`bind`函数把一个本地协议地址赋予一个套接字

	int bind(int sockfd, const struct sockaddr *myaddr, socklen_t addrlen);
		返回：若成功则为0，若出错则为-1
对于IPv4来说，通配地址由常值`INADDR_ANY`来指定，其值一般为0，它告知内核去选择IP地址。

对于IPv6则应该使用：`serv.sin6_addr = in6addr_any;`   

系统预先分配`in6addr_any`变量并将其初始化为常值`IN6ADDR_ANY_INIT`. 头文件`<netinet/in.h>`中包含有`in6addr_any`的extern声明。

listen函数仅由TCP服务器调用，它做两件事情：

	1、当socket函数创建一个套接字时，它被假设为一个主动套接字，也就是说，它是一个将调用connect发起连接的客户套接字。listen函数把一个未连接的套接字转换成一个被动套接字，指示内核应接受指向该套接字的连接请求。调用listen导致套接字从closed状态转换到listen状态。
	2、listen函数的第二个参数规定了内核应该为相应套接字排队的最大连接个数。
`<sys/socket.h>   int listen(int sockfd, int backlog);`     
返回：若成功则为0，若出错则为-1。


内核为任何一个给定的监听套接字维护两个队列：

	1、未完成连接队列（incomplete connection queue）：已由某个客户发出并到达服务器，而服务器正在等待完成相应的TCP三路握手过程。这些套接字处于SYN_RCVD状态。
	1、已完成连接队列（completed connection queue）：每个已完成TCP三路握手过程的客户对应其中一项。这些套接字处于ESTABLISHED状态。
 
 

`accept`函数由TCP服务器调用，用于从已完成连接队列队头返回下一个已完成连接。如果已完成连接队列为空，那么进程被投入睡眠(假设套接字为默认的阻塞方式)。

	<sys/socket.h>   
	int accept(int sockfd, struct sockaddr *cliaddr, socklen_t *addrlen);     
		返回：若成功则为非负描述符，若出错则为-1.
	参数cliaddr和addrlen用来返回已连接的对端进程（客户）的协议地址。
在讨论accept函数时，我们称它的第一个参数为监听套接字（`listening  socket`）描述符，称它的返回值为已连接套接字（`connected  socket`）描述符。

每个文件或套接字都有一个引用计数，当进程调用`close`时，相应的引用计数减1，当引用计数为0时，才会关闭TCP连接或文件。

	close函数用来关闭套接字，并终止TCP连接。
	<unistd.h>   int  close(int sockfd);   返回：如成功则为0， 若出错则为-1
	如果想在某个TCP连接上发送一个FIN，那么可以改用shutdown函数以代替close。

`getsockname`函数返回与某个套接字关联的本地协议地址，`getpeername`函数返回与某个套接字关联的外地协议地址。

	<sys/socket.h>
	int getsockname(int sockfd, struct sockaddr *localaddr, socklen_t *addrlen);
	int getpeername(int sockfd, struct sockaddr *peeraddr, socklen_t *addrlen);
		返回：若成功则为0，若出错则为-1
需要这两个函数的理由：

	1、在一个没有调用bind的TCP客户上，connect成功返回后，getsockname用于返回由内核赋予该连接的本地IP地址和本地端口号。
	2、在以端口号0调用bind（告知内核去选择本地端口号）后，getsockname用于返回由内核赋予的本地端口号。
	3、getsockname可用于获取某个套接字的地址族。
	4、在一个以通配IP地址调用bind的TCP服务器上，与某个客户的连接一旦建立（accept成功返回），getsockname就可以用于返回由内核赋予该连接的本地IP地址。
	5、当一个服务器是由调用过accept的某个进程通过调用exec执行程序时，它能够获取客户身份的唯一途径便是调用getpeername。


回射服务器的实现：

	(1)客户从标准输入读入一行文本，并写给服务器；
	(2)服务器从网络输入读入这行文本，并回射给客户；
	(3)客户从网络输入读入这行回射文本，并显示在标准输出上。
 

###POSIX信号处理

`信号（signal）`就是告知某个进程发生了某个事件的通知，有时也称为软件中断(`software interrupt`)。

信号通常是异步发生的，也就是说进程预先不知道信号的准确发生时刻。

建立信号处置的`POSIX`方法就是调用`sigaction`函数，而`signal`函数早于`POSIX`出现，可以构造自己的`signal`函数，调用`sigaction`函数。

设置`僵死(zombie)`状态的目的是维护子进程的信息，以便父进程在以后某个时刻获取。这些信息包括：子进程的进程ID、终止状态以及资源利用信息(CPU时间、内存使用量等等)。

