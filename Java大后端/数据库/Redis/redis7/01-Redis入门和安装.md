# 是什么
Redis是一种Key-Value类型的缓存数据库。全称是Remote Dictionary Server(远程字典服务器）
主要功能如下：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1691676239941-ee95ecf7-7ecb-41ef-956a-1a71e51b16f2.png#averageHue=%23fbfcf9&clientId=ub4db7845-e5fa-4&from=paste&height=646&id=u875f8716&originHeight=969&originWidth=1394&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=1210778&status=done&style=none&taskId=u04c15a0b-c81b-4f3a-8721-2b65ab01dd7&title=&width=929.3333333333334)

# 下载redis
直接官网下载就好：[https://redis.io/download/](https://redis.io/download/)
# Redis安装
## windows
下载地址：[https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)
也可以网上自己找window的安装包
### 启动redis服务端
用cmd 在redis安装目录下执行
```java
redis-server.exe redis.conf 
```
### 启动客户端
启动另一个cmd窗口。服务端的窗口不能关闭（关闭了就是关了服务端）
切换到redis目录下运行
```java
redis-cli.exe -h 127.0.0.1 -p 6379 
```
### 测试
设置键值对
```java
set myKey abc
```
取出键值对 
```java
get myKey
```
## Linux
Linux环境安装Redis必须先具备gcc编译环境
> yum -y install gcc-c+ +

安装完成可以查看版本
> gcc -v

### 安装步骤
这里使用的是7.0.0的版本
#### 解压安装

1. 下载获得redis-7.0.0.tar.gz后将它放入我们的Linux目录/opt
2. 解压redis
   1. tar -zxvf redis-7.0.0.tar.gz
3. 进入到解压后的目录
4. 执行make命令编译并安装
   1. make && make install
5. 查看默认安装目录: usr/local/bin
   1. Linux下的/usr/local类似我们windows系统的C: Program Files
   2. redis-benchmark:性能测试工具，服务启动后运行该命令，看看自己本子性能如何
   3. redis-check-aof: 修复有问题的AOF文件
   4. redis-check-dump: 修复有问题的dump.rdb文件
   5. redis-cli: 客户端，操作入口
   6. redis-sentinel: redis集群使用
   7. redis-server: Redis服务器启动命令
#### 启动服务

1. 回到解压目录，备份配置文件
   1. cp redis.conf redis.conf.bak
2. 修改redis.conf配置文件
```java
 1 默认daemonize no               改为  daemonize yes
 2 默认protected-mode  yes        改为  protected-mode no
 3 默认bind 127.0.0.1             改为  直接注释掉(默认bind 127.0.0.1只能本机访问)或改成本机IP地址，否则影响远程IP连接
 4 添加redis密码                  改为 requirepass 你自己设置的密码
```

3. 启动服务
4. /usr/local/bin目录下运行redis-server
   1. redis-server  你的解压目录/redis.conf
5. ps -ef | grep redis 查看 redis 进程
#### 连接服务

1. 连接服务
   1. redis cli -a 111111
#### 关闭服务

1. 关闭服务
   1. 单实例关闭: redis-cli -a 111111 shutdown
   2. 多实例关闭，指定端口关闭:redis-cli -p 6379 shutdown
   3. 强制关闭
      1. 使用ps -ef | grep redis查看得到进程ID
      2. kill -9  进程ID
#### 卸载redis

1. 卸载前停止redis服务
   1. redis-cli shutdown
2. 删除/usr/local/lib下redis相关文件
   1. rm -rf  /usr/local/lib/redis-*
3. 删除redis的解压目录


