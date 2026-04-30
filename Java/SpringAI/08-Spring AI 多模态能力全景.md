随着大模型能力的不断增强，AI 已经不再局限于“文本对话”，而是进入了**多模态时代**：可以理解文本、生成图片、识别语音、视频生成等等。AIGC做到了用AI来生成**万事万物**（夸张了一点^_^）。  而 Spring AI 正在把这些能力，变成后端开发者可以直接使用的“基础组件”。  

#  一、Spring AI 多模态能力介绍  
在使用多模态能力之前，首先要明确一个前提：并不是所有模型都支持所有能力。不同模型在输入输出形式上各有侧重，本质上对应的是不同类型的 AI 处理能力。

可以将常见能力拆解为如下几类：

| 能力 | 输入 | 输出 | 本质 |
| --- | --- | --- | --- |
| Embedding | 文本 | 向量 | 语义理解 |
| Image | 文本 | 图片 | 视觉生成 |
| Transcription | 音频 | 文本 | 语音识别 |
| Speech | 文本 | 音频 | 语音合成 |


## 引入spring-ai-alibaba
由于我这里使用的是阿里云百炼平台的API来演示，而且<font style="color:#080808;background-color:#ffffff;">spring-ai-alibaba对spring-ai做了深度集成，也复用了spring-ai的功能。如果熟悉了spring-ai。切换到spring-ai-alibaba也是水到渠成的事情。</font>所以这次我们直接引入阿里的<font style="color:#080808;background-color:#ffffff;">spring-ai-alibaba相关依赖来演示。对于版本兼容关系可以查看spring-ai-alibaba官网：</font>[https://java2ai.com/docs/versions](https://java2ai.com/docs/versions)

1. 添加spring-ai-alibaba-bom

```xml
<properties>
            <maven.compiler.source>17</maven.compiler.source>
            <maven.compiler.target>17</maven.compiler.target>
            <spring-ai.version>1.1.2</spring-ai.version>
            <spring-ai-alibaba-extensions.version>1.1.2.2</spring-ai-alibaba-extensions.version>
            <spring-ai-alibaba-bom.version>1.1.2.0</spring-ai-alibaba-bom.version>
</properties>

<dependencyManagement>
    <dependencies>
          <dependency>
                <groupId>org.springframework.ai</groupId>
                <artifactId>spring-ai-bom</artifactId>
                <version>${spring-ai.version}</version>
                <type>pom</type>
                <scope>import</scope>
          </dependency>
          <dependency>
                <groupId>com.alibaba.cloud.ai</groupId>
                <artifactId>spring-ai-alibaba-bom</artifactId>
                <version>${spring-ai-alibaba-bom.version}</version>
                <type>pom</type>
                <scope>import</scope>
          </dependency>
          <dependency>
                <groupId>com.alibaba.cloud.ai</groupId>
                <artifactId>spring-ai-alibaba-extensions-bom</artifactId>
                <version>${spring-ai-alibaba-extensions.version}</version>
                <type>pom</type>
                <scope>import</scope>
          </dependency>
    </dependencies>
</dependencyManagement>
```

2. 在项目pom.xml中添加相关starter

```java
<dependency>
    <groupId>com.alibaba.cloud.ai</groupId>
    <artifactId>spring-ai-alibaba-starter-dashscope</artifactId>
</dependency>
```

# 二、Embedding（文本向量模型） 
这类模型主要对文本进行向量化，把文本变成“向量坐标”。例如：

```plain
“我喜欢编程” → [0.12, -0.98, 0.33, ...]
```

可以用来计算相似度，做语义搜索，做RAG。 

## 测试文本向量化
1. 配置api-key

```yaml
spring:
  ai:
    dashscope:
      api-key: ${qwen}
```

2. 测试代码

<font style="color:#080808;background-color:#ffffff;">spring-ai-alibaba的API一般都是以DashScope开头，通常来讲从spring-ai切换到spring-ai-alibaba也就是换个API的事情。</font>

```java
@SpringBootTest
public class EmbeddingTest {

    @Autowired
    private DashScopeEmbeddingModel embeddingModel;

    @Test
    public void test() {
        EmbeddingResponse embeddingResponse = embeddingModel.call(new EmbeddingRequest(List.of("我喜欢编程"),
                DashScopeEmbeddingOptions.builder().model("text-embedding-v3").build()));

        System.out.println(Arrays.toString(embeddingResponse.getResult().getOutput()));
    }
}

```

#  三、Image（图片模型） 
 图片模型主要用于根据文本描述生成图像（Text-to-Image）。开发者只需要提供一段自然语言提示词（Prompt），模型即可生成对应风格和内容的图片。  

以下是一个简单的示例：

```java
@SpringBootTest
public class ImageTest {

    @Autowired
    private DashScopeImageModel imageModel;


    @Test
    public void test() {
        String prompt = "一只可爱的刺猬在草地上吃苹果，背景是蓝天白云，风格是卡通";
        ImageResponse call = imageModel.call(
                new ImagePrompt(prompt, DashScopeImageOptions.builder().model("wanx2.1-t2i-turbo").build())
        );
        System.out.println(call.getResult().getOutput().getUrl());
    }
}
```

输出结果

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1777451664651-7689e48a-11e1-4cac-a2bf-535f0705d9d7.png)

#  四、Transcription（语音识别）
语音识别（ASR，Automatic Speech Recognition）主要用于将语音内容转换为文本，在 Spring AI 中，可以通过阿里云百炼提供的语音识别模型，快速实现音频转文字功能。

代码示例如下：

```java
@SpringBootTest
public class TranscriptionTest {

    @Autowired
    private DashScopeAudioTranscriptionModel dashScopeTranscriptionModel;

    @Test
    public void test() {
        // 1. 构建语音识别参数
        DashScopeAudioTranscriptionOptions options =
                DashScopeAudioTranscriptionOptions.builder()
                        .model(DashScopeModel.AudioModel.PARAFORMER_V1.getValue())
                        .languageHints(List.of("zh", "en")) // 支持中英文混合识别
                        .disfluencyRemovalEnabled(false)   // 是否去除口语停顿（如“嗯”、“啊”）
                        .punctuationPredictionEnabled(true) // 自动补全标点
                        .build();

        // 2. 加载本地音频文件
        ClassPathResource audioFile = new ClassPathResource("xxx.mp3");

        // 3. 构建请求
        AudioTranscriptionPrompt request =
                new AudioTranscriptionPrompt(audioFile, options);

        // 4. 调用模型
        AudioTranscriptionResponse response =
                dashScopeTranscriptionModel.call(request);

        // 5. 输出识别结果
        System.out.println(response.getResult().getOutput());
    }
}
```

#  五、Speech（语音生成）  
 语音生成（TTS，Text-to-Speech）用于将文本转换为语音，可以实现语音播报、AI配音、语音助手等功能。  

```java
@SpringBootTest
public class SpeechTest {

    @Autowired
    private DashScopeAudioSpeechModel dashScopeAudioSpeechModel;

    @Test
    public void test() throws Exception {
        // 1. 配置语音参数
        DashScopeAudioSpeechOptions speechOptions =
                DashScopeAudioSpeechOptions.builder()
                        .model(DashScopeModel.AudioModel.COSYVOICE_V1.getValue())
                        .voice("longhua") // 发音人
                        .build();

        // 2. 构建文本转语音请求
        TextToSpeechPrompt speechPrompt =
                new TextToSpeechPrompt("支付宝到账1万元", speechOptions);

        // 3. 调用模型生成语音
        TextToSpeechResponse response =
                dashScopeAudioSpeechModel.call(speechPrompt);

        byte[] audioData = response.getResult().getOutput();

        // 4. 保存为音频文件
        try (FileOutputStream fos = new FileOutputStream("xxx.mp3")) {
            fos.write(audioData);
            System.out.println("文件保存成功: xxx.mp3");
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("文件保存失败: " + e.getMessage());
        }
    }
}
```

# 六：总结
本章内容整体不算复杂，主要对Spring AI 的多模态能力 进行了基础介绍。同时，也简单演示了如何从原生 Spring AI 生态，切换到 Spring Alibaba AI + 阿里云百炼模型 的实际用法。依赖于大模型的能力，我们可以很简单的实现语音识别 + 文本处理 + 语音生成等功能。这些能力单看可能比较简单，还需结合到实际项目当中去，构建出自己的AI应用。

示例代码已经上传到：[https://github.com/byone421/springai-demo/tree/main/ai-multimodal](https://github.com/byone421/springai-demo/tree/main/ai-multimodal)







