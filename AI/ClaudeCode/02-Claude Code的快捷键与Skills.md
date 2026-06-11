# 一、常用快捷键和指令
1. **权限模式轮转 (Shift+Tab) **

有三种模式：

+ **default（默认模式）**：仅读取操作无需审批，所有文件编辑和 Shell 命令都会逐项向你确认，适合新手或处理敏感代码。
+ **acceptEdits（自动接受编辑）**：文件读取和编辑自动批准，常见文件系统命令（mkdir、touch、mv、cp 等）也免确认，只有 Shell 命令仍需你确认，适合日常迭代开发。
+ **plan（计划模式）**：只读权限，Claude 只能分析、探索并给出方案，不会修改任何文件或执行命令，适合和claude探索方案，确认没问题再执行。

在输入框的下面会显示当前的模式

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778424116119-5b0c37a2-e502-48b5-aa5a-1e4773f40609.png)

2. **深度思考(Option/Alt+T)**

让 Claude 进行更长时间的思考，适合逻辑推理或复杂代码生成。可以用 **/effort **命令选择推理深度等级

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778424375363-ef6a58a4-f6d4-4ce0-8de2-05963abebc2b.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778424309222-d7b1b79a-4eb8-4a63-bd69-a2b602f58e8d.png)

3. **详细模式 (Ctrl+O)**

开启后能看到 Claude 的打印和工具调用细节，相当于查看 AI运行时日志

4. **后台任务控制** 
+ **Ctrl+B**：将当前运行的长任务（如 `bash`编译、Agent 工作流）放入后台，立即释放输入框给新指令。
+ **Ctrl+X, K**：一键杀死所有后台进程，清理资源。
5. **输入缓冲区操作 **
+ **Ctrl+U**：彻底清空当前输入框。
+ **Ctrl+Y**：后悔药。恢复刚才被 `Ctrl+U`清空的内容，避免长提示词误删重写。
6. **幕重绘 (Ctrl+L)**：

强制刷新终端显示，解决输出残影问题。

# 二、Claude Skills
## 2.1 什么是 Skills
Skills 是 Claude Code 中的一种：可被自动发现和复用的能力模块，<font style="color:rgb(0, 0, 0);">Skill 本质是一个 SKILL.md 文件</font>。Claude 会根据当前任务、项目上下文、工作目录、用户请求来自动判断：当前是否需要某个 Skill。

**为什么需要Skills？**

如果所有东西

```plain
- JavaScript规范
- Java规范
- SQL规范
- Redis规范
- Review规范
- Docker规范
- 发布规范
```

如果所有的的东西都放在CLAUDE.md 就会导致prompt爆炸，所以Skills还是有必要的。

##  2.2 Skills 的核心思想
SKILL.md在结构上明确分为 头部（Head / Frontmatter） 和 体部（Body / Content） 两个部分

+ **头部**：文件最顶端的 YAML 元数据块，由一对 `---`包裹。
+ **体部**：头部结束的 `---`之后的所有 Markdown 内容。用于编写给 Claude 的具体“执行指令”。

Claude会预先加载头部少量信息，按需加载体部全部信息，结果就是可安装大量 Skills，而不会占满上下文窗口。

```plain
Claude启动
    ↓
读取Skill描述
    ↓
知道有哪些能力
    ↓
用户触发任务
    ↓
再加载完整SKILL.md
```

## 2.3 Skills 的存放位置
Claude Skills 支持：

+  项目级（Project Scope） 
+  个人级（Personal Scope） 
+  插件级（Plugin Scope） 

**1. 项目级 Skills**

项目级 Skills 存放在(属于当前项目，可以提交到 Git )：

```plain
.claude/skills/<name>/SKILL.md
```

例如：

```plain
.claude/skills/code-review/SKILL.md
```

**2. 个人级 Skills**

个人级 Skills 存放在(只属于你自己、不提交到Git、所有项目都可以复用  )：

```plain
~/.claude/skills/<name>/SKILL.md
```

例如：

```plain
~/.claude/skills/mysql-optimize/SKILL.md
```

**3. Plugin Skills**

可以使用 **/plugin install <插件名>@<市场名>** 命令来安装现有的插件

比如：

```plain
/plugin install frontend-design@anthropic
```

## 2.4 Skills 的优先级
Claude Skills 的优先级：enterprise > personal > project

分别对应：

| Scope | 位置 | 谁管理 |
| --- | --- | --- |
| enterprise | 企业托管配置 | 公司/管理员 |
| personal | `~/.claude/skills/` | 个人 |
| project | `.claude/skills/` | 项目 |


## 2.5 Nested Skills
Claude 还有一个非常强的能力：自动发现嵌套 Skills

Claude 不仅会扫描项目根目录：

```plain
.claude/skills/
```

还会自动发现：子目录中的 Skills

例如：

```plain
packages/frontend/.claude/skills/
packages/backend/.claude/skills/
```

# 三、编写Skill.md
 如何写好 Claude Skill 的 description，让它真正“被触发”而不是只是摆设。可以总结成以下几点：

## 3.1 说明触发场景
在Skill.md的文件头，说明触发场景 写在 Skill 的 **frontmatter（头部元信息）里**

```plain
---
name: vue-state-review
description: xxx
when_to_use: xxxxx
---
```

比如之前的todo项目，项目里会有：

+  Vue3 Composition API 
+  组件拆分 
+  状态管理 
+  Todo 列表渲染 
+  表单输入 
+  性能优化 

这些都非常适合做 Skill。

**错误写法：**

```plain
description: Helps with Vue components
```

问题：

+  太模糊 
+  没有任务 
+  没有触发条件 
+  Claude 不知道什么时候调用 

**正确写法：**

```plain
description: Analyze large Vue3 components and refactor duplicated template logic, reactive state, and Composition API code. Use when components become difficult to maintain or contain repeated UI sections.
```

这个版本明显更容易触发。因为它包含了：

| 类型 | 内容 | 作用 |
| --- | --- | --- |
| 任务（Task） | analyze / refactor | Claude 要执行什么动作 |
| 技术领域（Domain） | Vue3 / Composition API | 属于哪个专业领域 |
| 触发条件（Trigger） | duplicated logic / difficult to maintain | 什么情况下应该调用 |


## 3.2  高质量 description 的结构
一个容易触发的 description，通常包含三部分：

**（1）任务（Task）**

比如：

+  analyze 
+  review 
+  debug 
+  optimize 
+  refactor 

例如：**Analyze Vue3 reactive state issues **就比** Vue helper **更优

**（2）领域（Domain）**

告诉 Claude这个 Skill 属于什么领域

例如：

+  Vue3 
+  Composition API 
+  Vite 
+  reactive 
+  template 
+  performance 

**（3）触发条件（Trigger）**

这是最关键的，例如：

```plain
Use when debugging unnecessary component rerenders, repeated reactive state, or complex Composition API logic.
```

Claude 会根据这些场景自动匹配。

## 3.3 description 和 when_to_use 的区别
**description适合放：**

+  核心关键词 
+  最重要触发条件 
+  高优先级信号 

例如：

```plain
description: Review Vue3 Composition API usage and detect duplicated reactive state, unnecessary watchers, and complex component logic.
```

---

**when_to_use适合放：**

+  更多场景 
+  边缘情况 
+  补充规则 

例如：

```plain
when_to_use: |
  Use when:
  - setup() becomes too large
  - watchers are difficult to track
  - components rerender too frequently
  - Todo list logic becomes tightly coupled
```

## 3.4 不要把所有东西塞进 SKILL.md
更好的方式是引用外部文件：

```plain
See [checklists/vue-review.md](checklists/vue-review.md)
```

让 Claude 需要时再读取。

## 3.5  一个 Todo 项目的实际例子
比如我有一个专门优化 Vue3 状态逻辑的 Skill：

```markdown
---
name: vue-state-review
description: Analyze Vue3 Composition API logic and reactive state patterns. Detect duplicated refs, unnecessary watchers, tightly coupled state, and performance issues in large components. Use when refactoring Vue3 components or debugging frontend state problems.
when_to_use: |
  Use when:
  - setup() becomes too large or hard to maintain
  - watch / watchEffect logic is complex or duplicated
  - reactive / ref usage is inconsistent
  - Todo state becomes tightly coupled between components
  - components rerender too frequently
  - debugging Vue3 frontend performance issues
---

# Vue State Review

You are a Vue3 架构审查专家 (Vue3 architecture reviewer).

Focus on / 重点关注以下方面：

- Composition API maintainability（可维护性）
- reactive / ref consistency（响应式状态一致性）
- watch / watchEffect simplification（副作用优化）
- component responsibility separation（组件职责拆分）
- Todo state decoupling（状态解耦）
- unnecessary reactive nesting（不必要的响应式嵌套）
- rendering performance（渲染性能）

---

## Review Behavior / 审查行为要求

When analyzing Vue code:

- Identify state management problems first
- Check Composition API structure before logic details
- Prefer simplification over over-engineering
- Highlight performance risks early
- Focus on maintainability over clever code

---

## Output Format / 输出格式

Always respond in this structure:

1. Problem Summary（问题总结）
2. Root Cause Analysis（原因分析）
3. Risk Level（风险等级）
4. Suggested Refactor（重构建议）
5. Code Example (if needed)（代码示例）

---

## Review Checklist

See detailed rules:
[checklists/vue-review-checklist.md](checklists/vue-review-checklist.md)
```

这种 Skill 在下面这些场景特别容易触发：

+  Todo 列表卡顿 
+  watch 太多 
+  setup() 过大 
+  reactive/ref 混乱 
+  组件状态耦合严重 

我把它放到.claude/skills/<name>/SKILL.md中就可以直接使用了。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1778636941429-05edd9fe-a442-42d6-84e3-7d86a3394e5d.png)

# <font style="color:rgb(51, 51, 51);">四、动态上下文与调用控制</font>
## 4.1 动态上下文（!command）
`!command` 会在 Skill 执行前先运行 shell 命令，并把**结果注入给 Claude**，而不是命令本身。

下方是一个例子：

```plain
---
name: pr-summary
description: Summarize pull request changes
---

## PR context
- Diff: !`gh pr diff`
- Comments: !`gh pr view --comments`
- Files: !`gh pr diff --name-only`

Summarize this PR.
```

+  把“实时的数据”放入上下文 
+  Claude 只看到结果，不知道命令 
+  适合 PR、日志、数据库查询等场景 

## 4.2 shell 执行环境控制
通过 `shell` 指定执行环境：

```plain
---
name: windows-helper
description: Manage Windows services and configurations
shell: powershell
---
```

## 4.3 Invocation 权限控制（谁可以触发）
### disable-model-invocation
```plain
---
name: deploy-service
description: Deploy service to production environment
disable-model-invocation: true
---

Deploy the service using production pipeline.
```

+  只允许用户手动触发（/skill-name） 比如上面这个就是：/deploy-service
+  Claude 不会自动调用 
+  适合高风险操作（deploy / push） 

### user-invocable
```plain
name: log-analyzer
description: Analyze system logs for errors
user-invocable: false
```

+ 与disable-model-invocation相反
+  Claude 可以自动调用 
+  但用户菜单里不显示
+  适合“后台能力型 Skill” 

## 4.4 路径控制（paths）
```plain
---
name: api-generator
description: Generate REST API endpoints from schema definitions
paths: ["src/**/*.ts", "tests/**"]
---
```

+  限制 Skill 只在特定文件范围生效 
+  避免污染全局上下文 
+  提升匹配精度 
+ 项目级 Skill 作用域隔离机制

## 4.5 推理强度控制
```plain
---
name: security-review
description: Scan code for security vulnerabilities
effort: high
---
```

+  low → 快速任务（模板 / 查询） 
+  medium → 默认任务 
+  high → 深度分析 
+  xhigh → 更强推理 
+  max → 会话级最强
+ 控制 Claude 分析深度，而不是只控制输出长度

## 4.6  子代理执行（context: fork）
```plain
---
name: deep-analysis
description: Analyze large codebase architecture
context: fork
agent: Explore
---
```

+  在独立子上下文执行 Skill 
+  主对话不会被污染 
+  适合全局代码分析 
+ 大规模搜索 
+ PR / repo 级任务 

## 4.7  agent 选择（执行模式）
```plain
agent: Explore
```

+  控制 Claude “以什么工作模式执行这个 Skill”  
+  Explore → 只读分析 / 搜索 
+  Plan → 任务规划 
+  general-purpose → 通用执行 

## 4.8 模型选择（model）
```plain
model: opus
```

+ 为不同 Skill 指定模型能力 
+ opus → 深度推理 / 架构分析 
+ sonnet → 快速执行 

# 五、Skills 参数与工具权限机制
## 1. 参数传递方式（Arguments）
Skill 支持两种参数系统：

### （1）$ARGUMENTS（整体参数）
```plain
$ARGUMENTS
```

+  捕获命令后所有输入 
+  当作一个完整字符串

例如：

```plain
/review-pr 456 high
```

则：

```plain
$ARGUMENTS = "456 high"
```

+  适合不需要拆分参数的场景 
+  适合简单指令 

#### 例子
```plain
---
name: analyze-vue-bug
description: Analyze Vue3 frontend issues
---

Analyze this Vue3 problem:

$ARGUMENTS
```

用户：

```plain
/analyze-vue-bug Todo 列表切换完成状态后页面偶尔不刷新
```

Claude 实际看到：

```plain
Analyze this Vue3 problem:

Todo 列表切换完成状态后页面偶尔不刷新
```

### （2）$0 / $1 / $2（结构化参数）
```plain
$0 $1 $2
```

+  按空格拆分参数 
+  每个位置单独使用 

例如：

```plain
/review-pr 456 high
```

映射为：

| 参数 | 值 |
| --- | --- |
| $0 | 456 |
| $1 | high |


+  适合需要优先级的任务
+  类型参数 
+  结构化任务 

## 2. argument-hint（命令提示）
```plain
argument-hint: "<pr-number> <priority>"
```

+  提示用户应该怎么输入参数 
+  增强 `/` 菜单自动补全体验 

例如用户看到：

```plain
/review-pr <pr-number> <priority>
```

## 3. allowed-tools（工具权限控制）
```plain
allowed-tools: Bash(gh *), Read, Grep, Glob
```

+ 这是控制Skill 的“能力边界 + 安全沙箱机制”  
+ 限制 Skill 能使用的工具范围：

| 工具 | 作用 |
| --- | --- |
| Bash(gh *) | GitHub CLI |
| Read | 文件读取 |
| Grep | 搜索内容 |
| Glob | 文件匹配 |


## 4. 示例：PR Review Skill
```plain
---
name: review-pr
description: Review a GitHub PR by number
argument-hint: "<pr-number> <priority>"
allowed-tools: Bash(gh *), Read, Grep, Glob
---

Review PR #$0 with priority $1.
Focus on:
- security
- performance
- maintainability

Reference:
See [standards/code-review.md](standards/code-review.md)

Usage:
/review-pr 456 high
```

# 六、内置skill
一些高级命令其实就是Claude的内置的skill

##  6.1 /fewer-permission-prompts 权限提取
 Claude 会分析你平时经常使用的“安全操作”，然后自动帮你生成权限白名单，减少以后频繁弹权限确认  

**输出到：**

```plain
.claude/settings.json
```

**例如：**

```plain
{
  "allowed-tools": [
    "Bash(git *)",
    "Bash(gh *)",
    "Read",
    "Grep"
  ]
}
```

## 6.2  /simplify（并行代码审查）
这个命令会对最近变更文件做代码质量 review，并自动拆分多个并行 agent 分析不同维度，是一个轻量 PR review 自动并行化系统

## 6.3  /batch（批修改任务）
```plain
/batch add JSDoc comments to all public functions in src/
```

对多个文件进行大规模重构变更，并自动规划执行步骤

## 6.4 /loop（定时重复执行）
```plain
/loop 2m check if the build finished
```

每隔固定时间重复执行 prompt。 上面命令就是每 2 分钟重复检查一次构建是否完成，直到任务结束为止  

## 6.5  /proactive（语义别名）
/proactive本质上等价于/loop

| 命令 | 含义 |
| --- | --- |
| loop | 强调“定时循环” |
| proactive | 强调“主动观察 + 响应” |


# 6.6 /debug（调试模式）
```plain
/debug
```

开启调式模式可以看到更多的信息。 



