在上一篇文章中，我们已经对 Spring AI 的基础 API 使用方式进行了介绍，包括基本的模型调用和简单的交互示例。本篇文章将继续深入 Spring AI，重点介绍在实际开发中非常重要的高阶 API，帮助你更好地理解 Spring AI 的扩展能力和高级用法。

# ChatMemory
由于大语言模型（LLM）本身是 **无状态（stateless）** 的，这意味着它不会自动记住之前的对话内容。如果每次请求都只发送当前问题，模型是无法理解上下文的，这在连续对话的场景下带来了一定限制。为了解决这个问题，Spring AI 提供了 Chat Memory 机制，该机制允许开发者在多次交互之间 存储和检索对话信息，从而让模型能够感知上下文，让大模型知道之前聊过什么东西。

Spring AI 中通过抽象 **ChatMemory** 来管理对话记忆。开发者可以基于该抽象实现不同类型的记忆策略，以满足不同的业务需求。而 ChatMemory 的实现类，则负责决定：

+ 哪些消息需要被保留
+ 什么时候删除旧消息

消息的实际存储由 **ChatMemoryRepository** 负责：它主要负责存储和读取消息。

## Chat Memory 和 Chat History对比
在继续之前，我们需要理解两个概念。

### Chat Memory（对话记忆）
指模型在当前对话过程中 用于维持上下文理解的一部分信息。通常只保留有限的消息，以保证上下文的连贯性，同时避免 Token 过多。

### Chat History（对话历史）
指 完整的对话记录，包含用户和模型之间所有交换的消息。 在实际系统中，通常会将 Chat History 持久化存储，而不是仅保存在内存中。  

### 总结
ChatMemory的设计目标是**管理模型需要的上下文信息**，而不是存储完整的聊天记录。

换句话说就是：

+ ChatMemory : 用于维持上下文
+ ChatHistory : 用于完整数据存档

## 开始使用
Spring AI 默认已经配置好了一个 **ChatMemory Bean**，我们开发者可以在应用中直接使用。

默认情况下：

+ 使用 **InMemoryChatMemoryRepository** 作为消息存储仓库（基于内存）
+ 使用 **MessageWindowChatMemory** 作为对话记忆的管理策略

在代码中可以直接注入即可使用：

```plain
@Autowired
ChatMemory chatMemory;
```

接下来我们将进一步介绍 Spring AI 提供的不同 Memory 类型以及存储方式。

## Memory Types
### 记忆管理策略
 ChatMemory是一个抽象接口，它允许开发者实现不同类型的 **对话记忆策略** 来满足不同的业务场景。

 LLM应用设计中常见的记忆管理策略有：

+ 保留最近 N 条消息
+ 保留某一时间范围内的消息
+ 根据 Token 数量限制保留消息

不同的 Memory 类型会直接影响：

+ 对话上下文的长度
+ Token 消耗
+ 系统性能

但在Spring AI中，目前的实现只有<font style="color:#080808;background-color:#ffffff;">MessageWindowChatMemory。如果有需求可以自己实现ChatMemory接口。</font>

### MessageWindowChatMemory
MessageWindowChatMemory 是 Spring AI 提供的一个窗口式记忆实现。

它会维护一个 **固定大小的消息窗口**：

+ 当消息数量 未超过限制 时，全部保留
+ 当消息数量 超过最大值 时，最旧的消息会被删除

默认情况下：

+ 最大消息数量：20 条

示例代码如下：

```java
@Configuration
public class ChatMemoryConfig {
    @Bean
    public ChatMemory chatMemory(){
        return MessageWindowChatMemory.builder()
        .maxMessages(10)
        .build();
    }
}
```

## Memory Storage
Spring AI 提供了 ChatMemoryRepository 接口，用于持久化存储对话。

ChatMemoryRepository的职责非常简单：

+ 存储消息
+ 查询消息
+ 删除消息

默认是 InMemoryChatMemoryRepository 基于内存存储， 需要注意的是，由于数据存储在内存中，应用重启后聊天记录会丢失。  

### JdbcChatMemoryRepository
JdbcChatMemoryRepository 是 Spring AI 提供的 基于 JDBC 的持久化实现，可以将聊天记忆存储到关系型数据库中。

#### 1.添加依赖和配置
首先在项目中引入依赖：

```xml
<dependency>
  <groupId>org.springframework.ai</groupId>
  <artifactId>spring-ai-starter-model-chat-memory-repository-jdbc</artifactId>
</dependency>
<!-- Spring Boot JDBC -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<!-- MySQL 驱动 -->
<dependency>
  <groupId>com.mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
  <scope>runtime</scope>
</dependency>
```

修改配置文件

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/数据库名?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
```

#### 2.建表
在**spring-ai-starter-model-chat-memory-repository-jdbc**包下面复制建表语句应用到你使用的数据库中。这里使用的是mysql，复制的是scheme-mysql.sql中的语句。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1773112491343-e0b3d20a-2f97-4cc1-b040-75867f55a32b.png)

#### 3.使用JdbcChatMemoryRepository 
Spring AI 为 JdbcChatMemoryRepository 提供了自动配置，可以直接注入使用：

```java
@Autowired
JdbcChatMemoryRepository chatMemoryRepository;
```

然后将其与 ChatMemory 结合使用：

```java
ChatMemory chatMemory = MessageWindowChatMemory.builder()
    .chatMemoryRepository(chatMemoryRepository)
    .maxMessages(10)
    .build();
```

#### 4.手动创建Repository
如果希望手动创建 JdbcChatMemoryRepository，需要提供两个参数：

+ `JdbcTemplate`
+ `JdbcChatMemoryRepositoryDialect`

示例代码：

```java
ChatMemoryRepository chatMemoryRepository = JdbcChatMemoryRepository.builder()
    .jdbcTemplate(jdbcTemplate)
    .dialect(new PostgresChatMemoryRepositoryDialect())
    .build();

ChatMemory chatMemory = MessageWindowChatMemory.builder()
    .chatMemoryRepository(chatMemoryRepository)
    .maxMessages(10)
    .build();
```

#### 5.更多的数据库
Spring AI 通过 **Dialect 抽象** 来适配不同数据库，目前支持以下数据库：

```java
//使用dialect来指定具体使用的数据库
.dialect(new XXXChatMemoryRepositoryDialect())
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1773050302991-ea27a93b-2932-479c-b6c3-be61945acaf7.png)

#### 6.测试代码
```java
@Resource
private ChatMemory chatMemory;
@Resource
ChatClient openAiChatClient;

@RestController
@RequestMapping("/chat")
public class ChatTestController {

    @Resource
    private ChatMemory chatMemory;
    @Resource
    ChatClient openAiChatClient;

    @GetMapping("/memory")
    public String chatExample(String userMessage) {
        String conversationId = "test-id";

        // 1. 保存用户消息
        chatMemory.add(conversationId, List.of(
                new UserMessage(userMessage)
        ));
        // 2. 获取当前上下文（保留最近消息）
        List<Message> context = chatMemory.get(conversationId);

        // 3. 调用 ChatClient，让模型回答
        String content = openAiChatClient.prompt()
                .messages(context)
                .call().content();

        // 4. 保存模型回答
        chatMemory.add(conversationId, List.of(new AssistantMessage(content)));

        // 5. 输出
        System.out.println("AI: " + content);
        return content;
    }
}
```

### <font style="color:rgb(20, 24, 24);">MongoChatMemoryRepository</font>
对于聊天数据其实更适合使用非关系型数据库存储，下面我们就使用MongoDB来保存数据。

#### 1.添加依赖和配置
首先在项目中引入依赖：

```java
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-chat-memory-repository-mongodb</artifactId>
</dependency>
<!-- Spring Boot MongoDB starter -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

修改配置文件

```java
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/goods?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
  data:
    mongodb:
      uri: mongodb://localhost:27017/mydb
```

修改配置类

```java
@Bean
public ChatMemoryRepository mongoChatMemoryRepository(MongoTemplate mongoTemplate) {
    return MongoChatMemoryRepository.builder()
            .mongoTemplate(mongoTemplate)
            .build();
}
@Bean
public ChatMemory mongoChatMemory(ChatMemoryRepository mongoChatMemoryRepository) {
    return MessageWindowChatMemory.builder()
            .chatMemoryRepository(mongoChatMemoryRepository)
            .maxMessages(20)
            .build();
}

```

#### 2.测试代码
与mysql的测试代码相同，只是切换了一下chatMemory

```java
@Resource
private ChatMemory mongoChatMemory;

@GetMapping("/memory/mongo")
public String memoryMongo( String userMessage) {
    String conversationId = "test-id-mongo";
    // 1. 保存用户消息
    mongoChatMemory.add(conversationId, List.of(
            new UserMessage(userMessage)
    ));
    // 2. 获取当前上下文（保留最近消息）
    List<Message> context = mongoChatMemory.get(conversationId);
    // 3. 调用 ChatClient，让模型回答
    String content = openAiChatClient.prompt()
            .messages(context)
            .call().content();
    // 4. 保存模型回答
    mongoChatMemory.add(conversationId, List.of(new AssistantMessage(content)));
    // 5. 输出
    System.out.println("AI: " + content);
    return content;
}
```

### 其他的<font style="color:rgb(20, 24, 24);">ChatMemoryRepository</font>
除了关系型数据库和mongodb。springai还适配了一些其他的数据库。这里只做一个了解

#### <font style="color:rgb(20, 24, 24);">CassandraChatMemoryRepository</font>
需要添加以下依赖

```java
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-chat-memory-repository-cassandra</artifactId>
</dependency>
```

#### <font style="color:rgb(20, 24, 24);">Neo4j ChatMemoryRepository</font>
需要添加以下依赖

```java
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-chat-memory-repository-neo4j</artifactId>
</dependency>
```

#### <font style="color:rgb(20, 24, 24);">CosmosDBChatMemoryRepository</font>
需要添加以下依赖

```java
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-chat-memory-repository-cosmos-db</artifactId>
</dependency>
```

## Memory Advisors
 Spring AI 主要是通过不同的 Memory Advisors 来结合ChatMemory和<font style="color:#080808;background-color:#ffffff;">ChatClient，从而</font>管理聊天的上下文，其中：

+ **ChatMemory**：负责存储和管理对话上下文
+ **ChatClient**：实际调用大语言模型（LLM）接口的客户端。
+ **Memory Advisor**：用于连接ChatMemory 与 ChatClient的桥梁，自动将历史消息加入到请求中。

### MessageChatMemoryAdvisor
使用MessageChatMemoryAdvisor时，Advisor 会自动从chatMemory中取出历史消息，同时也会把模型回答的消息自动存回chatMemory中。

**首先对ChatClient添加advisors**

```java
    @Bean
    public ChatClient openAiChatClient(@Qualifier("openAiModel") ChatModel model,@Qualifier("mongoChatMemory") ChatMemory mongoChatMemory) {
        return ChatClient.builder(model)
                 //添加MessageChatMemoryAdvisor
                .defaultAdvisors(MessageChatMemoryAdvisor.builder(mongoChatMemory).build())
                .build();
    }
```

**在Controller中测试**

```java
@Resource
ChatClient openAiChatClient;
@GetMapping("/message")
public String memoryMongo(String userMessage) {
    //指定是哪个对话id
    String conversationId = "009" ;
    // 1. 保存用户消息
    String response = openAiChatClient.prompt()
            .user(userMessage)
            //给advisors设置参数
            .advisors(a ->{
                a.param(ChatMemory.CONVERSATION_ID, conversationId);
            })
            .call()
            .content();
    return response;
}
```

## PromptChatMemoryAdvisor
PromptChatMemoryAdvisor 的做法是将历史对话 **以文本形式拼接到 System Prompt 中**，从而让模型在生成回答时能够参考之前的对话内容。

在自定义模板字符串时，必须包含以下两个占位符：

+ **${instructions}**：会被原始的System Prompt（系统提示词）替换
+ **${memory}**：会被历史对话记录替换

在请求发送给模型之前，Spring AI 会将占位符替换为真实内容，从而生成最终的 Prompt。

**以下是一段案例代码**

```java
 @Resource
 ChatMemory mongoChatMemory;
 @GetMapping("/prompt")
 public String memoryMongo(String userMessage) {
     
     PromptTemplate template = PromptTemplate.builder()
             .template("""
                 系统指令:
                 {instructions}
                 
                 历史对话:
                 {memory}
                 
                 """)
             .build();
     String chatClient = openAiChatClient.prompt()
             .advisors(
                     PromptChatMemoryAdvisor.builder(mongoChatMemory)
                             .systemPromptTemplate(template)
                             .conversationId("009")
                             .build()
             )
             .user(userMessage)
             .call()
             .content();
     return chatClient;
 }
```

## VectorStoreChatMemoryAdvisor
与 PromptChatMemoryAdvisor 类似，VectorStoreChatMemoryAdvisor 也需要使用 **模板占位符** 来将历史消息注入系统提示词中，但不同的是，它会将数据 **保存到向量存储（VectorStore）**，并通过语义检索找回最相关的历史消息。

+ **占位符要求**：
    - **${instructions}**：系统消息（System Prompt）
    - **${long_term_memory}**：从向量存储检索到的历史消息

下面演示的是将数据直接保存在内存中，而不是使用具体的向量数据库。

注意：使用向量模型进行语义检索时，需要你的API Key 有拥有访问对应模型的权限，否则无法生成向量。

### 1.添加依赖和配置
```yaml
spring:
  ai:
    openai:
      api-key: ${qwen}
      base-url: https://dashscope.aliyuncs.com/compatible-mode
      chat:
        options:
          model: qwen3.5-plus
          temperature: 0.7
      embedding:
        options:
          model: text-embedding-v3 #这里可以切换模型名称，需要你的apikey有权限使用对应的模型
```

添加依赖

```java
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-vector-store</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-advisors-vector-store</artifactId>
</dependency>
```

### 2.注入VectorStore
```java

 @Bean
 public VectorStore vectorStore(EmbeddingModel openAiEmbeddingModel) {
     return SimpleVectorStore.builder(openAiEmbeddingModel).build();
 }
```

### 3.测试使用
```java
    @GetMapping("/vector")
    public String vectorStoreChatMemory(String userMessage) {
        PromptTemplate template = PromptTemplate.builder()
                .template("""
                    系统指令:
                    {instructions}
                    
                    历史对话:
                    {long_term_memory}
                   
                    """)
                .build();

        String chatClient = openAiChatClient.prompt()
                .advisors(
                        VectorStoreChatMemoryAdvisor.builder(vectorStore)
                                //取出20条
                                .defaultTopK(20)
                                .conversationId("123")
                                .systemPromptTemplate(template)
                                .build()
                )
                .user(userMessage)
                .call()
                .content();
        return chatClient;
    }
```

## 三者对比
+ **MessageChatMemoryAdvisor**：将历史消息作为 Message 列表（User / Assistant） 发送给模型，还原完整的多轮对话结构，适合标准聊天场景。
+ **PromptChatMemoryAdvisor**：将历史消息 转换为文本并拼接到 Prompt 中，通过 Prompt的方式让模型感知上下文，适合自定义 Prompt 或优化模型指令的场景。
+ **VectorStoreChatMemoryAdvisor**：将历史消息 保存到向量存储（VectorStore），在每次调用时通过 语义检索 找回最相关的历史消息，并拼接到系统提示词中，实现 长期记忆 或 RAG（检索增强生成）场景。

| Advisor | 记忆方式 | 适合场景 |
| --- | --- | --- |
| MessageChatMemoryAdvisor | 最近 N 条消息 | 普通多轮对话 |
| PromptChatMemoryAdvisor | 文本拼接历史消息 | Prompt 控制 |
| VectorStoreChatMemoryAdvisor | 向量检索历史对话 | 长期记忆 / RAG |


**整体依赖关系**

```java
Controller
 │
 ▼
ChatClient
 │
 ▼
MemoryAdvisor
 │
 ▼
ChatMemory
 │
 ▼
ChatMemoryRepository 
 │
 ▼
查询内存或者数据库
```

# 相关示例代码
[https://github.com/byone421/springai-demo/tree/main/ai-chat-memory](https://github.com/byone421/springai-demo/tree/main/ai-chat-memory)

# 最后
通过本文，我们熟悉了Spring AI中ChatMemory相关的API， 希望我的内容对大家有帮助。同时文章也会在公众号同步更新，如果想及时收到更新通知，也欢迎大家关注：【说是爪哇】







