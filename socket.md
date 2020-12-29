`socket`

```
int socket(int domain, int type, int protocol);

domain:
	AF_INET, AF_INET6, AF_LOCAL, AF_ROUTE
type:
	SOCK_STREAM, SOCK_DGRAM, ...
protocal:
IPPROTO_TCP, IPPTOTO_UDP, IPPROTO_STCP, IPPROTO_TIPC
返回值：Socket文件描述符
```

`BIND`绑定IP地址及端口

```
int bind(int socket, const struct sockaddr *addr, socketlen_t addrlen);
sockfd是调用socket的返回值

```

```
struct sockaddr {
	sa_family_t sin_family; //地址族，IPv4 或者IPv6
	char sa_data[14]; //14字节包含套接字中的目标地址和端口信息
};

struct sockaddr_in {
	sa_family_t sin_family;
	in_port_t sin_port;
	struct in_addr sin_addr;
};

struct in_addr {
	unint32_t s_addr;
}
```

`htons()` 作用是将端口号由主机字节序转换为网络字节序的整数值

`ntohl()`相反

`inet_addr()` 作用是将一个IP字符串转化为一个网络字节序的整数，用于sockaddr_in.sin_addr.s_addr.

inet_ntoa() 作用是将一个sin_addr结构体输出成IP字符串



网络字节序与主机字节序

* 主机字节序就是我们平常说的大端和小端模式
* Little-Endian就是低位字节序排放在低地址端，高位字节排放在内存的高地址端
* Big-Endian即使高位字节排放在内存的低地址端，低位字节排放在内存的高地址端
* 一般的来说地址高字节顺序在下
* 网络字节序： 四个字节的32bit值以0~7bit， 8~15bit，16~23bit, 最后24~31bit传输次序称作大端字节序

`LISTEN`

```
int listen(int sockfd, int backlog);
sockfd是调用socket() 返回的套接字文件描述符。
backlog是在进入队列中允许连接数目。
返回值：发生错误的时候返回-1,并设置全局错误变量errno
```

`ACCEPT()`

```
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
函数说明：accept()用来接受参数sockfd的socket连线
参数的socket必须先经bind（）,listen（）函数处理过，当有连线进来时accept() 会返回一个新的socket处理代码，之后的数据传送与读取都是由这个新的socket来处理。原来的参数sockfd的socket能继续使用accept来接受新的socket处理，连线成功时，参数addr所指的结构会被系统填入远程主机的地址参数，参数addrlen为sockaddr的结构体长度
返回值：成功则返回新的socket处理代码，失败返回-1
```

connect() 建立连接

```
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
sockfd 是系统调用
```

![image-20201229160627374](C:\Users\wangsheng\AppData\Roaming\Typora\typora-user-images\image-20201229160627374.png)

close() 关闭连接

send() 发送数据

```
int send(int sockfd, const void *msg, int len, int flags);
sockfd是你想发送的套接字描述符（或者是调用socket()或者是accept()返回的，msg是你想发送的数据的指针，len 是数据的长度，把flags设置为0就可以了）。
返回值：返回实际发送的数据的字节数
错误的时候返回-1，并设置errno
```

recv()接受数据

```
int recv(int sockfd, void *buf, int len, unsigned);
sockfd, 是要读的套接字描述符。
buf是要读的信息的缓冲
len是缓冲的最大长度
flags可以设置为0
返回值：实际读入缓冲的数据的字节数，错误时返回-1，并设置errno
```

ISP网络服务提供商

DSL

HFC 

FTTH 

以太网，局域网，交换机