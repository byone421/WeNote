# MCP 概述
**Model Context Protocol（MCP）** 是一套标准化协议，用于实现 AI 模型与外部工具或资源的交互。它提供一致的接口，使 AI 模型能够访问数据库、API、文件系统及其他外部服务，同时支持多种传输机制，满足不同运行环境的需求。

## 在没有 MCP 协议之前
+  每个模型调用工具的方式各不相同 
+  接口格式不统一 
+  工具接入成本高 

模型调用外部资源缺乏标准化，接入复杂且不稳定。

## 有了MCP协议之后 
MCP核心就是：把“模型调用工具”这件事标准化了。它让AI可以 让 AI 可以像程序一样，稳定、标准地访问外部资源，比如：

+  查询数据库 
+  调用接口（API） 
+  读写文件 
+  访问企业系统 
+  使用内部服务 

# MCP Java SDK 架构
> 官网地址： [https://modelcontextprotocol.io/docs/sdk](https://modelcontextprotocol.io/docs/sdk)
>

MCP Java SDK 采用三层架构，以实现高内聚、低耦合和可扩展性：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775616732331-36d79f51-9227-4025-809d-23242bb0753e.png)

1. **Client/Server 层（顶层）**  
McpClient：负责客户端协议逻辑，包括连接管理、工具调用和资源访问  
McpServer：负责服务器端协议逻辑，实现工具暴露、请求处理和客户端管理
2. **Session 层（中间层）**  
McpSession：会话管理核心接口  
McpClientSession / McpServerSession：客户端/服务器会话实现  
负责维护连接状态及通信模式，提供同步和异步操作支持
3. **Transport 层（底层）**  
McpTransport：消息传输和 JSON-RPC 序列化  
支持多种传输方式：STDIO、HTTP/SSE、Streamable-HTTP 等  
为上层通信提供基础支持

总的来说就是：上层处理协议逻辑，中层管理会话状态，底层负责消息传输。

# 与大模型的交互流程
### MCP Client 工具注册 / 上报能力
 真正的 MCP Client 通常是“包着大模型的那一层应用/框架”，比如： Cursor，Cline，Claude Desktop等等

+ 触发点：MCP Client 启动或大模型初始化时。 
+ 功能： 
    1. 上报可用工具列表给大模型
        *  例如：数据库查询、文件访问、第三方 API 等。 
    2.  协商每个工具的能力、参数格式、权限范围。 
    3.  让大模型在生成调用指令时“知道自己能调用什么工具”。 
+ 效果：大模型可以在生成策略或回答时选择最合适的工具。

### 大模型发起请求
+  用户输入指令或触发事件。 
+  大模型参考 MCP Client 上报的工具能力，决定： 
    -  使用哪种工具 
    -  调用哪些参数 

### MCP Client 封装请求
+  将大模型的指令转成 MCP 标准消息发给给MCP Server。 
+  做版本与能力校验。 
+  支持同步或异步执行。 

### MCP Server / 工具执行
+  接收 MCP 消息。 
+  校验参数、权限。 
+  执行工具操作。 
+  返回结果或错误信息给MCP Client。 

### MCP Client 返回结果
+  MCP把结果进行格式化、封装结果。 
+  发送回大模型。 

### 大模型生成输出
+  利用工具结果生成自然语言或结构化响应。 
+  返回给用户。

# 与Tool Calling的对比
对于大模型调用外部工具的能力，之前的文章中已经讲过了一个Tool Calling ，这里怎么又来一个MCP。这两者有什么区别吗？这里我们把Tool Calling和MCP做一个对比。

1. Tool Calling 本质上是模型调用外部工具的能力。依赖于模型本身的能力， 如果模型本身没有“执行外部调用”的能力，Tool Calling 就无法独立运作。  
2. MCP 是工具管理和调用的标准协议。  MCP 让模型可以像调用本地函数一样去访问各种工具，解决了： 不同工具接口不统一的问题，MCP 并不赋能模型去调用工具，它提供了安全、统一、可扩展的“通道”，模型使用这个通道去执行工具调用。  

| **维度** | **Tool Calling** | **MCP** |
| --- | --- | --- |
| 层级 | 模型能力 | 系统协议 |
| 作用 | 决定“要不要调” | 定义“怎么调” |
| 是否执行工具 | 不执行 | 定义执行方式 |
| 是否标准化 | 各家不同 | 统一协议 |
| 是否跨进程/远程 | 一般不涉及 | 支持 |


# SpringAI中的MCP
Spring AI 是 MCP Java SDK 的 Spring 封装， 对 MCP 提供了全面支持，通过 Boot Starters 和 Java 注解简化开发。它让开发者在 Spring Boot 环境中能用最少的配置和代码就使用 MCP 协议。

在Spring AI，使开发者可以轻松实现：

+ 实现 MCP 客户端应用：调用 MCP 服务器提供的工具和资源
+ 实现 MCP 服务器应用：将服务或工具暴露给 MCP 客户端

## MCP Server Boot Starters（MCP服务端）  
Spring AI 提供自动配置的 Starter，让你可以快速搭建 MCP 服务器，包括多种协议、传输机制和开发模式  

### 一些核心特性
+  自动配置 MCP Servers 组件（工具/资源/提示/补全等）
+  多协议支持：**STDIO、SSE、Streamable‑HTTP、Stateless**
+  支持同步（sync）与异步（async）模式
+  注解式开发支持（基于 Spring Bean 扫描）

### MCP Server支持同步和异步模式
#### 1. Synchronous Server（同步服务器）
+ **实现类**：`McpSyncServer`
+ **特点**： 
    -  默认类型 
    -  请求-响应模式（blocking） 
    -  简单、直接，适合大多数传统应用 
+ **限制**：只注册 **同步** MCP 方法，异步方法会被忽略 
+ **配置**： 

```plain
spring.ai.mcp.server.type=SYNC
```

#### 2. Asynchronous Server（异步服务器）
+ **实现类**：`McpAsyncServer`
+ **特点**： 
    -  非阻塞、reactive 风格 
    -  内置 **Project Reactor** 支持 
    -  适合高并发、事件驱动或流式应用 
+ **限制**：只注册 **异步** MCP 方法，同步方法会被忽略 
+ **配置**： 

```plain
spring.ai.mcp.server.type=ASYNC
```

| 特性 | 同步 (SYNC) | 异步 (ASYNC) |
| --- | --- | --- |
| 编程模型 | 阻塞、请求-响应 | 非阻塞、Reactive |
| 注册方法 | 同步方法 | 异步方法 |
| 性能场景 | 普通请求处理 | 高并发、流式数据 |
| 内置支持 | 自动同步工具注册 | Project Reactor 异步工具注册 |


### MCP Server的协议类型
| 协议 | 工作方式 | 适用场景 |
| --- | --- | --- |
| **STDIO** | 与宿主进程同一进程，通过 stdin/stdout 通信 | 内嵌到应用中，无需独立服务器 |
| **SSE** | 独立服务器，支持实时推送 | 需要实时消息更新的多客户端场景 |
| **Streamable-HTTP** | 独立服务器，通过 HTTP POST/GET 通信，可选 SSE 流式推送 | HTTP 通信 + 多消息流场景 |
| **Stateless** | 独立服务器，不维护会话状态 | 微服务或云原生架构，简化部署 |


### 结合Cursor演示**STDIO协议类型**
1. 新建一个<font style="color:#080808;background-color:#ffffff;">ai-mcp-server-std模块，添加对应的依赖和配置</font>

```java
<dependencies>
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-starter-mcp-server</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-starter-mcp-server-webmvc</artifactId>
    </dependency>
</dependencies>
```

```java
server:
  port: 8086
spring:
  ai:
    mcp:
      server:
        name: std-mcp-server
        version: 1.0.0
        type: SYNC
        instructions: "这个服务提供了天气查询的功能"
        capabilities:
          tool: true
          resource: true
          prompt: true
          completion: true
        keep-alive-interval: 30s
        stdio: true
```

2. 定义工具并添加配置类

```java
@Component
public class WeatherTool {

    @Tool(description = "根据城市查询天气")
    public String getWeather(String city) {
        return city + " 的天气是：多云 20°C";
    }
}

```

```java
@Configuration
public class McpServerConfig
{
    @Bean
    public ToolCallbackProvider weatherTools(WeatherTool weatherTool)
    {
        return MethodToolCallbackProvider.builder()
                .toolObjects(weatherTool)
                .build();
    }
}
```

3. 将项目打包后放在D:/jar文件夹下面
4. 打开cursor配置我们自己的MCP Server

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775698042607-2b5a7e4d-e845-4477-9416-4905eb2923fb.png)

5. 在mcp.json添加自己的mcp，注意：不要在json文件中写注释。

```java
{
  "mcpServers": {
    "std-mcp-server": {
      "command": "java",
      "args": [
        "-Dspring.ai.mcp.server.stdio=true",
        "-Dspring.main.web-application-type=none",
        "-Dspring.main.banner-mode=off",
        "-jar",
        "D:/jar/ai-mcp-server-std"
      ]
    }
  }
}
```

6. 在cursor中测试

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775697967837-036522c0-0c2e-4edb-9998-14e491382243.png)

### 演示<font style="color:#080808;background-color:#ffffff;">STREAMABLE-HTTP</font>**协议类型**
#### 新建MCP Server
1. 新建一个<font style="color:#080808;background-color:#ffffff;">ai-mcp-server-streamable模块，添加对应的依赖和配置</font>

```java
<dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-starter-mcp-server-webflux</artifactId>
        </dependency>
</dependencies>
```

```java
server:
  port: 8087
spring:
  ai:
    mcp:
      server:
        enabled: true
        protocol: STREAMABLE
        name: streamable-mcp-server
        version: 1.0.0
        type: ASYNC
        instructions: "这个服务提供了天气查询的功能"
        resource-change-notification: true
        tool-change-notification: true
        prompt-change-notification: true
        capabilities:
          tool: true
          resource: true
          prompt: true
          completion: true
        streamable-http:
          mcp-endpoint: /mcp
          keep-alive-interval: 30s

```

2. 定义工具并添加配置类

```java
@Component
public class WeatherTool {

    @Tool(description = "根据城市查询天气")
    public String getWeather(String city) {
        return city + " 的天气是：多云 20°C";
    }
}

```

```java
@Configuration
public class McpServerConfig
{
    @Bean
    public ToolCallbackProvider weatherTools(WeatherTool weatherTool)
    {
        return MethodToolCallbackProvider.builder()
                .toolObjects(weatherTool)
                .build();
    }
}
```

3. 启动项目

#### 结合Cursor使用
1. 继续在cursor中添加MCP Server，由于没有认证，Authorization就这样。

```java
{
  "mcpServers": {
    "std-mcp-server": {
      "command": "java",
      "args": [
        "-Dspring.ai.mcp.server.stdio=true",
        "-Dspring.main.web-application-type=none",
        "-Dspring.main.banner-mode=off",
        "-jar",
        "D:/jar/ai-mcp-server-std.jar"
      ]
    },
    "streamable-mcp-server": {
      "type": "streamable-http",
      "url": "http://localhost:8087/mcp",
      "headers": {
        "Authorization": "Bearer your-token"
      }
    }
  }
}
```

2. 启用我们的mcp-server

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775718085640-74c04f40-dac7-4116-860d-6b66c0b764d5.png)

3. 测试通过

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775718194063-67d8ed3f-3440-414c-ba96-18b1e92e3c70.png)

#### 结合MCP Client使用
下方讲解MCP Client时再配合使用。

### <font style="color:rgb(20, 24, 24);">Stateless Streamable-HTTP</font>协议类型
和上述演示类似，可在项目代码 **<font style="color:#080808;background-color:#ffffff;">ai-mcp-server-stateless</font>**<font style="color:#080808;background-color:#ffffff;"> 模块</font>中查看



## MCP Client Boot Starter（MCP 客户端启动器）  
### 功能支持
MCP Client Boot Starter 提供了：

+  管理多个 MCP 客户端实例 
+  自动初始化客户端（可配置开启/关闭） 
+  支持多种传输方式：STDIO、HTTP/SSE、Streamable-HTTP、Stateless Streamable-HTTP 
+  与 Spring AI 的工具执行框架集成 
+  工具过滤与名称前缀定制 
+  生命周期管理（Context 关闭时自动清理资源） 
+  支持同步（SYNC）和异步（ASYNC）客户端 
+  支持注解方式的事件处理（如进度、日志、采样、工具变更通知等）

### 可用的starter
```java
<!-- 标准客户端 二选一 -->
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client</artifactId>
</dependency>

<!-- 基于 WebFlux 的客户端（二选一） -->
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-mcp-client-webflux</artifactId>
</dependency>
```

### 新建Client配合<font style="color:#080808;background-color:#ffffff;">STREAMABLE-HTTP服务</font>
1. 新建一个<font style="color:#080808;background-color:#ffffff;">ai-mcp-client模块</font>

```java
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-starter-mcp-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webflux</artifactId>
        </dependency>
        <!--   使用openai模型  -->
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-starter-model-openai</artifactId>
        </dependency>
    </dependencies>
```

```java
server:
  port: 8090

spring:
  main:
    allow-bean-definition-overriding: true
  ai:
    openai:
      api-key: ${qwen}
      base-url: https://dashscope.aliyuncs.com/compatible-mode
      chat:
        options:
          model: qwen3.5-plus
          temperature: 0.7
    mcp:
      client:
        enabled: true
        name: spring-ai-mcp-client
        version: 1.0.0
        type: ASYNC
        streamable-http:
          connections:
            server1:
              url: http://localhost:8087
              endpoint: /mcp
        toolcallback:
          enabled: true


```

2. 添加配置代码和Controller

```java
@Configuration
public class ChatClientConfig {
    @Bean("openAiModel")
    public ChatModel openAiModel(OpenAiChatModel openAiChatModel) {
        return openAiChatModel;
    }
    @Bean
    public ChatClient openAiChatClient(@Qualifier("openAiModel") ChatModel chatModel, ToolCallbackProvider tools) {
        return ChatClient.builder(chatModel)
                //通过了mcp协议获取了远程工具，在配置文件中有相关的配置
                .defaultToolCallbacks(tools.getToolCallbacks())
                .build();
    }
}
```

```java

@RestController
public class ChatController {

    @Autowired
    ChatClient openAiChatClient;
    
    @GetMapping("/chat")
    public Flux<String> chat(String msg) {

        return openAiChatClient.prompt()
                .user(msg)
                .stream()
                .content();
    }
}
```

3. 先启动<font style="color:#080808;background-color:#ffffff;">ai-mcp-server-streamable服务模块，再启动ai-mcp-client客户端模块</font>
4. <font style="color:#080808;background-color:#ffffff;">调用接口进行测试</font>

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775719966423-c5cf6be0-06b4-4b25-bce7-752829efc476.png)



### 调用第三方的MCP Server
这里一高德地图的mcp为例，需要提前去高德开发平台获取apikey

> [https://lbs.amap.com/](https://lbs.amap.com/)
>
> [https://mcp.so/server/amap-maps/amap?tab=tools](https://mcp.so/server/amap-maps/amap?tab=tools)
>

1. 新建一个mcp-servers.json

```java
{
  "mcpServers": {
    "amap-maps": {
      "command": "npx",
      "args": [
        "-y",
        "@amap/amap-maps-mcp-server"
      ],
      "env": {
        "AMAP_MAPS_API_KEY": "自己的apikey"
      }
    }
  }
}
```

2. 修改配置增加servers-configuration

```java
spring:
  ai:
    mcp:
      client:
        enabled: true
        name: spring-ai-mcp-client
        version: 1.0.0
        type: ASYNC
        streamable-http:
          connections:
            streamable-mcp-server:
              url: http://localhost:8087
              endpoint: /mcp
        toolcallback:
          enabled: true
        annotation-scanner:
          enabled: true
        stdio:
          servers-configuration: classpath:/mcp-servers.json
```

3. 新增一个接口进行测试

```java
 @GetMapping("/mcp/chat")
 public Flux<String> chatMcp(String msg) {
     return openAiChatClient.prompt()
             .user(msg)
             .stream()
             .content();
 }
```

4. 调用接口测试通过，这样我们就把高德的mcp功能集成进来了。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775793609993-fa159b54-1f4d-4593-a9d8-b1a9372a6b45.png)

# MCP Server注解
注解模块通常由 Starter 自动包含，所以只要引入相关 Boot Starters，就能直接使用。以下是一些常用的注解，可以让你以声明式方式定义 Server 的行为：

```java
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-mcp-annotations</artifactId>
</dependency>
```

+ **@McpTool**：暴露一个可调用的函数（如 API、脚本），让 AI 模型在需要时主动执行特定操作 
+ **@McpResource**：定义对资源（如文件、URL、数据库等）的访问接口 ，提供只读的数据内容，供 AI 读取并作为上下文参考
+ **@McpPrompt**：定义可复用的提示词模板，用于标准化与 AI 的交互流程（如预设角色、步骤、示例）
+ **@McpComplete**：实现自动补全功能（如参数建议）  

## @McpTool
```java
/**
     * @Mcptool是MCP协议专用的注解。@Tool比它更高级，也可以被识别成为MCP的工具
     * 由于是异步模式，返回的是Mono类型
     * @param op
     * @param a
     * @param b
     * @return
     */
    @McpTool(name = "calculator", description = "简单的数学计算器")
    public Mono<Double> calculate(
            @McpToolParam(description = "运算符: add, sub, mul, div") String op,
            @McpToolParam(description = "第一个操作数") double a,
            @McpToolParam(description = "第二个操作数") double b) {

        double result = switch (op) {
            case "add" -> a + b;
            case "sub" -> a - b;
            case "mul" -> a * b;
            case "div" -> a / b;
            default -> throw new IllegalArgumentException("Unsupported operator");
        };

        return Mono.just(result);
    }
```

## @McpPrompt
```java

    /**
     * 让AI生成工作日报，如果AI没有主动调用。可以明确使用daily_report工具。
     */
    @McpPrompt(name = "daily_report", description = "生成工作日报")
    public Mono<Prompt> dailyReportPrompt(
            @McpArg(name = "content", description = "今日工作内容") String content,
            @McpArg(name = "role", description = "你的角色") String role) {

        // 方法2：手动构建 Prompt（更灵活）
        SystemMessage systemMsg = new SystemMessage(String.format("""
            你是一个%s，请根据用户的工作内容生成一份专业的工作日报。
            日报需要包含：工作摘要、完成情况、遇到的问题、明日计划。
            使用 markdown 格式，语气专业正式。
            """, role));

        UserMessage userMsg = new UserMessage("今日工作内容：" + content);
        return Mono.just(new Prompt(systemMsg, userMsg));
    }
```

## @McpResource
```java
   @McpResource(uri = "resource://docs/help")
    public Mono<String> getHelpDoc() {
        return Mono.just("""
                # 系统帮助文档
                - 使用 `getWeather` 工具查询天气
                - 使用 `calculator` 工具进行计算
                """);
    }
```

## 在Cline中查看
由于cursor不方便查看。这里使用cline，其他软件类似

1. 添加MCP Servers
2. <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775726580569-853aa21d-7b04-4a68-9db7-86a0d38a344a.png)
3. <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775728973650-43a109a2-96f3-4ad8-90d4-0d31f3f3f441.png)
4. 进行测试

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775737946879-ea6f9c94-143f-41f9-b722-be5496cefb3e.png)

5. 测试prompt

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1775738381658-4433506f-06b2-4a97-b39c-0948caae9837.png)

# 结语
至此，我们已经基于 Spring AI 完整实现了 MCP Server 与 MCP Client 的搭建与调用流程，从工具定义到客户端调用，再到协议交互，整体链路已经打通。相关示例代码已整理并开源，地址如下：  
[https://github.com/byone421/springai-demo](https://github.com/byone421/springai-demo)  
希望本文能帮助你快速理解 MCP 的使用方式，并在实际项目中落地应用。  







###  


# 
