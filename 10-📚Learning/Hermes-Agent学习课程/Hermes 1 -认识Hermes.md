# Hermes 1: 认识 Hermes Agent

> 📅 学习日期: 2026-05-22
> 📝 学习状态: ✅ 已完成
> 📌 基础课程 #1

---

## 一、什么是 Hermes Agent

### 1.1 定位

Hermes Agent 是由 **Nous Research**（知名 AI 研究实验室，Hermes 4 模型系列团队）开发的 **自我进化 AI Agent 框架**。

```
Hermes Agent 定位：
┌─────────────────────────────────────────┐
│                                         │
│   Claude Code ──→ 编程开发              │
│                                         │
│   Hermes ──────→ 个人助手/日常任务       │
│                                         │
│   OpenClaw ────→ 本地文件/浏览器控制     │
│                                         │
└─────────────────────────────────────────┘
```

### 1.2 核心特点

| 特点 | 说明 |
|------|------|
| **自我进化** | 内置自学习循环，能自主创建技能 |
| **持久记忆** | 记住历史上下文和用户偏好 |
| **多平台接入** | 支持 Telegram/Discord/Slack 等 IM |
| **技能沉淀** | 完成任务后可逐渐积累经验 |
| **多执行后端** | 支持 local/Docker/SSH/Daytona/Singularity/Modal |

### 1.3 与 Claude Code 的区别

| 维度 | Hermes Agent | Claude Code |
|------|-------------|-------------|
| **主要用途** | 个人助手、日常任务 | 编程开发 |
| **记忆方式** | 自动持久记忆 | 手动 memory.md |
| **IM 集成** | ✅ Telegram/Discord | ❌ |
| **编程能力** | 一般 | 强大 |
| **上手难度** | 简单 | 中等 |

---

## 二、安装要求

### 2.1 系统要求

| 操作系统 | 支持情况 |
|----------|----------|
| **macOS** | ✅ 完全支持 |
| **Linux** | ✅ 完全支持 |
| **Windows** | ❌ 原生不支持，需 WSL2 |
| **Android** | ✅ Termux 环境 |

### 2.2 硬件要求

| 部署方式 | 最低配置 | 推荐配置 |
|----------|----------|----------|
| 使用外部 API | 1核 1GB | 2核 4GB+ |
| 本地运行模型 | 16GB+ 内存/显存 | 32GB+ |

### 2.3 软件依赖

```bash
# Python 3.10+
python3 --version

# Git
git --version

# 网络：能访问 GitHub
```

---

## 三、快速安装（macOS）

### 3.1 一行命令安装

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

### 3.2 安装后加载

```bash
# 加载环境变量
source ~/.zshrc

# 或重启终端后直接使用
hermes
```

### 3.3 验证安装

```bash
# 查看版本
hermes --version

# 查看帮助
hermes --help
```

---

## 四、与 Claude Code 的协作

### 4.1 互补关系

```
┌─────────────────────────────────────────┐
│                                         │
│   Claude Code ──→ 编程任务              │
│       │                                 │
│       │ 遇到编程问题                    │
│       ▼                                 │
│   Hermes ────→ 日常助手/记忆查询        │
│       │                                 │
│       │ 日常任务需要编程                │
│       ▼                                 │
│   Claude Code ──→ 编程任务              │
│                                         │
└─────────────────────────────────────────┘
```

### 4.2 使用场景对比

| 任务类型 | 推荐工具 | 原因 |
|----------|----------|------|
| 写代码 | Claude Code | 专业的编程能力 |
| 调试 Bug | Claude Code | 代码分析强大 |
| 查日程 | Hermes | 持久记忆+IM 集成 |
| 发消息 | Hermes | IM 网关支持 |
| 整理笔记 | 两者都行 | 文件操作能力 |
| 回答问题 | 两者都行 | AI 能力相似 |

---

## 五、界面预览

### 5.1 终端界面

```
┌─────────────────────────────────────────┐
│  🤖 Hermes Agent                        │
│  ─────────────────────────────────────  │
│                                         │
│  你：今天有什么安排？                    │
│                                         │
│  Hermes：今天是 2026-05-22，以下是您     │
│  的日程：                                │
│  - 10:00 会议                            │
│  - 15:00 项目评审                        │
│                                         │
│  > _                                    │
└─────────────────────────────────────────┘
```

### 5.2 Web 面板

```
启动命令：hermes start

浏览器访问：http://localhost:8080
```

---

## 💡 今日要点

> 1. **Hermes 定位**：个人助手 + 自我进化的 AI Agent
> 2. **核心优势**：持久记忆、IM 集成、技能沉淀
> 3. **与 Claude Code 互补**：编程用 Claude Code，日常用 Hermes
> 4. **安装简单**：一行命令即可安装

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已学习，无问题

---

## 🔗 关联资源

- [[00-Hermes课程目录]]
- [[Lesson 1 -认识Claude-Code]]
- [Hermes Agent GitHub](https://github.com/NousResearch/Hermes-Agent)

---

*Tags: #hermes #基础课程 #lesson-1 #AI-Agent # NousResearch*
