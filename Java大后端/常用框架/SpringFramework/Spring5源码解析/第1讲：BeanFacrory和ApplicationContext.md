# BeanFacrory和ApplicationContext
借助SpringBoot的引导类来讲BeanFactory 与 ApplicationContext 的区别
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
System.out.println(context);
```
选中 **ConfigurableApplicationContext** 按住 **ctrl+alt+u**弹出类图![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677421068228-c3f0e0b4-313b-4704-9ebe-270f2e444c90.png#averageHue=%23c3ecc9&clientId=uc6787cfa-ff4c-4&from=paste&height=337&id=ud45bdd49&name=image.png&originHeight=506&originWidth=2246&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=45208&status=done&style=none&taskId=u2d32768b-d757-4ae0-ae25-47cbfe5224c&title=&width=1497.3333333333333#averageHue=%23c3ecc9&from=url&id=uYWev&originHeight=506&originWidth=2246&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)可以看到 ApplicaionContext 间接继承了BeanFactory，也就是说对BeanFactory进行了功能上的扩展。主要体现在下图。 每继承一个父类就说明具备了一个新能力。说明ApplicationContext的功能比BeanFactory多。![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677421789709-e04e086f-5694-44ec-b637-f6b318f42274.png#averageHue=%23c3ecc9&clientId=u53e4a65b-5f67-4&from=paste&height=335&id=u22cfdeb6&name=image.png&originHeight=502&originWidth=2256&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=42505&status=done&style=none&taskId=uf20b72ae-db94-4849-9c37-d3891deb880&title=&width=1504#averageHue=%23c3ecc9&from=url&id=J2azn&originHeight=502&originWidth=2256&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)
BeanFactory才是Spring的核心容器。ApplicationContext 实现都【组合】了它的功能。
比如getBean：context.getBean("aaa");
按住 **ctrl +alt +b**调转到实现类![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677422356944-3429e3db-38e0-44ed-9756-309d19090747.png#averageHue=%23c9eecd&clientId=u53e4a65b-5f67-4&from=paste&height=249&id=u910a13e8&name=image.png&originHeight=374&originWidth=1225&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=54889&status=done&style=none&taskId=uf6ba20eb-9834-4aa4-898e-e5c14475d63&title=&width=816.6666666666666#averageHue=%23c9eecd&from=url&id=es4q4&originHeight=374&originWidth=1225&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)可以看到先拿到 beanFactory再去调用getBean()，也就说ApplictionContext扩展 beanFactory是通过组合的方式。功能是人家beanFactory提供的。从这可以看出beanFactory更为核心一些。
最后通过断点查看到ApplicaionContext有个成员就是beanFactory。![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677426834042-9432f6a0-d257-4cb6-895a-5a7e64b4d487.png#averageHue=%23f5f0d1&clientId=u53e4a65b-5f67-4&from=paste&height=563&id=u2e620c6f&name=image.png&originHeight=845&originWidth=1817&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=162169&status=done&style=none&taskId=ud6915e88-b119-4076-9d06-aff60adf4c2&title=&width=1211.3333333333333#averageHue=%23f5f0d1&from=url&id=n4ES8&originHeight=845&originWidth=1817&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)bean管理的方方面面都是通过BeanFactory来管的，beanFactory里面有个singletonObjects，用来保存容器中的单例对象。![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677427064253-3e01af76-1b55-4841-889a-30770f84c3a8.png#averageHue=%23f9f7f5&clientId=u53e4a65b-5f67-4&from=paste&height=492&id=u1c1c20e1&name=image.png&originHeight=738&originWidth=1563&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=119089&status=done&style=none&taskId=uc69e2f64-6a16-4efc-b4a3-336a84860fa&title=&width=1042#averageHue=%23f9f7f5&from=url&id=R6tV2&originHeight=738&originWidth=1563&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)
# BeanFactory的功能
找到 **BeanFactory** 按住 **F12** 可以查看这个接口或者类的全部方法![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677920753200-eaf6a182-6e68-4f1f-83b8-7ba8dad65044.png#averageHue=%23faf9f8&clientId=u4cd225a2-c7f9-4&from=paste&height=693&id=ub9950cf6&name=image.png&originHeight=1040&originWidth=1284&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=77081&status=done&style=none&taskId=u36627490-0840-4209-8cc0-977f6b7a1d3&title=&width=856#averageHue=%23faf9f8&from=url&id=wGOzW&originHeight=1040&originWidth=1284&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)可以看到方法比较少，表面上只有getBean()。但是不能只看接口，实际上控制反转、基本的依赖注入、直至 Bean 的生命周期的各种功能, 都由它的实现类提供。
这里稍微提一下 **DefaultListableBeanFactory**按住 **ctrl + alt+ u**  查看继承关系图。![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677921312782-41c2dd8c-0dad-49e2-bf93-d31df2dca9eb.png#averageHue=%23c4ecca&clientId=u4cd225a2-c7f9-4&from=paste&height=541&id=ue8ecd68c&name=image.png&originHeight=812&originWidth=2069&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=72464&status=done&style=none&taskId=u97ababe3-38c1-4693-86e7-8686ea869a9&title=&width=1379.3333333333333#averageHue=%23c4ecca&from=url&id=s8jpY&originHeight=812&originWidth=2069&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)可以看到BeanFactory只是众多父接口中的一个而已，提供了getBean。
其他接口则是提供了其他的功能：SingletonBeanRegistry （能注册单例），HierarchicalBeanFactory （和父子容器相关），ListableBeanFactory （可以列出容器中的所有bean），BeanDefinitionRegistry （用来注册新的bean）。
DefaultListableBeanFactory可以管理所有的bean，其中大家最为关心的就是单例bean的管理。这个在他父类DefaultSingletonBeanRegistry中。![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677921979660-95c643b7-5775-4436-a966-23206d72564d.png#averageHue=%23c6edcc&clientId=u4cd225a2-c7f9-4&from=paste&height=665&id=ud1cd224d&name=image.png&originHeight=997&originWidth=1648&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=136881&status=done&style=none&taskId=u709e9663-3a28-4f0a-a9ea-db1081179dd&title=&width=1098.6666666666667#averageHue=%23c6edcc&from=url&id=r4pPQ&originHeight=997&originWidth=1648&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)DefaultSingletonBeanRegistry的singletonObjects中就放着所有的单例bean。
我们可以通过反射调用来查看singletonObjects中存放的所有bean。
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
Field singletonObjects = DefaultSingletonBeanRegistry.class.getDeclaredField("singletonObjects");
singletonObjects.setAccessible(true);
ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
Map<String, Object> map = (Map<String, Object>) singletonObjects.get(beanFactory);
//.filter(e -> e.getKey().startsWith("component"))
map.entrySet().stream()
.forEach(e -> {
    System.out.println(e.getKey() + "=" + e.getValue());
});
```
# ApplicationContext的功能
## 介绍
我们现在来看ApplicationContext比起BeanFactory扩展了哪些功能。再看一下继承关系图![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677922790259-2ba9566b-16a1-44db-aa87-45faaaa443a2.png#averageHue=%23c4ecc9&clientId=u4cd225a2-c7f9-4&from=paste&height=365&id=uff480b6b&name=image.png&originHeight=547&originWidth=2058&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=41818&status=done&style=none&taskId=u22ef773e-97b0-4d31-9fe3-0ca2b67b96c&title=&width=1372#averageHue=%23c4ecc9&from=url&id=qozsg&originHeight=547&originWidth=2058&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)ApplicationContext的扩展功能主要体现在继承的4个父接口上面：

1. MessageSource（具备处理国际化资源的能力）
2. EnvironmentCapable （可以读取环境信息，比如读取系统环境变量，读取配置文件KV）
3. ResourcePatternResolver （通配符匹配资源的能力）
4. ApplicationEventPublisher （发布事件能力）
## MessageSource
先来讲一下MessageSource。语言的资源文件在项目代理里面已经有了 ![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1677923691261-d8d5ac07-8492-4481-aed2-7e72bec9c5e4.png#averageHue=%23f9f8f7&clientId=u4cd225a2-c7f9-4&from=paste&height=493&id=ua3c55449&name=image.png&originHeight=739&originWidth=555&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=34311&status=done&style=none&taskId=u47d2e037-c8c8-4a79-98cf-a180bf44376&title=&width=370#averageHue=%23f9f8f7&from=url&id=tqIvK&originHeight=739&originWidth=555&originalType=binary&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&title=)
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
//getMessage是MessageSource里面的方法。
System.out.println(context.getMessage("hi", null, Locale.CHINA));
System.out.println(context.getMessage("hi", null, Locale.ENGLISH));
System.out.println(context.getMessage("hi", null, Locale.JAPANESE));
```
**输出：** 你好  Hello  こんにちは
实际开发中，浏览器通过请求头带上语言参数告诉服务器使用什么语言。后台就匹配相应的语言。
## ResourcePatternResolver
通过getResources方法获取资源。
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
Resource[] resources = context.getResources("classpath:application.properties");
for (Resource resource : resources) {
    System.out.println(resource);
}
输出：
class path resource [application.properties]
```
上述代码只能找到一个资源文件。我们再举一个能找到多个资源文件的例子。
在SpringBoot的spring.factories里面有一些自动配置类，我们来获取一下。
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
//classpath只找类路径下，classpath*jar包里面也会去找。
Resource[] resources = context.getResources("classpath*:META-INF/spring.factories");
for (Resource resource : resources) {
    System.out.println(resource);
}

输出：
URL [file:/E:/Study/java/Spring%e9%ab%98%e7%ba%a7/%e4%bb%a3%e7%a0%81/show/target/classes/META-INF/spring.factories]
URL [jar:file:/E:/soft/maven/repository/org/springframework/boot/spring-boot/2.5.5/spring-boot-2.5.5.jar!/META-INF/spring.factories]
URL [jar:file:/E:/soft/maven/repository/org/springframework/boot/spring-boot-autoconfigure/2.5.5/spring-boot-autoconfigure-2.5.5.jar!/META-INF/spring.factories]
URL [jar:file:/E:/soft/maven/repository/org/springframework/spring-beans/5.3.10/spring-beans-5.3.10.jar!/META-INF/spring.factories]
URL [jar:file:/E:/soft/maven/repository/org/mybatis/spring/boot/mybatis-spring-boot-autoconfigure/2.2.0/mybatis-spring-boot-autoconfigure-2.2.0.jar!/META-INF/spring.factories]
URL [jar:file:/E:/soft/maven/repository/com/alibaba/druid-spring-boot-starter/1.2.8/druid-spring-boot-starter-1.2.8.jar!/META-INF/spring.factories]
URL [jar:file:/E:/soft/maven/repository/org/springframework/spring-test/5.3.10/spring-test-5.3.10.jar!/META-INF/spring.factories]
```
## EnvironmentCapable
用context.getEnvironment获取环境变量。其定义在EnvironmentCapable接口中
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
System.out.println(context.getEnvironment().getProperty("java_home"));
System.out.println(context.getEnvironment().getProperty("server.port"));
```
## ApplicationEventPublisher 
用context.publishEvent()发布事件。其定义在ApplicationEventPublisher 接口中。要发布事件，我们就需要先定义好一个事件，写一个类继承ApplicationEvent。source参数是事件源，也就是是谁发的这个事件。
```java
public class UserRegisteredEvent extends ApplicationEvent {
    public UserRegisteredEvent(Object source) {
        super(source);
    }
}
```
还需要一个事件的接收者，这里定义一个Component2来接收。
```java
@Component
public class Component2 {

    private static final Logger log = LoggerFactory.getLogger(Component2.class);

    @EventListener
    public void aaa(UserRegisteredEvent event) {
        log.debug("{}", event);
    }
}
```
最后调用context.publishEvent()发布一个事件。
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
context.publishEvent(new UserRegisteredEvent(context));
context.getBean(Component1.class).register();
```
事件主要可以用来解耦，比如我们可以用一个类发布一个事件，在另一个类去接受这个事件，然后再进行相应的处理。下面用Compent1来发布事件，Compent2来接收事件。
```java
@Component
public class Component1 {

    private static final Logger log = LoggerFactory.getLogger(Component1.class);

    @Autowired
    private ApplicationEventPublisher context;

    public void register() {
        log.debug("用户注册");
        context.publishEvent(new UserRegisteredEvent(this));
    }

}
```
```java
@Component
public class Component2 {

    private static final Logger log = LoggerFactory.getLogger(Component2.class);

    @EventListener
    public void aaa(UserRegisteredEvent event) {
        log.debug("{}", event);
        log.debug("发送短信");
    }
}
```
最后调用Component1的register方法，去发布事件。
```java
ConfigurableApplicationContext context = SpringApplication.run(A01.class, args);
context.publishEvent(new UserRegisteredEvent(context));
context.getBean(Component1.class).register();
```
# 总结

1. 到底什么是 BeanFactory 
   1. 它是 ApplicationContext 的父接口
   2. 它才是 Spring 的核心容器, 主要的 ApplicationContext 实现都【组合】了它的功能，【组合】是指ApplicationContext 的一个重要成员变量就是 BeanFactory
2. BeanFactory 能干点啥 
   1. 表面上只有 getBean
   2. 实际上控制反转、基本的依赖注入、直至 Bean 的生命周期的各种功能，都由它的实现类提供
   3. 例子中通过反射查看了它的成员变量 singletonObjects，内部包含了所有的单例 bean
3. ApplicationContext 比 BeanFactory 多点啥 
   1. ApplicationContext 组合并扩展了 BeanFactory 的功能
   2. 国际化、通配符方式获取一组 Resource 资源、整合 Environment 环境、事件发布与监听
   3. 新学一种代码之间解耦途径，事件解耦
