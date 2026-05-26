# Lesson 99+: Claude Desktop 桌面版

> 📅 学习日期: 2026-05-21
> 📝 学习状态: ✅ 已学习（2026-05-22）
> 📌 关联: [[Lesson 1: 认识 Claude Code]]

---

## 一、Claude Desktop vs Claude Code 对比

| 对比项 | Claude Desktop | Claude Code (终端) |
|--------|----------------|-------------------|
| **形态** | 图形界面应用 | 命令行工具 |
| **交互方式** | 鼠标点击 + 对话框 | 终端命令 |
| **MCP 支持** | ✅ 原生内置 | ⚠️ 需手动配置 |
| **代码编辑** | ⚠️ 基础编辑 | ✅ 完整工具链 |
| **Git 集成** | ❌ 无 | ✅ 深度集成 |
| **多会话管理** | ✅ 界面化管理 | ⚠️ 命令行管理 |
| **资源占用** | 较高（需运行 GUI）| 低（纯终端）|
| **适用场景** | 日常对话、快速问答、知识探索 | 专业开发、代码任务、CI/CD |

---

## 二、安装 Claude Desktop

### macOS 安装
```bash
# 方法1: 官网下载
# 访问 https://claude.ai/download 下载 .dmg 文件

# 方法2: Homebrew
brew install --cask anthropic
```

### Windows 安装
```bash
# 方法1: 官网下载
# 访问 https://claude.ai/download 下载 .exe 文件

# 方法2: Winget
winget install Anthropic.CaudeDesktop
```

---

## 三、核心功能

### 1. 对话界面
- 类似网页版 Claude，但本地运行
- 支持上传文件、图片
- 可以创建多个对话线程

### 2. MCP 服务器管理（重点功能）
Claude Desktop 最强大的功能之一：**内置 MCP 支持**

```
设置 → MCP Servers → 添加服务器
```

常用 MCP 服务器：
- **文件系统**：`filesystem` - 直接读写本地文件
- **Obsidian**：`obsidian` - 读写 Obsidian 笔记
- **GitHub**：`github` - 管理 GitHub 仓库
- **数据库**：`sqlite`、`postgresql` 等

### 3. 应用集成
- 可以与 Mac 的 Siri Shortcuts 集成
- 支持系统级快捷键

---

## 四、推荐使用策略

### 日常使用建议

| 场景 | 推荐工具 | 原因 |
|------|----------|------|
| 快速问答、翻译 | Claude Desktop | 界面友好，即开即用 |
| 知识探索、头脑风暴 | Claude Desktop | 多会话管理方便 |
| 代码开发、重构 | Claude Code | 工具链完整、Git 集成 |
| 批量任务处理 | Claude Code | 可脚本化、自动化 |
| 笔记管理 | Obsidian + MCP | 直接读写笔记 |

### 工作流设计

```
                    ┌─────────────────┐
                    │   Claude        │
                    │   Desktop       │
                    │   （对话）       │
                    └────────┬────────┘
                             │
                             ↓
┌──────────────┐    ┌───────────────┐    ┌─────────────────┐
│   Obsidian    │◄───│   MCP 连接    │───►│   Claude Code   │
│   （知识库）   │    │   （Desktop） │    │   （开发）      │
└──────────────┘    └───────────────┘    └─────────────────┘
```

---

## 五、个人设置建议

### Desktop 设置
1. **主题**：选择深色模式，保护眼睛
2. **快捷键**：设置全局呼出快捷键（如 Cmd+Shift+C）
3. **API 配置**：确认已连接你的 Volcengine API

### 与终端版共享配置
Claude Desktop 和 Claude Code 共享 `~/.claude/settings.json`：
```json
{
  "env": {
    "ANTHROPIC_API_KEY": "你的 API Key",
    "ANTHROPIC_BASE_URL": "你的 API 地址"
  }
}
```

---

## 六、常见问题

| 问题 | 解决方案 |
|------|----------|
| 桌面版无法联网 | 检查代理设置 |
| MCP 服务器连接失败 | 确认 MCP 配置正确 |
| 消耗过多 Token | 使用 Haiku 模型处理日常对话 |

---

## 💡 今日要点

> 1. **Claude Desktop 和 Code 互补使用**
> 2. **Desktop 适合日常对话，Code 适合开发任务**
> 3. **两者共享 API 配置，无需重复设置**
> 4. **Desktop 的 MCP 支持是其最大优势**

---

## 📝 个人笔记区

（在这里写下你的理解和问题）
1、已完成学习，暂无疑问

---

*Tags: #claude-code #claude-desktop #补充课程 #桌面版*