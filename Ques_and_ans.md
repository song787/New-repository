

## 1. 网络

**1.1 IPV4跟IPV6比较；**



**1.2 网桥工作哪一层**

- 物理层：双绞线、中继器、集线器
- 数据链路层：网桥、交换机、网卡
- 网络层：路由器、

**网桥**工作在数据链路层的介质访问控制(MAC)子层上,用于在多个使用同一种通信协议的网段中传送数据包的设备

**1.3 OSI七层协议**

分别是-物理层、数据链路层、网络层、传输层、会话层、表示层、应用层；

- 应用层：为应用程序提供服务；
- 表示层：数据格式转化、数据加密；
- 会话层：建立、管理和维护会话；
- 传输层：建立、管理和维护端到端的连接；
- 网络层：IP选址与路由选择；
- 数据链路层：提供介质访问与链路管理；
- 物理层：通过物理介质传输比特流；

**1.4 Get 与 Post区别？**

- **GET**用于获取资源，**POST**用于传输实体主体；

- **参数方面：**GET 和 POST 的请求都能使用额外的参数，但是 GET 的参数是以查询字符串出现在 URL 中，而 POST 的参数存储在实体主体中。不能因为 POST 参数存储在实体主体中就认为它的安全性更高，因为照样可以通过一些抓包工具查看。因为 URL 只支持 ASCII 码，因此 GET 的参数中如果存在中文等字符就需要先进行编码。例如 `中文` 会转换为 `%E4%B8%AD%E6%96%87`，而`空格`会转换为 `%20`。POST 参数支持标准字符集。

- **安全方面：**安全的 HTTP 方法不会改变服务器状态，也就是说它只是可读的。GET 方法是安全的，而 POST 却不是，因为 POST 的目的是传送实体主体内容，这个内容可能是用户上传的表单数据，上传成功之后，服务器可能把这个数据存储到数据库中，因此状态也就发生了改变。

  安全的方法除了 GET 之外还有：HEAD、OPTIONS。

  不安全的方法除了 POST 之外还有 PUT、DELETE。

- **幂等性：**幂等性：同样的请求被执行一次与连续执行多次的效果是一样的，服务器的状态也是一样的。所有的安全的方法都是幂等的，在正确实现的情况下：GET、HEAD、PUT、DELETE等是幂等的，POST是非幂等的；

- **可缓存：**如果要对响应进行缓存，需要满足以下条件：

  - 请求报文的 HTTP 方法本身是可缓存的，包括 GET 和 HEAD，但是 PUT 和 DELETE 不可缓存，POST 在多数情况下不可缓存的。
  - 响应报文的状态码是可缓存的，包括：200, 203, 204, 206, 300, 301, 404, 405, 410, 414, and 501。
  - 响应报文的 Cache-Control 首部字段没有指定不进行缓存。

- 为了阐述 POST 和 GET 的另一个区别，需要先了解 XMLHttpRequest：

  > XMLHttpRequest 是一个 API，它为客户端提供了在客户端和服务器之间传输数据的功能。它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。这使得网页只更新一部分页面而不会打扰到用户。XMLHttpRequest 在 AJAX 中被大量使用。

  - 在使用 XMLHttpRequest 的 POST 方法时，浏览器会先发送 Header 再发送 Data。但并不是所有浏览器会这么做，例如火狐就不会。
  - 而 GET 方法 Header 和 Data 会一起发送。

**1.5 HTTP与HTTPs区别？**

1. 端口不同：HTTP使用的是80端口，HTTPS使用443端口；
2. HTTP（超文本传输协议）信息是明文传输，HTTPS运行在SSL(Secure Socket Layer)之上，添加了加密和认证机制，更加安全；
3. HTTPS由于加密解密会带来更大的CPU和内存开销；
4. HTTPS通信需要证书，一般需要向证书颁发机构（CA）购买

**1.6 HTTPs加密过程？**

- 加密：
  - 对称密钥加密：对称密钥加密（Symmetric-Key Encryption），加密和解密使用同一密钥。所以运算速度会非常快，但是无法安全的将密钥传输给通信方；
  - 非对称密钥加密：非对称密钥加密，又称公开密钥加密（Public-Key Encryption），加密和解密使用不同的密钥。公开密钥所有人都可以获得，通信发送方获得接收方的公开密钥之后，就可以使用公开密钥进行加密，接收方收到通信内容后使用私有密钥解密。
  - 区别：对称加密速度更快，通常用于大量数据的加密；非对称加密安全性更高（不需要传送私钥）
- HTTPS 采用混合的加密机制：
  - 使用非对称密钥加密方式，传输对称密钥加密方式所需要的 Secret Key，从而保证安全性;
  - 获取到 Secret Key 后，再使用对称密钥加密方式进行通信，从而保证效率。（下图中的 Session Key 就是 Secret Key）
- 数字签名：
  - 非对称密钥除了用来加密，还可以用来进行签名。因为私有密钥无法被其他人获取，因此通信发送方使用其私有密钥进行签名，通信接收方使用发送方的公开密钥对签名进行解密，就能判断这个签名是否正确。

**1.7 Https的连接过程 ***

1. 客户端向服务器发送请求，同时发送客户端支持的一套加密规则（包括对称加密、非对称加密、摘要算法）；
2. 服务器从中选出一组加密算法与HASH算法，并将自己的身份信息以证书的形式发回给浏览器。证书里面包含了网站地址，**加密公钥**（用于非对称加密），以及证书的颁发机构等信息（证书中的私钥只能用于服务器端进行解密）；
3. 客户端验证服务器的合法性，包括：证书是否过期，CA 是否可靠，发行者证书的公钥能否正确解开服务器证书的“发行者的数字签名”，服务器证书上的域名是否和服务器的实际域名相匹配；
4. 如果证书受信任，或者用户接收了不受信任的证书，浏览器会生成一个**随机密钥**（用于对称算法），并用服务器提供的公钥加密（采用非对称算法对密钥加密）；使用Hash算法对握手消息进行**摘要**计算，并对摘要使用之前产生的密钥加密（对称算法）；将加密后的随机密钥和摘要一起发送给服务器；
5. 服务器使用自己的私钥解密，得到对称加密的密钥，用这个密钥解密出Hash摘要值，并验证握手消息是否一致；如果一致，服务器使用对称加密的密钥加密握手消息发给浏览器；
6. 浏览器解密并验证摘要，若一致，则握手结束。之后的数据传送都使用对称加密的密钥进行加密

总结：非对称加密算法用于在握手过程中加密生成的密码；对称加密算法用于对真正传输的数据进行加密；HASH算法用于验证数据的完整性。

**1.7 子网掩码详细作用？**

主要是用来区分网络地址与主机地址；如果网络地址相同，则证明是在同一个网段，如果不同则需要借助路由的转发；

是由于IPv4的地址资源紧缺，做不到每个网络设备都能获得一个网络地址。可变长度的网络地址分配方式相比以前主类网络划分方式更加灵活，在有限的网址资源的情况下，提高网络地址的利用率，减少网络地址的浪费。而灵活的代价就是：网络地址可以改变长度，没有规律可循了，只能靠子网掩码来划分了，所以这就是子网掩码的用途：**区分网络地址和主机地址。**

在没有子网掩码之前，网络地址按照主类网络的方式进行区分，但是这种划分方式不够灵活，也很浪费地址资源，并且随着通信设备的普及，有上网需求的通信设备不断增加，并且online和offline之间的切换频繁，数量是随着时间变化而变化的。因此子网掩码的出现，在**划分地址资源**方面做出了改进。

**1.8 TCP\UDP的区别；**

1. TCP是面向连接的，UDP是无连接的；
2. TCP是可靠的，UDP不可靠；
3. TCP只支持点对点通信，UDP支持一对一、一对多、多对一、多对多；
4. TCP是面向字节流的，UDP是面向报文的；
5. TCP有拥塞控制机制，UDP没有。网络出现的拥塞不会使源主机的发送速率降低，这对某些实时应用是很重要的，比如媒体通信，游戏；
6. TCP首部开销（20字节）比UDP首部开销（8字节）要大；
7. UDP 的主机不需要维持复杂的连接状态表；

**1.9 TCP为什么可靠**

1. 数据包校验
2. 对失序数据包重新排序（TCP报文具有序列号）
3. 丢弃重复数据
4. 应答机制：接收方收到数据之后，会发送一个确认（通常延迟几分之一秒）；
5. 超时重发：发送方发出数据之后，启动一个定时器，超时未收到接收方的确认，则重新发送这个数据；
6. 流量控制：确保接收端能够接收发送方的数据而不会缓冲区溢出

**1.10 TCP三次握手**



**1.11 TCP四次挥手**



**1.12 epoll模型的工作原理？**



**1.13 网关的作用是什么**



**1.14 滑动窗口机制**



**1.15 拥塞控制机制；**



**1.16 DNS解析过程；**



**1.17 URL请求到渲染的过程；**



**1.18 ARP协议**



1.19 

## 2. 操作系统

**2.1 线程的实现方式？有什么区别？**

  C++ 静态局部变量 能保证线程安全 

  问了别的方式std::call_once或者静态成员变量，但是一般都会用静态局部变量 

  C++没有java的volatile能阻止指令重排，不能用DCL 

**2.2  进程通信方法**



**2.3  操作系统调度方法**



**2.4 基于优先级调度的方法存在什么问题**



**2.5 程序跟进程比较，是否一一对应**



**2.6 进程线程地址空间**



**2.7 虚拟内存 常驻内存 共享内存？实际内存怎么计算？（常驻-共享）**



**2.8 既然多优先级队列+时间片轮转调度这么好，为什么还会出现死机情况?**



**2.9 系统调用的过程** 



**2.10 产生死锁的原因** 



**2.11 进程与线程的区别是啥；**



**2.12 线程同步的方法；**



**2.13 消息队列；**



## 3. Linux

**3.1 常用的linux指令讲一下**  

## 4. 设计模式

**4.1 手写单例模式。线程安全；**

## 5. 数据库

**5.1 redis 的 zset 的底层数据结构** 



**5.2 redis 实现分布式锁**



**5.3 数据库索引**



**5.4 Redis的5大结构；**



**5.5 B与B+的区别，B+树高度低带来的好处是啥；**



5.6 



## 6. C11

**6.1 左值右值的区别**



**6.2 完美转发**



**6.3 C++ 4种cast 区别和主要使用场景**

## 7. 网络编程

**7.1 多进程和多线程**



**7.2 阻塞IO、非阻塞IO、异步IO**



**7.3 线程池；**

## 8. C++

8.1 malloc和new

  malloc分配内存 

  new是操作符重载 分配内存+构造函数 

8.2 构造函数和析构函数能不能是虚函数



8.3 模板与继承的对比，模板的缺点，模板是如何编译的

模板默认内联，且没有虚表指针，调用开销小 ；

编译慢、编译产物更占用内存 ；

 编译期碰到了就生成需要的代码，每个.o都会有用到的定义，所以链接的时候要去掉重定义，因此编的慢；

8.4 智能指针-STL三种（细、手写智能指针）；



8.5 浅拷贝与深拷贝



8.6 一个类A，里面有两个整型变量，一个成员函数，两个虚函数，问这个类占多大内存



8.7 空悬指针与野指针？怎么避免空悬指针？



8.8 NULL与nullptr差别



8.9 虚函数以及虚函数表  虚函数表原理



8.10 .cpp与.c编译时的区别



8.11 数组和链表的区别



8.12 重载与重写



8.13  C++ 的内存管理是怎么实现的



8.14 指针和引用



8.15 strlen和sizeof



8.16 static关键字



8.17 时间复杂度，如果一个算法的时间复杂度是O(1)代表的含义是什么



8.18 头文件重复的包含和重复的引用



8.19 struct与union有什么区别？ 



8.20 struct内存对齐原则有哪些？



8.21 inline关键字有什么作用？ 



8.22 内联函数与宏定义的区别？ 



**8.23 什么是内存泄漏？如何避免？** 

<u>为什么会存在内存泄漏问题？</u>

通常来说，一个线程的栈内存是有限的，通常来说是 8M 左右（取决于运行的环境）。栈上的内存通常是由编译器来自动管理的。当在栈上分配一个新的变量时，或进入一个函数时，栈的指针会下移，相当于在栈上分配了一块内存。我们把一个变量分配在栈上，也就是利用了栈上的内存空间。当这个变量的生命周期结束时，栈的指针会上移，相同于回收了内存。

由于栈上的内存的分配和回收都是由编译器控制的，所以在栈上是不会发生内存泄露的，只会发生栈溢出（Stack Overflow），也就是分配的空间超过了规定的栈大小。

而堆上的内存是由程序直接控制的，程序可以通过 malloc/free 或 new/delete 来分配和回收内存，如果程序中通过 malloc/new 分配了一块内存，但忘记使用 free/delete 来回收内存，就发生了内存泄露。

<u>避免方法：</u>

- 尽量使用智能指针，而不是手动地去管理内存
- 使用 std::string 来替代 char*。使用 std::string 无需关心内存管理，它已经很好地在内部实现了内存管理。
- 无论如何都不要使用一个裸指针，除非逼不得已（比如要用它来指向一个旧的库）
- 在 C++ 中最好的避免内存泄漏的方式是尽量少的使用 new 和 delete 函数，最好是0使用。当你确实需要动态内存的时候，构造一个 RAII 类来在析构的时候自动地 delete 所有动态申请的内存。 RAII 类在构造函数中申请内存，而在析构函数释放内存，所以这可以保证在 RAII 对象离开作用域的时候，自动地释放内存。
- 在你每次确实需要申请动态内存的时候，先写 new 和 delete 语句。然后再在中间添加你的功能模块！这样做确保你不会忘记释放内存！

**8.24 strcpy和memcpy的区别？**



8.25 为什么引入抽象基类和纯虚函数？



8.26 为什么传指针比传引用安全？



8.27 虚函数和纯虚函数有什么区别



8.28 构造函数与析构函数的调用顺序是怎样的



8.29 多态与底层；



8.30 HashMap的底层实现；HashMap是否是线程安全的；



## 9. 其他

9.1  软件工程常用开发模型？ 软件开发流程 



9.2 Utf-8几个字节，汉字呢？



9.3 Memory load



9.4 move，forward



9.5 动态链接库原理？静态链接库跟动态链接库异同比较？动态链接库实际存了哪些东西? 怎么引用动态链接库，什么是动态库、静态库？



9.6 gdb常用命令有什么



9.7 哈希冲突有哪些解决方法；



9.8 LRU如何实现的；



## 10 手撕算法

leetcode 92 反转链表从m到n

LFU leetcode 460

场景类算法题有依赖关系的进程启动管理

二分法求浮点数平方根，不得递归，精度要求0.001 
  反转链表 

多线程打印ABCD 

手写大小端转换函数 

手写socket断点续传文件

手撕智能指针

斐波那契数列 

设计一个数据结构 list:  rpush rpop lpush lpop index 五种方法的时间复杂度均为 O(1)

二叉树的最大路径和、二叉树最大和的路径；

判断链表是否有环

判断两个链表是否相交

DFS、BFS

建立一个双向链表；

整数转化成字符串；

单链表只遍历一次，要找到链表的中间位置要怎么做；

层次遍历二叉树的过程；

迭代二叉树的深度；

0-n-1中缺失的数字，两种方法；

二叉搜索树后序遍历；

K个一组反转链表；

IP地址字符串转换为32位整数；

两个有序数组，其中一个有足够空位，不使用额外空间排序到含空位数组中；

求二叉树两个节点的最小距离；

对大规模数据进行去重；

找到两个链表的首个公共节点；

寻找无序整数数组中第一个缺失的正数；

对给出一个数组中的每个元素求因数个数；