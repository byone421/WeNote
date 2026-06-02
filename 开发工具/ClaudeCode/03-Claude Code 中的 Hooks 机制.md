# 一、Hooks 的核心定位
Claude Code Hooks 是一套面向 AI Agent 执行生命周期的事件驱动扩展机制。  
它允许在工具执行、任务创建、执行前后等关键节点注入自定义逻辑，从而实现对 Claude 工作流的约束与自动化增强。

其运行机制如下：

+  事件监听：Claude 在特定 Agent 生命周期节点（如工具调用前后、任务创建等）触发 Hook 事件 
+  脚本执行：系统根据配置调用本地或远程脚本 
+  上下文注入：脚本通过标准 JSON 输入获取当前执行上下文 
+  结果反馈：脚本通过退出码与结构化输出影响工具执行结果（如阻断执行、修改参数或返回提示信息）

# 二、hooks配置文件
项目级别的 Claude Code 的 Hooks 配置通常放在：

```plain
.claude/settings.json
```

示例json:

```plain
{
  "hooks": {
    "事件名": [
      {
        "matcher": "工具匹配",
        "if": "条件过滤",
        "hooks": [
          {
            "type": "command",
            "command": "执行脚本",
            "timeout": 10
          }
        ]
      }
    ]
  }
}
```

## matcher 与 if 的区别
1. **matcher：监听哪个工具**
+ 表示： 只监听 Bash 工具 

```plain
"matcher": "Bash"
```

+ 表示： 监听文件写入类工具 

```plain
"matcher": "Write|Edit"
```

+ 表示：监听 GitHub MCP 工具 

```plain
"matcher": "mcp__github__.*"
```

2. **if：这个工具的哪些调用才触发**
+ 表示：Bash 工具里  只有 git push 才触发 

```plain
"if": "Bash(git push*)"
```

3. **二者同时满足才执行**

```plain
matcher = 工具级过滤
if      = 调用级过滤
```

# 三、核心的Hook Event
## 3.1 Hook的输入机制
```python
Claude 内部事件发生
        ↓
构造 JSON payload
        ↓
写入 hook 程序 stdin
        ↓
执行你的脚本
        ↓
读取 stdout / exit code / stderr
        ↓
决定是否阻断 / 附加信息 / 继续执行
exitcode=2表示阻止执行 exitcode=0继续执行
```

## 3.2 PreToolUse
主要在工具执行前触发，用于校验、拦截、做安全限制

例如：

+  禁止 rm -rf 
+  禁止生产库操作 
+  禁止 git push main 
+  限制 curl 外网 

**setting.json**

```plain
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 .claude/hooks/check-dangerous.py"
          }
        ]
      }
    ]
  }
}
```

**check-dangerous.py  **

```python
import json
import sys
import os

data = json.load(sys.stdin)

cmd = data.get("tool_input", {}).get("command", "")

log_path = os.path.join(os.path.dirname(__file__), "hook.log")
with open(log_path, "a", encoding="utf-8") as f:
    f.write(f"[HOOK RUNNING] cmd={cmd}\n")
    f.write(f"[RAW DATA] {json.dumps(data, ensure_ascii=False)}\n")

dangerous = [
    "rm -rf",
    "sudo ",
    "git push origin main"
]

for d in dangerous:
    if d in cmd:
        with open(log_path, "a", encoding="utf-8") as f:
            f.write(f"[BLOCKED] dangerous command: {cmd}\n")
        print(f"Blocked dangerous command: {cmd}", file=sys.stderr)
        sys.exit(2)

with open(log_path, "a", encoding="utf-8") as f:
    f.write(f"[ALLOWED] cmd={cmd}\n")
sys.exit(0)
```

如果Claude 想执行：

```plain
rm -rf node_modules
```

直接被拦截：

```plain
Blocked dangerous command
```

## 3.3 PostToolUse
主要在工具执行后触发。当 Claude 调用的某个 tool（如 Bash / Write / Edit / MCP）执行完成之后，立刻触发。 主要用于：

+  自动 lint 
+  自动 format 
+  自动记录 
+  自动补充上下文 

**setting.json**

```python
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "bash .claude/hooks/auto-lint.sh"
          }
        ]
      }
    ]
  }
}
```

**auto-lint.sh**

```plain
#!/bin/bash

npm run eslint
npm run prettier
```

## 3.4 UserPromptSubmit
用户输入后、Claude 处理前触发。主要用于：

+  Prompt 改写 
+  自动注入规范 
+  自动增加上下文 
+  敏感词过滤 

**setting.json**

```python
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "python3 .claude/hooks/inject-rules.py"
          }
        ]
      }
    ]
  }
}
```

**inject-rules.py**

```python
import json
import sys

data = json.loads(sys.stdin.read())

prompt = data.get("prompt", "")

rules = """

【团队规范】
1. 不允许修改 public API
2. 必须兼容 Vue2
3. 必须写中文注释
"""

if "【团队规范】" not in prompt:
    data["prompt"] = prompt + rules

print(json.dumps(data), flush=True)
```

## 3.5 Stop
Claude 回复结束后触发。主要用于：

+  最终检查 
+  自动测试 
+  自动总结 
+  自动提交日志 

**setting.json**

```python
{
  "hooks": {
    "Stop": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "bash .claude/hooks/run-test.sh"
          }
        ]
      }
    ]
  }
}
```

**run-test.sh**

```plain
#!/bin/bash
echo "Running tests..."
mvn test
```

## 3.6 CwdChanged
主要用于目录变化触发， 自动切换环境  

例如：

+  自动切换 Python venv 
+  自动加载 .env 
+  自动切换 MCP 配置 

**settings.json**

```plain
{
  "hooks": {
    "CwdChanged": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "bash .claude/hooks/load-env.sh"
          }
        ]
      }
    ]
  }
}
```

**load-env.sh**

```plain
#!/bin/bash

if [ -f .env ]; then
  export $(cat .env | xargs)
fi
```

## 3.7 TaskCreated
创建子任务时触发。

**setting.json**

```python
{
  "hooks": {
    "TaskCreated": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "python3 .claude/hooks/log-task.py"
          }
        ]
      }
    ]
  }
}
```

**log-task.py**

```python
import json
import sys

data = json.load(sys.stdin)

print("=== Task Created ===")
print(json.dumps(data, indent=2))
```

## 3.8 WorktreeCreate
 当 Claude 遇到复杂任务，比如：  它可能不会在一个环境里做，而是拆成多个“并行 Agent”

例如：

+  Agent A：重构 service 
+  Agent B：修 bug 
+  Agent C：写测试 
+  Agent D：性能优化 

每个 Agent 会有不同的worktree

**setting.json**

```python
{
  "hooks": {
    "WorktreeCreate": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "python3 .claude/hooks/log-worktree.py"
          }
        ]
      }
    ]
  }
}
```

**log-worktree.py**

```python
import json
import sys

data = json.load(sys.stdin)

wt = data.get("worktree", {})

print("Worktree created:")
print("Path:", wt.get("path"))
print("Branch:", wt.get("branch"))
print("Task:", wt.get("task_id"))
```

## 3.9 PreCompact
Claude context 满了会压缩。PreCompact：压缩前触发，也可以阻止压缩 

主要用于：

+  snapshot 
+  保存上下文 
+  防止关键上下文丢失 

**setting.json**

```plain
{
  "hooks": {
    "PreCompact": [
      {
        "matcher": "auto",
        "hooks": [
          {
            "type": "command",
            "command": "python3 .claude/hooks/prevent-compact.py"
          }
        ]
      }
    ]
  }
}
```

** prevent-compact.py  **

```python
import json

print(json.dumps({
    "decision": "block",
    "reason": "大型重构进行中，禁止压缩上下文"
}))
```

# 四、Hook Type（Hook 的执行方式）
Claude Hooks 目前支持 5 类型：

```plain
command
prompt
agent
http
mcp_tool
```

## 4.1  command hook
用于执行本地的命令

比如：

+  shell 
+  python 
+  node 
+  bash 

## 4.2  prompt hook
它不是跑脚本，而是再调用一次模型来做判断，Claude执行任务完成了，但你想强制它检查：

+  有没有改完所有文件 
+  测试有没有通过 
+  是否遗漏 TODO

```plain
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "检查当前任务是否真的完成：1.所有相关文件是否修改完整？2.是否遗漏测试？3.是否存在未解决的 TODO 或报错？如果未完成，请列出缺失点并要求继续工作。",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

## 4.3 agent hook
这个是 直接启动一个“子agent”去完成一整套验证流程 ，比较重量级。比如Claude 改完代码后：开启一个自动重构验证 agent。

```plain
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "agent",
            "prompt": "检查当前代码是否符合 Clean Architecture：Controller/Service/Repository 是否分层正确？是否存在跨层调用？如果不符合，请指出具体文件和修改建议。"
          }
        ]
      }
    ]
  }
}
```

## 4.4. http hook
 把 hook 事件“转发到远程 HTTP 服务”的机制  ，POST JSON 到远程服务

```plain
{
  "hooks": {
    "PostToolUse": [
      {
        "hooks": [
          {
            "type": "http",
            "url": "https://api.my-company.com/claude/hooks"
          }
        ]
      }
    ]
  }
}
```

## 4.5 mcp_tool hook
 用 Claude 的 MCP 工具系统来执行 hook 动作 。比如 Claude 完成任务后发 Slack 消息 。

```plain
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "mcp_tool",
            "server": "slack",
            "tool": "send_message",
            "input": {
              "channel": "#deploys",
              "text": "Claude finished the task"
            }
          }
        ]
      }
    ]
  }
}
```

# 五、Skill Scoped Hooks
 Hook 不一定是全局生效的，它可以只在某个 Claude Skill 运行期间才触发的 Hooks  

## 5.1 普通 Hook vs Skill Hook
### 1. 全局 Hook（默认）
任何 Bash 都会触发

```plain
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "./check.sh"
          }
        ]
      }
    ]
  }
}
```

### 2. Skill Scoped Hook（局部）
在 Skill 内定义：

```plain
---
name: production-deploy
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/production-safety-check.sh"
          once: true
---
```



### 执行范围
| 类型 | 作用范围 |
| --- | --- |
| Global hook | 所有对话 |
| Skill hook | 仅当前 skill |


# 六、once: true
 这个 hook 在一个 session（会话）里只执行一次 ， 防止这个 hook 在同一轮 Claude 工作流里重复触发  

```plain
{
  "type": "command",
  "command": "./init-check.sh",
  "once": true
}
```

主要用于：

+  环境检查 
+  初始化 
+  login 

**初始化环境检查**

```plain
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/check-env.sh",
            "once": true
          }
        ]
      }
    ]
  }
}
```

**check-env.sh**

```bash
#!/bin/bash

echo "Checking environment..."

if [ ! -f ".env" ]; then
  echo "Missing .env file"
  exit 2
fi
```



# 
