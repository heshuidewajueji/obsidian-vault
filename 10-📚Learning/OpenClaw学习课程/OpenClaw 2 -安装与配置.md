# OpenClaw 2: 安装与配置

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #2

---

## 一、安装方式

### 1.1 npm 全局安装

```bash
npm install -g openclaw

# 验证安装
openclaw --version
```

### 1.2 npx 免安装运行

```bash
# 直接运行（每次下载最新版本）
npx openclaw --version

# 或使用特定版本
npx openclaw@2026.5.18 --version
```

### 1.3 检查 Node.js 版本

```bash
node --version  # 需要 18+
npm --version
```

---

## 二、基础配置

### 2.1 交互式配置

```bash
# 启动配置向导
openclaw configure

# 非交互式配置
openclaw config set <key> <value>
```

### 2.2 查看当前配置

```bash
# 查看配置文件路径
openclaw config file

# 查看所有配置
openclaw config show
```

### 2.3 配置文件

```
~/.openclaw/
├── config.json           # 主配置
├── .env                 # 环境变量（API Keys）
└── node.json            # 节点配置
```

---

## 三、Agent 管理

### 3.1 列出 Agent

```bash
openclaw agents list

# 输出示例：
# Agents:
# - main (default)
#   Identity: 🐾 OpenClaw
#   Workspace: ~/.openclaw/workspace
```

### 3.2 Agent 目录结构

```
~/.openclaw/agents/
├── main/              # 主 Agent（默认）
│   ├── sessions/       # 会话历史
│   ├── agent/          # Agent 特定配置
│   └── learnings/      # 学习记录
├── coding/            # 编程 Agent（可扩展）
└── assistant/         # 助手 Agent（可扩展）
```

### 3.3 创建新 Agent

```bash
openclaw agents create <name>

# 例如：创建编程 Agent
openclaw agents create coding
```

---

## 四、启动与停止

### 4.1 启动本地 TUI

```bash
openclaw chat

# 或使用别名
openclaw tui
```

### 4.2 启动 Gateway

```bash
# 前台运行
openclaw gateway

# 后台运行（需要终端复用器）
tmux new -s openclaw
openclaw gateway
# Ctrl+B, D 分离
```

### 4.3 停止 Gateway

```bash
# 查找进程
ps aux | grep openclaw

# 停止
kill <PID>
```

---

## 五、通道配置

### 5.1 查看通道

```bash
openclaw channels status

# 或探测所有通道
openclaw channels status --probe
```

### 5.2 添加通道

```bash
# 添加 Telegram
openclaw channels add telegram

# 添加 Discord
openclaw channels add discord

# 添加飞书
openclaw channels add feishu
```

### 5.3 通道状态检查

```bash
# 检查配置状态
openclaw channels status

# 探测连接
openclaw channels probe
```

---

## 六、环境变量

### 6.1 常用环境变量

```bash
# API Keys
OPENCLAW_API_KEY=sk-xxx
ANTHROPIC_API_KEY=sk-ant-xxx

# 代理设置
HTTPS_PROXY=http://proxy:8080
HTTP_PROXY=http://proxy:8080

# 数据目录
OPENCLAW_STATE_DIR=~/.openclaw
```

### 6.2 .env 文件

```bash
# ~/.openclaw/.env
ANTHROPIC_API_KEY=sk-ant-xxx
OPENROUTER_API_KEY=sk-or-xxx
TELEGRAM_BOT_TOKEN=xxx
DISCORD_BOT_TOKEN=xxx
```

---

## 七、验证安装

### 7.1 快速验证

```bash
# 1. 检查版本
openclaw --version

# 2. 列出 Agent
openclaw agents list

# 3. 查看配置
openclaw config file

# 4. 启动 TUI
openclaw chat
```

### 7.2 诊断命令

```bash
# 检查配置是否有效
openclaw config validate

# 查看详细日志
openclaw --log-level debug chat
```

---

## 八、当前环境状态

### 8.1 已安装版本

```
OpenClaw 2026.5.18 (50a2481)
```

### 8.2 节点配置

```json
{
  "version": 1,
  "nodeId": "dbe8221b-...",
  "displayName": "King-Macbook的MacBook Air",
  "gateway": {
    "host": "127.0.0.1",
    "port": 18789
  }
}
```

### 8.3 目录权限

```
~/.openclaw/           # 755
├── agents/           # Agent 目录
├── workspace/        # 工作区
├── memory/           # 记忆存储
├── plugins/          # 插件
└── identity/         # 身份配置
```

---

## 💡 今日要点

> 1. **安装方式**：npm 全局或 npx 免安装
> 2. **配置命令**：`openclaw configure` 交互式或 `openclaw config set` 非交互式
> 3. **Agent 管理**：`openclaw agents list/create`
> 4. **启动方式**：`openclaw chat` 本地 TUI 或 `openclaw gateway` 后台服务

---

## 📝 个人笔记区
1、已学习，无问题

---

## 🔗 关联资源

- [[OpenClaw 1 -认识OpenClaw]]
- [[OpenClaw 3 -核心功能]]
- [[00-OpenClaw课程目录]]

---

*Tags: #openclaw #安装 #配置 #lesson-2*