# OpenClaw 3: 核心功能

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #3

---

## 一、Agent 核心操作

### 1.1 Agent 结构

```
~/.openclaw/agents/
├── main/                          # 主 Agent（默认）
│   ├── sessions/                   # 会话历史
│   │   └── YYYYMMDD-HHMMSS/       # 按时间组织的会话
│   ├── agent/
│   │   ├── system-prompt.md       # 系统提示词
│   │   └── config.json            # Agent 配置
│   └── learnings/                 # 学习记录
├── coding/                        # 编程 Agent
└── assistant/                     # 助手 Agent
```

### 1.2 Agent 生命周期

```bash
# 查看所有 Agent
openclaw agents list

# 切换默认 Agent
openclaw agents switch <name>

# Agent 状态
openclaw agents status

# Agent 详情
openclaw agents info <name>
```

### 1.3 Agent 配置

```bash
# 编辑 Agent 配置
openclaw agents config <name>

# 或直接编辑配置文件
nano ~/.openclaw/agents/main/agent/config.json
```

---

## 二、对话交互

### 2.1 本地 TUI

```bash
# 启动交互式 TUI
openclaw chat

# 指定 Agent
openclaw chat --agent coding

# 带系统提示词
openclaw chat --system "你是一个代码审查助手"
```

### 2.2 单次对话

```bash
# 单次对话模式
openclaw prompt "解释 Vestige 记忆系统"

# 指定模型
openclaw prompt "解释 Vestige" --model claude-sonnet

# 指定 Agent
openclaw prompt "解释 Vestige" --agent assistant
```

### 2.3 对话历史

```bash
# 查看历史会话
openclaw sessions list

# 查看特定会话
openclaw sessions show <session-id>

# 删除会话
openclaw sessions delete <session-id>
```

---

## 三、记忆系统

### 3.1 Vestige 记忆

OpenClaw 使用 Vestige 作为向量记忆存储：

```bash
# 查看记忆状态
openclaw memory status

# 记忆统计
openclaw memory stats

# 搜索记忆
openclaw memory search "项目经验"
```

### 3.2 记忆管理

```bash
# 添加记忆（手动）
openclaw memory add --content "用户偏好早晨工作" --tags preference

# 导入记忆
openclaw memory import ~/.openclaw/memory/export.json

# 导出记忆
openclaw memory export > backup.json

# 清理记忆
openclaw memory clean --before 2026-01-01
```

### 3.3 跨 Agent 记忆共享

```bash
# 共享记忆池
openclaw memory shared

# 设置共享标签
openclaw memory tag "project-alpha" --shared

# 查看共享记忆
openclaw memory shared --list
```

---

## 四、路由规则

### 4.1 路由配置

```bash
# 查看路由规则
openclaw routing rules

# 测试路由
openclaw routing test --channel telegram --content "帮我写代码"
```

### 4.2 路由条件

```
channel:telegram + keyword:编程 ──► coding-agent
channel:discord + keyword:日常 ──► assistant-agent
channel:* + keyword:紧急 ──► main-agent
```

### 4.3 自定义路由

```bash
# 添加路由规则
openclaw routing add --condition "channel:slack" --agent support

# 删除路由
openclaw routing delete <rule-id>
```

---

## 五、Gateway 服务

### 5.1 Gateway 配置

```bash
# 查看 Gateway 状态
openclaw gateway status

# Gateway 配置
openclaw gateway config

# 重启 Gateway
openclaw gateway restart
```

### 5.2 Gateway API

Gateway 提供 REST API：

```bash
# API 端点
curl http://127.0.0.1:18789/api/status

# 发送消息
curl -X POST http://127.0.0.1:18789/api/message \
  -H "Content-Type: application/json" \
  -d '{"agent": "main", "content": "你好"}'
```

### 5.3 WebSocket 连接

```bash
# WebSocket URL
ws://127.0.0.1:18789/ws

# 连接示例（需要 wscat）
wscat -c ws://127.0.0.1:18789/ws
```

---

## 六、插件系统

### 6.1 查看插件

```bash
# 列出已安装插件
openclaw plugins list

# 插件市场
openclaw plugins search <keyword>
```

### 6.2 安装插件

```bash
# 安装插件
openclaw plugins install <plugin-name>

# 示例：安装天气插件
openclaw plugins install openclaw-plugin-weather
```

### 6.3 插件配置

```bash
# 配置插件
openclaw plugins config <plugin-name>

# 启用/禁用插件
openclaw plugins enable <plugin-name>
openclaw plugins disable <plugin-name>
```

---

## 七、实战示例

### 7.1 场景：创建编程 Agent

```bash
# 1. 创建 Agent
openclaw agents create coding

# 2. 配置系统提示词
cat > ~/.openclaw/agents/coding/agent/system-prompt.md << 'EOF'
# Coding Agent
你是一个专业的编程助手，擅长：
- 代码审查
- Bug 修复
- 重构优化
- 测试生成
EOF

# 3. 添加路由规则
openclaw routing add --condition "keyword:编程" --agent coding

# 4. 验证
openclaw agents list
```

### 7.2 场景：设置跨 Agent 记忆

```bash
# 1. 在 main agent 中添加共享记忆
openclaw memory add --content "用户项目代号 Alpha" --tags project-alpha --shared

# 2. 在 coding agent 中查询
openclaw agents switch coding
openclaw memory search "Alpha"

# 3. 验证共享成功
openclaw memory shared --list
```

### 7.3 场景：配置紧急路由

```bash
# 添加紧急优先级路由
openclaw routing add \
  --condition "keyword:紧急|urgent" \
  --agent main \
  --priority 100

# 测试路由
openclaw routing test --content "紧急！系统崩溃"
```

---

## 八、常用命令速查

| 命令 | 功能 |
|------|------|
| `openclaw agents list` | 列出所有 Agent |
| `openclaw chat` | 启动 TUI |
| `openclaw prompt` | 单次对话 |
| `openclaw memory search` | 搜索记忆 |
| `openclaw routing rules` | 查看路由规则 |
| `openclaw gateway status` | Gateway 状态 |
| `openclaw plugins list` | 列出插件 |

---

## 💡 今日要点

> 1. **Agent 操作**：`agents list/create/switch` 管理多 Agent
> 2. **对话模式**：TUI 交互 / 单次 prompt / 会话历史
> 3. **记忆系统**：Vestige 向量存储，支持跨 Agent 共享
> 4. **路由规则**：条件匹配自动路由到对应 Agent
> 5. **插件扩展**：通过插件增强功能

---

## 📝 个人笔记区
1、已学习，无问题

---

## 🔗 关联资源

- [[OpenClaw 2 -安装与配置]]
- [[OpenClaw 4 -通道集成]]
- [[OpenClaw 5 -记忆系统]]
- [[00-OpenClaw课程目录]]

---

*Tags: #openclaw #核心功能 #agent #lesson-3*