# 亿级系统中常见的四种统计
## 聚合统计
交差并等集合统计
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698156500921-79a6e45b-ba62-4a5c-9dab-5c5a1f8c8d4b.png#averageHue=%23f2f2f2&clientId=u21e88101-fe1b-4&from=paste&height=330&id=ucdea3838&originHeight=495&originWidth=1483&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=186910&status=done&style=none&taskId=ubf60f9c0-9a56-4d43-b149-f123998577d&title=&width=988.6666666666666)
## 排序统计
抖音短视频最新评论留言的场景，请你设计一个展现列表考察你的数据结构和设计思路
在面对需要展示最新列表、排行榜等场景时
如果数据更新频繁或者需要分页显示，建议使用ZSet
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698156604477-a35883b5-f65e-4981-a960-35debf2d7281.png#averageHue=%23fefefe&clientId=u21e88101-fe1b-4&from=paste&height=452&id=u9b81f2ad&originHeight=678&originWidth=1386&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=161920&status=done&style=none&taskId=u85d09242-29e1-413a-ade7-6596bc7bc8f&title=&width=924)
## 二值统计
集合元素的取值就只有0和1两种
在钉钉上班签到打卡的场景中，我们只用记录有签到(1)或没签到(0)
使用bitmap
## 基数统计
指统计一个集合中不重复的元素个数，使用hyperloglog
# Hyperloglog![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698156946401-1c16872b-354e-49df-a347-6b35b930caf1.png#averageHue=%23f7f6f5&clientId=u21e88101-fe1b-4&from=paste&height=418&id=u72d41ff0&originHeight=627&originWidth=1221&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=89096&status=done&style=none&taskId=u1b22f50d-c877-4f5a-9f9a-bedfd74d08d&title=&width=814)
## 看需求
很多计数类场景，比如 每日注册 IP 数、每日访问 IP 数、页面实时访问数 PV、访问用户数 UV等。
因为主要的目标高效、巨量地进行计数，所以对存储的数据的内容并不太关心。

也就是说它只能用于统计巨量数量，不太涉及具体的统计对象的内容和精准性。

统计单日一个页面的访问量(PV)，单次访问就算一次。
统计单日一个页面的用户访问量(UV)，即按照用户为维度计算，单个用户一天内多次访问也只算一次。
多个key的合并统计，某个门户网站的所有模块的PV聚合统计就是整个网站的总PV。
## 基本命令
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698157105230-2da63080-bab8-4aab-9565-1cd334caadde.png#averageHue=%23d9c9b3&clientId=u21e88101-fe1b-4&from=paste&height=774&id=uacf19e0f&originHeight=1161&originWidth=974&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=242750&status=done&style=none&taskId=u4cf54fd0-d121-443d-8e15-e8eaf26915b&title=&width=649.3333333333334)
## HyPerLogLog如何做的? 如何演化出来的?
**去重复统计你先会想到哪些方式?**

1. **HashSet**
2. **bitmap**

如果数据显较大亿级统计,使用bitmaps同样会有这个问题。

bitmap是通过用位bit数组来表示各元素是否出现，每个元素对应一位，所需的总内存为N个bit。
基数计数则将每一个元素对应到bit数组中的其中一位，比如bit数组010010101(按照从零开始下标，有的就是1、4、6、8)。
新进入的元素只需要将已经有的bit数组和新加入的元素进行按位或计算就行。这个方式能大大减少内存占用且位操作迅速。

But，假设一个样本案例就是一亿个基数位值数据，一个样本就是一亿
如果要统计1亿个数据的基数位值,大约需要内存100000000/8/1024/1024约等于12M,内存减少占用的效果显著。
这样得到统计一个对象样本的基数值需要12M。

如果统计10000个对象样本(1w个亿级),就需要117.1875G将近120G，可见使用bitmaps还是不适用大数据量下(亿级)的基数计数场景，
但是bitmaps方法是精确计算的。

3. **基数统计？**

通过牺牲准确率来换取空间，对于不要求绝对准确率的场景下可以使用，因为概率算法不直接存储数据本身，
通过一定的概率统计方法预估基数值，同时保证误差在一定范围内，由于又不储存数据故此可以大大节约内存。
HyperLogLog就是一种概率算法的实现。
### 原理说明
只是进行不重复的基数统计，不是集合也不保存数据，只记录数量而不是具体内容
Hyperloglog提供不精确的去重计数方案
牺牲准确率来换取空间，误差仅仅只是0.81%左右
这个误差如何来的?论文地址和出处：
[http://antirez.com/news/75](http://antirez.com/news/75)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698158953125-15e924e5-5344-4291-b17b-8321ebb48f19.png#averageHue=%23e4d6bc&clientId=ua269feef-55db-4&from=paste&height=595&id=u4a447019&originHeight=892&originWidth=1417&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=296383&status=done&style=none&taskId=ue10d2ac5-b77a-4e93-b971-2aa165a5ecb&title=&width=944.6666666666666)
## 淘宝网站首页亿级UV的Redis统计方案
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698159130583-e336ab8c-be41-45f6-b142-424042f798e0.png#averageHue=%23ddd3bc&clientId=ua269feef-55db-4&from=paste&height=632&id=u50f8e6b4&originHeight=948&originWidth=1367&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=363854&status=done&style=none&taskId=u132452ac-4316-4f4f-8669-60254002181&title=&width=911.3333333333334)
**示例代码**
```java
package com.atguigu.redis.service;

import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;
import javax.annotation.Resource;
import java.util.Random;
import java.util.concurrent.TimeUnit;

/**
 * @auther zzyy
 * @create 2021-05-02 18:16
 */
@Service
@Slf4j
public class HyperLogLogService
{
    @Resource
    private RedisTemplate redisTemplate;

    /**
     * 模拟后台有用户点击首页，每个用户来自不同ip地址
     */
    @PostConstruct
    public void init()
    {
        log.info("------模拟后台有用户点击首页，每个用户来自不同ip地址");
        new Thread(() -> {
            String ip = null;
            for (int i = 1; i <=200; i++) {
                Random r = new Random();
                ip = r.nextInt(256) + "." + r.nextInt(256) + "." + r.nextInt(256) + "." + r.nextInt(256);

                Long hll = redisTemplate.opsForHyperLogLog().add("hll", ip);
                log.info("ip={},该ip地址访问首页的次数={}",ip,hll);
                //暂停几秒钟线程
                try { TimeUnit.SECONDS.sleep(3); } catch (InterruptedException e) { e.printStackTrace(); }
            }
        },"t1").start();
    }

}

```
```java
package com.atguigu.redis.controller;

import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

/**
 * @auther zzyy
 * @create 2021-05-02 18:16
 */
@Api(description = "淘宝亿级UV的Redis统计方案")
@RestController
@Slf4j
public class HyperLogLogController
{
    @Resource
    private RedisTemplate redisTemplate;

    @ApiOperation("获得IP去重后的首页访问量")
    @RequestMapping(value = "/uv",method = RequestMethod.GET)
    public long uv()
    {
        //pfcount
        return redisTemplate.opsForHyperLogLog().size("hll");
    }

}
```
# GEO
**面试题说明：**
移动互联网时代LBS应用越来越多，交友软件中附近的小姐姐、外卖软件中附近的美食店铺、打车软件附近的车辆等等。
那这种附近各种形形色色的XXX地址位置选择是如何实现的？

会有什么问题呢？
1.查询性能问题，如果并发高，数据量大这种查询是要搞垮mysql数据库的
2.一般mysql查询的是一个平面矩形访问，而叫车服务要以我为中心N公里为半径的圆形覆盖。
3.精准度的问题，我们知道地球不是平面坐标系，而是一个圆球，这种矩形计算在长距离计算时会有很大误差，mysql不合适

## 美团地图位置附近的酒店推送
**关键点**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698161304470-f24c2be7-c884-45cf-99ae-de716c9bfe7a.png#averageHue=%23f7f6f5&clientId=ua269feef-55db-4&from=paste&height=75&id=u5fadbc98&originHeight=112&originWidth=1477&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=47862&status=done&style=none&taskId=uc53b48b3-7149-4079-b4a8-a9d53a7ee25&title=&width=984.6666666666666)
**编码**
```java
package com.atguigu.redis7.controller;

import com.atguigu.redis7.service.GeoService;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import lombok.extern.slf4j.Slf4j;
import org.springframework.data.geo.*;
import org.springframework.data.redis.connection.RedisGeoCommands;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @auther zzyy
 * @create 2022-12-25 12:12
 */
@Api(tags = "美团地图位置附近的酒店推送GEO")
@RestController
@Slf4j
public class GeoController
{
    @Resource
    private GeoService geoService;

    @ApiOperation("添加坐标geoadd")
    @RequestMapping(value = "/geoadd",method = RequestMethod.GET)
    public String geoAdd()
    {
        return geoService.geoAdd();
    }

    @ApiOperation("获取经纬度坐标geopos")
    @RequestMapping(value = "/geopos",method = RequestMethod.GET)
    public Point position(String member)
    {
        return geoService.position(member);
    }

    @ApiOperation("获取经纬度生成的base32编码值geohash")
    @RequestMapping(value = "/geohash",method = RequestMethod.GET)
    public String hash(String member)
    {
        return geoService.hash(member);
    }

    @ApiOperation("获取两个给定位置之间的距离")
    @RequestMapping(value = "/geodist",method = RequestMethod.GET)
    public Distance distance(String member1, String member2)
    {
        return geoService.distance(member1,member2);
    }

    @ApiOperation("通过经度纬度查找北京王府井附近的")
    @RequestMapping(value = "/georadius",method = RequestMethod.GET)
    public GeoResults radiusByxy()
    {
        return geoService.radiusByxy();
    }

    @ApiOperation("通过地方查找附近,本例写死天安门作为地址")
    @RequestMapping(value = "/georadiusByMember",method = RequestMethod.GET)
    public GeoResults radiusByMember()
    {
        return geoService.radiusByMember();
    }

}
```
```java
package com.atguigu.redis7.service;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.geo.Distance;
import org.springframework.data.geo.GeoResults;
import org.springframework.data.geo.Metrics;
import org.springframework.data.geo.Point;
import org.springframework.data.geo.Circle;
import org.springframework.data.redis.connection.RedisGeoCommands;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @auther zzyy
 * @create 2022-12-25 12:11
 */
@Service
@Slf4j
public class GeoService
{
    public static final String CITY ="city";

    @Autowired
    private RedisTemplate redisTemplate;

    public String geoAdd()
    {
        Map<String, Point> map= new HashMap<>();
        map.put("天安门",new Point(116.403963,39.915119));
        map.put("故宫",new Point(116.403414 ,39.924091));
        map.put("长城" ,new Point(116.024067,40.362639));

        redisTemplate.opsForGeo().add(CITY,map);

        return map.toString();
    }

    public Point position(String member) {
        //获取经纬度坐标
        List<Point> list= this.redisTemplate.opsForGeo().position(CITY,member);
        return list.get(0);
    }


    public String hash(String member) {
        //geohash算法生成的base32编码值
        List<String> list= this.redisTemplate.opsForGeo().hash(CITY,member);
        return list.get(0);
    }


    public Distance distance(String member1, String member2) {
        //获取两个给定位置之间的距离
        Distance distance= this.redisTemplate.opsForGeo().distance(CITY,member1,member2, RedisGeoCommands.DistanceUnit.KILOMETERS);
        return distance;
    }

    public GeoResults radiusByxy() {
        //通过经度，纬度查找附近的,北京王府井位置116.418017,39.914402
        Circle circle = new Circle(116.418017, 39.914402, Metrics.KILOMETERS.getMultiplier());
        //返回50条
        RedisGeoCommands.GeoRadiusCommandArgs args = RedisGeoCommands.GeoRadiusCommandArgs.newGeoRadiusArgs().includeDistance().includeCoordinates().sortAscending().limit(50);
        GeoResults<RedisGeoCommands.GeoLocation<String>> geoResults= this.redisTemplate.opsForGeo().radius(CITY,circle, args);
        return geoResults;
    }

    public GeoResults radiusByMember() {
        //通过地方查找附近
        String member="天安门";
        //返回50条
        RedisGeoCommands.GeoRadiusCommandArgs args = RedisGeoCommands.GeoRadiusCommandArgs.newGeoRadiusArgs().includeDistance().includeCoordinates().sortAscending().limit(50);
        //半径10公里内
        Distance distance=new Distance(10, Metrics.KILOMETERS);
        GeoResults<RedisGeoCommands.GeoLocation<String>> geoResults= this.redisTemplate.opsForGeo().radius(CITY,member, distance,args);
        return geoResults;
    }
}
```
# bitmap
## 大厂真实面试题案例

1. 日活统计
2. 连续签到打卡
3. 最近一周活跃的用户
4. 统计指定用户一年之中的登录天数
5. 某用户按照一年365天，哪几天登陆过? 哪几天没有登陆? 全年中登录的天数共计多少?
## 是什么
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698161617616-3bb9af8d-f9ee-460b-8a39-2a1e6884f6a8.png#averageHue=%23f7f7f5&clientId=ua269feef-55db-4&from=paste&height=217&id=uf7961152&originHeight=325&originWidth=1436&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=163346&status=done&style=none&taskId=u2b4d90de-31e5-4330-a669-ef9dd182a4b&title=&width=957.3333333333334)
说明：用String类型作为底层数据结构实现的一种统计二值状态的数据类型
位图本质是数组，它是基于String数据类型的按位的操作。该数组由多个二进制位组成，每个二进制位都对应一个偏移量(我们可以称之为一个索引或者位格)。Bitmap支持的最大位数是2^32位，它可以极大的节约存储空间，使用512M内存就可以存储多大42.9亿的字节信息(2^32 = 4294967296)
## 京东签到领取京豆
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1698161761478-38142912-41c7-4bd7-86dd-77662f1293b3.png#averageHue=%23f6f6f3&clientId=ua269feef-55db-4&from=paste&height=441&id=u6dfaaf8c&originHeight=662&originWidth=818&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=270856&status=done&style=none&taskId=u672d6e5f-6cf0-4fbe-9929-58423dca1b6&title=&width=545.3333333333334)
签到日历仅展示当月签到数据
签到日历需展示最近连续签到天数
假设当前日期是20210618，且20210616未签到
若20210617已签到且0618未签到，则连续签到天数为1
若20210617已签到且0618已签到，则连续签到天数为2
连续签到天数越多，奖励越大
所有用户均可签到
截至2020年3月31日的12个月，京东年度活跃用户数3.87亿，同比增长24.8%，环比增长超2500万，此外，2020年3月移动端日均活跃用户数同比增长46%假设10%左右的用户参与签到，签到用户也高达3千万。。。。。。o(╥﹏╥)o
### 小厂方法，传统mysql实现
```java
CREATE TABLE user_sign
(
  keyid BIGINT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  user_key VARCHAR(200),#京东用户ID
  sign_date DATETIME,#签到日期(20210618)
  sign_count INT #连续签到天数
)
 
INSERT INTO user_sign(user_key,sign_date,sign_count)
VALUES ('20210618-xxxx-xxxx-xxxx-xxxxxxxxxxxx','2020-06-18 15:11:12',1);
 
SELECT
    sign_count
FROM
    user_sign
WHERE
    user_key = '20210618-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
    AND sign_date BETWEEN '2020-06-17 00:00:00' AND '2020-06-18 23:59:59'
ORDER BY
    sign_date DESC
    LIMIT 1;
```
方法正确但是难以落地实现，o(╥﹏╥)o。 
签到用户量较小时这么设计能行，但京东这个体量的用户（估算3000W签到用户，一天一条数据，一个月就是9亿数据）
对于京东这样的体量，如果一条签到记录对应着当日用记录，那会很恐怖......
**如何解决这个痛点？**
1 一条签到记录对应一条记录，会占据越来越大的空间。
2 一个月最多31天，刚好我们的int类型是32位，那这样一个int类型就可以搞定一个月，32位大于31天，当天来了位是1没来就是0。
3 一条数据直接存储一个月的签到记录，不再是存储一天的签到记录。

### 大厂方法，基于Redis的Bitmaps实现签到
在签到统计时，每个用户一天的签到用1个bit位就能表示，
一个月（假设是31天）的签到情况用31个bit位就可以，一年的签到也只需要用365个bit位，根本不用太复杂的集合类型
