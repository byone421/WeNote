平时在使用各种AI聊天软件时候，都有一个输入框输入文字与AI进行交互。而在Spring AI 中，ChatClient 是我们与大模型交互的核心入口。 它封装了 Prompt 构造、模型调用、参数配置等复杂细节，让我们可以用一种极度优雅、链式调用的方式与 AI 交互，同时它支持同步和流式编程模式。下面我们就先从这家伙开始聊起吧。

# ChatClient
## 什么是ChatClient
ChatClient是 Spring AI 提供的高级抽象 API，用于构建和发送对话请求。它底层会调用具体的模型实现（如 OpenAI、DashScope、Ollama 等），但对开发者来说是透明的。

## 最基础的使用方式
 在 Spring Boot 中，我们可以直接通过构造器注入：  

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/my")
public class MyController {

    private final ChatClient chatClient;

    /**
     * 构造函数注入 ChatClient.Builder，Spring 会自动提供一个 ChatClient.Builder 的实例。
     * @param chatClientBuilder
     */
    public MyController(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }

    /**
     * 这个方法处理 GET 请求，路径为 /my/hello。它接受一个名为 userInput 的请求参数，如果没有提供该参数，则默认值为 "你好"。
     * @param userInput
     * @return
     */
    @GetMapping("/hello")
    public String hello(@RequestParam(value = "userInput", defaultValue = "你好") String userInput) {
        return this.chatClient.prompt()//开始创建一个对话提示
                .user(userInput)//设置用户输入
                .call()//执行对话提示，调用 AI 模型生成回答
                .content();// 从 AI 模型的响应中提取出纯文本内容（即生成的回答），并作为方法的返回值。
    }
}

```

## <font style="color:rgb(51, 51, 51);">角色预设</font>
### 针对全局设置
在构建ChatClient时，我们可以进行一些全局默认配置。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1772271572663-eb7258d3-7997-4d07-a470-14a17bfd7360.png)

 这里讲一下 **defaultSystem**和 **defaultUser** 的作用：

1. **defaultSystem()** 用来设置一个默认的 System Prompt，在每一次 prompt() 调用时都会自动带上，经常用来给模型设定一个“永久人设”，比如：
+ 你是一名诗人
+ 你只能用中文回答

用来确定系统的行为边界。

2. **defaultUser()**设置的是一个默认的用户消息

### 针对单次请求设置
也可以在在发送请求之前，对角色进行预设。使用system对系统人设进行设置，使用user设置用户消息

```java
@GetMapping("/hello")
public String hello(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
    return this.chatClient
    .prompt()
    .system("你是一个诗人，你喜欢作诗来回答用户的问题，你的名字叫李白。")
    .user("来一首望坤门山")
    .call()
    .content();
}
```

### 两者对比
| 方法 | 作用范围 | 使用场景 |
| --- | --- | --- |
| defaultSystem | 全局默认 system | 设定角色 |
| defaultUser | 全局默认 user | 提供默认提问 |
| system() | 单次请求 | 临时改变规则 |
| user() | 单次请求 | 实际提问 |


# ChatModel
ChatModel是 Spring AI 的核心接口之一， 它是ChatClient的底层接口，大概关系是这样子：

```java
ChatClient ->  ChatModel  ——>  具体模型实现（OpenAI / DashScope / Ollama）
```

ChatClient它是一个对话构建器，ChatModel才是真正调用大模型的接口。 Spring AI 已经帮我们实现好了不同厂商的模型适配器<font style="color:#080808;background-color:#ffffff;">：</font>

1. <font style="color:#080808;background-color:#ffffff;">OpenAiChatModel</font>
2. <font style="color:#080808;background-color:#ffffff;">OllamaChatModel</font>
3. <font style="color:#080808;background-color:#ffffff;">DeepSeekChatModel</font>
4. <font style="color:#080808;background-color:#ffffff;">等等.....</font>

具体的请参考官网：[https://docs.spring.io/spring-ai/reference/api/chat/comparison.html](https://docs.spring.io/spring-ai/reference/api/chat/comparison.html)

## 用ChatModel构建一个对话请求
ChatModel相对于ChatClient更为底层，所以在使用层面上就稍微更麻烦一些。所以更推荐先ChatClient。

```java
@Resource
ChatModel chatModel;

@GetMapping("/chatModel")
public String chatModelHello(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
     //手动创建一个 Prompt 对象，用Prompt来封装message
     Prompt prompt = new Prompt(
             List.of(
                     new SystemMessage("你是唐代诗人李白，只能用古诗风格回答问题。"),
                     new UserMessage(userInput)
             )
     );
     //同步调用 ChatModel 的 call 方法，传入创建的 Prompt 对象，获取 ChatResponse 对象。
     ChatResponse response = chatModel.call(prompt);
     return response.getResult()
             .getOutput()
             .getText();
 }
```

## 多ChatModel/ChatClient共存
在实际开发中，接入多个Model也是必要的，因为我们可以根据不同的场景和需求。来选择所需要使用的模型。

| 场景 | 解决方案 |
| --- | --- |
| 成本优化 | 普通问题走便宜模型 |
| 复杂推理 | 指定走高端模型 |
| 降级策略 | 主模型异常自动切换 |
| 本地优先 | 先走 Ollama |


### 配置文件
一下配置文件演示了同时配置了OpenAiChatModel和<font style="color:#080808;background-color:#ffffff;">OllamaChatModel</font>的模型参数，在项目中支持多种模型

```yaml
server:
  port: 8003
spring:
  ai:
    openai:
      api-key: ${qwen}
      base-url: https://dashscope.aliyuncs.com/compatible-mode
      chat:
        options:
          model: qwen3.5-plus
          temperature: 0.7
    ollama:
      base-url: http://localhost:11434
      chat:
        model: deepseek-r1:1.5b
  main:
    allow-bean-definition-overriding: true
```

### 配置类
新建一个ChatConfig配置类

```java
@Configuration
public class ChatConfig {

    @Bean("openAiModel")
    public ChatModel openAiModel(OpenAiChatModel openAiChatModel) {
        return openAiChatModel;
    }


    @Bean("ollamaChatModel")
    public ChatModel ollamaChatModel(OllamaChatModel ollamaChatModel) {
        return ollamaChatModel;
    }

    @Bean
    public ChatClient openAiChatClient(
            @Qualifier("openAiModel") ChatModel model) {
        return ChatClient.builder(model)
                .defaultSystem("你是一名诗人，你的名字叫李白，你只能用古诗风格回答问题")
                .build();
    }

    @Bean
    public ChatClient ollamaChatClient(
            @Qualifier("ollamaChatModel") ChatModel model) {
        return ChatClient.builder(model)
                .defaultSystem("你是一个编程专家，你的名字叫代码小助手，你只能用专业的编程术语回答问题")
                .build();
    }
}

```

### 使用
```java
@Resource
ChatClient openAiChatClient;
@Resource
ChatClient ollamaChatClient;


@GetMapping("/multipleChatModel")
public String multipleChatModelHello(@RequestParam(value = "userInput", defaultValue = "") String userInput,
                                    @RequestParam(value = "model", defaultValue = "") String model) {
    if("openAi".equals(model)){
        return this.openAiChatClient
                .prompt()
                .user(userInput)
                .call()
                .content();
    }else {
        return this.ollamaChatClient
                .prompt()
                .user(userInput)
                .call()
                .content();
    }
}

```

# <font style="color:rgb(20, 24, 24);">Prompt</font>
在 AI 应用开发中，我们编写的核心内容其实并不是传统的业务逻辑代码，而是 **Prompt（提示词）**。  
一个设计良好的 Prompt 可以帮助大模型更准确地理解我们的意图，从而返回更加可靠、更加符合预期的结果。

简单来说，**Prompt 就是发送给大模型的一段指令和上下文信息**，用于告诉模型“你是谁”“用户在问什么”“之前发生过什么”。

一个完整的 Prompt 通常由三部分组成：

+ **system（系统角色）**：用于设定模型的角色和行为规则，例如人设、回答风格或能力边界。
+ **user（用户输入）**：用户当前提出的问题或需求。
+ **assistant（历史回答）**：模型之前的回答内容，用于构建对话上下文，使模型能够理解整个对话过程。

因此，在向大模型发送请求之前，我们通常需要先 **构建一个 Prompt 对象**，将这些信息组织起来，然后再提交给模型进行推理和生成结果。

Spring AI 为我们提供了相关的API，可以非常方便地构建和发送 Prompt

## 使用 ChatModel 构建 Prompt
使用ChatModel API来构建Prompt是一种更底层的写法。

### 配置注入ChatModel
```java
@Bean("openAiModel")
public ChatModel openAiModel(OpenAiChatModel openAiChatModel) {
    return openAiChatModel;
}
```

###  手动构建Prompt
```java
@Resource
ChatModel openAiModel;

@GetMapping("/chatModel")
public String chatModelHello(@RequestParam(value = "userInput", defaultValue = "") String userInput) {

    //手动创建一个 Prompt 对象
    Prompt prompt = new Prompt(
        List.of(
            new SystemMessage("你是唐代诗人李白，只能用古诗风格回答问题。"),
            new UserMessage(userInput)
        )
    );
    //同步调用 ChatModel 的 call 方法，传入创建的 Prompt 对象，获取 ChatResponse 对象。
    ChatResponse response = openAiModel.call(prompt);

    return response.getResult()
        .getOutput()
        .getText();
}
```

## 使用 ChatClient 构建 Prompt
相比ChatModel API 用ChatClient API就更为便捷

### 配置注入ChatClient
```java
@Bean("openAiModel")
public ChatModel openAiModel(OpenAiChatModel openAiChatModel) {
    return openAiChatModel;
}

@Bean
public ChatClient openAiChatClient(
        @Qualifier("openAiModel") ChatModel model) {
    return ChatClient.builder(model)
            .defaultSystem("你是一名诗人，你的名字叫李白，你只能用古诗风格回答问题")
            .build();
}
```

### 使用 prompt() 构建
```java
@Resource
ChatClient openAiChatClient;

@GetMapping("/hello")
    public String hello(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
        return this.openAiChatClient
                .prompt()
                .user(userInput)
                .call()
                .content();
    }
```

## Prompt 参数化
在实际开发中，很多提示词并不是固定不变的，而是需要根据用户输入或业务数据动态拼接。Spring AI 提供了模板变量机制，开发者可以在 Prompt 中定义占位符，然后在调用时传入具体参数进行替换，从而生成最终的 Prompt。  

### 对默认配置进行参数化
在构建ChatClient时，定义变量占位

```java
@Bean
public ChatClient chatClient(ChatClient.Builder chatClientBuilder) {
    return chatClientBuilder
            .defaultSystem("你是一个诗人，你的名字叫{name}")
            .defaultUser("来一首咏坤")
            .build();
}

```

在发送请求前进行设置

```java
@GetMapping("/param")
public String param(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
        return this.chatClient
                .prompt()
                .system(sp -> sp.param("name", "李白"))
                .user(userInput)
                .call()
                .content();
}
```

### 对每次请求进行参数化
```java
    @GetMapping("/param2")
    public String param2Hello(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
        return this.openAiChatClient
                .prompt()
                .system(sp -> sp.param("name", "李白"))
                .user(u -> u.text("""
                        请写一首关于{type}的诗，要求用古诗风格，并且在诗中提到
                      """)
                        .param("type", userInput))
                .user(userInput)
                .call()
                .content();
    }
```

### 使用 Map 批量传参 
当参数太多，我可以使用map封装参数，一次性传递。

```java
@GetMapping("/mapParam")
public String mapParam(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
    Map<String, Object> params = Map.of(
            "name", "李白",
            "role", "架构师"
    );
    return  this.openAiChatClient.prompt()
            .system(sp -> sp
                    .text("你是{name}，{role}")
                    .params(params)
            )
            .user(userInput)
            .call()
            .content();
}
```

## 结构化输出
Prompt 可以规定模型按格式返回：  

```java
@GetMapping("/structured")
public String structured(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
    return this.openAiChatClient
            .prompt()
            .system("你是一个诗人")
            .user(u -> u.text("""
                            请根据以下参数写一首诗，要求用古诗风格。
                            诗的格式如下：
                            题目：{title}
                            作者：{author}
                            主题：{topic}
                            """).param("title", "春日随想")
                    .param("author", "李白")
                    .param("topic", "描写春天的景色与情感"))
            .call()
            .content();
}
```

# 流式响应
在前面的示例中，我们一直使用 `.call().content()` 来一次性获取 AI 返回的完整内容。这种方式会等模型生成完全部结果后再返回。如果希望实现类似“打字机效果”的实时输出，就可以使用 **流式响应**。在 Spring AI 中，只需要将 `.call()` 替换为 `.stream()`，并通过 `.content()` 持续接收模型逐步生成的内容。这样应用可以在模型生成文本的同时实时返回给前端，从而提升用户体验，特别适用于聊天、问答等需要实时反馈的场景。  

## <font style="color:#080808;background-color:#ffffff;">使用SseEmitter实现</font>
```java
    /**
     * 
     * 使用 SseEmitter 实现 Server-Sent Events (SSE) 流式响应的示例 
     * @param userInput
     * @return
     */
    @GetMapping(value = "/sse", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public SseEmitter sseStream(String userInput) {
        SseEmitter emitter = new SseEmitter();

        this.ollamaChatClient
                .prompt()
                .system("你是一个诗人")
                .user(userInput)
                .stream()
                .content()
                .subscribe(
                        data -> {
                            try {
                                emitter.send(SseEmitter.event().data(data));
                            } catch (IOException e) {
                                emitter.completeWithError(e);
                            }
                        },
                        emitter::completeWithError,
                        emitter::complete
                );

        return emitter;
    }
```

## 使用Flux实现
```java
/**
 * 使用 Reactor 的 Flux 实现响应式流式响应的示例
 * @param userInput
 * @return
 */
@GetMapping(value = "/flux", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public Flux<String> fluxStream(String userInput) {
    return this.openAiChatClient
            .prompt()
            .system("你是一个诗人")
            .user(userInput)
            .stream()
            .content();
}
```

## call和stream的对比
| **普通调用** | **流式调用** |
| --- | --- |
| .call() | .stream() |
| 同步返回 | 逐块返回 |
| 适合接口调用 | 适合聊天UI |
| 简单 | 体验更好 |
| 底层使用 ChatModel   | 底层使用<font style="color:#080808;background-color:#ffffff;"> StreamingChatModel  </font> |


# <font style="color:rgb(20, 24, 24);">结构化输出(Structured Output)</font>
在日常开发中，如果让大模型直接返回一段文本，我们往往还需要自己去解析字符串、提取字段，既不稳定也不优雅。而 Structured Output 的核心思想，就是让大模型按照我们定义的数据结构输出结果，并自动映射为 Java 对象，让 AI 输出真正变成“可直接使用的数据”，而不是一段难以控制的文本。  

## 一个基础的例子
### 定义返回结构  
```java
public record Poem(
        String title,
        String author,
        String content
) {}
```

### 调用AI
```java
@GetMapping(value = "/outputEntity")
public Poem outputEntityHello(String userInput) {
    return this.ollamaChatClient
    .prompt()
    .system("你是一个诗人")
    .user(userInput)
    .call()
    .entity(Poem.class);
}
```

成功返回内容

```java
{
    "title": "春天的足迹",
    "author": "王明",
    "content": "春天的脚步总是踩在青石板上，在溪水中轻轻摇曳。溪水的清冽中带着不屈的坚持，向着远方的天空飞去，为春天带来了无限的活力与希望。"
}
```

## SpringAI怎么做
Spring AI为什么能成功对返回内容进行成功的映射，难道就不会出现意外，返回的内容和实体压根不搭边？

Spring AI为了能成功映射主要做了这些事情

1.  **生成 JSON Schema**

把实体

```java
public record Poem(
        String title,
        String author,
        String content
) {}
```

转成JSON Schema  

```java

{
  "type": "object",
  "properties": {
    "title": { "type": "string" },
    "author": { "type": "string" },
    "content": { "type": "string" }
  },
  "required": ["title", "author", "content"]
}
```

2.  **把 Schema 发给模型**

 模型在生成每个 token 时，会被限制只能生成合法 JSON。 这会限制模型生成的内容。

3.  **模型输出合法 JSON **

```java
{
  "title": "春日",
  "author": "李白",
  "content": "春风吹绿柳..."
}
```

4. **Spring AI 内部反序列化**

```java
objectMapper.readValue(json, Poem.class);
```

## 输出为Map
```java
@GetMapping(value = "/outputMap")
public Map<String, Object> outputMapHello() {
    return this.ollamaChatClient
            .prompt()
            .system("你是一个诗人")
            .user("""
                          生成一名诗人的信息，要求输出一个包含以下字段的JSON对象:
                          - name
                          - age
                          - dynasty
                          JSON的格式如下:
                          {
                              "name": "李白",
                              "age": 42,
                              "dynasty": "唐代"
                          }
                    """)
            .call()
            .entity(new ParameterizedTypeReference<Map<String, Object>>() {
            });
}
```

## 输出为List
```java
@GetMapping(value = "/outputList")
public List<Poem> outputListHello() {
    var converter = new BeanOutputConverter<>(
            new ParameterizedTypeReference<List<Poem>>() {
            }
    );    
    return ollamaChatClient.prompt()
            .system("你是一个诗人")
            .user("""
                    推荐3个诗人的作品，返回作品名和作者和作品内容:
                    请返回一个 JSON 数组。
                    格式示例：
                    [
                      {
                        "title": "诗名",
                        "author": "作者"
                        "content": "诗的内容"
                      }
                    ]
                    不要返回对象，不要嵌套。
                    """)
            .call()
            .entity(converter);
}
```

## **原生结构化输出（Native Structured Output）**
### 什么是**原生结构化输出**
 现代大模型已经**原生支持 JSON Schema 结构化输出**。  

与传统“在 prompt 里拼 JSON 格式说明”不同：

+ 传统方式：在 Prompt 里拼格式要求
+ Native 方式：直接把 JSON Schema 发给模型的结构化输出 API

原生结构化输出让模型按 Schema 强制输出结构化 JSON，而不是靠提示词约束。

### 两者对比
| 方式 | 原理 | 可靠性 | Prompt整洁度 |
| --- | --- | --- | --- |
| Prompt拼格式 | 把 schema 写进 prompt | 不稳定 | 很乱： Prompt混杂了很多东西 |
| Native Structured Output | JSON Schema 走模型原生API | 模型保证结构正确 | 干净： Prompt 专注于业务语义   |


### 开启 Native Structured Output  
#### 全局开启
在构建ChatClient时候设置defaultAdvisors

```java
@Bean
ChatClient chatClient(ChatClient.Builder builder) {
    return builder
        .defaultAdvisors(AdvisorParams.ENABLE_NATIVE_STRUCTURED_OUTPUT)
        .build();
}
```

#### 单次开启
在发送请求前设置advisors

```java
chatClient.prompt()
                .system("你是一个诗人")
                .advisors(AdvisorParams.ENABLE_NATIVE_STRUCTURED_OUTPUT)
```

#### 示例代码：输出为List
```java
@GetMapping(value = "/outputListNative")
public List<Poem> outputListNativeHello() {
    var converter = new BeanOutputConverter<>(
            new ParameterizedTypeReference<List<Poem>>() {
            }
    );
    return ollamaChatClient.prompt()
            .system("你是一个诗人")
            .user("""
            推荐3个诗人的作品，返回作品名和作者和作品内容:
            """)
            .call()
            .entity(converter);
}
```

返回的内容

```java
[
    {
        "title": "莫言诗集",
        "author": "莫言",
        "content": "春水"
    },
    {
        "title": "白居易全集",
        "author": "白居易",
        "content": "诗画"
    },
    {
        "title": "鲁迅全集",
        "author": "鲁迅",
        "content": "文心事"
    }
]
```

## 消息元数据（MessageMeta）
### 什么是 Message Metadata
ChatClient支持给 **UserMessage** 和 **SystemMessage** 添加元数据（metadata）

它的作用是给消息附加额外的上下文信息不影响文本内容，但可用于下游处理。

### 给消息添加 Metadata
#### 使用metadata添加元数据
```java
@GetMapping(value = "/metadata")
public String metadata() {
    return ollamaChatClient.prompt().user(u -> u.text("你好")
                    .metadata("messageId", "xxxxx")
                    .metadata("userId", "1"))
            .call()
            .content();
}
```

####  利用map一次性添加
```java
@GetMapping(value = "/map")
public String metadataMap() {
    Map<String, Object> userMetadata = Map.of(
            "userId", "xxxx",
            "timestamp", System.currentTimeMillis()
    );
    return ollamaChatClient.prompt()
            .user(u -> u.text("你好")
                    .metadata(userMetadata))
            .call()
            .content();
}
```

####  配置默认Metadata  
```java
@Bean
ChatClient chatClient(ChatClient.Builder builder) {
    return builder
        .defaultSystem(s -> s.text("你是一名诗人")
            .metadata("assistantType", "DeepSeek")
            .metadata("version", "1.0"))
        .defaultUser(u -> u.text("来一首咏坤")
            .metadata("userId", "xxxxx"))
        .build();
}
```

### Metadata 校验规则
对于设置的metadata，Spring AI 内部会做严格校验：

| 规则 | 说明 |
| --- | --- |
| key 不能为空 | null 或 "" 会报错 |
| value 不能为 null | 必须有值 |
| Map 不能包含 null key/value | 否则 IllegalArgumentException |


错误示例：

```plain
.metadata(null, "value")  // key 为空
.metadata("key", null)    // value 为空
```

### 如何读取 Metadata
我们往Message里面已经设置好了Metadata，至于如何使用我们将在Advisor小结说明。

### 和 Prompt 参数的区别  
| 功能 | 作用对象 |
| --- | --- |
| text() | 发送给模型 |
| metadata() | 附加给消息对象 |
| param() | 替换 Prompt 模板变量 |


# ChatResponse
## 什么是 ChatResponse？  
一次模型调用的完整响应对象,  它不是“纯文本结果”，而是一个包含所有上下文信息的响应封装。

## ChatResponse包含的内容
不同springai版本可能结构不一样，当前使用是1.1.2

```java
ChatResponse
 ├── Result Results中的第一个结果
 ├── Results
 │     ├── Output (模型返回内容)
 │     ├── Metadata (模型返回元数据)
 ├── Metadata (响应级元数据)
```

### 获取文本内容
利用ChatResponse获取返回的文本内容

```java
@GetMapping("/hello")
public void  hello() {
    ChatResponse response = openAiChatClient.prompt()
            .user("用写诗的方式讲个笑话")
            .call()
            .chatResponse();
    for (Generation result : response.getResults()) {
        System.out.println("---------result.getOutput-----------");
        System.out.println("result.getOutput().getText():" + result.getOutput().getText());
        System.out.println("result.getOutput().getMetadata():" + result.getOutput().getMetadata());
        System.out.println("---------result.getOutput-----------");
    }
}
```

返回结果

```java
---------result.getOutput-----------
result.getOutput().getText():《戏作捞月》
醉卧青松月下眠，
欲捞倒影入酒船。
扑通跌入清波里，
惊起蛙声笑谪仙。
result.getOutput().getMetadata():{role=ASSISTANT, messageType=ASSISTANT, finishReason=STOP, refusal=, index=0, annotations=[{}], id=chatcmpl-f81fb384-abfe-9fcd-b0d8-f6245c03747e}
---------result.getOutput-----------
```

###  获取 Token 使用情况  
```java
ChatResponse response = openAiChatClient.prompt()
                .user("用写诗的方式讲个笑话")
                .call()
                .chatResponse();

ChatResponseMetadata metadata = response.getMetadata();
String model = metadata.getModel();
System.out.println("model:" + model);
Usage usage = metadata.getUsage();
System.out.println("Prompt Tokens: " + usage.getPromptTokens());
System.out.println("Completion Tokens: " + usage.getCompletionTokens());
System.out.println("Total Tokens: " + usage.getTotalTokens());
```

```java
model:qwen3.5-plus
Prompt Tokens: 35
Completion Tokens: 4456
Total Tokens: 4491
```

# <font style="color:rgb(20, 24, 24);">Advisors</font>
## 什么是 Advisor
 它是Spring AI 的拦截器（AI 调用生命周期切面），能在AI调用过程中进行拦截做一些操作。

## Advisor执行流程
1. 包含Prompt的请求
2. 执行Advisor链****
3. 调用大模型Model
4. Advisor后置处理
5. 返回ChatResponse

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1772617032435-67b2af09-2597-4671-99ca-b9d886d0b59b.png)

##  一个简单的例子
### 自定义一个Advisor
```java

/**
 *  CallAdvisor: 实现CallAdvisor拦截同步call调用
 *  StreamAdvisor: 实现StreamAdvisor拦截流式调用
 *  根据需求实现对应的接口，两个接口都实现则两个场景都能拦截
 */
public class SimpleLoggerAdvisor implements CallAdvisor,StreamAdvisor {

    @Override
    public String getName() {
        return "SimpleLoggerAdvisor";
    }

    /**
     * 控制Advisor的执行顺序，数值越小优先级越高，默认值为0
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }

    /**
     * @param request
     * @param chain
     * @return
     */
    @Override
    public ChatClientResponse adviseCall(ChatClientRequest request, CallAdvisorChain chain) {

        System.out.println("===== SimpleLoggerAdvisor CALL BEFORE =====");
        System.out.println("SimpleLoggerAdvisor User Prompt: " + request.prompt());

        long start = System.currentTimeMillis();

        // 调用下一个 Advisor
        ChatClientResponse response = chain.nextCall(request);

        long cost = System.currentTimeMillis() - start;

        System.out.println("=====SimpleLoggerAdvisor CALL AFTER =====");
        System.out.println("SimpleLoggerAdvisor Response: " + response.chatResponse().getResult().getOutput().getText());
        System.out.println("耗时: " + cost + " ms");


        return response;
    }

    /**
     * @param request
     * @param chain
     * @return
     */
    @Override
    public Flux<ChatClientResponse> adviseStream(ChatClientRequest request, StreamAdvisorChain chain) {
        System.out.println("===== STREAM BEFORE =====");
        System.out.println("User Prompt: " + request.prompt());

        long start = System.currentTimeMillis();

        return chain.nextStream(request)
                .doOnNext(chunk -> {
                    System.out.println("流式片段: " +
                            chunk.chatResponse().getResult().getOutput().getText());
                })
                .doOnComplete(() -> {
                    long cost = System.currentTimeMillis() - start;
                    System.out.println("===== STREAM COMPLETE =====");
                    System.out.println("耗时: " + cost + " ms");
                });
    }
}
```

### 对单个请求设置
定义一个接口

```java
@GetMapping("/simple")
public String simple(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
      return this.openAiChatClient
              .prompt()
              .system(sp -> sp.param("name", "李白"))
              .user(userInput)
              .advisors(new SimpleLoggerAdvisor())
              .call()
              .content();
  }
```

SimpleLoggerAdvisor打印

```java
===== SimpleLoggerAdvisor CALL BEFORE =====
SimpleLoggerAdvisor User Prompt: Prompt{messages=[SystemMessage{textContent='你是一名诗人，你的名字叫李白，你只能用古诗风格回答问题', messageType=SYSTEM, metadata={messageType=SYSTEM}}, UserMessage{content='你好', metadata={messageType=USER}, messageType=USER}], modelOptions=OpenAiChatOptions: {"streamUsage":false,"model":"qwen3.5-plus","temperature":0.7}}
=====SimpleLoggerAdvisor CALL AFTER =====
SimpleLoggerAdvisor Response: 青天明月照相逢，
客自何方入此中？
且置尘劳酌美酒，
与君一笑醉春风。
耗时: 59675 ms
```

###  全局设置
配置ChatClient的defaultAdvisors

```java
@Bean
public ChatClient openAiChatClient(
        @Qualifier("openAiModel") ChatModel model) {
    return ChatClient.builder(model)
            .defaultSystem("你是一名诗人，你的名字叫李白，你只能用古诗风格回答问题")
            .defaultAdvisors(new SimpleLoggerAdvisor())
            .build();
}
```

## 多个advisor执行顺序
如果存在多个advisor调用顺序如下：

```java
advisor1 before
    advisor2 before
        advisor3 before
            调用模型
        advisor3 after
    advisor2 after
advisor1 after
```

## 获取MetaData
定义一个接口并传入MetaData

```java
@GetMapping("/metadata")
public String metadata(@RequestParam(value = "userInput", defaultValue = "") String userInput) {
    return this.openAiChatClient
            .prompt()
            .system(sp -> {
                        sp.metadata("version", "1.0");
                        sp.param("name", "李白");
                    }
            )
            .user((up) -> {
                up.text(userInput);
                up.metadata("userID", "123");
            })
            .advisors(new MetaDataAdvisor())
            .call()
            .content();
}
```

定义一个MetaDataAdvisor获取元数据，并做一些操作

```java
public class MetaDataAdvisor implements CallAdvisor {

    @Override
    public String getName() {
        return "MetaDataAdvisor";
    }

    /**
     * 控制Advisor的执行顺序，数值越小优先级越高，默认值为0
     *
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }

    /**
     * @param request
     * @param chain
     * @return
     */
    @Override
    public ChatClientResponse adviseCall(ChatClientRequest request, CallAdvisorChain chain) {

        System.out.println("===== UserAdvisor CALL BEFORE =====");
        Map<String, Object> userMap = request.prompt().getUserMessage().getMetadata();
        userMap.forEach((s, o) -> System.out.println("UserAdvisor 拦截的user-metadata: " + s + " - " + o));
        Map<String, Object> systemMap = request.prompt().getSystemMessage().getMetadata();
        systemMap.forEach((s, o) -> System.out.println("UserAdvisor 拦截的system-metadata: " + s + " - " + o));

        // 调用下一个 Advisor
        ChatClientResponse response = chain.nextCall(request);
        System.out.println("=====UserAdvisor CALL AFTER =====");
        System.out.println("UserAdvisor Response: " + response.chatResponse().getResult().getOutput().getText());
        return response;
    }

}
```

# <font style="color:rgb(20, 24, 24);">相关示例代码</font>
> [https://github.com/byone421/springai-demo/tree/main/ai-chat](https://github.com/byone421/springai-demo/tree/main/ai-chat)
>

# 最后
通过本文，我们熟悉了Spring AI中一些基本的API。但由于Spring AI 目前仍处于快速发展阶段，不同版本之间在部分功能使用方式上可能会有所调整。但整体的 API 设计已经相对稳定，核心对象和使用模式通常不会发生较大的变化。在实际使用时，可以结合当前版本的官方文档进行参考。 







