
加在方法上的 @ModelAttribute执行时机 
```java
    @ControllerAdvice
    static class MyControllerAdvice {
        @ModelAttribute("a")
        public String aa() {
            return "aa";
        }
    }

```
```java
    public static void main(String[] args) throws Exception {
        AnnotationConfigApplicationContext context =
                new AnnotationConfigApplicationContext(WebConfig.class);

        RequestMappingHandlerAdapter adapter = new RequestMappingHandlerAdapter();
        adapter.setApplicationContext(context);
        //将方法上的 @ModelAttribute记录下来，给modelFactory使用
        adapter.afterPropertiesSet();

        MockHttpServletRequest request = new MockHttpServletRequest();
        request.setParameter("name", "张三");
        /*
            现在可以通过 ServletInvocableHandlerMethod 把这些整合在一起, 并完成控制器方法的调用, 如下
         */
        ServletInvocableHandlerMethod handlerMethod = new ServletInvocableHandlerMethod(
                new Controller1(), Controller1.class.getMethod("foo", User.class));

        ServletRequestDataBinderFactory factory = new ServletRequestDataBinderFactory(null, null);

        handlerMethod.setDataBinderFactory(factory);
        handlerMethod.setParameterNameDiscoverer(new DefaultParameterNameDiscoverer());
        handlerMethod.setHandlerMethodArgumentResolvers(getArgumentResolvers(context));

        ModelAndViewContainer container = new ModelAndViewContainer();

        // 反射获取模型工厂方法
        Method getModelFactory = RequestMappingHandlerAdapter.class.getDeclaredMethod("getModelFactory", HandlerMethod.class, WebDataBinderFactory.class);
        getModelFactory.setAccessible(true);
        ModelFactory modelFactory = (ModelFactory) getModelFactory.invoke(adapter, handlerMethod, factory);

        // 初始化模型数据，将方法上的 @ModelAttribute加入到模型数据
        modelFactory.initModel(new ServletWebRequest(request), container, handlerMethod);

        handlerMethod.invokeAndHandle(new ServletWebRequest(request), container);

        System.out.println(container.getModel());

        context.close();

        /*
            学到了什么
                a. 控制器方法是如何调用的
                b. 模型数据如何产生
                c. advice 之二, @ModelAttribute 补充模型数据
         */
    }
```
这其实也是一个扩展点，用来补充一些模型数据。配合@ControllerAdvice上对所有controller生效，配合@controller类上对当前类生效


**准备 @ModelAttribute** 在整个 HandlerAdapter 调用过程中所处的位置
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1689775028656-a64835d2-0751-475a-852d-2e8d94ac79dc.png#averageHue=%23ddb67a&clientId=u25a22be6-037f-4&from=paste&height=597&id=u5d8cf3c9&originHeight=896&originWidth=1561&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=78506&status=done&style=none&taskId=uf4cc6ab6-2fff-40c9-8b8b-0bd54bcc9b3&title=&width=1040.6666666666667)
# 收获💡

1. RequestMappingHandlerAdapter 初始化时会解析 @ControllerAdvice 中的 @ModelAttribute 方法
2. RequestMappingHandlerAdapter 会以类为单位，在该类首次使用时，解析此类的 @ModelAttribute 方法
3. 以上两种 @ModelAttribute 的解析结果都会缓存来避免重复解析
4. 控制器方法调用时，会综合利用本类的 @ModelAttribute 方法和 @ControllerAdvice 中的 @ModelAttribute 方法创建模型工厂
