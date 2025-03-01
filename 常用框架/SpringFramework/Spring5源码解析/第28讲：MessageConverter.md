# 代码参考
```java
private static void test4() throws IOException, HttpMediaTypeNotAcceptableException, NoSuchMethodException {
    MockHttpServletRequest request = new MockHttpServletRequest();
    MockHttpServletResponse response = new MockHttpServletResponse();
    ServletWebRequest webRequest = new ServletWebRequest(request, response);

    request.addHeader("Accept", "application/xml");
    response.setContentType("application/json");

    RequestResponseBodyMethodProcessor processor = new RequestResponseBodyMethodProcessor(
        List.of(
            new MappingJackson2HttpMessageConverter(), new MappingJackson2XmlHttpMessageConverter()
        ));
    processor.handleReturnValue(
        new User("张三", 18),
        new MethodParameter(A28.class.getMethod("user"), -1),
        new ModelAndViewContainer(),
        webRequest
    );
    System.out.println(new String(response.getContentAsByteArray(), StandardCharsets.UTF_8));
}
```

# 收获💡

1. MessageConverter 的作用 
   - [@ResponseBody ](/ResponseBody ) 是返回值处理器解析的 
   - 但具体转换工作是 MessageConverter 做的
2. 如何选择 MediaType 
   - 首先看 [@RequestMapping ](/RequestMapping ) 上有没有指定 
   - 其次看 request 的 Accept 头有没有指定
   - 最后按 MessageConverter 的顺序, 谁能谁先转换
