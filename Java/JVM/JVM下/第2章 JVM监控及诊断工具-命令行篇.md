# 1. 概述
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661528809495-0fa00b49-864b-40d2-8d15-49d04277e50a.png#averageHue=%23c1dbc5&clientId=u486d9a16-fef0-4&from=paste&height=195&id=uc688ad74&originHeight=292&originWidth=687&originalType=binary&ratio=1&rotation=0&showTitle=false&size=215030&status=done&style=none&taskId=u0eafa28b-a532-4fc7-9ac1-8413705bffe&title=&width=458)
简单的命令行工具
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661528826964-50191602-f799-456a-9759-b175dc69a9b7.png#averageHue=%23bbd5c0&clientId=u486d9a16-fef0-4&from=paste&height=93&id=u772ff87b&originHeight=139&originWidth=701&originalType=binary&ratio=1&rotation=0&showTitle=false&size=141732&status=done&style=none&taskId=u3482998e-198b-44d4-9846-3cb39744447&title=&width=467.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661528831636-4ee8130c-2efa-4151-a878-17e53c142af8.png#averageHue=%23ebece8&clientId=u486d9a16-fef0-4&from=paste&height=328&id=u35bf3dc1&originHeight=492&originWidth=711&originalType=binary&ratio=1&rotation=0&showTitle=false&size=193330&status=done&style=none&taskId=ua959d76d-7da5-4560-b359-caee1d2430b&title=&width=474)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661528836038-f98fee80-de66-4365-b5ad-66f72228c173.png#averageHue=%23eae8e2&clientId=u486d9a16-fef0-4&from=paste&height=348&id=u3e2caa4a&originHeight=522&originWidth=524&originalType=binary&ratio=1&rotation=0&showTitle=false&size=224198&status=done&style=none&taskId=ucf3f344f-4af7-47f1-ae1b-209a12bde66&title=&width=349.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661528839560-4341bb8f-cd3d-4698-bc9a-9fc21bd55da6.png#averageHue=%23bfd9c4&clientId=u486d9a16-fef0-4&from=paste&height=55&id=u5040d7e3&originHeight=83&originWidth=709&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53134&status=done&style=none&taskId=u85d25f6e-04ad-4d06-bfd6-69944660c6d&title=&width=472.6666666666667)
# 2. jps：查看正在运行的Java进程
## 1. 基本情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661530161964-891aa6e8-72bd-4e1e-8175-a7e95d886edc.png#averageHue=%23c2ddc7&clientId=u486d9a16-fef0-4&from=paste&height=132&id=ue7ed9052&originHeight=198&originWidth=688&originalType=binary&ratio=1&rotation=0&showTitle=false&size=104190&status=done&style=none&taskId=u9e564ee9-dc32-4060-84b1-9b447260cd2&title=&width=458.6666666666667)
## 2. 基本语法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661530219190-c134727d-335c-44d0-80a6-e743e01ff2ab.png#averageHue=%23c0dbc5&clientId=u486d9a16-fef0-4&from=paste&height=82&id=u4bcc341c&originHeight=123&originWidth=394&originalType=binary&ratio=1&rotation=0&showTitle=false&size=43614&status=done&style=none&taskId=u3cfbba02-398d-4edb-9e19-d3d5dc4f1f3&title=&width=262.6666666666667)
可以通过 jps -help 来查看对应的参数信息
### 2.1 options参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661530271455-1a8902b2-67a2-4b99-af39-addd6dfbc171.png#averageHue=%23c2ddc7&clientId=u486d9a16-fef0-4&from=paste&height=299&id=u1d04ebc1&originHeight=448&originWidth=689&originalType=binary&ratio=1&rotation=0&showTitle=false&size=233211&status=done&style=none&taskId=uc810bbb3-f255-4fe8-b668-56626762ec5&title=&width=459.3333333333333)
**综合使用：**
jps -l -m等价于jps -lm
**如何将信息输出到同级文件中：**
语法：命令 > 文件名称
例如：jps -l > a.tx
### 2.2 hostid参数
 ![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661530357132-a832d071-afa5-41a5-89f5-22501133ce58.png#averageHue=%23bfd9c4&clientId=u486d9a16-fef0-4&from=paste&height=159&id=u05af1e1f&originHeight=238&originWidth=693&originalType=binary&ratio=1&rotation=0&showTitle=false&size=172876&status=done&style=none&taskId=u0cf6e8e7-67e0-482c-8d15-74ee9034b36&title=&width=462)
# 3. jstat：查看JVM统计信息
## 3.1 基本情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661586024654-87e4969d-44c4-4a2a-bbcb-7515d07f4fff.png#averageHue=%23c1dcc6&clientId=u363823d1-9f82-4&from=paste&height=183&id=uc11ed1f1&originHeight=275&originWidth=692&originalType=binary&ratio=1&rotation=0&showTitle=false&size=194904&status=done&style=none&taskId=uced632d8-47ed-4397-bed8-b078a37fad6&title=&width=461.3333333333333)
## 3.2 基本语法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661586051312-abb21ebd-85be-47e1-94af-afef3ca0421b.png#averageHue=%23c5e0ca&clientId=u363823d1-9f82-4&from=paste&height=115&id=u92bdc7ce&originHeight=173&originWidth=659&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64011&status=done&style=none&taskId=uae27b4fb-cf9e-443c-b9ec-000c382e677&title=&width=439.3333333333333)
其中vmid是进程id号，也就是jps之后看到的前面的号码，如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661586065486-d23eafc1-d836-435a-a3e7-e03b100c32a9.png#averageHue=%23181513&clientId=u363823d1-9f82-4&from=paste&height=127&id=u76c80308&originHeight=191&originWidth=360&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14882&status=done&style=none&taskId=u7a336915-5e62-437a-a9b6-3836b804f71&title=&width=240)
### 1. option参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661586658961-9a42f5ef-bb0a-450c-a688-3337c440a1f6.png#averageHue=%23c3ddc7&clientId=u363823d1-9f82-4&from=paste&height=362&id=ub30f2915&originHeight=543&originWidth=694&originalType=binary&ratio=1&rotation=0&showTitle=false&size=324469&status=done&style=none&taskId=u89fff08b-b1cf-40fc-a62f-b8243f837f9&title=&width=462.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661586662704-656f1ec7-88bd-418b-9db3-b9885b3ba517.png#averageHue=%23c3dfc6&clientId=u363823d1-9f82-4&from=paste&height=135&id=u40dd7d6a&originHeight=202&originWidth=687&originalType=binary&ratio=1&rotation=0&showTitle=false&size=119549&status=done&style=none&taskId=u5298aa88-a534-491b-ae9e-0182a49b8bb&title=&width=458)

1. **-class举例**：jstat -class -t -h3 13152 1000 10，其中h3中的3代表每隔3个分隔一次，13152代表类的进程id，1000代表每隔1000毫秒打印一次，10代表一共打印10次，如下所示：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661586673032-942ebc93-0b71-4294-8616-25b80908aeba.png#averageHue=%23131110&clientId=u363823d1-9f82-4&from=paste&height=275&id=u399999c3&originHeight=413&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45251&status=done&style=none&taskId=u46f648f5-91d9-4b59-a293-117ee3369f2&title=&width=555.3333333333334)
其中Timestamp代表程序至今的运行时间，单位为秒；Loaded代表加载的类的数目；Bytes代表加载的类的总字节数；Unloaded代表卸载的类的数目；Bytes代表卸载的类的总字节数；Time代表类装载所消耗的时间； 

2. **-gc举例**：jstat -gc 9936，其中13152代表类的进程id，执行结果如下所示：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661598593604-72571016-610a-4756-9907-2ba97655608f.png#averageHue=%23f9f7f6&clientId=u8387ef3b-f5aa-4&from=paste&height=91&id=u037443dc&originHeight=136&originWidth=2107&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21704&status=done&style=none&taskId=ua801bca1-9190-43cf-a8f3-b74724b2b3f&title=&width=1404.6666666666667)
其中S0C代表幸存者0区的总容量，S1C代表幸存者1区的总容量，S0U代表幸存者0区使用的容量，S1U代表幸存者1区使用的容量，EC代表伊甸园区的总容量，EU代表伊甸园区使用的总容量，OC代表老年代的总容量，OU代表老年代已经使用的容量，MC代表方法区的总容量，MU代表方法区的总容量，CCSC代表压缩类的总容量，CCSU代表压缩类使用的容量，YGC代表年轻代垃圾回收的次数，YGCT年轻代进行垃圾回收需要的时间，FGC代表代表Full GC的次数，FGCT代表Full GC的时间，GCT代表垃圾回收的总时间

3. -gccapacity举例：jstat -gccapacity 13152，其中13152代表类的进程id，执行结果如下：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661598822862-9fce05f9-7c0a-467f-b7b3-b466b39f8616.png#averageHue=%2311100f&clientId=u8387ef3b-f5aa-4&from=paste&height=95&id=u0ec1c3ce&originHeight=142&originWidth=1892&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19317&status=done&style=none&taskId=u306ea743-6b0b-43ec-9cd9-53e22e24b26&title=&width=1261.3333333333333)

4. -gcutil举例：jstat -gcutil 13152，其中13152代表类的进程id，执行结果如下所示：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661598966638-e7b042d5-76c9-4df2-a9f0-590db4fa5db5.png#averageHue=%23131110&clientId=u8387ef3b-f5aa-4&from=paste&height=57&id=u4fe6b856&originHeight=85&originWidth=1146&originalType=binary&ratio=1&rotation=0&showTitle=false&size=9864&status=done&style=none&taskId=ufcf0ae18-2712-405d-9358-fb1ea94dffd&title=&width=764)
 以上是各区域占比以及垃圾回收的情况，S0代表幸存者0区，S1代表幸存者1区，E代表伊甸园区，O代表老年代，M代表方法区，CCS代表压缩类，以上这些值都是占比情况，YGC代表年轻代垃圾回收的次数，YGCT年轻代进行垃圾回收需要的时间，FGC代表代表Full GC的次数，FGCT代表Full GC的时间，GCT代表垃圾回收的总时间

5.  -gccause举例：jstat -gccause 13152，其中13152代表类的进程id，执行结果如下：

 以上是各区域占比以及垃圾回收的情况，还有触发垃圾回收的原因解释，S0代表幸存者0区，S1代表幸存者1区，E代表伊甸园区，O代表老年代，M代表方法区，CCS代表压缩类，以上这些值都是占比情况，YGC代表年轻代垃圾回收的次数，YGCT年轻代进行垃圾回收需要的时间，FGC代表代表Full GC的次数，FGCT代表Full GC的时间，GCT代表垃圾回收的总时间，LGCC和GCC代表垃圾回收的原因
### 2. interval参数
用于指定输出统计数据的周期，单位为毫秒。即：查询间隔
### 3. count参数
用于指定查询的总次数
### 4. -t 参数
可以在输出信息前加上一个Timestamp列，显示程序的运行时间。单位：秒
**经验：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661608081234-d686b87b-0607-4fe5-bf05-bb04b4cd1b87.png#averageHue=%23bfd9c3&clientId=u2428b815-7a73-4&from=paste&height=98&id=u2d437b90&originHeight=147&originWidth=773&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125531&status=done&style=none&taskId=u6d9ed486-0940-47bb-841c-44937d935f4&title=&width=515.3333333333334)
 我们执行jstat -gc -t 13152 1000 10，这代表1秒打印出1行，一共10行，-t代表打印出Timestamp总运行时间，结果如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661608093358-a4f38e74-615c-4548-8383-bccda0c07047.png#clientId=u2428b815-7a73-4&from=paste&height=425&id=u5cd1d13e&originHeight=637&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76557&status=done&style=none&taskId=ufdf03a77-9d1b-4d7f-a43e-ee99050f54d&title=&width=1280)
上方红色框框中代表Timestamp，而蓝色框框中代表垃圾回收时间，单位都是秒，如果让红色框框中的某两个值相减，假设这个值是num1，然后让对应行的蓝色框框中的另外两个值相减，假设这个值是num2，之后让num2/num1，得出的差值就是上述所说的GC时间占运行时间的比例
虽然这种方式比较繁琐，但是在项目部署之后就需要使用命令行去看了，就没有可视化界面了，所以这种方式也要会
### 5. -h参数
可以在周期性数据输出时，输出多少行数据后输出一个表头信息
## 3. 补充
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661609159130-9a364a2f-be6f-4752-8731-5d5ef8b625f6.png#clientId=u2428b815-7a73-4&from=paste&height=181&id=u4b4bc2af&originHeight=272&originWidth=770&originalType=binary&ratio=1&rotation=0&showTitle=false&size=186424&status=done&style=none&taskId=u5dd1e8b8-ddf9-4a98-a3ee-55de9bb3e2b&title=&width=513.3333333333334)
第1步可以执行命令：jstat -gc -t 13152 1000 10
# 4. jinfo：实时查看和修改JVM配置参数
## 1.基本情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661611419427-68fed068-046e-4937-aa7b-377862d05024.png#clientId=u2428b815-7a73-4&from=paste&height=142&id=u80a7e27f&originHeight=213&originWidth=773&originalType=binary&ratio=1&rotation=0&showTitle=false&size=177643&status=done&style=none&taskId=ub7d8ea0b-06bf-4029-978a-aa6b9bdcb48&title=&width=515.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661611423645-fa3743fd-97be-4d29-81e2-ffc6bfeb02db.png#clientId=u2428b815-7a73-4&from=paste&height=295&id=u3d66406a&originHeight=442&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=206069&status=done&style=none&taskId=u8ba37d61-b4f3-42fa-9416-b0de665a30a&title=&width=484.6666666666667)
## 2. 基本语法
### 2.1 查看

1. **jinfo -sysprops 进程id**

进程id可以通过jps命令查看，操作结果如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661611546596-797c4cc2-5c6b-4736-aa41-ddd1a40d3c34.png#clientId=u2428b815-7a73-4&from=paste&height=581&id=u4804d579&originHeight=871&originWidth=1265&originalType=binary&ratio=1&rotation=0&showTitle=false&size=127337&status=done&style=none&taskId=ucd6153e8-64de-4abb-8394-595e01216f7&title=&width=843.3333333333334)
其中13152代表进程id

2. **jinfo -flags 进程id**

进程id可以通过jps命令查看，参数赋值的一部分是我们自己设置的，另外一部分是系统自动优化设置的参数信息，具体操作如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661611649115-acc20d9b-1e8b-48ee-893b-d70e5ce6cd7d.png#clientId=u2428b815-7a73-4&from=paste&height=131&id=u5aa59e84&originHeight=196&originWidth=1893&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38556&status=done&style=none&taskId=ue7251172-45bb-4227-a15c-01a5c930a0a&title=&width=1262)

3. **jinfo -flag 参数名称 进程id**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661611780884-7039b5d3-c716-4cf6-b44e-3885ecef250d.png#clientId=u2428b815-7a73-4&from=paste&height=134&id=ud97feacf&originHeight=201&originWidth=559&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18187&status=done&style=none&taskId=u0581c966-6b8b-46b9-9569-674765b657b&title=&width=372.6666666666667)
### 3. 修改
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661612331655-7db84870-8d5b-4f02-9c11-9421ce8797da.png#clientId=u2428b815-7a73-4&from=paste&height=107&id=u70ad54f5&originHeight=161&originWidth=769&originalType=binary&ratio=1&rotation=0&showTitle=false&size=110767&status=done&style=none&taskId=ua9f923a3-70c9-4a48-a089-629fd32c8ef&title=&width=512.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661612334724-244fac9d-d6aa-4c63-b042-afa1d9bf4c59.png#clientId=u2428b815-7a73-4&from=paste&height=309&id=u18d704d9&originHeight=463&originWidth=775&originalType=binary&ratio=1&rotation=0&showTitle=false&size=279539&status=done&style=none&taskId=u370d556e-7228-4742-92f6-5879d5de333&title=&width=516.6666666666666)
## 3. 拓展

1. **java -XX:+PrintFlagsInitial**

查看所有JVM参数启动的初始值

2. **java -XX:+PrintFlagsFinal**

查看所有JVM参数的最终值
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661662994562-41da1f28-4460-4436-8b92-091407c7ea10.png#clientId=u30c9b10a-fd31-4&from=paste&height=117&id=uce770faf&originHeight=175&originWidth=1226&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35204&status=done&style=none&taskId=u80b3dbdb-bc59-4a21-a498-90111ae877a&title=&width=817.3333333333334)
值前面添加冒号:的是修改之后的值，没有添加的都是没有发生改变的初始值

3. **java -参数名称:+PrintCommandLineFlags**

查看那些已经被用户或者JVM设置过的详细的XX参数的名称和值
# 5.  jmap：导出内存映像文件&内存使用情况
## 5.1 基本情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661783760909-ac827c51-16c7-4d42-b46c-f0217cde2197.png#clientId=u225af7b5-598d-4&from=paste&height=179&id=u7d72cd49&originHeight=269&originWidth=762&originalType=binary&ratio=1&rotation=0&showTitle=false&size=180493&status=done&style=none&taskId=uae3eef50-6cac-444e-ac58-1f96cb05179&title=&width=508)
## 5.2 基本语法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661783818325-c9d108e1-9059-48b7-9f0d-f3271f4acf51.png#clientId=u225af7b5-598d-4&from=paste&height=305&id=u91551615&originHeight=457&originWidth=730&originalType=binary&ratio=1&rotation=0&showTitle=false&size=226733&status=done&style=none&taskId=u872b1b5f-5a43-4209-b078-dee58e77c81&title=&width=486.6666666666667)

1. 使用语法可以通过在DOS窗口中使用jmap/jmap -h/jmap -help查看
2. <executable <core>代表可执行的代码，比如使用> 文件名称来指定生成的dump文件的生成位置
3. [server_id@]<……>是为远程连接准备的




1. **-dump**

生成Java堆转储快照：dump文件
特别的：-dump:live只保存堆中的存活对象

2. -**heap**

输出整个堆空间的详细信息，包括GC的使用、堆配置信息，以及内存的使用信息等

3. -**histo**

输出堆中对象的同级信息，包括类、实例数量和合计容量
特别的：-histo:live只统计堆中的存活对象

4. **-permstat**

以ClassLoader为统计口径输出永久代的内存状态信息
仅linux/solaris平台有效

5. **-finalizerinfo**

显示在F-Queue中等待Finalizer线程执行finalize方法的对象
仅linux/solaris平台有效

6. -F

当虚拟机进程对-dump选项没有任何响应时，可使用此选项强制执行生成dump文件
仅linux/solaris平台有效

7. -h | -help

jamp工具使用的帮助命令

8. -J <flag>

传递参数给jmap启动的jvm
## 5.3 使用1：导出内存映像文件
```java
public class GCTest {
    public static void main(String[] args) throws InterruptedException {
         List<byte[]> list = new ArrayList<>();
        for (int i = 0; i < 1000; i++) {
            byte[] arr = new byte[1024 * 100];
            list.add(arr);
            Thread.sleep(200);
        }

    }
}
```
#### 1. 手动的方式
> jmap -dump:format=b,file=<filename.hprof> <pid>

** 说明：**
<filename.hprof>中的filename是文件名称，而.hprof是后缀名，<***>代表该值可以省略<>，当然后面的<pid>是进程id，需要通过jps查询出来
format=b表示生成的是标准的dump文件，用来进行格式限定
具体例子如下：
生成堆中所有对象的快照：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865246464-d3edd079-67fe-4ae1-96ec-359d984eb464.png#clientId=ub23ac634-d62c-4&from=paste&height=28&id=u28cca23d&originHeight=42&originWidth=587&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6130&status=done&style=none&taskId=u70faa313-2ef7-4a78-81e6-0857825323c&title=&width=391.3333333333333)
生成堆中存活对象的快照：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865261066-2bb97eeb-5f53-4bb9-9b30-028c49df1026.png#clientId=ub23ac634-d62c-4&from=paste&height=25&id=ua72854cb&originHeight=38&originWidth=588&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6601&status=done&style=none&taskId=u3de3f3a9-60ed-4602-a0fa-fa044a4fc37&title=&width=392)
其中file=后面的是生成的dump文件地址，最后的11696是进程id，可以通过jps查看

一般使用的是第二种方式，也就是生成堆中存活对象的快照，毕竟这种方式生成的dump文件更小，我们传输处理都更方便
> jmap -dump:live,format=b,file=<filename.hprof> <pid>

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865300510-9b4edae6-6a44-477e-bad0-a2000e6cccc4.png#clientId=ub23ac634-d62c-4&from=paste&height=195&id=u58e8d5db&originHeight=293&originWidth=763&originalType=binary&ratio=1&rotation=0&showTitle=false&size=244592&status=done&style=none&taskId=uda690ed9-fcaa-4682-8260-34e65e123a3&title=&width=508.6666666666667)
#### 2. 自动的方式
-XX:+HeapDumpOnOutOfMemoryError
-XX:HeapDumpPath=<filename.hprof>
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865347113-dc85ea79-c1ab-455d-abc3-733987f570e7.png#clientId=ub23ac634-d62c-4&from=paste&height=183&id=ua305c6de&originHeight=275&originWidth=755&originalType=binary&ratio=1&rotation=0&showTitle=false&size=205206&status=done&style=none&taskId=uf07c0ca2-a6cd-4396-8412-ef59fa55099&title=&width=503.3333333333333)
具体使用如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865350840-b4571124-5d9e-4914-b4ec-8a4a7907d0a2.png#clientId=ub23ac634-d62c-4&from=paste&height=451&id=u6a5478a9&originHeight=676&originWidth=949&originalType=binary&ratio=1&rotation=0&showTitle=false&size=233844&status=done&style=none&taskId=ub8bed307-322f-494c-ab10-42560d3f7ec&title=&width=632.6666666666666)

### 5.4 使用2：显示堆内存相关信息
> jmap -heap 进程id

jmap -heap 进程id只是时间点上的堆信息，而jstat后面可以添加参数，可以指定时间动态观察数据改变情况，而图形化界面工具，例如jvisualvm等，它们可以用图表的方式动态展示出相关信息，更加直观明了
例子如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865496441-66bfff44-a1c9-405c-8825-dc2371cad6aa.png#clientId=ub23ac634-d62c-4&from=paste&id=uda219a57&originHeight=25&originWidth=153&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4257&status=done&style=none&taskId=u99b5983d-8e1d-427d-a647-a0c4a0726be&title=)
> jmap -histo 进程id

输出堆中对象的同级信息，包括类、实例数量和合计容量，也是这一时刻的内存中的对象信息
例子如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865523981-480f010b-92c2-4017-b402-1198ebde0e95.png#clientId=ub23ac634-d62c-4&from=paste&height=11&id=ub4c4986e&originHeight=16&originWidth=156&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4125&status=done&style=none&taskId=udcf4a896-7757-47fc-95c8-9bb019328de&title=&width=104)
## 5.4 其他作用
> jmap -permstat 进程id

查看系统的ClassLoader信息
> jmap -finalizerinfo

查看堆积在finalizer队列中的对象
## 5.5 小结
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865576779-990e4073-29d0-43dc-921c-b5e4c2f0693a.png#clientId=ub23ac634-d62c-4&from=paste&height=206&id=u5730cfbd&originHeight=309&originWidth=753&originalType=binary&ratio=1&rotation=0&showTitle=false&size=251441&status=done&style=none&taskId=u85402b2f-a313-4fd4-82e4-48d244fe815&title=&width=502)

# 6. jhat：JDK自带堆分析工具
jhat命令在jdk9及其之后就被移除了，官方建议使用jvisualvm代替jhat，所以该指令只需简单了解一下即可
## 6.1 基本情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865667413-e046af34-3c3c-48fb-a5aa-443a14319950.png#clientId=ub23ac634-d62c-4&from=paste&height=205&id=u0fb2c1c4&originHeight=308&originWidth=759&originalType=binary&ratio=1&rotation=0&showTitle=false&size=206993&status=done&style=none&taskId=ued666705-6eb8-48f6-8358-27c5a665d8d&title=&width=506)
## 6.2 基本语法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865695507-83c78f95-5aa3-44b0-b0bc-5a01389e974d.png#clientId=ub23ac634-d62c-4&from=paste&height=53&id=u0770989a&originHeight=79&originWidth=259&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23166&status=done&style=none&taskId=ud699b7f0-eeb6-442a-97bf-b0c121d41a1&title=&width=172.66666666666666)
其中dumpfile代表dump文件的地址以及名称，例如：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865702382-96662747-9ded-40bd-914b-5071cee86c13.png#clientId=ub23ac634-d62c-4&from=paste&height=12&id=uae307673&originHeight=18&originWidth=112&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2885&status=done&style=none&taskId=u2bee21b7-9b1c-4793-9de7-40461bd88ab&title=&width=74.66666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865707344-c8aa6059-1d9a-4d95-af48-aba295e9ce40.png#clientId=ub23ac634-d62c-4&from=paste&height=276&id=uceed4df9&originHeight=414&originWidth=510&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102273&status=done&style=none&taskId=ud6e4d5a5-5e08-4a2d-9aff-c7b66275e3c&title=&width=340)
举例如下：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865718127-860a7ce7-115a-4b6b-b585-ecc8396f40ec.png#clientId=ub23ac634-d62c-4&from=paste&height=11&id=u05bdc60e&originHeight=16&originWidth=188&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4412&status=done&style=none&taskId=uc43c85cc-1922-49e7-b20b-b78d55f15de&title=&width=125.33333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865721012-b4018f12-bf8d-4ebf-a4a2-2b7a8c2e8e46.png#clientId=ub23ac634-d62c-4&from=paste&height=11&id=u6bf218f2&originHeight=16&originWidth=166&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4356&status=done&style=none&taskId=ubc50104c-3b3b-4ed9-b3e2-4ee07ec7ffe&title=&width=110.66666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865723952-8c52e810-6592-4422-899b-14dcfdb72169.png#clientId=ub23ac634-d62c-4&from=paste&height=11&id=ub9d8b45f&originHeight=16&originWidth=89&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2162&status=done&style=none&taskId=u4e42b76b-8b65-404d-8b0f-bfe6e005cf3&title=&width=59.333333333333336)
# 7. jstack：打印JVM中线程快照
## 7.1 基本情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865764100-642f50fb-ef72-4ee5-9f8a-0f010b1496a7.png#clientId=ub23ac634-d62c-4&from=paste&height=338&id=u05a0f0f6&originHeight=507&originWidth=763&originalType=binary&ratio=1&rotation=0&showTitle=false&size=344950&status=done&style=none&taskId=u94bc675c-4fe9-4604-9cfe-3c99ea9eeb8&title=&width=508.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661865767675-678866bd-076a-41c8-8bbc-852b73c49b57.png#clientId=ub23ac634-d62c-4&from=paste&height=40&id=uc2a0e97d&originHeight=60&originWidth=759&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31507&status=done&style=none&taskId=u1c38e551-fa5b-41a3-8660-5122cffec3a&title=&width=506)
## 7.2 基本语法

1. option参数：-F

当正常输出的请求不被响应时，强制输出线程堆栈

2. option参数：-l

除堆栈外，显示关于锁的附加信息

3. option参数：-m

如果调用本地方法的话，可以显示C/C++的堆栈

4. option参数：-h

帮助操作
# 8. jcmd：多功能命令行
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661866077334-afcbf767-a78b-4d81-9320-e6eee8afd17a.png#clientId=uf13ad775-29f4-4&from=paste&height=262&id=uaad610bb&originHeight=393&originWidth=397&originalType=binary&ratio=1&rotation=0&showTitle=false&size=161754&status=done&style=none&taskId=uf924c724-5cf2-4ca4-baa8-9210b47602b&title=&width=264.6666666666667)
一个顶多个
## 8.1 基本情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661866103962-29b71b95-1ebc-4a46-b6d3-3202e44ddd09.png#clientId=uf13ad775-29f4-4&from=paste&height=173&id=u75915991&originHeight=259&originWidth=759&originalType=binary&ratio=1&rotation=0&showTitle=false&size=171545&status=done&style=none&taskId=u2f6cb442-a26a-490e-9c71-111dd3bac07&title=&width=506)
## 8.2 基本语法

1. **jcmd -l**

列出所有的JVM进程

2. **jcmd 进程号 help**

针对指定的进程，列出支持的所有具体命令
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661866205504-faccbaba-fa0c-46c5-80f4-d35b8296fabd.png#clientId=uf13ad775-29f4-4&from=paste&height=424&id=ud6c20505&originHeight=636&originWidth=446&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45603&status=done&style=none&taskId=ufd391745-663c-4e95-9414-923b18af29f&title=&width=297.3333333333333)

3. **jcmd 进程号 具体命令**

显示指定进程的指令命令的数据
首先通过jcmd 进程号 help得出以下命令列表：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661866272310-f9f80cc5-e40e-4dfe-ad2a-36d2eaac05df.png#clientId=uf13ad775-29f4-4&from=paste&height=425&id=ua7998f95&originHeight=637&originWidth=448&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45661&status=done&style=none&taskId=uc3eae47b-12ee-4548-a5e3-215e2585efe&title=&width=298.6666666666667)
根据以上命令来替换之前的那些操作：
Thread.print 可以替换 jstack指令
GC.class_histogram 可以替换 jmap中的-histo操作
GC.heap_dump 可以替换 jmap中的-dump操作
GC.run 可以查看GC的执行情况
VM.uptime 可以查看程序的总执行时间，可以替换jstat指令中的-t操作
VM.system_properties 可以替换 jinfo -sysprops 进程id
VM.flags 可以获取JVM的配置参数信息
# 9. jstatd：远程主机信息收集
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1661866306494-b42f52d8-02ef-4178-a012-d14ced6002a7.png#clientId=uf13ad775-29f4-4&from=paste&height=255&id=u41f3a7c6&originHeight=382&originWidth=756&originalType=binary&ratio=1&rotation=0&showTitle=false&size=186176&status=done&style=none&taskId=ub38b8720-9ab5-4188-bd5b-a0485b2e7e4&title=&width=504)
