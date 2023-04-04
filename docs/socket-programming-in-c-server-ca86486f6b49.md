# C-Server 中的套接字编程

> 原文：<https://blog.devgenius.io/socket-programming-in-c-server-ca86486f6b49?source=collection_archive---------5----------------------->

![](img/cdd7d90157f09b3bf73f8119926e213b.png)

每天，我们都会向不同的服务器发送大量的请求，并收到相同的响应。但是你有没有想过这种交流是如何在引擎盖下进行的？？让我们去发现它！

今天，有大量的框架用于开发服务器(基本上是后端 API ),如 Flask、Django、NodeJS 等。但大多数旧服务器都是使用 C 或 C++从头开始构建的。这一系列博客将指导您使用 C 语言实现服务器-客户端通信，这就是所谓的套接字编程。

构建一个大规模的服务器需要很多东西。不可能涵盖本系列中的所有内容，但这可能是您学习套接字编程的良好开端。

在继续之前，我假设您已经对网络术语有了基本的了解，如服务器、客户机、请求、响应、协议(TCP/IP)等。

# 创建一个套接字

## 什么是插座？

套接字基本上是机器上的一个端点，用于在两个不同的进程之间进行通信。在我们的上下文中，在服务器和客户端进程之间。它用于在客户端和服务器之间发送和接收数据。

第一步是使用如下参数的 **socket()** 函数创建一个套接字，
**domain** :指定将用于通信的协议族。

网络中定义了多种协议。例如用于数据传输的互联网协议、用于网络问题处理的 ICMP 协议、用于地址解析的 ARP 等。在服务器-客户端通信中，进程之间的通信需要互联网协议(IPv4 或 IPv6)。

> IPv4 的 AF_INET 或 IPv6 的 AF_INET 可用于域

**类型**:插座类型。它为套接字指定了通信的语义。例如，它可以是顺序字节流(对于 TCP 套接字)、数据报(对于 UDP 套接字)等。

> SOCK_STREAM 用于创建 TCP 套接字，SOCK_DGRAM 用于创建 UDP 套接字。

**协议**:从选择的协议族中指定一个特定的协议。

> 如果通过 0，则采用默认协议，否则通过该协议的预定义的[号](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml)。

```
int  sockfd;
if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0)
{
   printf("Cannot create socket\n");
   exit(0);
}
```

socket()函数返回一个[文件描述符](https://www.computerhope.com/jargon/f/file-descriptor.htm)，它指向创建的套接字。

# 指定服务器属性

在创建套接字之后，需要指定服务器的几个属性，比如服务器将被托管的地址、服务器将提供服务的端口号等。

为此，使用了预定义的结构 **sockaddr_in** 。传递地址有两种方式，要么传递特定地址(32 位 IP 地址)，要么让 OS 自己决定地址( **INADDR_ANY** )。

> 当机器的 IP 地址未知时，使用特殊 IP 地址 INADDR_ANY。

```
struct sockaddr_in serv_addr;
serv_addr.sin_family = AF_INET; // Protocol family
serv_addr.sin_addr.s_addr = INADDR_ANY;
serv_addr.sin_port = 6000; // 16-bit number
```

# 绑定插座

套接字准备好了，服务器属性也定义好了。现在是时候捆绑他们了。
为此，使用了 **bind()** 函数，该函数将套接字文件描述符(*由 socket()* 返回)、服务器属性结构和该结构的大小作为参数。

```
if (bind(sockfd, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0)
{
    printf("Unable to bind local address\n");
    exit(0);
}
```

# 倾听客户的心声

服务器几乎准备好监听客户机请求了。

对于 TCP 套接字， **listen()** 函数告诉开始接受来自客户端的连接请求。它需要两个参数，socket FD 和在进一步的代码中接受任何请求之前可以考虑的连接数。

> 它基本上是 TCP 已经接受但在应用程序中尚未接受的连接。

```
listen(sockfd,5);
```

现在，服务器已经准备好接受客户机请求。

![](img/535640dbc8e278cad9d6dabfb1c90d05.png)

资料来源:tenor.com

# 接受连接请求

系统调用 **accept()** 接受客户端连接。它阻塞服务器，直到客户端请求到来。accept()系统调用在作为参数传递的类型为 **sockaddr_in** 的结构中填充客户端的详细信息。该结构的长度记录在**斜面**中。

它返回一个文件描述符，该描述符将用于与客户机的进一步通信。

```
clilen = sizeof(cli_addr);
newsockfd = accept(sockfd, (struct sockaddr *)&cli_addr, &clilen);
if (newsockfd < 0)
{
  printf("Accept error\n");
  exit(0);
}
```

一旦请求被接受，就会派生出一个子进程来处理这个请求。如果我们不派生一个进程来处理这个请求，那么服务器会一直忙于一次处理一个请求，并且不会接受其他请求，直到这个请求完成。

```
if (fork() == 0)
{
  close(sockfd);
  /* Close the old socket FD, since all communications for this request
  will be through the new socket FD. */
  ...
}
```

# 发送和接收数据

所有这些东西的主要目的是在服务器和客户机之间传输数据。这可以借助 **send()** 和 **recv()** 功能来完成。

**send()** :将数据发送给其他进程(即客户端)。
它将进行通信的套接字 FD、要传输的数据、数据的最大大小和一个标志作为参数。

**recv()** :接收来自其他进程(即来自客户端)的数据
它也采用正在进行通信的套接字 FD、存储传入数据的指针、要接收的数据的最大大小以及一个标志作为参数。

*例如，在连接请求被接受后，一个简单的字符串被发送到客户端，客户端也发送一个将被服务器接收的字符串。*

```
if (fork() == 0)
{
  close(sockfd);
  /* Close the old socket since all communications
  will be through the new socket. */

  // sending data
  for (i = 0; i < 100; i++)
    buf[i] = '\0';
  strcpy(buf, "Message from server");
  send(newsockfd, buf, 100, 0);

  // receivinng data
  for (i = 0; i < 100; i++)
    buf[i] = '\0';
  recv(newsockfd, buf, 100, 0);
  printf("%s\n", buf);
  close(newsockfd);
  exit(0);
}
close(newsockfd);
```

> 此外，我们需要关闭父服务器进程中新创建的套接字，以便其他请求可以使用它。

服务器基本上是不间断运行的程序。因此，为了实现这一特性，示例中将使用一个无限循环。

# 最终代码

```
int sockfd, newsockfd; /* Socket descriptors */
int clilen;
struct sockaddr_in cli_addr, serv_addr;
int i;
char buf[100];
if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0)
{
    printf("Cannot create socket\n");
    exit(0);
}
struct sockaddr_in serv_addr;
serv_addr.sin_family = AF_INET; // Protocol family
serv_addr.sin_addr.s_addr = INADDR_ANY;
serv_addr.sin_port = 6000; // 16-bit number
if (bind(sockfd, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0)
{
    printf("Unable to bind local address\n");
    exit(0);
}
listen(sockfd, 5);

while (1)
{
    clilen = sizeof(cli_addr);
    newsockfd = accept(sockfd, (struct sockaddr *)&cli_addr, &clilen);
    if (newsockfd < 0)
    {
        printf("Accept error\n");
        exit(0);
    }

    if (fork() == 0)
    {
        close(sockfd);
        /* Close the old socket since all communications
        will be through the new socket. */

        // sending data
        for (i = 0; i < 100; i++)
            buf[i] = '\0';
        strcpy(buf, "Message from server");
        send(newsockfd, buf, 100, 0);

        // receving data
        for (i = 0; i < 100; i++)
            buf[i] = '\0';
        recv(newsockfd, buf, 100, 0);
        printf("%s\n", buf);
        close(newsockfd);
        exit(0);
    }
    close(newsockfd);
}
```

万岁！！服务器已准备好进行部署。编译并运行代码，查看您的服务器在指定的端口上运行。

在本系列的下一个[部分](https://yashpaneliya.medium.com/socket-programming-in-c-client-4408231f9e65)中，我将演示如何使用套接字创建客户机，以及如何处理来自浏览器的请求，这将使我们更好地理解整个服务器-客户机通信。

如果你在本博客中发现任何问题或错误信息，请发表评论。这将有助于我和许多其他学习者理解这个概念。

如果你觉得这个博客有帮助，请表示感谢😄并与你的同事分享。

更了解我:【https://linktr.ee/yashpaneliya】T2