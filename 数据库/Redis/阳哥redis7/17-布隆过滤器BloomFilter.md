# 是什么
**一句话：**由一个初值都为零的bit数组和多个哈希函数构成
用来快速判断集合中是否存在某个元素
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698241507628-7546101c-fc82-4984-86f8-796dfa2a9676.png#averageHue=%23d9d9d9&clientId=ucf127a41-a301-4&from=paste&height=289&id=u097d9f2b&originHeight=434&originWidth=1072&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=93091&status=done&style=none&taskId=u5850269d-9cac-41bd-b128-741fdf79c11&title=&width=714.6666666666666)
**设计思想：**本质就是判断具体数据是否存在于一个大的集合中
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698241558591-8c8d9894-1df0-4d53-88d3-fb0a3c8f88a6.png#averageHue=%2373cabe&clientId=ucf127a41-a301-4&from=paste&height=437&id=ua7ea5141&originHeight=655&originWidth=1525&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=123315&status=done&style=none&taskId=ua838fb43-9f80-4def-95a4-b09c3a35db3&title=&width=1016.6666666666666)
布隆过滤器是一种类似set的数据结构只是统计结果在巨量数据下有点小瑕疵，不够完美
## 说明
布隆过滤器（英语：Bloom Filter）是 1970 年由布隆提出的。
它实际上是一个很长的二进制数组(00000000)+一系列随机hash算法映射函数，主要用于判断一个元素是否在集合中。
通常我们会遇到很多要判断一个元素是否在某个集合中的业务场景，一般想到的是将集合中所有元素保存起来，然后通过比较确定。
链表、树、哈希表等等数据结构都是这种思路。但是随着集合中元素的增加，我们需要的存储空间也会呈现线性增长，最终达到瓶颈。同时检索速度也越来越慢，上述三种结构的检索时间复杂度分别为O(n),O(logn),O(1)。这个时候，布隆过滤器（Bloom Filter）就应运而生。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698241982657-70a7d85e-70d6-4313-92d7-6a0afc189ab6.png#averageHue=%23eef0f3&clientId=ucf127a41-a301-4&from=paste&height=272&id=u95e49d2e&originHeight=408&originWidth=1314&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=50955&status=done&style=none&taskId=ufb346e5e-7d45-42b1-bea3-1e3df6b20fb&title=&width=876)
# 能干嘛（特点）
高效地插入和查询，占用空闻少，返回的结果是不确定性+不够完美，
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698242237585-bc9a67b3-3547-4612-8f57-6d5ef85cd514.png#averageHue=%23a9b2ae&clientId=ucf127a41-a301-4&from=paste&height=420&id=ub0aae592&originHeight=630&originWidth=1386&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=121743&status=done&style=none&taskId=u01601932-d75b-4599-bbb3-f913f1feed0&title=&width=924)
## 重点
一个元素如果判断结果存在时，元素不一定存在，但是判断结果为不存在时，则一定不存在。
布隆过滤器可以添加元素，但是不能删除元素。由于涉及hashcode判断依据，删掉元素会导致误判率增加

# 布隆过滤器原理
## 原理
**布隆过滤器(Bloom Filter) 是一种专门用来解决去重问题的高级数据结构。**
实质就是一个大型位数组和几个不同的无偏hash函数(无偏表示分布均匀)。由一个初值都为零的bit数组和多个个哈希函数构成，用来快速判断某个数据是否存在。但是跟 HyperLogLog 一样，它也一样有那么一点点不精确，也存在一定的误判概率
## 添加key时
使用多个hash函数对key进行hash运算得到一个整数索引值，对位数组长度进行取模运算得到一个位置，
每个hash函数都会得到一个不同的位置，将这几个位置都置1就完成了add操作。
## 查询key时
只要有其中一位是零就表示这个key不存在，但如果都是1，则不一定存在对应的key。
## Hash冲突导致数据不精准
当有变量被加入集合时，通过N个映射函数将这个变量映射成位图中的N个点，
把它们置为 1（假定有两个变量都通过 3 个映射函数）。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698242892878-5f3cae66-3415-4151-a890-a359e102ef7d.png#averageHue=%23d9d7d3&clientId=ucf127a41-a301-4&from=paste&height=341&id=uc6c1bd97&originHeight=512&originWidth=1062&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=181476&status=done&style=none&taskId=u2460363a-ba21-47e4-91f9-8a7f4b248b9&title=&width=708)
查询某个变量的时候我们只要看看这些点是不是都是 1， 就可以大概率知道集合中有没有它了
 
如果这些点，有任何一个为零则被查询变量一定不在，
如果都是 1，则被查询变量很可能存在，
为什么说是可能存在，而不是一定存在呢？那是因为映射函数本身就是散列函数，散列函数是会有碰撞的。（见上图3号坑两个对象都1）
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698242926253-d8e91671-bef8-4e4d-b50a-64bec3373bcd.png#averageHue=%23f1efec&clientId=ucf127a41-a301-4&from=paste&height=188&id=u1afbcfbb&originHeight=282&originWidth=1574&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=286259&status=done&style=none&taskId=ufbd7c494-42f4-492f-8464-7793e18f6a5&title=&width=1049.3333333333333)

## 使用3步骤
### 初始化bitmap
布隆过滤器 本质上 是由长度为 m 的位向量或位列表（仅包含 0 或 1 位值的列表）组成，最初所有的值均设置为 0
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698243104626-f7f259e0-9a12-4382-8702-7b5ef43359d4.png#averageHue=%23eef0f4&clientId=ucf127a41-a301-4&from=paste&height=193&id=u88efdc33&originHeight=290&originWidth=913&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=27689&status=done&style=none&taskId=u1fc7b6b4-0b48-428a-829c-0de8a8f302f&title=&width=608.6666666666666)

### 添加元素
当我们向布隆过滤器中添加数据时，为了尽量地址不冲突，会使用多个 hash 函数对 key 进行运算，算得一个下标索引值，然后对位数组长度进行取模运算得到一个位置，每个 hash 函数都会算得一个不同的位置。再把位数组的这几个位置都置为 1 就完成了 add 操作。

例如，我们添加一个字符串wmyskxz，对字符串进行多次hash(key) → 取模运行→ 得到坑位
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698243184704-72f0da24-0ce1-4680-8cad-f048e3a1cf8d.png#averageHue=%23e7ecf2&clientId=ucf127a41-a301-4&from=paste&height=335&id=u7dae1322&originHeight=502&originWidth=1619&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=67231&status=done&style=none&taskId=uc1b8ee45-82da-435d-9181-efc3411498e&title=&width=1079.3333333333333)
### 判断是否存在
就比如我们在 add 了字符串wmyskxz数据之后，很明显下面1/3/5 这几个位置的 1 是因为第一次添加的 wmyskxz 而导致的；
此时我们查询一个没添加过的不存在的字符串inexistent-key，它有可能计算后坑位也是1/3/5 ，这就是误判了......
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698243281652-efcf3cbb-46f7-4c65-8486-ee3c13fee222.png#averageHue=%23e9ecf1&clientId=ucf127a41-a301-4&from=paste&height=491&id=u62c5c4f2&originHeight=737&originWidth=1090&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=84868&status=done&style=none&taskId=u7e58e246-4cf4-48ba-a343-c52d61561b8&title=&width=726.6666666666666)

## 布隆过滤器误判率，为什么不要删除
布隆过滤器的误判是指多个输入经过哈希之后在相同的bit位置1了，这样就无法判断究竟是哪个输入产生的，
因此误判的根源在于相同的 bit 位被多次映射且置 1。

这种情况也造成了布隆过滤器的删除问题，因为布隆过滤器的每一个 bit 并不是独占的，很有可能多个元素共享了某一位。
如果我们直接删除这一位的话，会影响其他的元素
**特性**
布隆过滤器可以添加元素，但是不能删除元素。因为删掉元素会导致误判率增加。
## 小总结

1. 有，是可能有。无肯定是无。
2. 使用时最好不要让实际元素数量远大于初始化数量，一次给够避免扩容
3. 当实际元素数量超过初始化数量时，应该对布隆过滤器进行重建

重新分配一个 size 更大的过滤器，再将所有的历史元素批量 add 进行
# 使用场景
## 解决缓存穿透的问题，和redis结合bitmap使用
**缓存穿透是什么**
一般情况下，先查询缓存redis是否有该条数据，缓存中没有时，再查询数据库。
当数据库也不存在该条数据时，每次查询都要访问数据库，这就是缓存穿透。
缓存透带来的问题是，当有大量请求查询数据库不存在的数据时，就会给数据库带来压力，甚至会拖垮数据库。
**可以使用布隆过滤器解决缓存穿透的问题**
把已存在数据的key存在布隆过滤器中，相当于redis前面挡着一个布隆过滤器。

当有新的请求时，先到布隆过滤器中查询是否存在：
如果布隆过滤器中不存在该条数据则直接返回；
如果布隆过滤器中已存在，才去查询缓存redis，如果redis里没查询到则再查询Mysql数据库
## 黑名单校验，识别垃圾邮件
发现存在黑名单中的，就执行特定操作。比如：识别垃圾邮件，只要是邮箱在黑名单中的邮件，就识别为垃圾邮件。

假设黑名单的数量是数以亿计的，存放起来就是非常耗费存储空间的，布隆过滤器则是一个较好的解决方案。
把所有黑名单都放在布隆过滤器中，在收到邮件时，判断邮件地址是否在布隆过滤器中即可。
## 安全连接网址，全球10亿网址的判断

# 手写布隆过滤器，体会思想
结合bitmap类型手写一个简单的布隆过滤器，体会设计思想
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698244057093-ab78858d-1b74-4657-8a48-c5c5b7f31127.png#averageHue=%23efeeed&clientId=ucf127a41-a301-4&from=paste&height=509&id=u5ca5b841&originHeight=764&originWidth=1301&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=316233&status=done&style=none&taskId=u49dfd4b2-2b0f-4b6d-99d4-504c8f33a65&title=&width=867.3333333333334)
## 步骤设计
**redis的setbit/getbit**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698244630301-d160a7ff-9547-4d31-ba6e-ec7665d06c76.png#averageHue=%23f3f1f1&clientId=ucf127a41-a301-4&from=paste&height=345&id=u80ce1dfa&originHeight=518&originWidth=820&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=156169&status=done&style=none&taskId=u9ee554f8-333d-483f-821d-aa55ea34d0a&title=&width=546.6666666666666)
**setBit的构建过程**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698244649966-f92630a6-8221-4005-a20b-5294d811dc87.png#averageHue=%23f2f1ef&clientId=ucf127a41-a301-4&from=paste&height=199&id=u3cb2f068&originHeight=299&originWidth=726&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=47730&status=done&style=none&taskId=u3baf1f7f-e26d-48df-97b9-b5a917e72f0&title=&width=484)
**getBit查询是否存在**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698244671006-712461e8-f661-4841-86ba-987bccf19027.png#averageHue=%23f2f0ee&clientId=ucf127a41-a301-4&from=paste&height=149&id=ud455ffa7&originHeight=224&originWidth=744&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=32895&status=done&style=none&taskId=u6108aba3-88a1-4dd1-b915-c7d5be4fbb0&title=&width=496)
## 案例
**BloomFilterInit(白名单**
```java
package com.atguigu.redis7.filter;

import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import javax.annotation.Resource;

/**
 * @auther zzyy
 * @create 2022-12-27 14:55
 * 布隆过滤器白名单初始化工具类，一开始就设置一部分数据为白名单所有，
 * 白名单业务默认规定：布隆过滤器有，redis也有。
 */
@Component
@Slf4j
public class BloomFilterInit
{
    @Resource
    private RedisTemplate redisTemplate;

    @PostConstruct//初始化白名单数据，故意差异化数据演示效果......
    public void init()
    {
        //白名单客户预加载到布隆过滤器
        String uid = "customer:12";
        //1 计算hashcode，由于可能有负数，直接取绝对值
        int hashValue = Math.abs(uid.hashCode());
        //2 通过hashValue和2的32次方取余后，获得对应的下标坑位
        long index = (long) (hashValue % Math.pow(2, 32));
        log.info(uid+" 对应------坑位index:{}",index);
        //3 设置redis里面bitmap对应坑位，该有值设置为1
        redisTemplate.opsForValue().setBit("whitelistCustomer",index,true);
    }
}
```
**CheckUtils**
```java
package com.atguigu.redis7.utils;

import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;

/**
 * @auther zzyy
 * @create 2022-12-27 14:56
 */
@Component
@Slf4j
public class CheckUtils
{
    @Resource
    private RedisTemplate redisTemplate;

    public boolean checkWithBloomFilter(String checkItem,String key)
    {
        int hashValue = Math.abs(key.hashCode());
        long index = (long) (hashValue % Math.pow(2, 32));
        boolean existOK = redisTemplate.opsForValue().getBit(checkItem, index);
        log.info("----->key:"+key+"\t对应坑位index:"+index+"\t是否存在:"+existOK);
        return existOK;
    }
}

```
**CustomerSerivce**
```java
package com.atguigu.redis7.service;

import com.atguigu.redis7.entities.Customer;
import com.atguigu.redis7.mapper.CustomerMapper;
import com.atguigu.redis7.utils.CheckUtils;
import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;
import javax.annotation.Resource;
import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicInteger;

/**
 * @auther zzyy
 * @create 2022-07-23 13:55
 */
@Service
@Slf4j
public class CustomerSerivce
{
    public static final String CACHE_KEY_CUSTOMER = "customer:";

    @Resource
    private CustomerMapper customerMapper;
    @Resource
    private RedisTemplate redisTemplate;

    @Resource
    private CheckUtils checkUtils;

    public void addCustomer(Customer customer){
        int i = customerMapper.insertSelective(customer);

        if(i > 0)
        {
            //到数据库里面，重新捞出新数据出来，做缓存
            customer=customerMapper.selectByPrimaryKey(customer.getId());
            //缓存key
            String key=CACHE_KEY_CUSTOMER+customer.getId();
            //往mysql里面插入成功随后再从mysql查询出来，再插入redis
            redisTemplate.opsForValue().set(key,customer);
        }
    }

    public Customer findCustomerById(Integer customerId){
        Customer customer = null;

        //缓存key的名称
        String key=CACHE_KEY_CUSTOMER+customerId;

        //1 查询redis
        customer = (Customer) redisTemplate.opsForValue().get(key);

        //redis无，进一步查询mysql
        if(customer==null)
        {
            //2 从mysql查出来customer
            customer=customerMapper.selectByPrimaryKey(customerId);
            // mysql有，redis无
            if (customer != null) {
                //3 把mysql捞到的数据写入redis，方便下次查询能redis命中。
                redisTemplate.opsForValue().set(key,customer);
            }
        }
        return customer;
    }

    /**
     * BloomFilter → redis → mysql
     * 白名单：whitelistCustomer
     * @param customerId
     * @return
     */

    @Resource
    private CheckUtils checkUtils;
    public Customer findCustomerByIdWithBloomFilter (Integer customerId)
    {
        Customer customer = null;

        //缓存key的名称
        String key = CACHE_KEY_CUSTOMER + customerId;

        //布隆过滤器check，无是绝对无，有是可能有
        //===============================================
        if(!checkUtils.checkWithBloomFilter("whitelistCustomer",key))
        {
            log.info("白名单无此顾客信息:{}",key);
            return null;
        }
        //===============================================

        //1 查询redis
        customer = (Customer) redisTemplate.opsForValue().get(key);
        //redis无，进一步查询mysql
        if (customer == null) {
            //2 从mysql查出来customer
            customer = customerMapper.selectByPrimaryKey(customerId);
            // mysql有，redis无
            if (customer != null) {
                //3 把mysql捞到的数据写入redis，方便下次查询能redis命中。
                redisTemplate.opsForValue().set(key, customer);
            }
        }
        return customer;
    }
}
```
**CustomerController**
```java
package com.atguigu.redis7.controller;

import com.atguigu.redis7.entities.Customer;
import com.atguigu.redis7.service.CustomerSerivce;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.Random;
import java.util.Date;
import java.util.concurrent.ExecutionException;

/**
 * @auther zzyy
 * @create 2022-07-23 13:55
 */
@Api(tags = "客户Customer接口+布隆过滤器讲解")
@RestController
@Slf4j
public class CustomerController
{
    @Resource private CustomerSerivce customerSerivce;

    @ApiOperation("数据库初始化2条Customer数据")
    @RequestMapping(value = "/customer/add", method = RequestMethod.POST)
    public void addCustomer() {
        for (int i = 0; i < 2; i++) {
            Customer customer = new Customer();

            customer.setCname("customer"+i);
            customer.setAge(new Random().nextInt(30)+1);
            customer.setPhone("1381111xxxx");
            customer.setSex((byte) new Random().nextInt(2));
            customer.setBirth(Date.from(LocalDateTime.now().atZone(ZoneId.systemDefault()).toInstant()));

            customerSerivce.addCustomer(customer);
        }
    }

    @ApiOperation("单个用户查询，按customerid查用户信息")
    @RequestMapping(value = "/customer/{id}", method = RequestMethod.GET)
    public Customer findCustomerById(@PathVariable int id) {
        return customerSerivce.findCustomerById(id);
    }

    @ApiOperation("BloomFilter案例讲解")
    @RequestMapping(value = "/customerbloomfilter/{id}", method = RequestMethod.GET)
    public Customer findCustomerByIdWithBloomFilter(@PathVariable int id) throws ExecutionException, InterruptedException
    {
        return customerSerivce.findCustomerByIdWithBloomFilter(id);
    }
}

```
# 布隆过滤器优缺点
## 优点
高效地插入和查询，内存占用bit空间少
## 缺点
不能删除元素。
因为删掉元素会导致误判率增加，因为hash冲突同一个位置可能存的东西是多个共有的
你删除一个元素的同时可能也把其它的删除了。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698244879467-3fd79b33-b86b-423d-ae57-e6160907a76a.png#averageHue=%23fbfaf9&clientId=ucf127a41-a301-4&from=paste&height=94&id=u8513c255&originHeight=141&originWidth=956&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=26095&status=done&style=none&taskId=u73d47b53-d55b-4918-94b3-124305dd3c4&title=&width=637.3333333333334)
# 布谷鸟过滤器
为了解决布隆过滤器不能删除元素的问题，布谷鸟过滤器横空出世。
论文《Cuckoo Filter：Better Than Bloom》
[https://www.cs.cmu.edu/~binfan/papers/conext14_cuckoofilter.pdf#:~:text=Cuckoo%20%EF%AC%81lters%20support%20adding%20and%20removing%20items%20dynamically,have%20lower%20space%20overhead%20than%20space-optimized%20Bloom%20%EF%AC%81lters.](https://www.cs.cmu.edu/~binfan/papers/conext14_cuckoofilter.pdf#:~:text=Cuckoo%20%EF%AC%81lters%20support%20adding%20and%20removing%20items%20dynamically,have%20lower%20space%20overhead%20than%20space-optimized%20Bloom%20%EF%AC%81lters.)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698244936376-03f253e1-a0b7-4870-866e-ecf7dcd6e180.png#averageHue=%23c0a98f&clientId=ucf127a41-a301-4&from=paste&height=129&id=u4876856a&originHeight=193&originWidth=1078&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=117090&status=done&style=none&taskId=u4d32612a-0184-48f3-83f9-f54628a23cd&title=&width=718.6666666666666)
作者将布谷鸟过滤器和布隆过滤器进行了深入的对比，目前用的比较多比较成熟的就是布隆过滤器。
