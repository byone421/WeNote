# Aware与InitializingBean接口
Aware接口提供了一种内置的注入手段，可以注入BeanFactory,Applicationcontext，
InitializingBean提供了一种内置化的初始手段，内置的注入和初始化不受扩展功能的影响，总会被执行。
因此spring框架内部的类常用他们。

1. Aware 接口用于注入一些与容器相关信息, 例如
a. BeanNameAware 注入 bean 的名字
b. BeanFactoryAware 注入 BeanFactory 容器
c. ApplicationContextAware 注入 ApplicationContext 容器
d. EmbeddedValueResolverAware ${} 

先去执行bean的Aware接口再去执行初始化。

2.  有同学说: b、c、d 的功能用 [@Autowired ](/Autowired ) 就能实现啊, 为啥还要用 Aware 接口呢 
简单地说:
a. [@Autowired ](/Autowired ) 的解析需要用到 bean 后处理器, 属于扩展功能 
b. 而 Aware 接口属于内置功能, 不加任何扩展, Spring 就能识别
某些情况下, 扩展功能会失效, 而内置功能不会失效 

     例1: 你会发现用 Aware 注入 ApplicationContext 成功, 而 @Autowired 注入 ApplicationContext 失败。因为没有后置处理器。
# 配置类 @Autowired 失效分析
Java 配置类不包含 BeanFactoryPostProcessor 的情况
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1687791340742-05ba5e7c-8faa-4f64-a3b1-ee35b96b4d19.png#averageHue=%23faf9f9&clientId=udaa906cc-a0c1-4&from=paste&height=468&id=u9aff4baa&originHeight=702&originWidth=1505&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=64954&status=done&style=none&taskId=u2fa8c7c9-564d-4947-a3be-dbb267e9334&title=&width=1003.3333333333334)
Java 配置类包含 BeanFactoryPostProcessor 的情况，因此要创建其中的 BeanFactoryPostProcessor 必须提前创建 Java 配置类，而此时的 BeanPostProcessor 还未准备好，导致 @Autowired 等注解失效
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1687791360817-b22fbfc2-a94c-46be-a959-4afb4da39773.png#averageHue=%23c9a476&clientId=udaa906cc-a0c1-4&from=paste&height=430&id=u8c2605a6&originHeight=645&originWidth=1467&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=65408&status=done&style=none&taskId=ua24e1a17-5561-437b-b229-106c50ca336&title=&width=978)
对应的代码
```java
@Configuration
public class MyConfig1 {
    private static final Logger log = LoggerFactory.getLogger(MyConfig1.class);
    @Autowired
    public void setApplicationContext(ApplicationContext applicationContext) {
        log.debug("注入 ApplicationContext");
    }
    @PostConstruct
    public void init() {
        log.debug("初始化");
    }
    @Bean //  ⬅️ 注释或添加 beanFactory 后处理器对应上方两种情况
    public BeanFactoryPostProcessor processor1() {
        return beanFactory -> {
            log.debug("执行 processor1");
        };
    }
}
```
解决方法：

- 用内置依赖注入和初始化取代扩展依赖注入和初始化
- 用静态工厂方法代替实例工厂方法，避免工厂对象提前被创建
```java
@Configuration
public class MyConfig2 implements InitializingBean, ApplicationContextAware {

    private static final Logger log = LoggerFactory.getLogger(MyConfig2.class);

    @Override
    public void afterPropertiesSet() throws Exception {
        log.debug("初始化");
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        log.debug("注入 ApplicationContext");
    }

    @Bean //  beanFactory 后处理器
    public BeanFactoryPostProcessor processor2() {
        return beanFactory -> {
            log.debug("执行 processor2");
        };
    }
}
```
