# 为什么要用FastAPI
如今人工智能发展得如火如荼，Python凭借其在整个AI生态中的主导地位显得深不可测。相比之下，Java则显得有些鞭短莫及呀。而借助FastAPI，我们可以轻松利用这些AI生态来提供高效的AI服务。

# FastAPI介绍
对于初学者来讲，对于FastAPI 的介绍/特点/场景混个眼熟就好。只需要知道是个WEB框架就行。核心思想就是先用起来，再来回味。

## 简单介绍
**FastAPI** 是一个基于 Python 类型注解构建的现代化高性能 Web 框架，专门用于开发 RESTful API 和微服务。

它建立在 **Starlette** 和 **Pydantic** 之上，具备自动数据校验、自动生成 **OpenAPI/Swagger** 接口文档、异步编程支持以及极高的开发效率。

由于充分利用 Python 的类型提示机制，开发者只需定义请求和响应模型即可获得参数校验、序列化和文档生成等能力。

在保持代码简洁的同时提供接近 Node.js 和 Go 等框架的性能表现，因此被广泛应用于 AI 服务、微服务架构、数据平台以及机器学习模型接口开发场景。  

## 特点
| 特点 | 说明 |
| --- | --- |
| 高性能 | 基于 Starlette + Pydantic，性能接近 Node.js / Go，是 Python 中最快的 Web 框架之一 |
| 快速开发 | 使用 Python 类型注解即可完成参数校验、序列化、文档生成，开发效率提升显著 |
| 减少错误 | 类型系统 + 自动校验机制，可在开发阶段捕获大量参数与数据错误 |
| 自动文档 | 自动生成 Swagger UI 和 ReDoc，无需手写 API 文档 |
| 类型安全 | 基于标准 Python type hints，IDE 自动补全和静态检查能力强 |
| 异步支持 | 原生支持 async/await，适合高并发 IO 场景 |
| 依赖注入 | 内置轻量依赖注入系统，方便模块解耦 |
| 生态现代化 | 与 Pydantic / Uvicorn / Starlette 深度集成 |


## 适用场景
| 场景 | 说明 |
| --- | --- |
| REST API 后端 | 最核心使用场景，用于构建标准 Web API |
| 微服务架构 | 轻量、启动快，适合拆分服务 |
| AI / LLM 服务 | 常用于封装模型推理 API（非常主流） |
| 数据服务层 | JSON API、数据处理接口 |
| 实时通信 | 支持 WebSocket，可做实时推送服务 |
| 前后端分离项目 | 作为纯后端 API 服务非常合适 |
| 高并发 IO 服务 | async 模型适合数据库 / HTTP 调用密集型系统 |


# 安装FastAPI
在Python中，我们安装一个依赖通常都是pip install xxx。而pip install默认从 PyPI（Python Package Index，官方源）下载。我们在上面找到fastapi的某一个版本下载下来：[https://pypi.org/simple/fastapi/](https://pypi.org/simple/fastapi/)。

此处下载的是 fastapi-0.136.3.tar.gz

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781078281847-6bef7c7f-4a3f-42c3-ba95-9972afbb5281.png)

接下来打开压缩包中的pyproject.toml

## 环境和依赖
这里粘贴pyproject.toml的部分内容。

```powershell

requires-python = ">=3.10"

dependencies = [
    "starlette>=0.46.0",
    "pydantic>=2.9.0",
    "typing-extensions>=4.8.0",
    "typing-inspection>=0.4.2",
    "annotated-doc>=0.0.2",
]
version = "0.136.3"

[project.optional-dependencies]
standard = [
    "fastapi-cli[standard] >=0.0.8",
    "fastar >= 0.9.0",
    "httpx >=0.23.0,<1.0.0",
    "jinja2 >=3.1.5",
    "python-multipart >=0.0.18",
    "email-validator >=2.0.0",
    "uvicorn[standard] >=0.12.0",
    "pydantic-settings >=2.0.0",
    "pydantic-extra-types >=2.0.0",
]
standard-no-fastapi-cloud-cli = [
    "fastapi-cli[standard-no-fastapi-cloud-cli] >=0.0.8",
    "httpx >=0.23.0,<1.0.0",
    "jinja2 >=3.1.5",
    "python-multipart >=0.0.18",
    "email-validator >=2.0.0",
    "uvicorn[standard] >=0.12.0",
    "pydantic-settings >=2.0.0",
    "pydantic-extra-types >=2.0.0",
]
all = [
    "fastapi-cli[standard] >=0.0.8",
    "httpx >=0.23.0,<1.0.0",
    "jinja2 >=3.1.5",
    "python-multipart >=0.0.18",
    "itsdangerous >=1.1.0",
    "pyyaml >=5.3.1",
    "email-validator >=2.0.0",
    "uvicorn[standard] >=0.12.0",
    "pydantic-settings >=2.0.0",
    "pydantic-extra-types >=2.0.0",
]
```

从pyproject.toml的内容我们可以得知：

+ **当前版本：**version = "0.136.3"
+ **确定了需要python环境**：requires-python = ">=3.10"
+ **可选依赖：**[project.optional-dependencies]

这里我看了一眼FastAPI官网：[https://fastapi.tiangolo.com/#requirements](https://fastapi.tiangolo.com/#requirements)。

它选择是的标准可选依赖：

```python
pip install "fastapi[standard]"
```

为了学习方便我这里安装的是全量可选依赖，并且指定了版本号

```python
pip install "fastapi[all]==0.136.3"
```

这个命令会安装：

1.  FastAPI 核心依赖（`dependencies`） 
2. `all` 这个 optional-dependencies 里的依赖 
3.  这些依赖自身的传递依赖（transitive dependencies） 

### FastAPI 核心依赖
| 依赖 | 作用 |
| --- | --- |
| starlette | ASGI Web框架底座 |
| pydantic | 数据校验、序列化 |
| typing-extensions | Python类型扩展 |
| typing-inspection | 类型分析 |
| annotated-doc | Annotated文档支持 |


### fastapi[all] 额外安装
| 依赖 | 作用 |
| --- | --- |
| fastapi-cli[standard] | FastAPI官方CLI |
| httpx | HTTP客户端 |
| jinja2 | 模板引擎 |
| python-multipart | 文件上传 |
| itsdangerous | Session/签名 |
| pyyaml | YAML配置 |
| email-validator | Email字段校验 |
| uvicorn[standard] | ASGI服务器 |
| pydantic-settings | pydantic配置管理 |
| pydantic-extra-types | pydantic扩展类型库   |


# 创建项目和虚拟环境
为了避免与系统全局的Python包产生冲突，我们一般都会给单独的python项目创建虚拟环境

1. **检查python版本**

版本符合>=3.10

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781080576929-5e599bc8-2d88-49d5-8383-03e22fd53e32.png)

2. **创建虚拟环境**

创建一个fastapi-demo文件夹，创建虚拟环境并激活虚拟环境

```powershell
# 创建虚拟环境
python -m venv .venv

# macOS/Linux上激活虚拟环境
source venv/bin/activate

# Windows上激活虚拟环境
.venv\Scripts\activate
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781086291196-2eb9647a-4ec5-4087-869d-14f112919413.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781086327464-122f13b5-96aa-4911-b617-5f6af8fce66f.png)

激活 Python 虚拟环境后，命令行会显示以 `.venv`开头的提示符，说明当前已处于该环境中。

我们直接执行 **pip install "fastapi[all]==0.136.3"**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781081931759-0ad0ab1e-9b50-4220-9b30-422bb2ad1c0b.png)

## 镜像下载
如果访问官方源太慢，我们可以使用国内镜像网站来进行加速安装：

```python
pip install "fastapi[all]==0.136.3" -i https://pypi.tuna.tsinghua.edu.cn/simple
```

| **镜像** | **地址** |
| :--- | :--- |
| **清华大学** | `https://pypi.tuna.tsinghua.edu.cn/simple` |
| **阿里云** | `https://mirrors.aliyun.com/pypi/simple/` |
| **腾讯云** | `https://mirrors.cloud.tencent.com/pypi/simple/` |
| **华为云** | `https://repo.huaweicloud.com/repository/pypi/simple/` |


# 运行 FastAPI
用开发工具软件打开fastapi-demo项目，并编写main.py。这里使用的PyCharm。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781083482028-afe58d6e-baba-4b35-b9a9-76e1894e17d6.png)

**main.py**

```powershell
from fastapi import FastAPI
import uvicorn

# 创建fastapi实例
app = FastAPI()

# 定义一个接口
@app.get("/")
async def root():
    return {"message": "Hello World"}

```

## 启动项目
在PyCharm中打开命令行，好处就是会自动帮我们激活虚拟环境。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781086970406-e9ca87fd-116e-48b0-bd90-3a3b2aca087e.png)

## 通过<font style="color:#080808;background-color:#ffffff;">uvicorn启动</font>
```powershell
uvicorn main:app
```

+ **--host 0.0.0.0：**可选参数， 表示监听所有可用的网络接口，允许外部设备访问这个服务
+ **--port 8000：**可选参数 ,表示启动端口是8000
+ **--reload: ** 可选参数 ,开发模式，代码修改后自动重载服务器

## 通过<font style="color:#080808;background-color:#ffffff;">fastapi cli启动</font>
```powershell
fastapi dev
```

+ **fastapi dev:** 开发模式<font style="color:rgb(51, 51, 51);">，代码修改后自动重载服务器</font>
+ **fastapi run:** 生产模式
+ **--host 0.0.0.0：**可选参数， 表示监听所有可用的网络接口，允许外部设备访问这个服务
+ **--port 8000：**可选参数 ,表示启动端口是8000

## <font style="color:#080808;background-color:#ffffff;">通过Python代码启动</font>
在main.py中加上一个main方法。直接启动main方法

```python
from fastapi import FastAPI
import uvicorn

# 创建fastapi实例
app = FastAPI()

# 定义一个接口
@app.get("/")
async def root():
    return {"message": "Hello World"}

# 定义启动方法
if __name__ == "__main__":
    uvicorn.run(
        "main:app",
        host="127.0.0.1",
        port=9998,
        reload=True
    )
```

# 测试接口和API文档
## 测试接口
选择任意一种方式启动项目之后，我们直接访问控制台打印的地址

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1781085002305-c0a44c9d-8e1e-4f45-bbb4-e1347736da19.png)

能返回Hello Word表示已经成功。

```python
{"message":"Hello World"}
```

## API 文档
FastAPI 能根据代码中的类型注解，自动生成交互式 API 文档，默认自带 Swagger UI 和 ReDoc 两种界面。开发者可直接在文档里测试 API，无需借助其他工具。

**默认提供的三种 API 文档入口：**

| 地址 | 界面类型 | 说明 |
| --- | --- | --- |
| `http://IP:端口/docs` | Swagger UI | Swagger UI，支持在线调试 |
| `http://IP:端口/redoc` | ReDoc |  ReDoc，适合阅读 API 定义 |
| `http://IP:端口/openapi.json` | OpenAPI JSON |  OpenAPI 原始规范，供程序调用 |




如果觉得有帮助😃，欢迎点赞、收藏、加关注！我们一起成长🔥。下期再见～







