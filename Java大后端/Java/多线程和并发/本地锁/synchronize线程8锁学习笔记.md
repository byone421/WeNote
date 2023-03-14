通过8种情况演示锁运行案例，看看我们到底锁的是什么。
输出结果我会放在最底下。可以思考一下，然后对照，代码都是基于Java8运行。
# 案例：
**情况1：标准访问有a b两个线程，请问先打印邮件还是短信 ? **
```java
class Phone{
    public  synchronized void sendEmail(){
        
        System.out.println("-------------sendEmail");

    }
    public  synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone.sendSMS();
        },"b").start();

    }
}

```

**情况2：sendEmail故意停3秒？，请问先打印邮件还是短信 ? **
```java
class Phone{
    public  synchronized void sendEmail(){
        try {TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {e.printStackTrace();}
        System.out.println("-------------sendEmail");

    }
    public  synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone.sendSMS();
        },"b").start();

    }
}
```
**情况3：添加一个普通的hello方法，请问先打印邮件还是hello**
```java
class Phone{
    public  synchronized void sendEmail(){
        try {TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {e.printStackTrace();}
        System.out.println("-------------sendEmail");

    }
    public  synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

    public void hello(){
        System.out.println("---------hello");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone.hello();
        },"b").start();

    }
}
```
**情况4：有2部phone**_**，**_**请问先打印邮件还是短信 ? **
```java
class Phone{
    public  synchronized void sendEmail(){
        try {TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {e.printStackTrace();}
        System.out.println("-------------sendEmail");

    }
    public  synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

    public void hello(){
        System.out.println("---------hello");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        Phone phone2 = new Phone();
        
        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone2.sendSMS();
        },"b").start();

    }
}
```
**情况5：有2个静态同步方法，有一部phone_，_请问先打印邮件还是短信 ? **
```java
class Phone{
    public  static synchronized void sendEmail(){
        try {TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {e.printStackTrace();}
        System.out.println("-------------sendEmail");

    }
    public  static synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

    public void hello(){
        System.out.println("---------hello");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();


        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone.sendSMS();
        },"b").start();

    }
}

```
**情况6：有2个静态同步方法，有两部phone_，_请问先打印邮件还是短信 ? **
```java

class Phone{
    public  static synchronized void sendEmail(){
        try {TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {e.printStackTrace();}
        System.out.println("-------------sendEmail");

    }
    public  static synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

    public void hello(){
        System.out.println("---------hello");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        Phone phone2 = new Phone();


        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone2.sendSMS();
        },"b").start();

    }
}
```
**情况7：有1个静态同步方法，1个普通同步方法，有1部phone_，_请问先打印邮件还是短信 ? **
```java
class Phone{
    public  static synchronized void sendEmail(){
        try {TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {e.printStackTrace();}
        System.out.println("-------------sendEmail");

    }
    public   synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

    public void hello(){
        System.out.println("---------hello");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();



        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone.sendSMS();
        },"b").start();

    }
}

```

**情况8：有1个静态同步方法，1个普通同步方法，有2部phone_，_请问先打印邮件还是短信 ? **
```java
class Phone{
    public  static synchronized void sendEmail(){
        try {TimeUnit.SECONDS.sleep(3);} catch (InterruptedException e) {e.printStackTrace();}
        System.out.println("-------------sendEmail");

    }
    public   synchronized void sendSMS(){
        System.out.println("-------------sendSMS");
    }

    public void hello(){
        System.out.println("---------hello");
    }

}

public class Lock8 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        Phone phone2 = new Phone();


        new Thread(()->{
            phone.sendEmail();
        },"a").start();

        //暂停200ms 保证a先启动
        try {TimeUnit.MILLISECONDS.sleep(200);} catch (InterruptedException e) {e.printStackTrace();}

        new Thread(()->{
            phone2.sendSMS();
        },"b").start();

    }
}

```
# 锁的说明：
对于被synchronize关键字修饰普通方法。锁是this。
对于被synchronize关键字修饰static方法。锁是类对象。
对于同步方法块。锁的是synchronize（xxx）括号内的对象。
如果锁对象不同，就不会产生锁的竞争。
# 8种情况解释说明：
**情况1,2：**
sendEmail 和sendSMS 都是同步方法。而且锁对象都是phone。所以会产生竞争，从代码执行顺序来看，
sendEmail 先执行，那就先获得锁。sendSMS 则等待sendEmail  执行完毕。
**情况3：**
hello是个普通方法并未和synchronize修饰的sendEmail方法产生锁的争抢。但sendEmail中睡了3秒，所以hello先输出。
**情况4：**
有两个phone锁对象，sendEmail 和 sendSMS没有锁的竞争。但sendEmail中睡了3秒，所以sendSMS先输出。
**情况5，6：**
sendEmail 和 sendSMS 都是静态同步方法，锁对象都是当前类对象。sendEmail 会先抢到锁，sendSMS 进行等待。sendEmail先执行。
**情况7：**
sendEmail 是静态同步方法，锁对象都是当前类对象。sendSMS 是普通同步方法，锁对象是this。两者没有锁的竞争。但sendEmail中睡了3秒，所以sendSMS先输出。
**情况8：**
虽然有两个phone对象，但是道理还是和情况7一样。
# 运行结果：
**情况1:**
> -------------sendEmail
> -------------sendSMS

**情况2：**
> -------------sendEmail
> -------------sendSMS

**情况3：**
> ---------hello
> -------------sendEmail

**情况4：**
> -------------sendSMS
> -------------sendEmail

**情况5：**
> -------------sendEmail
> -------------sendSMS

**情况6：**
> -------------sendEmail
> -------------sendSMS

**情况7：**
> -------------sendSMS
> -------------sendEmail

**情况8：**
> -------------sendSMS
> -------------sendEmail

