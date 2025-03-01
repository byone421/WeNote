# Advisor和Aspect
## 高级切面类
常用，方便开发
```java
@Aspect // 高级切面类
@Order(1)
static class Aspect1 {
    @Before("execution(* foo())")
    public void before1() {
        System.out.println("aspect1 before1...");
    }
    @Before("execution(* foo())")
    public void before2() {
        System.out.println("aspect1 before2...");
    }
}
```
## 低级切面类
更底层，需要自己手写advisor
```java
@Configuration
static class Config {
    @Bean // 低级切面
    public Advisor advisor3(MethodInterceptor advice3) {
        AspectJExpressionPointcut pointcut = new AspectJExpressionPointcut();
        pointcut.setExpression("execution(* foo())");
        DefaultPointcutAdvisor advisor = new DefaultPointcutAdvisor(pointcut, advice3);
        return advisor;
    }
    @Bean
    public MethodInterceptor advice3() {
        return invocation -> {
            System.out.println("advice3 before...");
            Object result = invocation.proceed();
            System.out.println("advice3 after...");
            return result;
        };
    }
}
```
# findEligibleAdvisors和wrapIfNecessary
```java
package org.springframework.aop.framework.autoproxy;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.aop.Advisor;
import org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ConfigurationClassPostProcessor;
import org.springframework.context.support.GenericApplicationContext;
import org.springframework.core.annotation.Order;

import java.util.List;

public class A17 {
    public static void main(String[] args) {
        GenericApplicationContext context = new GenericApplicationContext();
        context.registerBean("aspect1", Aspect1.class);
        context.registerBean("config", Config.class);
        context.registerBean(ConfigurationClassPostProcessor.class);
        context.registerBean(AnnotationAwareAspectJAutoProxyCreator.class);
        context.refresh();
        
        /*
            第一个重要方法 findEligibleAdvisors 找到有【资格】的 Advisors
                a. 有【资格】的 Advisor 一部分是低级的, 可以由自己编写
                b. 有【资格】的 Advisor 另一部分是高级的, 由本章的主角解析 @Aspect 后获得
         */
        AnnotationAwareAspectJAutoProxyCreator creator = context.getBean(AnnotationAwareAspectJAutoProxyCreator.class);
        
        List<Advisor> advisors = creator.findEligibleAdvisors(Target2.class, "target2");
        for (Advisor advisor : advisors) {
            System.out.println(advisor);
        }

        /*
            第二个重要方法 wrapIfNecessary
                a. 它内部调用 findEligibleAdvisors, 只要返回集合不空, 则表示需要创建代理
         */
        Object o1 = creator.wrapIfNecessary(new Target1(), "target1", "target1");
        System.out.println(o1.getClass());
        Object o2 = creator.wrapIfNecessary(new Target2(), "target2", "target2");
        System.out.println(o2.getClass());

        ((Target1) o1).foo();
        /*
            学到了什么
                a. 自动代理后处理器 AnnotationAwareAspectJAutoProxyCreator 会帮我们创建代理
                b. 通常代理创建的活在原始对象初始化后执行, 但碰到循环依赖会提前至依赖注入之前执行
                c. 高级的 @Aspect 切面会转换为低级的 Advisor 切面, 理解原理, 大道至简
         */
    }

    static class Target1 {
        public void foo() {
            System.out.println("target1 foo");
        }
    }

    static class Target2 {
        public void bar() {
            System.out.println("target2 bar");
        }
    }

    @Aspect // 高级切面类
    @Order(1)
    static class Aspect1 {
        @Before("execution(* foo())")
        public void before1() {
            System.out.println("aspect1 before1...");
        }

        @Before("execution(* foo())")
        public void before2() {
            System.out.println("aspect1 before2...");
        }
    }

    @Configuration
    static class Config {
        /*@Bean // 低级切面
        public Advisor advisor3(MethodInterceptor advice3) {
            AspectJExpressionPointcut pointcut = new AspectJExpressionPointcut();
            pointcut.setExpression("execution(* foo())");
            DefaultPointcutAdvisor advisor = new DefaultPointcutAdvisor(pointcut, advice3);
            return advisor;
        }
        @Bean
        public MethodInterceptor advice3() {
            return invocation -> {
                System.out.println("advice3 before...");
                Object result = invocation.proceed();
                System.out.println("advice3 after...");
                return result;
            };
        }*/
    }

}

```
# 创建代理的时机
## 有两种情况
 创建 ->** (*)** 依赖注入 -> 初始化** (*)**
**（*）代表创建的时机**

1. 初始化之后 (无循环依赖时)
2. 实例创建后, 依赖注入前 (有循环依赖时), 并暂存于二级缓存
### 测试情况1
```java
package org.springframework.aop.framework.autoproxy;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;
import org.springframework.aop.Advisor;
import org.springframework.aop.aspectj.AspectJExpressionPointcut;
import org.springframework.aop.aspectj.annotation.AnnotationAwareAspectJAutoProxyCreator;
import org.springframework.aop.support.DefaultPointcutAdvisor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.CommonAnnotationBeanPostProcessor;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ConfigurationClassPostProcessor;
import org.springframework.context.support.GenericApplicationContext;

import javax.annotation.PostConstruct;

public class A17_1 {

    public static void main(String[] args) {
        GenericApplicationContext context = new GenericApplicationContext();
        context.registerBean(ConfigurationClassPostProcessor.class);
        context.registerBean(Config.class);
        context.refresh();
        context.close();

    }

    @Configuration
    static class Config {
        @Bean // 解析 @Aspect、产生代理
        public AnnotationAwareAspectJAutoProxyCreator annotationAwareAspectJAutoProxyCreator() {
            return new AnnotationAwareAspectJAutoProxyCreator();
        }

        @Bean // 解析 @Autowired
        public AutowiredAnnotationBeanPostProcessor autowiredAnnotationBeanPostProcessor() {
            return new AutowiredAnnotationBeanPostProcessor();
        }

        @Bean // 解析 @PostConstruct
        public CommonAnnotationBeanPostProcessor commonAnnotationBeanPostProcessor() {
            return new CommonAnnotationBeanPostProcessor();
        }

        @Bean
        public Advisor advisor(MethodInterceptor advice) {
            AspectJExpressionPointcut pointcut = new AspectJExpressionPointcut();
            pointcut.setExpression("execution(* foo())");
            return new DefaultPointcutAdvisor(pointcut, advice);
        }

        @Bean
        public MethodInterceptor advice() {
            return (MethodInvocation invocation) -> {
                System.out.println("before...");
                return invocation.proceed();
            };
        }

        @Bean
        public Bean1 bean1() {
            return new Bean1();
        }

        @Bean
        public Bean2 bean2() {
            return new Bean2();
        }
    }

    static class Bean1 {
        public void foo() {}
        public Bean1() {
            System.out.println("Bean1()");
        }
        @PostConstruct public void init() {
            System.out.println("Bean1 init()");
        }
    }

    static class Bean2 {
        public Bean2() {
            System.out.println("Bean2()");
        }
        @Autowired public void setBean1(Bean1 bean1) {
            System.out.println("Bean2 setBean1(bean1) class is: " + bean1.getClass());
        }
        @PostConstruct public void init() {
            System.out.println("Bean2 init()");
        }
    }
}

```
运行以上代码输入
```java
Bean1()
Bean1 init()
// 可以看到Bean1 init()之后有AnnotationAwareAspectJAutoProxyCreator的输入，说明是在创建之后才创建的代理
[TRACE] 22:46:36.388 [main] o.s.a.a.a.AnnotationAwareAspectJAutoProxyCreator - Creating implicit proxy for bean 'bean1' with 0 common interceptors and 2 specific interceptors 
Bean2()
// 可以看到bean2在setbean1的时候注入的是bean1的代理对象
Bean2 setBean1(bean1) class is: class org.springframework.aop.framework.autoproxy.A17_1$Bean1$$EnhancerBySpringCGLIB$$9fdb3164
Bean2 init()
```
### 测试情况2
将Bean1的代码改动一下，增加依赖注入 setBean2(Bean2 bean2)。造成循环依赖
```java
    static class Bean1 {
        public void foo() {

        }
        public Bean1() {
            System.out.println("Bean1()");
        }
        @Autowired public void setBean2(Bean2 bean2) {
            System.out.println("Bean1 setBean2(bean2) class is: " + bean2.getClass());
        }
        @PostConstruct public void init() {
            System.out.println("Bean1 init()");
        }
    }
```
运行输出
```java
Bean1()
//创建bean1 发现需要用到bean2,跑去创建Bean2
Bean2()
//bean2需要bean1，所以创建了bean1的代理对象给bean2
[TRACE] 22:54:54.682 [main] o.s.a.a.a.AnnotationAwareAspectJAutoProxyCreator - Creating implicit proxy for bean 'bean1' with 0 common interceptors and 2 specific interceptors 
Bean2 setBean1(bean1) class is: class org.springframework.aop.framework.autoproxy.A17_1$Bean1$$EnhancerBySpringCGLIB$$5455d566
Bean2 init()
//bean2创建完成，继续创建bean1
Bean1 setBean2(bean2) class is: class org.springframework.aop.framework.autoproxy.A17_1$Bean2
Bean1 init()
```
# 高级切面转低级切面
将Aspect转成Advisor
```java
package org.springframework.aop.framework.autoproxy;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Before;
import org.springframework.aop.Advisor;
import org.springframework.aop.aspectj.AspectInstanceFactory;
import org.springframework.aop.aspectj.AspectJExpressionPointcut;
import org.springframework.aop.aspectj.AspectJMethodBeforeAdvice;
import org.springframework.aop.aspectj.SingletonAspectInstanceFactory;
import org.springframework.aop.interceptor.ExposeInvocationInterceptor;
import org.springframework.aop.support.DefaultPointcutAdvisor;

import java.lang.reflect.Method;
import java.util.ArrayList;
import java.util.List;

public class A17_2 {

    static class Aspect {
        @Before("execution(* foo())")
        public void before1() {
            System.out.println("before1");
        }

        @Before("execution(* foo())")
        public void before2() {
            System.out.println("before2");
        }

        public void after() {
            System.out.println("after");
        }

        public void afterReturning() {
            System.out.println("afterReturning");
        }

        public void afterThrowing() {
            System.out.println("afterThrowing");
        }

        public Object around(ProceedingJoinPoint pjp) throws Throwable {
            try {
                System.out.println("around...before");
                return pjp.proceed();
            } finally {
                System.out.println("around...after");
            }
        }
    }

    static class Target {
        public void foo() {
            System.out.println("target foo");
        }
    }

    @SuppressWarnings("all")
    public static void main(String[] args) throws Throwable {

        AspectInstanceFactory factory = new SingletonAspectInstanceFactory(new Aspect());
        // 高级切面转低级切面类
        List<Advisor> list = new ArrayList<>();
        for (Method method : Aspect.class.getDeclaredMethods()) {
            if (method.isAnnotationPresent(Before.class)) {
                // 解析切点
                String expression = method.getAnnotation(Before.class).value();
                AspectJExpressionPointcut pointcut = new AspectJExpressionPointcut();
                pointcut.setExpression(expression);
                // 通知类
                AspectJMethodBeforeAdvice advice = new AspectJMethodBeforeAdvice(method, pointcut, factory);
                // 切面
                Advisor advisor = new DefaultPointcutAdvisor(pointcut, advice);
                list.add(advisor);
            }
        }
        for (Advisor advisor : list) {
            System.out.println(advisor);
        }

    }
}

```
### 小总结
  @Before 前置通知会被转换为下面原始的 AspectJMethodBeforeAdvice 形式, 该对象包含了如下信息
    	 a. 通知代码从哪儿来 
         b. 切点是什么
         c. 通知对象如何创建, 本例共用同一个 Aspect 对象
 类似的通知还有
         1. AspectJAroundAdvice (环绕通知)
         2. AspectJAfterReturningAdvice
         3. AspectJAfterThrowingAdvice
         4. AspectJAfterAdvice (环绕通知)
