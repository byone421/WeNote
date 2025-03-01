# SpringApplication构造分析
## 1. 演示获取 Bean Definition 源
```java
   static class Bean1 {

    }

    static class Bean2 {

    }

    static class Bean3 {

    }

    @Bean
    public Bean2 bean2() {
        return new Bean2();
    }

    @Bean
    public TomcatServletWebServerFactory servletWebServerFactory() {
        return new TomcatServletWebServerFactory();
    }
```
```java
    public static void main(String[] args) throws Exception {
        System.out.println("1. 演示获取 Bean Definition 源");
        SpringApplication spring = new SpringApplication(A39_1.class);
        spring.setSources(Set.of("classpath:b01.xml"));
        ConfigurableApplicationContext context = spring.run(args);
        for (String name : context.getBeanDefinitionNames()) {
            System.out.println("name: " + name + " 来源：" + context.getBeanFactory().getBeanDefinition(name).getResourceDescription());
        }
        context.close();
```
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bean1" class="com.itheima.a39.A39_1.Bean1"/>

</beans>
```
## 2. 演示推断应用类型
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690812590435-d7727808-3072-4438-8019-379fdb591697.png#averageHue=%23f2f1ea&clientId=u2b7846aa-f8c4-4&from=paste&height=547&id=u18710252&originHeight=821&originWidth=1555&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=717504&status=done&style=none&taskId=ub6c5ef2b-8c11-4b03-9f9c-5c7310b9d81&title=&width=1036.6666666666667)
```java
System.out.println("2. 演示推断应用类型");
Method deduceFromClasspath = WebApplicationType.class.getDeclaredMethod("deduceFromClasspath");
deduceFromClasspath.setAccessible(true);
System.out.println("\t应用类型为:"+deduceFromClasspath.invoke(null));
```
## 3. 演示 ApplicationContext 初始化器
在创建ApplicationContext和ApplicationContext.refresh之间调用初始化器 对 ApplicationContext 做扩展
```
// 创建 ApplicationContext
// 调用初始化器 对 ApplicationContext 做扩展
// ApplicationContext.refresh
```
读取配置文件中的初始化器
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690813536640-969a1f9c-d6d2-430c-af75-25173a89bb11.png#averageHue=%23eaece2&clientId=u2b7846aa-f8c4-4&from=paste&height=268&id=u6c167519&originHeight=402&originWidth=1336&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=405268&status=done&style=none&taskId=uf5caceba-bfdb-4caa-8c22-c31d1fecec7&title=&width=890.6666666666666)
```java
System.out.println("3. 演示 ApplicationContext 初始化器");
//为了演示方便，直接代码注册
spring.addInitializers(applicationContext -> {
    if (applicationContext instanceof GenericApplicationContext gac) {
        gac.registerBean("bean3", Bean3.class);
    }
});
```
## 4. 演示监听器与事件
与演示 ApplicationContext 初始化器类似。也是从配置文件中读取数据。
```java
System.out.println("4. 演示监听器与事件");
spring.addListeners(event -> System.out.println("\t事件为:" + event.getClass()));
```
## 5. 演示主类推断
**记录main方法所在的类：**
this.mainApplicationClass = deduceMainApplicationClass();
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690813826326-f3c189c3-b7ee-4a10-be0f-9e23e52d1d32.png#averageHue=%23c5edcb&clientId=u2b7846aa-f8c4-4&from=paste&height=409&id=u611a3f8c&originHeight=614&originWidth=1336&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=76406&status=done&style=none&taskId=u0da2fd75-60a0-4533-ae96-3875ce16827&title=&width=890.6666666666666)
```
System.out.println("5. 演示主类推断");
Method deduceMainApplicationClass = SpringApplication.class.getDeclaredMethod("deduceMainApplicationClass");
deduceMainApplicationClass.setAccessible(true);
System.out.println("\t主类是："+deduceMainApplicationClass.invoke(spring));
```
## 总结：SpringApplication 构造

1. 记录 BeanDefinition 源
2. 推断应用类型
3. 记录 ApplicationContext 初始化器
4. 记录监听器
5. 推断主启动类
# SpringApplication run 分析
## 主要步骤

1.  得到 SpringApplicationRunListeners，名字取得不好，实际是事件发布器 
   - 发布 application starting 事件1️⃣
2.  封装启动 args 
3.  准备 Environment 添加命令行参数（*） 
4.  ConfigurationPropertySources 处理（*） 
   - 发布 application environment 已准备事件2️⃣
5.  通过 EnvironmentPostProcessorApplicationListener 进行 env 后处理（*） 
   - application.properties，由 StandardConfigDataLocationResolver 解析
   - spring.application.json
6.  绑定 spring.main 到 SpringApplication 对象（*） 
7.  打印 banner（*） 
8.  创建容器 
9.  准备容器 
   - 发布 application context 已初始化事件3️⃣
10.  加载 bean 定义 
   - 发布 application prepared 事件4️⃣
11.  refresh 容器 
   - 发布 application started 事件5️⃣
12.  执行 runner 
   -  发布 application ready 事件6️⃣ 
   -  这其中有异常，发布 application failed 事件7️⃣ 
## SpringApplicationRunListeners 
SpringApplicationRunListeners 是SpringApplicationRunListener的一个组合器。
得到 SpringApplicationRunListeners，名字取得不好，实际是事件发布器，比如发布 application starting（springboot开始启动）事件。其主要是在一些重要节点执行完毕之后发一下特定的事件。

获取哪里使用了事件发送器。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690814697973-d262a4d9-8345-4279-9fdf-dee1f8bbe03c.png#averageHue=%23f5f5f2&clientId=u2b7846aa-f8c4-4&from=paste&height=493&id=u337d96b4&originHeight=740&originWidth=1213&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=432771&status=done&style=none&taskId=u3337b1aa-1868-436e-a5d9-dce0ef5fdb7&title=&width=808.6666666666666)

获取事件发送器实现类名
```java
// 添加 app 监听器
SpringApplication app = new SpringApplication();
app.addListeners(e -> System.out.println(e.getClass()));

 // 获取事件发送器实现类名
 List<String> names = SpringFactoriesLoader.loadFactoryNames(SpringApplicationRunListener.class, A39_2.class.getClassLoader());
 for (String name : names) {
     System.out.println(name);
     Class<?> clazz = Class.forName(name);
     Constructor<?> constructor = clazz.getConstructor(SpringApplication.class, String[].class);
     SpringApplicationRunListener publisher = (SpringApplicationRunListener) constructor.newInstance(app, args);
     // 发布事件
     DefaultBootstrapContext bootstrapContext = new DefaultBootstrapContext();
     publisher.starting(bootstrapContext); // spring boot 开始启动
     publisher.environmentPrepared(bootstrapContext, new StandardEnvironment()); // 环境信息准备完毕
     GenericApplicationContext context = new GenericApplicationContext();
     publisher.contextPrepared(context); // 在 spring 容器创建，并调用初始化器之后，发送此事件
     publisher.contextLoaded(context); // 所有 bean definition 加载完毕
     context.refresh();
     publisher.started(context); // spring 容器初始化完成(refresh 方法调用完毕)
     publisher.running(context); // spring boot 启动完毕
     publisher.failed(context, new Exception("出错了")); // spring boot 启动出错
 }
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690815743132-9c742e83-1b52-4ae0-a73a-13eb5cc7f85b.png#averageHue=%23e5daa9&clientId=u2b7846aa-f8c4-4&from=paste&height=311&id=u1fc5072c&originHeight=466&originWidth=1089&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=497368&status=done&style=none&taskId=ub5bd4013-aa78-40dd-9027-aaf57474ac9&title=&width=726)
### 小总结
```
学到了什么
a. 如何读取 spring.factories 中的配置
b. run 方法内获取事件发布器 (得到 SpringApplicationRunListeners) 的过程, 对应步骤中
    1.获取事件发布器
    发布 application starting 事件1️⃣
    发布 application environment 已准备事件2️⃣
    发布 application context 已初始化事件3️⃣
    发布 application prepared 事件4️⃣
    发布 application started 事件5️⃣
    发布 application ready 事件6️⃣
    这其中有异常，发布 application failed 事件7️⃣
```
## run - 8-11，2，12
```java

import org.springframework.beans.factory.support.DefaultListableBeanFactory;
import org.springframework.beans.factory.xml.XmlBeanDefinitionReader;
import org.springframework.boot.*;
import org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory;
import org.springframework.boot.web.reactive.context.AnnotationConfigReactiveWebServerApplicationContext;
import org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext;
import org.springframework.boot.web.servlet.server.ServletWebServerFactory;
import org.springframework.context.ApplicationContextInitializer;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.*;
import org.springframework.context.support.GenericApplicationContext;
import org.springframework.core.io.ClassPathResource;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.Arrays;
import java.util.Set;

// 运行时请添加运行参数 --server.port=8080 debug
public class A39_3 {
    @SuppressWarnings("all")
    public static void main(String[] args) throws Exception {
        SpringApplication app = new SpringApplication();

        //app.setSources();bean的来源  xml，配置类，扫包
        app.addInitializers(new ApplicationContextInitializer<ConfigurableApplicationContext>() {
            @Override
            public void initialize(ConfigurableApplicationContext applicationContext) {
                System.out.println("执行初始化器增强...");
            }
        });

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>> 2. 封装启动 args");
        DefaultApplicationArguments arguments = new DefaultApplicationArguments(args);

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>> 8. 创建容器");
        GenericApplicationContext context = createApplicationContext(WebApplicationType.SERVLET);

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>> 9. 准备容器");
        for (ApplicationContextInitializer initializer : app.getInitializers()) {
            initializer.initialize(context);
        }

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>> 10. 加载 bean 定义");
        DefaultListableBeanFactory beanFactory = context.getDefaultListableBeanFactory();
        AnnotatedBeanDefinitionReader reader1 = new AnnotatedBeanDefinitionReader(beanFactory);
        XmlBeanDefinitionReader reader2 = new XmlBeanDefinitionReader(beanFactory);
        ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(beanFactory);

        reader1.register(Config.class);
        reader2.loadBeanDefinitions(new ClassPathResource("b03.xml"));
        scanner.scan("com.itheima.a39.sub");

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>> 11. refresh 容器");
        context.refresh();

        for (String name : context.getBeanDefinitionNames()) {
            System.out.println("name:" + name + " 来源：" + beanFactory.getBeanDefinition(name).getResourceDescription());
        }

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>> 12. 执行 runner");
        for (CommandLineRunner runner : context.getBeansOfType(CommandLineRunner.class).values()) {
            runner.run(args);
        }

        for (ApplicationRunner runner : context.getBeansOfType(ApplicationRunner.class).values()) {
            runner.run(arguments);
        }

        /*
            学到了什么
            a. 创建容器、加载 bean 定义、refresh, 对应的步骤

         */
    }

    private static GenericApplicationContext createApplicationContext(WebApplicationType type) {
        GenericApplicationContext context = null;
        switch (type) {
            case SERVLET -> context = new AnnotationConfigServletWebServerApplicationContext();
            case REACTIVE -> context = new AnnotationConfigReactiveWebServerApplicationContext();
            case NONE -> context = new AnnotationConfigApplicationContext();
        }
        return context;
    }

    static class Bean4 {

    }

    static class Bean5 {

    }

    static class Bean6 {

    }

    @Configuration
    static class Config {
        @Bean
        public Bean5 bean5() {
            return new Bean5();
        }

        @Bean
        public ServletWebServerFactory servletWebServerFactory() {
            return new TomcatServletWebServerFactory();
        }

        @Bean
        public CommandLineRunner commandLineRunner() {
            return new CommandLineRunner() {
                @Override
                public void run(String... args) throws Exception {
                    System.out.println("commandLineRunner()..." + Arrays.toString(args));
                }
            };
        }

        @Bean
        public ApplicationRunner applicationRunner() {
            return new ApplicationRunner() {
                @Override
                public void run(ApplicationArguments args) throws Exception {
                    System.out.println("applicationRunner()..." + Arrays.toString(args.getSourceArgs()));
                    System.out.println(args.getOptionNames());
                    System.out.println(args.getOptionValues("server.port"));
                    System.out.println(args.getNonOptionArgs());
                }
            };
        }
    }
}

```
## Step3
```java
public class Step3 {
    public static void main(String[] args) throws IOException {
        //1.把环境变量准备好
        ApplicationEnvironment env = new ApplicationEnvironment(); // 系统环境变量, properties, yaml
        //2.添加命令行参数来源
        env.getPropertySources().addFirst(new SimpleCommandLinePropertySource(args));
        //3.properties是在后续步骤添加的
        env.getPropertySources().addLast(new ResourcePropertySource(new ClassPathResource("step3.properties")));

        for (PropertySource<?> ps : env.getPropertySources()) {
            System.out.println(ps);
        }
//        System.out.println(env.getProperty("JAVA_HOME"));

        System.out.println(env.getProperty("server.port"));
    }
}

```
## Step4

```java
public class Step4 {

    public static void main(String[] args) throws IOException, NoSuchFieldException {
        ApplicationEnvironment env = new ApplicationEnvironment();
        env.getPropertySources().addLast(
                new ResourcePropertySource("step4", new ClassPathResource("step4.properties"))
        );
        ConfigurationPropertySources.attach(env);
        for (PropertySource<?> ps : env.getPropertySources()) {
            System.out.println(ps);
        }

        System.out.println(env.getProperty("user.first-name"));
        System.out.println(env.getProperty("user.middle-name"));
        System.out.println(env.getProperty("user.last-name"));
    }
}

```
```java
user.first-name=George
user.middle_name=Walker
user.lastName=Bush

```

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1691068607546-f0426f4b-0e84-4bac-825d-dc1fa3d32599.png#averageHue=%23edeee9&clientId=u90650031-d131-4&from=paste&height=151&id=u0b93173d&originHeight=227&originWidth=1311&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=130465&status=done&style=none&taskId=ua5594415-2a79-4740-9749-4277a5b1af2&title=&width=874)
## Step5

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1691068801258-a4f29b73-98a0-4e6a-a216-391333a06038.png#averageHue=%23eae8dd&clientId=u90650031-d131-4&from=paste&height=549&id=u6088537d&originHeight=823&originWidth=1764&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=626649&status=done&style=none&taskId=udf086c4b-b070-40df-8201-a30feb608b9&title=&width=1176)
```java
 SpringApplication app = new SpringApplication();
 ApplicationEnvironment env = new ApplicationEnvironment();
 System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强前");
 for (PropertySource<?> ps : env.getPropertySources()) {
     System.out.println(ps);
 }
 ConfigDataEnvironmentPostProcessor postProcessor1 = new ConfigDataEnvironmentPostProcessor(new DeferredLogs(), new DefaultBootstrapContext());
 postProcessor1.postProcessEnvironment(env, app);
 System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强后");
 for (PropertySource<?> ps : env.getPropertySources()) {
     System.out.println(ps);
 }
 RandomValuePropertySourceEnvironmentPostProcessor postProcessor2 = new RandomValuePropertySourceEnvironmentPostProcessor(new DeferredLog());
 postProcessor2.postProcessEnvironment(env, app);
 System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强后");
 for (PropertySource<?> ps : env.getPropertySources()) {
     System.out.println(ps);
 }
 System.out.println(env.getProperty("server.port"));
 System.out.println(env.getProperty("random.int"));
 System.out.println(env.getProperty("random.int"));
 System.out.println(env.getProperty("random.int"));
 System.out.println(env.getProperty("random.uuid"));
 System.out.println(env.getProperty("random.uuid"));
 System.out.println(env.getProperty("random.uuid"));
```
spring 其实是从配置文件中读取。而不是手动在代码里面一个个加
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1691070832370-c6b35d2a-e616-42fe-8a54-e01c4ae31dc2.png#averageHue=%23f6f6f5&clientId=u90650031-d131-4&from=paste&height=297&id=ub2d3a76b&originHeight=446&originWidth=937&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=156219&status=done&style=none&taskId=u1a3a0b09-f53b-4c7e-9c42-4bbf8ac229d&title=&width=624.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1691070850270-4313c0a9-5125-4efb-b97e-f2ed067a4363.png#averageHue=%23e6eddf&clientId=u90650031-d131-4&from=paste&height=327&id=uad52a042&originHeight=491&originWidth=1001&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=436553&status=done&style=none&taskId=u3d99fdb0-8d4b-4c42-8366-d100a3af0d8&title=&width=667.3333333333334)
```java
/*
    可以添加参数 --spring.application.json={\"server\":{\"port\":9090}} 测试 SpringApplicationJsonEnvironmentPostProcessor
 */
public class Step5 {
    public static void main(String[] args) {
        SpringApplication app = new SpringApplication();
        app.addListeners(new EnvironmentPostProcessorApplicationListener());

        /*List<String> names = SpringFactoriesLoader.loadFactoryNames(EnvironmentPostProcessor.class, Step5.class.getClassLoader());
        for (String name : names) {
            System.out.println(name);
        }*/

        EventPublishingRunListener publisher = new EventPublishingRunListener(app, args);
        ApplicationEnvironment env = new ApplicationEnvironment();
        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强前");
        for (PropertySource<?> ps : env.getPropertySources()) {
            System.out.println(ps);
        }
        publisher.environmentPrepared(new DefaultBootstrapContext(), env);
        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强后");
        for (PropertySource<?> ps : env.getPropertySources()) {
            System.out.println(ps);
        }

    }

    private static void test1() {
        SpringApplication app = new SpringApplication();
        ApplicationEnvironment env = new ApplicationEnvironment();

        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强前");
        for (PropertySource<?> ps : env.getPropertySources()) {
            System.out.println(ps);
        }
        ConfigDataEnvironmentPostProcessor postProcessor1 = new ConfigDataEnvironmentPostProcessor(new DeferredLogs(), new DefaultBootstrapContext());
        postProcessor1.postProcessEnvironment(env, app);
        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强后");
        for (PropertySource<?> ps : env.getPropertySources()) {
            System.out.println(ps);
        }
        RandomValuePropertySourceEnvironmentPostProcessor postProcessor2 = new RandomValuePropertySourceEnvironmentPostProcessor(new DeferredLog());
        postProcessor2.postProcessEnvironment(env, app);
        System.out.println(">>>>>>>>>>>>>>>>>>>>>>>>> 增强后");
        for (PropertySource<?> ps : env.getPropertySources()) {
            System.out.println(ps);
        }
        System.out.println(env.getProperty("server.port"));
        System.out.println(env.getProperty("random.int"));
        System.out.println(env.getProperty("random.int"));
        System.out.println(env.getProperty("random.int"));
        System.out.println(env.getProperty("random.uuid"));
        System.out.println(env.getProperty("random.uuid"));
        System.out.println(env.getProperty("random.uuid"));
    }
}

```
## Step6
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1691072478470-c7b18735-3ba2-406b-b11f-b6c84741c7f9.png#averageHue=%23f8f8f7&clientId=u90650031-d131-4&from=paste&height=410&id=u9ae10782&originHeight=615&originWidth=1425&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=412163&status=done&style=none&taskId=u805b6977-ce19-4e67-8e51-bd576179ac1&title=&width=950)
```java
public class Step6 {
    // 绑定 spring.main 前缀的 key value 至 SpringApplication, 请通过 debug 查看
    public static void main(String[] args) throws IOException {
        SpringApplication application = new SpringApplication();
        ApplicationEnvironment env = new ApplicationEnvironment();
        env.getPropertySources().addLast(new ResourcePropertySource("step4", new ClassPathResource("step4.properties")));
        env.getPropertySources().addLast(new ResourcePropertySource("step6", new ClassPathResource("step6.properties")));

        //将配置属性和对象进行绑定
//        User user = Binder.get(env).bind("user", User.class).get();
//        System.out.println(user);

//        User user = new User();
//        Binder.get(env).bind("user", Bindable.ofInstance(user));
//        System.out.println(user);

        System.out.println(application);
        Binder.get(env).bind("spring.main", Bindable.ofInstance(application));
        System.out.println(application);
    }

    static class User {
        private String firstName;
        private String middleName;
        private String lastName;
        public String getFirstName() {
            return firstName;
        }
        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }
        public String getMiddleName() {
            return middleName;
        }
        public void setMiddleName(String middleName) {
            this.middleName = middleName;
        }
        public String getLastName() {
            return lastName;
        }
        public void setLastName(String lastName) {
            this.lastName = lastName;
        }
        @Override
        public String toString() {
            return "User{" +
                   "firstName='" + firstName + '\'' +
                   ", middleName='" + middleName + '\'' +
                   ", lastName='" + lastName + '\'' +
                   '}';
        }
    }
}

```
## Step7
```java
public class Step7 {
    public static void main(String[] args) {
        ApplicationEnvironment env = new ApplicationEnvironment();
        SpringApplicationBannerPrinter printer = new SpringApplicationBannerPrinter(
                new DefaultResourceLoader(),
                new SpringBootBanner()
        );
        // 测试文字 banner
//        env.getPropertySources().addLast(new MapPropertySource("custom", Map.of("spring.banner.location","banner1.txt")));
        // 测试图片 banner
//        env.getPropertySources().addLast(new MapPropertySource("custom", Map.of("spring.banner.image.location","banner2.png")));
        // 版本号的获取
        System.out.println(SpringBootVersion.getVersion());
        printer.print(env, Step7.class, System.out);
    }
}

```


![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1691073096678-d839c9aa-9cc4-4a2d-9058-75b28b606769.png#averageHue=%23f8f7f2&clientId=u90650031-d131-4&from=paste&height=609&id=u96e24222&originHeight=913&originWidth=1693&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=695586&status=done&style=none&taskId=ue54a9a6b-483c-49e1-8558-3356cd4dee8&title=&width=1128.6666666666667)

## 总结
```java

1. 创建事件发布器，发布程序启动事件
SpringApplicationRunListeners listeners = getRunListeners(args);
listeners.starting(bootstrapContext, this.mainApplicationClass);

2.封装参数对象，区别就是把参数分成选项参数（--开头）和非选项参数
ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);

3. 创建环境参数对象
	private ConfigurableEnvironment prepareEnvironment(SpringApplicationRunListeners listeners,
			DefaultBootstrapContext bootstrapContext, ApplicationArguments applicationArguments) {
		// Create and configure the environment
		ConfigurableEnvironment environment = getOrCreateEnvironment();
        //添加环境来源
		configureEnvironment(environment, applicationArguments.getSourceArgs());】
        //4. 对配置key进行规范处理，对last_name,lastName,last-name。统一成-分隔的。
        ConfigurationPropertySources.attach(environment);
        //5.基本环境已经准备好了，发个事件。应用程序会从配置文件加载对应的监听器。进行扩展添加更多的源。
        listeners.environmentPrepared(bootstrapContext, environment);
        //6.将spring.main打头的绑定到springapplicaton对象上面
        bindToSpringApplication(environment);
7. 打印banner信息。
Banner printedBanner = printBanner(environment);
8. 创建spring容器。根据构造方法中决定三种的一种。
context = createApplicationContext();

 private void prepareContext(DefaultBootstrapContext bootstrapContext, ConfigurableApplicationContext context,
			ConfigurableEnvironment environment, SpringApplicationRunListeners listeners,
			ApplicationArguments applicationArguments, Banner printedBanner) {
		context.setEnvironment(environment);
		postProcessApplicationContext(context);
        9.应用初始化器
		applyInitializers(context);
        发布事件
		listeners.contextPrepared(context);

      	// Load the sources
        10.加载各种来源的bean定义
		Set<Object> sources = getAllSources();
		Assert.notEmpty(sources, "Sources must not be empty");
		load(context, sources.toArray(new Object[0]));
		listeners.contextLoaded(context);

    11. 执行bean工厂的后置处理器和bean的后置处理器，初始化单例bean。
	protected void refresh(ConfigurableApplicationContext applicationContext) {
		applicationContext.refresh();
	}

	listeners.started(context);
    12.调用实现了ApplicationRunner和CommandLineRunner接口类
	callRunners(context, applicationArguments);
```
