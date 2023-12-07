# 1. 工具概述
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661867808780-f24b0fad-f14e-4f6b-9d5c-0feb0f454bbf.png#averageHue=%23c1dbc6&clientId=u039c8f1d-766e-4&from=paste&height=219&id=ud889e59f&originHeight=328&originWidth=746&originalType=binary&ratio=1&rotation=0&showTitle=false&size=250815&status=done&style=none&taskId=u85b90d77-bca3-42a2-ba2b-e5386ad30bc&title=&width=497.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661867832374-d1e7a0b1-8f90-4ac7-8410-d05a2d6418b6.png#averageHue=%23c1dbc6&clientId=u039c8f1d-766e-4&from=paste&height=320&id=u4fe2a0ef&originHeight=480&originWidth=748&originalType=binary&ratio=1&rotation=0&showTitle=false&size=331042&status=done&style=none&taskId=u1065cfea-15af-4fe9-8746-0c46964b691&title=&width=498.6666666666667)
# 2. JConsole
## 2.1 基本概述
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661868205638-6179837c-7861-40d2-beab-7b279f398855.png#averageHue=%23c5e0ca&clientId=u02314db2-cf99-4&from=paste&height=144&id=u55132515&originHeight=216&originWidth=870&originalType=binary&ratio=1&rotation=0&showTitle=false&size=128755&status=done&style=none&taskId=ub77e25b7-54fc-4c28-8f39-3ecd83a3f8e&title=&width=580)
## 2.2 启动
在jdk安装目录中找到jconsole.exe，双击该可执行文件就可以
打开DOS窗口，直接输入jconsole就可以了
## 2.3 三种连接方式

1. **Local**

使用JConsole连接一个正在本地系统运行的JVM，并且执行程序的和运行JConsole的需要是同一个用户。JConsole使用文件系统的授权通过RMI连接起链接到平台的MBean的服务器上。这种从本地连接的监控能力只有Sun的JDK具有。
注意：本地连接要求 启动jconsole的用户 和 运行当前程序的用户 是同一个用户
具体操作如下：
1、在DOS窗口中输入jconsole
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661869931936-c8d511ff-a32b-4f8e-bb28-49432f8f8072.png#averageHue=%2312100f&clientId=ubd551143-fb42-4&from=paste&height=37&id=u4d312948&originHeight=55&originWidth=321&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2816&status=done&style=none&taskId=u9851fa4d-fcfc-4fce-9be8-5af3f592ca2&title=&width=214)
2、在控制台上填写相关信息
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661869937147-0cfa3060-717c-4342-8e82-5250cbe8fc12.png#averageHue=%23aacca7&clientId=ubd551143-fb42-4&from=paste&height=491&id=ubaa37a40&originHeight=737&originWidth=878&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94720&status=done&style=none&taskId=u180bcef9-f665-4f0d-bccc-daa42eeec6b&title=&width=585.3333333333334)
3、选择“不安全的连接”
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661869945799-d7deb93c-2acf-42f8-b232-79cc65bc991f.png#averageHue=%23cfcfcf&clientId=ubd551143-fb42-4&from=paste&height=463&id=u044e6201&originHeight=694&originWidth=878&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42474&status=done&style=none&taskId=u936ead72-8c78-4cfb-8ca1-34fd5f95563&title=&width=585.3333333333334)
4、进入控制台页面
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661869956819-35d9ea3d-ec75-4165-9d3c-b9a76807af82.png#averageHue=%23f7f7f6&clientId=ubd551143-fb42-4&from=paste&height=489&id=u1f9eec50&originHeight=734&originWidth=878&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45792&status=done&style=none&taskId=uefeb7c3a-ae55-48e7-b9c5-9d7a872e137&title=&width=585.3333333333334)

2. **Remote**

使用下面的URL通过RMI连接器连接到一个JMX代理，service:jmx:rmi:///jndi/rmi://hostName:portNum/jmxrmi。JConsole为建立连接，需要在环境变量中设置mx.remote.credentials来指定用户名和密码，从而进行授权。

3. **Advanced**

使用一个特殊的URL连接JMX代理。一般情况使用自己定制的连接器而不是RMI提供的连接器来连接JMX代理，或者是一个使用JDK1.4的实现了JMX和JMX Rmote的应用
## 2.4 主要作用
1、概览
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870139085-5baf0411-316a-4c5e-9503-851e06b27446.png#averageHue=%23f9f9f9&clientId=ubd551143-fb42-4&from=paste&height=677&id=u182f9147&originHeight=1016&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81143&status=done&style=none&taskId=ufffece76-47a8-4db3-8a7e-88b5e78d597&title=&width=1280)
2、内存
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870148736-5cc5da6f-120e-4b6d-8c79-0d09c5d594e7.png#averageHue=%23f8f8f8&clientId=ubd551143-fb42-4&from=paste&height=677&id=u85c51437&originHeight=1015&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71904&status=done&style=none&taskId=u67549be4-83b4-49fb-987a-857e04d2a80&title=&width=1280)
 3、根据线程检测死锁
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870167348-a4d20de0-4cb2-4005-9e46-ebd0e503b37f.png#averageHue=%23fbfafa&clientId=ubd551143-fb42-4&from=paste&height=675&id=ub46c78c4&originHeight=1012&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87126&status=done&style=none&taskId=u547849f2-f06a-49d8-b82c-4b4fd7ab9a5&title=&width=1280)
 4、线程
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870185329-c5d36a0d-0dbd-423c-afa0-092b92f27e6b.png#averageHue=%23f9f9f9&clientId=ubd551143-fb42-4&from=paste&height=629&id=u32f3fc93&originHeight=943&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45505&status=done&style=none&taskId=u997df1da-c594-4264-b740-21138ee446d&title=&width=1280)
5、VM 概要、
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870197176-66600486-5d59-4f82-9eba-d30684009664.png#averageHue=%23f7f6f5&clientId=ubd551143-fb42-4&from=paste&height=543&id=u61e75c2f&originHeight=815&originWidth=1919&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91659&status=done&style=none&taskId=uaf40afb3-244d-47a5-9a03-8c4e234c694&title=&width=1279.3333333333333)
# 3. Visual VM
**jvisualvm和visual vm的区别：**
visual vm是单独下载的工具，然后将visual vm结合到jdk中就变成了jvisualvm，仅仅是添加了一个j而已，这个j应该是java的用处，所以说jvisualvm其实就是visual vm
## 3.1 基本概述
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870269460-4e70e194-81e9-4bc2-a06e-332b18013526.png#averageHue=%23bfdac5&clientId=ubd551143-fb42-4&from=paste&height=342&id=u4d9a50de&originHeight=513&originWidth=842&originalType=binary&ratio=1&rotation=0&showTitle=false&size=325087&status=done&style=none&taskId=u85a756f5-5477-4255-b128-8faa71be4b4&title=&width=561.3333333333334)
**使用：**
在jdk安装目录中找到jvisualvm.exe，然后双击执行即可
打开DOS窗口，输入jvisualvm就可以打开该软件
## 3.2 插件的安装
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870443796-ce8d3c63-fcb2-4368-a2c2-133531c8e403.png#averageHue=%23c6dfcd&clientId=ubd551143-fb42-4&from=paste&height=357&id=ue81e8d0c&originHeight=535&originWidth=836&originalType=binary&ratio=1&rotation=0&showTitle=false&size=331585&status=done&style=none&taskId=u6db4260d-2993-4720-bf47-9b9c183369d&title=&width=557.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870456261-e818d387-0229-4b2c-b247-8237ddc2f463.png#averageHue=%23c2dcc6&clientId=ubd551143-fb42-4&from=paste&height=61&id=u031a223f&originHeight=91&originWidth=756&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58280&status=done&style=none&taskId=u68ed5c94-6140-4d42-8c48-b7d252ed4e9&title=&width=504)
首先在IDEA中搜索VisualVM Launcher插件并安装
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870467153-49032cce-e8ee-4af7-a74f-7f98a823899c.png#averageHue=%23e2c295&clientId=ubd551143-fb42-4&from=paste&height=643&id=u03bf92b1&originHeight=964&originWidth=1510&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105790&status=done&style=none&taskId=u8cbd4bfc-bff7-4854-bbff-da9ce0d6ddc&title=&width=1006.6666666666666)2、重启IDEA，然后配置该插件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870480635-b71d25af-39df-4acc-9b9f-f7c755095d53.png#averageHue=%23cfa464&clientId=ubd551143-fb42-4&from=paste&height=375&id=u064543bb&originHeight=563&originWidth=1510&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78381&status=done&style=none&taskId=u42bb037a-08e3-42e8-9d87-d13e1ec4c41&title=&width=1006.6666666666666)
3、使用两种方式来运行程序
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870489236-162de558-93ae-4a77-b1ae-a7fe4d857db8.png#averageHue=%23f2f2f1&clientId=ubd551143-fb42-4&from=paste&height=199&id=ube21e1e9&originHeight=299&originWidth=830&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49900&status=done&style=none&taskId=u7c80ce0d-e4ca-45db-800d-6111c56e2cc&title=&width=553.3333333333334)
4、运行效果
还是打开jvisualvm界面，只是不需要我们手动打开jvisualvm而已
## 3.3 连接方式
**本地连接**
监控本地Java进程的CPU、类、线程等
**远程连接**
1-确定远程服务器的ip地址
2-添加JMX（通过JMX技术具体监控远程服务器哪个Java进程）
3-修改bin/catalina.sh文件，连接远程的tomcat
4-在…/conf中添加jmxremote.access和jmxremote.password文件
5-将服务器地址改成公网ip地址
6-设置阿里云安全策略和防火墙策略
7-启动tomcat，查看tomcat启动日志和端口监听
8-JMX中输入端口号、用户名、密码登录
## 3.4 主要功能

1. **生成/读取堆内存快照**

一、生成堆内存快照
1、方式1：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870766053-4706c758-795c-447e-b440-f6274b330e46.png#averageHue=%23f1f0ef&clientId=ubd551143-fb42-4&from=paste&height=219&id=ud6e40add&originHeight=329&originWidth=521&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33341&status=done&style=none&taskId=u54c3bf2a-0ced-489d-8f15-788f6755003&title=&width=347.3333333333333)
2、方式2：![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870786207-dd33d3fd-71e5-4e2f-ba15-8a668bc8a62e.png#averageHue=%23f6f3ef&clientId=ubd551143-fb42-4&from=paste&height=245&id=uff985b70&originHeight=368&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95505&status=done&style=none&taskId=u2872aafd-29ad-4a82-b06a-369e359cb5c&title=&width=1280)
注意：
生成堆内存快照如下图：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870800520-958fbe06-1aac-4d70-b039-901a42c931ab.png#averageHue=%23deaf6e&clientId=ubd551143-fb42-4&from=paste&height=39&id=ud4bf4a7a&originHeight=58&originWidth=439&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5983&status=done&style=none&taskId=u960460e2-f90b-4ad0-bc1d-aa5afe1bdbc&title=&width=292.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870803807-0f8be246-a68b-4f37-993b-8dc7ccea9039.png#averageHue=%23f4f3f2&clientId=ubd551143-fb42-4&from=paste&height=147&id=u5c7fbe3f&originHeight=221&originWidth=476&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19837&status=done&style=none&taskId=ude9d9097-3db8-4cb6-b978-44fe69889e3&title=&width=317.3333333333333)
二、装入堆内存快照
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661870820767-e51a17e5-df67-48c4-a445-b3e81abece1e.png#averageHue=%23f6f5f4&clientId=ubd551143-fb42-4&from=paste&height=482&id=uf7e46b00&originHeight=723&originWidth=1308&originalType=binary&ratio=1&rotation=0&showTitle=false&size=141913&status=done&style=none&taskId=ued6406a2-40e6-4fb0-87d7-c58ebfdd78c&title=&width=872)

2. **查看JVM参数和系统属性**
3. **查看运行中的虚拟机进程**
4. **生成/读取线程快照**

一、生成线程快照
1、方式1：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871171729-def6ab21-2410-4c37-9a5c-ae7647eaa589.png#averageHue=%23f2f0ef&clientId=ubd551143-fb42-4&from=paste&height=233&id=u2281174a&originHeight=349&originWidth=515&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25536&status=done&style=none&taskId=uf1e4b5d4-3fcf-4de4-a910-3167d1fe867&title=&width=343.3333333333333)
注意：
生成线程快照如下图：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871191294-053b19e6-0f7e-404b-b9b6-6ec3e7f32b23.png#averageHue=%2371ac5d&clientId=ubd551143-fb42-4&from=paste&height=36&id=u6e803ab7&originHeight=54&originWidth=503&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7088&status=done&style=none&taskId=ub1c7c330-8256-4b34-8bbc-9f9274410ae&title=&width=335.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871194079-b7e266ba-9c56-4d1d-964e-c34e742cc3c8.png#averageHue=%23f5f3f1&clientId=ubd551143-fb42-4&from=paste&height=146&id=uc07dd732&originHeight=219&originWidth=495&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19382&status=done&style=none&taskId=u470486b1-ec8f-4c35-8905-644d4359c72&title=&width=330)
 二、装入线程快照
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871219842-fc78d320-b4d1-4456-8707-9595e1ddb09e.png#averageHue=%23f7f6f4&clientId=ubd551143-fb42-4&from=paste&height=486&id=u03772b20&originHeight=729&originWidth=1292&originalType=binary&ratio=1&rotation=0&showTitle=false&size=144607&status=done&style=none&taskId=uc5ba0e00-f274-4c83-bd2e-dfb20b50791&title=&width=861.3333333333334)

5. **程序资源的实时监控**
6. **其他功能**
   1. JMX代理连接
   2. 远程环境监控
   3. CPU分析和内存分析
# 4. Eclipse MAT
## 4.1. 基本概述
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871322259-49234a15-39f8-4aae-99af-cdd5e99d5cd1.png#averageHue=%23c0dfba&clientId=ubd551143-fb42-4&from=paste&height=107&id=ub595b5e3&originHeight=160&originWidth=840&originalType=binary&ratio=1&rotation=0&showTitle=false&size=154279&status=done&style=none&taskId=ub42feabe-d375-4317-ba0b-4a01199b0d9&title=&width=560)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871332069-18710bd2-1c6b-45c6-8a5e-b4369ddc62d0.png#averageHue=%23eaece5&clientId=ubd551143-fb42-4&from=paste&height=320&id=u4c407226&originHeight=480&originWidth=723&originalType=binary&ratio=1&rotation=0&showTitle=false&size=248551&status=done&style=none&taskId=u614867ce-0ed0-4e76-9111-f3d383d2534&title=&width=482)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871339690-3da3d91e-35d6-4697-a8d8-9a8d489e6083.png#averageHue=%23c1ddbe&clientId=ubd551143-fb42-4&from=paste&height=363&id=u4d84b0a2&originHeight=545&originWidth=559&originalType=binary&ratio=1&rotation=0&showTitle=false&size=307311&status=done&style=none&taskId=u04337803-8af8-4342-95d0-7d5cf759242&title=&width=372.6666666666667)
## 4.2 获取堆dump文件

1. dump文件内存

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871397340-6c0ff7a1-5f51-460e-b30b-55386f4c6d76.png#averageHue=%23c0ddbc&clientId=ubd551143-fb42-4&from=paste&height=176&id=u7f1a5b8d&originHeight=264&originWidth=844&originalType=binary&ratio=1&rotation=0&showTitle=false&size=203314&status=done&style=none&taskId=u8aee6cae-bb4a-479f-8cdc-62d34e08471&title=&width=562.6666666666666)

2. 两点说明

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871445963-d60f77ff-5306-441e-b2e1-02ed229f0218.png#averageHue=%23c4e0c0&clientId=ubd551143-fb42-4&from=paste&height=357&id=uff0e29bc&originHeight=536&originWidth=837&originalType=binary&ratio=1&rotation=0&showTitle=false&size=319120&status=done&style=none&taskId=u1b1f192c-2aeb-4914-84fd-ea278ad2d3a&title=&width=558)

3. 获取dump文件

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871473929-e1cd4314-00dc-4ba5-9f6c-7222b60941c4.png#averageHue=%23c1debd&clientId=ubd551143-fb42-4&from=paste&height=223&id=u05efd068&originHeight=335&originWidth=841&originalType=binary&ratio=1&rotation=0&showTitle=false&size=239288&status=done&style=none&taskId=u610138d8-5a81-4727-a183-6244f130d30&title=&width=560.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871478483-130e579a-219d-46d9-852d-91716e608295.png#averageHue=%23c5e1c1&clientId=ubd551143-fb42-4&from=paste&height=154&id=u268301f3&originHeight=231&originWidth=775&originalType=binary&ratio=1&rotation=0&showTitle=false&size=113907&status=done&style=none&taskId=uf87138bb-2752-4a9b-ad5e-e959811acde&title=&width=516.6666666666666)
## 4.3 分析堆dump文件
### 1. histogram
展示了各个类的实例数目以及这些实例的Shallow heap或者Retained heap的总和
图标：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871797229-d20909ac-dd18-4874-80e2-bae761f6cc2e.png#averageHue=%23f8f7f7&clientId=ubd551143-fb42-4&from=paste&height=610&id=ub93f81e9&originHeight=915&originWidth=1373&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102349&status=done&style=none&taskId=ue5a007c4-4921-4acd-81a3-4f9fd9a895f&title=&width=915.3333333333334)
具体内容：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871805098-aea50efe-5313-4872-992f-e960187fda2c.png#averageHue=%23d9ae6c&clientId=ubd551143-fb42-4&from=paste&height=250&id=u0ddf64b6&originHeight=375&originWidth=1090&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46419&status=done&style=none&taskId=u4f5452e4-f84a-4a0b-b95e-0793d6578a7&title=&width=726.6666666666666)
### 2. thread overview
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871845746-43bc2ef7-73c1-4ba0-9aff-ee9e350dac55.png#averageHue=%238cc19a&clientId=ubd551143-fb42-4&from=paste&height=81&id=ubd912199&originHeight=121&originWidth=815&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22834&status=done&style=none&taskId=u1e2f48f4-f60a-40a4-b754-9597f06a507&title=&width=543.3333333333334)
具体信息：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871853107-5b4fb209-e722-483e-88a0-518859a07b7b.png#averageHue=%23e3be79&clientId=ubd551143-fb42-4&from=paste&height=201&id=u58a2a728&originHeight=302&originWidth=747&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47854&status=done&style=none&taskId=u928865f3-3a9b-4c3b-9c10-de197cc56fd&title=&width=498)
### 3. 获得对象互相引用的关系
with outgoing references
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871913922-6baf54e4-3c1b-4242-8288-feea270a4f86.png#averageHue=%23f0eeeb&clientId=ubd551143-fb42-4&from=paste&height=247&id=uc19fb4f5&originHeight=370&originWidth=1026&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65292&status=done&style=none&taskId=uf8104fa1-b42d-4de3-8c61-a198cadc64a&title=&width=684)
结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871925381-893513bf-9b47-4c39-ba4a-a5bc4e5a8ee5.png#averageHue=%23efece9&clientId=ubd551143-fb42-4&from=paste&height=361&id=u870f2469&originHeight=542&originWidth=822&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78731&status=done&style=none&taskId=u00ec62c6-3377-44dd-88ca-8653b34630e&title=&width=548)
with incoming references
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871937639-88d716db-6aa5-4f44-ba2c-78fc9e857e5e.png#averageHue=%23f0eeec&clientId=ubd551143-fb42-4&from=paste&height=282&id=ub1f656a9&originHeight=423&originWidth=1033&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69041&status=done&style=none&taskId=u817949db-b46c-4074-a640-6a62ba8620e&title=&width=688.6666666666666)
结果：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661871945707-e8b2f4dc-c274-42c0-84b1-2e40c8ac606d.png#averageHue=%23f2efed&clientId=ubd551143-fb42-4&from=paste&height=120&id=u1f22a5cf&originHeight=180&originWidth=925&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24174&status=done&style=none&taskId=u3c504f30-0f84-473f-8a1f-1edbec2d3c4&title=&width=616.6666666666666)
### 4. 浅堆与深堆

1. shallow heap

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872041389-f74adbe0-4b51-4339-8605-bfb18b02badf.png#averageHue=%23c2dec8&clientId=ubd551143-fb42-4&from=paste&height=245&id=ubc1b8d95&originHeight=367&originWidth=839&originalType=binary&ratio=1&rotation=0&showTitle=false&size=253750&status=done&style=none&taskId=ua54cbde9-8df3-4529-9226-caea216a884&title=&width=559.3333333333334)
对象头代表根据类创建的对象的对象头，还有对象的大小不是可能向8字节对齐，而是就向8字节对齐

2. retained heap

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872068997-cf3c9e4f-7e56-4ce9-8e09-2d5beedbf7d3.png#averageHue=%23c0dbc5&clientId=ubd551143-fb42-4&from=paste&height=200&id=u7fa09de3&originHeight=300&originWidth=840&originalType=binary&ratio=1&rotation=0&showTitle=false&size=237902&status=done&style=none&taskId=uc0aa701d-d668-41fb-942d-6a49d4da706&title=&width=560)
注意：
当前深堆大小 = 当前对象的浅堆大小 + 对象中所包含对象的深堆大小

3. 补充：对象实际大小

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872110529-3f9d5543-f7fd-4a41-8ea2-dc364baf8b63.png#averageHue=%23c2ddc6&clientId=ubd551143-fb42-4&from=paste&height=335&id=u880d1a55&originHeight=502&originWidth=838&originalType=binary&ratio=1&rotation=0&showTitle=false&size=268696&status=done&style=none&taskId=ue057c679-dfc3-4b23-8cc9-a7abf009d02&title=&width=558.6666666666666)

4. 练习

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872246277-7c95959b-85c1-4366-be96-2a11d18e9491.png#averageHue=%23cde3cf&clientId=ubd551143-fb42-4&from=paste&height=346&id=ua4a72224&originHeight=519&originWidth=510&originalType=binary&ratio=1&rotation=0&showTitle=false&size=132846&status=done&style=none&taskId=ub2843cf8-a172-4db7-ad86-af4665e4095&title=&width=340)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872252721-7646f037-0591-4fd8-aefe-59e3003ede18.png#averageHue=%23bfd9c3&clientId=ubd551143-fb42-4&from=paste&height=114&id=u5e534107&originHeight=171&originWidth=639&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90858&status=done&style=none&taskId=u03681a28-0a2a-48e5-909b-2556a7b2cec&title=&width=426)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872255896-57d0b636-bdb8-4f6b-9bb5-c19ceab6016f.png#averageHue=%23daecdb&clientId=ubd551143-fb42-4&from=paste&height=285&id=u4dcfcdd4&originHeight=427&originWidth=531&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108987&status=done&style=none&taskId=u1c32e715-201c-4de3-8643-6160daff0a6&title=&width=354)
### 5. 案例分析：StudentTrace
```java
/**
 * 有一个学生浏览网页的记录程序，它将记录 每个学生访问过的网站地址。
 * 它由三个部分组成：Student、WebPage和StudentTrace三个类
 *
 *  -XX:+HeapDumpBeforeFullGC -XX:HeapDumpPath=c:\code\student.hprof
 * @author shkstart
 * @create 16:11
 */
public class StudentTrace {
    static List<WebPage> webpages = new ArrayList<WebPage>();

    public static void createWebPages() {
        for (int i = 0; i < 100; i++) {
            WebPage wp = new WebPage();
            wp.setUrl("http://www." + Integer.toString(i) + ".com");
            wp.setContent(Integer.toString(i));
            webpages.add(wp);
        }
    }

    public static void main(String[] args) {
        createWebPages();//创建了100个网页
        //创建3个学生对象
        Student st3 = new Student(3, "Tom");
        Student st5 = new Student(5, "Jerry");
        Student st7 = new Student(7, "Lily");

        for (int i = 0; i < webpages.size(); i++) {
            if (i % st3.getId() == 0)
                st3.visit(webpages.get(i));
            if (i % st5.getId() == 0)
                st5.visit(webpages.get(i));
            if (i % st7.getId() == 0)
                st7.visit(webpages.get(i));
        }
        webpages.clear();
        System.gc();
    }
}

class Student {
    private int id;
    private String name;
    private List<WebPage> history = new ArrayList<>();

    public Student(int id, String name) {
        super();
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<WebPage> getHistory() {
        return history;
    }

    public void setHistory(List<WebPage> history) {
        this.history = history;
    }

    public void visit(WebPage wp) {
        if (wp != null) {
            history.add(wp);
        }
    }
}


class WebPage {
    private String url;
    private String content;

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}
```
图片：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872320014-22ebdafb-415e-4ae6-8ca8-6b7a53453a03.png#averageHue=%23f5f2f0&clientId=ubd551143-fb42-4&from=paste&height=399&id=u4fe83278&originHeight=598&originWidth=1229&originalType=binary&ratio=1&rotation=0&showTitle=false&size=94692&status=done&style=none&taskId=u0daa3fd0-4dd1-41b8-9bad-7af9cca736b&title=&width=819.3333333333334)
**结论：**
elementData数组的浅堆是80个字节，而elementData数组中的所有WebPage对象的深堆之和是1208个字节，所以加在一起就是elementData数组的深堆之和，也就是1288个字节
**解释：**
我说“elementData数组的浅堆是80个字节”，其中15个对象一共是60个字节，对象头8个字节，数组对象本身4个字节，这些的和是72个字节，然后总和要是8的倍数，所以“elementData数组的浅堆是80个字节”
我说“WebPage对象的深堆之和是1208个字节”，一共有15个对象，其中0、21、42、63、84、35、70不仅仅是7的倍数，还是3或者5的倍数，所以这几个数值对应的i不能计算在深堆之内，这15个对象中大多数的深堆是152个字节，但是i是0和7的那两个深堆是144个字节，所以(13*152+144*2)-(6*152+144)=1208，所以这也印证了我上面的话，即“WebPage对象的深堆之和是1208个字节”
因此“elementData数组的浅堆80个字节”加上“WebPage对象的深堆之和1208个字节”，正好是1288个字节，说明“elementData数组的浅堆1288个字节”
### 6. 支配树
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872366602-165d365f-a2b2-48ad-84f9-0916785ab0b6.png#averageHue=%23bed8c3&clientId=ubd551143-fb42-4&from=paste&height=275&id=u2b34e6e1&originHeight=413&originWidth=839&originalType=binary&ratio=1&rotation=0&showTitle=false&size=447811&status=done&style=none&taskId=u9ffd5cf3-6a51-4087-93a2-1687a2d4ce6&title=&width=559.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872370502-eb0bbf1d-e03a-42ea-a7ae-f580157d3a1c.png#averageHue=%23e4c075&clientId=ubd551143-fb42-4&from=paste&height=305&id=u276c51fb&originHeight=457&originWidth=838&originalType=binary&ratio=1&rotation=0&showTitle=false&size=184597&status=done&style=none&taskId=u5973c59f-1a68-400a-9658-b95a5ac8817&title=&width=558.6666666666666)
注意：
跟随我一起来理解如何从“对象引用图---》支配树”，首先需要理解支配者（如果要到达对象B，毕竟经过对象A，那么对象A就是对象B的支配者，可以想到支配者大于等于1），然后需要理解直接支配者（在支配者中距离对象B最近的对象A就是对象B的直接支配者，你要明白直接支配者不一定就是对象B的上一级，然后直接支配者只有一个），然后还需要理解支配树是怎么画的，其实支配树中的对象与对象之间的关系就是直接支配关系，也就是上一级是下一级的直接支配者，只要按照这样的方式来作图，肯定能从“对象引用图---》支配树”

**在Eclipse MAT工具中如何查看支配树：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872402087-af5b8e89-277c-46ef-9c32-c0b5baa2f93a.png#averageHue=%23c0dac2&clientId=ubd551143-fb42-4&from=paste&height=348&id=u48bc2a28&originHeight=522&originWidth=845&originalType=binary&ratio=1&rotation=0&showTitle=false&size=465176&status=done&style=none&taskId=u5cffe2b1-b1c7-45c6-971e-c254d740d1d&title=&width=563.3333333333334)、
## 4.4 案例：Tomcat堆溢出分析
**说明：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872839690-76ff93df-73cd-48f0-917e-c741ec014dcf.png#averageHue=%23b9d1bd&clientId=ubd551143-fb42-4&from=paste&height=57&id=u854061e7&originHeight=85&originWidth=836&originalType=binary&ratio=1&rotation=0&showTitle=false&size=130433&status=done&style=none&taskId=u19cddbc9-e0f5-424c-bf30-03e8da8a598&title=&width=557.3333333333334)
**分析过程：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872875024-f72cb0b3-6d4f-4b1a-9038-128a69de018e.png#averageHue=%23c0e1c7&clientId=ubd551143-fb42-4&from=paste&height=355&id=u7c4a815b&originHeight=532&originWidth=794&originalType=binary&ratio=1&rotation=0&showTitle=false&size=170232&status=done&style=none&taskId=uc7f41919-719c-4120-9101-fdb6f3a15e9&title=&width=529.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872887210-81f90f10-01db-4b4d-acbc-1932b6aee916.png#averageHue=%23c3e2c8&clientId=ubd551143-fb42-4&from=paste&height=261&id=u210f537f&originHeight=392&originWidth=781&originalType=binary&ratio=1&rotation=0&showTitle=false&size=150180&status=done&style=none&taskId=u359d839a-fa43-4b39-b212-e47b452c1a0&title=&width=520.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872892261-f92ba6da-56d3-4986-8ea8-47995f2d4b91.png#averageHue=%23bddac3&clientId=ubd551143-fb42-4&from=paste&height=152&id=uee5b6355&originHeight=228&originWidth=765&originalType=binary&ratio=1&rotation=0&showTitle=false&size=217333&status=done&style=none&taskId=u0c5a5968-943f-4e53-a92e-72940b7086f&title=&width=510)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872896178-06cfd805-8f0c-4d94-aadd-1b024b36ee78.png#averageHue=%23bfdbc4&clientId=ubd551143-fb42-4&from=paste&height=346&id=u4d334820&originHeight=519&originWidth=1026&originalType=binary&ratio=1&rotation=0&showTitle=false&size=609863&status=done&style=none&taskId=u54a96235-f422-4c7a-b6e0-09e22739b73&title=&width=684)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872899858-12a56c09-52a5-4da5-9d84-53bb79d06d52.png#averageHue=%23bad6bf&clientId=ubd551143-fb42-4&from=paste&height=325&id=ufba06a51&originHeight=487&originWidth=795&originalType=binary&ratio=1&rotation=0&showTitle=false&size=489506&status=done&style=none&taskId=u32314e6d-f7fd-4b33-b470-1f7ae213b05&title=&width=530)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872903409-6fb30ce5-e686-4efd-9a32-b99d14b3f312.png#averageHue=%23bddac2&clientId=ubd551143-fb42-4&from=paste&height=273&id=u2fa446d4&originHeight=409&originWidth=957&originalType=binary&ratio=1&rotation=0&showTitle=false&size=448638&status=done&style=none&taskId=u039769e3-f3f1-466f-886a-7704aa22a56&title=&width=638)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872907773-6c0f964e-deb9-4563-961c-155f4679f497.png#averageHue=%23b8d6be&clientId=ubd551143-fb42-4&from=paste&height=140&id=ucf1730b9&originHeight=210&originWidth=764&originalType=binary&ratio=1&rotation=0&showTitle=false&size=242828&status=done&style=none&taskId=u480d6eaf-1736-4fb7-82dd-da643ec91e8&title=&width=509.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872913066-686c5a40-1ffb-4e1a-8258-62373f87f89a.png#averageHue=%23bbd9c1&clientId=ubd551143-fb42-4&from=paste&height=371&id=ue7df8b3e&originHeight=557&originWidth=796&originalType=binary&ratio=1&rotation=0&showTitle=false&size=512110&status=done&style=none&taskId=u0522402c-ec45-4162-9cc1-345690f8ce9&title=&width=530.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661872917971-bf85b4c7-4f06-4466-a0c9-f69ce06718fb.png#averageHue=%23b2d7ba&clientId=ubd551143-fb42-4&from=paste&height=302&id=u6b51e4d8&originHeight=453&originWidth=1025&originalType=binary&ratio=1&rotation=0&showTitle=false&size=348211&status=done&style=none&taskId=ue36fc5c7-38af-450b-ac8e-cfc3c155546&title=&width=683.3333333333334)
# 5. 补充1：再谈内存泄露
## 5.1 内存泄露的理解与分析
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661873365176-ec1e4879-57de-4690-b711-7cc5e50cd4c2.png#averageHue=%23c5dec8&clientId=ud7e02e70-965a-4&from=paste&height=295&id=ub641a489&originHeight=442&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=258326&status=done&style=none&taskId=ub010bbdd-117b-499b-a9a8-0793ae5ddf3&title=&width=555.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661873391762-3590cd6c-ca81-4a20-bd63-50382979ed67.png#averageHue=%23c8e0c5&clientId=ud7e02e70-965a-4&from=paste&height=337&id=u46ad55d1&originHeight=505&originWidth=828&originalType=binary&ratio=1&rotation=0&showTitle=false&size=318780&status=done&style=none&taskId=u7f952d5c-f3e5-4a76-984b-cdd1d8c6140&title=&width=552)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661873395651-eaf9314e-9cc2-4902-a638-6ed599d03ee0.png#averageHue=%23c3dec8&clientId=ud7e02e70-965a-4&from=paste&height=242&id=u13420134&originHeight=363&originWidth=831&originalType=binary&ratio=1&rotation=0&showTitle=false&size=260974&status=done&style=none&taskId=u53a7267c-68e0-43cc-afb4-784b1bec935&title=&width=554)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661873398272-1aa863de-8036-4da1-8563-ff53df057027.png#averageHue=%23c0dbc5&clientId=ud7e02e70-965a-4&from=paste&height=123&id=udc34818c&originHeight=184&originWidth=813&originalType=binary&ratio=1&rotation=0&showTitle=false&size=144582&status=done&style=none&taskId=u642ba6f2-c256-4bbc-bcda-ce07a2094ec&title=&width=542)
## 5.2 Java中内存泄露的8种情况
1-静态集合类
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661873722812-c514dd06-c185-4757-9011-a4d1b8405220.png#averageHue=%23c2ddc7&clientId=ud7e02e70-965a-4&from=paste&height=204&id=uae14ee75&originHeight=306&originWidth=841&originalType=binary&ratio=1&rotation=0&showTitle=false&size=215309&status=done&style=none&taskId=u27be98d8-683f-45b7-8c3d-f9e59b9f467&title=&width=560.6666666666666)
2-单例模式
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661873744867-fd48e8a9-b1a7-47ab-8f32-70931542c221.png#averageHue=%23c2ddc7&clientId=ud7e02e70-965a-4&from=paste&height=204&id=u56e66fbc&originHeight=306&originWidth=841&originalType=binary&ratio=1&rotation=0&showTitle=false&size=215309&status=done&style=none&taskId=u51653e6b-35b5-4d0d-8b46-58f2bb3cdf2&title=&width=560.6666666666666)
3-内部类持有外部类
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661874140335-b269d337-7119-4690-b3c4-2d9c4b2aa5ba.png#averageHue=%23bbd4c0&clientId=ud7e02e70-965a-4&from=paste&height=58&id=u05e7b2d9&originHeight=87&originWidth=840&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112922&status=done&style=none&taskId=ub9842ac7-bf51-472e-a412-409532bf07e&title=&width=560)
4-各种连接，如数据库连接、网络连接和IO连接等
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661874223675-f69b3a9e-2e41-4fea-b404-13c37c58ad6d.png#averageHue=%23c2ddc7&clientId=ud7e02e70-965a-4&from=paste&height=341&id=u6ba546d8&originHeight=512&originWidth=831&originalType=binary&ratio=1&rotation=0&showTitle=false&size=331636&status=done&style=none&taskId=u2e425ded-fe47-4c67-b43c-a8067d190f3&title=&width=554)
5-变量不合理的作用域
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661874231191-a0a111a8-46d4-45a1-85b7-655f8b54e56d.png#averageHue=%23c0dbc5&clientId=ud7e02e70-965a-4&from=paste&height=325&id=u7c970b93&originHeight=488&originWidth=838&originalType=binary&ratio=1&rotation=0&showTitle=false&size=360237&status=done&style=none&taskId=u19e1a901-107b-45ab-b106-1b4640511b4&title=&width=558.6666666666666)
6-改变哈希值
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661874344512-c8c2af3f-40fe-4680-9bbd-a213544d6ca0.png#averageHue=%23bed8c3&clientId=ud7e02e70-965a-4&from=paste&height=191&id=u17e64a64&originHeight=286&originWidth=840&originalType=binary&ratio=1&rotation=0&showTitle=false&size=271714&status=done&style=none&taskId=u650bffae-4df8-4bae-906f-cbfc369bd17&title=&width=560)
 例1：
```java
/**
 * 演示内存泄漏
 *
 * @author shkstart
 * @create 14:43
 */
public class ChangeHashCode {
    public static void main(String[] args) {
        HashSet set = new HashSet();
        Person p1 = new Person(1001, "AA");
        Person p2 = new Person(1002, "BB");

        set.add(p1);
        set.add(p2);

        p1.name = "CC";//导致了内存的泄漏
        set.remove(p1); //删除失败

        System.out.println(set);

        set.add(new Person(1001, "CC"));
        System.out.println(set);

        set.add(new Person(1001, "AA"));
        System.out.println(set);

    }
}

class Person {
    int id;
    String name;

    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Person)) return false;

        Person person = (Person) o;

        if (id != person.id) return false;
        return name != null ? name.equals(person.name) : person.name == null;
    }

    @Override
    public int hashCode() {
        int result = id;
        result = 31 * result + (name != null ? name.hashCode() : 0);
        return result;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```
例2：
```java
/**
 * 演示内存泄漏
 * @author shkstart
 * @create 14:47
 */
public class ChangeHashCode1 {
    public static void main(String[] args) {
        HashSet<Point> hs = new HashSet<Point>();
        Point cc = new Point();
        cc.setX(10);//hashCode = 41
        hs.add(cc);

        cc.setX(20);//hashCode = 51  此行为导致了内存的泄漏

        System.out.println("hs.remove = " + hs.remove(cc));//false
        hs.add(cc);
        System.out.println("hs.size = " + hs.size());//size = 2

        System.out.println(hs);
    }

}

class Point {
    int x;

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + x;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (getClass() != obj.getClass()) return false;
        Point other = (Point) obj;
        if (x != other.x) return false;
        return true;
    }

    @Override
    public String toString() {
        return "Point{" +
                "x=" + x +
                '}';
    }
}

```
7-缓存泄露
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661874410119-c33a6976-a3e1-4982-9756-46c7aba62eb7.png#averageHue=%23bdd7c2&clientId=ud7e02e70-965a-4&from=paste&height=113&id=uf497a75e&originHeight=169&originWidth=835&originalType=binary&ratio=1&rotation=0&showTitle=false&size=182623&status=done&style=none&taskId=u7d7a28f3-0bbb-4c5e-b1fd-7d065e4f8a6&title=&width=556.6666666666666)
** 例子：**
```java
/**
 * 演示内存泄漏
 *
 * @author shkstart
 * @create 14:53
 */
public class MapTest {
    static Map wMap = new WeakHashMap();
    static Map map = new HashMap();

    public static void main(String[] args) {
        init();
        testWeakHashMap();
        testHashMap();
    }

    public static void init() {
        String ref1 = new String("obejct1");
        String ref2 = new String("obejct2");
        String ref3 = new String("obejct3");
        String ref4 = new String("obejct4");
        wMap.put(ref1, "cacheObject1");
        wMap.put(ref2, "cacheObject2");
        map.put(ref3, "cacheObject3");
        map.put(ref4, "cacheObject4");
        System.out.println("String引用ref1，ref2，ref3，ref4 消失");

    }

    public static void testWeakHashMap() {

        System.out.println("WeakHashMap GC之前");
        for (Object o : wMap.entrySet()) {
            System.out.println(o);
        }
        try {
            System.gc();
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("WeakHashMap GC之后");
        for (Object o : wMap.entrySet()) {
            System.out.println(o);
        }
    }

    public static void testHashMap() {
        System.out.println("HashMap GC之前");
        for (Object o : map.entrySet()) {
            System.out.println(o);
        }
        try {
            System.gc();
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("HashMap GC之后");
        for (Object o : map.entrySet()) {
            System.out.println(o);
        }
    }

}

```
结果：
```java
String引用ref1，ref2，ref3，ref4 消失
WeakHashMap GC之前
obejct2=cacheObject2
obejct1=cacheObject1
WeakHashMap GC之后
HashMap GC之前
obejct4=cacheObject4
obejct3=cacheObject3
Disconnected from the target VM, address: '127.0.0.1:51628', transport: 'socket'
HashMap GC之后
obejct4=cacheObject4
obejct3=cacheObject3
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661874445874-1438ea97-40ff-4949-92d4-e70bdba6a461.png#averageHue=%23d7dfd1&clientId=ud7e02e70-965a-4&from=paste&height=358&id=u77452bb9&originHeight=537&originWidth=837&originalType=binary&ratio=1&rotation=0&showTitle=false&size=247186&status=done&style=none&taskId=ue9a18c10-7a05-4a59-8789-ac3fb856d71&title=&width=558)
8-监听器和回调
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661874494244-b6ef52d2-5ce3-4951-bfcb-3ad02ba6c440.png#averageHue=%23c1dbc6&clientId=ud7e02e70-965a-4&from=paste&height=96&id=u4d344cb0&originHeight=144&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=111780&status=done&style=none&taskId=ue98d20f1-6529-4d80-a5fd-fea00cc3f31&title=&width=555.3333333333334)
## 5.3 内存泄露案例分析
```java
代码：
public class Stack {
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) { //入栈
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop() {
        if (size == 0)
            throw new EmptyStackException();
        Object result = elements[--size];
        elements[size] = null;
        return result;
    }

    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    }
}

```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954221644-649a593a-1820-412a-b0a7-523852331210.png#averageHue=%23bdd7c2&clientId=ubfef1bb3-f16e-4&from=paste&height=85&id=u005148df&originHeight=127&originWidth=822&originalType=binary&ratio=1&rotation=0&showTitle=false&size=127024&status=done&style=none&taskId=udebe68f4-c281-45e8-8603-5b413152843&title=&width=548)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954224467-5cac12d2-6b64-4b74-8659-bda7a49e4202.png#averageHue=%23f1f2e7&clientId=ubfef1bb3-f16e-4&from=paste&height=317&id=u69419b47&originHeight=476&originWidth=813&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91341&status=done&style=none&taskId=ua58f7728-1d5d-4072-829b-d212f8c7088&title=&width=542)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954229064-e8bc1381-55e6-4324-86a6-9a81c3ae2ddf.png#averageHue=%23f4f5eb&clientId=ubfef1bb3-f16e-4&from=paste&height=299&id=u787db315&originHeight=449&originWidth=788&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88572&status=done&style=none&taskId=u7efc4a69-b53c-4f2f-9912-a22f470f6f3&title=&width=525.3333333333334)
**解决办法：**
将代码中的pop()方法变成如下方法：
```java
public Object pop() { //出栈
    if (size == 0)
        throw new EmptyStackException();
    return elements[--size];
}
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954262328-525ec24a-f07d-4e9d-ba3d-6934868283ad.png#averageHue=%23fafaf3&clientId=ubfef1bb3-f16e-4&from=paste&height=333&id=u4440f512&originHeight=500&originWidth=840&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91655&status=done&style=none&taskId=u7b31be89-a771-4806-b4fc-f0e4cf8d276&title=&width=560)
# 6. 补充2：支持使用OQL语言查询对象信息
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954303427-bf66b690-537c-497e-930e-86db87fa9373.png#averageHue=%23bed9c1&clientId=ubfef1bb3-f16e-4&from=paste&height=40&id=u4b5df4a2&originHeight=60&originWidth=719&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58579&status=done&style=none&taskId=u8554e495-539a-4036-9f6d-40445a34210&title=&width=479.3333333333333)
在Eclipse MAT中如何用：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954314823-fa9145bb-8f63-419d-8789-5f6faaaee868.png#averageHue=%23b3d0a8&clientId=ubfef1bb3-f16e-4&from=paste&height=109&id=u0c67ffa1&originHeight=163&originWidth=645&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21562&status=done&style=none&taskId=udea442af-6dac-4f70-9414-4638d5cc0f7&title=&width=430)
栗子：
```java
例子：
select * from java.util.ArrayList（列出所有的ArrayList对象信息）
select v.elementData from java.util.ArrayList v（注意：elementData代表ArrayList中的数组，结果最终以数组形式将结果呈现出来）
select objects v.elementData from java.util.ArrayList v（注意：elementData代表ArrayList中的数组，objects代表对象类型，所以最终以对象形式将结果呈现出来，同时展示出来的还有浅堆、深堆）
select as retained set * from com.atguigu.mat.Student（得到对象的保留级）
select * from 0x6cd57c828（0x6cd57c828是Student类的地址值）
select * from char[] s where s.@length > 10（char型数组长度大于10的数组）
select * from java.lang.String s where s.value != null（字符串值不为空的字符串信息）
select toString(f.path.value) from java.io.File f（列出文件的路径值）
select v.elementData.@length from java.util.ArrayList v（列出Arraylist对象中ArrayList中的数组长度）
```
代码：
```java
/**
 * 有一个学生浏览网页的记录程序，它将记录 每个学生访问过的网站地址。
 * 它由三个部分组成：Student、WebPage和StudentTrace三个类
 *
 *  -XX:+HeapDumpBeforeFullGC -XX:HeapDumpPath=c:\code\student.hprof
 * @author shkstart
 * @create 16:11
 */
public class StudentTrace {
    static List<WebPage> webpages = new ArrayList<WebPage>();

    public static void createWebPages() {
        for (int i = 0; i < 100; i++) {
            WebPage wp = new WebPage();
            wp.setUrl("http://www." + Integer.toString(i) + ".com");
            wp.setContent(Integer.toString(i));
            webpages.add(wp);
        }
    }

    public static void main(String[] args) {
        createWebPages();//创建了100个网页
        //创建3个学生对象
        Student st3 = new Student(3, "Tom");
        Student st5 = new Student(5, "Jerry");
        Student st7 = new Student(7, "Lily");

        for (int i = 0; i < webpages.size(); i++) {
            if (i % st3.getId() == 0)
                st3.visit(webpages.get(i));
            if (i % st5.getId() == 0)
                st5.visit(webpages.get(i));
            if (i % st7.getId() == 0)
                st7.visit(webpages.get(i));
        }
        webpages.clear();
        System.gc();
    }
}

class Student {
    private int id;
    private String name;
    private List<WebPage> history = new ArrayList<>();

    public Student(int id, String name) {
        super();
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<WebPage> getHistory() {
        return history;
    }

    public void setHistory(List<WebPage> history) {
        this.history = history;
    }

    public void visit(WebPage wp) {
        if (wp != null) {
            history.add(wp);
        }
    }
}


class WebPage {
    private String url;
    private String content;

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}
```
## 6.1 SELECT子句
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954364236-d8392c75-db71-41d5-b46c-a921af1e39a7.png#averageHue=%23c1dcc6&clientId=ubfef1bb3-f16e-4&from=paste&height=301&id=u3b6cf191&originHeight=451&originWidth=752&originalType=binary&ratio=1&rotation=0&showTitle=false&size=280208&status=done&style=none&taskId=uef08fbd9-379c-4c91-bdc9-62adf73d38f&title=&width=501.3333333333333)
## 6.2 FROM子句
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954379802-db510cda-d527-4c13-96e9-cc9cc67308b3.png#averageHue=%23c2ddc7&clientId=ubfef1bb3-f16e-4&from=paste&height=195&id=ub8c09437&originHeight=292&originWidth=746&originalType=binary&ratio=1&rotation=0&showTitle=false&size=169531&status=done&style=none&taskId=u356aabdf-935d-4bfc-85c8-6d26e1e3909&title=&width=497.3333333333333)
## 6.3 where子句
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954398531-3da20daa-6384-4468-90c5-61d740c2202b.png#averageHue=%23c2ddc7&clientId=ubfef1bb3-f16e-4&from=paste&height=195&id=u6d9c0fd9&originHeight=292&originWidth=746&originalType=binary&ratio=1&rotation=0&showTitle=false&size=169531&status=done&style=none&taskId=u4c0a4b7d-4f0d-49b0-96ee-234518e4516&title=&width=497.3333333333333)
## 6.4 内置对象与方法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954416807-78bbcb29-4ced-40cd-890a-d35b89bac7a1.png#averageHue=%23c2ddc7&clientId=ubfef1bb3-f16e-4&from=paste&height=314&id=u1a493925&originHeight=471&originWidth=749&originalType=binary&ratio=1&rotation=0&showTitle=false&size=286260&status=done&style=none&taskId=ud25e5c5d-363a-4dc2-9204-5f56f7d7d36&title=&width=499.3333333333333)
# 7. JProfiler
## 7.1 基本概述
### 1. 介绍
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954752778-d111e455-d524-4b10-bade-7f0a3ba9520a.png#averageHue=%23c1e1bc&clientId=ubfef1bb3-f16e-4&from=paste&height=153&id=uafc4c848&originHeight=229&originWidth=832&originalType=binary&ratio=1&rotation=0&showTitle=false&size=170987&status=done&style=none&taskId=u121354c0-d125-4a37-8524-95c7df1eda8&title=&width=554.6666666666666)
### 2. 特点
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954765211-3612b49a-2ac5-4529-bb9f-df25ac55e1eb.png#averageHue=%23c5e1c0&clientId=ubfef1bb3-f16e-4&from=paste&height=259&id=uec456873&originHeight=388&originWidth=679&originalType=binary&ratio=1&rotation=0&showTitle=false&size=169836&status=done&style=none&taskId=u0fa33098-0e64-4b01-8fc1-4d5445ea014&title=&width=452.6666666666667)
### 3. 主要功能
1-方法调用
对方法调用的分析可以帮助您了解应用程序正在做什么，并找到提高其性能的方法
2-内存分配
通过分析堆上对象、引用链和垃圾收集能帮您修复内存泄露问题，优化内存使用
3-线程和锁
JProfiler提供多种针对线程和锁的分析视图助您发现多线程问题
4-高级子系统
许多性能问题都发生在更高的语义级别上。例如，对于JDBC调用，您可能希望找出执行最慢的SQL语句。JProfiler支持对这些子系统进行集成分析
## 7.2 . 安装与配置
### 1. 下载与安装
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954867722-8875e473-118d-4899-b025-6868504c9d90.png#averageHue=%23f4e3d7&clientId=ubfef1bb3-f16e-4&from=paste&height=341&id=ub16e5f38&originHeight=512&originWidth=649&originalType=binary&ratio=1&rotation=0&showTitle=false&size=137096&status=done&style=none&taskId=uce496183-c5c2-4127-b58b-2f720379fb7&title=&width=432.6666666666667)
### 2. JProfiler中配置IDEA

1、IDE Integrations
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954876766-0c287efb-7389-486d-b1eb-3b7d8ddd3977.png#averageHue=%23edecea&clientId=ubfef1bb3-f16e-4&from=paste&height=429&id=u269f9ae1&originHeight=643&originWidth=534&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49926&status=done&style=none&taskId=ua7fbb92c-7d4d-4de0-9bb8-4e2d61aefa3&title=&width=356)
2、选择合适的IDE版本
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954899203-7193dabe-ec46-4756-9f69-b5af89f5832f.png#averageHue=%23f4f3f2&clientId=ubfef1bb3-f16e-4&from=paste&height=320&id=u1a632100&originHeight=480&originWidth=925&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34500&status=done&style=none&taskId=uf708d680-a2e0-4e23-89d1-068c1afa7c4&title=&width=616.6666666666666)
3、开始集成
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954906726-de6a2f0a-c9b9-4b78-b497-5230b9d4264e.png#averageHue=%23f0efee&clientId=ubfef1bb3-f16e-4&from=paste&height=230&id=u6282748d&originHeight=345&originWidth=738&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23733&status=done&style=none&taskId=u64f86876-5b49-4baf-8601-956535388cc&title=&width=492)
4、正式集成
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954914876-9abfee2e-c1fe-476d-8041-5956fb3d40c2.png#averageHue=%23f7f5f0&clientId=ubfef1bb3-f16e-4&from=paste&height=466&id=ua94437bd&originHeight=699&originWidth=914&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87329&status=done&style=none&taskId=u1a024747-08ab-47c8-8bea-465419d4795&title=&width=609.3333333333334)
5、集成成功
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954936914-a5413009-bc22-4d66-a658-24afe134b07a.png#averageHue=%23f8f7f7&clientId=ubfef1bb3-f16e-4&from=paste&height=139&id=u96f97489&originHeight=208&originWidth=546&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11680&status=done&style=none&taskId=u8f921761-8cf7-4ea7-8def-e8680cc4aa7&title=&width=364)
### 3. IDEA集成JProfiler
方式1：在线安装
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661954975301-c321a41c-bf20-40bc-b4f3-570cfe10a9b7.png#averageHue=%23debf8a&clientId=ubfef1bb3-f16e-4&from=paste&height=304&id=u30fc30cc&originHeight=456&originWidth=1141&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100345&status=done&style=none&taskId=ue14e1648-6e03-441b-8dbb-b1d3b8e75e4&title=&width=760.6666666666666)
方式2、离线安装
首先下载插件：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955000043-7538d40a-df49-4802-b39a-d1173bee9413.png#averageHue=%23fdfcfb&clientId=ubfef1bb3-f16e-4&from=paste&height=153&id=u0effd397&originHeight=229&originWidth=451&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46431&status=done&style=none&taskId=u4e743b7f-aacf-4714-83a1-e95366cd4c6&title=&width=300.6666666666667)
 准备离线安装：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955024899-be425e9e-d252-4a52-8b54-c37748521a5e.png#averageHue=%23dcb883&clientId=ubfef1bb3-f16e-4&from=paste&height=199&id=u724f17a1&originHeight=298&originWidth=1773&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65074&status=done&style=none&taskId=ufd51b66d-c28b-46d5-8a58-ff30a31bbf0&title=&width=1182)
正式离线安装：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955031512-f0230a89-0cd2-4dd3-b95c-0437c8c9970e.png#averageHue=%23f6f6f5&clientId=ubfef1bb3-f16e-4&from=paste&height=505&id=u8811f479&originHeight=757&originWidth=635&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49223&status=done&style=none&taskId=uc3b39b00-d591-4dac-8d80-b07e2459e95&title=&width=423.3333333333333)
注意：无论采用方式1还是方式2都需要重启IDEA
 将JProfiler配置到IDEA中
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955043118-8273f05b-7661-4807-b950-683d400ff29b.png#averageHue=%23d5ad6f&clientId=ubfef1bb3-f16e-4&from=paste&height=291&id=u71207f50&originHeight=436&originWidth=1265&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76564&status=done&style=none&taskId=ufd60bbce-d7d3-4d9e-a642-6cb49d152dc&title=&width=843.3333333333334)
## 7.3 具体使用
### 1. 数据采集方式
instrumentation重构模式
Sampling抽样模式
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955330237-e924e0e4-c3cd-4227-9aaf-d928e00ad1b2.png#averageHue=%23bdd9b9&clientId=ubfef1bb3-f16e-4&from=paste&height=289&id=u8312b2ec&originHeight=433&originWidth=768&originalType=binary&ratio=1&rotation=0&showTitle=false&size=402801&status=done&style=none&taskId=u7eefbc5c-832c-4a45-9a27-16e6f8771e5&title=&width=512)
推荐使用Sampling方式，足够用来分析OOM问题了
### 2. 遥感监测 Telemetries
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955583550-e3bb6587-8a53-4cb6-9007-2269ba91561a.png#averageHue=%23f8f7f6&clientId=ubfef1bb3-f16e-4&from=paste&height=351&id=ud79937e2&originHeight=527&originWidth=399&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34206&status=done&style=none&taskId=uc8aff6be-7559-4112-b8a1-63f33296651&title=&width=266)
### 3. 内存视图 Live Memory
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955611215-ba4044a3-bfbf-4841-a653-5374b3389100.png#averageHue=%23c2dfbe&clientId=ubfef1bb3-f16e-4&from=paste&height=328&id=u025f47b6&originHeight=492&originWidth=825&originalType=binary&ratio=1&rotation=0&showTitle=false&size=304989&status=done&style=none&taskId=u2267cba8-b7b9-484f-9adc-c879c2acf12&title=&width=550)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955615723-8194dac1-d723-4f90-9de4-acb1329bd199.png#averageHue=%23bedeb8&clientId=ubfef1bb3-f16e-4&from=paste&height=43&id=u179d0e5e&originHeight=65&originWidth=590&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46182&status=done&style=none&taskId=u7e23cd6d-13d1-45dd-8e31-c98b618fee8&title=&width=393.3333333333333)
**分析：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955625428-c89897e5-644a-425e-859d-3c8ef3678ae6.png#averageHue=%23c3dfbe&clientId=ubfef1bb3-f16e-4&from=paste&height=101&id=ue4b702cd&originHeight=152&originWidth=813&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88077&status=done&style=none&taskId=u9064b52d-500d-404f-ab13-3fee5de926b&title=&width=542)
注意：
All Objects后面的Size大小是浅堆大小
Record Objects在判断内存泄露的时候使用，可以通过观察Telemetries中的Memory，如果里面出现垃圾回收之后的内存占用逐步提高，这就有可能出现内存泄露问题，所以可以使用Record Objects查看，但是该分析默认不开启，毕竟占用CPU性能太多
### 4. 堆遍历 heap walker
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955705894-d7e01f01-2fcd-4d69-a2ee-0b74726110bd.png#averageHue=%23eee8e6&clientId=ubfef1bb3-f16e-4&from=paste&height=202&id=ue1ebceaf&originHeight=303&originWidth=1147&originalType=binary&ratio=1&rotation=0&showTitle=false&size=55865&status=done&style=none&taskId=uca2ced93-392c-4952-8e6d-a7148908ab8&title=&width=764.6666666666666)
之后进行溯源，操作如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955718830-37d468aa-c347-47c8-8a38-0b2351997aa2.png#averageHue=%23f1efed&clientId=ubfef1bb3-f16e-4&from=paste&height=397&id=u8ec72583&originHeight=596&originWidth=1324&originalType=binary&ratio=1&rotation=0&showTitle=false&size=174257&status=done&style=none&taskId=ubda055e6-2929-4253-9dc5-918380e68f1&title=&width=882.6666666666666)
查看结果，并根据结果去看对应的图表：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955730201-f301d261-d0c2-42e2-81b7-1a35d1fdf9bb.png#averageHue=%23e2ac59&clientId=ubfef1bb3-f16e-4&from=paste&height=189&id=u3f1b3f02&originHeight=284&originWidth=1532&originalType=binary&ratio=1&rotation=0&showTitle=false&size=86120&status=done&style=none&taskId=uf64f01a1-d0d4-4d1c-9898-ba253f8fe9b&title=&width=1021.3333333333334)
以下是图表的展示情况：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955739943-2abc5d74-e5fa-40f6-a278-689e96614f14.png#averageHue=%23f6f4f3&clientId=ubfef1bb3-f16e-4&from=paste&height=348&id=ue6cf18bf&originHeight=522&originWidth=1829&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69910&status=done&style=none&taskId=u01c357e5-b1bd-413a-88d3-df148977a52&title=&width=1219.3333333333333)
### 5. cpu视图 cpu views
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955775441-af3875ff-1b36-4ee8-8648-5c8fcdc6e048.png#averageHue=%23bfdcbb&clientId=ubfef1bb3-f16e-4&from=paste&height=243&id=u34fad11a&originHeight=365&originWidth=844&originalType=binary&ratio=1&rotation=0&showTitle=false&size=317673&status=done&style=none&taskId=udb2a2ebc-dcbd-4a71-a8d2-d0fb086b729&title=&width=562.6666666666666)
具体使用：
1、记录方法统计信息
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955787393-fceb2651-31f9-4373-9c13-0a9a62ab4598.png#averageHue=%23f7f5f4&clientId=ubfef1bb3-f16e-4&from=paste&height=383&id=u077e25ee&originHeight=574&originWidth=708&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53516&status=done&style=none&taskId=u25337e87-4655-440a-b606-a816ec99c5f&title=&width=472)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955792809-20b69d37-4e67-4058-92bd-99e1b62e644c.png#averageHue=%23fbf9f8&clientId=ubfef1bb3-f16e-4&from=paste&height=77&id=u5c672ae6&originHeight=115&originWidth=900&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8349&status=done&style=none&taskId=u9963adde-ecee-4a78-8c50-6850dfb1be4&title=&width=600)
2、方法统计
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955805109-2b2bb2d7-c50a-4d10-a770-d04d91bb10e7.png#averageHue=%23f9f8f7&clientId=ubfef1bb3-f16e-4&from=paste&height=237&id=ucc0ceb19&originHeight=356&originWidth=1906&originalType=binary&ratio=1&rotation=0&showTitle=false&size=62947&status=done&style=none&taskId=u23a4c4af-2417-40d2-bb6c-70cea330529&title=&width=1270.6666666666667)
 3、具体分析
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955818508-a2c1f852-0378-4f51-8c1b-79b75237d712.png#averageHue=%23f6f4f3&clientId=ubfef1bb3-f16e-4&from=paste&height=246&id=u62845258&originHeight=369&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=154601&status=done&style=none&taskId=u4b570200-9ed8-41eb-a9de-00b93bbc099&title=&width=1280)
### 6. 线程视图 threads
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955838831-9bb09599-cdf3-4ca1-bc69-b24c71582611.png#averageHue=%23c2dfbe&clientId=ubfef1bb3-f16e-4&from=paste&height=290&id=u0b6c858f&originHeight=435&originWidth=823&originalType=binary&ratio=1&rotation=0&showTitle=false&size=225560&status=done&style=none&taskId=u4e4e48fb-98c8-4f09-927e-242169b8867&title=&width=548.6666666666666)
具体使用：
1、查看线程运行情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955852987-2984a0e7-2871-4624-a64b-c5e01d42ec37.png#averageHue=%23ece2e2&clientId=ubfef1bb3-f16e-4&from=paste&height=361&id=u8b3db014&originHeight=542&originWidth=1916&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83214&status=done&style=none&taskId=uf375023a-4f55-484c-a687-77eb61decbd&title=&width=1277.3333333333333)
2、新建线程dump文件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955864950-257e2e27-01f0-4919-a66b-895f3309958a.png#averageHue=%23f9f8f6&clientId=ubfef1bb3-f16e-4&from=paste&height=352&id=u2def0edd&originHeight=528&originWidth=1090&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79334&status=done&style=none&taskId=u9488d82c-1151-4a98-a220-7e9f307ff2a&title=&width=726.6666666666666)
### 7. 监视器&锁 Monitors&locks
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661955876982-22da40d6-3b85-4086-9eae-454521155fdb.png#averageHue=%23bedaba&clientId=ubfef1bb3-f16e-4&from=paste&height=165&id=ucf8b9dd6&originHeight=248&originWidth=826&originalType=binary&ratio=1&rotation=0&showTitle=false&size=225000&status=done&style=none&taskId=u98c15c15-f03b-4169-80b3-0d95d238770&title=&width=550.6666666666666)
## 7.4 案例分析

1. **案例1**
```java
/**
 * 功能演示测试
 * @author shkstart
 * @create 12:19
 */
public class JProfilerTest {
    public static void main(String[] args) {
        while (true){
            ArrayList list = new ArrayList();
            for (int i = 0; i < 500; i++) {
                Data data = new Data();
                list.add(data);
            }
            try {
                TimeUnit.MILLISECONDS.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
class Data{
    private int size = 10;
    private byte[] buffer = new byte[1024 * 1024];//1mb
    private String info = "hello,atguigu";
}
```

2. 案例2
```java
public class MemoryLeak {

    public static void main(String[] args) {
        while (true) {
            ArrayList beanList = new ArrayList();
            for (int i = 0; i < 500; i++) {
                Bean data = new Bean();
                data.list.add(new byte[1024 * 10]);//10kb
                beanList.add(data);
            }
            try {
                TimeUnit.MILLISECONDS.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

}

class Bean {
    int size = 10;
    String info = "hello,atguigu";
    static ArrayList list = new ArrayList();
}
```
我们通过JProfiler来看一下，如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956315835-a8bdbfa2-fab3-4648-be1e-e4a2f42d8745.png#averageHue=%2301b301&clientId=uc5a1aaca-cde4-4&from=paste&height=469&id=ude3fb897&originHeight=703&originWidth=1908&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47942&status=done&style=none&taskId=ue78862d4-cd80-4f40-9937-dfcbeb25008&title=&width=1272)你可以看到内存一个劲的往上涨，但是就是没有下降的趋势，说明这肯定有问题，过不了多久就会出现OOM，我们来到Live memory中，先标记看一下到底是哪些对象在进行内存增长，等一小下看看会不会触发垃圾回收，如果不触发的话，我们自己来触发垃圾回收，之后观察哪些对象没有被回收掉，如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956345642-72420124-9f46-4cd4-b859-16eb0a547f4b.png#averageHue=%23f4f2f1&clientId=uc5a1aaca-cde4-4&from=paste&height=395&id=u5cd193c7&originHeight=593&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=109057&status=done&style=none&taskId=u17345ee7-4b02-4638-88e6-e5aba3d1cb5&title=&width=1280)
我上面点击了Mark Current，发现有些对象在持续增长，然后点击了一下Run GC，结果如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956358176-5af3bc52-5e64-4297-ab17-f9f8ff8ba712.png#averageHue=%23f3f1ef&clientId=uc5a1aaca-cde4-4&from=paste&height=277&id=u330c7a2c&originHeight=415&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95739&status=done&style=none&taskId=u196f9f93-cc5a-44dd-8d8a-e5cff80622c&title=&width=1280)
可以看出byte[]没有被回收，说明它是有问题的，我们点击Show Selection In Heap Walker，如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956381952-837bfd54-32e6-4374-aaad-068af44cf7c8.png#averageHue=%23f2efed&clientId=uc5a1aaca-cde4-4&from=paste&height=194&id=u33817d36&originHeight=291&originWidth=1407&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47095&status=done&style=none&taskId=ua39d01c9-980c-44b1-85fd-bb6c69426a8&title=&width=938)
然后看一下该对象被谁引用，如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956398073-73a5d01b-6d48-43a0-a26a-14f4df0011b2.png#averageHue=%23f1efed&clientId=uc5a1aaca-cde4-4&from=paste&height=399&id=u351bbefa&originHeight=599&originWidth=1318&originalType=binary&ratio=1&rotation=0&showTitle=false&size=104291&status=done&style=none&taskId=ubf5bb30d-e36a-479c-90df-8b75cfa92cb&title=&width=878.6666666666666)
结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956410362-a9db52ef-0ea5-4b68-ad0b-5cbd5aeb62d5.png#averageHue=%23f2efeb&clientId=uc5a1aaca-cde4-4&from=paste&height=200&id=uf2f25d1e&originHeight=300&originWidth=1039&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50385&status=done&style=none&taskId=u452fedfc-6f4e-48da-85f4-1dd5fdf2bf6&title=&width=692.6666666666666)
可以看出byte[]来自于Bean类是的list中，并且这个list是ArrayList类型的静态集合，所以找到了：static ArrayList list = new ArrayList();
发现list是静态的，这不妥，因为我们的目的是while结束之后Bean对象被回收，并且Bena对象中的所有字段都被回收，但是list是静态的，那就是类的，众所周知，类变量随类而生，随类而灭，因此每次我们往list中添加值，都是往同一个list中添加值，这会造成list不断增大，并且不能回收，所以最终会导致OOM
# 8. Arthas
## 8.1 基本概述

1. **背景**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956541057-f2d64cf7-9f18-4e9e-b3bc-aaacba577cc1.png#averageHue=%23e9b473&clientId=uc5a1aaca-cde4-4&from=paste&height=316&id=ue950b969&originHeight=474&originWidth=679&originalType=binary&ratio=1&rotation=0&showTitle=false&size=233470&status=done&style=none&taskId=u9f8386ad-10aa-400b-975a-9b4fdbe367c&title=&width=452.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956552294-5ebabb84-1a1c-4cf4-a2a3-7e8e8c3788e0.png#averageHue=%23b0d9ad&clientId=uc5a1aaca-cde4-4&from=paste&height=319&id=uc541c0b7&originHeight=479&originWidth=820&originalType=binary&ratio=1&rotation=0&showTitle=false&size=229385&status=done&style=none&taskId=u44ed61bd-f374-4a56-ae9e-15e2d63b77b&title=&width=546.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956623437-bff9b2ef-d4d9-4059-9331-2001b9b676a4.png#averageHue=%23bedaba&clientId=uc5a1aaca-cde4-4&from=paste&height=141&id=u3969e821&originHeight=211&originWidth=825&originalType=binary&ratio=1&rotation=0&showTitle=false&size=203938&status=done&style=none&taskId=u8c580108-745f-491c-9b38-966655e0b79&title=&width=550)

2. **概述**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956660789-b680b95b-3d65-4612-9124-82b490b3372a.png#averageHue=%23bfdcbb&clientId=uc5a1aaca-cde4-4&from=paste&height=279&id=u70525b8b&originHeight=418&originWidth=824&originalType=binary&ratio=1&rotation=0&showTitle=false&size=332036&status=done&style=none&taskId=u19bc53a1-0d01-4185-8c88-45e648d01ff&title=&width=549.3333333333334)

3. **基于哪些工具开发而来**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956697015-59b1022d-4e9c-4c33-8c92-8d44ecfe6e35.png#averageHue=%23bcd8b8&clientId=uc5a1aaca-cde4-4&from=paste&height=315&id=u5f335320&originHeight=472&originWidth=816&originalType=binary&ratio=1&rotation=0&showTitle=false&size=489929&status=done&style=none&taskId=u2f948ee0-3a8d-481c-a693-417155ef678&title=&width=544)

4. **官方使用文档**

[https://arthas.aliyun.com/doc/quick-start.html](https://arthas.aliyun.com/doc/quick-start.html)
## 8.2 安装与使用
### 1. 安装
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956793754-45676754-4ecd-4bf2-92ff-2c53b07505b4.png#averageHue=%23c2dfbe&clientId=uc5a1aaca-cde4-4&from=paste&height=145&id=uc8bf02f3&originHeight=218&originWidth=717&originalType=binary&ratio=1&rotation=0&showTitle=false&size=119900&status=done&style=none&taskId=u6a591708-dcb4-4198-9532-40ed65ce1e9&title=&width=478)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956802551-9995ea7a-9e42-4e1f-8609-245ad421e87a.png#averageHue=%23cde5c9&clientId=uc5a1aaca-cde4-4&from=paste&height=253&id=uc54d6399&originHeight=379&originWidth=826&originalType=binary&ratio=1&rotation=0&showTitle=false&size=269802&status=done&style=none&taskId=u80cd4510-e291-4fd4-83d1-61328afa009&title=&width=550.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956806704-779539b6-02dc-4dc9-b6e7-4ecdba084fda.png#averageHue=%23c3e0be&clientId=uc5a1aaca-cde4-4&from=paste&height=122&id=ubbbab15c&originHeight=183&originWidth=588&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69799&status=done&style=none&taskId=u2e868a19-e7ef-44f0-aa8d-b3010927b02&title=&width=392)
### 2. 工程目录
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956842949-28ca06b3-459d-4a10-b8fc-ceaa4e808f01.png#averageHue=%23bfdbbb&clientId=uc5a1aaca-cde4-4&from=paste&height=287&id=u954af9d0&originHeight=430&originWidth=569&originalType=binary&ratio=1&rotation=0&showTitle=false&size=231161&status=done&style=none&taskId=u574ad07c-4eb0-4feb-80c8-e121e9cb79f&title=&width=379.3333333333333)
### 3. 启动
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956887198-f926b2da-b9db-4318-90a7-94459aa519d5.png#averageHue=%23c2dfbe&clientId=uc5a1aaca-cde4-4&from=paste&height=319&id=uf45c6d1e&originHeight=479&originWidth=815&originalType=binary&ratio=1&rotation=0&showTitle=false&size=285469&status=done&style=none&taskId=u79039088-b7cf-4ccb-919f-ddbd3ec7bce&title=&width=543.3333333333334)
### 4. 查看进程
jps
### 5. 查看日志
cat ~/logs/arthas/arthas.log
### 6. 查看帮助
java -jar arthas-boot.jar -h
### 7. web console
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956955180-fc9f6a86-82e8-4794-80f7-7997486d296a.png#averageHue=%23bcd8b8&clientId=uc5a1aaca-cde4-4&from=paste&height=47&id=u276aa876&originHeight=70&originWidth=823&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80098&status=done&style=none&taskId=uabc73b00-73ff-46b1-a5ee-f6015f85b6d&title=&width=548.6666666666666)
### 8. 退出
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661956984445-37517b9a-ffd4-4650-a2e7-17a4ffe08e11.png#averageHue=%23c2dfbe&clientId=uc5a1aaca-cde4-4&from=paste&height=91&id=ud77c5aa1&originHeight=137&originWidth=817&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89663&status=done&style=none&taskId=uae213040-d7c8-4b07-925a-8b5ee71f7b5&title=&width=544.6666666666666)
## 8.3 相关诊断指令
### 1. 基础指令
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957030019-2b3ad24b-843d-491a-a09d-3b6553f4fdd9.png#averageHue=%23e4e2de&clientId=uc5a1aaca-cde4-4&from=paste&height=216&id=u29aa4c94&originHeight=324&originWidth=663&originalType=binary&ratio=1&rotation=0&showTitle=false&size=186926&status=done&style=none&taskId=ud2712472-f522-4379-bd14-3fc50844c9e&title=&width=442)
### 2. jvm相关
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957093631-4036b380-b43f-4ca4-b3d1-44a6b435485e.png#averageHue=%23e7e5dd&clientId=uc5a1aaca-cde4-4&from=paste&height=241&id=ucb4c7438&originHeight=361&originWidth=536&originalType=binary&ratio=1&rotation=0&showTitle=false&size=182520&status=done&style=none&taskId=u6a4e114f-622f-4564-b36c-4fb68c0ee1a&title=&width=357.3333333333333)
命令列表：[https://arthas.aliyun.com/doc/commands.html#id1](https://arthas.aliyun.com/doc/commands.html#id1)

1. **dashboard**

链接：[https://arthas.aliyun.com/doc/dashboard.html](https://arthas.aliyun.com/doc/dashboard.html)
作用：当前系统的实时数据面板

2. **thread**

链接：[https://arthas.aliyun.com/doc/thread.html](https://arthas.aliyun.com/doc/thread.html)
作用：查看当前线程信息，查看线程的堆栈

3. **jvm**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957193090-c940ccc9-c037-4acd-b3a1-d7ec7dcd2e29.png#averageHue=%23393939&clientId=uc5a1aaca-cde4-4&from=paste&height=343&id=ua4010e77&originHeight=515&originWidth=654&originalType=binary&ratio=1&rotation=0&showTitle=false&size=196421&status=done&style=none&taskId=u5a608f56-6ff1-419c-9074-cb0d778101d&title=&width=436)

4. **其他**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957208992-672b734b-b5d7-4dce-a843-7ef90937479c.png#averageHue=%23c1dbc6&clientId=uc5a1aaca-cde4-4&from=paste&height=303&id=uc1e245be&originHeight=455&originWidth=455&originalType=binary&ratio=1&rotation=0&showTitle=false&size=146323&status=done&style=none&taskId=ucf467c7d-7ed4-4022-824f-91d1e58607b&title=&width=303.3333333333333)
### 3. class/classloader相关

1. sc

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957247201-6da73b21-75f5-4010-8488-8f3ec1dd8b45.png#averageHue=%23bfdac4&clientId=uc5a1aaca-cde4-4&from=paste&height=347&id=u50e492e2&originHeight=520&originWidth=672&originalType=binary&ratio=1&rotation=0&showTitle=false&size=345553&status=done&style=none&taskId=u9efd9d46-38e2-4273-8fb8-c6e68a8e412&title=&width=448)

2. sm

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957261161-5c150eb2-5c48-456a-9cf8-0575c14e3394.png#averageHue=%23c1dcc6&clientId=uc5a1aaca-cde4-4&from=paste&height=164&id=u73db9813&originHeight=246&originWidth=665&originalType=binary&ratio=1&rotation=0&showTitle=false&size=139316&status=done&style=none&taskId=u784da5be-0bc3-49a1-acb3-e317ad197f3&title=&width=443.3333333333333)

3. jad

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957275172-e5c85ea2-658a-4304-b45b-b3b3f6de0aa2.png#averageHue=%23c0dbc5&clientId=uc5a1aaca-cde4-4&from=paste&height=104&id=u224f806e&originHeight=156&originWidth=708&originalType=binary&ratio=1&rotation=0&showTitle=false&size=118962&status=done&style=none&taskId=u89995f25-de38-40a6-9a74-01f91f5e5cd&title=&width=472)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957280821-0ab7fc15-a5f5-42eb-b930-a1ccfa0a39b9.png#averageHue=%23eae8e5&clientId=uc5a1aaca-cde4-4&from=paste&height=292&id=u21f42753&originHeight=438&originWidth=701&originalType=binary&ratio=1&rotation=0&showTitle=false&size=235081&status=done&style=none&taskId=ua9ac5f90-695f-44d4-aa2b-1bc78523024&title=&width=467.3333333333333)

4. mc、redefine

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957293165-07713782-983a-47f7-a0fa-230692f90bf5.png#averageHue=%23bad3bd&clientId=uc5a1aaca-cde4-4&from=paste&height=232&id=ucb9b570a&originHeight=348&originWidth=668&originalType=binary&ratio=1&rotation=0&showTitle=false&size=150152&status=done&style=none&taskId=uf5224e6b-9923-495b-beb3-6019832d2d6&title=&width=445.3333333333333)

5. classloader

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957303949-4272d9d3-2a08-45d6-b051-074ef1cc99ff.png#averageHue=%23c1dbc6&clientId=uc5a1aaca-cde4-4&from=paste&height=161&id=uf48a34d7&originHeight=242&originWidth=746&originalType=binary&ratio=1&rotation=0&showTitle=false&size=162063&status=done&style=none&taskId=u587937ae-e0ac-44fb-80d9-2a53a65c472&title=&width=497.3333333333333)
### 4. monitor/watch/trace相关

1. monitor

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957342074-61e60578-74df-44e6-b581-dd3c21abc955.png#averageHue=%23c4dfc9&clientId=uc5a1aaca-cde4-4&from=paste&height=217&id=u6e963eb0&originHeight=326&originWidth=768&originalType=binary&ratio=1&rotation=0&showTitle=false&size=160944&status=done&style=none&taskId=u934626b1-ba07-43e2-89a2-e226316e13c&title=&width=512)

2. watch

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957352920-ca28633b-2919-4721-b89f-179fc7578f67.png#averageHue=%23c2ddc7&clientId=uc5a1aaca-cde4-4&from=paste&height=310&id=u6dcc5ef5&originHeight=465&originWidth=744&originalType=binary&ratio=1&rotation=0&showTitle=false&size=270949&status=done&style=none&taskId=u57b731c8-3fc6-4995-b689-89ae126e7ad&title=&width=496)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957356732-bbc85089-5ff5-4fd1-956f-fcc218c7ca7e.png#averageHue=%23bdd7c2&clientId=uc5a1aaca-cde4-4&from=paste&height=71&id=ud22e9e0d&originHeight=107&originWidth=752&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99095&status=done&style=none&taskId=u7d6011e4-7f15-49e8-9454-a0045a57ffa&title=&width=501.3333333333333)

3. trace

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957369168-603fb503-78ad-4de9-afed-68945f9de83e.png#averageHue=%23c0dac4&clientId=uc5a1aaca-cde4-4&from=paste&height=166&id=ube807d2a&originHeight=249&originWidth=412&originalType=binary&ratio=1&rotation=0&showTitle=false&size=111408&status=done&style=none&taskId=ua9e4f206-d0e9-4c3e-810d-1c430f6a4b3&title=&width=274.6666666666667)

4. stack
5. tt
### 5. 其他
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957413555-a6eb50fc-4e9f-48c4-ae4a-69aaeb5e10cd.png#averageHue=%23c4dfc9&clientId=uc5a1aaca-cde4-4&from=paste&height=285&id=u2b9805f1&originHeight=427&originWidth=750&originalType=binary&ratio=1&rotation=0&showTitle=false&size=184644&status=done&style=none&taskId=uc03bb563-754e-46f6-8da7-cf143c1c5be&title=&width=500)

1. profiler/火焰图

profiler：[https://arthas.aliyun.com/doc/profiler.html](https://arthas.aliyun.com/doc/profiler.html)

2. options

options：[https://arthas.aliyun.com/doc/options.html](https://arthas.aliyun.com/doc/options.html)

# 9. Java Misssion Control
## 9.1 历史
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957493363-5171c8a6-96d2-498d-b849-5a68b4ae3352.png#averageHue=%23c0dac5&clientId=uc5a1aaca-cde4-4&from=paste&height=255&id=u6052b09f&originHeight=383&originWidth=783&originalType=binary&ratio=1&rotation=0&showTitle=false&size=314923&status=done&style=none&taskId=u7a894b28-2918-464d-a3c7-973b234436f&title=&width=522)
## 9.2 启动
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957506317-a461faf9-7dfc-418a-b746-8da5cd6c457b.png#averageHue=%23e7e8e2&clientId=uc5a1aaca-cde4-4&from=paste&height=151&id=ucf590aa8&originHeight=227&originWidth=599&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97664&status=done&style=none&taskId=u7ddf61b7-e5fa-4408-86ef-0e0fb54a088&title=&width=399.3333333333333)
## 9.3 概述
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957520438-be03199f-6353-456c-8b7d-40be2032732e.png#averageHue=%23bfd9c4&clientId=uc5a1aaca-cde4-4&from=paste&height=168&id=u658ba8a4&originHeight=252&originWidth=800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=237567&status=done&style=none&taskId=ub03357b4-0745-4767-86ad-2ebeb09b1f9&title=&width=533.3333333333334)
## 9.4 功能：实时监控JVM运行时的状态
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957531158-d94aa74a-f09b-4831-9f46-24d428ff4525.png#averageHue=%23c0dac5&clientId=uc5a1aaca-cde4-4&from=paste&height=159&id=u1ce18945&originHeight=238&originWidth=670&originalType=binary&ratio=1&rotation=0&showTitle=false&size=148876&status=done&style=none&taskId=u714e6112-16ec-48c9-bf49-bf8366e42ea&title=&width=446.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957535058-c0acc195-1f2b-4644-813f-dee6770c74cf.png#averageHue=%23ddaa64&clientId=uc5a1aaca-cde4-4&from=paste&height=665&id=u347b1651&originHeight=997&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=174852&status=done&style=none&taskId=udf64709d-f86f-4a13-8dd3-0f27b4db0c5&title=&width=1280)
## 9.5 Java Flight Recorder
### 1. 事件类型
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957563455-9aaa0a47-17a9-4ca1-85ab-8077de3c936d.png#averageHue=%23bdd6c1&clientId=uc5a1aaca-cde4-4&from=paste&height=221&id=ub2d84d0d&originHeight=332&originWidth=790&originalType=binary&ratio=1&rotation=0&showTitle=false&size=333547&status=done&style=none&taskId=uf8703a0b-2af5-4740-aa4e-4a81c66e2d9&title=&width=526.6666666666666)
### 2. 启动方式
方式1：使用-XX:StartFlightRecording=参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957579332-deeedcb5-08f1-4a41-9134-0430a5e59bbe.png#averageHue=%23c2ddc7&clientId=uc5a1aaca-cde4-4&from=paste&height=262&id=u8c68dfd4&originHeight=393&originWidth=832&originalType=binary&ratio=1&rotation=0&showTitle=false&size=282152&status=done&style=none&taskId=u343c7e79-b5b1-451a-9d3c-045e7007360&title=&width=554.6666666666666)
方式2：使用jcmd的JFR.*子命令
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957592389-a43e405e-1b79-433a-8b10-f2fd511cfc2a.png#averageHue=%23c1dcc6&clientId=uc5a1aaca-cde4-4&from=paste&height=211&id=u10775dd4&originHeight=316&originWidth=823&originalType=binary&ratio=1&rotation=0&showTitle=false&size=220903&status=done&style=none&taskId=u740c6fe5-5053-4333-9dca-bc5a9335c97&title=&width=548.6666666666666)
方式3：JMC的JFR插件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957602708-7fd5b4e8-d312-4b7b-a2e8-7d6054561cf8.png#averageHue=%23daded9&clientId=uc5a1aaca-cde4-4&from=paste&height=163&id=uc134af13&originHeight=245&originWidth=545&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112744&status=done&style=none&taskId=u3789e096-9394-46cd-b6c8-7eed80af4ce&title=&width=363.3333333333333)
 具体使用：
 1、启动飞行记录仪
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957615422-04ba9828-4bf8-40b2-b0f6-29406cac31bb.png#averageHue=%23edeceb&clientId=uc5a1aaca-cde4-4&from=paste&height=503&id=u4bf37381&originHeight=754&originWidth=813&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66137&status=done&style=none&taskId=u08892fa3-b1a8-4282-a299-2a7ebf0220b&title=&width=542)
2、启动飞行记录
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957628914-d0a01ceb-2789-4654-8670-ab25f066ebed.png#averageHue=%23ecebeb&clientId=uc5a1aaca-cde4-4&from=paste&height=489&id=uce54bb81&originHeight=733&originWidth=810&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58713&status=done&style=none&taskId=uca3f1213-af41-4ae1-ad8e-2c32e804e70&title=&width=540)
3、 正式启动
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957637044-0312b17b-40f7-4d24-a610-68e861b5dd15.png#averageHue=%23efefee&clientId=uc5a1aaca-cde4-4&from=paste&height=503&id=ua614a019&originHeight=754&originWidth=813&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72348&status=done&style=none&taskId=u9c830a45-7b2d-418c-90f6-fbdd422d893&title=&width=542)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957644662-409282b0-17e9-4433-be27-9108fe15ecba.png#averageHue=%23f0f0ef&clientId=uc5a1aaca-cde4-4&from=paste&height=503&id=u75858e0b&originHeight=755&originWidth=815&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93216&status=done&style=none&taskId=uf703a0cc-51b1-4ea6-918e-5efdf1b4b68&title=&width=543.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957651202-2c956b80-38e2-4747-8893-8ae7a79240f5.png#averageHue=%23efeeed&clientId=uc5a1aaca-cde4-4&from=paste&height=496&id=ufea1d8ce&originHeight=744&originWidth=788&originalType=binary&ratio=1&rotation=0&showTitle=false&size=77831&status=done&style=none&taskId=ub657e11f-df83-4b20-8963-8e55e3e7ecb&title=&width=525.3333333333334)
### 3. Java Flight Recorder 取样分析
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957672412-e27da3b8-a251-4e13-81e9-d482d84535f6.png#averageHue=%23c6dfca&clientId=uc5a1aaca-cde4-4&from=paste&height=198&id=u50ac2321&originHeight=297&originWidth=541&originalType=binary&ratio=1&rotation=0&showTitle=false&size=116493&status=done&style=none&taskId=u64d9ae9b-e183-47e2-b437-e0863180edc&title=&width=360.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957675967-a3f2083a-4a9d-49b7-a380-dfaa21703d2c.png#averageHue=%23bcd5c0&clientId=uc5a1aaca-cde4-4&from=paste&height=163&id=u06c4bc84&originHeight=244&originWidth=824&originalType=binary&ratio=1&rotation=0&showTitle=false&size=228088&status=done&style=none&taskId=u70c97b1e-1254-4afe-ac0e-966247f7ba1&title=&width=549.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957679582-a1a622d7-e27b-4476-90a5-a69c020e33c6.png#averageHue=%23cae2ce&clientId=uc5a1aaca-cde4-4&from=paste&height=355&id=u4c18b85c&originHeight=532&originWidth=834&originalType=binary&ratio=1&rotation=0&showTitle=false&size=229124&status=done&style=none&taskId=ud912d141-4780-4469-9047-0ca4c2f3a99&title=&width=556)

1. 代码
```java
/**
 * -Xms600m -Xmx600m -XX:SurvivorRatio=8
 * @author shkstart  shkstart@126.com
 * @create 2020  21:12
 */
public class OOMTest {
    public static void main(String[] args) {
        ArrayList<Picture> list = new ArrayList<>();
        while(true){
            try {
                Thread.sleep(5);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            list.add(new Picture(new Random().nextInt(100 * 50)));
        }
    }
}

class Picture{
    private byte[] pixels;

    public Picture(int length) {
        this.pixels = new byte[length];
    }

    public byte[] getPixels() {
        return pixels;
    }

    public void setPixels(byte[] pixels) {
        this.pixels = pixels;
    }
}
```
结果
1、一般信息
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957718936-5acc5d44-2c8b-491e-9233-9b31e263096e.png#averageHue=%23e9b06e&clientId=uc5a1aaca-cde4-4&from=paste&height=597&id=u01ff6573&originHeight=896&originWidth=1491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=186012&status=done&style=none&taskId=u9520e2d1-0aec-46b0-8562-8c20019453b&title=&width=994)
2、内存
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957726394-5c068817-d948-4105-b750-f779b3e8e354.png#averageHue=%23d7a45b&clientId=uc5a1aaca-cde4-4&from=paste&height=599&id=ue0b20c4d&originHeight=898&originWidth=1491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=131684&status=done&style=none&taskId=u8c9aa81d-363f-494d-a0f1-e3a9c76372a&title=&width=994)
3、代码
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957735790-e6dc8de3-6e3f-421e-a24e-d0f6506916c6.png#averageHue=%23d0a966&clientId=uc5a1aaca-cde4-4&from=paste&height=597&id=ua7bbf297&originHeight=896&originWidth=1491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=116230&status=done&style=none&taskId=u1d43fd9a-11e1-4fe0-a283-dc172d86143&title=&width=994)
4、线程
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957746768-f647a297-75a7-4f8c-9460-c05350dc6abb.png#averageHue=%23eaaa6a&clientId=uc5a1aaca-cde4-4&from=paste&height=597&id=u58ca88ca&originHeight=896&originWidth=1488&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129736&status=done&style=none&taskId=uee9ae7d7-1f18-4964-83a2-f6c4b0353be&title=&width=992)
5、I/O
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957756819-700b1e18-f662-40cd-b5b3-81bd21dcefd7.png#averageHue=%23d4b072&clientId=uc5a1aaca-cde4-4&from=paste&height=598&id=udbe679ff&originHeight=897&originWidth=1486&originalType=binary&ratio=1&rotation=0&showTitle=false&size=122895&status=done&style=none&taskId=u32dca497-9b17-4a14-8998-5ce25238e43&title=&width=990.6666666666666)
6、系统
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957765914-55a7f5be-1e65-4522-88f9-c82003f4a1b6.png#averageHue=%23ddad66&clientId=uc5a1aaca-cde4-4&from=paste&height=597&id=u9b775aa1&originHeight=896&originWidth=1491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=159585&status=done&style=none&taskId=ub98b0f44-b094-493d-baf8-7f23268fce7&title=&width=994)
7、事件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957774578-37776ab3-26c0-41ae-8ed3-f08f16e2efad.png#averageHue=%23f2cb50&clientId=uc5a1aaca-cde4-4&from=paste&height=597&id=u7fa91b02&originHeight=896&originWidth=1491&originalType=binary&ratio=1&rotation=0&showTitle=false&size=134104&status=done&style=none&taskId=ud233c93c-a4f8-4512-a9ec-ca321e47bac&title=&width=994)
# 10. 其他工具

1. **Flame Graphs（火焰图）**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957819726-145438ee-b3fd-4a8f-a152-fd9ab3410e3c.png#averageHue=%23e8e8cb&clientId=uc5a1aaca-cde4-4&from=paste&height=477&id=u3961dc96&originHeight=715&originWidth=841&originalType=binary&ratio=1&rotation=0&showTitle=false&size=275550&status=done&style=none&taskId=u958558b8-bed0-4806-8fa5-ecf3d64cc4b&title=&width=560.6666666666666)

2. **Tprofiler**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957838998-6c016fde-6ed0-4fc0-bfee-c0ab6ace4035.png#averageHue=%23c0dac5&clientId=uc5a1aaca-cde4-4&from=paste&height=314&id=ud02db852&originHeight=471&originWidth=834&originalType=binary&ratio=1&rotation=0&showTitle=false&size=443014&status=done&style=none&taskId=uc9f2401e-4ebd-4dd5-93eb-d4151d22a95&title=&width=556)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957862076-7556dd1b-e2f7-4eb3-9e2f-2826876ab49b.png#averageHue=%23bdd6c1&clientId=uc5a1aaca-cde4-4&from=paste&height=38&id=u44e17988&originHeight=57&originWidth=399&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27858&status=done&style=none&taskId=u75bbde1e-5b19-4abb-aca9-ed884bbc21f&title=&width=266)

3. **Btrace**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661957873841-70bd82aa-2969-4a67-8499-d954f0f16a3a.png#averageHue=%23c0dac4&clientId=uc5a1aaca-cde4-4&from=paste&height=271&id=ue2c7501b&originHeight=407&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=372988&status=done&style=none&taskId=uf6b9ca0e-c0bc-4495-a909-254ca62610f&title=&width=555.3333333333334)

4. **YourKit**
5. **JProbe**
6. **Spring Insight**
