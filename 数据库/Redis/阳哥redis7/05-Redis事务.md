# 简单介绍
官网：https://redis.io/docs/manual/transactions
可以一次执行多个命令所有命令都会序列化，本质是一组命令的集合。一个事务中的
按顺序地串行化执行而不会被其它命令插入，不许加塞
一个队列中，一次性、顺序性、排他性的执行一系列命令
# Redis事务vs数据库事务
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692447810539-46fdbd9e-c773-4e50-86fb-b5b78b9fb962.png#averageHue=%23eaedf5&clientId=u5ee178e2-d53b-4&from=paste&height=208&id=u6deb38f1&originHeight=312&originWidth=1614&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=136869&status=done&style=none&taskId=u66abc4a7-6e34-4cc7-baf2-f7399b58efe&title=&width=1076)
# 怎么玩
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692448011453-3f29d922-58f9-468b-abf2-d954e8a6d537.png#averageHue=%23eee8d5&clientId=u5ee178e2-d53b-4&from=paste&height=465&id=u5e79f07f&originHeight=698&originWidth=971&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=209688&status=done&style=none&taskId=ua3a7d515-e620-4c45-ab58-055d9165df5&title=&width=647.3333333333334)

## Case1：正常执行
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450089151-936c9ff7-4560-4faf-bb9a-e0ab2a56e0b1.png#averageHue=%23fef3f3&clientId=u5ee178e2-d53b-4&from=paste&height=356&id=uc94ff413&originHeight=534&originWidth=705&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=84682&status=done&style=none&taskId=u116f2b72-e114-47da-ba71-f5521679414&title=&width=470)
## Case2：放弃事务
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450132546-0b7170fd-822e-4309-8b4b-332d0abe9629.png#averageHue=%23e1e1e0&clientId=u5ee178e2-d53b-4&from=paste&height=261&id=u4a9ceed8&originHeight=392&originWidth=554&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=82729&status=done&style=none&taskId=u01225836-14d2-41a3-ae59-68b4b273064&title=&width=369.3333333333333)
## Case3：全体连坐
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450183615-fbad6cae-fa05-4b8e-8286-abc90b7706e6.png#averageHue=%23fef6f6&clientId=u5ee178e2-d53b-4&from=paste&height=443&id=u1c3811a9&originHeight=665&originWidth=1326&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=218741&status=done&style=none&taskId=u65ae75cc-ce64-44d9-83f0-63d6f184492&title=&width=884)
## Case4：冤头债主
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450306951-726a497c-7e59-47f4-a7ae-3a51e3547d2b.png#averageHue=%23fef9f9&clientId=u5ee178e2-d53b-4&from=paste&height=444&id=uc6327e7c&originHeight=666&originWidth=1094&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=169603&status=done&style=none&taskId=uac25ee38-9be4-4a83-98c4-bd0e8772470&title=&width=729.3333333333334)
注意和传统数据库事务区别，不一定要么一起成功要么一起失败
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450325830-b5c64711-24d4-4a56-9917-6993e10ab622.png#averageHue=%23e1e5ce&clientId=u5ee178e2-d53b-4&from=paste&height=214&id=u8b1ada57&originHeight=321&originWidth=1003&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=94475&status=done&style=none&taskId=u03b553a2-3857-439f-b088-8b5d4c0a0cf&title=&width=668.6666666666666)
## Case5：事务监控
Redis使用Watch来提供乐观锁定，类似于CAS(Check-and-Set)
**悲观锁：**
悲观锁(Pessimistic Lock), 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁
**乐观锁：**
乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据。
乐观锁策略:提交版本必须   大于   记录当前版本才能执行更新
**CAS:**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450573982-6dc02a4f-8f1c-48fa-aafc-f1550162e088.png#averageHue=%23e0cfb9&clientId=u5ee178e2-d53b-4&from=paste&height=239&id=u8b3d3810&originHeight=358&originWidth=1094&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=119417&status=done&style=none&taskId=u260b455d-b0dd-469a-8c44-40e9958c8ad&title=&width=729.3333333333334)
### Watch
初始化k1和balance两个key,先监控再开启multi  保证两key变动在同一个事务内
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450664873-bc6fbdd5-d562-42bd-a2cd-e14f5fcca32c.png#averageHue=%23fcf8f8&clientId=u5ee178e2-d53b-4&from=paste&height=443&id=u3567f371&originHeight=665&originWidth=657&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=119339&status=done&style=none&taskId=u3506b3cd-4e05-48ab-a021-c1dcebbb088&title=&width=438)
**有加塞篡改**
watch命令是一种乐观锁的实现，Redis在修改的时候会检测数据是否被更改，如果更改了，则执行失败
第一个窗口蓝色框第5步执行结果返回为空，也就是相当于是失败，笔记见最下面官网说明
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450777345-55a62d49-b0c4-4399-acea-1767ec50890b.png#averageHue=%23e4e0da&clientId=u5ee178e2-d53b-4&from=paste&height=547&id=ue23b439b&originHeight=820&originWidth=1318&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=301011&status=done&style=none&taskId=u784a40bb-fcee-4a18-a257-eedadde4e4f&title=&width=878.6666666666666)
### Unwatch
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450822513-a1b84b50-3a17-4062-b92a-ce1233ec3c74.png#averageHue=%23faf1f1&clientId=u5ee178e2-d53b-4&from=paste&height=414&id=u8ee7648b&originHeight=621&originWidth=1163&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=174172&status=done&style=none&taskId=u179fa6cd-65d6-4f2a-834b-76d9834b159&title=&width=775.3333333333334)
### 小结
一旦执行了exec之前加的监控锁都会被取消掉了
当客户端连接丢失的时候(比如退出链接)，所有东西都会被取消监视
# 小总结
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692450975949-4098e119-9299-426b-8f8b-68f7071dc7e1.png#averageHue=%23f6f6f6&clientId=u5ee178e2-d53b-4&from=paste&height=417&id=uaa43560c&originHeight=625&originWidth=1014&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=144348&status=done&style=none&taskId=u0dced8de-5c4e-4005-bca9-a5f3d099862&title=&width=676)
**开启: **以MULTI开始一个事务
**入队: **将多个命令入队到事务中，接到这些命令并不会立即执行，而是放到等待执行的事务队列里面
**执行: **由EXEC命令触发事务
