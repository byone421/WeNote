# AOP 实现之 ajc 编译器
## 一个Service
```java
@Service
public class MyService {

    private static final Logger log = LoggerFactory.getLogger(MyService.class);

    public static void foo() {
        log.debug("foo()");
    }
}

```
## 一个切面类
```java
@Aspect // ⬅️注意此切面并未被 Spring 管理
public class MyAspect {

    private static final Logger log = LoggerFactory.getLogger(MyAspect.class);

    @Before("execution(* com.itheima.service.MyService.foo())")
    public void before() {
        log.debug("before()");
    }
}

```
## 测试类
```java

/*
    注意几点
    1. 版本选择了 java 8, 因为目前的 aspectj-maven-plugin 1.14.0 最高只支持到 java 16
    2. 一定要用 maven 的 compile 来编译, idea 不会调用 ajc 编译器
 */
@SpringBootApplication
public class A09 {

    private static final Logger log = LoggerFactory.getLogger(A09.class);

    public static void main(String[] args) {
//        ConfigurableApplicationContext context = SpringApplication.run(A10Application.class, args);
//        MyService service = context.getBean(MyService.class);
//        //编译时期增强并不会生成代理对象
//        log.debug("service class: {}", service.getClass());
//        service.foo();
//
//        context.close();

        new MyService().foo();
    }
}

```
## POM 文件
```java
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>aspectj-maven-plugin</artifactId>
                <version>1.14.0</version>
                <configuration>
                    <complianceLevel>1.8</complianceLevel>
                    <source>8</source>
                    <target>8</target>
                    <showWeaveInfo>true</showWeaveInfo>
                    <verbose>true</verbose>
                    <Xlint>ignore</Xlint>
                    <encoding>UTF-8</encoding>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <!-- use this goal to weave all your main classes -->
                            <goal>compile</goal>
                            <!-- use this goal to weave all your test classes -->
                            <goal>test-compile</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```
## 查看class
使用Aspectj静态编译之后Myservice得到了增强。在编译阶段进行了增强
```java
@Service
public class MyService {
    private static final Logger log = LoggerFactory.getLogger(MyService.class);

    public MyService() {
    }

    public static void foo() {
        MyAspect.aspectOf().before();
        log.debug("foo()");
    }
}
```
## 注意

- 版本选择了 java 8, 因为目前的 aspectj-maven-plugin 1.14.0 最高只支持到 java 16
- 一定要用 maven 的 compile 来编译, idea 不会调用 ajc 编译器
# 总结

1. aop 的原理并非代理一种, 编译器也能玩出花样
2. 编译器也能修改 class 实现增强
3. 编译器增强能突破代理仅能通过方法重写增强的限制：可以对构造方法、静态方法等实现增强
