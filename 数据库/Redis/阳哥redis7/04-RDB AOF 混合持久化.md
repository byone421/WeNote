# 是否可以共存
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692440738895-9f7515c2-045b-4331-9fbb-886ca1cd05a3.png#averageHue=%23fdf7f6&clientId=uab3d80ff-87a3-4&from=paste&height=437&id=u360a629b&originHeight=656&originWidth=1207&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=385442&status=done&style=none&taskId=u96b21d26-5c65-4606-9dee-a685c85026f&title=&width=804.6666666666666)
# 数据恢复顺序和加载流程
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692440776476-0dda553c-a2b4-481b-9775-3c33a2a1b379.png#averageHue=%2370c667&clientId=uab3d80ff-87a3-4&from=paste&height=456&id=ud21bab8b&originHeight=684&originWidth=1023&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=113047&status=done&style=none&taskId=u991a9dc1-a05d-48f8-ad18-bcf00d38aef&title=&width=682)
# 同时开启两种持久化方式
在这种情况下,当redis重启的时候会优先载入AOF文件来恢复原始的数据
因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整

RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件。那要不要只使用AOF呢?
作者建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份)，留着rdb作为一
个万一的手段

# RDB+AOF混合方式（推荐）
结合了RDB和AOF的优点，既能快速加载又能避免丢失过多的数据。
1 开启混合方式设置
设置aof-use-rdb-preamble的值为 yes   yes表示开启，设置为no表示禁用
2 RDB+AOF的混合方式---------> 结论：RDB镜像做全量持久化，AOF做增量持久化
先使用RDB进行快照存储，然后使用AOF持久化记录所有的写操作，当重写策略满足或手动触发重写的时候，将最新的数据存储为新的RDB记录。这样的话，重启服务的时候会从RDB和AOF两部分恢复数据，既保证了数据完整性，又提高了恢复数据的性能。简单来说：混合持久化方式产生的文件一部分是RDB格式，一部分是AOF格式。----》AOF包括了RDB头部+AOF混写
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1692443764128-cf50d35c-5573-498d-bed9-e27a7c287727.png#averageHue=%23f1eeee&clientId=uab3d80ff-87a3-4&from=paste&height=419&id=u4e92de4f&originHeight=629&originWidth=543&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=63238&status=done&style=none&taskId=ue3d33bfe-66d6-41e1-a709-202b670de7a&title=&width=362)
# 纯缓存模式
**同时关闭RDB+AOF**
save ""
禁用rdb：禁用rdb持久化模式下，我们仍然可以使用命令save、bgsave生成rdb文件
appendonly no
禁用aof：禁用aof持久化模式下，我们仍然可以使用命令bgrewriteaof生成aof文件
