# 演示 - FactoryBean
```java
@ComponentScan
public class A43 {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(A43.class);

        Bean1 bean1 = (Bean1) context.getBean("bean1");
        Bean1 bean2 = (Bean1) context.getBean("bean1");
        Bean1 bean3 = (Bean1) context.getBean("bean1");
        System.out.println(bean1);
        System.out.println(bean2);
        System.out.println(bean3);

        System.out.println(context.getBean(Bean1.class));

        System.out.println(context.getBean(Bean1FactoryBean.class));
        System.out.println(context.getBean("&bean1"));

        context.close();

        /*
            学到了什么: 一个在 Spring 发展阶段中重要, 但目前已经很鸡肋的接口 FactoryBean 的使用要点
            说它鸡肋有两点:
                1. 它的作用是用制造创建过程较为复杂的产品, 如 SqlSessionFactory, 但 @Bean 已具备等价功能
                2. 使用上较为古怪, 一不留神就会用错
                    a. 被 FactoryBean 创建的产品
                        - 会认为创建、依赖注入、Aware 接口回调、前初始化这些都是 FactoryBean 的职责, 这些流程都不会走
                        - 唯有后初始化的流程会走, 也就是产品可以被代理增强
                        - 单例的产品不会存储于 BeanFactory 的 singletonObjects 成员中, 而是另一个 factoryBeanObjectCache 成员中
                    b. 按名字去获取时, 拿到的是产品对象, 名字前面加 & 获取的是工厂对象
            但目前此接口的实现仍被大量使用, 想被全面废弃很难
         */
    }

}

```
```java
public class Bean1 implements BeanFactoryAware {
    private static final Logger log = LoggerFactory.getLogger(Bean1.class);

    private Bean2 bean2;

    @Autowired
    public void setBean2(Bean2 bean2) {
        log.debug("setBean2({})", bean2);
        this.bean2 = bean2;
    }

    public Bean2 getBean2() {
        return bean2;
    }

    @PostConstruct
    public void init() {
        log.debug("init");
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        log.debug("setBeanFactory({})", beanFactory);
    }
}

```
```java
@Component("bean1")
public class Bean1FactoryBean implements FactoryBean<Bean1> {

    private static final Logger log = LoggerFactory.getLogger(Bean1FactoryBean.class);

    // 决定了根据【类型】获取或依赖注入能否成功
    @Override
    public Class<?> getObjectType() {
        return Bean1.class;
    }

    // 决定了 getObject() 方法被调用一次还是多次
    @Override
    public boolean isSingleton() {
        return true;
    }

    @Override
    public Bean1 getObject() throws Exception {
        Bean1 bean1 = new Bean1();
        log.debug("create bean: {}", bean1);
        return bean1;
    }
}

```
```java
@Component
public class Bean1PostProcessor implements BeanPostProcessor {

    private static final Logger log = LoggerFactory.getLogger(Bean1PostProcessor.class);

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        if (beanName.equals("bean1") && bean instanceof Bean1) {
            log.debug("before [{}] init", beanName);
        }
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        if (beanName.equals("bean1") && bean instanceof Bean1) {
            log.debug("after [{}] init", beanName);
        }
        return bean;
    }
}

```
```java
@Component
public class Bean2 {
}

```


1. 它的作用是用制造创建过程较为复杂的产品, 如 SqlSessionFactory, 但 @Bean 已具备等价功能
2. 使用上较为古怪, 一不留神就会用错
   1. 被 FactoryBean 创建的产品
      - 会认为创建、依赖注入、Aware 接口回调、前初始化这些都是 FactoryBean 的职责, 这些流程都不会走
      - 唯有后初始化的流程会走, 也就是产品可以被代理增强
      - 单例的产品不会存储于 BeanFactory 的 singletonObjects 成员中, 而是另一个 factoryBeanObjectCache 成员中
   2. 按名字去获取时, 拿到的是产品对象, 名字前面加 & 获取的是工厂对象
