# 静态资源处理
主要负责静态资源处理
## 关键代码
```java
@Bean
public SimpleUrlHandlerMapping simpleUrlHandlerMapping(ApplicationContext context) {
    SimpleUrlHandlerMapping handlerMapping = new SimpleUrlHandlerMapping();
    Map<String, ResourceHttpRequestHandler> map 
        = context.getBeansOfType(ResourceHttpRequestHandler.class);
    handlerMapping.setUrlMap(map);
    return handlerMapping;
}

@Bean
public HttpRequestHandlerAdapter httpRequestHandlerAdapter() {
    return new HttpRequestHandlerAdapter();
}

@Bean("/**")
public ResourceHttpRequestHandler handler1() {
    ResourceHttpRequestHandler handler = new ResourceHttpRequestHandler();
    handler.setLocations(List.of(new ClassPathResource("static/")));
    return handler;
}

@Bean("/img/**")
public ResourceHttpRequestHandler handler2() {
    ResourceHttpRequestHandler handler = new ResourceHttpRequestHandler();
    handler.setLocations(List.of(new ClassPathResource("images/")));
    return handler;
}
```
## 小总结

1. SimpleUrlHandlerMapping 不会在初始化时收集映射信息，需要手动收集
2. SimpleUrlHandlerMapping 映射路径
3. ResourceHttpRequestHandler 作为静态资源 handler
4. HttpRequestHandlerAdapter, 调用此 handler
## 静态资源解析优化
```java
@Bean("/**")
public ResourceHttpRequestHandler handler1() {
    ResourceHttpRequestHandler handler = new ResourceHttpRequestHandler();
    handler.setLocations(List.of(new ClassPathResource("static/")));
    handler.setResourceResolvers(List.of(
        	// ⬇️缓存优化
            new CachingResourceResolver(new ConcurrentMapCache("cache1")),
        	// ⬇️压缩优化
            new EncodedResourceResolver(),
        	// ⬇️原始资源解析
            new PathResourceResolver()
    ));
    return handler;
}
```
# 欢迎页处理
## 关键代码
```java
@Bean
    public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext context) {
        Resource resource = context.getResource("classpath:static/index.html");
return new WelcomePageHandlerMapping(null, context, resource, "/**");
}

@Bean
    public SimpleControllerHandlerAdapter simpleControllerHandlerAdapter() {
    return new SimpleControllerHandlerAdapter();
}
```

 
## 收获💡

1. 欢迎页支持静态欢迎页与动态欢迎页
2. WelcomePageHandlerMapping 映射欢迎页（即只映射 '/'） 
   - 它内置的 handler ParameterizableViewController 作用是不执行逻辑，仅根据视图名找视图
   - 视图名固定为 forward:index.html
3. SimpleControllerHandlerAdapter, 调用 handler 
   - 转发至 /index.html
   - 处理 /index.html 又会走上面的静态资源处理流程

# 映射器与适配器小结

1. HandlerMapping 负责建立请求与控制器之间的映射关系 
   - RequestMappingHandlerMapping (与 [@RequestMapping ](/RequestMapping ) 匹配) 
   - WelcomePageHandlerMapping    (/)
   - BeanNameUrlHandlerMapping    (与 bean 的名字匹配 以 / 开头)
   - RouterFunctionMapping        (函数式 RequestPredicate, HandlerFunction)
   - SimpleUrlHandlerMapping      (静态资源 通配符 /** /img/**)
   - 之间也会有顺序问题, boot 中默认顺序如上
2. HandlerAdapter 负责实现对各种各样的 handler 的适配调用 
   - RequestMappingHandlerAdapter 处理：[@RequestMapping ](/RequestMapping ) 方法  
      - 参数解析器、返回值处理器体现了组合模式
   - SimpleControllerHandlerAdapter 处理：Controller 接口
   - HandlerFunctionAdapter 处理：HandlerFunction 函数式接口
   - HttpRequestHandlerAdapter 处理：HttpRequestHandler 接口 (静态资源处理)
   - 这也是典型适配器模式体现
