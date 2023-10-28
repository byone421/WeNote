# 问题由来
Redis是一种基于客户端-服务端模型以及请求/响应协议的TCP服务。一个请求会遵循以下步骤：
1 客户端向服务端发送命令分四步(发送命令→命令排队→命令执行→返回结果)，并监听Socket返回，通常以阻塞模式等待服务端响应。
2 服务端处理命令，并将结果返回给客户端。
上述两步称为：Round Trip Time(简称RTT,数据包往返于两端的时间)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692452629001-d500771a-2486-4004-bdde-87c49635a600.png#averageHue=%23fbfbfa&clientId=u3da74f59-9827-4&from=paste&height=355&id=ue2bfdf7c&originHeight=533&originWidth=787&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=147518&status=done&style=none&taskId=ua75e029b-725a-4c46-b1d7-a880ea81216&title=&width=524.6666666666666)
如果同时需要执行大量的命令，那么就要等待上一条命令应答后再执行，这中间不仅仅多了RTT（Round Time Trip），而且还频繁调用系统IO，发送网络请求，同时需要redis调用多次read()和write()系统方法，系统方法会将数据从用户态转移到内核态，这样就会对进程上下文有比较大的影响了，性能不太好，o(╥﹏╥)o
# 解决思路
引出管道这个概念：[https://redis.io/docs/manual/pipelining/](https://redis.io/docs/manual/pipelining/)
管道(pipeline)可以一次性发送多条命令给服务端，服务端依次处理完完毕后，通过一条响应一次性将结果返回，通过减少客户端与redis的通信次数来实现降低往返延时时间。pipeline实现的原理是队列，先进先出特性就保证数据的顺序性。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692453095851-fd89cf58-7253-4ef5-80be-4bbfb58bef7c.png#averageHue=%23fcfcfb&clientId=u3da74f59-9827-4&from=paste&height=356&id=u668c5ca7&originHeight=534&originWidth=1126&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=161357&status=done&style=none&taskId=u8dc70eb1-f9c2-4591-85d5-046468000cf&title=&width=750.6666666666666)
# 定义
对整个Redis的执行不造成其它任何影响，Pipeline是为了解决RTT往返回时，仅仅是将命令打包一次性发送
一句话：批处理命令变种优化措施，类似Redis的原生批命令(mget和mset)
# 案例
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692453245654-08805e55-b87a-4d4f-a5a3-4391abff7f78.png#averageHue=%23fefafa&clientId=u3da74f59-9827-4&from=paste&height=393&id=uc8df9bdb&originHeight=589&originWidth=1331&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=248382&status=done&style=none&taskId=u91c20a9b-0c03-4e3c-8bbf-3e6ffba2c66&title=&width=887.3333333333334)
# 小总结
**Pipeline与原生批量命令对比**
原生批量命令是原子性(例如:mset,mget)，pipeline是非原子性
原生批量命令一次只能执行一种命令，pipeline支持批量执行不同命令
原生批命令是服务端实现，而pipeline需要服务端与客户端共同完成pipeline缓冲的指令只是会依次执行，不保证原子性，如果执行中指令发生异常，将会继续执行后续的指令
使用pipeline组装的命令个数不能大多，不然数据量过大客户端阻塞的时间可能过久，同时服务端此时也被迫回复一个队列答复，占用很多内存
**Pipeline与事务对比**
事务具有原子性，管道不具有原子性，执行事务时会阻塞其他命令的执行，而执行管道中的命令时不会
曾道一次性将多条命令发送到服务器，事务是一条一条的发，事务只有在接收到exec命令后才会执行，管道不会
**使用Pipeline注意事项**
pipeline缓冲的指令只是会依次执行
，不保证原子性，如果执行中指令发生异常，将会继续执行后续的指令
使用pipeline组装的命令个数不能太多，不然数据量过大客户端阳塞的时间可能过久，同时服务端此时也被迫回复一个队列答复，占用很多内存

