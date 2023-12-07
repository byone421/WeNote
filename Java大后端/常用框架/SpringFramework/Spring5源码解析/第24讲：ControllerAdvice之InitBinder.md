InitBinder配合ControllerAdvice对所有Controller生效，写在controller里面对单个controller有效
```java
@Configuration
public class WebConfig {

    @ControllerAdvice
    static class MyControllerAdvice {
        @InitBinder
        public void binder3(WebDataBinder webDataBinder) {
            webDataBinder.addCustomFormatter(new MyDateFormatter("binder3 转换器"));
        }
    }

    @Controller
    static class Controller1 {
        @InitBinder
        public void binder1(WebDataBinder webDataBinder) {
            webDataBinder.addCustomFormatter(new MyDateFormatter("binder1 转换器"));
        }

        public void foo() {

        }
    }

    @Controller
    static class Controller2 {
        @InitBinder
        public void binder21(WebDataBinder webDataBinder) {
            webDataBinder.addCustomFormatter(new MyDateFormatter("binder21 转换器"));
        }

        @InitBinder
        public void binder22(WebDataBinder webDataBinder) {
            webDataBinder.addCustomFormatter(new MyDateFormatter("binder22 转换器"));
        }

        public void bar() {

        }
    }

}
```
# @InitBinder 的来源有两个
1. @ControllerAdvice 中 @InitBinder 标注的方法，由 RequestMappingHandlerAdapter 在初始化时解析并记录
2. @Controller 中 @InitBinder 标注的方法，由 RequestMappingHandlerAdapter 会在控制器方法首次执行时解析并记录
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1689692214166-e916beb2-91e9-49cc-b0ea-de179b9b4781.png#averageHue=%23c5edcb&clientId=u7184f4fa-8bfe-4&from=paste&height=467&id=u4d02249f&originHeight=701&originWidth=1777&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=113122&status=done&style=none&taskId=u46875984-bebb-48d4-93a5-b0be83b026e&title=&width=1184.6666666666667)
# 执行时机
## @ControllerAdvice 中 @InitBinder 
```java
    private static void showBindMethods(RequestMappingHandlerAdapter handlerAdapter) throws NoSuchFieldException, IllegalAccessException {
        Field initBinderAdviceCache = RequestMappingHandlerAdapter.class.getDeclaredField("initBinderAdviceCache");
        initBinderAdviceCache.setAccessible(true);
        Map<ControllerAdviceBean, Set<Method>> globalMap = (Map<ControllerAdviceBean, Set<Method>>) initBinderAdviceCache.get(handlerAdapter);
        log.debug("全局的 @InitBinder 方法 {}",
                globalMap.values().stream()
                        .flatMap(ms -> ms.stream().map(m -> m.getName()))
                        .collect(Collectors.toList())
        );

        Field initBinderCache = RequestMappingHandlerAdapter.class.getDeclaredField("initBinderCache");
        initBinderCache.setAccessible(true);
        Map<Class<?>, Set<Method>> controllerMap = (Map<Class<?>, Set<Method>>) initBinderCache.get(handlerAdapter);
        log.debug("控制器的 @InitBinder 方法 {}",
                controllerMap.entrySet().stream()
                        .flatMap(e -> e.getValue().stream().map(v -> e.getKey().getSimpleName() + "." + v.getName()))
                        .collect(Collectors.toList())
        );
    }
```
```java
AnnotationConfigApplicationContext context =
        new AnnotationConfigApplicationContext(WebConfig.class);
RequestMappingHandlerAdapter handlerAdapter = new RequestMappingHandlerAdapter();
handlerAdapter.setApplicationContext(context);
handlerAdapter.afterPropertiesSet();
log.debug("1. 刚开始...");
showBindMethods(handlerAdapter);
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1689692527100-fab99aad-6ce1-441b-92de-eda4010ffe40.png#averageHue=%23c5edcb&clientId=u7184f4fa-8bfe-4&from=paste&height=867&id=ub670251d&originHeight=1300&originWidth=1900&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=254618&status=done&style=none&taskId=u6498f63f-71b7-4ddf-a8e1-f6eda8c2f05&title=&width=1266.6666666666667)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1689692549098-93349aa2-6194-4597-b46f-c1b144824900.png#averageHue=%23e9e9e4&clientId=u7184f4fa-8bfe-4&from=paste&height=77&id=ucebfd6e6&originHeight=116&originWidth=1158&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=82398&status=done&style=none&taskId=u4902fc5b-e371-45b8-aef3-ce85f5ba829&title=&width=772)
## @Controller中@InitBinder 
在执行getDataBinderFactory的时候会初始化InitBinder 
```java
Method getDataBinderFactory = RequestMappingHandlerAdapter.class.getDeclaredMethod("getDataBinderFactory", HandlerMethod.class);
getDataBinderFactory.setAccessible(true);

log.debug("2. 模拟调用 Controller1 的 foo 方法时 ...");
getDataBinderFactory.invoke(handlerAdapter, new HandlerMethod(new WebConfig.Controller1(), WebConfig.Controller1.class.getMethod("foo")));
showBindMethods(handlerAdapter);

log.debug("3. 模拟调用 Controller2 的 bar 方法时 ...");
getDataBinderFactory.invoke(handlerAdapter, new HandlerMethod(new WebConfig.Controller2(), WebConfig.Controller2.class.getMethod("bar")));
showBindMethods(handlerAdapter);

context.close();
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1689693024108-a74f76a1-199e-4f5d-9f9b-ae5b87f0b2fb.png#averageHue=%23edeee5&clientId=u7184f4fa-8bfe-4&from=paste&height=190&id=u13cb3e36&originHeight=285&originWidth=1452&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=268594&status=done&style=none&taskId=ucd496f38-f62d-48cd-b81b-883c040ebb9&title=&width=968)

# ** @InitBinder** 在整个 HandlerAdapter 调用过程中所处的位置
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1689693277094-d471b58b-fd70-4601-bd22-9982996a1322.png#averageHue=%23fbfbfa&clientId=u7184f4fa-8bfe-4&from=paste&height=596&id=u1a944cbe&originHeight=894&originWidth=1457&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=78933&status=done&style=none&taskId=ua3a0ca2f-2deb-439c-82dc-055ffc735f2&title=&width=971.3333333333334)

- RequestMappingHandlerAdapter 在图中缩写为 HandlerAdapter
- HandlerMethodArgumentResolverComposite 在图中缩写为 ArgumentResolvers
- HandlerMethodReturnValueHandlerComposite 在图中缩写为 ReturnValueHandlers
# 收获💡

1. RequestMappingHandlerAdapter 初始化时会解析 [@ControllerAdvice ](/ControllerAdvice ) 中的 [@InitBinder ](/InitBinder ) 方法 
2. RequestMappingHandlerAdapter 会以类为单位，在该类首次使用时，解析此类的 [@InitBinder ](/InitBinder ) 方法 
3. 以上两种 [@InitBinder ](/InitBinder ) 的解析结果都会缓存来避免重复解析 
4. 控制器方法调用时，会综合利用本类的 [@InitBinder ](/InitBinder ) 方法和 [@ControllerAdvice ](/ControllerAdvice ) 中的 [@InitBinder ](/InitBinder ) 方法创建绑定工厂 
