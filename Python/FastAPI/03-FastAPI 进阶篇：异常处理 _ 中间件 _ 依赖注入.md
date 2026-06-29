在使用 FastAPI 开发接口时，我们通常很快就能完成基本的 CRUD 功能。但随着项目复杂度提升，仅仅会写接口是不够的，还需要处理一些工程化问题，比如异常统一处理、请求拦截等。

这些能力主要依赖 FastAPI 的四个核心机制：

+  异常处理（Exception Handling） 
+  中间件（Middleware） 
+  依赖注入（Dependency Injection） 

掌握这些内容，可以让你的 FastAPI 项目从“能用”提升到“规范、可维护”。 接下来我们逐个来看。  

# 全局统一返回结构（Result）  
在实际项目中，为了让前后端交互更规范，通常会统一接口返回格式，比如：  

```python
Result(code, msg, data)

{
  "code": 200,
  "msg": "success",
  "data": {}
}
```

## 定义统一返回对象  
可以先封装一个通用结构：

```python
from pydantic import BaseModel
from typing import Any, Optional

class Result(BaseModel):
    code: int = 200
    msg: str = "success"
    data: Optional[Any] = None

    @staticmethod
    def success(data=None, msg="success"):
        return Result(code=200, msg=msg, data=data)

    @staticmethod
    def error(code=400, msg="error", data=None):
        return Result(code=code, msg=msg, data=data)
```

## 接口中使用
```python
@app.get("/user")
def get_user():
    return Result.success(data={"name": "Tom"})
```

# 异常处理（Exception Handling）
 FastAPI 的异常处理分两层：  

## 内置异常：HTTPException
这是最常用的：

```python
@router.get("/detail/{user_id}")
async def get_user_detail(user_id: int):  # 路径参数会根据类型注解自动转换
    if user_id != 1:
        raise HTTPException(status_code=404, detail="User not found")
    return {
        "user_id": user_id
    }
```

当我们访问user_id为2的用户就会发生异常

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782370891906-fac66e85-949f-49af-a48f-375725a72339.png)

## 自定义异常 + 全局处理
你可以定义自己的异常：

```python
class MyException(Exception):
    def __init__(self, msg: str):
        self.msg = msg
```

然后注册全局异常处理器：

```python
from fastapi import Request
from fastapi.responses import JSONResponse

# app = FastAPI()
@app.exception_handler(MyException)
async def my_exception_handler(request: Request, exc: MyException):
    return JSONResponse(
        content=Result.error(msg=exc.msg).model_dump()
    )
```

**注意点：**

+ `@app.exception_handler(...)` 是**全局生效**
+  只要是这个 app 下的所有路由都会生效 

修改接口

```python
@router.get("/detail/{user_id}")
async def get_user_detail(user_id: int):
    if user_id != 1:
        raise MyException("用户id异常")
    return {
        "user_id": user_id
    }

```

进行测试

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782371467563-a2fce330-c9f0-4d2a-8224-981b4b689272.png)

## Pydantic 校验异常也统一 
pydantic校验不通过会抛出RequestValidationError，我们对RequestValidationError也进行处理。

注册异常处理器

```python
@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        content=Result.error(
            code=422,
            msg="参数校验失败",
            data=exc.errors()
        ).model_dump())
```

用之前的接口测试

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

@router.post("")
async def create_user(
        request: UserCreateRequest
):
    return request.model_dump()

```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782371752493-8a26c9a3-a426-42ca-95b1-12cf34ec8396.png)

测试一下返回结果

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1782371793931-c470e1ab-d905-46ae-ad89-3c22c7adca41.png)

# 中间件（Middleware）
中间件本质是：**请求进来 → 处理 → 响应出去 的“拦截器”**

## 执行顺序
```plain
请求 → middleware1 → middleware2 → 路由 → middleware2 → middleware1 → 返回
```

## 能干嘛
+ 记录日志（log） 
+  鉴权（token校验） 
+  请求耗时统计 
+  CORS（跨域） 
+  Trace ID（链路追踪） 

### 最基础中间件
```python
from fastapi import Request

# "http" 不是名字，而是“中间件类型
@app.middleware("http")
async def log_middleware(request: Request, call_next):
    print("请求来了:", request.url)

    response = await call_next(request)

    print("响应返回")
    return response
    
@app.middleware("http")
async def log_middleware2(request: Request, call_next):
    print("请求来了2:", request.url)

    response = await call_next(request)

    print("响应返回2")
    return response
```

调用一下[http://localhost:8000/user/detail/2](http://localhost:8000/user/detail/2)接口，并查看打印日志

```plain
请求来了2: http://localhost:8000/user/detail/2
请求来了: http://localhost:8000/user/detail/2
响应返回
响应返回2
```

这还有一个美名叫（洋葱模型）  

```python
log_middleware2   ← 外层（最后定义）
    log_middleware
        route
    log_middleware
log_middleware2   ← 外层返回
```

# 依赖注入
在 FastAPI 中，**依赖注入（Depends）就是：** 把函数执行结果自动注入到参数里 ，自动传给接口，而不是在接口里手动创建。

## 依赖注入的常见用途
### 获取数据库连接  
** database.py  **

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite:///./test.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)

SessionLocal = sessionmaker(
    autocommit=False,
    autoflush=False,
    bind=engine
)
```

** 依赖：获取 DB Session  **

```python
from .database import SessionLocal

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

** 使用 DB**

```python
from fastapi import Depends
from sqlalchemy.orm import Session

@app.get("/users")
def get_users(db: Session = Depends(get_db)):
    users = db.query(User).all()
    return users
```

###  获取当前用户
 模拟 token 获取  

```python
from fastapi import Header, HTTPException

def get_token(authorization: str = Header(None)):
    if not authorization:
        raise HTTPException(status_code=401, detail="No token")

    # 简化：Bearer xxx
    token = authorization.replace("Bearer ", "")
    return token
```

 模拟解析用户  

```python
# 这里依赖注入了get_token
def get_current_user(token: str = Depends(get_token)):
    # 模拟 token 解析
    if token != "valid-token":
        raise HTTPException(status_code=401, detail="Invalid token")

    return {
        "id": 1,
        "username": "Tom"
    }
```

##  嵌套依赖
在 FastAPI 中，依赖注入不仅可以用在接口函数上，还可以**在依赖函数里继续依赖其他依赖**，这就是嵌套依赖。

### 示例伪代码
最底层依赖：DB

```python
def get_db():
    db = "db_session"
    try:
        yield db
    finally:
        print("关闭 DB")
```

中间层依赖：用户（依赖 DB）

```python
from fastapi import Depends

def get_current_user(db=Depends(get_db)):
    return {
        "id": 1,
        "username": "Tom",
        "db": db
    }
```

接口层（依赖用户）

```python
@app.get("/profile")
def profile(user=Depends(get_current_user)):
    return user
```

### 执行流程
**当请求 **`**/profile**`** 时，执行顺序是：**

```plain
profile
  ↓
get_current_user
  ↓
get_db
  ↓
返回 db
  ↓
返回 user
  ↓
返回 response
```



以上就是关于 FastAPI 的一些进阶功能（异常处理、中间件、依赖注入）。通过这些内容，我们已经可以让一个 FastAPI 项目具备基本的工程化能力。也希望对大家有所帮助。下一期我们再继续深入，聊一聊 FastAPI 项目的架构设计，比如如何分层、如何组织模块以及如何在实际项目中落地。

