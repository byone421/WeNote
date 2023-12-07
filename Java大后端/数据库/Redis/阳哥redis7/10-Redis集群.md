# 是什么
由于数据量过大，单个Master复制集难以承担，因此需要对多个复制集进行集群，形成水平扩展每个复制集只负责存储整个数据集的一部分，这就是Redis的集群，其作用是提供在多个Redis节点间共享数据的程序集。
**官网：**
[https://redis.io/docs/reference/cluster-spec](https://redis.io/docs/reference/cluster-spec)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696686225047-1fff2b0b-a852-4a67-975f-69c2c98f3c85.png#averageHue=%23d8cbb1&clientId=uaa1589d0-f50b-4&from=paste&height=445&id=u4f851378&originHeight=668&originWidth=1136&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=499226&status=done&style=none&taskId=u30e888d4-94ab-4c00-9541-78501e24d32&title=&width=757.3333333333334)
# 能干嘛
**1.Redis集群支持多个Master，每个Master又可以挂载多个Slave**

1. 读写分离
2. 支持数据的高可用
3. 支持海量数据的读写存储操作

**2.由于Cluster自带Sentinel的故障转移机制，内置了高可用的支持，无需再去使用哨兵功能**
**3.客户端与Redis的节点连接，不再需要连接集群中所有的节点，只需要任意连接集群中的可用节点即可**
**4.槽位slot负责分配到各个物理服务节点，由对应的集群来负责维护节点、插槽和数据之间的关系**
# 集群算法-分片-槽位slot
## 官网出处
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696686541140-d7b9d4f4-f919-444d-a914-afea1dc9ccb3.png#averageHue=%23e1e3d5&clientId=uaa1589d0-f50b-4&from=paste&height=501&id=u36fea9fa&originHeight=751&originWidth=1415&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=446492&status=done&style=none&taskId=udd90c76b-8a36-4b9c-ac80-bafa7dc1054&title=&width=943.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696686562433-c7bc7cba-37f7-4896-8b86-5dce416903b1.png#averageHue=%23f4f3f1&clientId=uaa1589d0-f50b-4&from=paste&height=411&id=u95f05ceb&originHeight=616&originWidth=1660&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=450469&status=done&style=none&taskId=ufecd9330-9907-4044-8869-86b61fe16ce&title=&width=1106.6666666666667)
## 集群的槽位Slot
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696686862876-c0a2206f-c8b1-4811-b114-27ff3f348c92.png#averageHue=%23f8f2f1&clientId=uaa1589d0-f50b-4&from=paste&height=126&id=u1fc5f4d5&originHeight=189&originWidth=1293&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=110122&status=done&style=none&taskId=u76dcf907-9258-4815-8ea6-24bf8f86375&title=&width=862)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696686870853-14310c23-c6a9-4467-90f7-ac4403eb40d2.png#averageHue=%23f2ebea&clientId=uaa1589d0-f50b-4&from=paste&height=423&id=u5ca907b3&originHeight=635&originWidth=881&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=165878&status=done&style=none&taskId=u23752bb4-6067-40b6-8358-1a39dca1891&title=&width=587.3333333333334)
## 集群的分片
**分片是什么：**
使用Redis集群时我们会将存储的数据分散到多台redis机器上，这称为分片。简言之，集群中的每个Redis实例都被认为是整个数据的一个分片。
**如何找到给定key的分片：**
为了找到给定key的分片，我们对key进行CRC16(key)算法处理并通过对总分片数量取模。然后，使用确定性哈希函数，这意味着给定的key将多次始终映射到同一个分片，我们可以推断将来读取特定key的位置。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696686981361-8cef83f5-8853-4993-9a09-75e159f7bd6f.png#averageHue=%23f2ebeb&clientId=uaa1589d0-f50b-4&from=paste&height=415&id=u4dfdc274&originHeight=623&originWidth=893&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=165820&status=done&style=none&taskId=ue4787c0c-272c-4fb1-a4e8-6951fc454c1&title=&width=595.3333333333334)
## 他两的优势
最大优势，方便扩缩容和数据分派查找
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696687144420-0f557459-1606-476d-8691-ec48e0bf6c65.png#averageHue=%23d2bea6&clientId=uaa1589d0-f50b-4&from=paste&height=85&id=ue5566ffb&originHeight=127&originWidth=1308&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=143772&status=done&style=none&taskId=u9e034e10-b134-44bb-b114-fb7eba24fef&title=&width=872)
## slot槽位映射，一般业界有3种解决方案
### hash取余分区
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696687530484-dfd118ee-213f-4272-ba95-64102cf3bc90.png#averageHue=%23f5efef&clientId=uaa1589d0-f50b-4&from=paste&height=358&id=u5f5d049b&originHeight=537&originWidth=657&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=84958&status=done&style=none&taskId=u6f0d77bf-fe6b-49ca-9c8a-af9fb829ead&title=&width=438)
2亿条记录就是2亿个k,v，我们单机不行必须要分布式多机，假设有3台机器构成一个集群，用户每次读写操作都是根据公式：hash(key) % N个机器台数，计算出哈希值，用来决定数据映射到哪一个节点上。
**优点：**
简单粗暴，直接有效，只需要预估好数据规划好节点，例如3台、8台、10台，就能保证一段时间的数据支撑。使用Hash算法让固定的一部分请求落到同一台服务器上，这样每台服务器固定处理一部分请求（并维护这些请求的信息），起到负载均衡+分而治之的作用。
**缺点：**
原来规划好的节点，进行扩容或者缩容就比较麻烦了额，不管扩缩，每次数据变动导致节点有变动，映射关系需要重新进行计算，在服务器个数固定不变时没有问题，如果需要弹性扩容或故障停机的情况下，原来的取模公式就会发生变化：Hash(key)/3会变成Hash(key) /?。此时地址经过取余运算的结果将发生很大变化，根据公式获取的服务器也会变得不可控。某个redis机器宕机了，由于台数数量变化，会导致hash取余全部数据重新洗牌。
### 一致性哈希算法分区
**是什么：**
一致性Hash算法背景：一致性哈希算法在1997年由麻省理工学院中提出的，设计目标是为了解决
分布式缓存数据变动和映射问题，某个机器宕机了，分母数量改变了，自然取余数不OK了。
**能干嘛：**
提出一致性Hash解决方案目的是当服务器个数发生变动时，尽量减少影响客户端到服务器的映射关系
**三大步骤：**
**1.算法构建一致性哈希环**
一致性哈希环：
一致性哈希算法必然有个hash函数并按照算法产生hash值，这个算法的所有可能哈希值会构成一个全量集，这个集合可以成为一个hash空间[0,2^32-1]，这个是一个线性空间，但是在算法中，我们通过适当的逻辑控制将它首尾相连(0 = 2^32),这样让它逻辑上形成了一个环形空间。
它也是按照使用取模的方法，前面笔记介绍的节点取模法是对节点（服务器）的数量进行取模。而一致性Hash算法是对2^32取模，简单来说，一致性Hash算法将整个哈希值空间组织成一个虚拟的圆环，如假设某哈希函数H的值空间为0-2^32-1（即哈希值是一个32位无符号整形），整个哈希环如下图：整个空间按顺时针方向组织，圆环的正上方的点代表0，0点右侧的第一个点代表1，以此类推，2、3、4、……直到2^32-1，也就是说0点左侧的第一个点代表2^32-1， 0和2^32-1在零点中方向重合，我们把这个由2^32个点组成的圆环称为Hash环。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696688343498-86785bea-dae4-47db-9db5-8694dad11364.png#averageHue=%23d0e5e4&clientId=uaa1589d0-f50b-4&from=paste&height=381&id=u750a154a&originHeight=571&originWidth=538&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=66138&status=done&style=none&taskId=ucfda94fb-919d-4cda-928c-4471652613f&title=&width=358.6666666666667)
**2.redis服务器IP节点映射**
节点映射：
将集群中各个IP节点映射到环上的某一个位置。
将各个服务器使用Hash进行一个哈希，具体可以选择服务器的IP或主机名作为关键字进行哈希，这样每台机器就能确定其在哈希环上的位置。假如4个节点NodeA、B、C、D，经过IP地址的哈希函数计算(hash(ip))，使用IP地址哈希后在环空间的位置如下： 
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696688431829-3cfc2372-e473-4ed2-9478-92ff57f491ee.png#averageHue=%23f0f2eb&clientId=uaa1589d0-f50b-4&from=paste&height=421&id=u5bda4638&originHeight=632&originWidth=675&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=121279&status=done&style=none&taskId=ufb333c5d-68a0-46b1-83ed-2948bf9d060&title=&width=450)
**3.key落到服务器的落键规则**
当我们需要存储一个kv键值对时，首先计算key的hash值，hash(key)，将这个key使用相同的函数Hash计算出哈希值并确定此数据在环上的位置，从此位置沿环顺时针“行走”，第一台遇到的服务器就是其应该定位到的服务器，并将该键值对存储在该节点上。
如我们有Object A、Object B、Object C、Object D四个数据对象，经过哈希计算后，在环空间上的位置如下：根据一致性Hash算法，数据A会被定为到Node A上，B被定为到Node B上，C被定为到Node C上，D被定为到Node D上。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696688474399-4219afd7-0d77-4073-9800-dba7f797ea3f.png#averageHue=%23dbe7d7&clientId=uaa1589d0-f50b-4&from=paste&height=471&id=u0f28014c&originHeight=707&originWidth=689&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=198779&status=done&style=none&taskId=u90304c3f-01aa-4dc4-a6e8-3261b02c6e6&title=&width=459.3333333333333)

**优点**

1. 一致性哈希算法的容错性

假设Node C宕机，可以看到此时对象A、B、D不会受到影响。一般的，在一致性Hash算法中，如果一台服务器不可用，则受影响的数据仅仅是此服务器到其环空间中前一台服务器（即沿着逆时针方向行走遇到的第一台服务器）之间数据，其它不会受到影响。简单说，就是C挂了，受到影响的只是B、C之间的数据且这些数据会转移到D进行存储。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696688623604-e60635c2-e4d7-4bbb-bc35-933b0de76788.png#averageHue=%23e8efe4&clientId=uaa1589d0-f50b-4&from=paste&height=487&id=ue10f43e7&originHeight=731&originWidth=700&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=215457&status=done&style=none&taskId=ubafc2bc7-4c86-4dcf-9a87-d455ac44193&title=&width=466.6666666666667)

2. 一致性哈希算法的扩展性

数据量增加了，需要增加一台节点NodeX，X的位置在A和B之间，那收到影响的也就是A到X之间的数据，重新把A到X的数据录入到X上即可，
不会导致hash取余全部数据重新洗牌。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696688657324-81a50a52-38c8-447e-a616-39a3578408d2.png#averageHue=%23e8efe4&clientId=uaa1589d0-f50b-4&from=paste&height=427&id=u2c565521&originHeight=641&originWidth=702&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=219536&status=done&style=none&taskId=u9b5a29d2-36fb-4b5d-9b96-93beb4589f2&title=&width=468)

**缺点**
一致性哈希算法的数据倾斜问题
一致性Hash算法在服务节点太少时，容易因为节点分布不均匀而造成数据倾斜（被缓存的对象大部分集中缓存在某一台服务器上）问题，例如系统中只有两台服务器：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696688707674-b63c9eae-834a-4bda-ace6-a946c5285244.png#averageHue=%23cee6e3&clientId=uaa1589d0-f50b-4&from=paste&height=329&id=u5319bb47&originHeight=493&originWidth=517&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=62687&status=done&style=none&taskId=u5dbb1354-6667-413c-921b-0dd7ac1a887&title=&width=344.6666666666667)

**小总结：**
1.为了在节点数目发生改变时尽可能少的迁移数据
2.将所有的存储节点排列在收尾相接的Hash环上，每个key在计算Hash后会顺时针找到临近的存储节点存放。
而当有节点加入或退出时仅影响该节点在Hash环上顺时针相邻的后续节点。  
3.优点：加入和删除节点只影响哈希环中顺时针方向的相邻的节点，对其他节点无影响。
4.缺点：数据的分布和节点的位置有关，因为这些节点不是均匀的分布在哈希环上的，所以数据在进行存储时达不到均匀分布的效果。
### 哈希槽分区
**是什么**
1.为什么出现
致性哈希算法的数据倾斜问题。哈希槽实质就是一个数组，数组[0,2^14 -1]形成hash slot空间。
2. 能干什么
解决均匀分配的问题，在数据和节点之间又加入了一层，把这层称为哈希槽（slot），用于管理数据和节点之间的关系，现在就相当于节点上放的是槽，槽里放的是数据。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696689093345-e9edb9d3-da6d-403e-89ea-7396426cf740.png#averageHue=%23f5ece9&clientId=uaa1589d0-f50b-4&from=paste&height=149&id=uc570c6c1&originHeight=223&originWidth=675&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=27914&status=done&style=none&taskId=uea388c09-ae38-48b5-9144-03c603824bc&title=&width=450)
槽解决的是粒度问题，相当于把粒度变大了，这样便于数据移动。哈希解决的是映射问题，使用key的哈希值来计算所在的槽，便于数据分配
3.多少个hash槽
一个集群只能有16384个槽，编号0-16383（0-2^14-1）。这些槽会分配给集群中的所有主节点，分配策略没有要求。
集群会记录节点和槽的对应关系，解决了节点和槽的关系后，接下来就需要对key求哈希值，然后对16384取模，余数是几key就落入对应的槽里。HASH_SLOT = CRC16(key) mod 16384。以槽为单位移动数据，因为槽的数目是固定的，处理起来比较容易，这样数据移动问题就解决了。
HASH SLOT = CRC16(key) mod 16384
**哈希槽计算**
Redis 集群中内置了 16384 个哈希槽，redis 会根据节点数量大致均等的将哈希槽映射到不同的节点。当需要在 Redis 集群中放置一个 key-value时，redis先对key使用crc16算法算出一个结果然后用结果对16384求余数[ CRC16(key) % 16384]，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，也就是映射到某个节点上。如下代码，key之A 、B在Node2， key之C落在Node3上
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696689380019-9e8bfd97-bf8e-4192-9d1d-b5c028643818.png#averageHue=%23e6e1a4&clientId=uaa1589d0-f50b-4&from=paste&height=309&id=u58cb94d1&originHeight=463&originWidth=1606&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=211993&status=done&style=none&taskId=ua8f2129d-1c46-4a97-b89c-9eb862a8225&title=&width=1070.6666666666667)
## 为什么redis集群的最大槽数是16384个 ?
Redis集群并没有使用一致性hash而是引入了哈希槽的概念。Redis 集群有16384个哈希槽，每个key通过CRC16校验后对16384取模来决定放置哪个槽，集群的每个节点负责一部分hash槽。但为什么哈希槽的数量是16384（2^14）个呢？
> CRC16算法产生的hash值有16bit，该算法可以产生2^16=65536个值。
> 换句话说值是分布在0~65535之间，有更大的65536不用为什么只用16384就够？
> 作者在做mod运算的时候，为什么不mod65536，而选择mod16384？  HASH_SLOT = CRC16(key) mod 65536为什么没启用

[https://github.com/redis/redis/issues/2576](https://github.com/redis/redis/issues/2576)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696690332873-04ae77f6-b3fc-415d-aabe-3b4262673d47.png#averageHue=%23d6c5ad&clientId=uaa1589d0-f50b-4&from=paste&height=442&id=uc44081c4&originHeight=663&originWidth=1141&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=242013&status=done&style=none&taskId=u1c18ac5b-d735-4582-b65d-ba7311d052c&title=&width=760.6666666666666)
**说明一：**
正常的心跳数据包带有节点的完整配置，可以用幂等方式用旧的节点替换旧节点，以便更新旧的配置。
这意味着它们包含原始节点的插槽配置，该节点使用2k的空间和16k的插槽，但是会使用8k的空间（使用65k的插槽）。
同时，由于其他设计折衷，Redis集群不太可能扩展到1000个以上的主节点。
因此16k处于正确的范围内，以确保每个主机具有足够的插槽，最多可容纳1000个矩阵，但数量足够少，可以轻松地将插槽配置作为原始位图传播。请注意，在小型群集中，位图将难以压缩，因为当N较小时，位图将设置的slot / N位占设置位的很大百分比。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696690382944-741608f6-426a-45ca-bbbd-b581f0273571.png#averageHue=%2340454b&clientId=uaa1589d0-f50b-4&from=paste&height=366&id=u6b621b65&originHeight=549&originWidth=951&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=471050&status=done&style=none&taskId=u5f415a2b-2f02-49da-a400-a9dd7868c4b&title=&width=634)
**说明二：**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696690450692-a0d43565-4e19-4b38-ae90-ae9ad44623d6.png#averageHue=%233c4147&clientId=uaa1589d0-f50b-4&from=paste&height=361&id=u676a63d0&originHeight=542&originWidth=945&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=470712&status=done&style=none&taskId=ud10306db-886a-4f66-b317-e64b86dbe7a&title=&width=630)
(1)如果槽位为65536，发送心跳信息的消息头达8k，发送的心跳包过于庞大。
在消息头中最占空间的是myslots[CLUSTER_SLOTS/8]。 当槽位为65536时，这块的大小是: 65536÷8÷1024=8kb 
在消息头中最占空间的是myslots[CLUSTER_SLOTS/8]。 当槽位为16384时，这块的大小是: 16384÷8÷1024=2kb 
因为每秒钟，redis节点需要发送一定数量的ping消息作为心跳包，如果槽位为65536，这个ping消息的消息头太大了，浪费带宽。
(2)redis的集群主节点数量基本不可能超过1000个。
集群节点越多，心跳包的消息体内携带的数据越多。如果节点过1000个，也会导致网络拥堵。因此redis作者不建议redis cluster节点数量超过1000个。 那么，对于节点数在1000以内的redis cluster集群，16384个槽位够用了。没有必要拓展到65536个。
(3)槽位越小，节点少的情况下，压缩比高，容易传输
Redis主节点的配置信息中它所负责的哈希槽是通过一张bitmap的形式来保存的，在传输过程中会对bitmap进行压缩，但是如果bitmap的填充率slots / N很高的话(N表示节点数)，bitmap的压缩率就很低。 如果节点数很少，而哈希槽数量很多的话，bitmap的压缩率就很低。 
**计算结论：**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696690486829-032df60c-0fc1-4f4d-986e-d76cc6319b7f.png#averageHue=%23f7f4f1&clientId=uaa1589d0-f50b-4&from=paste&height=423&id=u366b96c5&originHeight=634&originWidth=1595&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=384208&status=done&style=none&taskId=u35e18f53-6b88-4d8e-96b9-4eeb26ece8c&title=&width=1063.3333333333333)
## Redis集群不保证强一致性，这意味着在特定的条件下，Redis集群可能会丢掉一些被系统收到的写入请求命令
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696690535080-10b5fea1-4bde-40be-8f86-f1e362c026b5.png#averageHue=%23fefdfc&clientId=uaa1589d0-f50b-4&from=paste&height=227&id=u7a7edcc0&originHeight=340&originWidth=1000&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=137478&status=done&style=none&taskId=ucc79692e-2354-4f35-b4b8-33a8ceaed45&title=&width=666.6666666666666)
# 集群案例
## 3主3从集群的配置

1. **IP: 192.168.111.175+端门6381/端门6382**

/myredis/cluster/redisCluster6381.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 6381
logfile "/myredis/cluster/cluster6381.log"
pidfile /myredis/cluster6381.pid
dir /myredis/cluster
dbfilename dump6381.rdb
appendonly yes
appendfilename "appendonly6381.aof"
requirepass 111111
masterauth 111111
 
cluster-enabled yes
cluster-config-file nodes-6381.conf
cluster-node-timeout 5000
```
/myredis/cluster/redisCluster6382.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 6382
logfile "/myredis/cluster/cluster6382.log"
pidfile /myredis/cluster6382.pid
dir /myredis/cluster
dbfilename dump6382.rdb
appendonly yes
appendfilename "appendonly6382.aof"
requirepass 111111
masterauth 111111
 
cluster-enabled yes
cluster-config-file nodes-6382.conf
cluster-node-timeout 5000
```

2. **IP: 192.168.111.172+端门6383/端门6384**

/myredis/cluster/redisCluster6383.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 6383
logfile "/myredis/cluster/cluster6383.log"
pidfile /myredis/cluster6383.pid
dir /myredis/cluster
dbfilename dump6383.rdb
appendonly yes
appendfilename "appendonly6383.aof"
requirepass 111111
masterauth 111111

cluster-enabled yes
cluster-config-file nodes-6383.conf
cluster-node-timeout 5000
```
/myredis/cluster/redisCluster6384.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 6384
logfile "/myredis/cluster/cluster6384.log"
pidfile /myredis/cluster6384.pid
dir /myredis/cluster
dbfilename dump6384.rdb
appendonly yes
appendfilename "appendonly6384.aof"
requirepass 111111
masterauth 111111
 
cluster-enabled yes
cluster-config-file nodes-6384.conf
cluster-node-timeout 5000
```

3. **IP: 192.168.111.174+端门6385/端门6386**

/myredis/cluster/redisCluster6385.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 6385
logfile "/myredis/cluster/cluster6385.log"
pidfile /myredis/cluster6385.pid
dir /myredis/cluster
dbfilename dump6385.rdb
appendonly yes
appendfilename "appendonly6385.aof"
requirepass 111111
masterauth 111111
 
cluster-enabled yes
cluster-config-file nodes-6385.conf
cluster-node-timeout 5000
```
/myredis/cluster/redisCluster6386.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 6386
logfile "/myredis/cluster/cluster6386.log"
pidfile /myredis/cluster6386.pid
dir /myredis/cluster
dbfilename dump6386.rdb
appendonly yes
appendfilename "appendonly6386.aof"
requirepass 111111
masterauth 111111
 
cluster-enabled yes
cluster-config-file nodes-6386.conf
cluster-node-timeout 5000
```
依次启动6台服务器：
redis-server /myredis/cluster/redisCluster6381.conf
......
### 通过redis-cli命令为6台机器构建集群关系
redis-cli -a 111111 --cluster create --cluster-replicas 1 192.168.111.175:6381 192.168.111.175:6382 192.168.111.172:6383 192.168.111.172:6384 192.168.111.174:6385 192.168.111.174:6386
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696768534833-ea1214b9-d77a-4579-8a11-94a4f3d5488b.png#averageHue=%23c8eda4&clientId=u09176ee9-49d6-4&from=paste&height=800&id=uc73dad02&originHeight=1200&originWidth=1366&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=754358&status=done&style=none&taskId=ua203c324-cb6d-4751-93dd-eecac052684&title=&width=910.6666666666666)
### 链接进入6381作为切入点，查看并检验集群状态
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696768585909-4d60ccdf-1005-421d-b3af-ee0598d0c99b.png#averageHue=%23fef9f9&clientId=u09176ee9-49d6-4&from=paste&height=379&id=u21b51ad6&originHeight=569&originWidth=1209&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=153289&status=done&style=none&taskId=u6ee90343-1e38-407e-b28d-8207ab2bf1b&title=&width=806)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696768592871-8915e42b-8c04-4f74-b975-744843ebfea4.png#averageHue=%23fefcfc&clientId=u09176ee9-49d6-4&from=paste&height=228&id=u4f73f40c&originHeight=342&originWidth=1618&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=346794&status=done&style=none&taskId=u5aec11d8-b346-4e3d-9319-8044f12213b&title=&width=1078.6666666666667)
#### info replication
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696768640663-39028ee6-674c-45db-8048-9fb6b1295596.png#averageHue=%23fefafa&clientId=u09176ee9-49d6-4&from=paste&height=259&id=u85f10595&originHeight=388&originWidth=809&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=95529&status=done&style=none&taskId=u846747a8-a3c9-4023-a66e-23fd5503716&title=&width=539.3333333333334)
#### cluster info
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696768677847-75b7a6a0-697b-4525-831e-e99166b48489.png#averageHue=%23fefcfc&clientId=u09176ee9-49d6-4&from=paste&height=317&id=ud5536f4e&originHeight=476&originWidth=560&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=168677&status=done&style=none&taskId=uac75de2a-8d46-4d76-a12a-1da4738f649&title=&width=373.3333333333333)
#### cluster nodes
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696768701886-a5fb13b9-d1be-45cd-b2e5-e7dbeba07d4f.png#averageHue=%23ebe9eb&clientId=u09176ee9-49d6-4&from=paste&height=189&id=u38294661&originHeight=284&originWidth=1621&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=275078&status=done&style=none&taskId=u75f26427-b7a1-4368-ab9e-41bdd57ce4d&title=&width=1080.6666666666667)
## 3主3从redis集群读写
### 对6381新增两个key，看看效果如何
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696859338327-e0066592-9945-4df2-8e06-5d8d00dbf220.png#averageHue=%23fdf2f2&clientId=u8c7275b1-0c0d-4&from=paste&height=209&id=u02d08129&originHeight=314&originWidth=533&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=58199&status=done&style=none&taskId=u9bc25a23-eebe-41e9-a61a-ecb22631d79&title=&width=355.3333333333333)
**报错原因：一定注意槽位的范围区间，需要路由到位**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696859370163-30e917d4-9f19-4cb6-b883-4e28bb5989cf.png#averageHue=%23e2e1e8&clientId=u8c7275b1-0c0d-4&from=paste&height=345&id=uce8b7f94&originHeight=517&originWidth=861&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=349972&status=done&style=none&taskId=u64bab3cb-fbf8-48ea-9484-e8c859c8f6e&title=&width=574)
### 如何解决
**加入参数-c，优化路由**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696859464066-bac9c24d-3f5e-4530-a60b-612ad3341667.png#averageHue=%23fef9f9&clientId=u8c7275b1-0c0d-4&from=paste&height=349&id=u32992530&originHeight=524&originWidth=1231&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=121268&status=done&style=none&taskId=u70f5ce31-4ad5-45b0-b912-416bc977c5d&title=&width=820.6666666666666)
### 查看集群信息
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696859538429-73403605-f638-49e5-883e-c6c019b9e837.png#averageHue=%23ece9eb&clientId=u8c7275b1-0c0d-4&from=paste&height=191&id=u5e94b4e3&originHeight=286&originWidth=1637&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=284672&status=done&style=none&taskId=u48d06be0-5748-45ba-8c7e-23c986b35cf&title=&width=1091.3333333333333)
### 查看某个key该属于对应的槽位值CLUSTER KEYSLOT 键名称
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696859591018-fc801f43-eb45-44bd-996c-cede026907e0.png#averageHue=%23fdf5f5&clientId=u8c7275b1-0c0d-4&from=paste&height=301&id=u441f4a1e&originHeight=452&originWidth=1638&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=165248&status=done&style=none&taskId=ub6f798c3-50b3-40fc-bf6f-fdc37ef8316&title=&width=1092)
## 主从容错切换迁移案例
先查看集群
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696859818146-f765d975-13e9-4524-b7d0-585c40f5d494.png#averageHue=%23fdfafa&clientId=u8c7275b1-0c0d-4&from=paste&height=209&id=u9f5d33cf&originHeight=313&originWidth=1629&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=283843&status=done&style=none&taskId=ua0bf12bb-2a76-4061-8b50-b8960945400&title=&width=1086)
6381master假如宕机了，6384是否会上位成为了新的master?
停止主机6381,再次查看集群信息
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696859861600-3dd05098-9ba8-47d3-83fc-3eeee725c8b1.png#averageHue=%23fefbfb&clientId=u8c7275b1-0c0d-4&from=paste&height=331&id=u0dbf496a&originHeight=496&originWidth=1625&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=347518&status=done&style=none&taskId=uf7397ca2-77a3-4da1-b8d6-7668ba84914&title=&width=1083.3333333333333)
6381宕机了，6384上位成为了新的master。
即使6381之后恢复了，也只会以从节点的形式回归
### 集群不保证数据一致性100%OK，一定会有数据丢失情况
Redis集群不保证强一致性，这意味着在特定的条件下，Redis集群
可能会丢掉一些被系统收到的写入请求命令

### 节点从属调整该如何处理
让6381重新做master
执行：CLUSTER FAILOVER
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696860608941-0eecc92f-9827-4437-87c9-49af74cb62f4.png#averageHue=%23fefafa&clientId=u8c7275b1-0c0d-4&from=paste&height=383&id=uae6214be&originHeight=574&originWidth=1526&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=503440&status=done&style=none&taskId=u36526cf1-062f-4398-aedd-595814ee1dd&title=&width=1017.3333333333334)
## 主从扩容案例
### 新建6387、6388两个服务实例配置文件+新建后启动
/myredis/cluster/redisCluster6387.conf
```java
搜索 
180%

便笺
bind 0.0.0.0
daemonize yes
protected-mode no
port 6387
logfile "/myredis/cluster/cluster6387.log"
pidfile /myredis/cluster6387.pid
dir /myredis/cluster
dbfilename dump6387.rdb
appendonly yes
appendfilename "appendonly6387.aof"
requirepass 111111
masterauth 111111

cluster-enabled yes
cluster-config-file nodes-6387.conf
cluster-node-timeout 5000
 
```
/myredis/cluster/redisCluster6388.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 6388
logfile "/myredis/cluster/cluster6388.log"
pidfile /myredis/cluster6388.pid
dir /myredis/cluster
dbfilename dump6388.rdb
appendonly yes
appendfilename "appendonly6388.aof"
requirepass 111111
masterauth 111111
 
cluster-enabled yes
cluster-config-file nodes-6388.conf
cluster-node-timeout 5000
 
```
### 启动87/88两个新的节点实例，此时他们自己都是master
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696860850452-932a5512-83d1-4523-be00-b02e161ead8f.png#averageHue=%23fef5f5&clientId=u8c7275b1-0c0d-4&from=paste&height=141&id=ue1f39744&originHeight=212&originWidth=1087&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=153909&status=done&style=none&taskId=u53f75b47-9afa-491f-996a-ded7364d5e2&title=&width=724.6666666666666)
### 将新增的6387节点(空槽号)作为master节点加入原集群
将新增的6387作为master节点加入原有集群
redis-cli -a 密码 --cluster add-node 自己实际IP地址:6387 自己实际IP地址:6381
6387 就是将要作为master新增节点
6381 就是原来集群节点里面的领路人，相当于6387拜拜6381的码头从而找到组织加入集群
redis-cli -a 111111  --cluster add-node 192.168.111.174:6387 192.168.111.175:6381
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861319950-5c42a7d6-7e33-4b33-a004-f954fa632dde.png#averageHue=%23faf8f8&clientId=u8c7275b1-0c0d-4&from=paste&height=723&id=u5664366b&originHeight=1084&originWidth=1910&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=716418&status=done&style=none&taskId=u8fddd05f-f45d-426f-ad22-07eac88467b&title=&width=1273.3333333333333)
### 检查集群情况第1次
**redis-cli -a 111111 --cluster check 192.168.111.175:6381**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861380062-fc42e81a-d20f-40ce-a2c7-e537e0e4c0d7.png#averageHue=%23fcf6f6&clientId=u8c7275b1-0c0d-4&from=paste&height=615&id=ua08a962c&originHeight=922&originWidth=1512&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=294802&status=done&style=none&taskId=u3b83c697-522a-4a3f-89d1-081d5e5b5f9&title=&width=1008)
### 重新分派槽号(reshard)
命令:redis-cli -a 密码 --cluster reshard IP地址:端口号
redis-cli -a 密码 --cluster reshard 192.168.111.175:6381
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861496560-e53ef217-92ce-4f0f-8848-df1e4b941bf5.png#averageHue=%23fcf4f4&clientId=u8c7275b1-0c0d-4&from=paste&height=606&id=u45899a1b&originHeight=909&originWidth=1198&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=439137&status=done&style=none&taskId=u6574c545-e37a-4847-8a99-c38fc987b03&title=&width=798.6666666666666)
### 槽号分派说明
为什么6387是3个新的区间，以前的还是连续？
重新分配成本太高，所以前3家各自匀出来一部分，从6381/6383/6385三个旧节点分别匀出1364个坑位给新节点6387
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861577103-ec071723-960d-4a73-800e-38605eddbb64.png#averageHue=%23faf0f0&clientId=u8c7275b1-0c0d-4&from=paste&height=594&id=ufc82e2d4&originHeight=891&originWidth=1345&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=693352&status=done&style=none&taskId=u0be14010-cfc7-4c1b-b154-a7fa1055928&title=&width=896.6666666666666)
### 为主节点6387分配从节点6388
命令：redis-cli -a 密码 --cluster add-node ip:新slave端口 ip:新master端口 --cluster-slave --cluster-master-id 新主机节点ID
redis-cli -a 111111 --cluster add-node 192.168.111.174:6388 192.168.111.174:6387 --cluster-slave --cluster-master-id 4feb6a7ee0ed2b39ff86474cf4189ab2a554a40f-------这个是6387的编号，按照自己实际情况
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861660896-20881060-cc32-43bb-bcb0-381250088a07.png#averageHue=%23fcf7f7&clientId=u8c7275b1-0c0d-4&from=paste&height=599&id=u624b4e87&originHeight=898&originWidth=2262&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=519319&status=done&style=none&taskId=u35f02779-5348-42fc-9084-9ef11da3ba0&title=&width=1508)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861673825-9ed4f943-333e-494b-b317-1818a44e7f19.png#averageHue=%23f0e2e2&clientId=u8c7275b1-0c0d-4&from=paste&height=173&id=u73f26cc2&originHeight=260&originWidth=1293&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=95328&status=done&style=none&taskId=ufcaa6141-961f-45ff-87fc-7f92c3f8b90&title=&width=862)
### 再次检查集群情况
redis-cli --cluster check 真实ip地址:6381
redis-cli -a 111111 --cluster check 192.168.111.175:6381
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861746266-d6b13d24-bc44-4137-843a-7f39ea52c3b2.png#averageHue=%23fcf9f9&clientId=u8c7275b1-0c0d-4&from=paste&height=565&id=uba6a06a3&originHeight=847&originWidth=1458&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=501764&status=done&style=none&taskId=u1580db94-a523-475e-9787-7855a93bc35&title=&width=972)
## 主从缩容案例
目的：6387和6388下线
### 检查集群情况第一次，先获得从节点6388的节点ID
redis-cli -a 密码 --cluster check 192.168.111.174:6388
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861886168-c4aa7753-cdde-400c-b317-497e656ca531.png#averageHue=%23bfef90&clientId=u8c7275b1-0c0d-4&from=paste&height=452&id=u1da614bf&originHeight=678&originWidth=1332&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=435119&status=done&style=none&taskId=uc590ccf2-f7d8-4816-a305-310bf84a532&title=&width=888)
### 从集群中将4号从节点6388删除
命令：redis-cli -a 密码 --cluster del-node ip:从机端口 从机6388节点ID
 
redis-cli -a 111111 --cluster del-node 192.168.111.174:6388 218e7b8b4f81be54ff173e4776b4f4faaf7c13da
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861962371-7c9c6268-4111-4b2d-8ad9-91051c29ac3e.png#averageHue=%23e8cdcd&clientId=u8c7275b1-0c0d-4&from=paste&height=139&id=u048aa580&originHeight=209&originWidth=2014&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=213101&status=done&style=none&taskId=u35abcb1d-4e38-4919-a5c2-d2c75c84b34&title=&width=1342.6666666666667)
redis-cli -a 111111 --cluster check 192.168.111.174:6385
检查一下发现，6388被删除了，只剩下7台机器了。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696861991526-514f8496-2f71-4937-b557-99855f0e8a1a.png#averageHue=%23fcf9f9&clientId=u8c7275b1-0c0d-4&from=paste&height=621&id=u5f9f1183&originHeight=931&originWidth=1206&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=459027&status=done&style=none&taskId=uce7738de-bc7a-474b-a9f8-fc2ecffbd0f&title=&width=804)
### 将6387的槽号清空，重新分配，本例将清出来的槽号都给6381
redis-cli -a 111111 --cluster reshard 192.168.111.175:6381
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862437655-6b15314a-542f-4d3f-bf8e-822b61379252.png#averageHue=%23fbf6f6&clientId=u8c7275b1-0c0d-4&from=paste&height=602&id=u27003d6e&originHeight=903&originWidth=1541&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=763382&status=done&style=none&taskId=ucf6a6af2-90eb-43fe-9b8b-f4a3d03978b&title=&width=1027.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862455920-c1dbc8cb-a81a-49b9-98db-ac631b5bdc9f.png#averageHue=%23fdfafa&clientId=u8c7275b1-0c0d-4&from=paste&height=666&id=ueb0ab3b6&originHeight=999&originWidth=1918&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=639335&status=done&style=none&taskId=u328cb6ab-bafc-48ff-8d20-008e9bf03e2&title=&width=1278.6666666666667)
### 检查集群情况第二次
 redis-cli -a 111111 --cluster check 192.168.111.175:6381 
4096个槽位都指给6381，它变成了8192个槽位，相当于全部都给6381了，不然要输入3次，一锅端
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862517876-9543744d-778f-4cd0-99d3-bd6256b0fc59.png#averageHue=%23e1e7d9&clientId=u8c7275b1-0c0d-4&from=paste&height=202&id=u478ffc3e&originHeight=303&originWidth=1522&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=212489&status=done&style=none&taskId=u9f736d9e-e312-4f1d-89d4-f3b5ee55b03&title=&width=1014.6666666666666)
### 将6387删除
命令：redis-cli -a 密码 --cluster del-node ip:端口 6387节点ID
 
redis-cli -a 111111 --cluster del-node 192.168.111.174:6387 4feb6a7ee0ed2b39ff86474cf4189ab2a554a40f
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862572771-e9ca6f51-a099-4787-a122-61faa0b20d29.png#averageHue=%23edd8d8&clientId=u8c7275b1-0c0d-4&from=paste&height=162&id=u303cf7b5&originHeight=243&originWidth=2231&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=244865&status=done&style=none&taskId=u4a0e8ac4-f781-4f1d-a4e1-8a3bb9bcff5&title=&width=1487.3333333333333)
### 检查集群情况第三次,6387/6388被彻底祛除
redis-cli -a 111111 --cluster check 192.168.111.175:6381
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862614398-361f271e-d237-4eb2-ad55-b4460e03c4de.png#averageHue=%23fcf4f4&clientId=u8c7275b1-0c0d-4&from=paste&height=618&id=u2b9a07ec&originHeight=927&originWidth=1602&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=487541&status=done&style=none&taskId=uc74f53e3-5222-42c2-b44a-66075846769&title=&width=1068)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862622181-aabf487f-91a5-4c9a-9c5b-4516614c6fe8.png#averageHue=%23fefbfb&clientId=u8c7275b1-0c0d-4&from=paste&height=473&id=u89772a4d&originHeight=709&originWidth=1567&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=327225&status=done&style=none&taskId=u2e7f4626-0d09-450b-96c2-a522e1d8ac7&title=&width=1044.6666666666667)
# 集群常用操作命令和CRC16算法分析
## 不在同一个slot槽位下的多键操作支持不好，通识占位符登场
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862698002-6175c545-c987-44b8-9e0a-e5ee9bc9bba2.png#averageHue=%23fef9f9&clientId=u8c7275b1-0c0d-4&from=paste&height=467&id=uc4c0bd30&originHeight=701&originWidth=1182&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=247980&status=done&style=none&taskId=uca37627a-36e7-4279-9b1d-8d259d3641a&title=&width=788)
不在同一个slot槽位下的键值无法使用mset、mget等多键操作
可以通过{}来定义同一个组的概念，使key中{}内相同内容的键值对放到一个slot槽位去，对照下图类似k1k2k3都映射为x，自然槽位一样
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862865641-908ad4ce-96e7-44f5-b8ea-be4171673257.png#averageHue=%23fef7f7&clientId=u8c7275b1-0c0d-4&from=paste&height=331&id=u2702b5db&originHeight=497&originWidth=1234&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=239482&status=done&style=none&taskId=u4ef08d24-a5f2-4b9a-84ce-b298c1218f2&title=&width=822.6666666666666)
### Redis集群有16384个哈希槽，每个key通过CRC16校验后对16384取模来决定放置哪个槽.
### ![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696862949862-a5979ec2-906e-4a03-a706-7450d6d3df4e.png#averageHue=%232a2b24&clientId=u8c7275b1-0c0d-4&from=paste&height=681&id=u34f2cb14&originHeight=1022&originWidth=1457&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=426258&status=done&style=none&taskId=u8f6b99a5-993c-4776-9136-4df490a7056&title=&width=971.3333333333334)
## 常用命令
### 集群是否完整才能对外提供服务
cluster-require-full-coverage
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696863016796-74d7a2e8-907c-49a5-a284-189b38e28b07.png#averageHue=%23fdf3e8&clientId=u8c7275b1-0c0d-4&from=paste&height=126&id=u88d227b8&originHeight=189&originWidth=1521&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=92207&status=done&style=none&taskId=u50fbda34-7236-499b-9627-cc9935d4494&title=&width=1014)
默认YES，现在集群架构是3主3从的redis cluster由3个master平分16384个slot，每个master的小集群负责1/3的slot，对应一部分数据。
cluster-require-full-coverage： 默认值 yes , 即需要集群完整性，方可对外提供服务 通常情况，如果这3个小集群中，任何一个（1主1从）挂了，你这个集群对外可提供的数据只有2/3了， 整个集群是不完整的， redis 默认在这种情况下，是不会对外提供服务的。

如果你的诉求是，集群不完整的话也需要对外提供服务，需要将该参数设置为no ，这样的话你挂了的那个小集群是不行了，但是其他的小集群仍然可以对外提供服务。

### CLUSTER COUNTKEYSINSLOT 槽位数字编号
1 槽位被占位 0 槽位空着
### CLUSTER KEYSLOT 键名称
该键应该存在哪个槽位上

