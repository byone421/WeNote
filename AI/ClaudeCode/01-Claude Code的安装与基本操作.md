# 1.安装Claude Code
**官方推荐用命令行一键安装：**

1. mac/linux用户使用curl安装

```plain
curl -fsSL https://claude.ai/install.sh | bash
```

2. windows上使用PowerShell安装

```plain
irm https://claude.ai/install.ps1 | iex
```

3. windows上使用CMD安装

```plain
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

由于访问claude.ai需要代理，请保证安装前开启了代理。

**提示：**官方已经不在推荐npm安装了，该方法已经过时。 

## 1.1通过PowerShell安装
1. 这里是使用的PowerShell安装，安装前通过命令设置代理，7897为我本地代理的端口。当然可以先试试能不能直接安装，不行再进行设置代理

```java
$env:HTTP_PROXY="http://127.0.0.1:7897"
$env:HTTPS_PROXY="http://127.0.0.1:7897"
```

2. 执行命令安装

```java
irm https://claude.ai/install.ps1 | iex
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1777510320104-0801d883-ae98-4447-8e62-0eb2d6c0ab5e.png)

3. 配置环境变量

安装完成后提示安装到了C:\Users\<用户名>\.local\bin。这里需要我们手动将C:\Users\<用户名>\.local\bin配置到PATH环境变量中。配置完成后测试一下：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1777515310499-349d630e-1f12-4436-a3cc-86020a1844d7.png)

# 2.斜杆命令
在终端输入claude命令来启动Claude，输入** / **之后会提示所有可用的命令。按 **↑  ↓ **键选择，**enter**键选中。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1777902374002-920fb562-a253-43fa-b63d-7e7f876cfa38.png)

这里只显示出了几条，按 ↓ 键 可以往下选择更多。每一个命令后面都有对应的解释。

## 2.1登录和配置
如果已经有 Anthropic 账号可以输入 **/login** 命令进行登录。当然Claude Code作为一个通用的API编程工具。不仅仅只限于Anthropic的模型。对于其它的大模型厂商，我们只需要将ApiKey和BaseUrl配置到环境变量中，具体配置可查看对应模型厂商的接入文档。

我这里是用的是kimi模型，配置了三个环境变量：

```plain
ANTHROPIC_API_KEY：xxxx
ANTHROPIC_BASE_URL: https://api.kimi.com/coding/
ANTHROPIC_MODEL: kimi-k2.6
```

## 2.2第一次使用
如果是第一次使用Claude。我们可以使用 **/powerup** 命令。它会再 CLI 里面运行互动课程，并带动画演示，带你了解关键功能。<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1777901810123-4c7e33ed-fec4-4645-86e2-6875db8eaa68.png)

## 2.3命令是否携带参数
### 常用带参数命令
这些命令通常用于配置、执行具体任务或管理内容，参数决定了它们的具体行为。

| 命令 | 参数说明 | 示例 |
| --- | --- | --- |
| `/compact` | 可选。指定压缩上下文时的聚焦点。 | /compact 保留登录相关的记录，其它都不用了 |
| `/model` | 必选。指定要切换的模型名称。 | /model opus |
| `/effort` | 必选。设置推理强度。 | /effort high |
| `/rename` | 必选。设置新的会话名称。 | /rename payment-refactor |
| `/debug` | 可选。添加问题描述以聚焦调试。 | /debug session timeout |
| `/export` | 可选。指定导出文件名。 | /export notes.md |
| `/plan` | 可选。直接输入初始任务描述。 | /plan migrate API to v3 |
| `/simplify` | 可选。指定重构的侧重点。 | /simplify memory usage |
| `/batch` | 可选。输入批量处理的任务目标。 | /batch update dependencies |


### 常用不带参数命令
这些命令通常是只读查询或瞬时动作，输入后直接显示结果，无需额外信息。

| 命令 | 功能 | 说明 |
| --- | --- | --- |
| `/context` | 查看上下文使用网格 | 显示 Token 分布，无参数 |
| `/cost` | 显示成本统计 | 立即输出用量，无参数 |
| `/status` | 查看系统状态 | 显示版本/模型/连接，无参数 |
| `/clear` | 清空会话历史 | 直接重置，无参数 |
| `/help` | 查看帮助 | 列出所有命令，无参数 |
| `/doctor` | 运行环境诊断 | 立即检查安装健康度 |


## 按功能区分命令
### 1. Context Management（上下文管理命令） 
这类命令控制 Claude 能看到多少历史对话，可以优化 Token 使用。

+ `/context`： 可视化查看上下文占用网格，无需参数
+ `/compact [instructions]` ：带参数压缩历史对话以节省token，例如：/compact 重点保留关于用户登录失败的调试日志，丢弃之前的代码草稿
+ `/clear`：彻底清空上下文对话历史，重新开始，无需参数，清空后便不会再记得之前问过什么

### 2. Session Tools（会话工具命令） 
这类命令主要用来管理对话分支、回溯历史、导出存档。

+ `/rename <name>` ： 带参数为会话设置名称
+ `/resume`： 恢复之前的会话，无需参数，比如关闭了窗口重新打开可以使用/resume选择要恢复的历史对话
+ `/branch` ： 把对话“复制一份”，然后走不同路线继续聊 ,如果当前会话历史很长（几百轮），分叉时会复制全部上下文，初始 Token 消耗较高，建议在关键决策点再分叉
+ `/rewind`：  后悔药，刚才改错了，想撤销重来  
+ `/export`： 将当前会话导出到文件或剪贴板
+ `/btw`: 旁白提问，在不污染正式对话上下文的前提下提问。如：/btw computed 和 watchEffect 区别是什么？  

### 3. Configuration（配置命令） 
这类命令适用于实时调整模型、推理深度、权限和主题设置。

+ `/model <name>` — 带参数切换模型，如：`/model opus`
+ `/effort <level>` — 带参数设置推理深度，如：`/effort high`、`/effort max`
+ `/permissions` — 管理 Claude 的工具执行权限
+ `/config` — 打开设置菜单
+ `/theme` — 创建和切换自定义主题

### 4. Diagnostics（诊断工具命令） 
这类命令可查看成本、检查系统状态、分析代码变更。

+ `/cost` — 立即显示会话成本、时长、代码变更和 Token 用量
+ `/status` — 显示版本、模型和账户信息
+ `/doctor` — 检查安装健康度
+ `/diff` — 交互式查看未提交的代码变更，便于审核
+ `/insights` — 生成一份详细的工作报告，统计本次会话的 Token 消耗、工具调用次数、任务完成度等数据。

# 3.记忆&上下文持久化
Claude 的上下文在每次会话结束后会重置，但项目级记忆可以通过文件持久化机制保存。主要包括 CLAUDE.md 和 Auto Memory 等内容。启动时 Claude 会加载CLAUDE.md，并结合自动生成的记忆文件一起初始化上下文。

## 记忆的三层结构
Claude 的 memory 实际是三层结构：

1. **CLAUDE.md（显式项目记忆）**

主要存放项目规范、约定、规则 等内容，CLAUDE.md 是其核心显式记忆来源。

2. **Auto Memory（隐式记忆）**

Claude基于项目自动生成的 

3. **当前会话上下文（短期记忆）**

临时的记忆，窗口和会话关闭就没了

##  CLAUDE.md 的层级
Claude Code 的记忆系统遵循**“就近覆盖”**原则。当指令冲突时，离当前目录越近的文件优先级越高。

从低到高分别是：

+ **组织级**：由组织统一规范
+ **用户级**：`~/.claude/CLAUDE.md`（你的个人习惯）
+ **项目级**：`./CLAUDE.md或 .claude/CLAUDE.md  `（提交到 Git，与团队共享）
+ **本地级**：`./CLAUDE.local.md`（不提交到Git，用于纯个人或本地临时调试）

### **拆分管理**
对于大型项目，可以将指令拆分到 `.claude/rules/*.md`多个文件中。

同时还支持作用域控制：

```markdown
---
paths: 
  - src/main/java/**/*Controller.java
  - src/main/java/**/*Api.java
  - src/main/java/**/api/**/*.java
---
# Spring Boot API 参数验证与异常处理规范

## 1. 验证注解要求
 **必须使用** Jakarta Bean Validation 注解（JSR-380）：
- `@NotNull`、`@NotEmpty`、`@NotBlank`
- `@Size`、`@Min`、`@Max`、`@Digits`
- `@Email`、`@Pattern`、`@Future`、`@Past`
- `@Valid` 级联验证

**必须使用** `@Valid` 或 `@Validated` 触发验证

## 2. 验证失败响应格式
当验证失败时，**必须返回**：
- **HTTP 状态码**: 400 Bad Request
- **响应体统一格式**:
```

## 创建或更新CLAUDE.md
1. **初始化记忆**
+  直接运行运行 `/init`。Claude 会自动分析项目代码，生成一个基础的 CLAUDE.md
2. **手动编辑记忆**
+  使用 `/memory`命令会打开memory文件，打开后可以直接修改，保存后 Claude 自动重新加载
3. **引用已有文档（避免重复写）**
+ 用 `@路径` 引用文件，好处就是不用复制内容，保持单一信息源

```plain
@README.md
@docs/architecture.md
@package.json
```

## 自动生成的记忆
1. **Auto Memory 是什么**

 Claude 会在使用过程中自动提取项目中的稳定规律和重要经验，并以结构化方式写入 `~/.claude/projects/<project>/memory/` 目录下的记忆文件（如 MEMORY.md）。这是 AI 总结出的长期有用的项目知识，相当于一个自动维护的“项目经验库”。  随着使用频率增加，Claude 会不断积累项目记忆和行为模式，整体协作体验会越来越顺畅。  

2. **加载机制**
+  启动时自动加载： 
    - `MEMORY.md`前 200 行 或 25KB（取较小）
+  其他文件（按需加载）： 
    - `debugging.md`
    - `api-conventions.md`
3. **开关控制**
    1. 使用`/memory`命令 → 打开设置，可以进行关闭<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778314255541-0059ed37-2eb4-46de-afcb-a74fa73b98db.png)
    2. 在 .claude/settings.json中关闭

```json
{
  "autoMemoryEnabled": false
}
```

4. **Monorepo 优化**

大项目里有很多 `CLAUDE.md`，容易污染上下文。可以在项目的 .claude/settings.json  的配置文件中进行排除

```json
{
  "claudeMdExcludes": [
    "**/node_modules/**",
    "**/vendors/**",
    "**/dist/**",
    "**/.next/**",
    "**/coverage/**",
    "**/legacy/**",
    "**/deprecated/**",
    "**/generated/**"
  ]
}
```

# 4.初始化项目
1. **先让AI帮我创建一个项目**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778409157315-69301597-0fc4-4fcb-81de-dd5833335690.png)

经过一番折腾之后，AI成功把项目创建出来了<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778409329464-d4024550-88ec-4b6b-9879-04a2b442591d.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778409393051-71e9066c-ba6e-4363-8f21-e0796a9de5ba.png)

2. **使用 /init 命令创建项目级别的CLAUDE.md**

我们进入到todo项目再次启动claude。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778414543092-a1c6626b-92b2-4671-94b3-cc31e8d0d112.png)

使用** /init ** 命令创建一份CLAUDE.md文件

```markdown
# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 提供该仓库的指引。

## 项目概述

一个基于 Vue 3 + Vite 的中文待办事项应用。所有功能集中在 `TodoApp.vue` 单个组件中，`App.vue` 仅为薄包装层。

## 常用命令

| 命令 | 用途 |
|------|------|
| `npm run dev` | 启动 Vite 开发服务器 |
| `npm run build` | 生产构建 |
| `npm run preview` | 预览生产构建 |

本项目未配置测试框架、Linter 或代码格式化工具。

## 架构说明

- **入口：** `src/main.js` 将根组件 `App.vue` 挂载到 `#app`。
- **状态管理：** 所有待办状态存放在 `src/components/TodoApp.vue` 中，使用 Vue 3 Composition API（`ref`、`computed`）。数据仅存于内存，无持久化层或后端。
- **样式：** `src/style.css` 定义全局 CSS 变量及亮/暗色主题方案。组件样式写在 `TodoApp.vue` 的 `<style scoped>` 中。
- **语言：** 界面文本为中文（`zh-CN`）。

```

## 4.1一份合格的CLAUDE.md
### 1.简洁与聚焦 
**目标**：让文件短小精悍，确保 AI 在几乎每次会话中都能用到其中的信息。

+ **长度控制**：严格控制在 **200 行以内**（理想状态）。
+ **普适性测试**：问自己——“这条规则是否适用于 90% 以上的开发场景？”
    - 如果答案是否定的，它就不该出现在这里。

### 2.内容取舍策略 
#### 应该放入 `CLAUDE.md`的内容（项目全局通用） 
这些内容构成了项目的“地基”，AI 随时可能用到：

| 类别 | 具体内容示例 |
| --- | --- |
| **技术栈与版本** | Node.js 20, Python 3.11, React 18, PostgreSQL 15 |
| **核心开发命令** | `npm install`, `npm run dev`, `pytest`, `make build` |
| **代码规范** | 非显而易见的命名约定（如：`useSnakeCase`vs `camelCase`） |
| **已知陷阱** | “切勿直接修改 `dist/`目录”、“Windows 路径需用双反斜杠” |


#### 应该剥离出去的内容（特定场景） 
如果某条规则**仅针对单一功能或模块**，请将其移至**路径作用域规则文件**中：

+ **错误示例**：在根目录 `CLAUDE.md`中详细描述
+ **正确做法**：在 `src/payments/`目录下创建 `CLAUDE.md`，专门存放支付相关的规则。

## 4.2.CLAUDE.md中主要的四个板块
为了让 `CLAUDE.md`发挥最大效用，建议优先包含以下四个部分，这里我们将AI生成的改造一下。

### 1. Tech Stack & Versions（技术栈与版本） 
明确环境依赖，防止 AI 使用过时的 API 或语法。

```plain
## 技术栈
- **框架**: Vue 3（单文件组件 SFC）
- **构建工具**: Vite
- **语言**: JavaScript（ES Modules，`.js` 后缀，**非 TypeScript**）
- **包管理器**: npm
- **架构特点**: 无状态管理库，无后端，无测试框架
```

### 2. Development Commands（开发命令） 
这是 AI 最常调用的操作，必须准确无误。

```plain
## 开发与构建命令
- `npm run dev` — 启动 Vite 开发服务器
- `npm run build` — 先用 `vue-tsc -b` 进行类型检查，再用 Vite 构建
- `npm run preview` — 本地预览生产构建
```

### 3. Naming Conventions（命名约定） 
只记录那些**不遵循主流惯例**的规则。

```plain
## 编码规范
- **组件写法**：统一使用 `<script setup>`（JavaScript 版）。
- **文件结构**：
  - 根组件：`App.vue`（薄包装层，仅负责挂载）。
  - 主逻辑组件：`src/components/TodoApp.vue`（**所有业务逻辑集中于此**，不使用额外组件拆分）。
  - 入口文件：`src/main.js`。
- **命名规则**：
  - 组件文件：`PascalCase.vue`（例如：`TodoApp.vue`）。
  - 变量/函数：标准的 `camelCase`。
- **样式规范**：
  - 全局样式：`src/style.css`（包含 CSS 变量及亮/暗色主题）。
  - 组件样式：使用 `<style scoped>`。
```

### 4. Known Gotchas（常见陷阱） 
这是最能体现我们经验的地方，帮助 AI 避开坑。

```plain
## 注意事项
以下内容是项目中最容易出错、但文档里又不会明说的“隐形规则”，请严格遵守，否则极易产生无法运行的代码。
###  禁止行为（一旦违反就会出问题）

- ** 不要使用 TypeScript**
  - 项目是纯 JavaScript（`.js` / `.vue`）。
  - 禁止使用：
    - `.ts` 文件
    - `type` / `interface`
    - 泛型 `<T>`
    - `vue-tsc`
  - 即使 Vue 3 支持 TS，这里也**明确禁用**。
```

# 5.Claude Code 权限机制
## 5.1基本行为
Claude Code 是运行在一个**权限系统**当中， Claude Code 默认是“谨慎模式”：  

+ 大多数文件写入（write）需要我们手动确定
+ 所有 bash 命令也需要手动确定

这样做是为了防止 AI 自动执行危险操作。但在真实开发过程，如果每次都确认会比较麻烦。

所以我们可以根据需要配置进行“预授权（pre-approve）”。

## 5.2 /permissions命令  
我们添加一条读取权限，允许读取整个项目所有文件。否则 Claude 看代码都要确认。

1. 选择 add a new rule

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778417305909-e221ddb6-7bd2-4c12-b76c-8ebb090ec7fc.png)

2. 填写我们的规则，并保存到./claude/settings.json下面
+ Read(**/*)   允许读取整个项目所有文件。  

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778417341534-f58a1fd3-e6e9-48d5-b1b7-68a9f2c10dce.png)

3. 查看settings.json

```json
{
  "permissions": {
    "allow": [
      "Read(**/*)"
    ]
  },
  "autoMemoryEnabled": false
}
```

4. 如果修改配置文件，命令行中也会同步生效
+  "Write(src/**/*)", 表示允许写入
+ "Edit(src/**/*)"  表示允许修改

```json
{
  "permissions": {
    "allow": [
      "Read(**/*)",
      "Write(src/**/*)", 
      "Edit(src/**/*)"  
    ]
  },
  "autoMemoryEnabled": false
}

```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778318274747-7f1c2489-747e-4cbd-bc9b-4124cc05f771.png)

5. 对于需要询问或者拒绝的命令，配置也是同理。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778417475205-4855ee52-4890-456a-ae30-8bcf744f5551.png)

6. 推荐的基本权限配置

```plain
前端项目：
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(node *)",
      "Bash(npm *)",
      "Bash(npx *)",
      "Read(**/*)",
      "Write(src/**/*)",
      "Edit(src/**/*)"
    ]
  }
}
Java项目：
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(mvn *)",
      "Bash(java *)",
      "Read(**/*)",
      "Write(src/**/*)",
      "Write(pom.xml)",
      "Edit(src/**/*)"
    ]
  }
}
```





# 






