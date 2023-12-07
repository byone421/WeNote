这是一组函数式控制器
```java
@Configuration
public class WebConfig {
    @Bean // ⬅️内嵌 web 容器工厂
    public TomcatServletWebServerFactory servletWebServerFactory() {
        return new TomcatServletWebServerFactory(8080);
    }

    @Bean // ⬅️创建 DispatcherServlet
    public DispatcherServlet dispatcherServlet() {
        return new DispatcherServlet();
    }

    @Bean // ⬅️注册 DispatcherServlet, Spring MVC 的入口
    public DispatcherServletRegistrationBean servletRegistrationBean(DispatcherServlet dispatcherServlet) {
        return new DispatcherServletRegistrationBean(dispatcherServlet, "/");
    }

    @Bean
    public RouterFunctionMapping routerFunctionMapping() {
        return new RouterFunctionMapping();
    }

    @Bean
    public HandlerFunctionAdapter handlerFunctionAdapter() {
        return new HandlerFunctionAdapter();
    }

    @Bean
    public RouterFunction<ServerResponse> r1() {
        return route(GET("/r1"), request -> ok().body("this is r1"));
    }

    @Bean
    public RouterFunction<ServerResponse> r2() {
        return route(GET("/r2"), request -> ok().body("this is r2"));
    }
}
```

流程说明：
初始化的时候会找到所有RouterFunction记录下来（匹配条件和处理函数），记录到RouterFunctionMapping里面。然后根据路径等条件进行匹配。匹配到之后交给HandlerFunctionAdapter处理。处理之后的响应在给HandlerFunctionAdapter返回给浏览器。
# 总结
函数式控制器
    a. RouterFunctionMapping, 通过 RequestPredicate 映射
    b. handler 要实现 HandlerFunction 接口
    c. HandlerFunctionAdapter, 调用 handler

对比
    a. RequestMappingHandlerMapping, 以 @RequestMapping 作为映射路径
    b. 控制器的具体方法会被当作 handler
    c. RequestMappingHandlerAdapter, 调用 handler



 
