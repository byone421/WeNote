#  两个切面概念
```
aspect =
     通知1(advice) +  切点1(pointcut)
     通知2(advice) +  切点2(pointcut)
     通知3(advice) +  切点3(pointcut)
     ...
advisor = 更细粒度的切面，包含一个通知和切点

```
平时用的多是aspect，advisor相对底层。一个aspect多个通知和切点也会被拆成多个advisor。

# Spring选择代理
Spring 中对切点、通知、切面的抽象如下

- 切点：接口 Pointcut，典型实现 AspectJExpressionPointcut
- 通知：典型接口为 MethodInterceptor 代表环绕通知
- 切面：Advisor，包含一个 Advice 通知，PointcutAdvisor 包含一个 Advice 通知和一个 Pointcut

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1688566278036-5547833f-14fd-489f-974d-d577f70ec9f5.png#averageHue=%23fcfcfc&clientId=ua49800aa-8f05-4&from=paste&height=567&id=u83f55e0c&originHeight=850&originWidth=1448&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=46667&status=done&style=none&taskId=u30df9011-e088-459a-84e7-b196db9e0fc&title=&width=965.3333333333334)
**代码1**
```java
public class A15 {
    public static void main(String[] args) {
        // 1. 备好切点
        AspectJExpressionPointcut pointcut = new AspectJExpressionPointcut();
        pointcut.setExpression("execution(* foo())");
        // 2. 备好通知
        MethodInterceptor advice = invocation -> {
            System.out.println("before...");
            Object result = invocation.proceed(); // 调用目标
            System.out.println("after...");
            return result;
        };
        // 3. 备好切面
        DefaultPointcutAdvisor advisor = new DefaultPointcutAdvisor(pointcut, advice);

        /*
		4. 创建代理
			a. proxyTargetClass = false, 目标实现了接口, 用 jdk 实现
			b. proxyTargetClass = false,  目标没有实现接口, 用 cglib 实现
			c. proxyTargetClass = true, 总是使用 cglib 实现
		*/
        Target1 target = new Target1();
        ProxyFactory factory = new ProxyFactory();
        factory.setTarget(target);
        factory.addAdvisor(advisor);
        Target1 proxy = (Target1) factory.getProxy();
        System.out.println(proxy.getClass());
        proxy.foo();
        proxy.bar();
    }

    interface I1 {
        void foo();

        void bar();
    }

    static class Target1 implements I1 {
        public void foo() {
            System.out.println("target1 foo");
        }

        public void bar() {
            System.out.println("target1 bar");
        }
    }

    static class Target2 {
        public void foo() {
            System.out.println("target2 foo");
        }

        public void bar() {
            System.out.println("target2 bar");
        }
    }
}

```
**代码2**
```java

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;
import org.springframework.aop.aspectj.AspectJExpressionPointcut;
import org.springframework.aop.framework.ProxyFactory;
import org.springframework.aop.support.DefaultPointcutAdvisor;

public class A15 {
    public static void main(String[] args) {
        /*
            两个切面概念
              =
                通知1(advice) +  切点1(pointcut)
                通知2(advice) +  切点2(pointcut)
                通知3(advice) +  切点3(pointcut)
                ...
            advisor = 更细粒度的切面，包含一个通知和切点
         */

        // 1. 备好切点
        AspectJExpressionPointcut pointcut = new AspectJExpressionPointcut();
        pointcut.setExpression("execution(* foo())");
        // 2. 备好通知
        MethodInterceptor advice = invocation -> {
            System.out.println("before...");
            Object result = invocation.proceed(); // 调用目标
            System.out.println("after...");
            return result;
        };
        // 3. 备好切面
        DefaultPointcutAdvisor advisor = new DefaultPointcutAdvisor(pointcut, advice);

        /*
           4. 创建代理
                a. proxyTargetClass = false, 目标实现了接口, 用 jdk 实现
                b. proxyTargetClass = false,  目标没有实现接口, 用 cglib 实现
                c. proxyTargetClass = true, 总是使用 cglib 实现
         */
        Target2 target = new Target2();
        ProxyFactory factory = new ProxyFactory();
        factory.setTarget(target);
        factory.addAdvisor(advisor);
        factory.setInterfaces(target.getClass().getInterfaces());
        factory.setProxyTargetClass(false);
        Target2 proxy = (Target2) factory.getProxy();
        System.out.println(proxy.getClass());
        proxy.foo();
        proxy.bar();
        /*
            学到了什么
                a. Spring 的代理选择规则
                b. 底层的切点实现
                c. 底层的通知实现
                d. ProxyFactory 是用来创建代理的核心实现, 用 AopProxyFactory 选择具体代理实现
                    - JdkDynamicAopProxy
                    - ObjenesisCglibAopProxy
         */
    }

    interface I1 {
        void foo();

        void bar();
    }

    static class Target1 implements I1 {
        public void foo() {
            System.out.println("target1 foo");
        }

        public void bar() {
            System.out.println("target1 bar");
        }
    }

    static class Target2 {
        public void foo() {
            System.out.println("target2 foo");
        }

        public void bar() {
            System.out.println("target2 bar");
        }
    }
}
```
# 代理相关类图

- AopProxyFactory 根据 proxyTargetClass 等设置选择 AopProxy 实现
- AopProxy 通过 getProxy 创建代理对象
- 图中 Proxy 都实现了 Advised 接口，能够获得关联的切面集合与目标（其实是从 ProxyFactory 取得）
- 调用代理方法时，会借助 ProxyFactory 将通知统一转为环绕通知：MethodInterceptor

![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1688567926520-7f5c111a-153e-4595-9bb5-112793095689.png#averageHue=%23fbfbfa&clientId=ua49800aa-8f05-4&from=paste&height=799&id=uce959450&originHeight=1199&originWidth=1377&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=100310&status=done&style=none&taskId=uc128b363-30c4-4025-960c-31f9e6b33c7&title=&width=918)

# 收获💡

1. 底层的切点实现
2. 底层的通知实现
3. 底层的切面实现
4. ProxyFactory 用来创建代理 
   - 如果指定了接口，且 proxyTargetClass = false，使用 JdkDynamicAopProxy
   - 如果没有指定接口，或者 proxyTargetClass = true，使用 ObjenesisCglibAopProxy 
      - 例外：如果目标是接口类型或已经是 Jdk 代理，使用 JdkDynamicAopProxy
