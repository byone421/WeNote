# 1. JVM参数选项
参数来源：[https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html)
## 1.1 类型一：标准参数选项
### 1. 特点
比较稳定，后续版本基本不会变化
> 以  -  开头

### 2. 各种选项
直接在DOS窗口中运行java或者java -help可以看到所有的标准选项
```java
-d32          使用 32 位数据模型 (如果可用)
-d64          使用 64 位数据模型 (如果可用)
-server       选择 "server" VM
               默认 VM 是 server.
 
-cp <目录和 zip/jar 文件的类搜索路径>
-classpath <目录和 zip/jar 文件的类搜索路径>
               用 ; 分隔的目录, JAR 档案
               和 ZIP 档案列表, 用于搜索类文件。
-D<名称>=<值>
               设置系统属性
-verbose:[class|gc|jni]
               启用详细输出
-version      输出产品版本并退出
-version:<值>
               警告: 此功能已过时, 将在
               未来发行版中删除。
               需要指定的版本才能运行
-showversion  输出产品版本并继续
-jre-restrict-search | -no-jre-restrict-search
               警告: 此功能已过时, 将在
               未来发行版中删除。
               在版本搜索中包括/排除用户专用 JRE
-? -help      输出此帮助消息
-X            输出非标准选项的帮助
-ea[:<packagename>...|:<classname>]
-enableassertions[:<packagename>...|:<classname>]
               按指定的粒度启用断言
-da[:<packagename>...|:<classname>]
-disableassertions[:<packagename>...|:<classname>]
               禁用具有指定粒度的断言
-esa | -enablesystemassertions
               启用系统断言
-dsa | -disablesystemassertions
               禁用系统断言
-agentlib:<libname>[=<选项>]
               加载本机代理库 <libname>, 例如 -agentlib:hprof
               另请参阅 -agentlib:jdwp=help 和 -agentlib:hprof=help
-agentpath:<pathname>[=<选项>]
               按完整路径名加载本机代理库
-javaagent:<jarpath>[=<选项>]
               加载 Java 编程语言代理, 请参阅 java.lang.instrument
-splash:<imagepath>
               使用指定的图像显示启动屏幕

```
### 3. 补充内容：-server与-client
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664377652672-5940eb63-17d6-45e6-86ac-81f26575fabd.png#averageHue=%23bfdcbb&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=191&id=u5553f2e9&originHeight=287&originWidth=732&originalType=binary&ratio=1&rotation=0&showTitle=false&size=225257&status=error&style=none&taskId=ufab31d16-c013-47c8-8f8d-55d87faa539&title=&width=488)
对于以上第2点，我们可以打开DOS窗口，输入java -version就可以看到64位机器上用的server模式，如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664377657391-d7c3938a-41f8-4f5a-bae1-dd9ad75fd3e0.png#averageHue=%23171512&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=69&id=u1683c53b&originHeight=103&originWidth=775&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14985&status=error&style=none&taskId=uc77df1ca-40f5-4825-b228-327621f6f1e&title=&width=516.6666666666666)
## 1.2 类型二：-X参数选项
### 1. 特点
非标准化参数
功能还是比较稳定的。但官方说后续版本可能会变更
以-X开头
### 2. 各种选项
直接在DOS窗口中运行java -X命令可以看到所有的X选项
```java
-Xmixed        混合模式执行 (默认)
-Xint             仅解释模式执行
-Xcomp        仅采用即时编译器模式
-Xbootclasspath:<用 ; 分隔的目录和 zip/jar 文件>
                   设置搜索路径以引导类和资源
-Xbootclasspath/a:<用 ; 分隔的目录和 zip/jar 文件>
                   附加在引导类路径末尾
-Xbootclasspath/p:<用 ; 分隔的目录和 zip/jar 文件>
                   置于引导类路径之前
-Xdiag            显示附加诊断消息
-Xnoclassgc       禁用类垃圾收集
-Xincgc           启用增量垃圾收集
-Xloggc:<file>    将 GC 状态记录在文件中 (带时间戳)
-Xbatch           禁用后台编译
-Xms<size>        设置初始 Java 堆大小
-Xmx<size>        设置最大 Java 堆大小
-Xss<size>        设置 Java 线程堆栈大小
-Xprof            输出 cpu 配置文件数据
-Xfuture          启用最严格的检查, 预期将来的默认值
-Xrs              减少 Java/VM 对操作系统信号的使用 (请参阅文档)
-Xcheck:jni       对 JNI 函数执行其他检查
-Xshare:off       不尝试使用共享类数据
-Xshare:auto      在可能的情况下使用共享类数据 (默认)
-Xshare:on        要求使用共享类数据, 否则将失败。
-XshowSettings    显示所有设置并继续
-XshowSettings:all
                   显示所有设置并继续
-XshowSettings:vm 显示所有与 vm 相关的设置并继续
-XshowSettings:properties
                   显示所有属性设置并继续
-XshowSettings:locale
                   显示所有与区域设置相关的设置并继续
 
-X 选项是非标准选项，如有更改，恕不另行通知
 

```
### 3. JVM的JIT编译模式相关的选项

1. **-Xint : **只使用解释器：所有字节码都被解释执行，这个模式的速度是很慢的
2. **-Xcomp :  **只使用编译器：所有字节码第一次使用就被编译成本地代码，然后在执行
3. **-Xmixed : **混合模式：这是默认模式，刚开始的时候使用解释器慢慢解释执行，后来让JIT即时编译器根据程序运行的情况，有选择地将某些热点代码提前编译并缓存在本地，在执行的时候效率就非常高了** **
### 4. 特别的
**-Xmx -Xms -Xss属于XX参数？**
单位：k/K、m/M、g/G
设置：-Xmx、-Xms最好设置成一样的值，避免扩容带来的损耗

1. **-Xms<size>   **  **  设置初始Java堆大小，等价于-XX:InitialHeapSize**

查看该参数值的时候，应该使用InitialHeapSize，例如jinfo -flag InitialHeapSize 进程id
等价证明：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664378717940-d2f40353-6a89-470f-9a69-89023611d1f3.png#averageHue=%23fbfaf9&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=183&id=u14e23fde&originHeight=275&originWidth=1877&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47087&status=error&style=none&taskId=ud9e513f0-caa9-4d5f-80a0-ddcd418d951&title=&width=1251.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664378721625-194210d6-a639-4beb-b92f-471b5f2fdf43.png#averageHue=%23fcfbfa&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=192&id=ub9f018df&originHeight=288&originWidth=1872&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41665&status=error&style=none&taskId=u762b174c-544e-4a02-a63f-729132cded3&title=&width=1248)

2. **-Xmx<size>       设置最大Java堆大小，等价于-XX:MaxHeapSize**

查看该参数值的时候，应该使用MaxHeapSize，例如jinfo flag InitialHeapSize 进程id
等价证明：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379214094-8aacdf10-1a49-41a4-b162-8a5f50208687.png#averageHue=%23faf9f7&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=230&id=uc8416294&originHeight=345&originWidth=1337&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47327&status=error&style=none&taskId=u12ee20a5-7f1a-44bf-98f2-314ef886d87&title=&width=891.3333333333334)

3. **-Xss<size>         设置Java线程堆栈大小，等价于-XX:ThreadStackSize**

查看该参数值的时候，应该使用ThreadStackSize，例如jinfo flag InitialHeapSize 进程id
## 1.3 类型三：-XX参数选项
### 1. 特点
非标准化参数
使用的最多的参数类型
这类选项属于实验性，不稳定
以-XX开头
### 2. 作用
用于开发和调试JVM
## 3. 分类
### 1. Boolean类型格式

1. -XX:+<option>  表示启用option属性
2. -XX:-<option>表示禁用option属性
3. 栗子：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379508062-a2ed2245-a99b-4cd6-9c5d-aca65d7695e3.png#averageHue=%23c0dcbc&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=70&id=ueca8aa40&originHeight=105&originWidth=682&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74443&status=error&style=none&taskId=u30111c29-6b60-4cbe-8ac4-dde5ee8f9d8&title=&width=454.6666666666667)

4. 说明：因为有的指令默认是开启的，所以可以使用-关闭
### 2. 非Boolean类型格式（key-value类型）
子类型1：数值型格式-XX:<option>=<number>
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379584311-e37da184-f305-4b1e-ad0d-5568ae710ea9.png#averageHue=%23c0ddbc&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=156&id=u1f2635bf&originHeight=234&originWidth=735&originalType=binary&ratio=1&rotation=0&showTitle=false&size=148162&status=error&style=none&taskId=u861a1662-d9c3-4411-83e7-a340e2fc542&title=&width=490)
子类型2：非数值型格式-XX:<name>=<string>
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379591271-3b8a0308-704a-4343-be04-5682004a9831.png#averageHue=%23c3e0bf&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=59&id=ue9fd540c&originHeight=88&originWidth=729&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42557&status=error&style=none&taskId=u23c6cfa3-4498-4f5d-a511-41dac35b891&title=&width=486)
## 4. 特别地

1. **-XX:+PrintFlagsFinal**

输出所有参数的名称和默认值
默认不包括Diagnostic和Experimental的参数
可以配合-XX:+UnlockDiagnosticVMOptions和-XX:UnlockExperimentalVMOptions使用
# 2. 添加JVM参数选项
## 2.1 Eclipse
1、在空白处单击右键，选择Run As，在选择Run Configurations……
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379826833-4d6eff0f-65a7-4284-b0ad-b57935639517.png#averageHue=%23cae5c6&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=441&id=ud3bcc276&originHeight=662&originWidth=661&originalType=binary&ratio=1&rotation=0&showTitle=false&size=223247&status=error&style=none&taskId=u1ec031c3-d932-4dce-afe9-0776cf2c96a&title=&width=440.6666666666667)
2、设置虚拟机参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379837309-681375f5-b8f7-4911-8203-dc1efbb99e0d.png#averageHue=%23cde4c8&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=497&id=u6446a519&originHeight=746&originWidth=752&originalType=binary&ratio=1&rotation=0&showTitle=false&size=377222&status=error&style=none&taskId=ud293baa2-4dcd-4e74-ad25-6ac7822e497&title=&width=501.3333333333333)
## 2.2 IDEA
1、Edit Configurations…
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379855831-cfef6603-26d5-4fac-8336-9b4bd5f388d3.png#averageHue=%23f4f3f1&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=249&id=u0b073105&originHeight=373&originWidth=426&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26565&status=error&style=none&taskId=u260a5ffa-ffa5-464a-8adc-9052b30199a&title=&width=284)
2、设置虚拟机参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664379864818-244eda78-23b5-4b3f-bb43-784a24d65dfe.png#averageHue=%23f1f1f1&clientId=ude49760b-a70b-4&errorMessage=unknown%20error&from=paste&height=442&id=u5a8306eb&originHeight=663&originWidth=940&originalType=binary&ratio=1&rotation=0&showTitle=false&size=278913&status=error&style=none&taskId=ucb462573-b27a-49a7-88c5-7f771791129&title=&width=626.6666666666666)
## 2.3 运行jar包
**java -Xms50m -Xmx50m -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -jar demo.jar**
这是在java -jar demo.jar中的java -jar之间添加了虚拟机配置信息
## 2.4 通过Tomcat运行war包
Linux系统下可以在tomcat/bin/catalina.sh中添加类似如下配置：
JAVA_OPTS="-Xms512M -Xmx1024M"
Windows系统下载catalina.bat中添加类似如下配置：
set "JAVA_OPTS=-Xms512M -Xmx1024M"
## 2.5 程序运行过程中
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664380302555-50579230-9bc9-40ab-a263-aae392f858dc.png#averageHue=%23c0dbc5&clientId=u69e1414e-cf80-4&errorMessage=unknown%20error&from=paste&height=159&id=u7839d68e&originHeight=239&originWidth=1146&originalType=binary&ratio=1&rotation=0&showTitle=false&size=195437&status=error&style=none&taskId=u6953811a-5ba1-4aab-823d-52e454590b4&title=&width=764)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664380291465-396907a0-0088-466c-b3f4-1065f4d26144.png#averageHue=%23c1debd&clientId=u69e1414e-cf80-4&errorMessage=unknown%20error&from=paste&height=511&id=ue0e834b6&originHeight=767&originWidth=1208&originalType=binary&ratio=1&rotation=0&showTitle=false&size=514257&status=error&style=none&taskId=u4d4f7575-cf9d-449b-ac6f-7cb113a2fbd&title=&width=805.3333333333334)
使用jinfo -flag <name>=<value> <pid>设置非Boolean类型参数
使用jinfo -flag [+|-]<name> <pid>设置Boolean类型参数
# 3-常用的JVM参数选项
## 3.1 打印设置的XX选项及值
### 1. -XX:+PrintCommandLineFlags
可以让程序运行前打印出用户手动设置或者JVM自动设置的XX选项
### 2. -XX:+PrintFlagsInitial
表示打印出所有XX选项的默认值
### 3. -XX:+PrintFlagsFinal
表示打印出XX选项在运行程序时生效的值
如果值的前面加上了:=，说明该值不是初始值，该值可能被jvm自动改变了，也可能被我们设置的参数改变了，如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664460269789-d2e575c2-082d-4bc0-9118-76a104d75fd1.png#averageHue=%23e4e1dc&clientId=u57729a7b-ea51-4&errorMessage=unknown%20error&from=paste&height=144&id=ub45c5e8e&originHeight=216&originWidth=988&originalType=binary&ratio=1&rotation=0&showTitle=false&size=112558&status=error&style=none&taskId=u2f9b0a9d-5801-4a55-8d53-a6fd6cce1b1&title=&width=658.6666666666666)
### 4. -XX:+PrintVMOptions
打印JVM的参数

## 3.2 堆、栈、方法区等内存大小设置
### 1. 栈
**-Xss128k**
等价于-XX:ThreadStackSize，设置每个线程的栈大小为128k
### 3. 堆内存
**-Xms3550m**
等价于-XX:InitialHeapSize，设置JVM初始堆内存为3500M
**-Xmx3550m**
等价于-XX:MaxHeapSize，设置JVM最大堆内存为3500M
**-Xmn2g**
设置年轻代大小为2G，即等价于-XX:NewSize=2g -XX:MaxNewSize=2g，也就是设置年轻代初始值和年轻代最大值都是2G
官方推荐配置为整个堆大小的3/8
**-XX:NewSize=1024m**
设置年轻代初始值为1024M
**-XX:MaxNewSize=1024m**
设置年轻代最大值为1024M
**-XX:SurvivorRatio=8**
设置年轻代中Eden区与一个Survivor区的比值，默认为8
只有显示使用Eden区和Survivor区的比例，才会让比例生效，否则比例都会自动设置，至于其中的原因，请看下面的-XX:+UseAdaptiveSizePolicy中的解释，最后推荐使用默认打开的-XX:+UseAdaptiveSizePolicy设置，并且不显示设置-XX:SurvivorRatio
** -XX:+UseAdaptiveSizePolicy**
自动选择各区大小比例，默认开启
1、分析
默认开启，将会导致Eden区和Survivor区的比例自动分配，因此也会引起我们默认值-XX:SurvivorRatio=8失效，所以真实比例可能不是8，比如可能是6等
2、如何设置Eden区和Survivor区的比例
-XX:SurvivorRatio=8
显示使用Eden区和Survivor区的比例，那就使用我自己的
没有显示使用Eden区和Survivor区的比例，无论打开或者关闭-XX:+UseAdaptiveSizePolicy，都会自动设置Eden区和Survivor区的比例
结论：
只有显示使用Eden区和Survivor区的比例，才会让比例生效，否则比例都会自动设置，最后推荐使用默认打开的-XX:+UseAdaptiveSizePolicy设置，并且不显示设置-XX:SurvivorRatio

** -XX:NewRatio=2**
设置老年代与年轻代（包括1个Eden区和2个Survivor区）的比值，默认为2
根据实际情况进行设置，主要根据对象生命周期来进行分配，如果对象生命周期很长，那么让老年代大一点，否则让新生代大一点
**-XX:PretenureSizeThreadshold=1024**
设置让大于此阈值的对象直接分配在老年代，单位为字节
只对Serial、ParNew收集器有效
不好控制
**-XX:MaxTenuringThreshold=15**
默认值为15
新生代每次MinorGC后，还存活的对象年龄+1，当对象的年龄大于设置的这个值时就进入老年代
使用比较少，一般用默认值
**-XX:+PrintTenuringDistribution**
让JVM在每次MinorGC后打印出当前使用的Survivor中对象的年龄分布
**-XX:TargetSurvivorRatio**
表示MinorGC结束后Survivor区域中占用空间的期望比例
### 3.3 方法区

1. **永久代**

-XX:PermSize=256m
设置永久代初始值为256M
-XX:MaxPermSize=256m
设置永久代最大值为256M

2. **元空间**

-XX:MetaspaceSize
初始空间大小
-XX:MaxMetaspaceSize
最大空间，默认没有限制
-XX:+UseCompressedOops
使用压缩对象指针
-XX:+UseCompressedClassPointers
使用压缩类指针
-XX:CompressedClassSpaceSize
设置Klass Metaspace的大小，默认1G
### 3.4 直接内存
-XX:MaxDirectMemorySize
指定DirectMemory容量，若未指定，则默认与Java堆最大值一样
## 3.3 OutOfMemory相关的选项
-XX:+HeapDumpOnOutMemoryError(在出现OOM的时候生成dump文件)和
-XX:+HeapDumpBeforeFullGC(在出现Full GC的时候生成dump文件)只能设置1个，
如果不设置-XX:HeapDumpPath=<path>，那么将会在当前目录下生成dump文件，
如果设置的话，将会在指定位置生成dump文件

1. -XX:+HeapDumpOnOutMemoryError

表示在内存出现OOM的时候，生成Heap转储文件，以便后续分析，-XX:+HeapDumpBeforeFullGC和-XX:+HeapDumpOnOutMemoryError只能设置1个

2. -XX:+HeapDumpBeforeFullGC

表示在出现FullGC之前，生成Heap转储文件，以便后续分析，-XX:+HeapDumpBeforeFullGC和-XX:+HeapDumpOnOutMemoryError只能设置1个，请注意FullGC可能出现多次，那么dump文件也会生成多个

3. -XX:HeapDumpPath=<path>

指定heap转存文件的存储路径，如果不指定，就会将dump文件放在当前目录中

4. -XX:OnOutOfMemoryError

指定一个可行性程序或者脚本的路径，当发生OOM的时候，去执行这个脚本
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664547654170-c73913e1-99f8-495e-86c1-2b0e4332c99c.png#averageHue=%23c6e2c9&clientId=ufbc3b86c-9c61-4&from=paste&height=339&id=udd3c1263&originHeight=508&originWidth=667&originalType=binary&ratio=1&rotation=0&showTitle=false&size=199155&status=done&style=none&taskId=u704805f6-b74e-429c-80da-5f5726d23c0&title=&width=444.6666666666667)
## 3.4 垃圾收集器相关选项
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664547764973-7dd9b989-e923-4672-b6ff-bf60f7658306.png#averageHue=%23c5dcc8&clientId=ufbc3b86c-9c61-4&from=paste&height=147&id=u59c633d0&originHeight=220&originWidth=733&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88887&status=done&style=none&taskId=u0a7006e4-6b43-460c-b874-f68410cadbe&title=&width=488.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664547771822-7c7c2320-a4bf-46c6-b9ea-d203fdd2426a.png#averageHue=%23e4aa63&clientId=ufbc3b86c-9c61-4&from=paste&height=246&id=uf74ec77d&originHeight=369&originWidth=615&originalType=binary&ratio=1&rotation=0&showTitle=false&size=143427&status=done&style=none&taskId=ud02fd9d7-5b9f-49d8-b3de-91a179d8e31&title=&width=410)
### 1. 查看默认的垃圾回收器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664547861355-601fd14d-29c6-4256-908b-b755426e25dc.png#averageHue=%23c3dec9&clientId=ufbc3b86c-9c61-4&from=paste&height=58&id=ua05afd22&originHeight=87&originWidth=707&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65028&status=done&style=none&taskId=u14a50b53-58fa-4327-9e63-aa8a03b664b&title=&width=471.3333333333333)
以上两种方式都可以查看默认使用的垃圾回收器，第一种方式更加准备，但是需要程序的支持；第二种方式需要去尝试，如果使用了，返回的值中有+号，否则就是-号
### 2. Serial回收器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664548063094-ad6460af-4d1c-489c-9572-c5f199f58fd0.png#averageHue=%23c1dbc6&clientId=ufbc3b86c-9c61-4&from=paste&height=112&id=u0d5c1984&originHeight=168&originWidth=732&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129493&status=done&style=none&taskId=ub1052e56-e88b-4f91-8973-7c3dff2f175&title=&width=488)
### 3.Parnew回收器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664548149678-41515927-f323-469f-a11a-7eff702b53b8.png#averageHue=%23c3dec8&clientId=ufbc3b86c-9c61-4&from=paste&height=65&id=u1b034b91&originHeight=98&originWidth=731&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50067&status=done&style=none&taskId=uc8bba7b1-df69-4d59-8b49-afb8c7d1f0c&title=&width=487.3333333333333)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664548181626-7873caab-95bd-41e1-9ca2-c0884816f43b.png#averageHue=%23bed8c2&clientId=ufbc3b86c-9c61-4&from=paste&height=69&id=uf20b8937&originHeight=104&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100977&status=done&style=none&taskId=u7321b731-9db4-465a-9242-5b781309fde&title=&width=484.6666666666667)
### 4. Parallel Scavenge回收器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664548436347-b2579d92-ea95-433c-9166-97a410c9ae7b.png#averageHue=%23bfd9c4&clientId=ufbc3b86c-9c61-4&from=paste&height=419&id=ue3231faa&originHeight=629&originWidth=731&originalType=binary&ratio=1&rotation=0&showTitle=false&size=567372&status=done&style=none&taskId=uad1e4bbf-6e22-4bf2-8ffd-ccca1c7d76f&title=&width=487.3333333333333)
Parallel回收器主打吞吐量，而CMS和G1主打低延迟，如果主打吞吐量，那么就不应该限制最大停顿时间，所以-XX:MaxGCPauseMills不应该设置
-XX:MaxGCPauseMills中的调整堆大小通过默认开启的-XX:+UseAdaptiveSizePolicy来实现
-XX:GCTimeRatio用来衡量吞吐量，并且和-XX:MaxGCPauseMills矛盾，因此不会同时使用
### 5. CMS回收器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664548776763-c9b319af-95c7-41c9-bee9-6d23f7e26de8.png#averageHue=%23c0dbc5&clientId=ufbc3b86c-9c61-4&from=paste&height=347&id=u9ece5c11&originHeight=520&originWidth=725&originalType=binary&ratio=1&rotation=0&showTitle=false&size=463309&status=done&style=none&taskId=ucf602606-fede-4948-89ba-be3b81fd943&title=&width=483.3333333333333)
-XX:ParallelCMSThreads和ParallelGCThreads有关系，ParallelGCThreads在上面Parnew回收器中有提到

1. **补充参数**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664549104542-c8d102cb-9789-4968-b0ef-834ff3ed585e.png#averageHue=%23c2ddc8&clientId=ufbc3b86c-9c61-4&from=paste&height=293&id=u0963826f&originHeight=440&originWidth=733&originalType=binary&ratio=1&rotation=0&showTitle=false&size=360894&status=done&style=none&taskId=u7e6689a3-5d64-4eea-a4dc-e25e37138d8&title=&width=488.6666666666667)

2. **特别说明**

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664549110214-88bed82a-06e0-4a3b-b5df-10e8d4c016a2.png#averageHue=%23c3dec8&clientId=ufbc3b86c-9c61-4&from=paste&height=228&id=u917795a9&originHeight=342&originWidth=735&originalType=binary&ratio=1&rotation=0&showTitle=false&size=268453&status=done&style=none&taskId=u143dddb8-fc9d-457e-9d46-9ff27c8fa40&title=&width=490)
### 6. G1回收器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664549152002-6cd0e39a-1c99-4171-9b2e-2dfbf926a245.png#averageHue=%23c3dec8&clientId=ufbc3b86c-9c61-4&from=paste&height=459&id=uf1adb445&originHeight=689&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=400799&status=done&style=none&taskId=ue0bb5ee7-98a7-4849-aa2b-83fc959dc0e&title=&width=484.6666666666667)
如果使用G1垃圾收集器，不建议设置-Xmn和-XX:NewRatio，毕竟可能影响G1的自动调节

1. Mixed GC调优参数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664549176540-ebc451af-53fb-46df-8d84-80454888c55c.png#averageHue=%23bfdac4&clientId=ufbc3b86c-9c61-4&from=paste&height=317&id=udcad46fd&originHeight=475&originWidth=725&originalType=binary&ratio=1&rotation=0&showTitle=false&size=440664&status=done&style=none&taskId=ubf5c152f-4af3-46fb-9958-aed90e517fa&title=&width=483.3333333333333)
### 6. 怎么选择垃圾收集器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664549245801-f800743e-2428-4907-9eca-5fd5a0a293ba.png#averageHue=%23c0dbc5&clientId=ufbc3b86c-9c61-4&from=paste&height=222&id=u2758c4f2&originHeight=333&originWidth=723&originalType=binary&ratio=1&rotation=0&showTitle=false&size=228357&status=done&style=none&taskId=u030c8c3e-fcdf-4cab-a385-425d33d4c21&title=&width=482)
## 3.5 GC日志相关选项
### 1. 常用参数

1. **-verbose:gc**

输出日志信息，默认输出的标准输出
可以独立使用
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664550813625-94589042-0412-4f45-a5a6-fee9743b5c3e.png#averageHue=%23b6cdb3&clientId=ufbc3b86c-9c61-4&from=paste&height=91&id=u171f5f17&originHeight=136&originWidth=694&originalType=binary&ratio=1&rotation=0&showTitle=false&size=160913&status=done&style=none&taskId=u265a815f-eaf4-45bf-934f-b6a35667225&title=&width=462.6666666666667)

2. **-XX:+PrintGC**

等同于-verbose:gc
表示打开简化的日志
可以独立使用

3. **-XX:+PrintGCDetails**

在发生垃圾回收时打印内存回收详细的日志，
并在进程退出时输出当前内存各区域的分配情况
可以独立使用
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664553955358-c954da04-a120-44e9-8b32-236515f98165.png#averageHue=%23b5d0b2&clientId=ufbc3b86c-9c61-4&from=paste&height=217&id=uef16113c&originHeight=326&originWidth=1191&originalType=binary&ratio=1&rotation=0&showTitle=false&size=624167&status=done&style=none&taskId=ua9686d42-e24f-4877-972e-94601cdd1e1&title=&width=794)

4. **-XX:+PrintGCTimeStamps**

程序启动到GC发生的时间秒数
不可以独立使用，需要配合-XX:+PrintGCDetails使用
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664554114460-0933413d-225f-47b1-b6b8-3491b29b28c3.png#averageHue=%23b5ceb4&clientId=ufbc3b86c-9c61-4&from=paste&height=220&id=ud34211c4&originHeight=330&originWidth=1337&originalType=binary&ratio=1&rotation=0&showTitle=false&size=684265&status=done&style=none&taskId=ua3ba0068-5c93-47c4-8a2b-7539efe5e76&title=&width=891.3333333333334)

5. **-XX:+PrintGCDateStamps**

输出GC发生时的时间戳（以日期的形式，例如：2013-05-04T21:53:59.234+0800）
不可以独立使用，可以配合-XX:+PrintGCDetails使用
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664555038442-c443ec7a-9e5a-4d29-b9a3-e57417c011d1.png#averageHue=%23b8d1b6&clientId=ufbc3b86c-9c61-4&from=paste&height=213&id=uf4916f78&originHeight=320&originWidth=1332&originalType=binary&ratio=1&rotation=0&showTitle=false&size=668016&status=done&style=none&taskId=u68a0d1d8-9e03-45f9-9eb7-d8bfffb8f0b&title=&width=888)

6. **-XX:+PrintHeapAtGC**

每一次GC前和GC后，都打印堆信息
可以独立使用
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664555070865-fcc0356a-d003-4bd0-a6b6-cf3ab631bdaa.png#averageHue=%23b0c8af&clientId=ufbc3b86c-9c61-4&from=paste&height=211&id=ud2ed7c37&originHeight=316&originWidth=1182&originalType=binary&ratio=1&rotation=0&showTitle=false&size=588330&status=done&style=none&taskId=u099797ee-e5c6-4b18-8d33-62884b3e196&title=&width=788)

7. **-XIoggc:<file>**

把GC日志写入到一个文件中去，而不是打印到标准输出中
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664555136165-62ce57cd-14d1-4844-ba2e-2d225f378b23.png#averageHue=%23afc4ac&clientId=ufbc3b86c-9c61-4&from=paste&height=274&id=u4aefce73&originHeight=411&originWidth=714&originalType=binary&ratio=1&rotation=0&showTitle=false&size=636290&status=done&style=none&taskId=u7a5acc65-2d25-47a6-acde-bd847d3e8cc&title=&width=476)
### 2. 其他参数
-XX:TraceClassLoading
监控类的加载
-XX:PrintGCApplicationStoppedTime
打印GC时线程的停顿时间
-XX:+PrintGCApplicationConcurrentTime
垃圾收集之前打印出应用未中断的执行时间
-XX:+PrintReferenceGC
记录回收了多少种不同引用类型的引用
-XX:+PrintTenuringDistribution
让JVM在每次MinorGC后打印出当前使用的Survivor中对象的年龄分布
-XX:+UseGCLogFileRotation
启用GC日志文件的自动转储
-XX:NumberOfGCLogFiles=1
GC日志文件的循环数目
-XX:GCLogFileSize=1M
控制GC日志文件的大小
## 3.6 其他参数

1. -XX:+DisableExplicitGC

禁用hotspot执行System.gc()，默认禁用

2. -XX:ReservedCodeCacheSize=<n>[g|m|k]、-XX:InitialCodeCacheSize=<n>[g|m|k]

指定代码缓存的大小

3. -XX:+UseCodeCacheFlushing

使用该参数让jvm放弃一些被编译的代码，
避免代码缓存被占满时JVM切换到interpreted-only的情况

4. -XX:+DoEscapeAnalysis

开启逃逸分析

5. -XX:+UseBiasedLocking

开启偏向锁

6. -XX:+UseLargePages

开启使用大页面

7. -XX:+PrintTLAB

打印TLAB的使用情况

8. -XX:TLABSize

设置TLAB大小

# 4. 通过Java代码获取JVM参数
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664555521284-d3fd0203-2dcd-453e-836d-75ebbfb50abc.png#averageHue=%23bbd7b7&clientId=ufbc3b86c-9c61-4&from=paste&height=97&id=u1c33d771&originHeight=146&originWidth=802&originalType=binary&ratio=1&rotation=0&showTitle=false&size=155800&status=done&style=none&taskId=u6ffbb771-204c-4e39-9145-0378a00e319&title=&width=534.6666666666666)
```java
/**
 *
 * 监控我们的应用服务器的堆内存使用情况，设置一些阈值进行报警等处理
 *
 * @author shkstart
 * @create 15:23
 */
public class MemoryMonitor {
    public static void main(String[] args) {
        MemoryMXBean memorymbean = ManagementFactory.getMemoryMXBean();
        MemoryUsage usage = memorymbean.getHeapMemoryUsage();
        System.out.println("INIT HEAP: " + usage.getInit() / 1024 / 1024 + "m");
        System.out.println("MAX HEAP: " + usage.getMax() / 1024 / 1024 + "m");
        System.out.println("USE HEAP: " + usage.getUsed() / 1024 / 1024 + "m");
        System.out.println("\nFull Information:");
        System.out.println("Heap Memory Usage: " + memorymbean.getHeapMemoryUsage());
        System.out.println("Non-Heap Memory Usage: " + memorymbean.getNonHeapMemoryUsage());

        System.out.println("=======================通过java来获取相关系统状态============================ ");
        System.out.println("当前堆内存大小totalMemory " + (int) Runtime.getRuntime().totalMemory() / 1024 / 1024 + "m");// 当前堆内存大小
        System.out.println("空闲堆内存大小freeMemory " + (int) Runtime.getRuntime().freeMemory() / 1024 / 1024 + "m");// 空闲堆内存大小
        System.out.println("最大可用总堆内存maxMemory " + Runtime.getRuntime().maxMemory() / 1024 / 1024 + "m");// 最大可用总堆内存大小

    }
}
```
## 1. 上篇通过Runtime获取
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664555545339-e892b2eb-3770-4fd8-8ee0-10f7c675e69a.png#averageHue=%23c1dfbd&clientId=ufbc3b86c-9c61-4&from=paste&height=203&id=u3a242d16&originHeight=305&originWidth=734&originalType=binary&ratio=1&rotation=0&showTitle=false&size=204475&status=done&style=none&taskId=u20d73abd-a9e1-4fdd-8ce7-1c67f143b4f&title=&width=489.3333333333333)
