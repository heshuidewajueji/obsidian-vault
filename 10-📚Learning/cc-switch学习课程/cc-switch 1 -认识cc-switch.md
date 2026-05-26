# cc-switch 1: 认识 cc-switch

> 📅 学习日期: 2026-05-24
> 📝 学习状态: ✅ 已学习
> 📌 基础课程 #1

---

## 一、什么是 cc-switch

### 1.1 定位

cc-switch 是一个 **AI Agent 统一配置管理工具**，口号是：

> "Switch between AI agents seamlessly"
> （在不同 AI Agent 之间无缝切换）

**核心价值**：
- 统一管理多个 AI Agent 配置
- 跨工具的 Skills 共享
- Provider 故障转移
- 统一的工具调用管理

### 1.2 与其他工具对比

```
┌─────────────────────────────────────────────────────────────┐
│                    AI Agent 工具生态                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   Claude Code ───► 专业编程助手                             │
│        │                                                       │
│        │ (cc-switch 统一管理)                                 │
│        ▼                                                       │
│   cc-switch ────► 统一配置 + Skills 共享                   │
│        │                                                       │
│        ▼                                                       │
│   Hermes ───────► 个人助手 + 自进化                          │
│        │                                                       │
│        ▼                                                       │
│   OpenClaw ─────► Agent 编排 + 协作平台                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.3 核心特性

| 特性 | 说明 |
|------|------|
| **多工具切换** | 原生支持 Claude Code / Claude Desktop / Codex / Gemini 等 |
| **Provider 管理** | 统一管理 API Providers，支持故障转移 |
| **Skills 同步** | 跨工具共享 Skills，统一管理 |
| **MCP 集成** | 支持 MCP Server 配置和管理 |
| **界面工具** | 提供 Tray 界面和 App Window |
| **配置同步** | 通过 symlink 或 copy 同步配置 |

---

## 二、架构概览

### 2.1 核心组件

```
cc-switch
├── App Window          # GUI 界面
├── Tray Icon           # 系统托盘
├── Provider Manager    # Provider 管理
├── Skill Sync Engine   # Skills 同步引擎
├── MCP Integration     # MCP 集成
└── Local Proxy         # 本地代理
```

### 2.2 目录结构

```
~/.cc-switch/
├── settings.json       # 全局设置
├── cc-switch.db       # 工具配置数据库
├── providers.db       # Provider 配置
├── backups/           # 备份目录
├── skills/           # Skills 目录
├── logs/             # 日志目录
└── skill-backups/    # Skills 备份
```

### 2.3 与 Hermes Skills 的关系

| 组件 | cc-switch | Hermes |
|------|----------|--------|
| Skills 位置 | `~/.cc-switch/skills/` | `~/.king-ai/skills/` |
| 同步方式 | symlink / copy | 自动加载 |
| 共享 | ✅ 跨工具 | 仅 Hermes |
| 注册 | Skill Registry | 内置发现 |

---

## 三、支持的工具

### 3.1 支持列表

| 工具 | 状态 | 说明 |
|------|------|------|
| Claude Code | ✅ | 专业编程助手 |
| Claude Desktop | ✅ | Anthropic 桌面应用 |
| Codex | ✅ | OpenAI 编程助手 |
| Gemini | ✅ | Google AI 助手 |
| OpenCode | ✅ | 开源 AI 编程 |
| **OpenClaw** | ✅ | Agent 编排框架 |
| **Hermes** | ✅ | 个人助手 + 自进化 |
| Yuanbao | ✅ | 元宝助手 |

### 3.2 工具切换

cc-switch 可以在不同工具之间切换：

```bash
# 使用 cc-switch 启动工具
cc-switch run claude
cc-switch run hermes
cc-switch run openclaw

# 查看可用工具
cc-switch list-apps
```

### 3.3 可见应用配置

```json
// settings.json 中的 visibleApps
{
  "visibleApps": {
    "claude": true,
    "claude-desktop": true,
    "codex": true,
    "gemini": true,
    "opencode": true,
    "openclaw": true,
    "hermes": true
  }
}
```

---

## 四、Skills 生态

### 4.1 Skills 分类

| 类别 | 数量 | 示例 |
|------|------|------|
| **Hermes 技能** | 8 | hermes-self-evolution, hermes-jury-system |
| **汽车行业** | 3 | automotive-8d-report, automotive-ppap |
| **文档处理** | 4 | docx, pdf, pptx, xlsx |
| **开发工具** | 15+ | cloudflare, vercel, mongodb, ollama |
| **Obsidian** | 3 | obsidian-bases, obsidian-cli, obsidian-markdown |
| **AI 协作** | 5+ | brainstorming, session-handoff, subagent-handoff |

### 4.2 Hermes 相关 Skills

| Skill | 功能 |
|-------|------|
| `hermes-self-evolution` | Hermes 自进化 |
| `hermes-jury-system` | 评审系统 |
| `hermes-task-state` | 任务状态管理 |
| `hermes-shared-learnings` | 共享学习 |
| `hermes-system-health-monitoring` | 系统健康监控 |
| `hermes-feishu-gateway-setup` | 飞书网关配置 |
| `hermes-api-fallback-mechanism` | API 故障转移 |
| `hermes-to-openclaw` | Hermes 转 OpenClaw |

### 4.3 Skills 同步方式

```json
// settings.json
{
  "skillSyncMethod": "symlink",  // 或 "copy"
  "skillStorageLocation": "cc_switch"
}
```

- **symlink**：创建符号链接，修改同步
- **copy**：复制文件，独立修改

---

## 五、Provider 管理

### 5.1 Provider 配置

cc-switch 统一管理 API Providers：

```
~/.cc-switch/providers.db  →  API Keys 配置
```

支持的 Provider 类型：
- **Claude**: default / claude-3-5-sonnet
- **Claude Desktop**: default
- **OpenAI**: OpenAI API
- **Google**: Gemini API

### 5.2 故障转移

```json
// settings.json
{
  "enableFailoverToggle": true,
  "failoverConfirmed": true
}
```

### 5.3 Provider 状态

| Provider | 当前配置 |
|-----------|----------|
| Claude | default |
| Claude Desktop | default |

---

## 六、当前环境状态

### 6.1 已安装版本

```
cc-switch: GUI 应用 + Skills 库
Skills 数量: 66+
```

### 6.2 当前配置

```json
{
  "showInTray": true,
  "minimizeToTrayOnClose": true,
  "launchOnStartup": true,
  "enableLocalProxy": true,
  "enableFailoverToggle": true,
  "language": "zh"
}
```

### 6.3 目录结构

```
~/.cc-switch/
├── settings.json      # 全局设置
├── cc-switch.db      # 工具配置
├── providers.db     # Provider 配置
├── backups/         # 备份
├── skills/          # Skills 库 (66+)
├── logs/            # 日志
└── skill-backups/  # Skills 备份
```

---

## 七、安装与快速开始

### 7.1 安装

根据官方文档安装 cc-switch：

```bash
# 查看官方安装方式
# https://github.com/farion1231/cc-switch
```

### 7.2 验证安装

```bash
# 启动 GUI
cc-switch

# 或使用 Tray 模式
cc-switch --tray

# 查看配置
cat ~/.cc-switch/settings.json
```

### 7.3 基本命令

```bash
# 列出可用工具
cc-switch list-apps

# 切换工具
cc-switch switch <tool-name>

# 查看 Skills
ls ~/.cc-switch/skills/

# 同步 Skills
cc-switch sync-skills
```

---

## 💡 今日要点

> 1. **cc-switch 定位**：多 AI Agent 工具的统一配置管理
> 2. **核心功能**：工具切换、Skills 共享、Provider 管理
> 3. **与 Hermes Skills**：共享 `~/.cc-switch/skills/` 库
> 4. **主要命令**：`cc-switch list-apps`、`cc-switch switch`
> 5. **66+ Skills**：涵盖汽车、开发、文档、AI 协作等多个领域

---

## 📝 个人笔记区

---

## 🔗 关联资源

- [[cc-switch 2 -安装与配置]]
- [[00-Hermes课程目录]]
- [[00-OpenClaw课程目录]]
- [[10-📚Learning/Claude-Code学习课程/00-课程毕业总结]]

---

*Tags: #cc-switch #AI-Agent #统一配置 #lesson-1*