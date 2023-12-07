在当前版本的 Spring 和 Spring Boot 程序中，支持五种 Scope

- singleton，容器启动时创建（未设置延迟），容器关闭时销毁
- prototype，每次使用时创建，不会自动销毁，需要调用 DefaultListableBeanFactory.destroyBean(bean) 销毁
- request，每次请求用到此 bean 时创建，请求结束时销毁
- session，每个会话用到此 bean 时创建，会话结束时销毁
- application，web 容器用到此 bean 时创建，容器停止时销毁
- 有些文章提到有 globalSession 这一 Scope，也是陈旧的说法，目前 Spring 中已废弃

测试代码：
```java
@Scope("request")
@Component
public class BeanForRequest {
    private static final Logger log = LoggerFactory.getLogger(BeanForRequest.class);
    @PreDestroy
    public void destroy() {
        log.debug("destroy");
    }
}

```
```java

 @Lazy
 @Autowired
 private BeanForRequest beanForRequest;
 @GetMapping(value = "/test", produces = "text/html")
 public String test(HttpServletRequest request, HttpSession session) {
     ServletContext sc = request.getServletContext();
     String sb = "<ul>" +
                 "<li>" + "request scope:" + beanForRequest + "</li>" +
                 "<li>" + "session scope:" + beanForSession + "</li>" +
                 "<li>" + "application scope:" + beanForApplication + "</li>" +
                 "</ul>";
     return sb;
 }

```
# Scope失效解决1,2
#### 分析 - singleton 注入其它 scope 失效
以单例注入多例为例
有一个单例对象 E
```java
@Component
public class E {
    private static final Logger log = LoggerFactory.getLogger(E.class);

    private F f;

    public E() {
        log.info("E()");
    }

    @Autowired
    public void setF(F f) {
        this.f = f;
        log.info("setF(F f) {}", f.getClass());
    }

    public F getF() {
        return f;
    }
}
```
要注入的对象 F 期望是多例
```java
@Component
@Scope("prototype")
public class F {
    private static final Logger log = LoggerFactory.getLogger(F.class);

    public F() {
        log.info("F()");
    }
}
```
测试
```java
E e = context.getBean(E.class);
F f1 = e.getF();
F f2 = e.getF();
System.out.println(f1);
System.out.println(f2);

com.itheima.demo.cycle.F@6622fc65
com.itheima.demo.cycle.F@6622fc65
```
发现它们是同一个对象，而不是期望的多例对象
对于单例对象来讲，依赖注入仅发生了一次，后续再没有用到多例的 F，因此 E 用的始终是第一次依赖注入的 F
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1688040113661-a4188e3e-1559-417a-8197-0ad27d14791b.png#averageHue=%23fdfdfd&clientId=ue7a45f02-7e98-4&from=paste&height=117&id=ubc713ab4&originHeight=176&originWidth=1100&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=10340&status=done&style=none&taskId=u9490bd19-3e7f-4a94-8c3f-66b78cada0f&title=&width=733.3333333333334)
##### 解决1

- 仍然使用 @Lazy 生成代理
- 代理对象虽然还是同一个，但当每次**使用代理对象的任意方法**时，由代理创建新的 f 对象

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1688040130986-22c94877-c452-44d7-a061-ea91f9bc3a5c.png#averageHue=%23fcfcfc&clientId=ue7a45f02-7e98-4&from=paste&height=274&id=u72b21160&originHeight=411&originWidth=1151&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=28782&status=done&style=none&taskId=u19a34e7b-8b72-49ad-b743-7e710075f11&title=&width=767.3333333333334)
```java
@Component
public class E {

    @Autowired
    @Lazy
    public void setF(F f) {
        this.f = f;
        log.info("setF(F f) {}", f.getClass());
    }

    // ...
}
```
**_注意_**

- [@Lazy ](/Lazy ) 加在也可以加在成员变量上，但加在 set 方法上的目的是可以观察输出，加在成员变量上就不行了 
- [@Autowired ](/Autowired ) 加在 set 方法的目的类似 

输出
```java
E: setF(F f) class com.itheima.demo.cycle.F$$EnhancerBySpringCGLIB$$8b54f2bc
F: F()
com.itheima.demo.cycle.F@3a6f2de3
F: F()
com.itheima.demo.cycle.F@56303b57
```
从输出日志可以看到调用 setF 方法时，f 对象的类型是代理类型

##### 解决2
@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
##### 解决3
ObjectFactory
```java
@Autowired
private ObjectFactory<F3> f3;
public F3 getF3() {
    return f3.getObject();
}
```
##### 解决4
```java
    @Autowired
    private ApplicationContext context;
    public F4 getF4() {
        return context.getBean(F4.class);
    }
```
解决方法虽然不同，但理念上殊途同归: 都是推迟其它 scope bean 的获取
