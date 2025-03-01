官网地址：[https://redis.io/docs/management/replication](https://redis.io/docs/management/replication)
主从复制，master以写为主，Slave以读为主
当master数据变化的时候，自动将新的数据异步同步到其它slave数据库
# 怎么玩
master如果配置了requirepass参数，需要密码登陆
那么slave就要配置masterauth来设置校验密码
否则的话master会拒绝slave的访问请求
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692499857272-1c40f359-e2d7-47c3-b8e6-387acbd2afea.png#averageHue=%23e1c29f&clientId=uafcddd5b-62cc-4&from=paste&height=147&id=u16cc9e8b&originHeight=220&originWidth=609&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=80811&status=done&style=none&taskId=u3bc8e828-e710-478d-99d4-bf421e8672a&title=&width=406)
## 基本操作命令
**info replication**
可以查看复制节点的主从关系和配置信息
**replicaof 主库IP 主库端口**
一般写入进redis.conf配置文件内
**slaveof 主库IP 主库端**
每次与master断开之后，都需要重新连接，除非你配置进redis.conf文件
在运行期间修改slave节点的信息，如果该数据库已经是某个主数据库的从数据库
那么会停止和原主数据库的同步关系转而和新的主数据库同步，重新拜码头
**slaveof no one**
使当前数据库停止与其他数据库的同步，
转成主数据库，自立为王
# 案例
一个master两个slave，三边网络相互ping通且注意防火墙配置
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692501254841-4eee472f-a673-4e31-809c-19db9365d98a.png#averageHue=%23faf7f0&clientId=uafcddd5b-62cc-4&from=paste&height=274&id=ud816d16f&originHeight=411&originWidth=865&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=119746&status=done&style=none&taskId=uf4009e57-5ff4-422b-843c-ad370c92206&title=&width=576.6666666666666)
## 修改配置文件细节
redis6379.conf为例，步骤
**1.开启daemonize yes**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502204643-80ae30b2-b13b-4db2-bd4f-4960aced106b.png#averageHue=%23fde1df&clientId=uafcddd5b-62cc-4&from=paste&height=49&id=u8ff80e8f&originHeight=73&originWidth=377&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=18105&status=done&style=none&taskId=uda86f9f5-34e2-47e7-b4eb-8dee42bff8e&title=&width=251.33333333333334)
**2.注释掉bind 127.0.0.1**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502216121-7d27f632-0a75-46e6-874b-206b6360e4b4.png#averageHue=%23dfba94&clientId=uafcddd5b-62cc-4&from=paste&height=62&id=uc0010d41&originHeight=93&originWidth=470&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=15601&status=done&style=none&taskId=u06c268bd-8f02-441a-aa48-8c9dc28e112&title=&width=313.3333333333333)
**3.protected-mode no**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502229340-7627ca2f-d8dd-446c-a810-c372f4b949f1.png#averageHue=%23e2c5a2&clientId=uafcddd5b-62cc-4&from=paste&height=84&id=u4c875c7f&originHeight=126&originWidth=602&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=44634&status=done&style=none&taskId=uf96a2c41-ac35-4443-a0a3-a7d1c22314c&title=&width=401.3333333333333)
**4.指定端口**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502243891-95252fc7-8096-4379-baf0-81e7d3b826ce.png#averageHue=%23e0c29f&clientId=uafcddd5b-62cc-4&from=paste&height=71&id=uf0d96408&originHeight=107&originWidth=700&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=36355&status=done&style=none&taskId=u2426e9c9-2a93-4cd9-a215-5dfae2dea80&title=&width=466.6666666666667)
**5.指定当前工作目录，dir**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502258264-23c0e9d1-8fd8-486b-8de7-122e71610fc2.png#averageHue=%23fcebe9&clientId=uafcddd5b-62cc-4&from=paste&height=71&id=ud1a37210&originHeight=106&originWidth=475&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=30224&status=done&style=none&taskId=u99d5d8d5-e92e-4710-adcb-b5af02ff6d8&title=&width=316.6666666666667)
**6.pid文件名字,pidfile**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502306250-3f720202-45f8-47dc-9706-bbdf4008409c.png#averageHue=%23dac076&clientId=uafcddd5b-62cc-4&from=paste&height=85&id=ufc7213fd&originHeight=128&originWidth=556&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=41496&status=done&style=none&taskId=u9b95f639-03cd-4d11-a448-c97dbfa54ab&title=&width=370.6666666666667)
**7.log文件名字,logfile**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502341695-71a6cd94-7566-4e87-8bf9-d4babc19ddbe.png#averageHue=%23dec87d&clientId=uafcddd5b-62cc-4&from=paste&height=125&id=uee0f9420&originHeight=188&originWidth=537&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=56954&status=done&style=none&taskId=u9fd224f7-959a-4115-b744-449f7953191&title=&width=358)
**8.requirepass**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502409073-935a8eeb-6147-4576-8b63-b832ac6d0093.png#averageHue=%23e4d268&clientId=uafcddd5b-62cc-4&from=paste&height=78&id=u84efa9a9&originHeight=117&originWidth=510&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=34802&status=done&style=none&taskId=ue0ebefce-fb23-4fa5-8502-d97fd61b68e&title=&width=340)
**9.dump.rdb名字**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502420589-b2ee3b37-6a81-44dc-8283-35753440178b.png#averageHue=%23fcede5&clientId=uafcddd5b-62cc-4&from=paste&height=80&id=uf049bca7&originHeight=120&originWidth=630&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=33164&status=done&style=none&taskId=uf760c257-69f7-4902-82e5-5eb7303755c&title=&width=420)
**10.aof文件,appendfilename**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502435264-e22d32b3-e786-4523-afff-ecdea1737e54.png#averageHue=%23d4c85a&clientId=uafcddd5b-62cc-4&from=paste&height=370&id=u65fabef3&originHeight=555&originWidth=1185&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=164045&status=done&style=none&taskId=u1ac11f2b-3e91-48b0-bb60-f788b99ef1b&title=&width=790)
**11.从机访问主机的通行密码masterauth，必须（从机需要配置，主机不用）**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692502470833-05fcffe5-9960-486b-82e9-6ad110da0d23.png#averageHue=%23fdf2ee&clientId=uafcddd5b-62cc-4&from=paste&height=215&id=ub49c6f5a&originHeight=323&originWidth=904&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=136561&status=done&style=none&taskId=u288f1ffc-78ab-4dbb-a361-b4a54052803&title=&width=602.6666666666666)
## 一主二从
### 方案1: 配置文件固定写死
**1.配置文件执行：replicaof 主库IP 主库端口**
**配置从机6380**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692513650555-30581ded-4d4f-427c-a528-64a7af32a4ee.png#averageHue=%23fdf0ed&clientId=u841d4ba2-19db-4&from=paste&height=194&id=u36bf977c&originHeight=291&originWidth=762&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=120626&status=done&style=none&taskId=ud0ed30a9-e5b1-43fa-bb39-beabd353604&title=&width=508)
**配置从机6381**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692513663215-40796200-6bf3-4080-a19b-6fa89537c1bf.png#averageHue=%23fdfcf9&clientId=u841d4ba2-19db-4&from=paste&height=185&id=ua8ab8975&originHeight=277&originWidth=706&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=115099&status=done&style=none&taskId=uf3353ba3-49d6-447f-adbc-b33dea8c722&title=&width=470.6666666666667)
**先启动master再启动slave**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692513714796-a38d9fec-ce15-4134-8ec4-ee88da3a3bc9.png#averageHue=%23fdf5f5&clientId=u841d4ba2-19db-4&from=paste&height=471&id=u3de6d4dc&originHeight=706&originWidth=1036&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=258261&status=done&style=none&taskId=u2ece1f93-5efb-4130-a9cd-31c844a8b80&title=&width=690.6666666666666)
**主从关系查看**
主机日志
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692515489849-8bb95381-186b-4a58-8f0a-48e937608afa.png#averageHue=%23e1d88b&clientId=u841d4ba2-19db-4&from=paste&height=529&id=u93fb576a&originHeight=793&originWidth=1534&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=628916&status=done&style=none&taskId=u585c1e0f-1e6e-42fd-8f92-cc85c37b2f2&title=&width=1022.6666666666666)
从机日志
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692515502053-584465e6-5a8c-4bdf-95bc-3b4ca510b3a2.png#averageHue=%23e2e2e1&clientId=u841d4ba2-19db-4&from=paste&height=554&id=uef28297f&originHeight=831&originWidth=1450&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=643787&status=done&style=none&taskId=ud5b830af-e755-476b-916e-b6a7c65f760&title=&width=966.6666666666666)
info replication命令查看
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692515597931-c2db2e52-11c3-4109-89f4-fe02c7591e26.png#averageHue=%23e2e2e1&clientId=u841d4ba2-19db-4&from=paste&height=501&id=uce03490e&originHeight=751&originWidth=1274&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=278528&status=done&style=none&taskId=ub066747d-f3ae-4ac7-a7a4-04845734c33&title=&width=849.3333333333334)
### 问题演示
1**.从机可以执行写命令吗?**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692515968725-50be92a2-a83e-4e8c-9c27-2c86ead07d8b.png#averageHue=%23e6e6e5&clientId=u841d4ba2-19db-4&from=paste&height=89&id=u9703a979&originHeight=133&originWidth=857&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=36220&status=done&style=none&taskId=u4305870d-48f8-48cf-93bc-5c3cc2e6a12&title=&width=571.3333333333334)
**2.从机切入点问题**
slave是从头开始复制还是从切入点开始复制?
master启动，写到k3
slave1跟着master同时启动，跟着写到k3
slave2写到k3后才启动，那之前的是否也可以复制？
Y，首次一锅端，后续跟随，master写，slave跟
**3.主机shutdown后，从机会上位吗?**
从机不动，原地待命，从机数据可以正常使用；等待主机重启动归来
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692516263829-cf057319-4d16-4d16-ae21-5f5428848446.png#averageHue=%23fef7f7&clientId=u841d4ba2-19db-4&from=paste&height=335&id=u4277d87e&originHeight=502&originWidth=959&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=220873&status=done&style=none&taskId=ud98803fa-ce94-43b8-a0d8-a18d119ea6a&title=&width=639.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692516280748-ee42a07c-25db-47e4-917f-356dfc9e2fc6.png#averageHue=%23fefafa&clientId=u841d4ba2-19db-4&from=paste&height=433&id=u5f420a54&originHeight=650&originWidth=1581&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=502686&status=done&style=none&taskId=u217069b5-85ea-4fa4-aee6-ec1cba89577&title=&width=1054)
**4.主机shutdown后，重启后主从关系还在吗? 从机还能否顺利复制?**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692516440085-beb5455f-141a-4f7a-beb0-cc9cf39486b9.png#averageHue=%23fdf7f7&clientId=u841d4ba2-19db-4&from=paste&height=624&id=u43801ab1&originHeight=936&originWidth=971&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=366347&status=done&style=none&taskId=u8aa5d6fa-13d7-40d9-95b3-829f1c86679&title=&width=647.3333333333334)
**5.某台从机down后，master继续，从机重启后它能跟上大部队吗?**
类似第二点
### 方案2：命令手动操作
从机停机去掉配置文件中的配置项
3台目前都是主机状态，各不从属
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692517547692-a468fdae-3dd4-42b2-9908-d5c9d6820002.png#averageHue=%23e6e6e5&clientId=u841d4ba2-19db-4&from=paste&height=529&id=u6bc50b1c&originHeight=793&originWidth=869&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=309716&status=done&style=none&taskId=uec403dfe-7197-4415-b73a-4517b7cfc97&title=&width=579.3333333333334)
在从机上执行：slaveof 主库IP 主库端口
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692517582015-313b0117-a11a-4d94-92d2-65afeb05a184.png#averageHue=%23e4e4e3&clientId=u841d4ba2-19db-4&from=paste&height=416&id=u543798fd&originHeight=624&originWidth=779&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=143752&status=done&style=none&taskId=u73c9a8f7-4632-486f-845f-45f094433b6&title=&width=519.3333333333334)
用命令使用的话，2台从机重启后，关系还在吗?
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692517632180-be79b28e-e0a1-42a4-bcd1-f157ca1f961b.png#averageHue=%23fdf6f6&clientId=u841d4ba2-19db-4&from=paste&height=421&id=ud8bb00a5&originHeight=632&originWidth=745&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=239899&status=done&style=none&taskId=u6941db4d-c1b6-418b-80e9-6af044e57b8&title=&width=496.6666666666667)
所以配置文件，持久问题。命令当次生效，重启失效。
## 薪火相传
上一个slave可以是下一个slave的master，slave同样可以接收其他
slaves的连接和同步请求，那么该slave作为了链条中下一个的master.
可以有效减轻主master的写压力
slaveof 新主库IP 新主库端口
中途变更转向:会清除之前的数据，重新建立拷贝最新的
## 反客为主
SLAVEOF  no  one
使当前数据库停止与其他数据库的同步，转成主数据库
# 复制原理和工作流程
**复制流程**

1. 主服务器接收到SYNC命令后，会执行BGSAVE命令，生成RDB持久化文件（快照）。
2. 主服务器使用缓冲区保存执行期间的写命令。
3. 主服务器将保存的RDB文件传输给从服务器。
4. 从服务器接收到主服务器发送的RDB文件后，会将当前数据集清空，并加载RDB文件中的数据。
5. 主服务器将在缓冲区中保存的写命令发送给从服务器。
6. 从服务器执行接收到的写命令，使自己的数据集与主服务器保持同步。
7. 当主服务器执行完所有缓冲区的写命令后，将再次将命令发送给从服务器。
8. 此后，主服务器将每个收到的写命令实时地转发给从服务器，以确保从服务器始终与主服务器保持同步。

值得注意的是，从服务器在初始化同步过程时可以选择全量复制（通过RDB文件）或增量复制（通过传输写命令）。这取决于从服务器在SYNC命令之前是否已经与主服务器建立了连接。如果从服务器与主服务器的连接已断开，它将执行全量复制；否则，它将执行增量复制
**心跳持续，保持通信**
repl-ping-replica-period 10
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692541082770-0d8706ba-b30a-49a2-bdec-22c9d6e1e75b.png#averageHue=%2394a693&clientId=u841d4ba2-19db-4&from=paste&height=177&id=u239fdc57&originHeight=276&originWidth=1001&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=81194&status=done&style=none&taskId=uab704509-fabc-4855-ab88-8aa82a73861&title=&width=643.3333740234375)
**从机下线，重新续传**
master会检查backlog里面的offset，master和slave都会保存一个复制的offset还有一个masterId
offset是保存在backlog中的。Master只会把已经复制的offset后面的数据复制给Slave，类似断点续传

# 复制的缺点
**复制延时，信号衰减**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692541196728-99bec489-b488-4748-9347-cb4a484db533.png#averageHue=%23b4bfbe&clientId=u841d4ba2-19db-4&from=paste&height=446&id=u4bee3901&originHeight=669&originWidth=1474&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=163122&status=done&style=none&taskId=udb8b4b20-d334-43b8-8ad3-81a3eb37d1e&title=&width=982.6666666666666)
**master挂了如何办?**
默认情况下，不会在slave节点中自动重选一个master，需要用到哨兵
