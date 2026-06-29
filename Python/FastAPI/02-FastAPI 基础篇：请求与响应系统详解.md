在Web服务中，HTTP请求（Request）和HTTP响应（Response）是客户端和服务端交互的核心机制。

客户端通过发送请求来获取资源或执行操作，服务端则根据请求内容返回相应的响应数据。

FastAPI 提供了简洁高效的 `Request`和 `Response`对象，帮助开发者方便地处理传入的请求数据和构建返回的响应内容。例如：

+ 通过 `Request`对象可以轻松获取路径参数、查询参数、请求体、请求头、Cookie 等信息。
+ 通过 `Response`对象可以自定义状态码、响应头、响应体，并支持 JSON、HTML、纯文本、文件流等多种格式输出。

# 一、请求（Request）
## 1. 查询参数(Query Parameters)
查询参数（Query Parameters） 是指 URL 中 `?`之后的部分。多个参数之间用 `&`分隔

### 基础用法
**新建一个user/router.py**

```python
from fastapi import APIRouter

router = APIRouter(prefix="/user", tags=["用户"])

@router.get("")
async def get_users(
        user_name: str,  # 必填参数，不填会报错
        age: int = 0,  # 有默认值的查询参数，不传给默认值
        address: str | None = None,  # 可选查询参数，不传也行
):
    user = {"user_name": user_name, "age": age, "address": address}
    return user

```

**再main.py中注册**

```python
from user.router import router as user_router

# 创建fastapi实例
app = FastAPI()

# 注册路由
app.include_router(user_router)

```

**请求测试**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782203634189-459e5b57-8046-459e-9439-44f612aa723d.png)

**必填参数不传会返回错误**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782203675691-7eeeda90-bb45-4599-8e9e-b9f7435afea2.png)

### 参数校验
FastAPI 通过 `Query` 和 `Annotated` 为查询参数提供声明式校验能力。开发者可以直接在参数定义处配置长度限制、数值范围、正则匹配等规则，并自动生成接口文档，无需编写额外的校验代码。  

**在user/router.py新增一个接口**

```python
@router.get("/page")
async def page_users(
    # 必填参数
    user_name: Annotated[
        str,
        Query(
            min_length=2,
            max_length=20,
            description="用户名，长度2~20个字符"
        )
    ],

    # 有默认值
    age: Annotated[
        int,
        Query(
            ge=0,
            le=150,
            description="年龄，范围0~150"
        )
    ] = 18,

    # 可选参数
    address: Annotated[
        str | None,
        Query(
            max_length=100,
            description="地址"
        )
    ] = None,

    # 正则校验
    phone: Annotated[
        str | None,
        Query(
            pattern=r"^1[3-9]\d{9}$",
            description="手机号"
        )
    ] = None,

    # 分页参数
    page: Annotated[
        int,
        Query(
            ge=1,
            description="页码"
        )
    ] = 1,

    size: Annotated[
        int,
        Query(
            ge=1,
            le=100,
            description="每页条数"
        )
    ] = 10,
):
    return {
        "user_name": user_name,
        "age": age,
        "address": address,
        "phone": phone,
        "page": page,
        "size": size
    }
```

**在文档上也会体现对应的规则**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782204956245-7dc1404a-57c1-4953-b3fa-bfb5ee0dcd26.png)

** Query 常用参数**

| 参数 | 作用 | 示例 |
| --- | --- | --- |
| description | 描述信息 | `description="年龄"` |
| min_length | 最小长度 | `min_length=2` |
| max_length | 最大长度 | `max_length=20` |
| pattern | 正则表达式 | `pattern=r"^\d+$"` |
| ge | 大于等于 | `ge=0` |
| gt | 大于 | `gt=0` |
| le | 小于等于 | `le=100` |
| lt | 小于 | `lt=100` |
| alias | 参数别名 | `alias="userName"` |
| deprecated | 标记废弃 | `deprecated=True` |


### Pydantic 模型
对于参数较多的查询接口，推荐使用 Pydantic 模型封装 Query 参数。这样既能享受 Pydantic 的数据校验能力，又能让接口定义更加简洁清晰，特别适用于分页、筛选等复杂查询场景。  

**新建一个user/****<font style="color:#080808;background-color:#ffffff;">schemas.py</font>**

```python
from pydantic import BaseModel, Field


class UserQuery(BaseModel):
    user_name: str | None = Field(default=None, max_length=20)
    age: int | None = Field(default=None, ge=0, le=150)
    address: str | None = None
    page: int = Field(default=1, ge=1)
    size: int = Field(default=10, ge=1, le=100)
```

**在user/router.py中新增一个测试接口**

```python
@router.get("/page2")
async def page_users2(
        # 使用Annotated是为了告诉FastAPI： 
        # UserQuery 的数据来源于 Query 参数而不是 Request Body
        query: Annotated[UserQuery, Query()]  
):
    # 使用model_dump，也会对返回前的对象进行校验。
    return query.model_dump()

```

## 2. 路径参数
路径参数是 URL 路径中的动态部分，通常用于标识某个具体资源。FastAPI 会根据函数参数名称自动提取路径中的值，并进行类型转换和校验。 

### 基础用法
```python
@router.get("/detail/{user_id}") 
async def get_user_detail(user_id: int): # 路径参数会根据类型注解自动转换
   return {
        "user_id": user_id
}
```

上述接口中{user_id}就是路径参数，路径参数可以有多个:

```python
@router.get("/users/{user_id}/orders/{order_id}")
async def get_user_order(user_id: int, order_id: int): 
    return {
        "user_id": user_id,
        "order_id": order_id
    }
```

**请求测试**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782208447015-d7ad3246-69cc-4f4e-96ab-7d728e88e2c4.png)

**需要的int参数，如果传字符串就会报错**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782208496481-557763fe-2976-4e57-9bbd-1be42c81794e.png)

### 使用 Path 增加校验规则
 和 Query 参数类似，FastAPI 提供了 `Path` 用于声明路径参数的校验规则和元数据。  

```python
@router.get("/detail/{user_id}") 
async def get_user_detail(
    user_id: Annotated[
        int,
        Path(
            ge=1,
            description="用户ID"
        )
    ]
):
    return {"user_id": user_id}
```

### 一个注意点
```python
# 这是动态路由
@router.get("/users/{user_name}")
async def get_user(user_name: str):
    return {"user_name": user_name}


# 这是静态路由
@router.get("/users/hello")
async def hello():
    return {"msg": "hello"}
```

当我访问 GET /users/hello时会执行哪个函数。取决于那个函数写在前面。像上述的写法就会导致永远只会执行到动态路由，而静态路由不会执行。

所以官方推荐的是：静态路由写在前面，动态路由写在后面：

```python
# 这是静态路由
@router.get("/users/hello")
async def hello():
    return {"msg": "hello"}


# 这是动态路由
@router.get("/users/{user_name}")
async def get_user(user_name: str):
    return {"user_name": user_name}
```



## 3. 请求体参数（Request Body）  
 前面介绍的 Query 参数和 Path 参数都来自 URL， 而对于创建、修改等操作，需要传递大量结构化数据，此时通常使用 **请求体（Request Body）**。   在 FastAPI 中，请求体通常使用 **Pydantic 模型** 定义，FastAPI 会自动完成请求数据解析 ，类型转换，参数校验 ，Swagger 文档生成 。

### 基础用法
**在user/schemes.py中定义UserCreateRequest**

```python
class UserCreateRequest(BaseModel):
    # 定义请求体字段包含参数校验
    user_name: str = Field(
        min_length=2,
        max_length=20
    )
    age: int = Field(
        ge=0,
        le=150
    )
```

在**user/router.py**新增接口

```python
@router.post("")
async def create_user(
    request: UserCreateRequest
):
    return request.model_dump()
```

在文档测试

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782210044894-14e8449a-4068-412b-850c-618a19988f7b.png)

###  请求体与查询参数混用  
```python
@router.post("")
async def create_user(
    request: UserCreateRequest,
    notify: bool = False
):
    return {
        "user": request.model_dump(),
        "notify": notify
    }
```

请求：

```plain
POST /user?notify=true
```

请求体：

```plain
{
  "user_name": "Tom",
  "age": 18
}
```

### 嵌套对象  
请求体最大的优势就是支持复杂结构。

```python
from pydantic import BaseModel


class Address(BaseModel):
    city: str
    street: str


class UserCreateRequest(BaseModel):
    user_name: str
    age: int
    # 嵌套对象
    address: Address
```

请求体样例如下：

```json
{
  "user_name": "Tom",
  "age": 18,
  "address": {
    "city": "Shanghai",
    "street": "Pudong"
  }
}
```

## 4. 表单参数
Form 参数用于接收表单提交的数据，常见于登录、注册等传统 HTML 表单场景，数据格式为 application/x-www-form-urlencoded 或 multipart/form-data。  

### 基础用法
**在auth/router.py中新增一个登陆接口**

```python
router = APIRouter(prefix="/auth", tags=["认证"])

@router.post("/login")
async def login(
    username: str = Form(),
    password: str = Form()
):
    return {
        "username": username
    }
```

**在文档中测试可以看到**

```python
POST /auth/login
Content-Type: application/x-www-form-urlencoded
```

**请求体为：**

```plain
username=admin&password=123456
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782293984585-23ff9073-b7be-4b67-af05-17b88960ec83.png)

### Form + File 上传（UploadFile）  
新增一个表单参数和文件混合的接口

```python
@router.post("/profile")
async def update_profile(
    # ... 是 Python 内置的 Ellipsis 对象，FastAPI 只是借用它来表示“必填参数”，
    # FastAPI 全部组件都支持：Query(...)，Path(...)，Form(...)
    username: str = Form(...),
    age: int = Form(...),
    avatar: UploadFile = File(...)
):
    return {
        "username": username,
        "age": age,
        "file": avatar.filename
    }
```

** 当表单中包含文件时， Content-Type就是：multipart/form-data  **

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782294723835-0ef7dea2-8646-4ad2-98b7-47cea515a447.png)

## 5. 请求头
HTTP 请求头用于携带**元信息**，比如：

+  token 
+  user-agent 
+  language 
+  content-type

### 基础用法
**我们继续在auth/router.py新增一个接口**

```python

@router.get("/headers")
async def get_headers(
    token: str | None = Header(default=None)
):
    return {
        "token": token
    }
```

**测试一下，能成功获取到token**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782295527783-af138306-a58c-4b79-a174-c6390eed38d1.png)

### 一个注意点
 Header 默认会做 **自动转换命名。**

 因为 Python 变量不能有 `-` ，FastAPI会把 **user_agent** 转成**User-Agent** ：

```python
@router.get("/headers")
async def get_headers(
        token: str | None = Header(default=None), 
         #可以通过Header(convert_underscores=False)关闭自动转换，
         #一般不这样做
        user_agent: str = Header() 
):
    return {
        "token": token,
        "user-agent": user_agent
    }
```

## 6. cookie
 Cookie 是浏览器存储在客户端的小数据， 浏览器会自动保存  ，自动随请求发送。

### 基础用法
```python
from fastapi import Cookie


@router.get("/cookie")
async def get_cookie(
    session_id: str | None = Cookie(default=None)
):
    return {
        "session_id": session_id
    }
```

在文档上测试cookie发现传了没用。这里切换成专业的api工具测试通过（apifox）。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782354785740-5dd6d96b-dcbc-4f28-b254-90d3b7f6908c.png)

# 二、响应（Response）
FastAPI 默认会把你返回的 Python 数据（如字典、列表、Pydantic 模型）自动转成 JSON 并返回，不需要你手动做序列化。

如果要返回 HTML、文件或流数据，可以使用 FastAPI 提供的专门响应类型（如 `HTMLResponse`、`FileResponse`、`StreamingResponse`）。

## 1. JSONResponse 
这是FastAPI默认的响应类型：

```python
content-type: application/json
```

### 基本用法
**在src/resp/router.py中新增一个接口**

```python
from fastapi import APIRouter, Query
from starlette.responses import JSONResponse

router = APIRouter(prefix="/resp", tags=["响应测试"])

@router.get("/json")
def json_demo():
    return JSONResponse(content={"msg": "ok", "code": 200})
```

**进行测试**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782357395977-36df2365-27e6-4130-b14a-63a64dcb9a1d.png)

### 定制Response
我们也可以对Response对象进行设置来返回具体的内容和响应头，响应码等信息。

```python
@router.get("/json")
def json_demo():
    resp = JSONResponse(content={"msg": "ok", "code": 200},
                        status_code=200,
                        headers={"Content-Type": "application/json"})
    resp.set_cookie(
        key="session_id",
        value="abc123"
    )
    return resp
```

### response_class  
在 FastAPI 中，可以在装饰器上通过 `response_class` 和 `status_code` 来预先定义响应类型和状态码，这样在函数内部只需要返回“原始数据”，不需要手动构造 Response，使代码更简洁。  

```python
@router.get("/json", status_code=200, response_class=JSONResponse)
def json_demo():
    return {"msg": "ok", "code": 200}
```

## 2. HTMLResponse 
用于返回html网页。响应类型为：

```python
content-type：text/html; 
```

### 基础用法
```python
@router.get("/html", response_class=HTMLResponse)
def html_demo():
    return "<h1 style='color:red'>Hello FastAPI</h1>"
```

**进行测试**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782357613751-9f57fad9-24d7-44d3-a294-a9d7d73ccae6.png)

## 3. PlainTextResponse  
用于返回纯文本。响应类型为：

```python
content-type: text/plain
```

### 基础用法
```python
@router.get("/text")
def text_demo():
    return PlainTextResponse("hello world")
```

进行测试

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782358038093-02dac86b-0c47-4dda-b497-93a8b7434dbe.png)

## 4. FileResponse 
主要用于文件下载，浏览器访问会直接下载文件。响应类型为：

```python
content-disposition: attachment; filename="my-image.png"
content-type: image/png
```

### 基础用法
```python
@router.get("/download")
def download():
    return FileResponse(
        # 服务器的文件
        path="D:/temp/1.png",
        # 下载的文件名
        filename="my-image.png"
    )
```

## 5. StreamingResponse
流式响应：一边生成一边返回 ，避免一次性返回大量的数据

### 示例1
下载大文件

```python
def file_stream():
    """
    大文件分段下载不会爆内存
    """
    with open("bigfile.zip", "rb") as f:
        while chunk := f.read(1024 * 1024):
            yield chunk


@router.get("/stream-file")
def stream_file():
    return StreamingResponse(file_stream())
```

### 示例2
模拟ai流式响应

```python
def ai_stream():
    for word in ["Hello", "FastAPI", "Streaming"]:
        yield (word + " ").encode("utf-8")
        time.sleep(0.5)

@router.get("/ai")
def ai():
    return StreamingResponse(ai_stream())
```

## <font style="color:rgb(0,0,0);">6. RedirectResponse</font>
RedirectResponse 用于让接口返回一个重定向指令，使客户端自动跳转到另一个 URL。

### 基础用法
```python
@router.get("/login")
def login():
    # 登录成功后跳转到首页
    return RedirectResponse(url="/home")
```

## 7. response_model
`response_model` 用来定义接口“返回数据的结构”，FastAPI 会自动做过滤 + 校验 + 文档生成。  

### 基础用法
定义接口和模型对象

```python
class User(BaseModel):
    name: str
    age: int


@router.get("/user", response_model=User)# 最终返回的数据必须“符合 User 模型结构”
def get_user():
    return {
        "name": "Tom",
        "age": 18,
        "password": "secret"  # 会被自动过滤掉
    }

```

测试一下

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782366992395-7bb31cd0-2faa-46d1-98c5-af7ba98c6fed.png)



 对于 FastAPI 中请求与响应的基础内容，我们就先介绍到这里。关于更多 深入 FastAPI 的内容，我们下期再见👋。







