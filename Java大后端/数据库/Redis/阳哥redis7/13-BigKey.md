# 插入大量数据
## Linux Bash下面执行，插入100W

1. 生成100W条redis批量设置kv的语句(key=kn,value=vn)写入到/tmp目录下的redisTest.txt文件中

for((i=1;i<=100*10000;i++)); do echo "set k$i v$i" >> /tmp/redisTest.txt ;done;

2. cat /tmp/redisTest.txt | /opt/redis-7.0.0/src/redis-cli -h 127.0.0.1 -p 6379 -a 111111 --pipe

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697552373405-e74e44eb-3b20-498e-a3c3-91ea92005230.png#averageHue=%23fdf7f7&clientId=u5d9f11e5-f475-4&from=paste&height=343&id=u2c91da38&originHeight=514&originWidth=1721&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=205334&status=done&style=none&taskId=u602fbdfb-543c-4025-9e9e-d3a4c41b95b&title=&width=1147.3333333333333)
# MoreKey
## keys * 你试试100W花费多少秒遍历查询
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697552450076-fdcadced-9bc6-4edb-aae6-cc71ff27be8f.png#averageHue=%23afb4b4&clientId=u5d9f11e5-f475-4&from=paste&height=543&id=u869fe296&originHeight=815&originWidth=1307&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=341849&status=done&style=none&taskId=uee819545-8743-4bd3-9ac3-29d0a8707dc&title=&width=871.3333333333334)
## 生产上限制keys */flushdb/flushall等危险命令以防止误删误用?
通过配置项禁用
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697552506644-7197d70a-51e6-4692-aaf5-e88b7be433cc.png#averageHue=%23e2d1bc&clientId=u5d9f11e5-f475-4&from=paste&height=511&id=u29248bab&originHeight=766&originWidth=1205&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=192003&status=done&style=none&taskId=u0f5ee1f3-a4f1-4a2e-b991-7b24e8c4a1c&title=&width=803.3333333333334)
## 不用key * ，用scan
[https://redisio/commands/scan/](https://redisio/commands/scan/)
[https://redis.com.cn/commands/scan.htm](https://redis.com.cn/commands/scan.htm)l

**Scan 命令用于迭代数据库中的数据库键**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697552798626-c09108dd-690d-4127-826f-a54c4e9c86ee.png#averageHue=%23d7c8ae&clientId=u5d9f11e5-f475-4&from=paste&height=156&id=u73571d67&originHeight=234&originWidth=1236&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=52713&status=done&style=none&taskId=uf60d531e-d6b8-435b-a1a7-56caeff5289&title=&width=824)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697552888764-53c4866f-4a3e-491b-8396-c227aae0e59e.png#averageHue=%23e7e7e6&clientId=u5d9f11e5-f475-4&from=paste&height=305&id=ubfaf96d9&originHeight=458&originWidth=1163&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=250564&status=done&style=none&taskId=u4105a1dc-2e68-42cc-9152-bda7fac4779&title=&width=775.3333333333334)

**SCAN 返回一个包含两个元素的数组， **
第一个元素是用于进行下一次迭代的新游标， 
第二个元素则是一个数组， 这个数组中包含了所有被迭代的元素。如果新游标返回零表示迭代已结束。

**SCAN的遍历顺序**
非常特别，它不是从第一维数组的第零位一直遍历到末尾，而是采用了高位进位加法来遍历。之所以使用这样特殊的方式进行遍历，是考虑到字典的扩容和缩容时避免槽位的遍历重复和遗漏。
### 使用
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697554121515-84b8d539-fc9b-4492-a37e-3b77d3b25728.png#averageHue=%23f8f8f6&clientId=u5d9f11e5-f475-4&from=paste&height=515&id=ub28234da&originHeight=773&originWidth=534&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=127547&status=done&style=none&taskId=u117298dd-9a96-4f57-b78e-cf80011955f&title=&width=356)
# BigKey
## 多大算bigkey
参考阿里云Redis规范
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718177876-6d446d32-57f3-466e-a24f-f1ed94e1449d.png#averageHue=%23f4ecec&clientId=u13d40377-e58d-4&from=paste&height=348&id=u3aa65e8e&originHeight=522&originWidth=1311&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=168677&status=done&style=none&taskId=u594a632b-f008-477c-a39c-01218f5dc63&title=&width=874)
string 最大可以512MB但是>=10KB就是bigKey
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718251641-c7187c4b-5290-41d9-ae59-2db34d8a67f0.png#averageHue=%23fbfaf9&clientId=u13d40377-e58d-4&from=paste&height=177&id=u4236da7e&originHeight=266&originWidth=1089&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=97519&status=done&style=none&taskId=u32dcbba0-8ee3-47ad-a0f1-cbbf417171e&title=&width=726)
## 如何发现
**redis-cli --bigkeys**
> redis-cli -h 127.0.0.1 -p 6379 -a 111111 --bigkeys

好处：
给出每种数据结构Top 1 bigkey，同时给出每种数据类型的键值个数+平均大小
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718408025-68a327e3-1cb8-4c71-8f54-97cea54a5b1d.png#averageHue=%23fefdfd&clientId=u13d40377-e58d-4&from=paste&height=479&id=u13c06847&originHeight=718&originWidth=1024&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=342712&status=done&style=none&taskId=u6eb34132-0c7b-4bb1-ba7b-9e8a9f479d1&title=&width=682.6666666666666)
不足：
想查询大于10kb的所有key，--bigkeys参数就无能为力了，需要用到memory usage来计算每个键值的字节数

## MEMORY USAGE 键
[https://redis.com.cn/commands/memory-usage.htm](https://redis.com.cn/commands/memory-usage.htm.)l
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718474322-3e279e0d-685a-4599-9bff-0fa801bc9c88.png#averageHue=%23f6f6f5&clientId=u13d40377-e58d-4&from=paste&height=376&id=u570dd9d1&originHeight=564&originWidth=973&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=146504&status=done&style=none&taskId=udf17efa1-c968-45e3-80ff-6e38be5a1d7&title=&width=648.6666666666666)
## 如何删除
### String
一般用del，如果过于庞大unlink
### Hash
使用hscan每次获取少量field-value，再使用hdel删除每个field
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718887051-299f1895-2382-4544-b852-8e48de1b5764.png#averageHue=%23fbf7f6&clientId=u13d40377-e58d-4&from=paste&height=380&id=uefc85a61&originHeight=570&originWidth=860&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=107178&status=done&style=none&taskId=u206c981e-259c-4bd7-8547-57e605150c1&title=&width=573.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718913491-7a8aff3c-0440-49ad-b38b-35db8bbbae8d.png#averageHue=%23f3f5f8&clientId=u13d40377-e58d-4&from=paste&height=521&id=uf4eac879&originHeight=781&originWidth=1226&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=217372&status=done&style=none&taskId=udec4ce93-c90d-4b84-9856-5264a71d6cd&title=&width=817.3333333333334)
### list
使用Itrim渐进式逐步删除，直到全部删除完成
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718989045-dbc7212e-cce5-4dfe-a3d1-a3c39c3cbbac.png#averageHue=%23fbf8f7&clientId=u13d40377-e58d-4&from=paste&height=349&id=u7f6706f5&originHeight=524&originWidth=1282&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=122954&status=done&style=none&taskId=uaaab334c-e597-41c7-93e0-6ad28e86165&title=&width=854.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697718998052-fca7cf62-759b-4405-96e1-a38d411c90ac.png#averageHue=%23fdfdfd&clientId=u13d40377-e58d-4&from=paste&height=342&id=u16dfaa23&originHeight=513&originWidth=630&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=88531&status=done&style=none&taskId=udf31d882-b864-473b-ac8b-a21b4d206b1&title=&width=420)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697719008033-934d8494-5643-41cf-9c6a-0d85de347b96.png#averageHue=%23f3f5f8&clientId=u13d40377-e58d-4&from=paste&height=426&id=u309c4496&originHeight=639&originWidth=1119&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=130693&status=done&style=none&taskId=u27384c8c-48a5-49c1-8c2f-071333dc1b2&title=&width=746)
### set
使用sscan每次获取部分元素，再使用srem命令删除每个元素
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697719164386-adf303e6-a4ef-4488-b5e9-b59298f61ade.png#averageHue=%23fdfdfd&clientId=u13d40377-e58d-4&from=paste&height=392&id=ub97d7435&originHeight=588&originWidth=503&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=73934&status=done&style=none&taskId=ua2544236-f0f8-4f36-808d-7ae348a6cf3&title=&width=335.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697719196232-d4500e42-1a48-412b-ae02-a63d40e5ce18.png#averageHue=%23f2f5f8&clientId=u13d40377-e58d-4&from=paste&height=524&id=u459ba375&originHeight=786&originWidth=1127&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=198807&status=done&style=none&taskId=u4f260031-e344-4e49-ad04-087cb8ec4bf&title=&width=751.3333333333334)
### zset
使用zscan每次获取部分元素，再使用ZREMRANGEBYRANK命令删除每个元素
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697719227406-c445a6cc-492c-4ed6-bd58-10ec25ccefa6.png#averageHue=%23fdfdfd&clientId=u13d40377-e58d-4&from=paste&height=711&id=u56c8fb32&originHeight=1066&originWidth=964&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=160745&status=done&style=none&taskId=u23c95300-1188-4154-9030-3e3e6b0fbeb&title=&width=642.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697719261650-2f5c5d10-4676-42ac-9056-5910e3d5038f.png#averageHue=%23f2f5f8&clientId=u13d40377-e58d-4&from=paste&height=519&id=ua6ca6f6f&originHeight=779&originWidth=1096&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=180128&status=done&style=none&taskId=uda2994d1-5ba8-4433-932a-cf4d169c881&title=&width=730.6666666666666)

## 调优配置
阻塞和非阻塞删除命令
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697719425570-755d7935-53b1-4304-bbf1-04538bd1d51c.png#averageHue=%23ceb99c&clientId=u13d40377-e58d-4&from=paste&height=521&id=uce7e624a&originHeight=781&originWidth=1905&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=1005682&status=done&style=none&taskId=u1b75f8b1-d410-458e-bb3f-2f22bef407d&title=&width=1270)
优化配置
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1697719443962-03bf239d-9e9d-42ca-ae72-012db1195b5a.png#averageHue=%23fdf7f5&clientId=u13d40377-e58d-4&from=paste&height=476&id=u9c173630&originHeight=714&originWidth=1545&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=254386&status=done&style=none&taskId=u1440e16c-85e7-42f7-900b-36a11fde5a1&title=&width=1030)

