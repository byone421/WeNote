当浏览器发送一个请求 `http://localhost:8080/hello` 后，请求到达服务器，其处理流程是：

1.  服务器提供了 DispatcherServlet，它使用的是标准 Servlet 技术 
   - 路径：默认映射路径为 `/`，即会匹配到所有请求 URL，可作为请求的统一入口，也被称之为**前控制器** 
      - jsp 不会匹配到 DispatcherServlet
      - 其它有路径的 Servlet 匹配优先级也高于 DispatcherServlet
   - 创建：在 Boot 中，由 DispatcherServletAutoConfiguration 这个自动配置类提供 DispatcherServlet 的 bean
   - 初始化：DispatcherServlet 初始化时会优先到容器里寻找各种组件，作为它的成员变量 
      - HandlerMapping，初始化时记录映射关系
      - HandlerAdapter，初始化时准备参数解析器、返回值处理器、消息转换器
      - HandlerExceptionResolver，初始化时准备参数解析器、返回值处理器、消息转换器
      - ViewResolver
2.  DispatcherServlet 会利用 RequestMappingHandlerMapping 查找控制器方法 
   -  例如根据 /hello 路径找到 @RequestMapping("/hello") 对应的控制器方法 
   -  控制器方法会被封装为 HandlerMethod 对象，并结合匹配到的拦截器一起返回给 DispatcherServlet 
   -  HandlerMethod 和拦截器合在一起称为 HandlerExecutionChain（调用链）对象 
3.  DispatcherServlet 接下来会： 
   1. 调用拦截器的 preHandle 方法
   2. RequestMappingHandlerAdapter 调用 handle 方法，准备数据绑定工厂、模型工厂、ModelAndViewContainer、将 HandlerMethod 完善为 ServletInvocableHandlerMethod 
      - [@ControllerAdvice ](/ControllerAdvice ) 全局增强点1️⃣：补充模型数据 
      - [@ControllerAdvice ](/ControllerAdvice ) 全局增强点2️⃣：补充自定义类型转换器 
      - 使用 HandlerMethodArgumentResolver 准备参数 
         - [@ControllerAdvice ](/ControllerAdvice ) 全局增强点3️⃣：RequestBody 增强 
      - 调用 ServletInvocableHandlerMethod
      - 使用 HandlerMethodReturnValueHandler 处理返回值 
         - [@ControllerAdvice ](/ControllerAdvice ) 全局增强点4️⃣：ResponseBody 增强 
      - 根据 ModelAndViewContainer 获取 ModelAndView 
         - 如果返回的 ModelAndView 为 null，不走第 4 步视图解析及渲染流程 
            - 例如，有的返回值处理器调用了 HttpMessageConverter 来将结果转换为 JSON，这时 ModelAndView 就为 null
         - 如果返回的 ModelAndView 不为 null，会在第 4 步走视图解析及渲染流程
   3. 调用拦截器的 postHandle 方法
   4. 处理异常或视图渲染 
      - 如果 1~3 出现异常，走 ExceptionHandlerExceptionResolver 处理异常流程 
         - [@ControllerAdvice ](/ControllerAdvice ) 全局增强点5️⃣：[@ExceptionHandler ](/ExceptionHandler ) 异常处理 
      - 正常，走视图解析及渲染流程
   5. 调用拦截器的 afterCompletion 方法
