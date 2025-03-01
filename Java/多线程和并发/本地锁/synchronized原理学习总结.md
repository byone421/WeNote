# synchronized字节码分析
在 Java 中，可以使用 synchronized 关键字来实现线程之间的同步。当一个对象被 synchronized 关键字修饰的代码块或方法锁定时，其他线程无法进入这段代码块或方法，直到该对象的锁被释放为止。

当执行进入同步代码块时，Java 虚拟机会自动插入 monitorenter 指令，在退出同步代码块时则会插入 monitorexit 指令。
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
# Java对象头
一些与锁有关的信息会被记录在Java对象头的Mark Work中。
那对于一个对象的对象头而言里面有什么这里先以64位虚拟机说明一下。
Java对象头包括三部分：

1. Mark Work：Mark Word记录对象的HashCode、锁标志位和偏向锁信息等
2. 类型指针：指向该对象的类元数据
3. 数组长度：仅在对象为数组时存在，表示该数组的长度

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680526478389-752ffb34-2c69-4f9f-aefd-04e6b50da3c5.png#averageHue=%23fcfcfc&clientId=uc78f1aa5-b30e-4&from=paste&height=518&id=u4a6ef803&name=image.png&originHeight=777&originWidth=1258&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=49288&status=done&style=none&taskId=u2774589b-810c-4ce4-948a-80336c6d9e2&title=&width=838.6666666666666)
但是由于存在指针压缩，对象头可能并不是说一定是固定的。这里可以查找指针压缩的相关知识。

锁标志位就存储在Mark Word里面：
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680879901271-2a8d31e3-71b2-4278-86c3-85b58b706e82.png#averageHue=%23e6e8eb&clientId=u608e5dae-fb8d-4&from=paste&height=769&id=uf2a4f172&name=image.png&originHeight=1154&originWidth=1815&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=228316&status=done&style=none&taskId=u21e8bb6f-9166-434b-8d1d-a7555c21d19&title=&width=1210)
Mark Word 最后两位为 11 代表当前对象处于不可用的状态，即在进行垃圾回收时对象已经被标记为不可达

# Monitor锁
我们都知道，synchronized底层关联了一个ObjectMonitor对象。如果使用 synchronized 给对象上锁（重量级锁的情况下。）该对象头的 Mark Word 中就会设置一个指向ObjectMonitor对象指针。从而达到对象和ObjectMonitor相关联。
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
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680880697614-2de10dc3-a816-41a1-8809-f5a31c7fedaf.png#averageHue=%23fdf5ee&clientId=u608e5dae-fb8d-4&from=paste&height=393&id=u08b804b1&name=image.png&originHeight=589&originWidth=1394&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=61927&status=done&style=none&taskId=u6f759a42-b66c-43c1-9b46-19644c7a5fd&title=&width=929.3333333333334)
**流程简述：**

1. 判断_count是否为0
   1. 为0：锁没有被占用，执行加锁操作，_count+1，_recursions+1， _woner指向当前线程
   2. 不为0：判断 _woner是否是当前线程
      1. 是：直接进入同步方法
      2. 否：进入阻塞队列
2.  当前线程执行完同步代码块的内容，然后唤醒 _EntryList 中等待的线程来竞争锁，竞争的时是非公平的
# 锁的升级
为了提高并发性能和减少锁竞争，Java从1.6版本开始引入了锁升级机制，将synchronized锁从偏向锁状态转换为轻量级锁状态、重量级锁状态等级别。锁升级的过程是自动进行的，开发者无需手动干预。Java虚拟机会根据当前锁的状态、竞争情况以及硬件的支持情况等因素自动决定锁的级别。
**升级过程是从无锁-->偏向锁-->轻量锁-->重量锁**
### 偏向锁（Biased Locking）
它适用于只有一个线程访问对象的情况。当一个线程获得了对象的锁并且这个对象没有被其他线程所访问时，
该线程会进入偏向锁状态，并在对象头中记录下该线程的ID。此时，如果其他线程想要访问该对象，只需要检查对象头中的线程ID是否与自己相同即可。

1. 如果是同一个线程请求获取锁，则表示该对象还没有被其他线程竞争过，JVM 会认为该线程仍然在执行同步方法，直接将偏向锁标记为有效
2. 如果不同，表示发生了竞争，已经有其他线程来访问了。这个时候会尝试用CAS来替换MarkWord里面的线程ID
3. 如果 CAS 操作成功，那么表示该线程已经获得了偏向锁，可以直接进入临界区执行同步操作。如果 CAS 操作成功，那么表示该线程已经获得了偏向锁，可以直接进入临界区执行同步操作。
4. 如果 CAS 操作失败，可能由于竞争太激烈或者存在其它线程已经持有偏向锁而导致，JVM 就需要撤销偏向锁，并尝试使用轻量级锁或重量级锁来保证线程安全
### 轻量级锁（Lightweight Locking）
当多个线程竞争同一个锁时，会进入轻量级锁状态。此时，系统会在当前线程的栈帧中创建一个Lock Record（锁记录），并将对象头中的Mark Word复制到该锁记录中，并将对象头中的Mark Word指向该锁记录。然后，当前线程会尝试使用CAS原子操作来修改对象头的Mark Word为指向锁记录的指针。

1. 如果成功，当前线程就获得了锁并可以直接执行代码块。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680963199977-a4d22a1d-29a7-4bc2-9c3d-33792782dc01.png#averageHue=%236eaa46&clientId=u608e5dae-fb8d-4&from=paste&height=537&id=u585230a6&name=image.png&originHeight=805&originWidth=1330&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=36419&status=done&style=none&taskId=u35c2b396-0632-429c-8b86-caba262ce18&title=&width=886.6666666666666)

2. 如果CAS失败,有两种情况
   1. 如果是其它线程已经持有了该 Object 的轻量级锁，这时表明有竞争，会膨胀为重量级锁
   2. 如果是自己执行了 synchronized 锁重入，那么再添加一条 Lock Record 作为重入的计数
   3. 当退出 synchronized 代码块的时候，如果有取值为 null 的锁记录，表示有重入，这时重置锁记录，表示重入计数减一
   4. ![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1680963513580-cb70b5ad-85af-4d56-bb70-dc669567d8ff.png#averageHue=%236eaa46&clientId=u608e5dae-fb8d-4&from=paste&height=547&id=u66096d98&name=image.png&originHeight=820&originWidth=1261&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=36117&status=done&style=none&taskId=u5c08b14a-aea9-4361-a758-cd936e56b1a&title=&width=840.6666666666666)
   5. 当退出 synchronized 代码块（解锁时）锁记录的值不为 null，这时使用 CAS 将 Mark Word 的值恢复给对象头
      1. 如果成功，则解锁成功
      2. 如果失败，说明轻量级锁进行了锁膨胀

CAS的次数其实是自适应的，如果线程自旋成功了，那么下次自旋的最大次数会增加，JVM认为你上次成功了，那么这一次也很大概率会成功，相反如果很少自旋成功，那么次数就会减少，避免CPU空转。
### 重量级锁（Heavyweight Locking）
当多个线程竞争同一个锁时，且轻量级锁升级失败，则会进入重量级锁状态。此时，系统会在内存中分配一个互斥量（Mutex），并将该互斥量与对象关联起来。也就是上面所说的Monitor锁。

1. 当 Thread-1 进行轻量级加锁时，Thread-0 已经对该对象加了轻量级锁
   1. Thread-1将对象引用指向了Object，然后尝试CAS交换MarkWord,发现Object的MarkWord 最后两位已经是00了，已经被其他轻量级锁占用了。所以Thread-1CAS失败。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1681018460993-11a35e8c-70cc-43a8-8b6c-7549bf140734.png#averageHue=%236eaa46&clientId=u608e5dae-fb8d-4&from=paste&height=475&id=ufe6e5860&name=image.png&originHeight=713&originWidth=1963&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=50813&status=done&style=none&taskId=u0033cd89-4972-4800-8441-845f40a7644&title=&width=1308.6666666666667)

2. 由于Thread-1CAS失败，锁会膨胀成为重量级锁
   1. 即为 Object 对象申请 Monitor 锁，让 Object 指向重量级锁地址。
   2. 然后Thread-1自己进入 Monitor 的 EntryList BLOCKED进行等待。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1681019304335-dd5a8d1b-e765-400c-8f66-99cc80191fe9.png#averageHue=%236eaa46&clientId=u608e5dae-fb8d-4&from=paste&height=522&id=u5318d34d&name=image.png&originHeight=783&originWidth=1956&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=52434&status=done&style=none&taskId=uc3921150-6e4e-4d35-8066-95cfd3b9be5&title=&width=1304)

3. 当 Thread-0 退出同步块解锁时，使用 cas 将 Mark Word 的值恢复给对象头，会失败。
4. 这时会进入重量级解锁 流程，即按照 Monitor 地址找到 Monitor 对象，将自己的Markd Word信息记录在Monitor, 设置 Owner 为 null，唤醒 EntryList 中 BLOCKED 线程

# 锁和HashCode
在锁升级为轻量级或者重量级锁后，Mark Word中保存的分别是线程栈帧里的锁记录指针和重量级指针，已经没有位置保存HashCode，GC年龄了，那这些信息去哪里了呢？

1. 对于无锁状态：当对象第一次调用hashCode()方法，jvm会生成hashcode并存在Mark Word中。
2. 对于偏向锁：如果一个对象已经调用过hashCode()方法，则这个对象不能被设置偏向锁。如果是在偏向锁的状态下，调用hashCode()方法，会造成锁的升级。
3. 对于轻量级锁：JVM会在当前线程的栈帧记录中创建一个锁记录空间。用于存储MarkWord的信息，锁释放后还原回去。
4. 对于重量级锁：对象MardWord信息则被保存在Monitor对象中，锁释放后还原回去。
# 其他
在JDK 15偏向锁已经被标记过时。




