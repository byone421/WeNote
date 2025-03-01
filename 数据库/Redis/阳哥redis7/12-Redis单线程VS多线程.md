# Redis为什么选择单线程?
## 是什么
这种问法其实并不严谨，为啥这么说呢?
Redis的版本很多3.x、4.x、6.x，版本不同架构也是不同的，不限定版本问是否单线程也不太严谨。
1 版本3.x ，最早版本，也就是大家口口相传的redis是单线程，阳哥2016年讲解的redis就是3.X的版本。
2 版本4.x，严格意义来说也不是单线程，而是负责处理客户端请求的线程是单线程，但是开始加了点多线程的东西(异步删除)。---貌似
3 2020年5月版本的6.0.x后及2022年出的7.0版本后，告别了大家印象中的单线程，用一种全新的多线程来解决问题。---实锤
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697031748930-a1c5dbcd-a6ca-4bc0-a890-ecce78f730a7.png#averageHue=%23f9f5f3&clientId=ub431c0ac-b271-4&from=paste&height=423&id=udb196a39&originHeight=634&originWidth=1369&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=202297&status=done&style=none&taskId=u6cbe9d98-6fa9-4413-8b2c-fe9355855c1&title=&width=912.6666666666666)
## Redis单线程究竟何意
主要是指Redis的网络IO和键值对读写是由一个线程来完成的，Redis在处理客户端的请求时包括获取 (socket 读)、解析、执行、内容返回 (socket 写) 等都由一个顺序串行的主线程处理，这就是所谓的“单线程”。这也是Redis对外提供键值存储服务的主要流程。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697031954877-9c999c27-15e1-4364-81e7-64e63d87f528.png#averageHue=%23f7f0ee&clientId=ub431c0ac-b271-4&from=paste&height=328&id=u3ec15d6e&originHeight=492&originWidth=1258&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=125273&status=done&style=none&taskId=u40785da4-c0ca-43b1-ace4-466e5d147b2&title=&width=838.6666666666666)
但Redis的其他功能，比如持久化RDB、AOF、异步删除、集群数据同步等等，其实是由额外的线程执行的。
Redis命令工作线程是单线程的，但是，整个Redis来说，是多线程的；
## Redis3.x单线程时代但性能依日很快的主要原因

1. 基于内存操作: Redis 的所有数据都存在内存中，因此所有的运算都是内存级别的，所以他的性能比较高;
2. 数据结构简单: Redis 的数据结构是专门设计的，杂度都是 O(1)因此性能比较高
3. 多路复用和非阻塞 I/0: Redis使用 I/O多路复用功能来监听多个 socket连接客户端，这样就可以使用一个线程连接来处理多个请求，减少线程切换带来的开销，同时也避免了 I/O 阻塞操作
4. 避免,上下文切换:因为是单线程模型，因此就避免了不必要的上下文切换和多线程高争这就省去了多线程切换带来的时间和性能上的消耗而且单线程不会导致死锁问题的发生

简单来说，Redis4.0之前一直采用单线程的主要原因有以下三个：
1 使用单线程模型是 Redis 的开发和维护更简单，因为单线程模型方便开发和调试；
2 即使使用单线程模型也并发的处理多客户端的请求，主要使用的是IO多路复用和非阻塞IO；
3 对于Redis系统来说，主要的性能瓶颈是内存或者网络带宽而并非 CPU。

# 既然单线程这么好，为什么逐渐又加入了多线程特性?
## 单线程的苦恼
正常情况下使用 del 指令可以很快的删除数据，而当被删除的 key 是一个非常大的对象时，例如时包含了成千上万个元素的 hash 集合时，那么 del 指令就会造成 Redis 主线程卡顿。
这就是redis3.x单线程时代最经典的故障，大key删除的头疼问题，
由于redis是单线程的，del  bigKey .....
等待很久这个线程才会释放，类似加了一个synchronized锁，你可以想象高并发下，程序堵成什么样子？
## 案例
比如当我（Redis）需要删除一个很大的数据时，因为是单线程原子命令操作，这就会导致 Redis 服务卡顿，
于是在 Redis 4.0 中就新增了多线程的模块，当然此版本中的多线程主要是为了解决删除数据效率比较低的问题的。
```java
unlink key
flushdb async
flushall async
把删除工作交给了后台的小弟 (子线程) 异步来删除数据了。
```
因为Redis是单个主线程处理，redis之父antirez一直强调"Lazy Redis is better Redis".
而lazy free的本质就是把某些cost(主要时间复制度，占用主线程cpu时间片)较高删除操作，
从redis主线程剥离让子线程来处理，极大地减少主线阻塞时间。从而减少删除导致性能和稳定性问题。

**在Redis 4.0就引入了多个线程来实现数据的异步惰性删除等功能但是其处理读写请求的仍然只有一个线程，所以仍然算是狭义上的单线程**
# redis6/7的多线程特性和IO多路复用入门篇
## redis6/7，真正多线程登场
在Redis6/7中，非常受关注的第一个新特性就是多线程。
这是因为，Redis一直被大家熟知的就是它的单线程架构，虽然有些命令操作可以用后台线程或子进程执行（比如数据删除、快照生成、AOF重写）。但是，从网络IO处理到实际的读写命令处理，都是由单个线程完成的。
随着网络硬件的性能提升，Redis的性能瓶颈有时会出现在网络IO的处理上，也就是说，单个主线程处理网络请求的速度跟不上底层网络硬件的速度。
为了应对这个问题:
采用多个IO线程来处理网络请求，提高网络请求处理的并行度，Redis6/7就是采用的这种方法。
但是，Redis的多IO线程只是用来处理网络请求的，对于读写操作命令Redis仍然使用单线程来处理。这是因为，Redis处理请求时，网络处理经常是瓶颈，通过多个IO线程并行处理网络操作，可以提升实例的整体处理性能。而继续使用单线程执行命令操作，就不用为了保证Lua脚本、事务的原子性，额外开发多线程互斥加锁机制了(不管加锁操作处理)，这样一来，Redis线程模型实现就简单了
## 主线程和IO线程是怎么协作完成请求处理的-精讲版
**读取**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697121725484-50fdff92-6c0f-4433-90bb-e6731329a00b.png#averageHue=%23f3e9c6&clientId=u625818c6-f040-4&from=paste&height=531&id=u7e2c7d2f&originHeight=796&originWidth=1096&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=305392&status=done&style=none&taskId=ua49bf4e2-8abd-43ca-9727-0e08834a819&title=&width=730.6666666666666)
**回写**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697121738028-b5af6751-23ec-418f-a219-af164948a0e1.png#averageHue=%23f3ebcd&clientId=u625818c6-f040-4&from=paste&height=437&id=ucce4dc03&originHeight=655&originWidth=1094&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=236357&status=done&style=none&taskId=u0128f3bb-765f-4da1-9e6c-4e3cbd48a4e&title=&width=729.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697121725558-e3a33576-b7eb-4d46-bcd6-fa351b6c9210.png#averageHue=%23eeeeee&clientId=u625818c6-f040-4&from=paste&height=503&id=uc661eeb8&originHeight=755&originWidth=1252&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=359521&status=done&style=none&taskId=u6fcb255f-d842-4350-9d99-eb68455423f&title=&width=834.6666666666666)
## Unix网络编程中的五种IO模型
### Blocking IO- 阻塞IO
### NoneBlocking lO-非阳塞IO
### lO multiplexing - I多路复用
Linux世界一切皆文件
文件描述符、简称FD，句柄：文件描述符（File descriptor）是计算机科学中的一个术语，是一个用于表述指向文件的引用的抽象化概念。文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。在程序设计中，文件描述符这一概念往往只适用于UNIX、Linux这样的操作系统。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697203313431-cd693f67-fd31-429b-b319-5e589c84f83d.png#averageHue=%23fcfbf9&clientId=ud4d84067-4506-4&from=paste&height=499&id=u4beffd5d&originHeight=749&originWidth=1154&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=280187&status=done&style=none&taskId=ue879e09d-b39c-4231-997a-fabba910f70&title=&width=769.3333333333334)
#### IO多路复用是什么
种同步的IO模型，实现一个线程监视多个文件句柄
一旦某个文件句柄就绪就能够通知到对应应用程序进行相应的读写操作，没有文件句柄就绪时就会阻塞应用程序
从而释放CPU资源

1. I/0 : 网络0，尤其在操作系统层面指数据在内核态和用户态之间的读写操作
2. 多路: 多个客户端连接 (连接就是套接字描述符，即 socket 或者 channel)
3. 复用: 复用一个或几个线程

也就是说一个或一组线程处理多个TCP连接使用单进程就能够实现同时处理多个客户端的连接无需创建或者维护过多的进程/线程
**一句话：一个服务端进程可以同时处理多个套接字描述符，实现IO多路复用的模型有3种: 可以分select->poll->epoll三个阶段来描述**
#### 引出epoll

1. **场景解析**

模拟一个tcp服务器处理30个客户socket。
假设你是一个监考老师，让30个学生解答一道竞赛考题，然后负责验收学生答卷，你有下面几个选择：
第一种选择(轮询)：按顺序逐个验收，先验收A，然后是B，之后是C、D。。。这中间如果有一个学生卡住，全班都会被耽误,你用循环挨个处理socket，根本不具有并发能力。
第二种选择(来一个new一个，1对1服务)：你创建30个分身线程，每个分身线程检查一个学生的答案是否正确。 这种类似于为每一个用户创建一个进程或者线程处理连接。
第三种选择(响应式处理，1对多服务)，你站在讲台上等，谁解答完谁举手。这时C、D举手，表示他们解答问题完毕，你下去依次检查C、D的答案，然后继续回到讲台上等。此时E、A又举手，然后去处理E和A。。。这种就是IO复用模型。Linux下的select、poll和epoll就是干这个的

2. **IO多路复用**

将用户socket对应的文件描述符(FileDescriptor)注册进epoll，然后epoll帮你监听哪些socket上有消息到达，这样就避免了大量的无用操作。此时的socket应该采用非阻塞模式。这样，整个过程只在调用select、poll、epoll这些调用的时候才会阻塞，收发客户消息是不会阻塞的，整个进程或者线程就被充分利用起来，这就是事件驱动，所谓的reactor反应模式。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697204495509-7b4e29ea-dacc-47c1-ad00-534ac42f574b.png#averageHue=%23f6f6f5&clientId=ud4d84067-4506-4&from=paste&height=303&id=u2211420d&originHeight=454&originWidth=860&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=106360&status=done&style=none&taskId=ud4a9e91f-3a41-4253-b3bb-cc22232ce4e&title=&width=573.3333333333334)
在单个线程通过记录跟踪每一个Sockek(I/O流)的状态来同时管理多个I/O流. 一个服务端进程可以同时处理多个套接字描述符。目的是尽量多的提高服务器的吞吐能力。
大家都用过nginx，nginx使用epoll接收请求，ngnix会有很多链接进来， epoll会把他们都监视起来，然后像拨开关一样，谁有数据就拨向谁，然后调用相应的代码处理。redis类似同理，这就是IO多路复用原理，有请求就响应，没请求不打扰。
#### 只使用一个服务端进程可以同时处理多个套接字描述符连接
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697204693391-4ce7372b-551b-4ff3-a04c-5de33c4d526e.png#averageHue=%23f2eded&clientId=ud4d84067-4506-4&from=paste&height=395&id=u1b4a767a&originHeight=592&originWidth=1969&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=346471&status=done&style=none&taskId=ua6d624bd-d273-4773-9cc0-7f60f54bce1&title=&width=1312.6666666666667)
### signal driven IO - 信号动IO
### asynchronous IO-异步IO
## 主线程和IO线程是怎么协作完成请求处理的-精简版
从Redis6开始，就新增了多线程的功能来提高 I/O 的读写性能，他的主要实现思路是将主线程的 IO 读写任务拆分给一组独立的线程去执行，这样就可以使多个 socket 的读写可以并行化了，采用多路 I/O 复用技术可以让单个线程高效的处理多个连接请求（尽量减少网络IO的时间消耗），将最耗时的Socket的读取、请求解析、写入单独外包出去，剩下的命令执行仍然由主线程串行执行并和内存的数据交互。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697204893459-c55d111a-7b7e-4a43-b682-e12ea2eb552a.png#averageHue=%23f9f3f1&clientId=ud4d84067-4506-4&from=paste&height=417&id=uef752fb0&originHeight=625&originWidth=1296&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=141957&status=done&style=none&taskId=uf99d19a7-3d4f-4df6-a2e3-2005aa5c302&title=&width=864)
结合上图可知，网络IO操作就变成多线程化了，其他核心部分仍然是线程安全的，是个不错的折中办法
## Redis7默认是否开启了多线程?
如果你在实际应用中，发现Redis实例的CPU开销不大但吞吐量却没有提升
可以考虑使用Redis7的多线程机制，加速网络处理，进而提升实例的吞吐量
Redis7将所有数据放在内存中，内存的响应时长大约为100纳秒，对于小数据包，Redis服务器可以处理8W到10W的QPS，
这也是Redis处理的极限了，对于80%的公司来说，单线程的Redis已经足够使用了。
在Redis6.0及7后，多线程机制默认是关闭的，如果需要使用多线程功能，需要在redis.conf中完成两个设置
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697208424066-4347a4ef-9f74-4d2e-bffa-7a684401b22e.png#averageHue=%23e5cb8f&clientId=ud4d84067-4506-4&from=paste&height=779&id=u7e359f30&originHeight=1169&originWidth=1345&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=631368&status=done&style=none&taskId=ueaa80384-4ffb-40fd-93ab-65662ef95fe&title=&width=896.6666666666666)
1.设置io-thread-do-reads配置项为yes，表示启动多线程。
2。设置线程个数。关于线程数的设置，官方的建议是如果为 4 核的 CPU，建议线程数设置为 2 或 3，如果为 8 核 CPU 建议线程数设置为 6，线程数一定要小于机器核数，线程数并不是越大越好。
