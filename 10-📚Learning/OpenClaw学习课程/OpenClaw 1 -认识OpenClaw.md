# OpenClaw 1: 认识 OpenClaw

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #1

---

## 一、什么是 OpenClaw

### 1.1 定位

OpenClaw 是一个开源的 **AI Agent 编排框架**，口号是：

> "All your chats, one OpenClaw"
> （所有对话，一个 OpenClaw）

**核心价值**：
- 统一管理多个 AI Agent
- 跨平台消息通道集成
- 共享记忆与知识库
- 多 Agent 协作编排

### 1.2 与其他工具对比

```
┌─────────────────────────────────────────────────────────────┐
│                    AI Agent 工具生态                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Claude Code ───► 专业编程助手                             │
│        │                                                       │
│        │                                                      │
│   Hermes ───────► 个人助手 + 自进化                          │
│        │                                                       │
│        │                                                      │
│   OpenClaw ─────► Agent 编排 + 协作平台                     │
│                                                             │
│   cc-switch ───► 多工具统一配置管理                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.3 核心特性

| 特性 | 说明 |
|------|------|
| **多 Agent 管理** | 原生支持多个独立 Agent |
| **通道集成** | Telegram/Discord/Slack/飞书等多平台 |
| **记忆共享** | Vestige 向量存储，跨 Agent 共享 |
| **ACP 协议** | Agent Communication Protocol |
| **路由规则** | 根据条件路由到不同 Agent |
| **自我改进** | SelfImproving Agent |

---

## 二、架构概览

### 2.1 核心组件

```
OpenClaw
├── Gateway         # 网关，管理连接
├── Agent Engine     # Agent 执行引擎
├── Channel Manager # 通道管理（Telegram/Discord等）
├── Memory Store     # 记忆存储（Vestige）
└── ACP Protocol     # Agent 间通信协议
```

### 2.2 Agent 结构

```
~/.openclaw/
├── agents/                 # Agent 目录
│   └── main/              # 主 Agent
│       ├── sessions/       # 会话记录
│       └── agent/          # Agent 配置
├── workspace/             # 工作区
│   ├── .learnings/        # 学习记录
│   └── node.json          # 节点配置
├── memory/                # 记忆存储
├── plugins/               # 插件
└── identity/              # 身份配置
```

### 2.3 与 Hermes 的关系

| 组件 | OpenClaw | Hermes |
|------|----------|--------|
| 网关 | 内置 | 内置 |
| 记忆 | Vestige (向量) | MEMORY.md (文本) |
| 多 Agent | ✅ | ❌ |
| 通道 | 多平台 | 多平台 |
| 自进化 | SelfImproving Agent | 内置学习闭环 |

---

## 三、核心概念

### 3.1 ACP (Agent Communication Protocol)

Agent 之间的通信协议，支持：
- 跨 Agent 消息传递
- 任务委托
- 状态同步

### 3.2 Vestige Memory

向量记忆存储系统：
- 支持语义搜索
- 跨 Agent 共享
- 自动学习与整合

### 3.3 路由规则

根据条件路由到不同 Agent：
```
channel:telegram + keyword:编程 ──► coding-agent
channel:discord + keyword:日常 ──► assistant-agent
```

### 3.4 SelfImproving Agent

自我改进机制：
- 记录错误和纠正
- 学习最佳实践
- 自动优化提示词

---

## 四、安装与快速开始

### 4.1 安装

```bash
# npm 安装
npm install -g openclaw

# 或 npx 运行
npx openclaw --version
```

### 4.2 验证安装

```bash
openclaw --version
# OpenClaw 2026.5.18 (50a2481)

openclaw --help
```

### 4.3 基本命令

```bash
# 打开本地 TUI
openclaw chat

# 查看配置
openclaw config show

# 列出 Agent
openclaw agents list

# 查看通道状态
openclaw channels status
```

---

## 五、当前环境状态

### 5.1 已安装版本

```
OpenClaw 2026.5.18 (50a2481)
```

### 5.2 当前 Agent

```
Agents:
- main (default)
  Identity: 🐾 OpenClaw
  Workspace: ~/.openclaw/workspace
  Routing: default
```

### 5.3 目录结构

```
~/.openclaw/
├── agents/main/       # 主 Agent
├── workspace/         # 工作区
├── memory/            # 记忆存储
├── plugins/           # 插件
├── identity/          # 身份配置
└── node.json         # 节点配置
```

---

## 💡 今日要点

> 1. **OpenClaw 定位**：Agent 编排 + 协作平台
> 2. **核心优势**：多 Agent 管理 + Vestige 向量记忆
> 3. **与 Hermes 互补**：Hermes 专注个人助手，OpenClaw 管理多 Agent 协作
> 4. **主要命令**：`openclaw chat`、`openclaw agents list`、`openclaw channels status`

---

## 📝 个人笔记区
1、已学习，无疑问

---

## 🔗 关联资源

- [[OpenClaw 2 -安装与配置]]
- [[00-OpenClaw课程目录]]
- [[Hermes Agent 学习课程目录]]

---

*Tags: #openclaw #agent #acp #lesson-1*