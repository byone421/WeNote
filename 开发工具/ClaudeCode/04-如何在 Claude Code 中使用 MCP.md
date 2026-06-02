# 一、MCP 是什么
MCP（Model Context Protocol）是 Anthropic 提出的一个协议。它让 Claude 可以**实时访问外部系统数据** Claude 可以：

+  实时访问外部服务 
+  动态发现工具 
+  执行长生命周期工作流 
+  与 GitHub / 数据库 / Slack / 浏览器等系统交互

# 二、添加 MCP Server 的方式
我们可以使用claude mcp add 命令来添加一个MCP Server。支持三种方式通信

| 类型 | 用途 |
| --- | --- |
| http | 远程服务 |
| stdio | 本地进程 |
| sse | 旧版远程协议（已逐渐废弃） |


**1. 添加远程 HTTP MCP**

```plain
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

**2. 添加本地 Node.js MCP**

```plain
claude mcp add everything -- npx -y @modelcontextprotocol/server-everything
```

**3. 添加时携带鉴权 Header**

```plain
claude mcp add --transport http my-api https://api.example.com/mcp \
  --header "Authorization: Bearer $TOKEN"
```

添加完成后，启动Claude Code。使用**/mcp**命令可以查看mcp 服务的状态，对于需要认证的服务，选中即可进行授权操作。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1779174271221-05c79ac6-ae55-439b-9946-9886fecc6d26.png)

# 三、常用管理命令
除了使用斜杆命令要启动claude，其他命令直接在控制台直接执行即可。

+ `claude mcp list`：列出mcp服务器 
+ `claude mcp get <name>`：查看指定mcp的配置 
+ `claude mcp remove <name>`：查看指定的mcp
+ `/mcp`：会话内查看连接 + 触发鉴权
+ `claude mcp reset-project-choices`：清除你对“项目级 MCP 服务器”的授权与交互选择记录
+ `claude mcp add-from-claude-desktop`：从Claude Desktop（桌面端应用）同步
+ `claude mcp serve`：Claude Code 自身作为一个 MCP Server 启动，供其他支持 MCP 的客户端（如 Claude Desktop、Cursor，或其他智能体）来调用。

# 四、配置文件位置
1. **用户级**

```plain
~/.claude.json
```

2. **项目级（推荐团队共享）**

```plain
.mcp.json
```

示例：

```plain
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

# 五、临时使用方式
1. **只在当前会话加载**

```plain
claude --mcp-config ./ci-servers.json
```

2. **多文件叠加**

```plain
claude --mcp-config "./shared.json ./override.json"
```

3. **完全隔离**

开启“严格模式，在这个模式下，Claude Code 不会再去加载全局（`~/.claude.json`）、项目级（`.mcp.json`）或其他地方的默认 MCP 配置，会话环境完全由 `./repro.json`独占决定

```plain
claude --strict-mcp-config --mcp-config ./repro.json
```

# 六、MCP 的三种 Scope（作用域）
MCP 配置有三层作用域：

| Scope | 位置 | 作用 |
| --- | --- | --- |
| User Scope | ~/.claude.json | 所有项目 |
| Project Scope | .mcp.json | 团队共享 |
| Local Scope | ~/.claude.json 中项目项 | 当前项目私有 |


优先级：

```plain
Local > Project > User
```

这意味着团队统一配置 GitHub MCP 后，你仍然可以进行本地调式，不影响别人。

# 七、MCP Prompt 的调用方式
 MCP Prompt 就是提前定义好的“提示词模板”，Claude 会把它自动注册成一个斜杆命令，让你像执行命令一样一键调用，本质上就是：把复杂 Prompt、上下文注入以及工具调用流程封装起来，不用每次重复手写长篇提示词，直接通过一条命令触发完整的 AI 工作流  

```plain
/mcp__servername__promptname
```

例如：

```plain
/mcp__github__review_pr
```

# 八、MCP Resource 引用方式
MCP 不只是只能调用外部工具，还支持“资源系统” 

资源可直接 inline 引用：

```plain
@server:protocol://resource/path
```

例如你连接了一个 `github`MCP Server，可以这样提问：

```plain
"帮我分析一下 @github:issue://123 并提出修复建议"
```



# 十、alwaysLoad: true
默认mcpServers的 tools是懒加载，即是按需加载的。但可以使用alwaysLoad：true来强制加载tools。

```plain
{
  "mcpServers": {
    "tavily-search": {
      "type": "http",
      "url": "https://mcp.tavily.com/mcp/",
      "alwaysLoad": true
    }
  }
}
```

适合：

+  高频使用工具 
+  小型工具集 
+  核心 MCP server  

# 十一、Subagent Scoped MCP
这是 Claude Code 里一种非常重要的 Agent 权限隔离机制。核心思想是不同 Agent：

+  拥有不同 MCP 权限 
+  不共享所有工具 

例如：

```plain
Agent A 只能访问数据库
Agent B 只能访问浏览器
Agent C 只能访问 GitHub
```

配置位置写在：

```plain
.claude/agents/*.md
```

例如：

```plain
.claude/agents/data-analyst.md
```

### 示例
配置某个agent只能访问database MCP和 playwright MCP 

```plain
---
name: data-analyst
description: Analyze production data
mcpServers:
  - database
  - playwright:
      type: stdio
      command: npx
      args: ["-y", "@playwright/mcp@latest"]
---
```

# 十二、MCP 的使用方式
Claude中 MCP 的核心不是API 调用，而是通过自然语言和AI交互。 本质变化不是“更方便的 API 调用”，而是从 指令式操作 → 语义式任务描述。Claude会自动帮我去调用合适的MCP工具。下方是一些例子说明：

1. **GitHub MCP 示例**

用户：

```plain
列出所有已打开（open）且超过3天没有被审查（review）过的PR。
```

Claude：

+  查询 GitHub 
+  获取 PR 
+  分析 review 时间 
+  返回结果 
2. **创建 Issue**

```plain
为登录超时（login timeout）这个 bug 创建一个 Issue，并设置为中等优先级（medium priority）。
```

Claude 自动：

+  组织参数 
+  调用 tool 
+  创建 issue 
3. **Database MCP：自然语言数据库**

以前：

```plain
SELECT ...
```

现在：

```plain
找出在过去30天内下单超过5次的所有用户
```

Claude：

+  理解业务语义 
+  自动生成查询 
+  返回结果 

