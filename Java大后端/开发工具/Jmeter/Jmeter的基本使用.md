# 1. Jemter的下载和启动
**下载网址：**
[https://jmeter.apache.org/download_jmeter.cgi](https://jmeter.apache.org/download_jmeter.cgi)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327072301-6f1e4bcd-9a01-451d-ad9d-0ded94c05d95.png#averageHue=%23f7f4f1&clientId=uf3070d31-43b8-4&from=paste&height=773&id=u8088738f&originHeight=1159&originWidth=1570&originalType=binary&ratio=1&rotation=0&showTitle=false&size=181898&status=done&style=none&taskId=ucb1704d8-89f1-4298-b5a9-2abf92dfaa9&title=&width=1046.6666666666667)
**解压使用：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327373758-55a652ad-628f-44ac-bebd-1a00abf28ce8.png#averageHue=%23fbfaf9&clientId=uf3070d31-43b8-4&from=paste&height=900&id=uafd23812&originHeight=1350&originWidth=1066&originalType=binary&ratio=1&rotation=0&showTitle=false&size=123655&status=done&style=none&taskId=u1dc7b610-0028-42a0-b32c-559255249c0&title=&width=710.6666666666666)

1. 打开bin下面的jmeter.properties中配置。修改sampleresult.default.encoding的值为UTF-8
> sampleresult.default.encoding=UTF-8

2. 双击  **ApacheMeter.jar ** 启动
# 2. 开始使用

1. 测试计划--右键--线程--添加线程组

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327753651-3bedf79c-05c5-45f9-b504-2f346cb0053a.png#averageHue=%233d4248&clientId=uf3070d31-43b8-4&from=paste&height=323&id=u71b5378b&originHeight=485&originWidth=1170&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64542&status=done&style=none&taskId=uc1be6576-4cfe-4038-a6f3-b4849939ba3&title=&width=780)

2. 线程组--右键--取样器--http请求

这里以请求百度为例，内容根据需要填上对应的值
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327861179-71d23b95-563e-4195-a306-b81139fa8ae6.png#averageHue=%233e4144&clientId=uf3070d31-43b8-4&from=paste&height=466&id=u79f1cc5f&originHeight=699&originWidth=2000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80716&status=done&style=none&taskId=u8e705e41-76e2-4ce9-8715-a009d3d7655&title=&width=1333.3333333333333)

3. 点中测试计划--右键--添加监听器--查看结果树

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327964763-e2e31b65-115d-4c35-954f-1ec39df77eec.png#averageHue=%233d4348&clientId=uf3070d31-43b8-4&from=paste&height=369&id=u17e14576&originHeight=554&originWidth=890&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75881&status=done&style=none&taskId=ue55bad56-b0e7-487a-accf-1d7e9fa8fdd&title=&width=593.3333333333334)

4. 点中查看结果树，运行。

如果接口返回json格式化结果那里可以选择json
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670328150374-8b051edd-f4da-4bd8-914b-98cb4f137665.png#averageHue=%233c4043&clientId=uf3070d31-43b8-4&from=paste&height=689&id=u5aaae3d3&originHeight=1034&originWidth=1945&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129056&status=done&style=none&taskId=u2ebafce9-bcd2-4a62-9d19-74a12b9a50f&title=&width=1296.6666666666667)

5. 将测试计划保存下来，方便下次使用

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329204646-a9b5e119-48b6-4e4c-a8f4-2ce8184f29b1.png#averageHue=%233d4144&clientId=uf3070d31-43b8-4&from=paste&height=468&id=ub10e4f31&originHeight=702&originWidth=1794&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84567&status=done&style=none&taskId=uda5e11a6-279a-446c-ad2e-657bd4565c7&title=&width=1196)
# 3. 概念性理解
**测试计划，线程组，http请求，查看结果树**

1. 测试计划可以看做一个项目，
2. 项目下面有多个模块（线程组），
3. 模块下面有好多接口（http请求）。
4. 查看结果数就是接口返回的结果。
# 4. 查看结果树的额外说明

1. 点中Http请求，ctrl+c。 再点中线程组 ctrl+v，复制一个Http请求（改个名字改成Http请求2）

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329085789-bbcf8488-c81f-476e-9f95-1441aacbec71.png#averageHue=%233e4245&clientId=uf3070d31-43b8-4&from=paste&height=423&id=ubeb0f11e&originHeight=635&originWidth=2132&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78872&status=done&style=none&taskId=u4f9878ba-336e-4423-8585-a66b275a0b4&title=&width=1421.3333333333333)

2. 点中线程组，ctrl+c 。再点中执行计划 ctrl+v，复制出一个线程组2

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329263238-89f948eb-8439-42a1-91f3-f283b4a0e225.png#averageHue=%233d4144&clientId=uf3070d31-43b8-4&from=paste&height=533&id=u4d059e7a&originHeight=799&originWidth=1912&originalType=binary&ratio=1&rotation=0&showTitle=false&size=92273&status=done&style=none&taskId=ub848d822-fac0-456e-b120-0941b61506c&title=&width=1274.6666666666667)

3. 现在  查看结果树，线程组，线程组2平级。都是在测试计划下面。

 选中查看结果树点击运行按钮。我们可以看到四个请求全部发送
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329542239-256d87f4-5491-4c9f-be55-8b48d485fdbb.png#averageHue=%233e4144&clientId=uf3070d31-43b8-4&from=paste&height=491&id=ube26143e&originHeight=737&originWidth=1822&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95282&status=done&style=none&taskId=u688369d8-98ec-4a15-93fb-284f1120d38&title=&width=1214.6666666666667)

4. 把查看结果数拖到线程组内，再次点击运行，只会查看当前线程组内的请求结果

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329600284-70788025-ac4b-4fca-b6d4-d6cea46e622e.png#averageHue=%233d4043&clientId=uf3070d31-43b8-4&from=paste&height=521&id=ue9ba1dde&originHeight=781&originWidth=1664&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82945&status=done&style=none&taskId=u09d12f79-4f54-4b15-9335-1cc6533573a&title=&width=1109.3333333333333)
# 5. 并发和顺序执行
**并发执行**：多个线程组同时执行（默认情况）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670330035306-3b8cf8f5-bb35-4212-bffb-b3f936d448ef.png#averageHue=%233e4144&clientId=uf3070d31-43b8-4&from=paste&height=462&id=u0aaa0cdb&originHeight=693&originWidth=1901&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88531&status=done&style=none&taskId=u0ce3c05d-cdbc-4730-a729-8a0d73be39b&title=&width=1267.3333333333333)
可以看出不同线程组下面是没有顺序的。

**顺序执行：**多个线程组顺序执行
在测试计划下勾上独立运行每个线程组
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670330167304-c58eb502-fd37-475a-a8ad-e2268db71e38.png#averageHue=%233c3f42&clientId=uf3070d31-43b8-4&from=paste&height=929&id=u38947ddc&originHeight=1393&originWidth=2110&originalType=binary&ratio=1&rotation=0&showTitle=false&size=116694&status=done&style=none&taskId=ud903c011-07f0-4d31-ad77-1a6606ff60e&title=&width=1406.6666666666667)
再次运行就能看到请求是有顺序的
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670330202097-f03b989f-b0c1-4d91-9c35-4cf62d60f36d.png#averageHue=%233d4144&clientId=uf3070d31-43b8-4&from=paste&height=535&id=ub52575c6&originHeight=803&originWidth=1696&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89078&status=done&style=none&taskId=u2d53898e-2ff6-4dc1-8037-db166188727&title=&width=1130.6666666666667)
# 5. 优先和最后执行的线程组
点击测试计划新增两个线程组
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670331278347-8aa212dd-6839-4471-b0f8-5cccd5418f8a.png#averageHue=%233d4246&clientId=uf3070d31-43b8-4&from=paste&height=341&id=ub4855423&originHeight=512&originWidth=1164&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76621&status=done&style=none&taskId=uc5986740-78e2-450f-b346-33d5cee8ec7&title=&width=776)
然后执行
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670331318597-2b8a9d66-566a-45e0-a2c9-827e9adc53f6.png#averageHue=%233d4144&clientId=uf3070d31-43b8-4&from=paste&height=571&id=uef57b33e&originHeight=856&originWidth=2040&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102356&status=done&style=none&taskId=u0a2ecace-1784-499d-b904-a1e2df51fc2&title=&width=1360)
可以看出**setUp 线程组**的优先级比较高会先执行。
**tearDown** 线程组的优先级比较低会最后执行。

**疑问：如果有两个setUp或者两个tearDown哪个优先级高？**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670331627092-50264d34-45f2-4631-8061-0e93c49c0d5c.png#averageHue=%233d4144&clientId=uf3070d31-43b8-4&from=paste&height=575&id=uad7bee8d&originHeight=863&originWidth=1688&originalType=binary&ratio=1&rotation=0&showTitle=false&size=147704&status=done&style=none&taskId=u9efc2339-d79b-4952-831c-f8d8f4e5db4&title=&width=1125.3333333333333)
**结论：哪个线程组在上面，哪个就是爸爸。优先高**
# 6. 线程组常用的属性
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670409844537-143ccbc6-fa9d-452f-ab68-9490e5722776.png#averageHue=%23eeedec&clientId=u07dcf2fe-fe1b-4&from=paste&height=467&id=ub64d3581&originHeight=700&originWidth=1644&originalType=binary&ratio=1&rotation=0&showTitle=false&size=445315&status=done&style=none&taskId=u734fd854-51aa-43a2-a280-ab3e56f9655&title=&width=1096)
# 7. HTTP请求默认值
对于多个接口url而言，可能就是请求路径部分不同。而请求协议，请求的服务器名或ip，端口都一样。这样我就可以吧共同的部分抽取出来。在jmeter中http请求默认值可能做到这一点。

1. 新增一个Http请求默认值

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475413505-631731c9-3559-441f-b6e9-f84fa736311d.png#averageHue=%233e4247&clientId=u02933f9d-49e9-4&from=paste&height=385&id=u92604883&originHeight=578&originWidth=1200&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91076&status=done&style=none&taskId=u5e7bbfda-6310-47b4-ba73-24a599f73d6&title=&width=800)

2. 把共同的部分抽取出来

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475477969-15222f8d-c668-401b-823b-dce0dc5492e8.png#averageHue=%233d4042&clientId=u02933f9d-49e9-4&from=paste&height=702&id=u3a572b7e&originHeight=1053&originWidth=2023&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85832&status=done&style=none&taskId=u49e03467-bf3d-4fc9-a798-93d5a4cb27a&title=&width=1348.6666666666667)

3. http请求中只要写路径

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475526502-ba586545-413e-4bbe-a0ba-cb99d41c8d94.png#averageHue=%233d4143&clientId=u02933f9d-49e9-4&from=paste&height=622&id=ucc000c09&originHeight=933&originWidth=1958&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81498&status=done&style=none&taskId=ufaa50604-d5b4-4a3c-a1a2-b7513555037&title=&width=1305.3333333333333)
# 8. Http信息头管理器
对于一些新增或者修改接口。我们需要提交一些json数据，如果没添加请求头可能会报一些错误。
如下：新增一个用户，接口报错
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476080708-c66e4a73-4294-4b1a-a659-7b117cb5c7a2.png#averageHue=%233e4244&clientId=u02933f9d-49e9-4&from=paste&height=473&id=ufc8e594a&originHeight=710&originWidth=2502&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80066&status=done&style=none&taskId=u298d968a-fa9a-4a13-9f5e-e275ac58a9f&title=&width=1668)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476112137-e3aeee63-b434-4fd3-9373-165692ee263b.png#averageHue=%233c4042&clientId=u02933f9d-49e9-4&from=paste&height=529&id=ua4f02a7e&originHeight=794&originWidth=2514&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89999&status=done&style=none&taskId=u6a4ed4cc-65a2-4dff-b987-16def6afe1e&title=&width=1676)
这时我们需要用**http信息头管理器**添加请求头解决
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475936372-4806c7e6-224d-463e-b3d5-6e153a97e78c.png#averageHue=%233d4145&clientId=u02933f9d-49e9-4&from=paste&height=533&id=u3ddf3f98&originHeight=800&originWidth=1384&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108001&status=done&style=none&taskId=u1e6d8962-a853-41ff-bca9-f5c52429e39&title=&width=922.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476317833-17c66dc3-0068-481e-9406-f07f9feeaa20.png#averageHue=%233d4144&clientId=u02933f9d-49e9-4&from=paste&height=446&id=u36e89fe3&originHeight=669&originWidth=1986&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71222&status=done&style=none&taskId=u271ac4a9-e9cc-4a00-8c01-e592373dd7b&title=&width=1324)
# 9. 参数化
## 1. 用户定义的变量
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476856016-4fe318c8-a9e2-42cf-a909-c60b0d1a2727.png#averageHue=%233d4246&clientId=u02933f9d-49e9-4&from=paste&height=536&id=ud0ac8331&originHeight=804&originWidth=985&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100946&status=done&style=none&taskId=uc4771492-0255-4f80-bd3d-f802dcc1bc0&title=&width=656.6666666666666)
新增一个addUser的变量。值为接口路径（/user）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476975036-03e7e4cb-0ec4-400f-9fd6-d76b75564b65.png#averageHue=%233b4043&clientId=u02933f9d-49e9-4&from=paste&height=441&id=u1c781250&originHeight=661&originWidth=1987&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75158&status=done&style=none&taskId=u1c2a8ff2-1396-4850-b4b2-b219ee15928&title=&width=1324.6666666666667)
在请求中引用变量即可
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476994370-c47c1e97-0307-4190-aae1-4cd5419915c0.png#averageHue=%233f4345&clientId=u02933f9d-49e9-4&from=paste&height=444&id=u86c3c681&originHeight=666&originWidth=2002&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87632&status=done&style=none&taskId=uf661d85e-08a5-49c1-a6c3-4bfe57066ff&title=&width=1334.6666666666667)
## 2. csv批量操作
假设我想新增多个user，我们可以用csv进行批量添加。

1. 我们先用Excel写好三个用户并另存为csv文件。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670477646670-bab95372-ce09-4a80-a763-84ad18626be0.png#averageHue=%23f9f9f9&clientId=u02933f9d-49e9-4&from=paste&height=471&id=ua56d41ec&originHeight=706&originWidth=1161&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46144&status=done&style=none&taskId=u899896b4-b4e2-4d1b-a0c5-629b9052fff&title=&width=774)

2. 新增csv数据文件设置

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670477846456-e8458970-0f2f-4d88-9031-cd3228a18865.png#averageHue=%233d4247&clientId=u02933f9d-49e9-4&from=paste&height=446&id=ue5e7a2de&originHeight=669&originWidth=1075&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101339&status=done&style=none&taskId=u00e110ea-0f89-454a-bb68-0efabdab286&title=&width=716.6666666666666)

3. 我们的csv中有两列。我们在变量名称那一行定义好变量名用英文逗号分割

**变量名name,age对应csv中的姓名和年龄**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478113191-c85287b9-1514-4d3a-88d6-eb1efbe78f90.png#averageHue=%233f4245&clientId=u02933f9d-49e9-4&from=paste&height=411&id=u8e71d47a&originHeight=616&originWidth=1950&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64037&status=done&style=none&taskId=u87047c7c-4b12-4c72-bea7-c6aa2b7c8ac&title=&width=1300)

4. 在请求中引用name和age变量

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478357962-9e921d78-798e-4545-bcea-cb049099dbcd.png#averageHue=%233f4244&clientId=u02933f9d-49e9-4&from=paste&height=584&id=uf9284062&originHeight=876&originWidth=2193&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97439&status=done&style=none&taskId=u92d202f8-0435-40c5-b33b-0912f4b13ee&title=&width=1462)

5. 选中查看结果树，点击运行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478532770-cd43c8e2-663e-44a6-9504-e7276beac06f.png#averageHue=%233f4245&clientId=u02933f9d-49e9-4&from=paste&height=629&id=u2c58a83c&originHeight=943&originWidth=2178&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103881&status=done&style=none&taskId=u7eb71d0b-ca42-455e-a2f1-521719cc90a&title=&width=1452)
我们发现只是新增了一个。还有的数据没有新增。是因为我们还需要做一项设置，把线程组的永远勾上。就会循环添加所有用户了。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478589525-0e3b4260-667c-4f4c-8d2b-47a00b1b1eaf.png#averageHue=%233c4042&clientId=u02933f9d-49e9-4&from=paste&height=575&id=u61a55a67&originHeight=862&originWidth=1544&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89210&status=done&style=none&taskId=ua9cb9615-96df-47f1-94c0-dca7418873a&title=&width=1029.3333333333333)

6. 再次运行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478638434-44ba5cd1-527e-4138-8f45-97da38323da2.png#averageHue=%233e4245&clientId=u02933f9d-49e9-4&from=paste&height=520&id=u74487a9d&originHeight=780&originWidth=1185&originalType=binary&ratio=1&rotation=0&showTitle=false&size=82709&status=done&style=none&taskId=u4d3212e2-75ab-4b75-9c9f-09329df535d&title=&width=790)
## 3. 用户参数

1. 在请求上添加用户参数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484176847-7373e5c8-9a4e-442d-9cb5-a6fdc64678d6.png#averageHue=%233d4347&clientId=u02933f9d-49e9-4&from=paste&height=405&id=u4feedf73&originHeight=608&originWidth=1156&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93713&status=done&style=none&taskId=u12e78578-8f62-47bd-a204-6f494e38b8c&title=&width=770.6666666666666)

2. 添加两个用户

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484497405-709980e4-e6b4-4e6c-a3c2-92e3404e7f6d.png#averageHue=%233c4042&clientId=u02933f9d-49e9-4&from=paste&height=989&id=uebca458f&originHeight=1483&originWidth=2539&originalType=binary&ratio=1&rotation=0&showTitle=false&size=128975&status=done&style=none&taskId=ufcdf3467-4fa7-4ce7-92e9-f5478b15d56&title=&width=1692.6666666666667)

3. 点击线程组，把线程数调整为2，因为我们这里模拟的是两个用户操作

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484688644-d96f0419-99e6-476a-89ae-afc74d992833.png#averageHue=%233d4042&clientId=u02933f9d-49e9-4&from=paste&height=732&id=ub843a063&originHeight=1098&originWidth=2517&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105839&status=done&style=none&taskId=u89d5b57c-e9ff-4e4a-bfbc-052ad7b2ecd&title=&width=1678)

4. 点击查看结果树，运行

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484719423-d258940e-5440-49c1-8067-f2d4f6554991.png#averageHue=%233f4345&clientId=u02933f9d-49e9-4&from=paste&height=611&id=ua47edd71&originHeight=916&originWidth=2413&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94773&status=done&style=none&taskId=u05749b08-3c39-403d-a5e5-ac58fe0ece2&title=&width=1608.6666666666667)
## 4. 计数器函数

1. 我们定义一个请求百度的http请求，并给线程组设置如下

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485333663-dcbe80bc-8d15-4563-93d1-7336e497ae1f.png#averageHue=%233d4143&clientId=uc77fba16-b259-4&from=paste&height=591&id=u2d5817c3&originHeight=887&originWidth=2557&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102006&status=done&style=none&taskId=u68f01845-69ff-464f-825a-fa2a5488787&title=&width=1704.6666666666667)

2. 执行后我们可以看到发送了6次请求 （2 * 3 ）

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485371457-af7f967e-15c8-44b5-a7c2-0722ed08d02c.png#averageHue=%233d4043&clientId=uc77fba16-b259-4&from=paste&height=656&id=u38e55ae5&originHeight=984&originWidth=2081&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90612&status=done&style=none&taskId=u4a14ecfd-c49e-45d7-bc6b-9efe40903ec&title=&width=1387.3333333333333)

3. 这时我们想统计总共发了几次请求。就需要加一个计数器函数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485578405-082d81bf-fbf1-4258-bc90-bb8dbcafd6ba.png#averageHue=%233d4143&clientId=uc77fba16-b259-4&from=paste&height=755&id=u3e4acf56&originHeight=1132&originWidth=1857&originalType=binary&ratio=1&rotation=0&showTitle=false&size=144211&status=done&style=none&taskId=udc30f75e-9754-43de-aec0-1ab1764db3e&title=&width=1238)

4. 使用复制的变量

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485647490-da517dae-d9d9-4247-b9ff-8d85e6d9aa02.png#averageHue=%233d4143&clientId=uc77fba16-b259-4&from=paste&height=593&id=u0344dd67&originHeight=890&originWidth=1977&originalType=binary&ratio=1&rotation=0&showTitle=false&size=61895&status=done&style=none&taskId=u969c67e4-b0d2-48a3-98fd-72cf3edef41&title=&width=1318)

5. 可以看到有2个用户。每个用户单独统计

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485656792-33e22b05-1649-4b64-95ed-0c204a177966.png#averageHue=%233d4043&clientId=uc77fba16-b259-4&from=paste&height=685&id=ub048b8f2&originHeight=1027&originWidth=2320&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90804&status=done&style=none&taskId=u576c3800-c61c-4a22-a9ee-03a6061f7a8&title=&width=1546.6666666666667)

6. 如果把变量中的true改成false 则每个请求都会计数一次，可以看到一共发送了6个请求

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485722236-79504222-f1d9-45f1-b88b-d3327a8402ff.png#averageHue=%233d4144&clientId=uc77fba16-b259-4&from=paste&height=521&id=u653fb41c&originHeight=782&originWidth=1631&originalType=binary&ratio=1&rotation=0&showTitle=false&size=68966&status=done&style=none&taskId=ud6f70168-ff07-4158-8b20-2fd54bc7329&title=&width=1087.3333333333333)
## 5. 随机数函数

1. 复制出一个请求。然后将禁用上一个计数器函数的请求

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486059846-5ff7f61e-e0a5-47a0-8d38-6f7377a04b0d.png#averageHue=%233d4043&clientId=uc77fba16-b259-4&from=paste&height=517&id=u1df0c509&originHeight=775&originWidth=1576&originalType=binary&ratio=1&rotation=0&showTitle=false&size=98849&status=done&style=none&taskId=ue2e4d885-a4e0-46cf-8724-86e03e63412&title=&width=1050.6666666666667)

2. 选择随机数函数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486123083-57e58fe8-c4f4-4fe3-af4d-5ff7b0dce627.png#averageHue=%233d4042&clientId=uc77fba16-b259-4&from=paste&height=836&id=ud6df82de&originHeight=1254&originWidth=2115&originalType=binary&ratio=1&rotation=0&showTitle=false&size=135687&status=done&style=none&taskId=u8f4d2783-ab79-48f8-97cf-32842a405f9&title=&width=1410)

3. 在参数位置使用随机函数可以生成随机数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486467740-4e9cfc5e-0fc5-4127-99c1-499dd4b8fbac.png#averageHue=%23404446&clientId=uc77fba16-b259-4&from=paste&height=687&id=u49898a16&originHeight=1030&originWidth=2437&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87850&status=done&style=none&taskId=u318f3258-e476-4d2b-aaf1-51bf7528705&title=&width=1624.6666666666667)

4. 运行结果

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486515902-d4dc21f5-b996-45f9-aafb-6eb96d0c9c8c.png#averageHue=%233f4346&clientId=uc77fba16-b259-4&from=paste&height=712&id=ud19faafe&originHeight=1068&originWidth=2317&originalType=binary&ratio=1&rotation=0&showTitle=false&size=113764&status=done&style=none&taskId=ud6b6a203-431d-403d-808b-d6bb1bbd266&title=&width=1544.6666666666667)
## 6. 时间函数

1. 再复制一个请求，禁用调其他两个

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486798306-ca55cfad-e8f3-41ea-b74a-485f4fd3ff8b.png#averageHue=%233e4245&clientId=uc77fba16-b259-4&from=paste&height=347&id=ub87c8507&originHeight=520&originWidth=1892&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76801&status=done&style=none&taskId=u363632be-b273-4585-a115-592c04d8623&title=&width=1261.3333333333333)

2. 选择time函数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486789475-23d59dd9-b6bc-46e6-8f1f-c1064368c0c8.png#averageHue=%233e4244&clientId=uc77fba16-b259-4&from=paste&height=791&id=u964a9303&originHeight=1187&originWidth=2316&originalType=binary&ratio=1&rotation=0&showTitle=false&size=163578&status=done&style=none&taskId=u93f5ef51-3dbd-4520-8c0a-d6d20335b79&title=&width=1544)

3. 运行可以看到每个请求后面都带了时间，

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486886937-bb11f82d-2a6e-43f6-a714-2507c73d5d12.png#averageHue=%233f4244&clientId=uc77fba16-b259-4&from=paste&height=710&id=u925fbe66&originHeight=1065&originWidth=2203&originalType=binary&ratio=1&rotation=0&showTitle=false&size=107802&status=done&style=none&taskId=u625faf5f-5308-4cd4-91e6-91e9dd8fd29&title=&width=1468.6666666666667)

4. 当然我们也可以用这个放在请求参数的位置，来作为请求参数，这里就不演示了
# 10.JDBC请求
我们可以创建一个jdbc请求来查询数据库

1. 在测试计划里面设置我们的mysql驱动包

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670487959943-5baf0cef-8730-420e-8b93-c0ff4b268ff8.png#averageHue=%233d4144&clientId=uc77fba16-b259-4&from=paste&height=538&id=u628282b6&originHeight=807&originWidth=1945&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103863&status=done&style=none&taskId=ua81f5852-0a34-47b6-a8f3-f57577470b9&title=&width=1296.6666666666667)

2. 添加jdbc配置元件

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488148642-1b2f15e4-67d1-4181-a6e3-807958bb4898.png#averageHue=%233d4146&clientId=uc77fba16-b259-4&from=paste&height=517&id=u97dcbc20&originHeight=775&originWidth=1081&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101007&status=done&style=none&taskId=uf3e23849-06a6-4634-a3e4-4ff61bfeb0b&title=&width=720.6666666666666)

3. 配置如下：起个名字和配置一下连接信息

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488326220-921ea303-3435-4ff3-950e-7a77f184baa0.png#averageHue=%23404446&clientId=uc77fba16-b259-4&from=paste&height=981&id=u8573a5ad&originHeight=1471&originWidth=2558&originalType=binary&ratio=1&rotation=0&showTitle=false&size=123942&status=done&style=none&taskId=u2a018bc7-e3a0-40ee-bc8f-925296c218b&title=&width=1705.3333333333333)

4. 在线程组下面添加一个Jdbc请求

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488035558-a7438a9e-1bf3-48ba-9564-879f16a5fb0f.png#averageHue=%233d4146&clientId=uc77fba16-b259-4&from=paste&height=551&id=u09345ced&originHeight=826&originWidth=1065&originalType=binary&ratio=1&rotation=0&showTitle=false&size=101892&status=done&style=none&taskId=u8f251459-d339-4ef6-a7e7-fc00a69b5a2&title=&width=710)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488536392-a695ba09-dd47-442c-a28b-c1ebe3643e4b.png#averageHue=%23404446&clientId=uc77fba16-b259-4&from=paste&height=985&id=u4849fc5f&originHeight=1478&originWidth=2549&originalType=binary&ratio=1&rotation=0&showTitle=false&size=150885&status=done&style=none&taskId=ud56ea173-f60a-4d01-a290-1fd9375a393&title=&width=1699.3333333333333)

5. 执行可以看到结果

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488766179-85c95924-40a5-4ade-b738-65486ee01040.png#averageHue=%233c4042&clientId=uc77fba16-b259-4&from=paste&height=545&id=u5477f9d2&originHeight=817&originWidth=1930&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81769&status=done&style=none&taskId=u46789db3-9665-478a-aa1c-946abb3df61&title=&width=1286.6666666666667)

6. 再新建一个调式取样器

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489158524-24688dce-8496-4ef4-9bdb-ce9a85a181bb.png#averageHue=%233d434a&clientId=uc77fba16-b259-4&from=paste&height=276&id=u57eac8d2&originHeight=414&originWidth=1029&originalType=binary&ratio=1&rotation=0&showTitle=false&size=62340&status=done&style=none&taskId=u50664a88-598c-4c91-bc5a-2f0ad4bfd9f&title=&width=686)

7. 再次执行，我们就能看到之前的userName变量的用处了

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489267398-a688020a-a979-403d-8625-74e15a478a54.png#averageHue=%233c4042&clientId=uc77fba16-b259-4&from=paste&height=747&id=uc941a3c9&originHeight=1120&originWidth=2175&originalType=binary&ratio=1&rotation=0&showTitle=false&size=128568&status=done&style=none&taskId=u9c19fef1-85fe-41cf-8e8a-7ff484a8cab&title=&width=1450)

8. 我们可以拿到上一步的userName_xxx变量，请求百度搜索

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489490806-fc6199e5-225f-42ae-ae5d-3ccd7b6e9363.png#averageHue=%233d4043&clientId=uc77fba16-b259-4&from=paste&height=680&id=u39f4de83&originHeight=1020&originWidth=2522&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102743&status=done&style=none&taskId=u4196dc0c-b236-4f4d-906c-d264feb56ae&title=&width=1681.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489508988-8715d302-69a7-40f0-87ad-9ed17fb0ac80.png#averageHue=%233c4043&clientId=uc77fba16-b259-4&from=paste&height=734&id=ud508a0c9&originHeight=1101&originWidth=2259&originalType=binary&ratio=1&rotation=0&showTitle=false&size=145176&status=done&style=none&taskId=u993ad84a-25dc-40fc-9970-adf021e505d&title=&width=1506)
# 11. 断言
## 1. 响应断言
用来判断响应内容和响应状态码

1. 对请求添加一个响应断言

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670490825291-2f162669-b076-4ffb-adc3-4dccc3e44b6d.png#averageHue=%233c4247&clientId=uc77fba16-b259-4&from=paste&height=435&id=ua8529f96&originHeight=652&originWidth=919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76826&status=done&style=none&taskId=u0c9f3bc5-f40a-4ee5-8bb8-acd1f867a43&title=&width=612.6666666666666)

2. 对断言进行设置

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670490919592-fabc35ee-65f5-4fcf-bf43-28f2e5369861.png#averageHue=%233d4042&clientId=uc77fba16-b259-4&from=paste&height=940&id=u09940a07&originHeight=1410&originWidth=1943&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89821&status=done&style=none&taskId=u03edd27a-14f6-4596-a5a5-5de5d73e491&title=&width=1295.3333333333333)

3. 由于响应体里面包含张三所有没有报错

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491088482-5a9f61b8-e5e6-4cf4-9bd9-4822e6de2245.png#averageHue=%233e4245&clientId=uc77fba16-b259-4&from=paste&height=361&id=u7c872ede&originHeight=541&originWidth=1954&originalType=binary&ratio=1&rotation=0&showTitle=false&size=51516&status=done&style=none&taskId=u8c964911-4134-40e2-880e-8cd0039c733&title=&width=1302.6666666666667)

4. 我们改一下条件让响应体里面包含李四，再次执行，可以看到断言失败

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491166933-cf8f35d0-4958-4ce9-8e4f-89b450b53119.png#averageHue=%233c4043&clientId=uc77fba16-b259-4&from=paste&height=529&id=uc34e74a2&originHeight=793&originWidth=1904&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69677&status=done&style=none&taskId=u646da411-088b-473f-bce5-e7f12ffcd4b&title=&width=1269.3333333333333)
对于其他的响应类型可以自行尝试
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491211085-2e164443-de9f-4518-a50c-b3e91be31ef2.png#averageHue=%233c3f41&clientId=uc77fba16-b259-4&from=paste&height=160&id=u57299ca9&originHeight=240&originWidth=1919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26770&status=done&style=none&taskId=u7e4f34dc-1076-49f9-8ada-8006c7a7fdb&title=&width=1279.3333333333333)
## 2. 大小断言
判断响应内容的字节长度

1. 新增一个大小断言

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491441422-306b7385-328d-431c-b9aa-de977abbe04a.png#averageHue=%233c4146&clientId=uc77fba16-b259-4&from=paste&height=457&id=u488bb96f&originHeight=685&originWidth=969&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76434&status=done&style=none&taskId=u699b11d7-1be1-46ad-99ae-94d19effac6&title=&width=646)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491482847-b1db7a6d-4426-4f7a-8ec4-9a893f49e760.png#averageHue=%233c4042&clientId=uc77fba16-b259-4&from=paste&height=770&id=ubd0fe1c5&originHeight=1155&originWidth=1899&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63104&status=done&style=none&taskId=uaba14185-99cf-4702-a5c0-f73b709479e&title=&width=1266)
## 3. 断言持续时间
判断响应时间

1. 新增断言持续时间

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491520152-21bb0b11-67be-4b0b-b5b5-c541c3074cf3.png#averageHue=%233d4248&clientId=uc77fba16-b259-4&from=paste&height=366&id=ue8150877&originHeight=549&originWidth=886&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70915&status=done&style=none&taskId=udc2932df-3f8a-43d4-970a-ec87c7783ca&title=&width=590.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491666125-9f935a4b-2df8-4d7c-8024-ba17c5241d1a.png#averageHue=%233c3f41&clientId=uc77fba16-b259-4&from=paste&height=827&id=u1187bf28&originHeight=1241&originWidth=1945&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54283&status=done&style=none&taskId=u6d7e0f6d-62bb-4933-935e-182ff166c9d&title=&width=1296.6666666666667)

2. 执行报错，因为接口超过了1ms

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491709333-34a52f34-bb3f-4106-a9f8-dbbdac5fc56e.png#averageHue=%233d4043&clientId=uc77fba16-b259-4&from=paste&height=458&id=u97ab3bd5&originHeight=687&originWidth=1787&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81051&status=done&style=none&taskId=u5e33c3df-6a42-4371-86de-a7414858d49&title=&width=1191.3333333333333)

