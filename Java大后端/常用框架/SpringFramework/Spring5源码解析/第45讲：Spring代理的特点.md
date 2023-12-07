# 1.演示 spring 代理的设计特点
依赖注入和初始化影响的是原始对象
代理与目标是两个对象，二者成员变量并不共用数据
```java
public static void main(String[] args) throws Exception {
    ConfigurableApplicationContext context = SpringApplication.run(A45.class, args);

    Bean1 proxy = context.getBean(Bean1.class);
    showProxyAndTarget(proxy);

    System.out.println(">>>>>>>>>>>>>>>>>>>");
    System.out.println(proxy.getBean2());
    System.out.println(proxy.isInitialized());
}
    public static void showProxyAndTarget(Bean1 proxy) throws Exception {
        System.out.println(">>>>> 代理中的成员变量");
        System.out.println("\tinitialized=" + proxy.initialized);
        System.out.println("\tbean2=" + proxy.bean2);

        if (proxy instanceof Advised advised) {
            System.out.println(">>>>> 目标中的成员变量");
            Bean1 target = (Bean1) advised.getTargetSource().getTarget();
            System.out.println("\tinitialized=" + target.initialized);
            System.out.println("\tbean2=" + target.bean2);
        }
    }
```
```java

@Aspect
@Component
public class MyAspect {

    // 故意对所有方法增强
    @Before("execution(* com.itheima.a45.Bean1.*(..))")
    public void before() {
        System.out.println("before");
    }
}

```
```java
@Component
public class Bean1 {

    private static final Logger log = LoggerFactory.getLogger(Bean1.class);

    protected Bean2 bean2;

    protected boolean initialized;

    @Autowired
    public void setBean2(Bean2 bean2) {
        log.debug("setBean2(Bean2 bean2)");
        this.bean2 = bean2;
    }

    @PostConstruct
    public void init() {
        log.debug("init");
        initialized = true;
    }

    public Bean2 getBean2() {
        log.debug("getBean2()");
        return bean2;
    }

    public boolean isInitialized() {
        log.debug("isInitialized()");
        return initialized;
    }

    public void m1() {
        System.out.println("m1() 成员方法");
    }

    final public void m2() {
        System.out.println("m2() final 方法");
    }

    static public void m3() {
        System.out.println("m3() static 方法");
    }

    private void m4() {
        System.out.println("m4() private 方法");
    }

}

```
```java
@Component
public class Bean2 {
}

```
# 2.演示 static 方法、final 方法、private 方法均无法增强
```java
        /*
            2.演示 static 方法、final 方法、private 方法均无法增强
         */

        proxy.m1();
        proxy.m2();
        proxy.m3();
        Method m4 = Bean1.class.getDeclaredMethod("m4");
        m4.setAccessible(true);
        m4.invoke(proxy);

        context.close();
```
# 总结：
spring 代理的设计特点

-  依赖注入和初始化影响的是原始对象 
   - 因此 cglib 不能用 MethodProxy.invokeSuper()
-  代理与目标是两个对象，二者成员变量并不共用数据 

static 方法、final 方法、private 方法均无法增强

- 进一步理解代理增强基于方法重写
