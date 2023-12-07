# 1. GC日志参数
## 1.1 -verbose:gc
输出gc日志信息，默认输出到标准输出
## 1.2 -XX:+PrintGC
输出GC日志。类似：-verbose:gc
## 1.3 -XX:+PrintGCDetails
在发生垃圾回收时打印内存回收相处的日志，
并在进程退出时输出当前内存各区域分配情况
## 1.4 -XX:+PrintGCTimeStamps
输出GC发生时的时间戳
## 1.5-XX:+PrintGCDateStamps
输出GC发生时的时间戳（以日期的形式，例如：2013-05-04T21:53:59.234+0800）
## 1.6 -XX:+PrintHeapAtGC
每一次GC前和GC后，都打印堆信息
## 1.7 -Xloggc:<file>
表示把GC日志写入到一个文件中去，而不是打印到标准输出中
# 2. GC日志格式
## 2.1. 复习：GC分类
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664591463544-c5bf4ad0-aa4f-4d9d-bc07-5a6dbced2b3f.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=222&id=u3b1a3b01&originHeight=333&originWidth=803&originalType=binary&ratio=1&rotation=0&showTitle=false&size=290189&status=error&style=none&taskId=ubd1fe691-6eaf-4800-96c7-eb2e7230ec2&title=&width=535.3333333333334)

1. **新生代收集：**当Eden区满的时候就会进行新生代收集，所以新生代收集和S0区域和S1区域无关
2. **老年代收集和新生代收集的关系： 进行老年代收集之前会先进行一次年轻代的垃圾收集，原因如下**：一个比较大的对象无法放入新生代，那它自然会往老年代去放，如果老年代也放不下，那会先进行一次新生代的垃圾收集，之后尝试往新生代放，如果还是放不下，才会进行老年代的垃圾收集，之后在往老年代去放，这是一个过程，我来说明一下为什么需要往老年代放，但是放不下，而进行新生代垃圾收集的原因，这是因为新生代垃圾收集比老年代垃圾收集更加简单，这样做可以节省性能
3. **进行垃圾收集的时候，堆包含新生代、老年代、元空间/永久代：**可以看出Heap后面包含着新生代、老年代、元空间，但是我们设置堆空间大小的时候设置的只是新生代、老年代而已，元空间是分开设置的

![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664591712070-b6dd6fcd-3b8d-4998-8c67-d1ca95142b13.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=133&id=u53dbbe8b&originHeight=199&originWidth=1086&originalType=binary&ratio=1&rotation=0&showTitle=false&size=352150&status=error&style=none&taskId=udd309a32-8e8a-49ad-9e19-33762bbfc7d&title=&width=724)

4. 哪些情况会触发Full GC：老年代空间不足、方法区空间不足、显示调用System.gc()、Minior GC进入老年代的数据的平均大小 大于 老年代的可用内存、大对象直接进入老年代，而老年代的可用空间不足
## 2.2 不同GC分类的GC细节
```java
/**
 *  -XX:+PrintCommandLineFlags
 *
 *  -XX:+UseSerialGC:表明新生代使用Serial GC ，同时老年代使用Serial Old GC
 *
 *  -XX:+UseParNewGC：标明新生代使用ParNew GC
 *
 *  -XX:+UseParallelGC:表明新生代使用Parallel GC
 *  -XX:+UseParallelOldGC : 表明老年代使用 Parallel Old GC
 *  说明：二者可以相互激活
 *
 *  -XX:+UseConcMarkSweepGC：表明老年代使用CMS GC。同时，年轻代会触发对ParNew 的使用
 * @author shkstart
 * @create 17:19
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[1024 * 10];//10kb
            list.add(arr);
//            try {
//                Thread.sleep(5);
//            } catch (InterruptedException e) {
//                e.printStackTrace();
//            }
        }
    }
} 

```
### 1. 老年代使用CMS GC
**GC设置方法：**参数中使用-XX:+UseConcMarkSweepGC，说明老年代使用CMS GC，同时年轻代也会触发对ParNew的使用，因此添加该参数之后，新生代使用ParNew GC，而老年代使用CMS GC，整体是并发垃圾收集，主打低延迟
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592371886-d6712020-0ab9-4e2b-af25-5d145184e3e2.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=189&id=ub7a48baf&originHeight=283&originWidth=649&originalType=binary&ratio=1&rotation=0&showTitle=false&size=124757&status=error&style=none&taskId=uaa010a52-7934-4b05-8093-823bd09f17c&title=&width=432.6666666666667)
**打印出来的GC细节：**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592384054-63346e9c-bdf8-489c-9ff6-e760ded9fe22.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=253&id=u0a715b4d&originHeight=379&originWidth=1410&originalType=binary&ratio=1&rotation=0&showTitle=false&size=587571&status=error&style=none&taskId=u1329af48-d7fb-4460-b33d-e9b590a7468&title=&width=940)
### 2. 新生代使用Serial GC
** GC设置方法：**参数中使用-XX:+UseSerialGC，说明新生代使用Serial GC，同时老年代也会触发对Serial Old GC的使用，因此添加该参数之后，新生代使用Serial GC，而老年代使用Serial Old GC，整体是串行垃圾收集
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592486929-ed7879c8-bf4d-4661-bc5c-3531ba8da852.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=185&id=ud38d3b57&originHeight=277&originWidth=647&originalType=binary&ratio=1&rotation=0&showTitle=false&size=104300&status=error&style=none&taskId=uca9950b8-db70-4af2-95ab-b03255db8a9&title=&width=431.3333333333333)
 打印出来的GC细节：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592494250-2193dcfc-ad3e-458c-8074-35de339d3fed.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=145&id=u72b5f712&originHeight=217&originWidth=1424&originalType=binary&ratio=1&rotation=0&showTitle=false&size=487217&status=error&style=none&taskId=u40a17019-89dc-4fd7-a8d6-7838143bbd7&title=&width=949.3333333333334)
DefNew代表新生代使用Serial GC，然后Tenured代表老年代使用Serial Old GC
## 2.3 GC日志分类
### 1. MinorGC
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592535193-001316d6-37f6-4268-bad2-43aea22ff0cf.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=357&id=uf3d66dc9&originHeight=536&originWidth=788&originalType=binary&ratio=1&rotation=0&showTitle=false&size=243598&status=error&style=none&taskId=u5db91610-dd12-47f2-9263-97f2430f873&title=&width=525.3333333333334)
### 2. FullGC
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592557070-a7ec3dd8-aa50-49c3-970e-edd7841a5c3b.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=343&id=u22019968&originHeight=515&originWidth=782&originalType=binary&ratio=1&rotation=0&showTitle=false&size=262907&status=error&style=none&taskId=ud9c4d5f7-a192-4f25-ab72-12e1e9a1624&title=&width=521.3333333333334)
## 2.4 GC日志结构剖析
### 1.垃圾收集器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592725652-11881808-ca69-4d01-9cdd-2dfb3501c298.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=195&id=u41f166bd&originHeight=292&originWidth=804&originalType=binary&ratio=1&rotation=0&showTitle=false&size=231972&status=error&style=none&taskId=u176418a0-702b-44a8-b3f2-73f67ee4fb3&title=&width=536)
### 2. GC前后情况
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592800843-580d3da0-5971-4b4c-9234-5f502c566a6c.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=122&id=ua09e2fe9&originHeight=183&originWidth=800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=141566&status=error&style=none&taskId=u9afab87c-2b29-49a1-800d-f8ddc27e39b&title=&width=533.3333333333334)
### 3. GC时间
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664592877561-d2879da1-8a66-4af7-b18e-8b395a9c8cdf.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=241&id=ufb94ab37&originHeight=361&originWidth=809&originalType=binary&ratio=1&rotation=0&showTitle=false&size=347471&status=error&style=none&taskId=u2b5c8e1a-d899-44af-b1bc-02bf60c9d16&title=&width=539.3333333333334)
## 2.5 Minor GC 日志解析
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664593080596-66a114d2-1bce-4ff4-b82a-676807e39d32.png#clientId=ub7c4e63c-5019-4&errorMessage=unknown%20error&from=paste&height=50&id=ud6a95f23&originHeight=75&originWidth=783&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91954&status=error&style=none&taskId=u8fe29a2c-74be-4806-bc39-17e4f36c8ca&title=&width=522)
### 1. 2020-11-20T17:19:43.265-0800
日志打印时间 日期格式 如
2013-05-04T21:53:59.234+0800
添加-XX:+PrintGCDateStamps参数
### 2.  0.822:
gc发生时，Java虚拟机启动以来经过的秒数
添加-XX:+PrintGCTimeStamps该参数
### 3. [GC(Allocation Failure)
发生了一次垃圾回收，这是一次Minior GC。它不区分新生代还是老年代GC，括号里的内容是gc发生的原因，这里的Allocation Failure的原因是新生代中没有足够区域能够存放需要分配的数据而失败
### 4. [PSYoungGen:76800K->8433K(89600K)
PSYoungGen：表示GC发生的区域，区域名称与使用的GC收集器是密切相关的

1. Serial收集器：Default New Generation 显示Defnew
2. ParNew收集器：ParNew
3. Parallel Scanvenge收集器：PSYoung
4. 老年代和新生代同理，也是和收集器名称相关
### 5. 76800K->8433K(89600K)：GC前该内存区域已使用容量->GC后盖区域容量(该区域总容量)

1. 如果是新生代，总容量则会显示整个新生代内存的9/10，即eden+from/to区
2. 如果是老年代，总容量则是全身内存大小，无变化
### 6.  76800K->8449K(294400K)
在显示完区域容量GC的情况之后，会接着显示整个堆内存区域的GC情况：GC前堆内存已使用容量->GC后堆内存容量（堆内存总容量），并且堆内存总容量 = 9/10 新生代 + 老年代，然后堆内存总容量肯定小于初始化的内存大小
虽然本次是Minor GC，只会进行新生代的垃圾收集，但是也肯定会打印堆中总容量相关信息
### 7.  ,0.0088371
整个GC所花费的时间，单位是秒
### 8. [Times：user=0.02 sys=0.01,real=0.01 secs]

1. user：指CPU工作在用户态所花费的时间
2. sys：指CPU工作在内核态所花费的时间
3. real：指在此次事件中所花费的总时间
## 2.6 Full GC 日志解析
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664616358984-e8a656e4-b668-465b-8b0e-b9133d06c295.png#clientId=u87c3954c-c8ef-4&errorMessage=unknown%20error&from=paste&height=85&id=u971b4bf7&originHeight=127&originWidth=763&originalType=binary&ratio=1&rotation=0&showTitle=false&size=130923&status=error&style=none&taskId=u7a533358-3b28-4353-9302-6f79a286bdb&title=&width=508.6666666666667)
### 1. 2020-11-20T17:19:43.794-0800
添加-XX:+PrintGCDateStamps参数
 日志打印时间 日期格式 如
2013-05-04T21:53:59.234+0800
### 2. 1.351
添加-XX:+PrintGCTimeStamps该参数
gc发生时，Java虚拟机启动以来经过的秒数
### 3. Full GC(Metadata GCThreshold)
括号中是gc发生的原因，原因：Metaspace区不够用了。
除此之外，还有另外两种情况会引起Full GC，如下：
1、Full GC(FErgonomics)
原因：JVM自适应调整导致的GC
2、Full GC（System）
原因：调用了System.gc()方法
### 4. [PSYoungGen: 100082K->0K(89600K)]
#### 1. PSYoungGen：表示GC发生的区域，区域名称与使用的GC收集器是密切相关的

1. Serial收集器：Default New Generation 显示DefNew
2. ParNew收集器：ParNew
3. Parallel Scanvenge收集器：PSYoungGen
4. 老年代和新生代同理，也是和收集器名称相关
#### 2. 10082K->0K(89600K)：GC前该内存区域已使用容量->GC该区域容量(该区域总容量)
如果是新生代，总容量会显示整个新生代内存的9/10，即eden+from/to区
如果是老年代，总容量则是全部内存大小，无变化
### 5. ParOldGen：32K->9638K(204800K)
老年代区域没有发生GC，因此本次GC是metaspace引起的
### 6. 10114K->9638K(294400K),
在显示完区域容量GC的情况之后，会接着显示整个堆内存区域的GC情况：GC前堆内存已使用容量->GC后堆内存容量（堆内存总容量），并且堆内存总容量 = 9/10 新生代 + 老年代，然后堆内存总容量肯定小于初始化的内存大小
### 7. [Meatspace:20158K->20156K(1067008K)],
metaspace GC 回收2K空间
# 3.  GC日志分析工具
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664617291539-4e90a28b-ef64-4ba8-80a4-1c95695ce102.png#clientId=u10d9c343-1498-4&from=paste&height=167&id=u230d8f76&originHeight=251&originWidth=809&originalType=binary&ratio=1&rotation=0&showTitle=false&size=193889&status=done&style=none&taskId=uda68fbe2-3ab6-4a5c-98a2-b0dcf5f620d&title=&width=539.3333333333334)
## 3.1 GCEasy
基本概述
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664628429552-3be8fcdc-208c-4cda-a6d9-dad092024eec.png#clientId=u10d9c343-1498-4&from=paste&height=133&id=ub02dc9b7&originHeight=199&originWidth=820&originalType=binary&ratio=1&rotation=0&showTitle=false&size=195596&status=done&style=none&taskId=ufd399e7f-6e17-41fa-a22f-bc2fa14cfce&title=&width=546.6666666666666)
安装
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664628450648-24b41107-d4e0-4156-bbec-46e5a3aff3b6.png#clientId=u10d9c343-1498-4&from=paste&height=122&id=u85ad5d59&originHeight=183&originWidth=770&originalType=binary&ratio=1&rotation=0&showTitle=false&size=120923&status=done&style=none&taskId=u017f5bbc-3c43-46e6-b02f-a098b5a578e&title=&width=513.3333333333334)
## 3.2 其他工具
### 1. GChisto
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1664628476859-9fac8a3b-2977-40f1-989a-304924e341e0.png#clientId=u10d9c343-1498-4&from=paste&height=77&id=ucd12ed70&originHeight=115&originWidth=805&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89465&status=done&style=none&taskId=u6d5d2900-1378-4d53-836f-f2bbffb97cc&title=&width=536.6666666666666)

1. 官网上没有下载的地方，需要自己从SVN上拉下来编译
2. 不过这个工具似乎没怎么维护了，存在不少bug
### 2. HPjmeter

1. 工具很强大，但是只能打开由以下参数生成的GC log，-verbose:gc -Xloggc:gc.log。添加其他参数生成的gc.log无法打开
2. HPjmeter集成了以前的HPjtune功能，可以分析在HP机器上产生的垃圾回收日志文件
