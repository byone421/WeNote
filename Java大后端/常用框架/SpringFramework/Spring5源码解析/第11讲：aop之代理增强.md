# JDK代理
```java
public class JdkProxyDemo {
    interface Foo {
        void foo();
    }

    static final class Target implements Foo {
        public void foo() {
            System.out.println("target foo");
        }
    }

    // jdk代理 只能针对接口代理
    public static void main(String[] param) throws IOException {
        // 目标对象
        Target target = new Target();

        // 用来加载在运行期间动态生成的字节码
        ClassLoader loader = JdkProxyDemo.class.getClassLoader();
        Foo proxy = (Foo) Proxy.newProxyInstance(loader, new Class[]{Foo.class}, (p, method, args) -> {
            System.out.println("before...");
            // 目标.方法(参数)
            // 方法.invoke(目标, 参数);
            Object result = method.invoke(target, args);
            System.out.println("after....");
            // 让代理也返回目标方法执行的结果
            return result;
        });

        System.out.println(proxy.getClass());

        proxy.foo();
        System.in.read();
    }
}
```
#### 收获💡

- jdk 动态代理要求目标**必须**实现接口，生成的代理类实现相同接口，因此代理与目标之间是平级兄弟关系
# CGLIB代理
```java
public class CglibProxyDemo {

    static class Target {
        public void foo() {
            System.out.println("target foo");
        }
    }

    // 代理是子类型, 目标是父类型
    public static void main(String[] param) {
        Target proxy = (Target) Enhancer.create(Target.class, (MethodInterceptor) (p, method, args, methodProxy) -> {
            System.out.println("before...");
            // 用方法反射调用目标
			// Object result = method.invoke(target, args); 
            // methodProxy 它可以避免反射调用
            // 内部没有用反射, 需要目标 （spring）
			//Object result = methodProxy.invoke(target, args); 
            // 内部没有用反射, 需要代理
            Object result = methodProxy.invokeSuper(p, args); 
            System.out.println("after...");
            return result;
        });
        proxy.foo();

    }
}
```
运行结果与 jdk 动态代理相同

#### 收获💡

- cglib 不要求目标实现接口，它生成的代理类是目标的子类，因此代理与目标之间是子父关系
- 限制⛔：根据上述分析 final 类无法被 cglib 增强
