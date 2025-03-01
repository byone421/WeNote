# 1. 逻辑控制器
## 1. if逻辑控制器
要求：如果用户是张三我们就发送一个百度请求，不过不是就不发送请求

1. 新增一个用户变量 userName

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492245743-02722639-6b82-4fa7-a51a-ed62fe5642f6.png#averageHue=%233c4147&clientId=u9419d501-6644-4&from=paste&height=475&id=u9624e7a2&originHeight=713&originWidth=958&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88690&status=done&style=none&taskId=u9302da45-66c5-436c-aeef-c823420374b&title=&width=638.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492253933-75b107c5-9503-43da-90bc-68cfe026c437.png#averageHue=%233e4143&clientId=u9419d501-6644-4&from=paste&height=268&id=u5703c86e&originHeight=402&originWidth=1800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18537&status=done&style=none&taskId=uff68a6a6-d54a-4a58-829d-190cdde5714&title=&width=1200)

2. 新增一个逻辑控制器if

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492296283-37688055-8fd8-4a69-aadb-86ad339b5546.png#averageHue=%233d4245&clientId=u9419d501-6644-4&from=paste&height=496&id=u6bd1b982&originHeight=744&originWidth=1299&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105937&status=done&style=none&taskId=u8d018309-5dca-44a7-804c-3f47d9ebdd0&title=&width=866)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492390963-b8289364-a792-4d03-b940-91e08a4cc389.png#averageHue=%23414547&clientId=u9419d501-6644-4&from=paste&height=967&id=u79c5d683&originHeight=1450&originWidth=1953&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70432&status=done&style=none&taskId=u27161bba-1f6a-49d1-9422-6744fa2e62c&title=&width=1302)

3. 把http请求拖到if控制器下面，让if控制器成为Http请求的父亲

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492462082-4f76284c-1c40-4d07-bb3b-a3ac2efb4e05.png#averageHue=%233d4043&clientId=u9419d501-6644-4&from=paste&height=750&id=u87dd163c&originHeight=1125&originWidth=2461&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90763&status=done&style=none&taskId=ud2c6a648-c651-433e-a8f2-c286a7920fb&title=&width=1640.6666666666667)

4. 执行发现条件满足，请求成功发送

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492517181-391fc82d-d86b-441b-9f69-f2f654608c36.png#averageHue=%233c4043&clientId=u9419d501-6644-4&from=paste&height=589&id=u9324cff1&originHeight=884&originWidth=1611&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78042&status=done&style=none&taskId=u93467bae-1e73-465f-b737-0ffbc3ee1c4&title=&width=1074)

5. 改变条件。再次运行。我们可以发现条件不满足，请求不会发送

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492573454-6f923793-ec88-4378-b756-f2adab3f0d76.png#averageHue=%23414546&clientId=u9419d501-6644-4&from=paste&height=423&id=u5e5b698e&originHeight=634&originWidth=1405&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22058&status=done&style=none&taskId=uc914324d-6e52-4e3d-814d-5f647ac2b11&title=&width=936.6666666666666)
## 2. foreach控制器
要求：有一组关关键字，需要循环取出然后在用百度搜索

1. 先创建出如下结构

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492958980-fcfd4a9d-28ed-461c-85ca-f605d4169909.png#averageHue=%233e4244&clientId=u9419d501-6644-4&from=paste&height=461&id=u8ecded4d&originHeight=691&originWidth=1949&originalType=binary&ratio=1&rotation=0&showTitle=false&size=61177&status=done&style=none&taskId=u8d964e44-4d7d-4613-b9b4-eabeeeb4be1&title=&width=1299.3333333333333)

2. 创建3个有规律的变量名

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670493020044-ce42ba6b-b781-4153-9756-0c02cdbaa8dc.png#averageHue=%233d4043&clientId=u9419d501-6644-4&from=paste&height=356&id=u85658f58&originHeight=534&originWidth=2489&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66840&status=done&style=none&taskId=uf019a4bc-5b54-4da9-a36d-c39b3e453c6&title=&width=1659.3333333333333)

3. 设置foreach控制器

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670493222097-bfb5c5d7-083a-43e2-a999-550d947f29af.png#averageHue=%233d4143&clientId=u9419d501-6644-4&from=paste&height=572&id=uf1c142d5&originHeight=858&originWidth=2494&originalType=binary&ratio=1&rotation=0&showTitle=false&size=128591&status=done&style=none&taskId=u1719cc5f-fee0-45c2-aa85-87da6c0f3df&title=&width=1662.6666666666667)

4. 设置http请求并运行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670493278741-ee53ed86-bca6-4212-a19a-ceed08851583.png#averageHue=%233d4043&clientId=u9419d501-6644-4&from=paste&height=483&id=ue236a667&originHeight=724&originWidth=2446&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69384&status=done&style=none&taskId=u11f5d8fa-1d61-4ce5-afe9-8d9189e07f2&title=&width=1630.6666666666667)
# 2. 接口关联
比如接口2的参数需要接口1的结果作为查询条件
要求：获取[https://www.itcast.cn/](https://www.itcast.cn/)网页的title字段作为www.baidu.com的查询条件

1. 新建一个Http请求

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670495080165-4e968fb0-40ba-4dea-b6c6-164e0ee95346.png#averageHue=%233d4144&clientId=u69545ab7-c163-4&from=paste&height=343&id=u6ae1f8df&originHeight=514&originWidth=2409&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57551&status=done&style=none&taskId=u6042993d-e2b5-491c-bd6a-d33afbe8de5&title=&width=1606)

2. 在请求下面新建一个Xpath提取器。用来提取网页中的title

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670495121852-6c886524-3991-44b5-b878-cc2f04522495.png#averageHue=%233d4146&clientId=u69545ab7-c163-4&from=paste&height=509&id=u71dc5a6a&originHeight=763&originWidth=1069&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97111&status=done&style=none&taskId=u7c941331-c5d2-44d5-bcde-e529ce9b91f&title=&width=712.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670495655938-742352ad-682f-43a1-9071-72f256a11dd3.png#averageHue=%233d4043&clientId=u69545ab7-c163-4&from=paste&height=717&id=u1425f29a&originHeight=1075&originWidth=2478&originalType=binary&ratio=1&rotation=0&showTitle=false&size=127242&status=done&style=none&taskId=u5eb25444-e188-4d4e-9f97-f3824145358&title=&width=1652)

3. 再新建一个请求百度，并引用val变量

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670496162156-151fe82c-55ce-49f2-99a3-9745c04c91c7.png#averageHue=%233d4143&clientId=u383144c0-174f-4&from=paste&height=605&id=u205d2045&originHeight=908&originWidth=2465&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85834&status=done&style=none&taskId=u5ef62049-bbab-4a24-8ae4-30fa6bb01bc&title=&width=1643.3333333333333)

4. 运行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670496188353-bb4dd7f3-305a-4400-8a82-9e64f8d6b204.png#averageHue=%23404345&clientId=u383144c0-174f-4&from=paste&height=811&id=u51fd836d&originHeight=1216&originWidth=2513&originalType=binary&ratio=1&rotation=0&showTitle=false&size=115227&status=done&style=none&taskId=u5f4081a7-be74-4d1b-8ce3-f9d41a8d6da&title=&width=1675.3333333333333)
# 3. Json提取器
上一个我们说了xpath提取器这里我们演示一下json提取器，其他的可以自行尝试

要求： 从接口返回的json中提取所有name值

1. 设置http请求

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500285375-64da8d1a-252a-406e-8623-0adb0397f5ad.png#averageHue=%233e4244&clientId=u2d366119-1efe-4&from=paste&height=415&id=u892299a1&originHeight=622&originWidth=2517&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83223&status=done&style=none&taskId=u9d62151b-0543-4641-9ffd-f1ae3c17614&title=&width=1678)

2. 新增一个json提取器

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670499973847-e49a7126-d243-453a-b3cf-c19983290c7f.png#averageHue=%233d4145&clientId=u383144c0-174f-4&from=paste&height=449&id=u19c55a44&originHeight=673&originWidth=1084&originalType=binary&ratio=1&rotation=0&showTitle=false&size=92166&status=done&style=none&taskId=u7af5c392-3ba3-4f79-9468-4262b4357cf&title=&width=722.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500423664-de3aea68-a400-4bfd-a249-c3cd8e26f8a2.png#averageHue=%233f4245&clientId=u2d366119-1efe-4&from=paste&height=374&id=u8f87ddcb&originHeight=561&originWidth=2534&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108478&status=done&style=none&taskId=ufa3853d9-777c-45da-87b7-d24fe358858&title=&width=1689.3333333333333)

2. 添加一个调式取样器

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500132535-6df748a2-0446-4152-af20-46b9f819fd4c.png#averageHue=%233d4145&clientId=u2d366119-1efe-4&from=paste&height=632&id=uf606b978&originHeight=948&originWidth=1057&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102889&status=done&style=none&taskId=ue55dcbf1-0dd7-4203-a115-93d2b0d1c63&title=&width=704.6666666666666)

3. 运行查看接口返回和调式取样器的结果

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500451151-1cf32c7b-3b35-437f-81e1-a697f8a3f73a.png#averageHue=%233d4143&clientId=u2d366119-1efe-4&from=paste&height=468&id=u51e46689&originHeight=702&originWidth=1933&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78257&status=done&style=none&taskId=u4d6dcdaf-6002-4b41-9709-f22fed9fb99&title=&width=1288.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500461515-df564d98-f78e-4cc0-bfbe-5f24c5fc0536.png#averageHue=%233d4043&clientId=u2d366119-1efe-4&from=paste&height=699&id=u5e501464&originHeight=1048&originWidth=1935&originalType=binary&ratio=1&rotation=0&showTitle=false&size=109918&status=done&style=none&taskId=u409c917c-b5d9-4a67-a6e2-64766762fa2&title=&width=1290)
# 4. 跨越线程组传值
我的定义的变量一般只能在当前线程组中使用。不同线程组之间变量无法引用。那我们要怎么解决？
还是上面从获取https://www.itcast.cn/网页的title字段作为www.baidu.com的查询条件的例子。
我们把[https://www.itcast.cn/](https://www.itcast.cn/)的请求和www.baidu.com的请求放在不同线程组

1. 构建请求

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670501168056-1014c126-2535-4cc0-bf73-497cf727d6ef.png#averageHue=%233d4144&clientId=ud89a925d-58de-4&from=paste&height=518&id=ua2608e37&originHeight=777&originWidth=2534&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90698&status=done&style=none&taskId=ue288c77b-24b9-4073-bd7a-ec5503f2ab7&title=&width=1689.3333333333333)

2. 使用函数助手给全局设置一个变量

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670501383643-8a8ed8c1-0fff-410e-a605-dcc88baae24c.png#averageHue=%233d4042&clientId=ud89a925d-58de-4&from=paste&height=778&id=u89a3ede4&originHeight=1167&originWidth=2197&originalType=binary&ratio=1&rotation=0&showTitle=false&size=157602&status=done&style=none&taskId=u2d2f3fe9-04e8-42d3-a627-baac38eacc6&title=&width=1464.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670501429005-2eb3656a-947e-452d-9138-a333af5082b1.png#averageHue=%233d4145&clientId=ud89a925d-58de-4&from=paste&height=550&id=uddbe86de&originHeight=825&originWidth=1404&originalType=binary&ratio=1&rotation=0&showTitle=false&size=119186&status=done&style=none&taskId=ua4657e18-1b97-4dc9-a57f-0e6ff40b821&title=&width=936)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670502063193-4c2e88de-9eee-44a5-b080-ac9f2ff16baf.png#averageHue=%233f4345&clientId=ud89a925d-58de-4&from=paste&height=490&id=u0ff50c2b&originHeight=735&originWidth=1812&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64379&status=done&style=none&taskId=u35952cc9-6732-4d78-a009-295d8dca17b&title=&width=1208)

3. 有了全局变量Gval我们怎么获取呢？还是使用函数助手生成获取全局变量的表达式

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670502189171-8e1f019a-1471-4bbb-a084-7a9deb2a80d7.png#averageHue=%233d4143&clientId=ud89a925d-58de-4&from=paste&height=686&id=u1b391677&originHeight=1029&originWidth=2084&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125692&status=done&style=none&taskId=u64b58779-ac1c-418b-9a53-bef0e6fca56&title=&width=1389.3333333333333)

4. 给百度请求设置好变量

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670502239528-75d46cfa-205c-46b6-b4bc-f6f7d8bcac2c.png#averageHue=%233d4043&clientId=ud89a925d-58de-4&from=paste&height=625&id=ube2ad00d&originHeight=937&originWidth=2445&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101619&status=done&style=none&taskId=u2e598c24-179f-466e-97b4-8370178d0f7&title=&width=1630)

5. 为了保证每个线程组顺序执行，记得勾选独立运行每个线程组

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670501742776-f04c8372-6986-4b1f-b683-9877ca98d4f0.png#averageHue=%233c3f42&clientId=ud89a925d-58de-4&from=paste&height=900&id=u34260d95&originHeight=1350&originWidth=2240&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99435&status=done&style=none&taskId=u674c242f-b645-4c9f-9afe-daa6fe89ca6&title=&width=1493.3333333333333)

6. 选中查看结果树，运行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670502379996-8d9a766f-475a-4d6d-afeb-c25e40129db7.png#averageHue=%233d4144&clientId=ud89a925d-58de-4&from=paste&height=648&id=u5e03a669&originHeight=972&originWidth=949&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71989&status=done&style=none&taskId=u5ec9d642-8fec-4623-a4d9-a93b7f5ee6f&title=&width=632.6666666666666)
# 5. 性能测试
模拟各种正常的，峰值的测试环境，检测程序的各项性能指标是否达标
## 1. 高并发
要求： 同一时刻有100个人去访问同一个接口。统计高并发情况下平均响应时间和错误率

1. 添加请求

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503645805-ee608e6e-e751-4dc1-a46c-3e0e15a6e419.png#averageHue=%233d4143&clientId=ufdbdc1be-4e7a-4&from=paste&height=491&id=u128dd92d&originHeight=736&originWidth=2551&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63054&status=done&style=none&taskId=ua4e3120e-312d-41cc-bfdd-43b251c08b1&title=&width=1700.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503676483-cb45bdc3-9e2e-47e3-a365-9ffbe16cbdcb.png#averageHue=%233e4244&clientId=ufdbdc1be-4e7a-4&from=paste&height=433&id=u60fb3628&originHeight=650&originWidth=2521&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79825&status=done&style=none&taskId=u3ecadcb3-c055-42f8-984c-c4fe065e77e&title=&width=1680.6666666666667)

2. 为请求添加同步器

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503729207-9d37cc57-130e-4dc2-bb7d-84492dbb603e.png#averageHue=%233d4246&clientId=ufdbdc1be-4e7a-4&from=paste&height=502&id=uf3434485&originHeight=753&originWidth=1212&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108444&status=done&style=none&taskId=u0c08ddbb-2574-403e-aa36-8293f72a1aa&title=&width=808)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503922000-8e79b0b3-4c08-44a0-9103-32ecede76b4f.png#averageHue=%233c4042&clientId=ufdbdc1be-4e7a-4&from=paste&height=585&id=ud953e781&originHeight=877&originWidth=2554&originalType=binary&ratio=1&rotation=0&showTitle=false&size=96764&status=done&style=none&taskId=ub8eb65a2-745c-466e-bbb5-58d5f04b867&title=&width=1702.6666666666667)

3. 添加聚合报告

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504006573-3664ac78-6cf8-4e9d-a4fc-e0b5f7f393d5.png#averageHue=%233d4246&clientId=ufdbdc1be-4e7a-4&from=paste&height=515&id=ue9f27670&originHeight=773&originWidth=966&originalType=binary&ratio=1&rotation=0&showTitle=false&size=96107&status=done&style=none&taskId=u53bef239-8cc3-4b34-af29-e5ba81a22d6&title=&width=644)

4. 选中聚合报告，运行，查看结果

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504133151-eb60e3ad-9600-4450-b823-efc0b80bb0fb.png#averageHue=%233d4043&clientId=ufdbdc1be-4e7a-4&from=paste&height=675&id=uda86c577&originHeight=1012&originWidth=2550&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94230&status=done&style=none&taskId=ud7b0c334-5791-46b5-bc88-d6c9b650002&title=&width=1700)
## 2. 高频率
QPS: query per seconds 每秒查询数，每秒访问多少次服务器
要求： 一个用户以20QPS的频率访问一个接口，持续15秒，统计服务器平均响应时间

1. 添加一个常数吞吐量定时器

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504669539-b7b15ae5-e5a9-46a0-b662-3bc2864c117c.png#averageHue=%233d4246&clientId=ufdbdc1be-4e7a-4&from=paste&height=463&id=u3016589c&originHeight=694&originWidth=1101&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103984&status=done&style=none&taskId=ua4efa5d5-fd19-44f4-87f9-744a2ae417f&title=&width=734)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504735640-8821a20f-f88e-43a7-840e-5c4e13e18775.png#averageHue=%233c4042&clientId=ufdbdc1be-4e7a-4&from=paste&height=585&id=u6893301e&originHeight=877&originWidth=2408&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87619&status=done&style=none&taskId=u2bd3065d-6852-4662-bdc5-c81cbd8cd1d&title=&width=1605.3333333333333)

2. 设置线程组

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504793019-5f02dad1-e99f-44ed-81e0-c896969a7ea5.png#averageHue=%233d4043&clientId=ufdbdc1be-4e7a-4&from=paste&height=647&id=u87454fbd&originHeight=971&originWidth=2548&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107362&status=done&style=none&taskId=u99626c19-b70f-4ec7-8adf-fb0fe9f5789&title=&width=1698.6666666666667)
3.运行查看结果
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504967666-17e5c2b6-6ab2-474a-ada8-2f449c45c760.png#averageHue=%233e4245&clientId=ufdbdc1be-4e7a-4&from=paste&height=333&id=u8777fa2e&originHeight=500&originWidth=2535&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65618&status=done&style=none&taskId=u0899d1af-5bc4-485e-9831-eccbdefcaa3&title=&width=1690)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670505024213-5ea7b189-1efd-4948-9c16-83a2ad585492.png#averageHue=%233d4042&clientId=ufdbdc1be-4e7a-4&from=paste&height=635&id=u00335730&originHeight=952&originWidth=2560&originalType=binary&ratio=1&rotation=0&showTitle=false&size=138619&status=done&style=none&taskId=ub917abe8-9cd5-4d6c-bcac-639431c18d6&title=&width=1706.6666666666667)
# 6. 分布式
单台机器模拟的资源有限。我们可以多台机器协作，以集群的方式完成测试任务。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670505606603-b5770566-41f2-4bad-bf81-5b2911a0008a.png#averageHue=%23f3ece8&clientId=ucc16d8dd-4f28-4&from=paste&height=165&id=u2d548ffe&originHeight=248&originWidth=580&originalType=binary&ratio=1&rotation=0&showTitle=false&size=120927&status=done&style=none&taskId=u573266fb-1e16-453a-830f-08276607126&title=&width=386.6666666666667)

1. 修改控制机的配置文件

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670505639751-b7359e22-6c46-4f72-8097-6e14e30b19c9.png#averageHue=%23fcfbfb&clientId=ucc16d8dd-4f28-4&from=paste&height=133&id=ua28de1d1&originHeight=200&originWidth=1312&originalType=binary&ratio=1&rotation=0&showTitle=false&size=104579&status=done&style=none&taskId=u3bc58f93-a81c-48e5-be5e-d7269ba55e5&title=&width=874.6666666666666)

2. 控制机和远程机都打开远程访问相关属性

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670505707049-41226826-cabd-4b82-afca-f82984ce2b0b.png#averageHue=%23fbfaf9&clientId=ucc16d8dd-4f28-4&from=paste&height=159&id=u298fc135&originHeight=239&originWidth=1472&originalType=binary&ratio=1&rotation=0&showTitle=false&size=139700&status=done&style=none&taskId=ude14c107-53c5-42b7-9521-b858a65b116&title=&width=981.3333333333334)

3. 点击jmter.bat。启动所有的执行机
4. 控制机点击运行所有

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670505807843-e04b23f6-60a4-4470-83b1-0e0d389f109e.png#averageHue=%233c4146&clientId=ucc16d8dd-4f28-4&from=paste&height=409&id=u0b354bf3&originHeight=614&originWidth=615&originalType=binary&ratio=1&rotation=0&showTitle=false&size=56999&status=done&style=none&taskId=ud19d8924-64c0-40ee-8db1-d78e0da3583&title=&width=410)

# 7. 生成图形化报告
在Jmeter中可以以图形化（饼状图，柱状图。。。）的方式显示脚本运行结果，较于之前的聚合报告或者查看结果树更直接美观
生成图形化报告命令：
> jmeter -n -t jmx脚本文件 -l 日志文件 -e -o 目录

```dockerfile
-n 无图形化运行
-t 被运行的脚本
-l 将日志写到哪里
-e 生成测试报告
-o 输出到制定目录
```

1. 我们拿这个做个测试

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670506409432-6907a883-06a5-4af4-b6a5-695215b440cd.png#averageHue=%233d4144&clientId=ucc16d8dd-4f28-4&from=paste&height=485&id=uf3e9f40b&originHeight=727&originWidth=2552&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87193&status=done&style=none&taskId=ucc91c2a9-af20-40a9-9e64-51a6e241250&title=&width=1701.3333333333333)

2. 在jmter的bin目录执行一下命令
> jmeter -n -t D:\文档\jmeter\聚合报告.jmx -l D:\文档\jmeter\test.log -e -o D:\文档\jmeter\res

3. 查看生成的结果

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670506632590-f8cfeb6f-44f7-4e60-bbae-c485f48c9a17.png#averageHue=%23fdfcfc&clientId=ucc16d8dd-4f28-4&from=paste&height=259&id=ue99b308b&originHeight=388&originWidth=1326&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25502&status=done&style=none&taskId=u040b7d21-5745-4f50-a64c-1dbe9c7c577&title=&width=884)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670506642752-862c3228-13ed-475c-93f8-647a89c731db.png#averageHue=%23f9f9f9&clientId=ucc16d8dd-4f28-4&from=paste&height=901&id=u2a93b4bb&originHeight=1352&originWidth=2557&originalType=binary&ratio=1&rotation=0&showTitle=false&size=154365&status=done&style=none&taskId=ub334d4be-9ad4-4171-9012-122570185d2&title=&width=1704.6666666666667)

