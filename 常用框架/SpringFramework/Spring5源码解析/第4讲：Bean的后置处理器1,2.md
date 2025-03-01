# Bean的后置处理器1,2
[TOC]
前面说过Bean的后置处理器主要是为Bean的生命周期的各个阶段提供扩展。
我们也提过主要的6个扩展点：实例化前后，依赖注入阶段，初始化前后，销毁之前。
这一节主要讲解一些常用的后置处理器，先来看代码：
```java
 public static void main(String[] args) {
        // ⬇️GenericApplicationContext 是一个【干净】的容器
        // 没有帮我们添加一些额外的bean工厂处理器和bean处理器，这就相对来讲干净一点，方便测试
        GenericApplicationContext context = new GenericApplicationContext();

        // ⬇️用原始方法注册三个 bean
        context.registerBean("bean1", Bean1.class);
        context.registerBean("bean2", Bean2.class);
        context.registerBean("bean3", Bean3.class);

        // ⬇️初始化容器
        context.refresh(); // 执行beanFactory后处理器, 添加bean后处理器, 初始化所有单例 

        // ⬇️销毁容器
        context.close();
    }
```
bean1:
```java
public class Bean1 {
    private static final Logger log = LoggerFactory.getLogger(Bean1.class);

    private Bean2 bean2;

    @Autowired
    public void setBean2(Bean2 bean2) {
        log.debug("@Autowired 生效: {}", bean2);
        this.bean2 = bean2;
    }

    @Autowired
    private Bean3 bean3;

    @Resource
    public void setBean3(Bean3 bean3) {
        log.debug("@Resource 生效: {}", bean3);
        this.bean3 = bean3;
    }

    private String home;

    @Autowired
    public void setHome(@Value("${JAVA_HOME}") String home) {
        log.debug("@Value 生效: {}", home);
        this.home = home;
    }

    @PostConstruct
    public void init() {
        log.debug("@PostConstruct 生效");
    }

    @PreDestroy
    public void destroy() {
        log.debug("@PreDestroy 生效");
    }
}

```
bean2和bean3是个空类，只是用于bean1进行注入。
我们运行main方法发现并没有注入我的bean。bean中加入的注解也没有生效。
那我们下面就添加一些bean的后置处理器。
```java
    public static void main(String[] args) {
        // ⬇️GenericApplicationContext 是一个【干净】的容器
        GenericApplicationContext context = new GenericApplicationContext();

        // ⬇️用原始方法注册三个 bean
        context.registerBean("bean1", Bean1.class);
        context.registerBean("bean2", Bean2.class);
        context.registerBean("bean3", Bean3.class);
         // 避免@value解析值报错
        context.getDefaultListableBeanFactory().setAutowireCandidateResolver(new ContextAnnotationAutowireCandidateResolver());
        // 处理 @Autowired @Value
        context.registerBean(AutowiredAnnotationBeanPostProcessor.class);
        // 处理@Resource @PostConstruct @PreDestroy
        context.registerBean(CommonAnnotationBeanPostProcessor.class); 
        
        // ⬇️初始化容器
        context.refresh(); // 执行beanFactory后处理器, 添加bean后处理器, 初始化所有单例
        // ⬇️销毁容器
        context.close();
    }
```
执行结果：
```java
 - @Resource 生效: com.itheima.a04.Bean3@37f1104d 
 - @Autowired 生效: com.itheima.a04.Bean2@6c779568 
 - @Value 生效: E:\soft\javase\jdk1.8.0_341 
 - @PostConstruct 生效 
 - @PreDestroy 生效 

```
可以看到在依赖注入阶段：可以看到先解析的是@Resource再进行@Autowired的解析。
# Bean的后置处理器3
我们都知道SpringBoot中有一个属性绑定的注解@ConfigurationProperties(prefix = "xxx")
```java
/*
    java.home=
    java.version=
 */
@ConfigurationProperties(prefix = "java")
public class Bean4 {

    private String home;

    private String version;

    public String getHome() {
        return home;
    }

    public void setHome(String home) {
        this.home = home;
    }

    public String getVersion() {
        return version;
    }

    public void setVersion(String version) {
        this.version = version;
    }

}
```
把Bean4添加到容器, 注册对象的后置处理器
ConfigurationPropertiesBindingPostProcessor.register(context.getDefaultListableBeanFactory());
```java
    public static void main(String[] args) {
        // ⬇️GenericApplicationContext 是一个【干净】的容器
        GenericApplicationContext context = new GenericApplicationContext();

        // ⬇️用原始方法注册三个 bean
        context.registerBean("bean1", Bean1.class);
        context.registerBean("bean2", Bean2.class);
        context.registerBean("bean3", Bean3.class);
         // 解析@value表达式
        context.getDefaultListableBeanFactory().setAutowireCandidateResolver(new ContextAnnotationAutowireCandidateResolver());
        // 处理 @Autowired @Value
        context.registerBean(AutowiredAnnotationBeanPostProcessor.class);
        // 处理@Resource @PostConstruct @PreDestroy
        context.registerBean(CommonAnnotationBeanPostProcessor.class); 
        //ConfigurationProperties
       ConfigurationPropertiesBindingPostProcessor.register(context.getDefaultListableBeanFactory());
        
        // ⬇️初始化容器
        context.refresh(); // 执行beanFactory后处理器, 添加bean后处理器, 初始化所有单例
        // ⬇️销毁容器
        context.close();
    }
```
以上就是一些常用的后置处理。
# @Autowired bean后置处理器的执行分析
这里主要是对AutowiredAnnotationBeanPostProcessor的分析。
使用registerSingleton注册单例，会认为bean是一个成品的bean。不会在对bean进行创建过程，依赖注入，初始化的操作。
```java
public class DigInAutowired {
    public static void main(String[] args) throws Throwable {
        DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();
        beanFactory.registerSingleton("bean2", new Bean2()); // 创建过程,依赖注入,初始化
        beanFactory.registerSingleton("bean3", new Bean3());
        beanFactory.setAutowireCandidateResolver(new ContextAnnotationAutowireCandidateResolver()); // @Value
    }
}
```
为了研究AutowiredAnnotationBeanPostProcessor 我们手动使用他，处理的方法就是postProcessProperties
```cpp
public class DigInAutowired {
public static void main(String[] args) throws Throwable {
    DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();
    beanFactory.registerSingleton("bean2", new Bean2()); // 创建过程,依赖注入,初始化
    beanFactory.registerSingleton("bean3", new Bean3());
    beanFactory.setAutowireCandidateResolver(new ContextAnnotationAutowireCandidateResolver()); // @Value

 
     // 1. 查找哪些属性、方法加了 @Autowired, 这称之为 InjectionMetadata
     AutowiredAnnotationBeanPostProcessor processor = new AutowiredAnnotationBeanPostProcessor();
     processor.setBeanFactory(beanFactory);
     Bean1 bean1 = new Bean1();
     System.out.println(bean1);
     processor.postProcessProperties(null, bean1, "bean1"); // 执行依赖注入 @Autowired @Value
     System.out.println(bean1);
}
}
```
运行代码结果：
```cpp
Bean1{bean2=null, bean3=null, home='null'}
@Autowired 生效: com.itheima.a04.Bean2@1283bb96 
@Value 生效: ${JAVA_HOME} 
Bean1{bean2=com.itheima.a04.Bean2@1283bb96, bean3=com.itheima.a04.Bean3@3023df74, home='${JAVA_HOME}'}
```
可以看到第一次输入没有注入，第二次输入已经注入成功，起到作用的就是postProcessProperties 方法
点进去查看源码：
```cpp
	@Override
	public PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName) {
		// 1. findAutowiringMetadata找到@Autowired 封装成一个InjectionMetadata
        InjectionMetadata metadata = findAutowiringMetadata(beanName, bean.getClass(), pvs);
		try {
            // 2. 利用反射进行赋值
			metadata.inject(bean, beanName, pvs);
		}
		catch (BeanCreationException ex) {
			throw ex;
		}
		catch (Throwable ex) {
			throw new BeanCreationException(beanName, "Injection of autowired dependencies failed", ex);
		}
		return pvs;
	}
```

1. 现在来看看findAutowiringMetadata, 利用如下代码来执行findAutowiringMetadata。
```java
Method findAutowiringMetadata = AutowiredAnnotationBeanPostProcessor.class.getDeclaredMethod("findAutowiringMetadata", String.class, Class.class, PropertyValues.class);
findAutowiringMetadata.setAccessible(true);
// 获取 Bean1 上加了 @Value @Autowired 的成员变量，方法参数信息
InjectionMetadata metadata = (InjectionMetadata) findAutowiringMetadata.invoke(processor, "bean1", Bean1.class, null);
System.out.println(metadata);



```
执行代码断点查看metadata
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1684668651314-9f18b74e-fbfd-42ed-803f-c1958d902d09.png#averageHue=%23f5f4f3&clientId=u5c73a946-1176-4&from=paste&height=315&id=ub92d3f62&originHeight=473&originWidth=1884&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=509514&status=done&style=none&taskId=u75139fd5-a54c-4b21-9c4c-42bd52f5f0b&title=&width=1256)
这个集合就是加了@Autowired的属性字段或者方法。

2. 再执行inject方法注入值。
```java
// 2. 调用 InjectionMetadata 来进行依赖注入, 注入时按类型查找值
metadata.inject(bean1, "bean1", null);
System.out.println(bean1);
```

3. 如何按类型查找值

对于成员变量
```
@Autowired
private Bean3 bean3;
```
以下代码是核心思路：
```java
//先获取对象的bean3 属性
Field bean3 = Bean1.class.getDeclaredField("bean3");
//封装成一个DependencyDescriptor
DependencyDescriptor dd1 = new DependencyDescriptor(bean3, false);
//根据类型去找
Object o = beanFactory.doResolveDependency(dd1, null, null, null);
System.out.println(o);
```
对于set方法
```java
    @Autowired
    public void setBean2(Bean2 bean2) {
        log.debug("@Autowired 生效: {}", bean2);
        this.bean2 = bean2;
    }
```
以下代码是核心思路：
```java
 Method setBean2 = Bean1.class.getDeclaredMethod("setBean2", Bean2.class);
 DependencyDescriptor dd2 =
         new DependencyDescriptor(new MethodParameter(setBean2, 0), true);
 Object o1 = beanFactory.doResolveDependency(dd2, null, null, null);
 System.out.println(o1);
```
对于方法参数有@value
```java
 @Autowired
 public void setHome(@Value("${JAVA_HOME}") String home) {
     log.debug("@Value 生效: {}", home);
     this.home = home;
 }
```
以下代码是核心思路：
```java
 Method setHome = Bean1.class.getDeclaredMethod("setHome", String.class);
 DependencyDescriptor dd3 = new DependencyDescriptor(new MethodParameter(setHome, 0), true);
 Object o2 = beanFactory.doResolveDependency(dd3, null, null, null);
 System.out.println(o2);
```
# 总结
#### 后处理器作用

1. [@Autowired ](/Autowired ) 等注解的解析属于 bean 生命周期阶段（依赖注入, 初始化）的扩展功能，这些扩展功能由 bean 后处理器来完成 
2. 每个后处理器各自增强什么功能 
   - AutowiredAnnotationBeanPostProcessor 解析 [@Autowired ](/Autowired ) 与 [@Value ](/Value ) 
   - CommonAnnotationBeanPostProcessor 解析 @Resource、@PostConstruct、[@PreDestroy ](/PreDestroy ) 
   - ConfigurationPropertiesBindingPostProcessor 解析 @ConfigurationProperties
3. 另外 ContextAnnotationAutowireCandidateResolver 负责获取 [@Value ](/Value ) 的值，解析 @Qualifier、泛型、[@Lazy ](/Lazy ) 等 
#### @Autowired bean 后处理器运行分析

1. AutowiredAnnotationBeanPostProcessor.findAutowiringMetadata 用来获取某个 bean 上加了 [@Value ](/Value ) [@Autowired ](/Autowired ) 的成员变量，方法参数的信息，表示为 InjectionMetadata 
2. InjectionMetadata 可以完成依赖注入
3. InjectionMetadata 内部根据成员变量，方法参数封装为 DependencyDescriptor 类型
4. 有了 DependencyDescriptor，就可以利用 beanFactory.doResolveDependency 方法进行基于类型的查找
