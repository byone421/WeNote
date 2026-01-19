# synchronized字节码分析
在 Java 中，可以使用 synchronized 关键字来实现线程之间的同步操作。当一个对象被 synchronized 关键字修饰的代码块或方法锁定时，其他线程无法进入这段代码块或方法，直到该对象的锁被释放为止。



当你编写一个使用 synchronized的 Java 方法时，Java 编译器（javac）在将源	代码编译为字节码（.class 文件）时，就已经在相应的位置插入了 monitorenter和 monitorexit指令。

接下来我们稍微看一下使用synchronized的几种情况，以及其对应的字节码：

## 情况1
现在我们写一段简单的代码，并查看他的字节码。

```java
public void hello(){
    synchronized (this){
       System.out.println("hello");
    }
}
```

该方法编译后的字节码如下：

```java
//将布局变量表中的第0位压入操作数栈，（因为这是一个普通方法，第0位存的是当前对象，所以就是将当前对象压入操作数栈）
0 aload_0
//复制当前对象引用并将其压入操作数栈
1 dup
//将复制的对象引用存储在局部变量表下标为1的位置
2 astore_1
//获取对象锁，进入同步块
3 monitorenter
//从System类的out字段中获取静态PrintStream对象的引用
4 getstatic #2 <java/lang/System.out : Ljava/io/PrintStream;>
//将常量池中索引为#5的字符串“hello”压栈
7 ldc #5 <hello>
//调用PrintStream对象的println()方法来打印堆栈顶部的字符串对象
9 invokevirtual #4 <java/io/PrintStream.println : (Ljava/lang/String;)V>
//从局部变量表1的位置中加载先前保存的对象引用
12 aload_1
//退出同步块并释放对象锁
13 monitorexit
//跳转到第 22 行代码（即方法返回处），跳过异常处理代码
14 goto 22 (+8)
//捕获任何异常并将其存储在局部变量表下标为2的位置。
17 astore_2
//从局部变量1中的位置加载对象引用。
18 aload_1
//退出同步块并释放对象锁
19 monitorexit
//将先前捕获的异常重新抛出
20 aload_2
//抛出异常
21 athrow
//正常返回方法
22 return
```



我们可以看到上述字节码中有一个monitorenter指令，但是有两个monitorexit指令。

这是因为在synchronized块中，即使您的代码中没有明显的异常，也可能存在隐式异常，例如NullPointerException等。因此，即使代码中没有明显的异常，也有可能在字节码层面上存在多个monitorexit指令。JVM需要确保监视器锁得到释放，以避免死锁。



## 情况2
现在我们再来看一下其他情况：

将代码加上 throw new RuntimeException();

```java
public void hello(){
     synchronized (this){
            System.out.println("hello");
            throw new RuntimeException();
     }
}
```

代码对应的字节码如下：

```java
//将布局变量表中的第0位压入操作数栈，（因为这是一个普通方法，第0位存的是当前对象，所以就是将当前对象压入操作数栈）
0 aload_0
//复制当前对象引用并将其压入操作数栈
1 dup
//将复制的对象引用存储在局部变量表下标为1的位置
2 astore_1
//获取对象锁，进入同步块
3 monitorenter
//从System类的out字段中获取静态PrintStream对象的引用
4 getstatic #2 <java/lang/System.out : Ljava/io/PrintStream;>
//将常量池中索引为#5的字符串“hello”压栈
7 ldc #5 <hello>
//调用PrintStream对象的println()方法来打印堆栈顶部的字符串对象
9 invokevirtual #4 <java/io/PrintStream.println : (Ljava/lang/String;)V>
//创建一个 RuntimeException 实例。
12 new #6 <java/lang/RuntimeException>
//复制操作数栈栈顶的值。并压栈（这里就是复制RuntimeException实例）
15 dup
//调用 RuntimeException 对象的构造函数。（调用init方法，消耗了一个实例）
16 invokespecial #7 <java/lang/RuntimeException.<init> : ()V>
//抛出栈顶的异常
19 athrow
//将操作数栈栈顶的数值存储到局部变量表中下标为 2 的位置。（这里放的还是RuntimeException实例）
20 astore_2
//将局部变量表中下标为 1 的元素加载到操作数栈中。
21 aload_1
//释放对象的监视器锁。
22 monitorexit
//将局部变量表中下标为 2 的变量压入操作数栈。
23 aload_2
//抛出栈顶的异常
24 athrow
```



我们可以看到显式抛出异常后，只有一个monitorenter和一个monitorexit。方法执行结束后自动释放锁。并把未处理的异常抛出。

## 情况3
现在我们用synchronized修改方法

```java
public synchronized void hello(){
        System.out.println("hello");
}
```

其对应的字节码如下

```java
0 getstatic #2 <java/lang/System.out : Ljava/io/PrintStream;>
3 ldc #3 <hello>
5 invokevirtual #4 <java/io/PrintStream.println : (Ljava/lang/String;)V>
8 return
```

可以看到同步方法在字节码层面没有monitorenter，monitorexit相关的指令

当synchronized锁修饰方法时，被修饰的方法会比普通方法的多一个ACC_SYNCHRONIZED 标识符，根据这个是否有这个标识来决定是否要获取锁对象。

## 总结
如果说看不懂上面的字节码其实对我们继续深入synchronized影响并不大。我们只需要知道，无论是`<font style="color:rgb(68, 71, 70);">monitorenter</font>``<font style="color:rgb(68, 71, 70);">monitorexit</font>`<font style="color:rgb(68, 71, 70);">还是</font>`<font style="color:rgb(68, 71, 70);">ACC_SYNCHRONIZED</font>`， 它们是 Java 虚拟机（JVM）实现同步（synchronized）的两种不同机制  

好了，下面总结一下：

| **<font style="color:rgb(31, 31, 31);">特性</font>** | **<font style="color:rgb(31, 31, 31);">同步代码块 (Synchronized Block)</font>** | **<font style="color:rgb(31, 31, 31);">同步方法 (Synchronized Method)</font>** |
| --- | --- | --- |
| **<font style="color:rgb(31, 31, 31);">字节码表现</font>** | <font style="color:rgb(31, 31, 31);">包含显式的 </font>`<font style="color:rgb(68, 71, 70);">monitorenter</font>`<br/><font style="color:rgb(31, 31, 31);"> 和 </font>`<font style="color:rgb(68, 71, 70);">monitorexit</font>` | <font style="color:rgb(31, 31, 31);">无特殊指令，仅有 </font>`<font style="color:rgb(68, 71, 70);">ACC_SYNCHRONIZED</font>`<br/><font style="color:rgb(31, 31, 31);"> 标志</font> |
| **<font style="color:rgb(31, 31, 31);">触发时机</font>** | <font style="color:rgb(31, 31, 31);">执行到指令时尝试获取 monitor</font> | <font style="color:rgb(31, 31, 31);">方法调用指令（如 </font>`<font style="color:rgb(68, 71, 70);">invokevirtual</font>`<br/><font style="color:rgb(31, 31, 31);">）识别到标志时</font> |
| **<font style="color:rgb(31, 31, 31);">锁的对象</font>** | <font style="color:rgb(31, 31, 31);">括号中指定的对象</font> | `<font style="color:rgb(68, 71, 70);">this</font>`<br/><font style="color:rgb(31, 31, 31);"> 对象（实例方法）或 </font>`<font style="color:rgb(68, 71, 70);">Class</font>`<br/><font style="color:rgb(31, 31, 31);"> 对象（静态方法）</font> |
| **<font style="color:rgb(31, 31, 31);">异常处理</font>** | <font style="color:rgb(31, 31, 31);">需要显式的异常路径来确保 </font>`<font style="color:rgb(68, 71, 70);">monitorexit</font>` | <font style="color:rgb(31, 31, 31);">由 JVM 隐式确保方法退出（无论正常还是异常）时释放锁</font> |


# 对象的内存布局
在继续深入synchronized之前，我们有必要先知道一下java对象的内存布局。

在HotSpot虚拟机里，对象在堆内存中的存储布局可以划分为三个部分：

1. 对象头（Header）
2. 实例 数据（Instance Data）
3. 对齐/内存填充（Padding）

对于synchronized我们重点需要关注一下对象头（Header）。那对于一个对象的对象头而言里面有什么呢？这里先以64位虚拟机说明一下。

Java对象头包括三部分：

1. Mark Work：Mark Word记录对象的HashCode、锁标志位和偏向锁信息等
2. 类型指针：指向该对象的类元数据
3. 数组长度：仅在对象为数组时存在，表示该数组的长度

<!-- 这是一张图片，ocr 内容为：数组对象 实例对象 MARK WORD(64BIT) MARK WORD(64BIT) 对象头 对象头 CLASS POINTER(64BIT) CLASS POINTER(64BIT) 数组长度(64BIT) 实例数据 实例数据 内存填充 内存填充 -->
![](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680526478389-752ffb34-2c69-4f9f-aefd-04e6b50da3c5.png)

但是由于存在指针压缩，对象头大小并不是说是固定的。这里可以查阅指针压缩的相关知识。



java中使用一个对象来作为一把锁，其锁的状态分为4种（由轻到重）：无锁->偏向锁->轻量锁->重量锁，锁状态的标志位就存储在对象头的Mark Word中最后2个比特位。

<!-- 这是一张图片，ocr 内容为：锁状态 1BIT 2BIT 1BIT 31BIT 4BIT 25BIT 偏向锁位 锁标志位 分代年龄 无锁 HASHCODE 01 UNUSED UNUSED 锁状态 1BIT 2BIT 2BIT 4BIT 1BIT 54BIT 偏向锁位 锁标志位 偏向锁位 偏向锁 0 1 01 UNUSED EPOCH 当前线程指针 锁状态 2BIT 62BIT 锁标志位 指向线程栈中LOCK RECORD的指针 轻量锁 00 锁状态 2BIT 62BIT 锁标志位 指向重量级锁(OBJECTMOINTER)的指针 重量锁 10 2BIT 62BIT 锁标志位 GC标记信息 11 -->
![](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680879901271-2a8d31e3-71b2-4278-86c3-85b58b706e82.png)

Mark Word 最后两位为 11 代表当前对象处于不可用的状态，即在进行垃圾回收时对象已经被标记为不可达

# synchronized锁的升级
通过上述了解，我们也知道了synchronized锁是有等级的。为了提高并发性能和减少锁竞争，Java从1.6版本开始引入了锁升级机制，将synchronized锁从偏向锁状态转换为轻量级锁状态、重量级锁状态等级别。锁升级的过程是自动进行的，开发者无需手动干预。Java虚拟机会根据当前锁的状态、竞争情况等因素自动决定锁的级别。下面由轻到重来讲解：

### 偏向锁（Biased Locking）
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1768790687752-d3d88f2b-e3e7-49c6-8c24-3e46ba56a34e.png)

 偏向锁中的“偏”，指的是“偏心、偏向”。它的含义是：锁会优先偏向第一个获取它的线程。如果在后续的执行过程中，没有其他线程来竞争这把锁，那么这个线程在整个使用期间都无需再进行任何同步操作。  

当一个线程获得了对象的锁并且这个对象没有被其他线程所访问时，该线程会进入偏向锁状态，并在对象头中记录下该线程的ID（如上图所示）。此时，如果其他线程想要访问该对象，只需要检查对象头中的线程ID是否与自己相同即可。

1. 如果是同一个线程请求获取锁，则表示该对象还没有被其他线程竞争过，JVM 会认为该线程仍然在执行同步方法，直接将偏向锁标记为有效
2. 如果不同，表示发生了竞争，已经有其他线程来访问了。这个时候会尝试用CAS来替换MarkWord里面的线程ID
3. 如果 CAS 操作成功，那么表示该线程已经获得了偏向锁，可以直接进入同步方法中执行同步操作。
4. 如果 CAS 操作失败，可能由于竞争太激烈或者存在其它线程已经持有偏向锁而导致，JVM 就需要撤销偏向锁，并尝试使用轻量级锁或重量级锁来保证线程安全

### 轻量级锁（Lightweight Locking）
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1768791328555-c766ea0b-5387-4b07-a420-638c3274f28e.png)

当多个线程竞争同一个锁时，会进入轻量级锁状态。此时，系统会在当前线程的栈帧中创建一个Lock Record（锁记录），并将对象头中的Mark Word复制到该锁记录中，并将对象头中的Mark Word指向该锁记录。然后，当前线程会尝试使用CAS原子操作来修改对象头的Mark Word为指向锁记录的指针。

1. 如果成功，当前线程就获得了锁并可以直接执行代码块。

<!-- 这是一张图片，ocr 内容为：OBJECT THREAD LOCK RECORD地址 00 CLASS POINTER LOCK RECORD 实例数据 MARKWORD信息 OBJECT REFERENCE 内存填充 -->
![](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680963199977-a4d22a1d-29a7-4bc2-9c3d-33792782dc01.png)



2. 如果CAS失败,有两种情况
    1. <font style="color:rgb(51,51,51);">如果是其它线程已经持有了该 Object 的轻量级锁，这时表明有竞争， 且通过 CAS 自旋无法成功获取锁时 ，会膨胀为重量级锁</font>
    2. 如果同一线程再次进入被 `synchronized ` 修饰的代码块（发生锁重入），JVM 不会重新竞争锁，而是在该线程的栈中再创建一个` obj` 为 `null` 的 Lock Record，用以表示一次重入层级。
    3. <font style="color:rgb(51,51,51);">当线程退出 </font>`<font style="color:rgb(51,51,51);">synchronized  </font>`<font style="color:rgb(51,51,51);">代码块时，JVM 会弹出当前对应的；如果该 Lock Record 的 obj 为 null，说明这是一次锁重入的退出，仅减少一层重入，不会真正释放锁；只有在退出最外层同步块、弹出最初的 Lock Record 时，才会执行真正的解锁操作。</font>
    4. <!-- 这是一张图片，ocr 内容为：OBJECT THREAD LOCK RECORD 00 NULL CLASS POINTER OBJECT REFERENCE 实例数据 MARK WORD信息 OBJECT REFERENCE 内存填充 -->
![](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680963513580-cb70b5ad-85af-4d56-bb70-dc669567d8ff.png)
    5. 当线程退出 `synchronized` 代码块进行解锁时，如果当前弹出的 Lock Record 的 `obj` 不为 `null`，JVM 会通过 CAS 操作尝试将对象头中的 Mark Word 恢复为锁前的状态;  
    i. 如果 CAS 成功，说明没有发生竞争，轻量级锁被正常释放；  
    ii. 如果 CAS 失败，则表明在持锁期间发生了竞争，轻量级锁已经膨胀为重量级锁，后续解锁将由重量级锁机制完成。
    6.  CAS 的自旋次数是自适应的：如果上一次自旋成功获得了锁，JVM 会增加下次的最大自旋次数，因为认为锁很快就能释放；反之，如果自旋多次未成功，JVM 会减少自旋次数，以避免线程在 CPU 上空转浪费资源。  

### 重量级锁（Heavyweight Locking）
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1768792663854-036196b4-ee7f-4922-8b3a-38dd708ddefc.png)

#### **Monitor锁**
 在介绍重量级锁之前，需要先了解一个概念：**Monitor 锁**。它是 JVM 层面的锁，由 C++ 实现。  
在底层，`synchronized` 会关联一个 `ObjectMonitor` 对象。当一个对象在重量级锁状态下被 `synchronized` 锁住时，该对象头的 **Mark Word** 会存储一个指向 `ObjectMonitor` 的指针，从而将对象与其对应的 `ObjectMonitor` 关联起来，实现真正的锁管理。  

其底层对应的objectMonitor代码可以在openjdk的官网和GitHub找到：

**官网：**

1. [https://openjdk.org/](https://openjdk.org/)
2. [https://hg.openjdk.org/jdk8u/jdk8u-jsse-incubator/hotspot/file/741cd0f77fac/src/share/vm/runtime/objectMonitor.hpp](https://hg.openjdk.org/jdk8u/jdk8u-jsse-incubator/hotspot/file/741cd0f77fac/src/share/vm/runtime/objectMonitor.hpp)

**gayhub:**

1. [https://github.com/openjdk/jdk/blob/master/src/hotspot/share/runtime/objectMonitor.hpp](https://github.com/openjdk/jdk/blob/master/src/hotspot/share/runtime/objectMonitor.cpp)

```java
  ObjectMonitor() {
    //初始值是0，用于判断当前对象是否被锁定。加锁+1，解锁-1
    _count        = 0;
    //锁重入次数
    _recursions   = 0;
    //锁定当前对象的线程ID
    _owner        = NULL;
    //等待队列，存放等待的线程
    _WaitSet      = NULL;
    //阻塞队列，存放阻塞的线程
    _EntryList    = NULL ;
  }
```

<!-- 这是一张图片，ocr 内容为：MONITOR 等待的线程 COUNT 当前线程 OWNER WAITSET 阻塞的线程 ENTRYLIST -->
![](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680880697614-2de10dc3-a816-41a1-8809-f5a31c7fedaf.png)**加锁流程简述：**

1. <font style="color:rgb(51,51,51);">判断_count是否为0</font>
    1. <font style="color:rgb(51,51,51);">为0：锁没有被占用，执行加锁操作，_count+1，</font>_recursions+1，<font style="color:rgb(51,51,51);"> _woner指向当前线程</font>
    2. <font style="color:rgb(51,51,51);">不为0：判断 _woner是否是当前线程</font>
        1. <font style="color:rgb(51,51,51);">是：直接进入同步方法</font>
        2. <font style="color:rgb(51,51,51);">否：进入阻塞队列</font>
2. <font style="color:rgb(51,51,51);"> 当前线程执行完同步代码块的内容，然后唤醒 _EntryList 中等待的线程来竞争锁，竞争的时是非公平的</font>

#### 升级成为重量级锁
 当多个线程竞争同一个锁，并且轻量级锁无法通过自旋成功获取时，锁会膨胀为重量级锁。此时，JVM 会在内存中为该对象分配一个 **ObjectMonitor** 对象，并将对象头的 Mark Word 指向它，从而将对象与 Monitor 锁关联起来，实现线程间的互斥和等待管理。  

1. <font style="color:rgb(51,51,51);">当 Thread-1 进行轻量级加锁时，Thread-0 已经对该对象加了轻量级锁</font>
    1. <font style="color:rgb(51,51,51);">Thread-1将对象引用指向了Object，然后尝试CAS交换MarkWord,发现Object的MarkWord 最后两位已经是00了，已经被其他轻量级锁占用了。所以Thread-1CAS失败。</font>

<!-- 这是一张图片，ocr 内容为：THREAD1 OBJECT THREADO LOCK RECORD 地址 00 CLASS POINTER CAS失败 实例数据 LOCK RECORD 地址 MARK WORD信息 OBJECT REFERENCE OBJECT REFERENCE 内存填充 -->
![](https://cdn.nlark.com/yuque/0/2023/png/12600036/1681018460993-11a35e8c-70cc-43a8-8b6c-7549bf140734.png)

2. 由于<font style="color:rgb(51,51,51);">Thread-1CAS失败，锁会膨胀成为重量级锁</font>
    1. <font style="color:rgb(51,51,51);">即为 Object 对象申请 Monitor 锁，让 Object 指向重量级锁地址。</font>
    2. <font style="color:rgb(51,51,51);">然后Thread-1自己进入 Monitor 的 EntryList BLOCKED进行等待。</font>

<!-- 这是一张图片，ocr 内容为：OBJECT THREADO MONITOR MONITOR地址 COUNT CLASS POINTER OWNER 实例数据 WAITSET MARK WORD信息 THREAD1 ENTRYLIST OBJECT REFERENCE 内存填充 -->
![](https://cdn.nlark.com/yuque/0/2023/png/12600036/1681019304335-dd5a8d1b-e765-400c-8f66-99cc80191fe9.png)

3. <font style="color:rgb(51,51,51);">当 Thread-0 退出同步块解锁时，使用 cas 将 Mark Word 的值恢复给对象头，会失败。</font>
4. <font style="color:rgb(51,51,51);"> 此时，线程会进入重量级解锁流程：通过对象头找到对应的 </font>**ObjectMonitor**<font style="color:rgb(51,51,51);">，将 Monitor 的 </font>`<font style="color:rgb(51,51,51);">Owner</font>`<font style="color:rgb(51,51,51);"> 设置为 </font>`<font style="color:rgb(51,51,51);">null</font>`<font style="color:rgb(51,51,51);">，并唤醒 Monitor 中 </font>**EntryList**<font style="color:rgb(51,51,51);"> 队列里的阻塞线程，让它们重新竞争锁。  </font>



# 4.锁和HashCode
在锁升级为轻量级或者重量级锁后，Mark Word中保存的分别是线程栈帧里的锁记录指针和重量级指针，已经没有位置保存HashCode，GC年龄了，那这些信息去哪里了呢？

1. 对于无锁状态：当对象第一次调用hashCode()方法，jvm会生成hashcode并存在Mark Word中。
2. 对于偏向锁：如果一个对象已经调用过hashCode()方法，则这个对象不能被设置偏向锁。如果是在偏向锁的状态下，调用hashCode()方法，会造成锁的升级。它的偏向状态会被立即撤销，并且锁会膨胀为重量级锁。
3. 对于轻量级锁：JVM会在当前线程的栈帧记录中创建一个锁记录空间。用于存储MarkWord的信息，锁释放后还原回去。
4. 对于重量级锁：对象MardWord信息则被保存在Monitor对象中，锁释放后还原回去。

# 5.锁的其他优化
## 自旋锁/自适应
JVM的自旋次数是通过PreBlockSpin参数控制。这个参数可以在如下网址看到

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1768801393377-d0de39ff-d576-435d-b59a-92772983ecbb.png)[https://www.oracle.com/java/technologies/javase/vmoptions-jsp.html](https://www.oracle.com/java/technologies/javase/vmoptions-jsp.html)

在自适应锁出现之前，JVM 的自旋次数是“一刀切”的。而自适应自旋将这种固定次数进化成了基于历史经验的动态预测， JVM默认的自旋次数是10。

**1. 动态增加**

如果一个线程在某个锁对象上，刚刚成功地通过自旋获得过锁，且当前持有锁的线程正在运行中。

+ 动作： JVM 会认为这次自旋成功的概率很高。
+ 结果： 动态地增加自旋次数（例如从默认的 10 次增加到 50 次甚至 100 次）。
+ 目的： 尽量通过自旋拿到锁，避免线程切入内核态导致挂起，提升效率。

**2. 动态减少/取消**

如果对于某个锁，自旋很少成功获得过。

+ 动作： JVM 认为这个锁竞争太激烈，或者持有锁的时间太长，自旋只是在浪费 CPU。
+ 结果： 动态地减少自旋次数，甚至在下一次直接跳过自旋阶段。
+ 目的： 节省 CPU 资源，直接让线程进入阻塞状态，等待操作系统唤醒。

## 锁消除
当 JVM 的即时编译器（JIT）在运行时检测到某些代码虽然使用了锁，但其实根本不存在共享数据竞争时，就会把这个锁删掉。这主要依靠逃逸分析（Escape Analysis）。如果 JVM 发现一个对象只会在当前线程内部使用（不会逃逸到其他线程），那给它加锁就是白费力气。

 我们常用的 `StringBuffer` 是线程安全的，它的 `append` 方法带了 `synchronized`。下面是一个栗子：

```java
public String concatString(String s1, String s2) {
    // StringBuffer 是局部变量，不会被其他线程访问
    // 它属于“非逃逸对象”
    StringBuffer sb = new StringBuffer(); 
    sb.append(s1);
    sb.append(s2);
    return sb.toString();
}
```

在上述代码中，`sb` 对象只在 `concatString` 方法内部有效。JVM最终会发现：没有任何其他线程能访这个 `sb`。此时，它会大胆地将 `append` 方法内部的同步锁直接消除。

## 锁粗化
原则上我们建议同步块越小越好（只在必要时加锁）。但如果 JVM 发现一系列连续的操作都对**同一个对象**反复加锁、解锁，甚至锁出现在循环体内部，它就会把加锁的范围**扩大**。原因是因为频繁地“获取-释放”锁会产生大量的资源消耗。为了减少这种无谓的性能损耗，JVM 会将多个连续的锁合并成一个范围更大的锁。

比如说下面这段代码：

```java
for (int i = 0; i < 1000; i++) {
    synchronized(lock) {
        // do something
    }
}
```

会优化成：

```java
synchronized(lock) {
    for (int i = 0; i < 1000; i++) {
         // do something
    }
}
```

# 6.其他
对于偏向锁，在JDK15标记为弃用，在JDK17进行相关实现逐步移除。在java的技术浪潮中已经成为了前浪。主要原因是 它的性能优势在现代硬件和 JVM 优化下越来越小，而实现复杂度高。







