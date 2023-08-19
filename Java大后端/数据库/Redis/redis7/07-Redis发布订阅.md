# 是什么
种消息通信模式:发送者(PUBLISH)发送消息，订阅者(SUBSCRIBE)接收消息，可以实现进程间的消息传递
官网：[https://redisio/docs/manual/pubsub/](https://redisio/docs/manual/pubsub/)
一句话：
Redis可以实现消息中间件MQ的功能，通过发布订阅实现消息的引导和分流。
仅代表我人人，不推荐使用该功能，专业的事情交给专业的中间件处理，redis就做好分布式缓存功能
# 能干嘛
Redis客户端可以订阅任意数量的频道
类似我们微信关注多个公众号
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692454955810-2567aeac-a21f-472c-9052-f6d0dfd6a3fb.png#averageHue=%23f9f8f7&clientId=u2bf64916-05a8-4&from=paste&height=199&id=u738a45f1&originHeight=299&originWidth=850&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=61229&status=done&style=none&taskId=u7e6c73c4-9c73-459e-8ff6-10d502d7585&title=&width=566.6666666666666)
当有新消息通过PUBLISH命令发送给频道channel1时
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692454967868-ff4d7636-64a6-433e-a700-a39fa59aa8b7.png#averageHue=%23d8d195&clientId=u2bf64916-05a8-4&from=paste&height=249&id=u4e7400ac&originHeight=374&originWidth=459&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=48207&status=done&style=none&taskId=ufca15a48-f583-437b-9432-4397e3a6f39&title=&width=306)
发布/订阅其实是一个轻量的队列，只不过数据不会被持久化，一般用来处理实时性较高的异步消息
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692454995577-11fb82c7-cf2e-4a2b-94ab-55c05944a5ab.png#averageHue=%2331a8d8&clientId=u2bf64916-05a8-4&from=paste&height=397&id=u76b294bd&originHeight=596&originWidth=878&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=231832&status=done&style=none&taskId=u7ad8563e-a816-45f2-97e1-261d977abd7&title=&width=585.3333333333334)
# 常用的命令

1. **SUBSCRIBE channel [channel ...]**

订阅给定的一个或多个频道的信息
推荐先执行订阅后再发布，订阅成功之前发布的消息是收不到的
订阅的客户端每次可以收到一个 3个参数的消息：
a.消息的种类
b.始发频道的名称
c.实际的消息内容
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692455494703-04720c0c-c4af-4da2-ae1d-671d86976864.png#averageHue=%23fefefe&clientId=u2bf64916-05a8-4&from=paste&height=111&id=u3be65a9e&originHeight=167&originWidth=536&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=31840&status=done&style=none&taskId=udc79f714-9c03-4853-81a7-f3c917a7ee2&title=&width=357.3333333333333)

2. **PUBLISH channel message**

发布消息到指定的频道

3. **PSUBSCRIBE pattern [pattern ....]**

按照模式批量订阅，订阅一个或多个符合给定模式(支持*号?号之类的)的频道

4. **PUBSUB subcommand [argument [argument ...]]**

查看订阅与发布系统状态
a. PUBSUB CHANNELS
由活跃频道组成的列表
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692455647901-b45312c5-8125-4ced-ba49-6519ee0cb04b.png#averageHue=%23fdfdfd&clientId=u2bf64916-05a8-4&from=paste&height=68&id=ua62ddb5c&originHeight=102&originWidth=716&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=28056&status=done&style=none&taskId=u85f13255-076f-4d3c-b58c-f1135986bc8&title=&width=477.3333333333333)
b. PUBSUB NUMSUB [channel [channel ...]]
某个频道有几个订阅者
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692455683931-679471a8-7f98-416b-b466-60310686d050.png#averageHue=%23fcfcfc&clientId=u2bf64916-05a8-4&from=paste&height=101&id=u66cb1329&originHeight=151&originWidth=734&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=40321&status=done&style=none&taskId=u595bdd6b-9b3e-46fe-849b-90d31fedbde&title=&width=489.3333333333333)
c. PUBSUB NUMPAT
只统计使用PSUBSCRIBE命令执行的，返回客户端订阅的唯一模式的数量
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692455730880-430c3a9d-9a6b-401d-ab39-28b9c039e9e0.png#averageHue=%23fcf9f9&clientId=u2bf64916-05a8-4&from=paste&height=481&id=ue6e209bf&originHeight=722&originWidth=1233&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=162235&status=done&style=none&taskId=u3fe4002a-58f0-4bee-9ffe-cf5fe9bd942&title=&width=822)

5. **UNSUBSCRIBE [channel [channel ...]]**

取消订阅

6. **PUNSUBSCRIBE [pattern [pattern ...]]**

退订所有给定模式的频道
# 案例
**开启3个客户端，演示客户端A、B订阅消息，客户端C发布消息**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692455970840-99452a2f-cc3e-4337-98e8-16fcbfa69005.png#averageHue=%23f8f4f4&clientId=u2bf64916-05a8-4&from=paste&height=264&id=ub0b5a59a&originHeight=396&originWidth=1678&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=167858&status=done&style=none&taskId=u60d3fdfb-a0e2-4f47-97fd-3f909f19643&title=&width=1118.6666666666667)
**演示批量订阅和发布**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692456048456-a78d9d16-4cce-4f61-917a-83d3d32f115d.png#averageHue=%23e5e5e4&clientId=u2bf64916-05a8-4&from=paste&height=456&id=u19f36788&originHeight=684&originWidth=1338&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=240311&status=done&style=none&taskId=ub6460644-e713-4a75-ba48-2daf0a16364&title=&width=892)
**取消订阅**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692456082217-bc340735-6c86-4122-aa6f-343e94232ffd.png#averageHue=%23fefafa&clientId=u2bf64916-05a8-4&from=paste&height=313&id=u3e5ba01c&originHeight=469&originWidth=1117&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=113041&status=done&style=none&taskId=ua4775ce2-0762-4eb2-bd44-f125a01806c&title=&width=744.6666666666666)
# 总结
发布的消息在Redis系统中不能持久化，必须先执行订阅，再等待消息发布。如果先发布了消息，那么该消息由于没有订阅者，消息将被直接丢弃。因此消息只管发送对于发布者而言消息是即发即失的，不管接收，也没有ACK机制，无法保证消息的消费成功。
以上的缺点导致Redis的Pub/Sub模式就像个小玩具，在生产环境中几乎无用武之地，为此
Redis5.0版本新增了stream数据结构，不但支持多播，还支持数据持久化，相比Pub/Sub更加的强大

