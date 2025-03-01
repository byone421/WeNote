# Redis分布式锁-Redlock红锁算法
Distributed locks with Redis
> [https://redis.io/docs/manual/patterns/distributed-locks](https://redis.io/docs/manual/patterns/distributed-locks)

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698459033249-baa4eb27-7196-4c1d-b3df-47033c018111.png#averageHue=%23fbfaf9&clientId=u6c6b2f3c-dbf0-4&from=paste&height=738&id=uf9bfcd7b&originHeight=1107&originWidth=1394&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=555660&status=done&style=none&taskId=u8fd084bd-9e34-4788-b776-5d6def1adc3&title=&width=929.3333333333334)
# 为什么学习，怎么产生的
## 之前手写的有什么缺点
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698459213412-e95f8f9b-2e85-4dd7-a1b2-a6fac86d098b.png#averageHue=%23fef6f6&clientId=u6c6b2f3c-dbf0-4&from=paste&height=633&id=u84cc0f8c&originHeight=950&originWidth=1265&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=448638&status=done&style=none&taskId=uf67bee06-4da5-463b-9603-dd8466d9763&title=&width=843.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698459237917-bad76f11-9350-44c7-add7-af1fb464e4e6.png#averageHue=%23f6f3df&clientId=u6c6b2f3c-dbf0-4&from=paste&height=434&id=u3110e138&originHeight=651&originWidth=1566&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=245043&status=done&style=none&taskId=udac817f1-bb96-489a-9755-91497eadd51&title=&width=1044)
线程 1 首先获取锁成功，将键值对写入 redis 的 master 节点，在 redis 将该键值对同步到 slave 节点之前，master 发生了故障；redis 触发故障转移，其中一个 slave 升级为新的 master，此时新上位的master并不包含线程1写入的键值对，因此线程 2 尝试获取锁也可以成功拿到锁，此时相当于有两个线程获取到了锁，可能会导致各种预期之外的情况发生，例如最常见的脏数据
**我们加的是排它独占锁，同一时间只能有一个建redis锁成功并持有锁，严禁出现2个以上的请求线程拿到锁。危险的**
# RedLock算法设计理念
## redis之父提出了Redlock算法解决这个问题
Redis也提供了Redlock算法，用来实现基于多个实例的分布式锁。
锁变量由多个实例维护，即使有实例发生了故障，锁变量仍然是存在的，客户端还是可以完成锁操作。
Redlock算法是实现高可靠分布式锁的一种有效解决方案，可以在实际开发中使用
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698459913307-e0d594df-9575-448c-8bac-8079ae31e9cb.png#averageHue=%2338231f&clientId=u6c6b2f3c-dbf0-4&from=paste&height=553&id=u4068cd37&originHeight=829&originWidth=1177&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=486797&status=done&style=none&taskId=u246d1fdd-4fff-4641-bc88-19f51a5d051&title=&width=784.6666666666666)
## 设计理念
该方案也是基于（set 加锁、Lua 脚本解锁）进行改良的，所以redis之父antirez 只描述了差异的地方，大致方案如下。
假设我们有N个Redis主节点，例如 N = 5这些节点是完全独立的，我们不使用复制或任何其他隐式协调系统，
为了取到锁客户端执行以下操作：

1. 获取当前时间，以毫秒为单位；
2. 依次尝试从5个实例，使用相同的 key 和随机值（例如 UUID）获取锁。当向Redis 请求获取锁时，客户端应该设置一个超时时间，这个超时时间应该小于锁的失效时间。例如你的锁自动失效时间为 10 秒，则超时时间应该在 5-50 毫秒之间。这样可以防止客户端在试图与一个宕机的 Redis 节点对话时长时间处于阻塞状态。如果一个实例不可用，客户端应该尽快尝试去另外一个 Redis 实例请求获取锁；
3. 客户端通过当前时间减去步骤 1 记录的时间来计算获取锁使用的时间。当且仅当从大多数（N/2+1，这里是 3 个节点）的 Redis 节点都取到锁，并且获取锁使用的时间小于锁失效时间时，锁才算获取成功；
4. 如果取到了锁，其真正有效时间等于初始有效时间减去获取锁所使用的时间（步骤 3 计算的结果）
5. 如果由于某些原因未能获得锁（无法在至少 N/2 + 1 个 Redis 实例获取锁、或获取锁的时间超过了有效时间），客户端应该在所有的 Redis 实例上进行解锁（即便某些Redis实例根本就没有加锁成功，防止某些节点获取到锁但是客户端没有得到响应而导致接下来的一段时间不能被重新获取锁）。

该方案为了解决数据不一致的问题，直接舍弃了异步复制只使用 master 节点，同时由于舍弃了 slave，为了保证可用性，引入了 N 个节点，官方建议是 5
演示用3台实例来做说明。
客户端只有在满足下面的这两个条件时，才能认为是加锁成功。
条件1：客户端从超过半数（大于等于N/2+1）的Redis实例上成功获取到了锁；
条件2：客户端获取锁的总耗时没有超过锁的有效时间
## 解决方案
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698461401806-0db38ce9-0434-40a0-a20d-5b705d682b92.png#averageHue=%23f6f5dc&clientId=u6c6b2f3c-dbf0-4&from=paste&height=191&id=u80015a6f&originHeight=286&originWidth=1219&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=56948&status=done&style=none&taskId=u98f7449b-b07c-41bf-b4c3-2ded1e7ebce&title=&width=812.6666666666666)
** N = 2X + 1   (N是最终部署机器数，X是容错机器数)**

**1 先知道什么是容错**
失败了多少个机器实例后我还是可以容忍的，所谓的容忍就是数据一致性还是可以Ok的，CP数据一致性还是可以满足
加入在集群环境中，redis失败1台，可接受。2X+1 = 2 * 1+1 =3，部署3台，死了1个剩下2个可以正常工作，那就部署3台。
加入在集群环境中，redis失败2台，可接受。2X+1 = 2 * 2+1 =5，部署5台，死了2个剩下3个可以正常工作，那就部署5台。
**2 为什么是奇数？**
 最少的机器，最多的产出效果
 加入在集群环境中，redis失败1台，可接受。2N+2= 2 * 1+2 =4，部署4台
 加入在集群环境中，redis失败2台，可接受。2N+2 = 2 * 2+2 =6，部署6台
## 天上飞的理念(RedLock)必然有落地的实现(Redisson)
官网：
[https://redisson.org/](https://redisson.org/)
GitHub：
[https://github.com/redisson/redisson/wiki/%E7%9B%AE%E5%BD%95](https://github.com/redisson/redisson/wiki/%E7%9B%AE%E5%BD%95)
分布式锁：
[https://github.com/redisson/redisson/wiki/8.-Distributed-locks-and-synchronizers](https://github.com/redisson/redisson/wiki/8.-Distributed-locks-and-synchronizers)

# 使用Redission进行编码改造
pom.xml
```java
<dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson</artifactId>
    <version>3.13.4</version>
</dependency>
```
RedisConfig
```java
import org.redisson.Redisson;
import org.redisson.api.RedissonClient;
import org.redisson.config.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.lettuce.LettuceConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * @auther zzyy
 * @create 2022-10-22 15:14
 */
@Configuration
public class RedisConfig
{
    @Bean
    public RedisTemplate<String, Object> redisTemplate(LettuceConnectionFactory lettuceConnectionFactory)
    {
        RedisTemplate<String,Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(lettuceConnectionFactory);
        //设置key序列化方式string
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        //设置value的序列化方式json
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());

        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(new GenericJackson2JsonRedisSerializer());

        redisTemplate.afterPropertiesSet();

        return redisTemplate;
    }

    //单Redis节点模式
    @Bean
    public Redisson redisson()
    {
        Config config = new Config();
        config.useSingleServer().setAddress("redis://192.168.111.175:6379").setDatabase(0).setPassword("111111");
        return (Redisson) Redisson.create(config);
    }
}
```
InventoryController
```java
import com.atguigu.redislock.service.InventoryService;
import com.atguigu.redislock.service.InventoryService2;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @auther zzyy
 * @create 2022-10-22 15:23
 */
@RestController
@Api(tags = "redis分布式锁测试")
public class InventoryController
{
    @Autowired
    private InventoryService inventoryService;

    @ApiOperation("扣减库存，一次卖一个")
    @GetMapping(value = "/inventory/sale")
    public String sale()
    {
        return inventoryService.sale();
    }

    @ApiOperation("扣减库存saleByRedisson，一次卖一个")
    @GetMapping(value = "/inventory/saleByRedisson")
    public String saleByRedisson()
    {
        return inventoryService.saleByRedisson();
    }
}
```

InventoryService2
```java
import cn.hutool.core.util.IdUtil;
import com.atguigu.redislock.mylock.DistributedLockFactory;
import com.atguigu.redislock.mylock.RedisDistributedLock;
import lombok.extern.slf4j.Slf4j;
import org.redisson.Redisson;
import org.redisson.api.RLock;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.script.DefaultRedisScript;
import org.springframework.stereotype.Service;

import java.util.Arrays;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;

/**
 * @auther zzyy
 * @create 2022-10-25 16:07
 */
@Service
@Slf4j
public class InventoryService2
{
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
    @Value("${server.port}")
    private String port;
    @Autowired
    private DistributedLockFactory distributedLockFactory;

    @Autowired
    private Redisson redisson;
    public String saleByRedisson()
    {
        String retMessage = "";
        String key = "zzyyRedisLock";
        RLock redissonLock = redisson.getLock(key);
        redissonLock.lock();
        try
        {
            //1 查询库存信息
            String result = stringRedisTemplate.opsForValue().get("inventory001");
            //2 判断库存是否足够
            Integer inventoryNumber = result == null ? 0 : Integer.parseInt(result);
            //3 扣减库存
            if(inventoryNumber > 0) {
                stringRedisTemplate.opsForValue().set("inventory001",String.valueOf(--inventoryNumber));
                retMessage = "成功卖出一个商品，库存剩余: "+inventoryNumber;
                System.out.println(retMessage);
            }else{
                retMessage = "商品卖完了，o(╥﹏╥)o";
            }
        }finally {

          redissonLock.unlock();
        }
        return retMessage+"\t"+"服务端口号："+port;
    }
}
```
Jmeter测试
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698465898358-f36a0d93-223c-4f10-845b-55bc50cef94a.png#averageHue=%23f6edea&clientId=u6c6b2f3c-dbf0-4&from=paste&height=317&id=u8d3da27e&originHeight=475&originWidth=1691&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=441766&status=done&style=none&taskId=ubb698cf2-c292-4f3f-917e-debad5927fc&title=&width=1127.3333333333333)
```java

import cn.hutool.core.util.IdUtil;
import com.atguigu.redislock.mylock.DistributedLockFactory;
import com.atguigu.redislock.mylock.RedisDistributedLock;
import lombok.extern.slf4j.Slf4j;
import org.redisson.Redisson;
import org.redisson.api.RLock;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.core.script.DefaultRedisScript;
import org.springframework.stereotype.Service;

import java.util.Arrays;
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;

/**
 * @auther zzyy
 * @create 2022-10-25 16:07
 */
@Service
@Slf4j
public class InventoryService
{
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
    @Value("${server.port}")
    private String port;
    @Autowired
    private DistributedLockFactory distributedLockFactory;

    @Autowired
    private Redisson redisson;
    public String saleByRedisson()
    {
        String retMessage = "";
        String key = "zzyyRedisLock";
        RLock redissonLock = redisson.getLock(key);
        redissonLock.lock();
        try
        {
            //1 查询库存信息
            String result = stringRedisTemplate.opsForValue().get("inventory001");
            //2 判断库存是否足够
            Integer inventoryNumber = result == null ? 0 : Integer.parseInt(result);
            //3 扣减库存
            if(inventoryNumber > 0) {
                stringRedisTemplate.opsForValue().set("inventory001",String.valueOf(--inventoryNumber));
                retMessage = "成功卖出一个商品，库存剩余: "+inventoryNumber;
                System.out.println(retMessage);
            }else{
                retMessage = "商品卖完了，o(╥﹏╥)o";
            }
        }finally {
            if(redissonLock.isLocked() && redissonLock.isHeldByCurrentThread())
            {
                redissonLock.unlock();
            }
        }
        return retMessage+"\t"+"服务端口号："+port;
    }
}
```
# 源码解析
**守护线程“续命"**
额外起一个线程，定期检查线程是否还持有锁，如果有则延长过期时间。
Redisson 里面就实现了这个方案，使用“看门狗”定期检查（每1/3的锁时间检查1次），如果线程还持有锁，则刷新过期时间；
在获取锁成功后，给锁加一个 watchdog，watchdog会起一个定时任务，在锁没有被释放且快要过期的时候会续期
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698466906220-957055da-2cc0-4fd7-87e9-b03e700c313c.png#averageHue=%23dddfdd&clientId=u6c6b2f3c-dbf0-4&from=paste&height=680&id=u7662ef65&originHeight=1020&originWidth=1897&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=1050497&status=done&style=none&taskId=uf34c71ac-b834-40ad-8967-efc3a77a770&title=&width=1264.6666666666667)
## 分析1
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698467732125-c16808a0-509a-4ea6-b613-73bf8787ba48.png#averageHue=%23f9f5f3&clientId=u6c6b2f3c-dbf0-4&from=paste&height=688&id=u8e5b9764&originHeight=1032&originWidth=1913&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=427308&status=done&style=none&taskId=ud36af4e4-0401-4e9d-8d75-9d2b384b477&title=&width=1275.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698467740923-5bfe6b24-0f4f-4efa-a49c-8bfdae6e39df.png#averageHue=%23fcf9f8&clientId=u6c6b2f3c-dbf0-4&from=paste&height=668&id=ufdee14b8&originHeight=1002&originWidth=1073&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=281485&status=done&style=none&taskId=u35cf9dcf-2f69-4093-b9c0-680d63c2483&title=&width=715.3333333333334)
## 分析2
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698468743373-f38390b6-2d59-4bb9-831d-7f85888a1fb7.png#averageHue=%23e8d9bd&clientId=u6c6b2f3c-dbf0-4&from=paste&height=612&id=u2ba28a2d&originHeight=918&originWidth=1985&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=366558&status=done&style=none&taskId=udb0c2e18-f565-4d84-91bc-e6d11501551&title=&width=1323.3333333333333)
## 分析3
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698468763736-22c39223-af2b-4081-bad0-21573a150888.png#averageHue=%23fdf8f6&clientId=u6c6b2f3c-dbf0-4&from=paste&height=462&id=uaf2a5473&originHeight=693&originWidth=1817&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=361331&status=done&style=none&taskId=u04321490-5161-4f22-b10e-c877c3dc3aa&title=&width=1211.3333333333333)
通过exists判断，如果锁不存在，则设置值和过期时间，加锁成功
通过hexists判断，如果锁已存在，并且锁的是当前线程，则证明是重入锁，加锁成功
如果锁已存在，但锁的不是当前线程，则证明有其他线程持有锁
返回当前锁的过期时间(代表了锁key的剩余生存时间)，加锁失败
## 分析4
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698468913037-060a8bba-678c-4627-97fe-c5e7d4fcdd6f.png#averageHue=%23fbf4f1&clientId=u6c6b2f3c-dbf0-4&from=paste&height=267&id=ub7bf7f51&originHeight=401&originWidth=1528&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=150960&status=done&style=none&taskId=ua62e7297-670f-424b-b42e-9a6b715ff0a&title=&width=1018.6666666666666)
这里面初始化了一个定时器，dely 的时间是 internalLockLeaseTime/3。
在 Redisson 中，internalLockLeaseTime 是 30s，也就是每隔 10s 续期一次，每次 30s。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698468929669-4d5a9604-3523-467c-a516-6a5a57b70b4d.png#averageHue=%23fcfbf9&clientId=u6c6b2f3c-dbf0-4&from=paste&height=595&id=ua8734b3d&originHeight=892&originWidth=1069&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=235088&status=done&style=none&taskId=uf13eac78-79bc-4cd9-bdea-139923551ae&title=&width=712.6666666666666)
### 自动延期机制
客户端A加锁成功，就会启动一个watch dog看门狗，他是一个后台线程，会每隔10秒检查一下，如果客户端A还持有锁key，那么就会不断的延长锁key的生存时间，默认每次续命又从30秒新开始
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698468986292-0db86763-c521-4d15-bebc-93e2a34d2d45.png#averageHue=%23fdf7f6&clientId=u6c6b2f3c-dbf0-4&from=paste&height=409&id=ua5e69c52&originHeight=614&originWidth=1436&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=154366&status=done&style=none&taskId=u57d81210-6787-4fbc-a3da-54c27a92f29&title=&width=957.3333333333334)
### 续期lua分析
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698469008681-dc4d4ea3-5923-4e59-ad93-faf23b76e60d.png#averageHue=%23fbf6f2&clientId=u6c6b2f3c-dbf0-4&from=paste&height=258&id=uc006b0e7&originHeight=387&originWidth=1440&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=188374&status=done&style=none&taskId=ue6d654df-3091-4660-9a41-841b1d7b76d&title=&width=960)
## 解锁
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698469027513-07510861-714a-4de4-b250-cbfe33fd08d4.png#averageHue=%23fdfaf8&clientId=u6c6b2f3c-dbf0-4&from=paste&height=437&id=uc7bf10c1&originHeight=656&originWidth=2033&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=351998&status=done&style=none&taskId=u669ee49a-cd6d-4ae8-b9da-c216e82865a&title=&width=1355.3333333333333)
# 多机案例
## 理论参考来源
redis之父提出了Redlock算法解决这个问题
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698469227495-c46a73ae-b8d1-4ea7-94d1-75d13a09f81a.png#averageHue=%23f7f5f2&clientId=u6c6b2f3c-dbf0-4&from=paste&height=740&id=u1f21aadf&originHeight=1110&originWidth=1054&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=615384&status=done&style=none&taskId=u07498ac1-d2aa-48fe-9d00-4dcb441195f&title=&width=702.6666666666666)
## 小总结
这个锁的算法实现了多redis实例的情况，相对于单redis节点来说，优点在于 防止了 单节点故障造成整个服务停止运行的情况且在多节点中锁的设计，及多节点同时崩溃等各种意外情况有自己独特的设计方法。

Redisson 分布式锁支持 MultiLock 机制可以将多个锁合并为一个大锁，对一个大锁进行统一的申请加锁以及释放锁。
**最低保证分布式锁的有效性及安全性的要求如下：**
1.互斥；任何时刻只能有一个client获取锁
2.释放死锁；即使锁定资源的服务崩溃或者分区，仍然能释放锁
3.容错性；只要多数redis节点（一半以上）在使用，client就可以获取和释放锁

**网上讲的基于故障转移实现的redis主从无法真正实现Redlock:**
因为redis在进行主从复制时是异步完成的，比如在clientA获取锁后，主redis复制数据到从redis过程中崩溃了，导致没有复制到从redis中，然后从redis选举出一个升级为主redis,造成新的主redis没有clientA 设置的锁，这是clientB尝试获取锁，并且能够成功获取锁，导致互斥失效；
## 代码参考来源
[https://github.com/redisson/redisson/wiki/8.-Distributed-locks-and-synchronizers](https://github.com/redisson/redisson/wiki/8.-Distributed-locks-and-synchronizers)
## MultiLock多重锁
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698470231579-e7e61dd8-615f-44f5-a0cc-7ee303353268.png#averageHue=%23f8f5f4&clientId=u6c6b2f3c-dbf0-4&from=paste&height=664&id=u1c2b919b&originHeight=996&originWidth=1151&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=701907&status=done&style=none&taskId=u52a52b44-aebc-4189-bfda-0d0a4f10c62&title=&width=767.3333333333334)
## 案例
配置文件
```java
server.port=9090
spring.application.name=redlock


spring.swagger2.enabled=true


spring.redis.database=0
spring.redis.password=
spring.redis.timeout=3000
spring.redis.mode=single

spring.redis.pool.conn-timeout=3000
spring.redis.pool.so-timeout=3000
spring.redis.pool.size=10

spring.redis.single.address1=192.168.111.185:6381
spring.redis.single.address2=192.168.111.185:6382
spring.redis.single.address3=192.168.111.185:6383
```
一些配置类
CacheConfiguration
```java
import org.apache.commons.lang3.StringUtils;
import org.redisson.Redisson;
import org.redisson.api.RedissonClient;
import org.redisson.config.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnExpression;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@Configuration
@EnableConfigurationProperties(RedisProperties.class)
public class CacheConfiguration {

    @Autowired
    RedisProperties redisProperties;

    @Bean
    RedissonClient redissonClient1() {
        Config config = new Config();
        String node = redisProperties.getSingle().getAddress1();
        node = node.startsWith("redis://") ? node : "redis://" + node;
        SingleServerConfig serverConfig = config.useSingleServer()
                .setAddress(node)
                .setTimeout(redisProperties.getPool().getConnTimeout())
                .setConnectionPoolSize(redisProperties.getPool().getSize())
                .setConnectionMinimumIdleSize(redisProperties.getPool().getMinIdle());
        if (StringUtils.isNotBlank(redisProperties.getPassword())) {
            serverConfig.setPassword(redisProperties.getPassword());
        }
        return Redisson.create(config);
    }

    @Bean
    RedissonClient redissonClient2() {
        Config config = new Config();
        String node = redisProperties.getSingle().getAddress2();
        node = node.startsWith("redis://") ? node : "redis://" + node;
        SingleServerConfig serverConfig = config.useSingleServer()
                .setAddress(node)
                .setTimeout(redisProperties.getPool().getConnTimeout())
                .setConnectionPoolSize(redisProperties.getPool().getSize())
                .setConnectionMinimumIdleSize(redisProperties.getPool().getMinIdle());
        if (StringUtils.isNotBlank(redisProperties.getPassword())) {
            serverConfig.setPassword(redisProperties.getPassword());
        }
        return Redisson.create(config);
    }

    @Bean
    RedissonClient redissonClient3() {
        Config config = new Config();
        String node = redisProperties.getSingle().getAddress3();
        node = node.startsWith("redis://") ? node : "redis://" + node;
        SingleServerConfig serverConfig = config.useSingleServer()
                .setAddress(node)
                .setTimeout(redisProperties.getPool().getConnTimeout())
                .setConnectionPoolSize(redisProperties.getPool().getSize())
                .setConnectionMinimumIdleSize(redisProperties.getPool().getMinIdle());
        if (StringUtils.isNotBlank(redisProperties.getPassword())) {
            serverConfig.setPassword(redisProperties.getPassword());
        }
        return Redisson.create(config);
    }


    /**
     * 单机
     * @return
     */
    /*@Bean
    public Redisson redisson()
    {
        Config config = new Config();

        config.useSingleServer().setAddress("redis://192.168.111.147:6379").setDatabase(0);

        return (Redisson) Redisson.create(config);
    }*/

} 
```
RedisPoolProperties 
```java
import lombok.Data;

@Data
public class RedisPoolProperties {

    private int maxIdle;

    private int minIdle;

    private int maxActive;

    private int maxWait;

    private int connTimeout;

    private int soTimeout;

    /**
     * 池大小
     */
    private  int size;

}
```
RedisSingleProperties
```java
import lombok.Data;

@Data
public class RedisSingleProperties {
    private  String address1;
    private  String address2;
    private  String address3;
}

 
```

RedisProperties 
```java
import lombok.Data;
import lombok.ToString;
import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "spring.redis", ignoreUnknownFields = false)
@Data
public class RedisProperties {

    private int database;

    /**
     * 等待节点回复命令的时间。该时间从命令发送成功时开始计时
     */
    private int timeout;

    private String password;

    private String mode;

    /**
     * 池配置
     */
    private RedisPoolProperties pool;

    /**
     * 单机信息配置
     */
    private RedisSingleProperties single;


}

```
RedLockController
```java
import cn.hutool.core.util.IdUtil;
import lombok.extern.slf4j.Slf4j;
import org.redisson.Redisson;
import org.redisson.RedissonMultiLock;
import org.redisson.RedissonRedLock;
import org.redisson.api.RLock;
import org.redisson.api.RedissonClient;
import org.redisson.config.Config;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.data.redis.RedisProperties;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

@RestController
@Slf4j
public class RedLockController {

    public static final String CACHE_KEY_REDLOCK = "ATGUIGU_REDLOCK";

    @Autowired
    RedissonClient redissonClient1;

    @Autowired
    RedissonClient redissonClient2;

    @Autowired
    RedissonClient redissonClient3;

    boolean isLockBoolean;

    @GetMapping(value = "/multiLock")
    public String getMultiLock() throws InterruptedException
    {
        String uuid =  IdUtil.simpleUUID();
        String uuidValue = uuid+":"+Thread.currentThread().getId();

        RLock lock1 = redissonClient1.getLock(CACHE_KEY_REDLOCK);
        RLock lock2 = redissonClient2.getLock(CACHE_KEY_REDLOCK);
        RLock lock3 = redissonClient3.getLock(CACHE_KEY_REDLOCK);

        RedissonMultiLock redLock = new RedissonMultiLock(lock1, lock2, lock3);
        redLock.lock();
        try
        {
            System.out.println(uuidValue+"\t"+"---come in biz multiLock");
            try { TimeUnit.SECONDS.sleep(30); } catch (InterruptedException e) { e.printStackTrace(); }
            System.out.println(uuidValue+"\t"+"---task is over multiLock");
        } catch (Exception e) {
            e.printStackTrace();
            log.error("multiLock exception ",e);
        } finally {
            redLock.unlock();
            log.info("释放分布式锁成功key:{}", CACHE_KEY_REDLOCK);
        }

        return "multiLock task is over  "+uuidValue;
    }

}

```
# Redis的缓存过期淘汰策略
## Redis内存满了怎么办
### redis默认内存多少? 在哪里查看?如如何设置修改?

1. 查看Redis最大占用内存

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698474425445-8884ab64-4953-42c4-8479-2bfdc98fc3f4.png#averageHue=%23e8ead1&clientId=u6c6b2f3c-dbf0-4&from=paste&height=514&id=u2ef41c07&originHeight=771&originWidth=1246&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=230782&status=done&style=none&taskId=u94bdf91a-b5c0-491e-a2e2-5e4a4e42542&title=&width=830.6666666666666)

2. redis默认内存多少可以用?

注意，在 64bit 系统下，maxmemory 设置为 0表示不限制 Redis 内存使用，32位最多使用3G

3. 一般生产上你如何配置?

一般推荐Redis设置内存为最大物理内存的四分之三

4. 什么命令查看redis内存使用情况?

info memory
config get maxmemory
### 真要打满了会怎么样?
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698475641956-a288a441-cb66-4977-abf9-b329d7f9fe90.png#averageHue=%23e5de9c&clientId=u6c6b2f3c-dbf0-4&from=paste&height=423&id=u642d2415&originHeight=634&originWidth=1352&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=203380&status=done&style=none&taskId=u1741b93a-c5ab-4ed3-8b30-f4126da6f39&title=&width=901.3333333333334)
### 小总结
没有加上过期时间就会导致数据写满maxmemory
为了避免类似情况，引出下一章内存淘汰策略

# 往redis里写的数据是怎么没了的?它如何删除的?
### 讨论三种不同的删除策略

1. 立即删除

总结: 对CPU不友好，用处理器性能换取存储空间
(拿时间换空间)

2. 惰性删除

总结: 对memory不友好，用存储空间换取处理器性能 (拿空间换时间)
开启惰性淘汰，lazyfree-lazy-eviction=yes

3. 定期删除

期删除策略是前两种策略的折中：
定期删除策略每隔一段时间执行一次删除过期键操作并通过限制删除操作执行时长和频率来减少删除操作对CPU时间的影响。
周期性轮询redis库中的时效性数据，采用随机抽取的策略，利用过期数据占比的方式控制删除频度 
特点1：CPU性能占用设置有峰值，检测频度可自定义设置 
特点2：内存压力不是很大，长期占用内存的冷数据会被持续清理 
总结：周期性抽查存储空间 （随机抽查，重点抽查） 
**举例：**
redis默认每隔100ms检查是否有过期的key，有过期key则删除。注意：redis不是每隔100ms将所有的key检查一次而是随机抽取进行检查(如果每隔100ms,全部key进行检查，redis直接进去ICU)。因此，如果只采用定期删除策略，会导致很多key到时间没有删除。

定期删除策略的难点是确定删除操作执行的时长和频率：如果删除操作执行得太频繁或者执行的时间太长，定期删除策略就会退化成立即删除策略，以至于将CPU时间过多地消耗在删除过期键上面。如果删除操作执行得太少，或者执行的时间太短，定期删除策略又会和惰性删除束略一样，出现浪费内存的情况。因此，如果采用定期删除策略的话，服务器必须根据情况，合理地设置删除操作的执行时长和执行频率。

### 小总结
1 定期删除时，从来没有被抽查到
2 惰性删除时，也从来没有被点中使用过
上述两个步骤======>  大量过期的key堆积在内存中，导致redis内存空间紧张或者很快耗尽

必须要有一个更好的兜底方案，也就是redis的缓存淘汰策略
# redis缓存淘汰策略
## redis配置文件
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698476440650-279af8fb-32fb-4a9e-a51f-9f40eb15cb1b.png#averageHue=%23e0cba2&clientId=u6c6b2f3c-dbf0-4&from=paste&height=515&id=u7a2d7f11&originHeight=772&originWidth=1127&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=226359&status=done&style=none&taskId=ucdccc691-2ff7-4f46-901a-a3a54035099&title=&width=751.3333333333334)
## lru和lfu算法的区别是什么
**区别**
LRU：最近最少使用页面置换算法，淘汰最长时间未被使用的页面，看页面最后一次被使用到发生调度的时间长短，首先淘汰最长时间未被使用的页面。
LFU：最近最不常用页面置换算法，淘汰一定时期内被访问次数最少的页，看一定时间段内页面被使用的频率，淘汰一定时期内被访问次数最少的页
## 举个栗子
某次时期Time为10分钟,如果每分钟进行一次调页,主存块为3,若所需页面走向为2 1 2 1 2 3 4
假设到页面4时会发生缺页中断
若按LRU算法,应换页面1(1页面最久未被使用)，但按LFU算法应换页面3(十分钟内,页面3只使用了一次)
可见LRU关键是看页面最后一次被使用到发生调度的时间长短,而LFU关键是看一定时间段内页面被使用的频率!
## 有哪些（Redis7版本）

1. noeviction:不会驱逐任何key，表示即使内存达到上限也不进行置换，所有能引起内存增加的命令都会返回error
2. allkeys-lru: 对所有key使用LRU算法进行删除，优先删除掉最近最不经常使用的key，用以保存新数据
3. volatile-lru: 对所有设置了过期时间的key使用LRU算法进行删除
4. allkeys-random:对所有key随机删除
5. volatile-random: 对所有设置了过期时间的key随机删除
6. volatile-ttl: 删除马上要过期的key
7. allkeys-lfu: 对所有key使用LFU算法进行删除
8. volatile-lfu: 对所有设置了过期时间的key使用LFU算法进行删除
## 你平时使用哪一种
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698477194113-14075595-6a41-4bde-bfa9-a012eb8d7923.png#averageHue=%23e6e4e2&clientId=u6c6b2f3c-dbf0-4&from=paste&height=205&id=ud4ebbb47&originHeight=308&originWidth=1344&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=176017&status=done&style=none&taskId=u7e3b22ca-c747-4bf6-ade5-c695a2d78a5&title=&width=896)
## 建议
避免储存bigkey
开启惰性删除：lazyfree-lazy-eviction=yes
