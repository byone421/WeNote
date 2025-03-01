# Tomcat异常处理
```java

@Configuration
public class WebConfig {
    @Bean
    public TomcatServletWebServerFactory servletWebServerFactory() {
        return new TomcatServletWebServerFactory();
    }

    @Bean
    public DispatcherServlet dispatcherServlet() {
        return new DispatcherServlet();
    }

    @Bean
    public DispatcherServletRegistrationBean servletRegistrationBean(DispatcherServlet dispatcherServlet) {
        DispatcherServletRegistrationBean registrationBean = new DispatcherServletRegistrationBean(dispatcherServlet, "/");
        registrationBean.setLoadOnStartup(1);
        return registrationBean;
    }

    @Bean // @RequestMapping
    public RequestMappingHandlerMapping requestMappingHandlerMapping() {
        return new RequestMappingHandlerMapping();
    }

    @Bean // 注意默认的 RequestMappingHandlerAdapter 不会带 jackson 转换器
    public RequestMappingHandlerAdapter requestMappingHandlerAdapter() {
        RequestMappingHandlerAdapter handlerAdapter = new RequestMappingHandlerAdapter();
        handlerAdapter.setMessageConverters(List.of(new MappingJackson2HttpMessageConverter()));
        return handlerAdapter;
    }

    //tomcat 添加处理界面
    @Bean // 修改了 Tomcat 服务器默认错误地址
    public ErrorPageRegistrar errorPageRegistrar() { // 出现错误，会使用请求转发 forward 跳转到 error 地址
        return webServerFactory -> webServerFactory.addErrorPages(new ErrorPage("/error"));
    }
 
    @Bean
    public ErrorPageRegistrarBeanPostProcessor errorPageRegistrarBeanPostProcessor() {
        return new ErrorPageRegistrarBeanPostProcessor();
    }

    @Controller
    public static class MyController {
        @RequestMapping("test")
        public ModelAndView test() {
            int i = 1 / 0;
            return null;
        }

        /*@RequestMapping("/error")
        @ResponseBody
        public Map<String, Object> error(HttpServletRequest request) {
            Throwable e = (Throwable) request.getAttribute(RequestDispatcher.ERROR_EXCEPTION);
            return Map.of("error", e.getMessage());
        }*/
    }

    @Bean
    public BasicErrorController basicErrorController() {
        ErrorProperties errorProperties = new ErrorProperties();
        errorProperties.setIncludeException(true);
        return new BasicErrorController(new DefaultErrorAttributes(), errorProperties);
    }

    @Bean
    public View error() {
        return new View() {
            @Override
            public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
                System.out.println(model);
                response.setContentType("text/html;charset=utf-8");
                response.getWriter().print("""
                        <h3>服务器内部错误</h3>
                        """);
            }
        };
    }

    @Bean
    public ViewResolver viewResolver() {
        return new BeanNameViewResolver();
    }
}
```
```java
public class A32 {

    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        AnnotationConfigServletWebServerApplicationContext context =
                new AnnotationConfigServletWebServerApplicationContext(WebConfig.class);
        RequestMappingHandlerMapping handlerMapping = context.getBean(RequestMappingHandlerMapping.class);
        handlerMapping.getHandlerMethods().forEach((RequestMappingInfo k, HandlerMethod v) -> {
            System.out.println("映射路径:" + k + "\t方法信息:" + v);
        });
    }
}

```
# SpringBoot中BasicErrorController
这个类也是配合Tomcat来做错误界面处理
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690102860183-2e2104a6-5453-4324-8ea0-4a016bc920e7.png#averageHue=%23f1f0e5&clientId=u4c5012f3-c0d7-4&from=paste&height=269&id=u3a66bddc&originHeight=404&originWidth=1075&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=239538&status=done&style=none&taskId=ub7e655ca-1163-4aa8-a5f4-b45d21f79ab&title=&width=716.6666666666666)
进行配置。
```java
   @Bean
    public BasicErrorController basicErrorController() {
        ErrorProperties errorProperties = new ErrorProperties();
        errorProperties.setIncludeException(true);
        return new BasicErrorController(new DefaultErrorAttributes(), errorProperties);
    }
```
## 用postman测试返回
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690104130459-dabd2f52-15c0-409a-a4da-147734ea58c4.png#averageHue=%23f8f8f8&clientId=u4c5012f3-c0d7-4&from=paste&height=565&id=u3aba5fb7&originHeight=848&originWidth=1543&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=230366&status=done&style=none&taskId=ud46c06e7-33f4-4383-ab46-c30eef8bf42&title=&width=1028.6666666666667)
这些错误信息都是由DefaultErrorAttributes来提供的，如果想返回错误的信息需要做如下设置
```java
    @Bean
    public BasicErrorController basicErrorController() {
        ErrorProperties errorProperties = new ErrorProperties();
        errorProperties.setIncludeException(true);
        return new BasicErrorController(new DefaultErrorAttributes(), errorProperties);
    }
```
## 浏览器测试
浏览器默认返回的html
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690111539373-d0202c38-f21d-4706-a48a-45ba376d95a7.png#averageHue=%23ebe9e0&clientId=u4c5012f3-c0d7-4&from=paste&height=437&id=ue05c5344&originHeight=655&originWidth=1267&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=500979&status=done&style=none&taskId=uff1c57de-5839-45c0-a77f-3270ff98d28&title=&width=844.6666666666666)
如果需要自定义界面需要提供View对象和ViewResolver对象。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690111241358-2635e80f-d5b1-4d76-a24e-acb3be71b82c.png#averageHue=%23eddcc8&clientId=u4c5012f3-c0d7-4&from=paste&height=503&id=u3c126599&originHeight=754&originWidth=1410&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=718716&status=done&style=none&taskId=uec1bdd3c-a445-4251-aed7-91393b74c23&title=&width=940)
