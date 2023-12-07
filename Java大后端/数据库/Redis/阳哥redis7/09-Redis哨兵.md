官网地址：[https://redis.io/docs/manual/sentinel](https://redis.io/docs/manual/sentinel)
# 是什么
吹哨人巡查监控后台master主机是否故障，如果故障了根据投票数自动
将某一个从库转换为新主库，继续对外服务
**哨兵的作用：**
1、监控redis运行状态，包括master和slave
2、当master down机，能自动将slave切换成新master
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692714649406-649b1893-949c-4a1e-9d1a-3c104540abae.png#averageHue=%23ca9f58&clientId=ub862862e-805d-4&from=paste&height=355&id=u8019524e&originHeight=533&originWidth=661&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=253413&status=done&style=none&taskId=u5d1255ba-6b04-41a5-9f52-dcdb9192087&title=&width=440.6666666666667)
# 能干嘛
主从监控：监控主从redis库运行是否正常
消息通知：哨兵可以将故障转移的结果发送给客户端
故障转移：如果Master异常，则会进行主从切换，将其中一个Slave作为新Master
配置中心：客户端通过连接哨兵来获得当前Redis服务的主节点地址

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692714690545-da05d1ad-1bc6-4ea9-8528-b9ab9d2f0065.png#averageHue=%23fefafa&clientId=ub862862e-805d-4&from=paste&height=401&id=u84099e4a&originHeight=602&originWidth=1150&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=324939&status=done&style=none&taskId=u819b20b5-38f7-40c3-91d5-6875564bbb3&title=&width=766.6666666666666)
# 怎么玩
## Redis Sentinel架构
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692715313169-56964624-f203-4857-9339-12496a566bc0.png#averageHue=%23faf9f6&clientId=ub862862e-805d-4&from=paste&height=393&id=u476e59e9&originHeight=590&originWidth=585&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=95966&status=done&style=none&taskId=u07768fe3-f730-4d85-9c8f-60b79433ba6&title=&width=390)
**3个哨兵：**自动监控和维护集群，不存放数据，只是吹哨人
**1主2从：**用于数据读取和存放
## 不服就干
在安装目录下先拷贝sentinel.conf文件做备份
### 重点参数

1. **bind**

服务监听地址，用于客户端连接，默认本机地址

2. **daemonize**

是否以后台daemon方式运行

3. **protected-mode**

安全保护模式

4. **port**

端口

5. **logfile**

日志文件路径

6. **pidfile**

pid文件路径

7. **dir**

工作目录

8. **sentinel monitor <master-name> <ip> <redis-port> <quorum>**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692716237265-36d184c7-ab15-444b-b1f8-d297322b997f.png#averageHue=%23f0e364&clientId=ub862862e-805d-4&from=paste&height=323&id=u4f94a3c6&originHeight=485&originWidth=1182&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=265919&status=done&style=none&taskId=u3a7b9a0d-aca0-418c-b70e-adcb2b5e583&title=&width=788)
设置要监控的master服务器
quorum表示最少有几个哨兵认可客观下线
同意故障迁移的法定票数。
我们知道，网络是不可靠的，有时候一个sentinel会因为网络堵塞而误以为一个master redis已经死掉了，在sentinel集群环境下需要多个sentinel互相沟通来确认某个master是否真的死了，quorum这个参数是进行客观下线的一个依据，意思是至少有quorum个sentinel认为这个master有故障，才会对这个master进行下线以及故障转移。因为有的时候，某个sentinel节点可能因为自身网络原因，导致无法连接master，而此时master并没有出现故障，所以，这就需要多个sentinel都一致认为该master有问题，才可以进行下一步操作，这就保证了公平性和高可用。

9. **sentinel auth-pass <master-name> <password >**

master设置了密码，连接master服务的密码
### 其他参数

1. **sentinel down-after-milliseconds <master-name> <milliseconds>：**

指定多少毫秒之后，主节点没有应答哨兵，此时哨兵主观上认为主节点下线

2. **sentinel parallel-syncs <master-name> <nums>：**

表示允许并行同步的slave个数，当Master挂了后，哨兵会选出新的Master，此时，剩余的slave会向新的master发起同步数据

3. **sentinel failover-timeout <master-name> <milliseconds>：**

故障转移的超时时间，进行故障转移时，如果超过设置的毫秒，表示故障转移失败

4. **sentinel notification-script <master-name> <script-path> ：**

配置当某一事件发生时所需要执行的脚本

5. **sentinel client-reconfig-script <master-name> <script-path>：**

客户端重新配置主节点参数脚本
### 本次案例哨兵sentinel文件通用配置
由于机器硬件关系，我们的3个哨兵都同时配置进192.168.111.169同一台机器
sentinel26379.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 26379
logfile "/myredis/sentinel26379.log"
pidfile /var/run/redis-sentinel26379.pid
dir /myredis
sentinel monitor mymaster 192.168.111.169 6379 2
sentinel auth-pass mymaster 111111
```
sentinel26380.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 26380
logfile "/myredis/sentinel26380.log"
pidfile /var/run/redis-sentinel26380.pid
dir "/myredis"
sentinel monitor mymaster 192.168.111.169 6379 2
sentinel auth-pass mymaster 111111
```
sentinel26381.conf
```java
bind 0.0.0.0
daemonize yes
protected-mode no
port 26381
logfile "/myredis/sentinel26381.log"
pidfile /var/run/redis-sentinel26381.pid
dir "/myredis"
sentinel monitor mymaster 192.168.111.169 6379 2
sentinel auth-pass mymaster 111111
```
master主机配置文件说明
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692716774631-7d085ed0-5f75-4aa4-a57b-8630d19cc547.png#averageHue=%23fdf3f3&clientId=ub862862e-805d-4&from=paste&height=221&id=uebdc628f&originHeight=331&originWidth=912&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=126071&status=done&style=none&taskId=u360935b5-a284-4322-88ef-e54d70c2d33&title=&width=608)
### 先启动一主二从3个redis实例，测试正常的主从复制
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692716893934-4297f273-4f50-4857-b8fe-5afc6ff90e37.png#averageHue=%2381a9bc&clientId=ub862862e-805d-4&from=paste&height=430&id=u4a665a8e&originHeight=645&originWidth=1670&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=161499&status=done&style=none&taskId=uef4c2107-2dfd-4de2-a3c8-526038a5d77&title=&width=1113.3333333333333)
** 主机6379**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692716971277-5a6f1207-de3d-469f-898e-1f05341e19c3.png#averageHue=%23fdfcf8&clientId=ub862862e-805d-4&from=paste&height=179&id=uf4b3e4c3&originHeight=268&originWidth=1116&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=96116&status=done&style=none&taskId=u9dd4471d-dddd-4cd6-a51a-f860a2ba61d&title=&width=744)
6379后续可能会变成从机，需要设置访问新主机的密码， 请设置masterauth项访问密码为111111，
不然后续可能报错master_link_status:down
**6380**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692717008684-df1a7cea-e637-4e00-95de-48c93817bf9f.png#averageHue=%23fdfbf5&clientId=ub862862e-805d-4&from=paste&height=214&id=u767c17ee&originHeight=321&originWidth=1116&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=149690&status=done&style=none&taskId=ucce638cb-42a5-47eb-b4ce-18ef0c6cd75&title=&width=744)
具体IP地址和密码根据你本地真实情况，酌情修改
**6381**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692717036897-5db756ac-27d9-4e27-956f-5937ddbc826a.png#averageHue=%23fdfcf8&clientId=ub862862e-805d-4&from=paste&height=201&id=ud7448f8d&originHeight=302&originWidth=1149&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=151516&status=done&style=none&taskId=u065e1dc1-1574-4ddd-a3b4-aa4220f1cfb&title=&width=766)
具体IP地址和密码根据你本地真实情况，酌情修改
### 再启动3个哨兵，完成监控
redis-sentinel sentinel26379.conf --sentinel
redis-sentinel sentinel26380.conf --sentinel
redis-sentinel sentinel26381.conf --sentinel
启动3个哨兵监控后再测试一次主从复制
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692718576530-db33cd55-a6d3-4184-b5fd-1ec217b5f55d.png#averageHue=%23e5e5e4&clientId=ub862862e-805d-4&from=paste&height=591&id=ua8f17a8d&originHeight=887&originWidth=1304&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=463602&status=done&style=none&taskId=u4ad7fa37-4fed-4b62-a89c-6c08a9d5cb9&title=&width=869.3333333333334)
### 原有的master挂了
我们自己手动关闭6379服务器，模拟master挂了
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692800897833-db3ba356-99fa-4dd2-8a0f-3d10e240ec66.png#averageHue=%23fefcfc&clientId=ubc283dac-0cc8-4&from=paste&height=385&id=u809af2cf&originHeight=577&originWidth=1017&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=230174&status=done&style=none&taskId=u1c5a910e-bda3-4023-bf9e-bd6d3ddc527&title=&width=678)
由于master断了，需要重新选举。可能会造成一点小错误，需要重新连接
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692802318219-f2678511-3a0a-443d-9efb-5a94cef713de.png#averageHue=%23fef8f8&clientId=ubc283dac-0cc8-4&from=paste&height=460&id=ufbf9fda1&originHeight=690&originWidth=806&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=219270&status=done&style=none&taskId=u8ed756ec-894c-4d67-8b16-637394bba06&title=&width=537.3333333333334)
了解 Broken Pipe
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692802358305-227f0b5b-2c65-48f2-8107-a95da59b9017.png#averageHue=%23eef0f7&clientId=ubc283dac-0cc8-4&from=paste&height=403&id=u67dabda2&originHeight=605&originWidth=1587&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=480740&status=done&style=none&taskId=ua1344fdf-0a6a-4660-8143-2f54e7a856c&title=&width=1058)

### 重新选举后的master
6381被选为新master，上位成功
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692802411132-1c6d9f7d-a8db-4420-a8e9-8225635447ec.png#averageHue=%23fef9f9&clientId=ubc283dac-0cc8-4&from=paste&height=440&id=uc91c7114&originHeight=660&originWidth=1019&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=299010&status=done&style=none&taskId=udd3b39d4-3946-4f61-9e7e-b1f1dd3bf27&title=&width=679.3333333333334)
以前的6379从master降级变成了slave
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692802428578-ff67c868-ce3b-4025-acfc-233f0ef177d8.png#averageHue=%23fef5f5&clientId=ubc283dac-0cc8-4&from=paste&height=463&id=u80f072fa&originHeight=695&originWidth=1137&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=236999&status=done&style=none&taskId=u838f7398-bf96-485b-a0d2-5d49107bfa8&title=&width=758)
6380还是slave，只不过换了个新老大6381(6379变6381)，6380还是slave
### 配置文件自动被修改
在运行期间会被sentinel动态进行更改文件的内容，
Master-Slave切换后，master redis.conf、slave redis.conf和sentinel.con的内容都会发生改变
即master _redis.conf中会多一行slaveof的配置，sentinel.con的监控目标会随之调换
# 哨兵运行流程和选举原理
当一个主从配置中的master失效之后，sentinel可以选举出一个新的master
用于自动接替原master的工作，主从配置中的其他redis服务器自动指向新的master同步数据
般建议sentinel采取奇数台，防止某一台sentinel无法连接到master导致误切换
## 正常情况
三个哨兵监控一主二从，正常运行
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696594231962-53fa9a32-17ce-41fd-a135-431113c4c0b9.png#averageHue=%23faf9f7&clientId=u9a847967-72b1-4&from=paste&height=427&id=uf1918bc1&originHeight=641&originWidth=606&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=99519&status=done&style=none&taskId=u5d05a925-5f98-4551-b8dc-8ed01aa5f0f&title=&width=404)
## SDown主观下线(Subjectively Down)
SDOWN(主观不可用)是单个sentinel自己主观上检测到的关于master的状态，从sentinel的角度来看
如果发送了PING心跳后，在一定时间内没有收到合法的回复，就达到了SDOWN的条件。
sentinel配置文件中的down-after-milliseconds设置了判断主观下线的时间长度
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696594412721-6c1b13e0-b883-4d27-a1af-1d2ed6ff230b.png#averageHue=%23fcf2e7&clientId=u9a847967-72b1-4&from=paste&height=179&id=ufedf7f4d&originHeight=269&originWidth=1145&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=145098&status=done&style=none&taskId=u4cd6572a-90c8-4524-8e3a-dc05f44cce6&title=&width=763.3333333333334)
## ODown客观下线(Objectively Down)
ODOWN需要一定数量的sentinel，多个哨兵达成一致意见才能认为一个master客观上已经宕掉
四个参数含义：
masterName是对某个master+slave组合的一个区分标识(一套sentinel可以监听多组master+slave这样的组合)
quorum这个参数是进行客观下线的一个依据，法定人数/法定票数
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696594702322-0b2330f2-fac6-4424-a80c-86feae7610c1.png#averageHue=%23e0c29e&clientId=u9a847967-72b1-4&from=paste&height=358&id=u78628d28&originHeight=537&originWidth=1024&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=282977&status=done&style=none&taskId=u2cfd3d36-8b67-4108-adfd-c598e01a556&title=&width=682.6666666666666)

意思是至少有quorum个sentinel认为这个master有故障才会对这个master进行下线以及故障转移。因为有的时候，某个sentinel节点可能因为自身网络原因导致无法连接master，而此时master并没有出现故障，所以这就需要多个sentinel都一致认为该master有问题，才可以进行下一步操作，这就保证了公平性和高可用。
## 选举出领导者哨兵(哨兵中选出兵王)
当主节点被判断客观下线以后，各个哨兵节点会进行协商，先选举出一个领导者哨兵节点 (兵王) 并由该领导者节点也即被选举出的兵王进行failover (故障迁移)
### 如何选出来
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696597427656-00d2bfd7-9526-46d2-9ddd-3b25c8e80b00.png#averageHue=%23f8f8f8&clientId=u9a847967-72b1-4&from=paste&height=173&id=u30bcab2c&originHeight=260&originWidth=798&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=58981&status=done&style=none&taskId=u6995b5b9-ab86-4f6f-866d-36af7b718ab&title=&width=532)
监视该主节点的所有哨兵都有可能被选为领导者，选举使用的算法是Raft算法；Raft算法的基本思路是先到先得：
即在一轮选举中，哨兵A向B发送成为领导者的申请，如果B没有同意过其他哨兵，则会同意A成为领导者
### 由兵王开始推动故障切换流程并选出一个新master
#### 第一步
选出新master的规则，剩余slave节点健康前提下
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696599356927-66d80332-e7cb-4a8e-8b8b-c746de7fe895.png#averageHue=%23cac5be&clientId=u9a847967-72b1-4&from=paste&height=479&id=ud97b1d52&originHeight=719&originWidth=402&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=150754&status=done&style=none&taskId=u278cc246-d55b-4618-8df0-9137040325b&title=&width=268)

1. redis.conf文件中，优先级slave-priority或者replica-priority最高的从节点(数字越小优先级越高

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1696599411730-a641e66a-5890-411d-8e60-16558b7c778e.png#averageHue=%23fdfaf2&clientId=u9a847967-72b1-4&from=paste&height=285&id=ub8ae61a5&originHeight=428&originWidth=1066&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=208204&status=done&style=none&taskId=udf1c9cac-5932-444b-87ea-24123639148&title=&width=710.6666666666666)

2. 复制偏移位置offset最大的从节点
3. 最小Run ID的从节点：字典顺序，ASCII码
#### 第二步
Sentinel leader会对选举出的新master执行slaveof no one操作，将其提升为master节点
Sentinelleader向其它slave发送命令，让剩余的slave成为新的master节点的slave
Sentinel leader向其它slave发送命令，让剩余的slave成为新的master节点的slave
#### 第三步
下线的老master设置为新选出的新master的从节点，当老master重新一线后它会成为新master的从节点
Sentinel leader会让原来的master降级为slave并恢复正常工作。
# 哨兵使用建议
哨兵节点的数量应为多个，哨兵本身应该集群，保证高可用
哨兵节点的数量应该是奇数
各个哨兵节点的配置应一致
如果哨兵节点部署在Docker等容器里面，尤其要注意端口的正确映射
哨兵集群+主从复制，并不能保证数据零丢失
