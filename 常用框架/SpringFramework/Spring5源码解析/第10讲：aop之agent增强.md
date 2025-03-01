# agent增强是指在类加载的过程中进行增强
启动需要加上代理：
> -javaagent: C:/Users/manyh/.m2/repository/org/aspectj/aspectjweaver/1.9.7/aspectjweaver-1.9.7.jar

路径换成自己的jar包路径
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1699238041197-44d73b5b-eefd-4a62-80d4-013c332dd1ad.png#averageHue=%232c2e33&clientId=u8c0083e2-4796-4&from=paste&height=816&id=u5549a04f&originHeight=734&originWidth=1535&originalType=binary&ratio=0.8999999761581421&rotation=0&showTitle=false&size=185967&status=done&style=none&taskId=u86334f7e-e314-4a57-8632-6e54d3ca420&title=&width=1705.5556007373493)
## 测试代码
```java
/*
    注意几点
    1. 版本选择了 java 8, 因为目前的 aspectj-maven-plugin 1.14.0 最高只支持到 java 16
    2. 运行时需要在 VM options 里加入 -javaagent:C:/Users/manyh/.m2/repository/org/aspectj/aspectjweaver/1.9.7/aspectjweaver-1.9.7.jar
        把其中 C:/Users/manyh/.m2/repository 改为你自己 maven 仓库起始地址
 */
@SpringBootApplication
public class A10 {

    private static final Logger log = LoggerFactory.getLogger(A10.class);

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(A10.class, args);
        MyService service = context.getBean(MyService.class);

        // ⬇️MyService 并非代理, 但 foo 方法也被增强了, 做增强的 java agent, 在加载类时, 修改了 class 字节码
        log.debug("service class: {}", service.getClass());
        service.foo();
    }
}

```

## [arthas](https://github.com/alibaba/arthas)查询增强后的代码
官网：[https://github.com/alibaba/arthas](https://github.com/alibaba/arthas)
通过阿尔萨斯的jad命令反编译运行中的类，查看MyService增强后的代码
下载后运行jar包（官网有说明）
## 选择进程
这里输入** 2**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1699238135680-a92acac4-d3e1-4d21-bb0e-30f24dbeb906.png#averageHue=%23171717&clientId=u8c0083e2-4796-4&from=paste&height=317&id=fHtQr&originHeight=285&originWidth=985&originalType=binary&ratio=0.8999999761581421&rotation=0&showTitle=false&size=35224&status=done&style=none&taskId=u1447b122-0ae5-49a1-a1fd-bfd0ca15f53&title=&width=1094.444473437322)
## 使用jad命令查看
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1699238218793-7a8aaa22-199d-4ac4-845b-5d483a95b385.png#averageHue=%230f0f0f&clientId=u8c0083e2-4796-4&from=paste&height=851&id=udcbf31d2&originHeight=766&originWidth=1141&originalType=binary&ratio=0.8999999761581421&rotation=0&showTitle=false&size=72113&status=done&style=none&taskId=ucbf70389-9570-4286-b9d3-b32e4b1a58a&title=&width=1267.7778113624204)

# 总结：
AOP 底层实现方式之一是代理，由代理结合通知和目标，提供增强功能
除此以外，aspectj 提供了两种另外的 AOP 底层实现：

-  第一种是通过 ajc 编译器在**编译** class 类文件时，就把通知的增强功能，织入到目标类的字节码中 
-  第二种是通过 agent 在**加载**目标类时，修改目标类的字节码，织入增强功能 
-  作为对比，之前学习的代理是**运行**时生成新的字节码 

简单比较的话：

- aspectj 在编译和加载时，修改目标字节码，性能较高
- aspectj 因为不用代理，能突破一些技术上的限制，例如对构造、对静态方法、对 final 也能增强
- 但 aspectj 侵入性较强，且需要学习新的 aspectj 特有语法，因此没有广泛流行
