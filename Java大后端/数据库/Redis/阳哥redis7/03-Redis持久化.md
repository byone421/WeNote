官网地址：[https://redis.io/docs/manual/persistence/](https://redis.io/docs/manual/persistence/)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692100862267-1864ede3-50d1-4988-9a29-f6a4c08519df.png#averageHue=%23fcfdfa&clientId=u8036d556-fe59-4&from=paste&height=248&id=u31946b38&originHeight=372&originWidth=1522&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=339897&status=done&style=none&taskId=u91e5a843-5b66-4f0c-91d3-affe1a9c282&title=&width=1014.6666666666666)
# RDB (Redis DataBase)
RDB（Redis 数据库）：RDB 持久性以指定的时间间隔执行数据集的时间点快照。
实现类似照片记录效果的方式，就是把某一时刻的数据和状态以文件的形式写到磁盘上，也就是
快照。这样一来即使故障宕机，快照文件也不会丢失，数据的可靠性也就得到了保证。
这个快照文件就称为RDB文件(dump.rdb)，其中，RDB就是Redis DataBase的缩写。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692100975184-2c245820-ed71-4bee-ade7-b6648dfa0c14.png#averageHue=%23739867&clientId=u8036d556-fe59-4&from=paste&height=363&id=u82d106a6&originHeight=545&originWidth=735&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=79873&status=done&style=none&taskId=uaf794f87-ee5f-4822-ab7c-c5c4bd40bfb&title=&width=490)
## 配置文件
### Redis6.0.16以下
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692101114093-5b883e64-a967-455c-a026-0d64ef0fa001.png#averageHue=%2333332b&clientId=u8036d556-fe59-4&from=paste&height=761&id=u213563e4&originHeight=1142&originWidth=1269&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=579153&status=done&style=none&taskId=u88ee1725-b749-4eb6-a9b2-bbb14980edd&title=&width=846)
### Redis6.2以及Redis7
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692101199261-73d4ba68-ba20-4c3a-a60b-eaf46590600d.png#averageHue=%232f2f27&clientId=u8036d556-fe59-4&from=paste&height=605&id=ub2cd35d3&originHeight=908&originWidth=1225&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=416606&status=done&style=none&taskId=u4f30c745-b4e5-40f8-ba57-da23f44292b&title=&width=816.6666666666666)
## 自动触发
**本次案例5秒2次修改**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692106805696-ff36a7b4-cf80-4976-87e7-2348d7f9bed3.png#averageHue=%23fdf5f3&clientId=ue982c285-a78e-4&from=paste&height=466&id=uabf38e01&originHeight=699&originWidth=762&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=170232&status=done&style=none&taskId=uea2f1d40-161b-4759-8f65-d2d001035cd&title=&width=508)
**修改dump文件保存路径**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692106859373-d6ae5391-e700-4864-9afe-996901ca57a7.png#averageHue=%23dfe0d8&clientId=ue982c285-a78e-4&from=paste&height=765&id=uca1c723f&originHeight=1148&originWidth=1050&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=335003&status=done&style=none&taskId=ud35d8891-1d41-4212-b7ab-fff0c5b55bc&title=&width=700)
**修改dump文件名称**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692106883070-b33604c3-bbd0-4682-a7b5-dc7afab001ca.png#averageHue=%23dfbf99&clientId=ue982c285-a78e-4&from=paste&height=96&id=u34676768&originHeight=144&originWidth=991&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=31036&status=done&style=none&taskId=u5b7b1da2-f454-4ed1-b4c7-41e15d1b28b&title=&width=660.6666666666666)
**触发备份**
第一种情况
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692107180222-16fbeb26-af72-4040-8992-a641b63a120d.png#averageHue=%23faf9f9&clientId=ue982c285-a78e-4&from=paste&height=267&id=u4d130f90&originHeight=401&originWidth=1632&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=143949&status=done&style=none&taskId=u92a75332-1205-4039-8837-e408d742254&title=&width=1088)
第二种情况
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692107189815-621c0245-53ea-453c-8428-a8759746aaf1.png#averageHue=%23faf6f5&clientId=ue982c285-a78e-4&from=paste&height=327&id=uba216259&originHeight=490&originWidth=1637&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=257694&status=done&style=none&taskId=uc8f1397b-1c76-40a5-8e76-bca62399f15&title=&width=1091.3333333333333)
**如何恢复**
将备份文件(dump.rdb) 移动到 redis 配置的rdb目录即可
备份成功后故意用flushdb清空redis，看看是否可以恢复数据
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692107339814-867b3ee5-2197-47e9-b2ee-9df0d26bceb2.png#averageHue=%23fbf9f9&clientId=ue982c285-a78e-4&from=paste&height=341&id=u3f2902e6&originHeight=512&originWidth=1657&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=328373&status=done&style=none&taskId=u794c290f-2485-4709-bb85-e5cb86c4bfd&title=&width=1104.6666666666667)
结论：执行flushall/flushdb命令也会产生dump.rdb文件，但里面是空的，无意义
## 手动触发
Redis提供了两个命令来生成RDB文件，分别是**save**和**bgsave**
### SAVE
在主程序中执行会阻塞当前redis服务器，直到持久化工作完成
执行save命令期间，Redis不能处理其他命令，线上禁止使用
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692107775918-bc860350-38d2-4bf1-a1a7-36fb967bbca1.png#averageHue=%23e7e7e1&clientId=ue982c285-a78e-4&from=paste&height=443&id=u46be56c6&originHeight=664&originWidth=1638&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=217927&status=done&style=none&taskId=u48f55d53-c64b-4542-8780-839dd4d0f26&title=&width=1092)
### BGSAVE(默认)
Redis会在后台异步进行快照操作，不阻塞
快照同时还可以响应客户端请求,该触发方式
会fork一个子进程由子进程复制持久化过程
Redis会使用bgsave对当前内存中的所有数据做快照
这个操作是子进程在后台完成的，这就允许主进程同时可以修改数据![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692108333204-fed12641-6bea-4cf4-8766-3aeed7403c6f.png#averageHue=%23f2f6ed&clientId=ue982c285-a78e-4&from=paste&height=497&id=u4a663779&originHeight=746&originWidth=1667&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=258064&status=done&style=none&taskId=u017bc2eb-f0d9-4050-b7b4-7376eaddd9a&title=&width=1111.3333333333333)
**LASTSAVE**
可以通过lastsave命令获取最后一次成功执行快照的时间
## 优势
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692430896901-7d14a314-064c-49a9-8c59-0afbc8c2dba2.png#averageHue=%23ececef&clientId=uc749d4e4-db97-4&from=paste&height=233&id=uffd9a04f&originHeight=350&originWidth=999&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=204455&status=done&style=none&taskId=uf4b35b57-fa67-45f3-a500-275449f6e60&title=&width=666)
**总结就是：**
适合大规模的数据恢复
按照业务定时备份
对数据完整性和一致性要求不高
RDB 文件在内存中的加载速度要比 AOF 快得多
## 劣势
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692431019833-27212779-9c18-4267-ab92-899720e1eadb.png#averageHue=%23eaeaec&clientId=uc749d4e4-db97-4&from=paste&height=276&id=ua5f79591&originHeight=414&originWidth=955&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=234434&status=done&style=none&taskId=uce2b1e34-a119-4868-92d9-0c4564ec979&title=&width=636.6666666666666)
**总结就是：**
在一定间隔时间做一次备份，所以如果redis意外down掉的话，就
会丢失从当前至最近一次快照期间的数据，快照之间的数据会丢失
内存数据的全量同步，如果数据量太大会导致I/0严重影响服务器性能
RDB依赖于主进程的fork，在更大的数据集中，这可能会导致服务请求的瞬间延迟
fork的时候内存中的数据被克隆了-份，大致2倍的膨胀性，需要考虑
## 如何修复dump.rdb文件
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692431205266-b390761a-859a-4e4d-9e43-0a7ce50fef7b.png#averageHue=%23fefafa&clientId=uc749d4e4-db97-4&from=paste&height=433&id=u0e092f68&originHeight=649&originWidth=1163&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=316813&status=done&style=none&taskId=ueb4c645a-e031-43b9-aad7-87ace4c5f52&title=&width=775.3333333333334)
## 哪些情况会触发RDB
配置文件中默认的快照配置
于动save/bgsave命令
执行flushall/flushdb命令也会产生dump.rdb文件，但里面是空的，无意义
执行shutdown且没有设置开启AOF持久化
主从复制时，主节点自动触发
## 如何禁用快照
动态所有停止RDB保存规则的方法: redis-cli config set save ""
快照禁用
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692431395031-9009a887-3e3c-45d2-b3b7-67330c71addd.png#averageHue=%232c2c26&clientId=uc749d4e4-db97-4&from=paste&height=427&id=ua50f6483&originHeight=640&originWidth=1015&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=208650&status=done&style=none&taskId=u70a1b359-7256-4d4b-9cf8-2c821feac43&title=&width=676.6666666666666)
## 优化配置项
**<seconds> <changes>**
**save**
**dbfilename**
**dir**
**stop-writes-on-bgsave-error**
默认yes
如果配置成no，表示你不在乎数据不一致或者有其他的手段发现和控制这种不一致，那么在快照写入失败时，
也能确保redis继续接受新的写请求
**rdbcompression**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692431749384-8451987b-0089-49f7-8aed-7de786594ae2.png#averageHue=%239eabad&clientId=uc749d4e4-db97-4&from=paste&height=286&id=u8a4ccc8c&originHeight=429&originWidth=1170&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=148967&status=done&style=none&taskId=u0290a784-1f96-4fb5-a821-fdd796a2a60&title=&width=780)
**rdbchecksum**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692431769485-bbbebf9f-9689-4817-b4ec-95b64cb1d296.png#averageHue=%239db4bd&clientId=uc749d4e4-db97-4&from=paste&height=300&id=u8f85019b&originHeight=450&originWidth=1638&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=186710&status=done&style=none&taskId=u34dde6c4-3ade-41af-8b83-d37a331bae6&title=&width=1092)
**rdb-del-sync-files**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692431781392-0e1cabdb-0967-47e1-be36-4ca91d2fa5d5.png#averageHue=%23eeeed5&clientId=uc749d4e4-db97-4&from=paste&height=377&id=u227a9e5d&originHeight=566&originWidth=1246&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=297792&status=done&style=none&taskId=udb5deca3-af5f-467a-ad71-0397b3e6a63&title=&width=830.6666666666666)
## 总结
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692431843005-5b454cf2-1c05-4be0-9c7a-8be0e586ed78.png#averageHue=%23faf9f8&clientId=uc749d4e4-db97-4&from=paste&height=419&id=u6b642a62&originHeight=629&originWidth=1172&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=230119&status=done&style=none&taskId=u0ae5e6a1-d50a-477e-803e-4af339dbae0&title=&width=781.3333333333334)
# AOP (Append Only File)
## 是什么
以日志的形式来记录每个写操作，将Redis执行过的所有写指令记录下来(读操作不记录),
只许追加文件伯不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis
重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作
默认情况下，redis是没有开启AOF(append only file)的
开启AOF功能需要设置配置: appendonly yes
Aof保存的是appendonly.aof文件
## 持久化工作流程
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692432119127-46e8680a-c9fc-4bb8-8e44-c35484c5557d.png#averageHue=%23f5f4f4&clientId=uc749d4e4-db97-4&from=paste&height=248&id=u68816ea6&originHeight=372&originWidth=1313&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=75201&status=done&style=none&taskId=u2156a688-492b-4366-a485-cdcec3124c7&title=&width=875.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692432222526-995a6a71-8218-45fe-992a-db7f2928cc64.png#averageHue=%23e9edf5&clientId=uc749d4e4-db97-4&from=paste&height=201&id=udf553864&originHeight=301&originWidth=1505&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=137532&status=done&style=none&taskId=ub1055c34-9964-4d9a-96ac-c7ded7da463&title=&width=1003.3333333333334)
## AOF缓冲区三种回写策略
**Always：**
同步写回，每个写命令执行完立刻同步地将日志写回磁盘
**everysec：**
每秒写回，每个写命令执行完，只是先把日志写到AOF文件的内存缓冲区，每隔1秒把缓冲区中的内容写入磁盘
**no：**
操作系统控制的写回，每个写命令执行完，只是先把日志写到AOF文件的内存缓冲区，由操作系统决定何时将缓冲区内容写回磁盘
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692432985205-9dff01d7-81c0-4e26-be6f-3fcaf2657099.png#averageHue=%23ecdb90&clientId=uc749d4e4-db97-4&from=paste&height=239&id=u4229960b&originHeight=359&originWidth=1270&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=270452&status=done&style=none&taskId=u4d4d68bf-84eb-4fde-b54c-60757d89f75&title=&width=846.6666666666666)
## 配置、启动、修复、恢复
### 开启
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692433131038-b33ce8a1-fdc5-4c01-82cf-f87d36110678.png#averageHue=%23fcf1ec&clientId=uc749d4e4-db97-4&from=paste&height=88&id=ub13ad5e0&originHeight=132&originWidth=986&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=60511&status=done&style=none&taskId=u05697dad-aaf3-47fd-a775-ccbb931222f&title=&width=657.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692433139441-2858cdb9-07f0-45ba-b3db-255a7e35cd9e.png#averageHue=%23fdf2f2&clientId=uc749d4e4-db97-4&from=paste&height=113&id=u008f1689&originHeight=169&originWidth=491&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=38835&status=done&style=none&taskId=ueb35d4f1-27ea-4f85-adc6-f58369810c8&title=&width=327.3333333333333)
### aof保存路径
**reids6**
AOF保存文件的位置和RDB保存文件的位置一样
都是通过redis.conf配置文件的 dir 配置
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692433191475-f88708f1-78af-4772-b35d-259e26509a11.png#averageHue=%23eee163&clientId=uc749d4e4-db97-4&from=paste&height=195&id=u62bdd83c&originHeight=293&originWidth=956&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=93746&status=done&style=none&taskId=u9b4a3945-74c4-4ca8-b6fc-77eaf8acdb9&title=&width=637.3333333333334)
**redis7之后**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692433234737-9ae828da-a47a-49bd-9dbd-e49a890350ad.png#averageHue=%23dfbf9a&clientId=uc749d4e4-db97-4&from=paste&height=125&id=ud6cdc201&originHeight=187&originWidth=1132&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=75316&status=done&style=none&taskId=u604eed34-dfc3-4448-8c4a-769cfa63917&title=&width=754.6666666666666)
最终
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692433246939-6cbc6251-2d57-4172-a1a1-6a1b1d8c1f70.png#averageHue=%23e2c97e&clientId=uc749d4e4-db97-4&from=paste&height=401&id=ud5472d89&originHeight=601&originWidth=1050&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=214248&status=done&style=none&taskId=uc9d95d23-2f29-4539-9449-10b19903e6b&title=&width=700)
### aof保存名称
**redis6**
有且只有一个
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692433469573-aee7394c-0784-4ac8-b68e-0fd3bbe55b09.png#averageHue=%232a2b25&clientId=uc749d4e4-db97-4&from=paste&height=187&id=u9084527e&originHeight=280&originWidth=1107&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=111758&status=done&style=none&taskId=u880a971f-fb08-4fa2-8d74-6beea55261a&title=&width=738)
**Redis7.0 Multi Part AOF的设计**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692434197051-e7366210-67a7-491e-988b-159f11b1476d.png#averageHue=%23ded4aa&clientId=uc749d4e4-db97-4&from=paste&height=724&id=ud73bf4ce&originHeight=1086&originWidth=1270&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=526913&status=done&style=none&taskId=u30119a2c-a137-4ecf-813e-5e194515514&title=&width=846.6666666666666)
Redis7.0config 中对应的配置项
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692434210802-32bc7407-b87f-45df-b7e6-b56dcb851723.png#averageHue=%23323542&clientId=uc749d4e4-db97-4&from=paste&height=299&id=uaeb8d997&originHeight=448&originWidth=590&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=93457&status=done&style=none&taskId=u88ded9a4-e7a3-45a3-9316-4b26722c3a6&title=&width=393.3333333333333)
### 正常恢复
启动: 设置Yes
修改默认的appendonly no，改为yes
写操作继续，生成aof文件到指定的目录
重启redis然后重新加载，结果OK
### 异常恢复
故意乱写正常的AOF文件
模拟网络闪断文件写error
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692434869755-4f564887-2d77-4f25-804d-487c4a29306b.png#averageHue=%23fcf8f7&clientId=uc749d4e4-db97-4&from=paste&height=452&id=u27fd2d8c&originHeight=678&originWidth=1002&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=71011&status=done&style=none&taskId=u5de42d52-bfa5-487c-8879-7b98f05b4d2&title=&width=668)
重启 Redis 之后就会进行 AOF 文件的载入，发现启动都不行
异常修复命令: redis-check-aof --fix 进行修复
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692434918234-4e39c54c-d7e9-4ecc-aa12-5b1b947409a7.png#averageHue=%23fef3f3&clientId=uc749d4e4-db97-4&from=paste&height=319&id=ue0f6831a&originHeight=479&originWidth=1483&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=262663&status=done&style=none&taskId=uc7d9adb3-892c-4b52-a4ca-649d22c2e43&title=&width=988.6666666666666)
重新OK
## 优势
更好的保护数据不丢失 、性能高、可做紧急恢复
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435516605-4de8a21c-d487-44d1-867f-07ab2a36be0e.png#averageHue=%23e9e9eb&clientId=uc749d4e4-db97-4&from=paste&height=336&id=u25d6e9d6&originHeight=504&originWidth=989&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=326572&status=done&style=none&taskId=u1f9aed96-2406-4dab-9564-1657b480bbc&title=&width=659.3333333333334)
## 劣势
相同数据集的数据而言aof文件要远大于rdb文件，恢复速度慢于rdb
aof运行效率要慢于rdb,每秒同步策略效率较好，不同步效率和rdb相同
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435547893-b3c0ef8c-530a-4faa-aba8-093577696412.png#averageHue=%23eeeff0&clientId=uc749d4e4-db97-4&from=paste&height=168&id=u16e8cb59&originHeight=252&originWidth=990&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=110449&status=done&style=none&taskId=u3394236d-bf0e-4a54-8f9e-7e4fe7b76a0&title=&width=660)
## AOF重写机制
由于AOF持久化是Redis不断将写命令记录到 AOF 文件中，随着Redis不断的进行，AOF 的文件会越来越大，
文件越大，占用服务器内存越大以及 AOF 恢复要求时间越长。
为了解决这个问题，Redis新增了重写机制，当AOF文件的大小超过所设定的峰值时，Redis就会自动启动AOF文件的内容压缩，
只保留可以恢复数据的最小指令集
或者
可以手动使用命令 bgrewriteaof 来重新
### 触发机制
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435645368-ad7dc1ad-bfee-4196-81c2-51ee94d6e43d.png#averageHue=%23e7dc7a&clientId=uc749d4e4-db97-4&from=paste&height=486&id=ub0eec808&originHeight=729&originWidth=997&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=292336&status=done&style=none&taskId=u6020869c-4968-47d2-9f4e-84d455c0abd&title=&width=664.6666666666666)
**自动触发**
满足配置文件中的选项后，Redis会记录上次重写时的AOF大小
默认配置是当AOF文件大小是上次rewrite后大小的一倍目文件大于64M时
**手动触发**
客户端向服务器发送bgrewriteaof命令
### 案例说明
启动AOF文件的内容压缩，只保留可以恢复数据的最小指令集。

举个例子：比如有个key 
一开始你  set k1 v1
然后改成  set k1 v2
最后改成  set k1 v3
如果不重写，那么这3条语句都在aof文件中，内容占空间不说启动的时候都要执行一遍，共计3条命令；
但是，我们实际效果只需要set k1 v3这一条，所以，
开启重写后，只需要保存set k1 v3就可以了只需要保留最后一次修改值，相当于给aof文件瘦身减肥，性能更好。
AOF重写不仅降低了文件的占用空间，同时更小的AOF也可以更快地被Redis加载。
#### 先修改配置
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435793383-2b34b262-c172-44a1-937e-9afbcd18b2df.png#averageHue=%23f9ebe5&clientId=uc749d4e4-db97-4&from=paste&height=63&id=u76ea1a0f&originHeight=95&originWidth=1299&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=34169&status=done&style=none&taskId=u20e24d4d-e59a-4e3f-bb6e-b4be0be3f2f&title=&width=866)
重写峰值修改为1k
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435811991-62dfd63d-d9a1-43ef-9d54-391c414f60cd.png#averageHue=%23fced73&clientId=uc749d4e4-db97-4&from=paste&height=86&id=u040440be&originHeight=129&originWidth=539&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=19215&status=done&style=none&taskId=u87e9df65-f320-40d1-9cb0-986862ef659&title=&width=359.3333333333333)
关闭混合，设置为no
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435830025-3c278db1-3c1a-44cf-bb93-ec0ac56fdc76.png#averageHue=%23e9df6f&clientId=uc749d4e4-db97-4&from=paste&height=117&id=u922cf3f1&originHeight=176&originWidth=657&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=58495&status=done&style=none&taskId=u927c727d-752e-4ba7-aa5b-7d5094cfc3f&title=&width=438)
删除之前的全部aof和rdb，清除干扰项
#### 自动触发案例
完成上述正确配置，重启redis服务器
执行set k1 v1查看aof文件是否正常
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435873137-43169a97-1920-42e2-a6ee-4ebed5a713fe.png#averageHue=%23e3e3e2&clientId=uc749d4e4-db97-4&from=paste&height=357&id=u826bb2ab&originHeight=535&originWidth=942&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=193284&status=done&style=none&taskId=u20e86030-e9be-4008-8203-2f82194d6dc&title=&width=628)
k1不停1111111暴涨
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435938129-8ec6e813-1a93-4f1e-8a71-fb64f0f192db.png#averageHue=%23f7f3f3&clientId=uc749d4e4-db97-4&from=paste&height=317&id=uff09af4b&originHeight=476&originWidth=1825&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=281579&status=done&style=none&taskId=u370a24ee-86b6-40a4-9944-a37e4e0b1cd&title=&width=1216.6666666666667)
重写触发
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692435962552-7f33d48b-1a36-4e15-92aa-492fa62d69bb.png#averageHue=%23fef7f7&clientId=uc749d4e4-db97-4&from=paste&height=423&id=u1474253e&originHeight=634&originWidth=965&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=216975&status=done&style=none&taskId=uc3489fb9-10c6-4af3-9f90-86fa391c516&title=&width=643.3333333333334)
#### 手动触发案例
客户端向服务器发送bgrewriteaof命令
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692436047166-1e35cc4a-fa17-4ee8-a463-17c12945758a.png#averageHue=%23faf5f5&clientId=uc749d4e4-db97-4&from=paste&height=398&id=ua862ca32&originHeight=597&originWidth=1605&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=315258&status=done&style=none&taskId=u92db9498-ac5d-4570-91f3-6a92ab2a803&title=&width=1070)
### 结论
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692436086167-e56ac188-d114-4a89-801d-1d6bf4bf80ef.png#averageHue=%23b19a7d&clientId=uc749d4e4-db97-4&from=paste&height=133&id=u7265ee73&originHeight=199&originWidth=1362&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=199511&status=done&style=none&taskId=u0853e8f1-d7c1-401b-a60e-e9f7f961c67&title=&width=908)
### 重写原理
**可以参考知乎链接：**[**https://zhuanlan.zhihu.com/p/467217082**](https://zhuanlan.zhihu.com/p/467217082)
### AOF优化配置项详解
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692436176267-70649c0c-a39d-483e-bcbb-533177fc9e73.png#averageHue=%23e5dbc3&clientId=uc749d4e4-db97-4&from=paste&height=277&id=uac46ff96&originHeight=415&originWidth=1373&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=218504&status=done&style=none&taskId=ua037030c-8459-450e-bae3-95ebb8a1dd3&title=&width=915.3333333333334)
### 总结
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692436199849-2ab8f433-b6de-4aec-9615-245256157ec7.png#averageHue=%23faf8f7&clientId=uc749d4e4-db97-4&from=paste&height=443&id=ue4f77f52&originHeight=664&originWidth=917&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=237951&status=done&style=none&taskId=u264e3b30-2a7f-4383-a009-282fb1d476f&title=&width=611.3333333333334)
