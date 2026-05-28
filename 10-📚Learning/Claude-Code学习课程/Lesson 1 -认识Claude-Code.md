# Lesson 1: 认识 Claude Code

> 📅 学习日期: 2026-05-21
> 📝 学习状态: ✅ 已学习（2026-05-21）

---

## 一、Claude Code 是什么？

Claude Code 是 Anthropic（Claude AI 的开发者）于 **2025年** 推出的**命令行 AI 编程助手**。

### 核心定位
- **不是简单的代码补全工具**，而是"代理式"(agentic)助手
- 能根据你的自然语言指令，**自主规划步骤、执行任务**
- 直接在终端中运行，理解代码库内容，简化开发过程

### 技术特点
| 特点 | 说明 |
|------|------|
| 自然语言交互 | 用中文/英文描述需求即可 |
| 自主执行 | 自动规划、分步骤完成任务 |
| 上下文理解 | 理解整个代码库的结构和逻辑 |
| 工具集成 | 内置文件读写、Git、搜索等工具 |
| 安全可控 | "宪法式"AI安全机制，破坏前需确认 |

---

## 二、安装配置

### 系统要求
- **Node.js** v18+
- **Git**（用于版本控制）
- 网络环境（访问 Anthropic API）

### 安装方式对比

| 安装方式     | 命令                                                | 适用平台                 |
| -------- | ------------------------------------------------- | -------------------- |
| 原生安装     | `curl -fsSL https://claude.ai/install.ps1 \| iex` | Windows              |
| npm 安装   | `npm install -g @anthropic-ai/claude-code`        | Mac/Linux（含 Node 环境） |
| Homebrew | `brew install claude-code`                        | macOS                |
| WinGet   | `winget install Claude.ClaudeCode`                | Windows              |

### 验证安装
```bash
claude --version
```
看到版本号即成功。

---

## 三、启动与认证

### 基本启动命令
```bash
# 在项目目录中启动
cd /path/to/your/project
claude
```

### 认证流程
首次启动会引导你完成 API 认证：
1. 输入 API Key（从 Anthropic 官网获取）
2. 配置环境变量（如需代理）
3. 完成认证，开始使用

---

## 四、与传统 IDE 插件的区别

| 对比项 | 传统代码补全（如 Copilot） | Claude Code |
|--------|--------------------------|-------------|
| 交互方式 | 被动响应代码片断 | 主动规划执行完整任务 |
| 上下文 | 当前文件或光标附近 | 整个代码库 + 对话历史 |
| 任务能力 | 补全几行代码 | 重构、调试、新功能开发 |
| 工具使用 | 无 | 内置完整工具链 |

---

## 五、核心使用场景

1. **代码生成**：描述需求 → 生成完整模块
2. **代码审查**：发现潜在问题、优化建议
3. **调试修复**：定位 bug 并修复
4. **重构优化**：改进代码结构和性能
5. **文档编写**：生成或补充文档
6. **学习助手**：解释代码、讲解概念

---

## 六、学习路径建议

```
入门期（1-2天）
  ↓
掌握基础命令和交互方式
  ↓
成长期（1周）
  ↓
熟练使用规划模式和工具
  ↓
进阶期（长期）
  ↓
自定义 Skills、MCP 扩展、团队协作
```

---

## 💡 今日要点

> 记住这三点：
> 1. **Claude Code 是代理式编程助手**，不是代码补全
> 2. **安装前确保 Node.js 已就绪**
> 3. **首次使用需要配置 API 认证**

---

## 📝 个人笔记区（老师已解答）

1、在我对ai-agent认知不足时我进行了很多不专业的尝试，然后电脑的环境也被我搞得很乱，我该怎么成体系的重新搭建？1.2、怎么使用nvm进行管理？
   > ✅ 回答：
   > **重装思路**：
   > - **Node.js**：用 `nvm`（Node Version Manager）管理，可以随时切换版本，干净可控
   > - **Claude Code**：先卸载 `npm uninstall -g @anthropic-ai/claude-code`，再重装最新版
   > - **CC-Switch**：这是模型切换工具，很好！保留它
   > - **建议顺序**：`nvm install 25` → `npm install -g @anthropic-ai/claude-code` → 配置 API
   >
   > **1.2 nvm 使用指南**：
   > ```bash
   > # 安装 nvm（如果你还没有）
   > curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
   >
   > # 常用命令
   > nvm install 25        # 安装 Node v25
   > nvm install --lts      # 安装最新 LTS 版本
   > nvm use 25            # 切换到 v25
   > nvm use --lts         # 切换到 LTS 版本
   > nvm list              # 查看已安装的版本
   > nvm alias default 25  # 设置默认版本
   >
   > # 卸载全局旧版本 Node（可选，清理环境）
   > npm uninstall -g node  # 先卸载
   > ```
   > **注意**：你当前用 `n` 作为 nvm 替代品，命令类似：`n 25` 安装，`n` 切换版本

2、我已经安装好了node就绪、claude安装完成、配置了api、安装了cc-switch，围绕这些我该怎么进一步深入学习？
   > ✅ 回答：
   > **你的进度很好！** 建议这样深入：
   > 1. **先熟悉本课程**：Lesson 2-4 是核心用法
   > 2. **创建 CLAUDE.md**：在项目根目录建一个，记录项目规范，让 AI 更懂你的项目
   > 3. **CC-Switch 使用**：切换不同模型（Haiku 便宜快速/Sonnet 平衡/Opus 最强）
   > 4. **尝试 Skills**：把重复工作流封装成可复用技能
   > 5. **实战项目**：找一个实际项目边学边用

3.1、我听说claude 还有桌面版本，我该怎么安装，它和终端界面命令交互有什么优缺点，我用哪个更好？3.2、有课程安排我学习claude的桌面版吗？
   > ✅ 回答：
   > **Claude Desktop（桌面版）vs 终端版对比**：
   >
   > | 对比项 | Claude Desktop | Claude Code (终端) |
   > |--------|----------------|-------------------|
   > | **优点** | 界面友好、内置 MCP 支持、可视化 | 轻量、可脚本化、与 Git 深度集成 |
   > | **缺点** | 需要图形界面、占用资源 | 无可视化、依赖命令行 |
   > | **适合场景** | 日常对话、快速提问 | 专业开发、代码任务、CI/CD |
   >
   > **建议**：两者都用！Desktop → 日常问答；Code → 编程开发

4、我是不是单独设定一个Obsidian的结构目录文件夹，放入我对自己描述，方便ai-agent更懂我？
   > ✅ 回答：
   > **推荐做法**：
   > 1. 在 `.claude/` 目录创建 `memory.md` 文件
   > 2. 或在 Vault 创建 `📁 个人档案/` 文件夹（简介/偏好/注意事项）
   > 3. **核心**：用 `@` 引用这些文件给 AI 提供上下文，AI 不会自动读目录下的描述

5、你能扫描我电脑的环境及ai-agent配置情况给提出科学合理的系统化重构建议步骤吗？我希望是围绕claude=工作流助手、知识库管理，Obsidian=知识库、ai-agent的备份区及第二大脑、hermes=个人助手、Openclaw=hermes的任务执行助手方向搭建，这样合理吗？
   > ✅ 回答：
   > **你的架构设计非常合理！** 角色分工清晰，形成互补：
   >
   > | 工具 | 定位 | 核心职责 |
   > |------|------|----------|
   > | **Claude Code** | 工作流助手 | 代码开发、Git 操作、任务规划 |
   > | **Obsidian** | 第二大脑 | 知识库、笔记整理、AI 备份区 |
   > | **Hermes** | 个人助手 | 对话助手、日常问答、信息处理 |
   > | **OpenClaw** | 任务执行 | 自动化任务、脚本执行 |
   >
   > **你当前环境扫描结果**：
   > ```
   > ✅ Node.js v24.13.1     ✅ npm 11.8.0
   > ✅ Homebrew 5.1.11     ✅ Python 3.14.4
   > ❌ Claude Code 未安装  ⚠️ nvm 未安装（用 n 替代）
   > ✅ Volcengine API 已配置
   > ✅ .hermes/ 已存在     ✅ .openclaw/ 已存在
   > ✅ Newone_Obsidian-Vault/ 已存在
   > ```
   >
   > **系统化重构建议**：
   >
   > | 步骤 | 任务 | 状态 |
   > |------|------|------|
   > | 1 | 安装 Claude Code：`npm install -g @anthropic-ai/claude-code` | 🔴 待完成 |
   > | 2 | 配置 nvm 或使用 n 管理 Node 版本（避免全局污染） | 🔴 待完成 |
   > | 3 | 整理 Obsidian Vault：合并 King_Obsidian-Vault 和 Newone_Obsidian-Vault | 🔴 待完成 |
   > | 4 | 在 Obsidian 创建 `📁 个人档案/` 目录，定义你的使用习惯 | 🟡 规划中 |
   > | 5 | 清理 .hermes 和 .openclaw 配置，确保不冲突 | 🟡 规划中 |
   > | 6 | 测试 Claude Code + Obsidian 联动（通过 Vault 作为共享上下文） | 🟢 进阶 |
   >
   > **架构优化建议**：
   > - 建议你选择一个 Vault 作为主知识库（避免重复）
   > - Claude Code 可以在多个项目目录工作，不限于 Vault
   > - OpenClaw 可作为 Hermes 的执行层，形成 "对话→规划→执行" 闭环

---

**📌 老师补充**：你的问题质量很高！这些问题会在 Lesson 5（Memory）和 Lesson 6（Skills）详细讲解。

---

## 🔗 关联资源

- [[Lesson 2: 核心指令系统]]
- [[Lesson 2+: Claude Desktop 桌面版]]
- [[Lesson 5+: CC-Switch使用指南]]
- [[Lesson 6+: ~/.claude 目录详解]]（补充课程）
- [Claude Code 官方文档](https://docs.anthropic.com/claude-code)
- [CSDN 教程](https://blog.csdn.net/2501_93058131/article/details/155862222)

---

*Tags: #claude-code #lesson-1 #入门 #学习课程*

---

## ✅ 课后实践任务表

> 📌 基于第一课解答，汇总的行动项。完成后打 ✓

| # | 任务项 | 说明 | 优先级 | 状态 |
|---|--------|------|--------|------|
| 1 | ~~安装 Claude Code~~ | ✅ 已完成 v2.1.145 | - | ✅ |
| 2 | ~~验证安装~~ | ✅ claude --version 成功 | - | ✅ |
| 3 | ~~配置 nvm~~ | ⚠️ 网络超时,继续用 n 替代 | 🟡 中 | ✅ |
| 4 | ~~整理 Obsidian Vault~~ | ✅ 确定 Newone 为主 Vault，King 已压缩备份 | 🟡 中 | ✅ |
| 5 | ~~创建个人档案目录~~ | ✅ 已创建并填充内容 | 🔴 高 | ✅ |
| 6 | ~~创建 CLAUDE.md~~ | ✅ 已创建项目规范文件 | 🟡 中 | ✅ |
| 7 | 测试 Claude Desktop | 下载安装 Claude Desktop 与终端版对比 | 🟢 低 | ⬜ |
| 8 | ~~配置 CC-Switch~~ | ✅ 已在运行中 (:15721) | 🟢 低 | ✅ |

---

## 🔧 AI 工具环境配置总览（2026-05-21）

| 工具 | 状态 | 配置 | 建议 |
|------|------|------|------|
| **Claude Code** | ✅ 运行中 | Volcengine API + cc-switch | 当前最佳，保持 |
| **Hermes** | ✅ 运行中 | MiniMax-M2.7 | 当前最佳，保持 |
| **OpenClaw** | ✅ 运行中 | node 版本 | 当前最佳，保持 |
| **Obsidian** | ✅ 主 Vault | Newone_Obsidian-Vault | 当前最佳，保持 |

**架构设计（你的定位）**：
- Claude Code → 工作流助手/代码开发
- Hermes → 个人助手/日常对话
- OpenClaw → 任务执行/自动化
- Obsidian → 知识库/第二大脑

**结论**：当前配置协调良好，无需还原。建议专注学习，实践中发现问题再调整。

**今日行动小目标**:
- [x] 任务 1-2:安装并验证 Claude Code ✅
- [x] 任务 3:配置 nvm(或确认 n 够用) ✅ (网络超时,继续用 n 替代)
- [x] 任务 4:确定主 Vault ✅ (Newone_Obsidian-Vault)
- [x] 任务 5:填充个人档案目录 ✅ (简介.md/偏好.md/注意事项.md)