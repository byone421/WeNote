# Bean的生命周期
[TOC]
这里准备好了一个Springboot引导类
```java
@SpringBootApplication
public class A03 {
    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(A03.class, args);
        context.close();
    }
}

```
还有一个LifeCycleBean
```java
@Component
public class LifeCycleBean {
    private static final Logger log = LoggerFactory.getLogger(LifeCycleBean.class);

    public LifeCycleBean() {
        log.debug("构造");
    }

    @Autowired
    public void autowire(@Value("${JAVA_HOME}") String home) {
        log.debug("依赖注入: {}", home);
    }

    @PostConstruct
    public void init() {
        log.debug("初始化");
    }

    @PreDestroy
    public void destroy() {
        log.debug("销毁");
    }
}

```
那么LifeCycleBean里面的方法执行顺序是怎么样的呢？直接说一下结论：

1. 先执行构造方法
2. 然后执行依赖注入方法
3. 调用初始化方法
4. 当容器销毁时，执行销毁方法（需要需要的是只有在bean是单例的情况下才会调用销毁方法）

大家可以运行看效果。
说完这4个方法，这里在说一下bean的后置处理器。前面说过beanFactory只有一些简单的功能。一些其他功能可以通过加后置处理器来实现。有一类后置处理器是beanFactory处理器用来补充bean的定义信息。还有一类就是bean的后置处理器，提供对bean的生命周期各个阶段的一个扩展。我们看一下下面这个bean的后置处理器
```java
@Component
public class MyBeanPostProcessor implements InstantiationAwareBeanPostProcessor, DestructionAwareBeanPostProcessor {

    private static final Logger log = LoggerFactory.getLogger(MyBeanPostProcessor.class);

    @Override
    public void postProcessBeforeDestruction(Object bean, String beanName) throws BeansException {
        if (beanName.equals("lifeCycleBean"))
            log.debug("<<<<<< 销毁之前执行, 如 @PreDestroy");
    }

    @Override
    public Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
        if (beanName.equals("lifeCycleBean"))
            log.debug("<<<<<< 实例化之前执行, 这里返回的对象会替换掉原本的 bean");
        return null;
    }

    @Override
    public boolean postProcessAfterInstantiation(Object bean, String beanName) throws BeansException {
        if (beanName.equals("lifeCycleBean")) {
            log.debug("<<<<<< 实例化之后执行, 这里如果返回 false 会跳过依赖注入阶段");
//            return false;
        }
        return true;
    }

    @Override
    public PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName) throws BeansException {
        if (beanName.equals("lifeCycleBean"))
            log.debug("<<<<<< 依赖注入阶段执行, 如 @Autowired、@Value、@Resource");
        return pvs;
    }

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        if (beanName.equals("lifeCycleBean"))
            log.debug("<<<<<< 初始化之前执行, 这里返回的对象会替换掉原本的 bean, 如 @PostConstruct、@ConfigurationProperties");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        if (beanName.equals("lifeCycleBean"))
            log.debug("<<<<<< 初始化之后执行, 这里返回的对象会替换掉原本的 bean, 如代理增强");
        return bean;
    }
}

```
这几个方法都是见名知意的：我们执行看一下效果
```java
 <<<<<< 实例化之前执行, 这里返回的对象会替换掉原本的 bean 
 构造 
 <<<<<< 实例化之后执行, 这里如果返回 false 会跳过依赖注入阶段 
 <<<<<< 依赖注入阶段执行, 如 @Autowired、@Value、@Resource 
 依赖注入: E:\soft\javase\jdk1.8.0_341 
 <<<<<< 初始化之前执行, 这里返回的对象会替换掉原本的 bean, 如 @PostConstruct、@ConfigurationProperties 
 初始化 
 <<<<<< 初始化之后执行, 这里返回的对象会替换掉原本的 bean, 如代理增强 
 <<<<<< 销毁之前执行, 如 @PreDestroy 
 销毁 
```
从这个例子我们学到 bean的后置处理器一般可以在bean 4个阶段 **  创建，依赖注入，初始化，销毁** 对其进行增强

# 模板方法
我们来看一下模板方法设计模式，先看一下模板方法的作用，回过来头来再看定义。
其作用就是提高现有代码的扩展能力。现在有一个需求就是让你模拟spring写一个bean工厂。
以下是一个简化过程：我们创建了一个对象，过程涉及到构造，依赖注入，初始化...
```java
static class MyBeanFactory {
        public Object getBean() {
            Object bean = new Object();
            System.out.println("构造 " + bean);
            System.out.println("依赖注入 " + bean); // @Autowired, @Resource
            System.out.println("初始化 " + bean);
            return bean;
}
```
这种写法在设计上不好的。扩展性差。比如：刚开始只实现了一个最基本的依赖注入，现在我需要扩展支持@Autowired, @Resource。那么就必须去改动原有代码去增加逻辑。这样就会越来越臃肿。那应该怎么做呢？
我们先设计一个接口：
```java
static interface BeanPostProcessor {
    public void inject(Object bean); // 对依赖注入阶段的扩展
}
```
再添加一个接口集合
```java
static class MyBeanFactory {
        public Object getBean() {
            Object bean = new Object();
            System.out.println("构造 " + bean);
            System.out.println("依赖注入 " + bean); // @Autowired, @Resource
            for (BeanPostProcessor processor : processors) {
                processor.inject(bean);
            }
            System.out.println("初始化 " + bean);
            return bean;
        }

        private List<BeanPostProcessor> processors = new ArrayList<>();

        public void addBeanPostProcessor(BeanPostProcessor processor) {
            processors.add(processor);
        }
}
```
测试代码如下：
```java
    public static void main(String[] args) {
        MyBeanFactory beanFactory = new MyBeanFactory();
        beanFactory.addBeanPostProcessor(bean -> System.out.println("解析 @Autowired"));
        beanFactory.addBeanPostProcessor(bean -> System.out.println("解析 @Resource"));
        beanFactory.getBean();
    }
```
```java
构造 java.lang.Object@27fa135a
依赖注入 java.lang.Object@27fa135a
解析 @Autowired
解析 @Resource
初始化 java.lang.Object@27fa135a

```
# 总结
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1687791872573-0bb680a7-c25c-40c5-bd36-578314d5e424.png#averageHue=%23fcfcfc&clientId=u2b34724b-e034-4&from=paste&height=117&id=u968ea1cc&originHeight=175&originWidth=1299&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=12165&status=done&style=none&taskId=u5ed527d6-38ba-458a-8e55-aebbf7e5afe&title=&width=866)
创建前后的增强

- postProcessBeforeInstantiation 
   - 这里返回的对象若不为 null 会替换掉原本的 bean，并且仅会走 postProcessAfterInitialization 流程
- postProcessAfterInstantiation 
   - 这里如果返回 false 会跳过依赖注入阶段

依赖注入前的增强

- postProcessProperties 
   - 如 @Autowired、@Value、[@Resource ](/Resource ) 

初始化前后的增强

- postProcessBeforeInitialization 
   - 这里返回的对象会替换掉原本的 bean
   - 如 @PostConstruct、@ConfigurationProperties
- postProcessAfterInitialization 
   - 这里返回的对象会替换掉原本的 bean
   - 如代理增强

销毁之前的增强

- postProcessBeforeDestruction 
   - 如 [@PreDestroy ](/PreDestroy ) 
