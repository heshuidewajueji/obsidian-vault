# Lesson 5: 项目管理

> 📅 学习日期: 2026-05-21
> 📝 学习状态: ✅ 已学习（2026-05-22）

---

## 一、CLAUDE.md 项目规范文件

### 什么是 CLAUDE.md

`CLAUDE.md` 是一个**特殊文件**，Claude Code 启动时会**自动读取**。

### 放置位置

| 位置 | 作用范围 |
|------|----------|
| 项目根目录 | 该项目 |
| `~/.claude/` | 全局（所有项目）|

### 常用内容

```markdown
# 项目名称

## 项目信息
- 技术栈：Node.js + Express
- 数据库：PostgreSQL

## 代码规范
- 使用 ES Module
- 命名：camelCase
- 注释：中文

## 常用命令
| 命令 | 说明 |
|------|------|
| `npm install` | 安装依赖 |
| `npm run dev` | 开发模式 |

## 注意事项
- ⚠️ 不要修改 config.yaml
- ⚠️ 生产环境必须通过 CI/CD 部署
```

---

## 二、Memory 记忆系统

### 什么是 Memory

Memory 让 Claude Code **记住跨会话的信息**。

### 记忆文件位置

| 位置 | 作用 |
|------|------|
| `~/.claude/memory.md` | 全局记忆 |
| `项目/.claude/memory.md` | 项目记忆 |

### 记忆内容示例

```markdown
# 我的记忆

## 关于我
- 主要使用中文交流
- 喜欢简洁的代码风格

## 偏好
- 使用 npm，不是 yarn
- 优先使用 TypeScript

## 常用项目
- project-a：React 项目
- project-b：Node.js API
```

### 管理命令

```
/memory         # 查看当前记忆
/memory add    # 添加记忆
/memory clear  # 清除记忆
```

---

## 三、Agents 代理系统

### 什么是 Agents

Agents 是 Claude Code 的**子代理**，用于特定任务。

### 内置代理类型

| 代理类型 | 说明 | 适用场景 |
|----------|------|----------|
| **general-purpose** | 通用代理 | 复杂多步骤任务 |
| **Explore** | 快速探索 | 代码库搜索 |
| **Plan** | 规划代理 | 设计实现方案 |
| **Bash** | 命令执行 | 运行终端命令 |

### 使用方式

```bash
# 使用特定代理
claude --agent plan "帮我设计这个项目的架构"

# 查看可用代理
claude agents list
```

### 创建自定义 Agent

在 `.claude/agents/` 目录创建 `.md` 文件：

```markdown
# code-review-agent

> 代码审查代理

## 描述
专注审查代码质量和安全问题。

## 触发条件
description: "当用户要求代码审查时使用"

## 行为
- 检查代码规范
- 查找潜在 bug
- 提供改进建议
```

---

## 四、项目配置最佳实践

### 文件结构

```
项目/
├── CLAUDE.md           # 项目规范（必须）
├── .claude/
│   ├── agents/         # 自定义代理
│   ├── commands/       # 自定义命令
│   ├── skills/         # 技能
│   └── memory.md       # 项目记忆
├── src/
└── README.md
```

### 分层配置

| 配置 | 范围 | 优先级 |
|------|------|--------|
| 全局 `~/.claude/` | 所有项目 | 低 |
| 项目 `.claude/` | 当前项目 | 中 |
| 项目 `CLAUDE.md` | 当前项目 | 高 |

---

## 五、实战技巧

### 技巧 1: 渐进式配置

```
第一步：只创建 CLAUDE.md（基础规范）
第二步：添加常用命令
第三步：添加项目记忆
第四步：创建自定义 Skills
```

### 技巧 2: 团队共享配置

将 `CLAUDE.md` 纳入 Git 管理：
```bash
git add CLAUDE.md
git commit -m "添加项目规范"
```

### 技巧 3: 使用模板

```bash
# 为项目生成基础配置
claude init
```

---

## 六、常见问题

| 问题 | 解答 |
|------|------|
| CLAUDE.md 不生效 | 确认放在项目根目录 |
| 记忆丢失 | 检查文件权限和位置 |
| 多项目冲突 | 使用项目级配置覆盖全局 |
| 代理不执行 | 检查 description 触发条件 |

---

## 💡 今日要点

> 1. **CLAUDE.md** - 项目规范，让 AI 懂你的项目
> 2. **Memory** - 跨会话记忆，保持上下文
> 3. **Agents** - 专用代理，处理特定任务
> 4. **分层配置** - 全局→项目→文件

---

## 📝 个人笔记区（老师已解答）

1、为啥我没有在/Users/king-macbookair/.claude 目录下发现你说的memory和agents？
   > ✅ 回答：
   > **因为这些目录默认不存在，需要手动创建！**
   >
   > 已创建的内容：
   > ✅ `~/.claude/memory.md` 已创建（你可以在里面写记忆）
   >
   > 需要手动创建的目录：
   > ```bash
   > mkdir -p ~/.claude/agents    # 自定义代理
   > mkdir -p ~/.claude/commands  # 自定义命令
   > mkdir -p ~/.claude/skills    # 自定义技能
   > ```
   >
   > **自动生成的目录**（不要手动改）：
   > - `projects/`、`sessions/`、`history.jsonl`
   > - `backups/`、`cache/`、`telemetry/`

2、我可以在 Obsidian 的 Claude Code 文件夹底下创建 claude 项目吗？项目命名是否有要求？怎么搭建项目文件夹里面的文件科学合理？
   > ✅ 回答：
   > **可以！** Obsidian Vault 本身就可以作为 Claude Code 的工作项目。
   >
   > **项目命名建议**：
   > - 可以用中文或英文，避免拼音
   > - 避免特殊字符和空格（用 `-` 或 `_` 代替空格）
   > - 示例：`Claude-Code-学习`、`my-projects`、`code-workspace`
   >
   > **推荐的 Claude 项目文件夹结构**：
   > ```
   > 20-AI与工具/
   > └── 01-Claude Code/
   >     ├── CLAUDE.md           # 项目规范（必须）
   >     ├── 笔记/
   >     ├── 脚本/
   >     └── .claude/
   >         ├── agents/         # 自定义代理
   >         ├── skills/         # 自定义技能
   >         └── memory.md       # 项目记忆
   > ```
   >
   > **注意**：你的 Vault 根目录已经有 `CLAUDE.md`，子目录也可以有自己的 `CLAUDE.md`。

3、能否单独开一课，指导我认识 ~/.claude 目录文件都是什么？起什么作用？我该怎么人工维护？
   > ✅ 回答：
   > **已创建！** 详见 [[Lesson 6+: ~/.claude 目录详解]]
   >
   > **快速总结**：
   >
   > | 文件/目录 | 是否需要创建 | 作用 |
   > |-----------|--------------|------|
   > | `memory.md` | ✅ 建议创建 | 跨会话记忆 |
   > | `agents/` | ⬜ 可选 | 自定义代理 |
   > | `commands/` | ⬜ 可选 | 自定义命令 |
   > | `skills/` | ⬜ 可选 | 自定义技能 |
   > | `CLAUDE.md` | ✅ 已创建 | 全局规范 |
   > | `settings.json` | ❌ 不要动 | API 配置 |
   > | `settings.local.json` | ⚠️ 谨慎 | 权限配置 |

4、我有cc-switch，我看到其内有针对claude code进行skills、提示词、会话、mcp管理该怎么配合项目管理使用最合理？
   > ✅ 回答：
   >
   > **cc-switch 与 Claude Code 配合架构**：
   >
   > ```
   > cc-switch（模型层）
   >   ├── 模型切换（Haiku/Sonnet/Opus）
   >   ├── 提示词管理（系统级规则）
   >   └── 会话管理
   >          ↓
   > Claude Code（项目层）
   >   ├── CLAUDE.md（项目规范）
   >   ├── memory.md（项目记忆）
   >   ├── skills/（项目技能）
   >   └── agents/（项目代理）
   >          ↓
   > MCP（扩展层）
   >   └── Obsidian/其他工具
   > ```
   >
   > **分层配合建议**：
   >
   > | 层级 | cc-switch 管理 | Claude Code 管理 | 推荐配置位置 |
   > |------|----------------|-----------------|--------------|
   > | **模型** | ✅ 模型切换 | - | cc-switch |
   > | **提示词** | ✅ 全局提示词 | 项目级提示词 | cc-switch + CLAUDE.md |
   > | **会话** | ✅ 会话历史 | - | cc-switch |
   > | **Skills** | - | ✅ 项目 Skills | `~/.claude/skills/` |
   > | **MCP** | ✅ MCP 配置 | - | cc-switch |
   >
   > **推荐的工作流**：
   >
   > ### 1. 提示词管理配合
   > ```
   > cc-switch（全局）
   > # 使用中文交流
   > # 代码使用中文注释
   > # 删除前确认
   >
   > CLAUDE.md（项目级）
   > ## 项目规范
   > - 技术栈：Python + Flask
   > - 代码风格：PEP8
   > ```
   >
   > **原则**：项目级 > 全局级
   >
   > ### 2. Skills 管理配合
   > ```
   > 全局 Skills（~/.claude/skills/）
   > - code-review/     # 通用代码审查
   > - simplify/       # 代码优化
   >
   > 项目 Skills（项目/.claude/skills/）
   > - project-specific/  # 项目专用
   > ```
   >
   > **原则**：项目专用 > 全局通用
   >
   > ### 3. MCP 管理配合
   > ```
   > cc-switch 配置 MCP 服务器
   > ├── obsidian-mcp    # 读写 Obsidian
   > ├── filesystem-mcp   # 文件操作
   > └── github-mcp      # GitHub 集成
   >
   > Claude Code 通过 MCP 访问项目文件
   > ```
   >
   > ### 4. 推荐的配合方式
   >
   > | 场景 | cc-switch | Claude Code |
   > |------|-----------|------------|
   > | 简单问答 | Haiku | - |
   > | 日常开发 | Sonnet | CLAUDE.md |
   > | 复杂重构 | Opus | /plan + 项目规范 |
   > | 代码审查 | Sonnet | 项目 skills |
   > | 跨工具协作 | Sonnet | MCP |
   >
   > **总结**：
   > - **cc-switch**：负责模型层和系统级配置
   > - **Claude Code**：负责项目层和任务执行
   > - **两者互补**，不需要重复配置

---

**📌 老师补充**：我已经帮你创建了 `~/.claude/memory.md`，你现在可以打开它写入你的个人偏好！

---

## 🔗 关联资源

- [[Lesson 6: Skills 系统]]
- [[Lesson 4: 规划模式]]
- [Claude Code 官方文档 - Configuration](https://docs.anthropic.com/claude-code/configuration)

---

*Tags: #claude-code #lesson-5 #项目管理 #CLAUDE-md #Memory #Agents*