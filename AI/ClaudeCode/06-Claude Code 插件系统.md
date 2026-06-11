Plugin 是 Claude Code 最高级别的扩展方式，可以把 Skill、Subagent、Hook、MCP Server、LSP 配置等打包成一个可安装的插件。只需要安装一次插件，就能自动获得所有配置，无需逐个手动配置。

# 一、Plugin 目录结构
最简单插件结构示例：

```plain
my-plugin/
└── .claude-plugin/
    └── plugin.json
```

唯一必须文件：

```plain
.claude-plugin/plugin.json
```

一个高级 Plugin 可以同时拥有以下的能力，完整结构示例如下：

```plain
enterprise-plugin/                     # 插件根目录（目录名任意，建议 kebab-case）
├── .claude-plugin/
│   └── plugin.json                    # 唯一必需文件——插件清单（manifest）
│
├── skills/                            # 推荐：Agent Skills（优先于 commands/）
│   ├── code-reviewer/
│   │   └── SKILL.md                   # Skill 内容 + YAML frontmatter
│   └── pdf-processor/
│       ├── SKILL.md
│       ├── scripts/                   # 可选：Skill 私有脚本
│       └── refs.md                    # 可选：参考资料
│
├── commands/                          # 可选：旧式 Slash Commands（Markdown）
│   ├── status.md
│   └── logs.md
│
├── agents/                            # 可选：自定义 Sub-Agent 定义
│   ├── security-reviewer.md
│   ├── performance-tester.md
│   └── compliance-checker.md
│
├── hooks/                             # 可选：事件钩子
│   ├── hooks.json                     # PreToolUse / PostToolUse / Session 等
│   └── security-hooks.json
│
├── bin/                               #  可选：可执行脚本（自动加入 Bash PATH）
│   └── my-tool
│
├── monitors/                          # 可选：后台监控
│   └── monitors.json
│
├── output-styles/                     # 可选：输出风格定义
│   └── terse.md
│
├── themes/                            # 可选：颜色主题
│   └── dracula.json
│
├── scripts/                           # 可选：通用脚本（hooks / skills 共用）
│   ├── security-scan.sh
│   ├── format-code.py
│   └── deploy.js
│
├── .mcp.json                          # 可选：MCP Server 配置
├── .lsp.json                          # 可选：LSP Server 配置
├── settings.json                      # 可选：插件启用时的默认 settings
│
├── README.md                          # 推荐
├── CHANGELOG.md                       # 推荐
└── LICENSE                            # 推荐
```

**假设你做一个 Java 开发插件：**

```plain
java-dev-plugin
```

目录：

```plain
java-dev-plugin/
├── .claude-plugin/plugin.json
├── skills/
│   ├── springboot/
│   └── mybatis/
├── agents/
│   ├── architect.md
│   └── performance.md
├── hooks/
│   └── hooks.json
├── .mcp.json
└── .lsp.json
```

安装后 Claude 自动获得：

```plain
SpringBoot 专家技能
MyBatis 专家技能
架构师 Agent
性能优化 Agent
Java LSP
GitHub MCP
自动 Hook
```

相当于一次安装一个完整开发工具包。

# 二、Plugin Manifest（插件清单文件）
插件身份信息写在：

```plain
.claude-plugin/plugin.json
```

示例：

```plain
{
  "name": "plugin-name",
  "displayName": "Plugin Name",
  "version": "1.2.0",
  "description": "Brief plugin description",
  "author": {
    "name": "Author Name",
    "email": "author@example.com",
    "url": "https://github.com/author"
  },
  "homepage": "https://docs.example.com/plugin",
  "repository": "https://github.com/author/plugin",
  "license": "MIT",
  "keywords": ["keyword1", "keyword2"],
  "skills": "./custom/skills/",
  "commands": ["./custom/commands/special.md"],
  "agents": ["./custom/agents/reviewer.md"],
  "hooks": "./config/hooks.json",
  "mcpServers": "./mcp-config.json",
  "outputStyles": "./styles/",
  "lspServers": "./.lsp.json",
  "experimental": {
    "themes": "./themes/",
    "monitors": "./monitors.json"
  },
  "dependencies": [
    "helper-lib",
    { "name": "secrets-vault", "version": "~2.1.0" }
  ]
}
```

字段说明：

| 字段 | 作用 |
| --- | --- |
| name | 插件名称（唯一标识，用于系统识别和加载） |
| displayName | 插件展示名称（给用户看的名字） |
| description | 插件说明（功能简介） |
| version | 版本号（用于更新和兼容性管理） |
| author | 作者信息（name/email/url） |
| homepage | 插件主页或文档地址 |
| repository | 代码仓库地址 |
| license | 开源协议（如 MIT / Apache） |
| keywords | 关键词（用于搜索和分类） |
| skills | 技能模块路径或列表（插件能力集合） |
| commands | 自定义命令定义（可执行指令或prompt） |
| agents | AI 角色/Agent 定义（不同任务人格） |
| hooks | 钩子配置（事件触发逻辑） |
| mcpServers | MCP 服务配置（外部工具接入） |
| outputStyles | 输出样式/模板配置 |
| lspServers | 语言服务配置（代码补全/分析能力） |
| experimental | 实验性功能配置 |
| dependencies | 插件依赖列表 |


## userConfig 配置项
插件可以通过 `userConfig` 声明需要用户填写的配置项。配置项根据 `sensitive` 属性分为两类：

```plain
{
  "userConfig": {
    "api_endpoint": {
      "type": "string",
      "title": "API endpoint",
      "description": "Your team's API endpoint"
    },
    "api_token": {
      "type": "string",
      "title": "API token",
      "description": "API authentication token",
      "sensitive": true
    }
  }
}
```

**1. 普通配置（默认）**

未设置 `sensitive: true` 的配置项属于普通配置，例如：

```plain
{
  "api_endpoint": {
    "type": "string"
  }
}
```

Claude Code 会将这类配置持久化保存到配置文件（如用户级或项目级 settings）中，方便插件后续继续使用。

**2. 敏感配置**

设置了 `sensitive: true` 的配置项属于敏感配置，例如：

```plain
{
  "api_token": {
    "type": "string",
    "sensitive": true
  }
}
```

这类配置通常包含：

+  API Key 
+  Access Token 
+  OAuth Refresh Token 
+  数据库密码 
+  私有认证凭据 

Claude Code 不会将其以明文形式写入配置文件，而是优先存储到操作系统提供的安全凭据存储中

## 插件环境变量
### ${CLAUDE_PLUGIN_DATA}
通过 ${CLAUDE_PLUGIN_DATA} 环境变量 配置插件数据的目录。用于存放插件在运行时产生的数据，不是插件代码本身。

###  ${CLAUDE_PLUGIN_ROOT}
通过${CLAUDE_PLUGIN_DATA}环境变量 配置插件安装目录。

运行脚本可以引用${CLAUDE_PLUGIN_ROOT}：

```plain
{
  "command": "node ${CLAUDE_PLUGIN_ROOT}/bin/audit.js"
}
```

Claude 自动展开：

```plain
node ~/.claude/plugins/my-plugin/bin/audit.js
```

# 三、Plugin Monitors和LSP 支持
对于** skills hook agent mcp**这些概念之前已经提过了。这里再提一下**Monitor**和**LSP**。

## Plugin Monitor 
Claude Code 插件可以声明长期运行的后台监控进程（Monitor），Claude 会自动启动它们，并持续接收它们输出的内容。  

**配置在哪里：**

```plain
my-plugin/
├── plugin.json
└── monitors/
    └── monitors.json
```

**配置的内容格式：**

```plain
[
  {
    "name": "...",
    "command": "...",
    "description": "..."
  }
]
```

### 必填字段
**name：插件内部唯一**

```plain
{
  "name": "deploy-status"
}
```

**command：真正执行的命令**

```plain
{
  "command": "tail -F ./logs/error.log"
}
```

**description：描述信息**

```plain
{
  "description": "Application error log"
}
```

**when：控制什么时候启动**

默认不写等价于插件加载就启动。

```plain
{
  "when": "always"
}
```

**on-skill-invoke**

只有执行 deug skill以后才启动。

```plain
{
  "when": "on-skill-invoke:debug"
}
```

## LSP 支持
当遇到某些类型的文件时，启动对应的 LSP（Language Server Protocol）服务，然后通过 LSP 获取代码智能信息  

例如：

```plain
{
  "typescript": {
    "command": "typescript-language-server",
    "args": ["--stdio"],
    "extensionToLanguage": {
      ".ts": "typescript",
      ".tsx": "typescriptreact"
    }
  }
}
```

解释：

```plain
当 Claude 编辑 .ts/.tsx 文件时：

1. 启动 TypeScript Language Server
2. 实时获取编译错误
3. 实时获取类型信息
4. 支持跳转定义
5. 支持查找引用
6. 支持代码补全
7. 支持重构和重命名

让 Claude 像 VS Code 一样理解整个 TypeScript 项目
而不是仅靠读取文本进行推断。
```

这相当于把 VS Code 背后的 TypeScript 编译器能力直接接给 Claude 使用。 LSP 最大价值其实不是诊断，而是语义信息。这样 Claude 不需要靠大模型猜。

# 四、加载插件
## 本地开发测试
开发插件时不需要安装。直接通过命令加载本地插件文件夹。

```plain
claude --plugin-dir ./my-plugin
```

Claude 会临时加载：

```plain
./my-plugin
```

当前会话结束即失效。

也可以同时测试多个插件：

```plain
claude \
  --plugin-dir ./my-plugin \
  --plugin-dir ./another-plugin
```

## 远程 ZIP 测试
加载远程zip有一定风险，请确保来源可信。

如果插件已经打包成：

```plain
plugin.zip
```

可以：

```plain
claude --plugin-url https://example.com/my-plugin.zip
```

直接加载。

多个：

```plain
claude \
  --plugin-url https://a.zip \
  --plugin-url https://b.zip
```

## 热重载
开发最舒服的功能，修改文件之后使用命令重新加载：

```plain
/reload-plugins
```

## Marketplace（插件市场）
我们更多的可能是使用一些现有的插件。使用斜杠命令 /plugins 进入插件管理界面。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1780209367057-217d8240-fad2-4d14-a1ed-31badffd23f1.png)

Claude Code 默认只有官方市场：

```plain
claude-plugins-official
```

安装：

```plain
/plugin install code-review@claude-plugins-official
```

### 添加一个现有的仓库：
添加其他插件仓库（Marketplace）就是把一个 Git 仓库注册到 Claude Code。  

[https://github.com/ccplugins/awesome-claude-code-plugins](https://github.com/ccplugins/awesome-claude-code-plugins)

```plain
/plugin marketplace add ccplugins/awesome-claude-code-plugins
```

**在Discover界面我们可以搜索现有的插件。**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1780210869264-53acc644-3658-4b33-b774-4a620c6822e0.png)



# 五、Subagent 权限限制
在 Claude Code 插件体系中：

```plain
Plugin = 权限主体
Agent = 行为主体
```

Agent（Subagent）只负责定义：

```plain
如何思考
关注哪些问题
使用什么审查策略
```

例如：

```plain
Security Expert
Performance Expert
Architect Expert
```

本质上都是不同的专家角色。

### Agent 不允许声明的内容
Plugin 内部的 Agent 不允许声明：

```plain
hooks
mcpServers
permissionMode
```

例如下面这种配置是不允许的：

```plain
{
  "name": "Security Expert",
  "mcpServers": ["github"],
  "hooks": ["post-edit"],
  "permissionMode": "bypass"
}
```

### Plugin 才是权限主体
只有 Plugin 可以声明系统能力，核心目的是避免 Agent 通过动态申请能力实现权限提升。

```plain
{
  "name": "github-review",
  "mcp": "./.mcp.json",
  "hooks": "./hooks.json"
}
```

Plugin 可以配置：

```plain

MCP
Hook
Monitor
LSP
UserConfig
```

因为用户安装插件时：

```plain
安装 Plugin
↓
查看权限
↓
用户授权
↓
Plugin 获得对应能力
```

所以权限授予发生在 Plugin 级别，而不是 Agent 级别。

# 六、管理插件
## 生命周期命令
Claude Code 提供了一组插件管理命令，用于查看、验证和卸载插件。

**查看已安装插件**

```plain
claude plugin list
```

显示当前已安装的插件，例如：

```plain
Installed Plugins

✓ code-review
```

方便查看当前环境中有哪些插件处于启用状态。

**校验插件**

```plain
claude plugin validate
```

用于检查插件配置是否正确。帮助开发者在安装或发布前发现问题。

**卸载插件**

```plain
claude plugin uninstall code-review
```

## Enable / Disable
插件安装后可关闭。

```plain
claude plugin disable code-review
```

重新启用：

```plain
claude plugin enable code-review
```

如果有同名的插件需要指定一下插件仓库名

```plain
claude plugin disable  code-review@claude-plugins-official  
```

# 七、Scope 机制（作用域控制）
Claude Code 插件支持 **多层作用域（Scope）管理**，用来控制插件配置影响的范围。

## User（用户级，默认）
作用范围：

+  当前用户 
+  所有项目 

配置位置：

```plain
~/.claude/settings.json
```

例如：

```plain
claude plugin disable code-review
```

等价于：

```plain
claude plugin disable code-review --scope user
```

特点：

+  只影响你自己 
+  不会同步给团队 
+  换项目仍然生效 

## Project（项目级）
作用范围：

+  当前仓库/项目 
+  所有协作者 

配置位置：

```plain
.claude/settings.json
```

例如：

```plain
claude plugin disable code-review --scope project
```

特点：

+  通常提交到 Git 
+  团队成员 clone 项目后也会看到相同配置 
+  适合团队统一插件 

## Local（本地项目级）
Local = 私有项目配置（Personal Project Override）。仅当前机器 + 当前项目生效，并且通常不会提交到 Git。

配置位置：

```plain
.claude/settings.local.json
```

例如：

```plain
claude plugin disable code-review --scope local
```

特点：

+  只影响你自己 
+  只影响当前仓库 
+  不共享给团队 
+  通常被 `.gitignore` 忽略 





